# pfSense Firewall Rules

## Network Overview
| Interface | Purpose | Subnet |
|---------|--------|--------|
| WAN | Internet | DHCP |
| MGMT | Management LAN | 192.168.1.0/24 |
| SERVER | Server VLAN | 192.168.2.0/24 |
| SECURITY | Security / Lab VLAN | 192.168.3.0/24 |

---

## Firewall Rule Strategy
Default deny is enforced on all internal interfaces.
Only required traffic is explicitly allowed.

---

## Management LAN Rules
- Allow HTTPS access to pfSense GUI
- Allow SSH from admin workstation
- Allow DNS to pfSense
- Block lateral movement to other VLANs

---

## Server VLAN Rules
- Allow inbound from MGMT for administration
- Allow outbound DNS and updates
- Block direct access to SECURITY VLAN

---

## Security VLAN Rules
- Allow outbound internet access
- Allow access to Server VLAN services (controlled)
- Block access to Management LAN

---

## Detailed Rule Documentation

Detailed firewall rules are documented per interface:

- Management LAN: `firewall/mgmgt_lan_rules.md`
- Server VLAN: `firewall/server_lan_rules.md`
- Security VLAN: `firewall/security_lan_rules.md`
- Aliases: `firewall/aliases.md`
- Rule order considerations: `firewall/rule_order_explained.md`


## Notes
Rules were tested using packet capture and interface counters.
