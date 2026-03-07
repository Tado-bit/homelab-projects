# pfSense SSH Management Access

## Objective

Provide secure SSH access to pfSense restricted to the Management LAN only.

---

## Management Network

Management LAN: 192.168.1.0/24  
pfSense Mgmt IP: 192.168.1.254  

SSH is NOT permitted from Server or Security LANs.

---

## Firewall Rule (Management LAN)

Interface: Management LAN  
Action: Pass  
Protocol: TCP  
Source: 192.168.1.0/24  
Destination: This Firewall  
Destination Port: 22  

Description:

Allow SSH to pfSense from Mgmt LAN only

---

## SSH Configuration

System → Advanced → Admin Access

- Secure Shell enabled
- SSH Port: 22
- Password login disabled
- Public key authentication only

---

## Client Access

SSH keys generated on admin workstation:

ssh-keygen  
ssh-copy-id admin@192.168.1.254  

Login verified:

ssh admin@192.168.1.254

---

## Security Controls

- Management-plane isolated to Mgmt VLAN
- Password SSH disabled
- Key-based authentication enforced
- Firewall limits SSH to Mgmt subnet

---

## Lessons Learned

- Firewall rules apply to pfSense itself
- SSH should never be exposed across all VLANs
- Documenting access controls prevents configuration drift
