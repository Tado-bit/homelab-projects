# Homelab Projects

This repository documents my hands-on networking and security homelab projects,
focused on building, testing, and hardening segmented network environments.

The labs simulate real-world enterprise scenarios using pfSense, VLANs, and
controlled red/blue testing to validate firewall behavior and security design.

---

## Focus Areas

- pfSense firewall configuration and multi-VLAN segmentation
- Management plane isolation and server microsegmentation
- DHCP, DNS, and internal network services
- Troubleshooting and packet capture analysis
- Red/Blue (purple team) validation of security controls
- Security-focused lab design with full documentation

---

## Lab Environment

- pfSense (virtualized)
- Kali Linux (security testing)
- Linux servers
- Virtualized network segments (VLANs)
- CompTIA Security+ aligned practices

---

## Tools & Technologies

- pfSense
- Kali Linux
- Git & GitHub
- VMware / Proxmox
- Nmap, Hydra (controlled lab testing)
- Linux networking utilities

---

## Project Structure

Each project typically includes:

- Network design and objectives
- Firewall rules and segmentation logic
- Red-team validation (controlled attack simulation)
- Blue-team hardening and mitigation
- Troubleshooting notes
- Lessons learned
- Version-controlled documentation

---

## Project Index

### pfSense Multi-VLAN Security Lab
- Management plane isolation
- Server, Security, and User VLAN segmentation
- Floating RFC1918 lateral movement blocking
- Controlled red-team validation from Security VLAN
- Blue-team hardening and firewall logging

Location: `/pfsense`

More projects will be added as the lab expands.

---

## Approach

Each lab follows a structured workflow:

1. Design the network and security model  
2. Implement firewall and VLAN rules  
3. Validate behavior through testing  
4. Observe logs and traffic patterns  
5. Harden based on findings  
6. Document outcomes in Git  

This approach reinforces both networking fundamentals and defensive security
engineering practices.

---

This repository is actively updated as my skills continue to grow.
