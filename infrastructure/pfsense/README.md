# Lab Objectives

- Implement VLAN-based network segmentation
- Restrict pfSense management access to a dedicated management network
- Isolate servers, users, and security/testing hosts from each other
- Allow controlled outbound internet access
- Understand and document pfSense firewall rule evaluation behavior
- Validate segmentation through controlled red/blue testing

---

## Environment

- Firewall: pfSense (virtualized)
- Hypervisor: VMware / Proxmox (homelab)
- Client Systems: Kali Linux, Linux servers
- Networking: VLANs with interface-based firewall rules

---

## Network Design

The network is segmented into logical security zones using VLANs and
pfSense interface-based firewall rules. Firewall aliases are used to
represent each security zone while the underlying IP subnets are
explicitly documented for clarity and maintainability.

- **MGMT_LAN** (192.168.1.0/24)  
  Dedicated management network used exclusively for pfSense administration
  and trusted administrative hosts.

- **SERVER_LAN_NET** (192.168.2.0/24)  
  Isolated server network hosting internal services and infrastructure
  components.

- **SECURITY_LAN_NET** (192.168.3.0/24)  
  Security and testing network used for monitoring, analysis, and
  controlled experimentation (Kali / red-team simulation).

- **USER_LAN_NET** (192.168.4.0/24)  
  End-user network representing standard client devices.

---

## Lab Architecture

The lab follows a hub and spoke model with pfSense acting as the central
routing and security control point across all VLANs.

Logical flow:

USER_LAN / SECURITY_LAN / SERVER_LAN  
            ↓  
         pfSense  
            ↓  
          Internet  

Management traffic is isolated to MGMT_LAN and never traverses user or
security networks.

A topology diagram will be added under `/diagrams` to visually represent
this design.


## Firewall Design Principles

- Interface-based rule evaluation (rules apply on ingress)
- Default-deny posture between internal networks
- Explicit allow rules for required traffic only
- Alias-based rule definitions for clarity and scalability
- Floating RFC1918 block rule to prevent lateral movement
- Logging enabled on block rules for visibility and troubleshooting

---

## VLAN Segmentation Overview

A centralized floating firewall rule is used to block RFC1918 traffic
between SERVER, SECURITY, and USER VLANs, preventing internal pivoting.

Additional interface rules enforce:

- pfSense Web GUI accessible only from MGMT_LAN
- SSH to servers permitted only from MGMT_LAN
- DNS allowed per VLAN
- Controlled outbound internet access

Detailed VLAN segmentation is documented in:

➡️ `vlan-segmentation.md`

---

## Firewall Rules & Aliases Documentation

Detailed firewall configuration is documented separately to keep this
README high-level and readable:

- `firewall/aliases.md` – Firewall alias definitions
- `firewall/mgmt_lan_rules.md` – Management network firewall rules
- `firewall/server_lan_rules.md` – Server network firewall rules
- `firewall/security_lan_rules.md` – Security network firewall rules
- `firewall/rule_order_explained.md` – pfSense rule processing behavior

---

## Red / Blue Team Validation

Controlled red-team simulations were performed from SECURITY_LAN to validate:

- VLAN isolation
- Management plane protection
- Firewall logging and visibility

Blue-team hardening included:

- GUI isolation
- RFC1918 lateral movement blocking
- Server LAN microsegmentation
- Login protection

Results and mitigations are documented within this repository.

---

## Troubleshooting & Lessons Learned

- Misordered firewall rules can unintentionally block outbound traffic
- Broad aliases such as RFC1918 or "LAN subnets" can cause unintended blocks
- pfSense applies the first matching rule (top-down evaluation)
- DHCP APIPA issues were traced to incorrect interface bindings
- Proper documentation simplifies troubleshooting and future changes

---

## Current State

- pfSense management accessible only from MGMT_LAN
- Internal networks isolated from each other
- Server access restricted to Management VLAN
- Internet access permitted from all LANs
- Firewall rules documented and version-controlled in Git

---

## Future Improvements

- Add a DMZ network with inbound NAT rules
- Implement VPN access (WireGuard) terminating on MGMT_LAN
- Centralized logging and monitoring (pfSense + Wazuh)
- Traffic flow diagram with firewall rule mapping
