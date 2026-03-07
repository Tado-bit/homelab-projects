# VLAN Segmentation – pfSense Lab

## Networks

Management LAN: 192.168.1.0/24  
Server LAN: 192.168.2.0/24  
Security LAN: 192.168.3.0/24  
User LAN: 192.168.4.0/24  

pfSense gateways: *.254

---

## Design Goals

- pfSense GUI accessible only from Management LAN
- Prevent lateral movement between VLANs
- Allow DNS to pfSense per VLAN
- Allow internet access
- Harden Server LAN

---

## Floating Rule

A floating firewall rule is used to block RFC1918 traffic between:

- Server LAN
- Security LAN
- User LAN

This prevents VLAN-to-VLAN pivoting using a single centralized rule.

Destination alias:

RFC1918  
(10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16)

---

## Interface Rules

### Management LAN

- Allow HTTPS to pfSense
- Allow SSH from admin workstation
- Allow DNS
- Allow internet

---

### Server LAN

- Block pfSense GUI access
- Allow DNS
- Allow SSH only from Management LAN
- Allow internet

---

### Security LAN

- Block pfSense GUI
- Allow DNS
- Allow internet

---

### User LAN

- Block pfSense GUI
- Allow DNS
- Allow internet

---

## Outcome

- Management plane isolated
- Servers accessible only from Management LAN
- User and Security VLANs cannot pivot
- Internal networks protected via RFC1918 block

