# Pi-hole DNS Integration

## Lab Objective

* Deploy Pi-hole as centralized DNS filtering layer
* Integrate Pi-hole with pfSense DNS Forwarder (dnsmasq)
* Enforce DNS usage across all VLANs
* Prevent DNS bypass via firewall rules
* Enable visibility of client queries across segmented networks
* Document deployment, issues, and fixes for future reference

---

## Environment

* Hypervisor: Proxmox VE (deadpool)
* Firewall: pfSense
* DNS Filtering: Pi-hole (Debian LXC)
* Upstream DNS: Cloudflare (1.1.1.1 / 1.0.0.1)
* Network: Multi-VLAN architecture

---

## DNS Architecture

Client → pfSense (dnsmasq) → Pi-hole → Cloudflare

* pfSense acts as DNS entry point (via DHCP)
* Pi-hole performs filtering and logging
* Upstream queries forwarded to Cloudflare

---

## Network Integration

* **Pi-hole** (192.168.10.199)
* **pfSense Gateways:**

  * LAN: 192.168.10.254
  * SECURITY_LAN: 192.168.20.254
  * SERVER_LAN: 192.168.30.254

All VLANs forward DNS traffic to pfSense, which forwards to Pi-hole.

---

## Deployment Summary

### Pi-hole Setup

* Deployed Debian LXC container
* Installed Pi-hole
* Assigned static IP address

### Network Configuration

* Brought interface online
* Configured default gateway

### pfSense Configuration

* Enabled DNS Forwarder (dnsmasq)
* Set Pi-hole as upstream DNS server
* Disabled DNS Resolver (Unbound)

---

## Issues Encountered

### Interface Down (eth0)

* Container unreachable
* Cause: Missing `auto eth0`

Fix:

```
ip link set eth0 up
```

---

### No Default Gateway

* No internet connectivity

Fix:

```
ip route add default via 192.168.10.254
```

---

### FTL REFUSED Queries

* DNS queries rejected

Fix:

```
pihole-FTL --config dns.listeningMode ALL
systemctl restart pihole-FTL
```

---

### Missing Upstream DNS

* No DNS resolution

Fix:

```
pihole-FTL --config dns.upstreams '["1.1.1.1","1.0.0.1"]'
```

---

### Port 53 Conflict

* dnsmasq failed to start
* Cause: Unbound using port 53

Fix:

* Disable DNS Resolver in pfSense

---

### Conditional Forwarding Errors

Fix format:

```
true,<subnet>,<gateway>,<domain>
```

---

## Conditional Forwarding

Configured for:

* 192.168.10.0/24
* 192.168.20.0/24
* 192.168.30.0/24
* 192.168.40.0/24
* 192.168.50.0/24

Domain: `homelab.local`

---

## Security Controls

### DNS Enforcement

Firewall rules applied per VLAN:

Allow:

```
ANY → 192.168.10.199:53
```

Block:

```
ANY → ANY:53
```

All DNS traffic is forced through Pi-hole.

---

## Validation

* `nslookup google.com` resolves successfully
* `ads.google.com` returns 0.0.0.0 (blocked)
* Pi-hole dashboard shows client IPs

---

## Lessons Learned

* LXC networking must be configured before installing services
* Pi-hole v6 introduces breaking configuration changes
* Empty upstream DNS config results in total failure
* pfSense services can conflict silently (Unbound vs dnsmasq)
* Validate interface, routing, and DNS chain step-by-step

---

## Current State

* Pi-hole operational across all VLANs
* DNS filtering active
* DNS bypass prevention enforced
* pfSense forwarding DNS correctly

---

## Future Improvements

* Persist gateway configuration in container
* Validate DHCP DNS settings per VLAN
* Update Pi-hole
* Snapshot container in Proxmox
* Integrate DNS logs into Splunk

---

**Project:** SOC Homelab
**Author:** Thato Ngakane
