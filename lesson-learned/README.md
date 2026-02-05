## DHCP Labs & Lessons Learned

This homelab includes multiple DHCP-focused exercises designed to mirror
real-world enterprise scenarios, ranging from centralized DHCP deployment
to misconfiguration and security testing.

### Centralized DHCP with Relay (Day 9)

Implemented enterprise-style DHCP using a dedicated Ubuntu 24.x DHCP server
and pfSense as a relay across MGMT_LAN and SECURITY_LAN.

Key outcomes:
- DHCP subnet selection is based on relay ingress interface (GIADDR), not
  on the DHCP server’s physical network.
- Incorrect VLAN or port group placement leads to valid but unexpected leases.
- Packet capture (DISCOVER/OFFER/REQUEST/ACK) was essential for validating
  relay behavior and broadcast-domain separation.
- Separating DHCP services from the firewall reflects enterprise design
  principles and simplifies troubleshooting.

Reference: `troubleshooting/dhcp-enterprise-relay-day9.md`

---

### DHCP Scope Leakage

Explored how improper relay configuration or overlapping scopes can cause
clients to receive addresses from unintended networks.

Key outcomes:
- Scope leakage is typically caused by Layer 2 misplacement or relay path
  errors rather than DHCP service failure.
- Broadcast containment and correct interface mapping are critical.

Reference: `troubleshooting/dhcp-scope-leakage.md`

---

### DHCP Server Platform Comparison (ISC vs Kea)

Compared ISC DHCP with Kea DHCP to understand modern DHCP architecture,
configuration differences, and operational considerations.

Key outcomes:
- Kea introduces modular architecture and API-driven management.
- ISC DHCP remains simple and effective for small environments.
- Platform choice impacts scalability, automation, and monitoring.

Reference: `troubleshooting/kea-dhcp4-notes.md`

---

### DHCP Security (Day 10 – Rogue DHCP)

Simulated rogue DHCP behavior using Kali to demonstrate how unauthorized
servers can compete for leases and disrupt clients.

Key outcomes:
- DHCP is unauthenticated by default.
- Rogue servers can provide malicious gateways or DNS settings.
- Firewalls cannot prevent rogue DHCP; mitigation occurs at Layer 2
  (DHCP snooping, trusted ports, switch enforcement).
- Detection relies on packet capture and monitoring.

Reference: `troubleshooting/dhcp-rogue-detection-day10.md`
