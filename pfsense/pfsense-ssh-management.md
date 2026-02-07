# pfSense SSH Management Access (Mgmt LAN Only)

## Objective

Provide secure command-line access to pfSense while maintaining management-plane isolation.

SSH is accessible ONLY from the Management VLAN (192.168.1.0/24).

No access is permitted from Server or Security VLANs.

---

## Network Context

Management LAN: 192.168.1.0/24  
pfSense Mgmt IP: 192.168.1.254  

---

## Configuration Summary

### 1. Enable SSH

System → Advanced → Admin Access

- Enable Secure Shell
- SSH Port: 22
- Disable password login
- Allow public key authentication only

---

### 2. Firewall Rule (Management LAN)

Firewall → Rules → Management LAN

Action: Pass  
Protocol: TCP  
Source: 192.168.1.0/24  
Destination: This Firewall  
Port: 22  

Description:

Allow SSH to pfSense from Mgmt LAN only

---

### 3. SSH Key Authentication

On admin workstation:

ssh-keygen  
ssh-copy-id admin@192.168.1.254  

Test:

ssh admin@192.168.1.254

Passwordless login confirmed.

---

## Security Benefits

- Management plane isolated to Mgmt VLAN
- No SSH exposure to Server or Security LANs
- Password authentication disabled
- Key-based authentication enforced

---

## Lessons Learned

- Firewall rules still apply even when services are enabled
- SSH keys dramatically improve security
- Proper VLAN separation simplifies infrastructure protection
- Always document firewall and management access

---

## Future Improvements

- Restrict SSH to single admin IP
- Change default SSH port
- Enable centralized logging
