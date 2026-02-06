# pfSense Multi-VLAN Security Lab – Summary

## Objective

Design and implement a segmented network using pfSense to simulate
enterprise security architecture and validate controls through
controlled red/blue testing.

---

## Architecture

- MGMT_LAN – pfSense administration only
- SERVER_LAN – internal services
- SECURITY_LAN – testing and attack simulation (Kali)
- USER_LAN – client devices

All VLANs terminate on pfSense.

---

## Key Implementations

- pfSense Web GUI restricted to Management VLAN
- Floating RFC1918 block rule preventing lateral movement
- Server LAN hardened to allow SSH only from Management VLAN
- DNS allowed per VLAN
- Controlled internet access
- Firewall logging enabled for visibility

---

## Validation

Red-team simulations were performed from SECURITY_LAN using Kali Linux to:

- Scan internal networks
- Enumerate services
- Attempt controlled credential testing

Results were observed in pfSense firewall logs and used to improve rule
design and management plane protection.

---

## Outcome

- Management plane isolated
- Internal pivoting blocked
- Servers protected from user networks
- Firewall behavior documented and version-controlled

---

## Skills Demonstrated

- VLAN segmentation
- Firewall rule design
- Management plane security
- Red/blue validation workflow
- Troubleshooting and documentation
