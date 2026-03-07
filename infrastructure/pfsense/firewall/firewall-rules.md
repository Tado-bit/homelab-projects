# pfSense Firewall Rules

## Network Overview

| Interface | Purpose | Subnet |
|---------|--------|--------|
| WAN | Internet | DHCP |
| MGMT_LAN | Management LAN | 192.168.1.0/24 |
| SERVER_LAN | Server VLAN | 192.168.2.0/24 |
| SECURITY_LAN | Security / Lab VLAN | 192.168.3.0/24 |
| USER_LAN | User VLAN | 192.168.4.0/24 |

---

## Firewall Rule Strategy

- Default deny posture between internal networks
- Interface-based ingress filtering
- Centralized floating RFC1918 block to prevent lateral movement
- Explicit allow rules for required traffic only
- Management plane isolated to MGMT_LAN
- Server LAN microsegmented for administrative access
- Logging enabled on block rules for visibility

---

## Floating Rules

### RFC1918 Lateral Movement Block

A floating rule blocks RFC1918 traffic between:

- SERVER_LAN
- SECURITY_LAN
- USER_LAN

This prevents internal pivoting using a single centralized rule.

Destination alias:

RFC1918  
(10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16)

MGMT_LAN is excluded.

---

## Management LAN Rules (192.168.1.0/24)

- Allow HTTPS access to pfSense GUI
- Allow SSH from admin workstation
- Allow DNS to pfSense
- Allow outbound internet

Management network is dedicated exclusively to firewall administration.

---

## Server VLAN Rules (192.168.2.0/24)

- Block pfSense GUI access
- Allow SSH only from MGMT_LAN
- Allow DNS
- Allow outbound internet

Servers are not reachable from Security or User VLANs.

---

## Security VLAN Rules (192.168.3.0/24)

- Block pfSense GUI
- Allow DNS
- Allow outbound internet

Used for controlled testing and red-team validation only.

No access to internal VLANs.

---

## User VLAN Rules (192.168.4.0/24)

- Block pfSense GUI
- Allow DNS
- Allow outbound internet

Represents standard client devices.

---

## Validation

Controlled red-team simulations were performed from SECURITY_LAN to verify:

- VLAN isolation
- Management plane protection
- Firewall logging behavior

Results informed rule hardening and segmentation design.

---

## Detailed Rule Documentation

Per-interface rules and aliases:

- Management LAN: `firewall/mgmt_lan_rules.md`
- Server VLAN: `firewall/server_lan_rules.md`
- Security VLAN: `firewall/security_lan_rules.md`
- Aliases: `firewall/aliases.md`
- Rule order considerations: `firewall/rule_order_explained.md`

---

## Notes

Rules were validated using pfSense firewall logs, packet capture,
and interface counters.
