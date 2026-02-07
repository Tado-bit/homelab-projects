# MGMT_LAN Firewall Rules

## Network Details

Management LAN: 192.168.1.0/24  
pfSense MGMT IP: 192.168.1.254  

Purpose: Dedicated management network for administering infrastructure devices.

No production traffic is allowed from this VLAN.

---

## Rule Order (Top → Bottom)

### 1. Anti-lockout Rule (pfSense Default)

Allows WebGUI access to prevent administrator lockout.

---

### 2. Allow MGMT_LAN → This Firewall (HTTPS + SSH)

Interface: MGMT_LAN  
Action: Pass  
Protocol: TCP  
Source: 192.168.1.0/24  
Destination: This Firewall  
Ports:

- 445 (HTTPS WebGUI)
- 22 (SSH)

Description:

Allow secure administrative access to pfSense from MGMT LAN only.

Notes:

- SSH uses public key authentication only
- Password login disabled
- Management plane isolated to MGMT VLAN

---

### 3. Allow MGMT_LAN → INTERNAL_NETS (Administration)

Interface: MGMT_LAN  
Action: Pass  
Protocol: Any  
Source: MGMT_LAN subnet  
Destination: INTERNAL_NETS alias  

Purpose:

Permit administrators to manage servers, security hosts, and lab systems.

---

### 4. Allow MGMT_LAN → Internet

Interface: MGMT_LAN  
Action: Pass  
Protocol: Any  
Source: MGMT_LAN subnet  
Destination: Any  

Purpose:

Allow updates, package installs, and external access from admin hosts.

---

## Security Controls Implemented

- Management VLAN separated from Server and Security VLANs
- SSH restricted to MGMT subnet
- SSH password authentication disabled
- Key-based SSH enforced
- Firewall rules explicitly control pfSense access

---

## Lessons Learned

- Firewall rules apply to pfSense itself
- Management access must be isolated
- SSH keys significantly improve security
- Clear documentation prevents configuration drift

---

## Future Improvements

- Restrict SSH to single admin IP
- Change SSH port
- Centralized logging
