# SECURITY_LAN Firewall Rules

## Network Details

Security LAN: 192.168.3.0/24  

Purpose: Offensive / defensive testing network (Kali, purple-team hosts, attack simulations).

This VLAN is treated as untrusted.

---

## Rule Processing

Rules are evaluated top-down (first match wins).

Explicit deny rules are placed before allow rules.

---

## Rule Order (Top → Bottom)

### 1. Block SECURITY_LAN → This Firewall

Interface: SECURITY_LAN  
Action: Block  
Protocol: Any  
Source: SECURITY_LAN subnet  
Destination: This Firewall  

Purpose:

Prevent direct access to pfSense from attack systems.

---

### 2. Block SECURITY_LAN → MGMT_LAN

Interface: SECURITY_LAN  
Action: Block  
Protocol: Any  
Source: SECURITY_LAN subnet  
Destination: MGMT_LAN subnet  

Purpose:

Protect management plane from compromised or hostile hosts.

---

### 3. Block SECURITY_LAN → SERVER_LAN

Interface: SECURITY_LAN  
Action: Block  
Protocol: Any  
Source: SECURITY_LAN subnet  
Destination: SERVER_LAN subnet  

Purpose:

Prevent lateral movement toward servers.

---

### 4. Allow SECURITY_LAN → Internet

Interface: SECURITY_LAN  
Action: Pass  
Protocol: Any  
Source: SECURITY_LAN subnet  
Destination: Any  

Purpose:

Allow tools, updates, and external testing traffic.

---

## Security Controls Implemented

- Security VLAN isolated from Management VLAN
- Security VLAN isolated from Server VLAN
- No direct access to pfSense
- Explicit deny rules prevent lateral movement
- Only outbound Internet traffic permitted

---

## Design Philosophy

SECURITY_LAN is treated as hostile by default.

All internal access is denied.

Only external access is permitted.

This models real-world red-team containment.

---

## Lessons Learned

- Always explicitly block sensitive VLANs
- Never rely on implicit deny for security zones
- Rule order is critical
- Document intent, not just rules

---

## Future Improvements

- IDS/IPS on SECURITY_LAN interface
- Logging on all block rules
- Rate limiting outbound traffic
