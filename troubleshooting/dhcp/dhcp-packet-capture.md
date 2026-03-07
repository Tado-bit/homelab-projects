# DHCP Packet Capture Lab (pfSense)

Screenshots referenced in this document are stored in the screenshots/ directory.

---

## Objective
Validate DHCP operation using packet capture and understand the DHCP DORA process
(Discover, Offer, Request, Acknowledge).

---

## Lab Environment
- Firewall: pfSense 2.8.1
- Client: Kali Linux
- VLAN: Security LAN (192.168.3.0/24)
- DHCP Server: pfSense
- Tool: pfSense Packet Capture

---

## Method
- Packet capture performed on the SECURITY_LAN interface
- Custom filter applied for UDP ports 67 and 68
- DHCP lease released and renewed from the client
- Capture reviewed in real time on pfSense

---

## Packet Capture Results
The packet capture showed a complete DHCP DORA exchange:

- DHCP Discover: 0.0.0.0 → 255.255.255.255
- DHCP Offer: pfSense (192.168.3.254) → client
- DHCP Request: client → broadcast
- DHCP Acknowledge: pfSense → client

This confirmed that DHCP traffic was permitted by firewall rules and correctly processed
on the Security VLAN.

---

## Evidence
![DHCP DORA Packet Capture](screenshots/dhcp-dora-pfsense.png)

---

## Observations
- DHCP renewals may appear before a full Discover sequence
- Multiple DHCP clients on Linux can result in more than one IP address on an interface
- Packet capture must be combined with forced traffic generation

---

## Lessons Learned
- Packet capture is the fastest way to isolate DHCP issues
- APIPA addresses indicate DHCP failure, not client misconfiguration
- Firewall rule order directly impacts DHCP behavior
