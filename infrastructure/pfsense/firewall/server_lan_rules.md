# SERVER_LAN Firewall Rules

## Network Details

Server LAN: 192.168.2.0/24  

Purpose: Hosts infrastructure services (DHCP, servers, lab services).

This VLAN is semi-trusted but isolated from management and security networks.

---

## Rule Processing

Rules are evaluated top-down (first match wins).

Explicit deny rules are placed before allow rules.

---

## Rule Order (Top → Bottom)

### 1. Block SERVER_LAN → This Firewall

Interface: SERVER_LAN  
Action: Block  
Protocol: Any  
Source: SERVER_LAN subnet  
Destination: This Firewall  

Purpose:

Prevent direct administrative access to pfSense from servers.

---

### 2. Block SERVER_LAN → MGMT_LAN

Interface: SERVER_LAN  
Action: Block  
Protocol: Any  
Source: SERVER_LAN subnet  
Destination: MGMT_LAN subnet  

Purpose:

Protect management plane from compromised servers.

---

### 3. Block SERVER_LAN → SECURITY_LAN

Interface: SERVER_LAN  
Action: Block  
Protocol: Any  
Source: SERVER_LAN subnet  
Destination: SECURITY_LAN subnet  

Purpose:

Prevent interaction between servers and attack systems.

---

### 4. Allow SERVER_LAN → Internet

Interface: SERVER_LAN  
Action: Pass  
Protocol: Any  
Source: SERVER_LAN subnet  
Destination: Any  

Purpose:

Allow system updates, package installs, and outbound service access.

---

## Security Controls Implemented

- Server VLAN isolated from Management VLAN
- Server VLAN isolated from Security VLAN
- No direct pfSense access from servers
- Explicit deny rules prevent lateral movement
- Only outbound Internet traffic permitted

---

## Design Philosophy

Servers should never administer firewalls.

Servers should never communicate with attack systems.

All management must originate from MGMT_LAN.

This models real-world production segmentation.

---

## Lessons Learned

- Infrastructure VLANs must be isolated
- Explicit deny rules reduce attack paths
- Management traffic belongs in a dedicated network
- Documentation enforces discipline

---

## Future Improvements

- Limit outbound ports (80/443 only)
- Add server-specific aliases
- Enable logging on block rules
