# pfSense Firewall Aliases

This document defines all network aliases used in firewall rules.

Aliases are used to simplify rule creation, improve readability, and enforce consistent segmentation.

---

## MGMT_LAN

Type: Network  
Value: 192.168.1.0/24  

Purpose:

Dedicated management network for administrative access to pfSense and lab infrastructure.

Only trusted admin hosts reside here.

Used for:

- WebGUI access
- SSH access
- Infrastructure management

---

## SERVER_LAN_NET

Type: Network  
Value: 192.168.2.0/24  

Purpose:

Server and services network.

Hosts DHCP, lab servers, and infrastructure services.

Isolated from MGMT_LAN and SECURITY_LAN.

---

## SECURITY_LAN_NET

Type: Network  
Value: 192.168.3.0/24  

Purpose:

Security testing network.

Contains Kali and purple-team systems.

Treated as hostile by default.

---

## INTERNAL_NETS

Type: Network Group  

Members:

- MGMT_LAN (192.168.1.0/24)
- SERVER_LAN_NET (192.168.2.0/24)
- SECURITY_LAN_NET (192.168.3.0/24)

Purpose:

Represents all internal lab networks.

Used in MGMT_LAN rules to allow administrative access across internal VLANs.

---

## Design Notes

- Aliases reduce firewall rule duplication
- Aliases enforce consistent segmentation
- INTERNAL_NETS is used ONLY from MGMT_LAN
- SECURITY_LAN is never permitted to INTERNAL_NETS

---

## Lessons Learned

- Naming conventions matter
- Aliases simplify rule management
- Network groups improve scalability
- Clear documentation prevents misconfiguration
