Homelab Projects

This repository showcases hands-on networking and security engineering projects focused on designing, implementing, and validating segmented network environments.

The lab environments simulate real-world enterprise architectures using pfSense, VLAN segmentation, and controlled adversarial testing to evaluate and strengthen security controls.

Core Focus Areas
pfSense firewall deployment and multi-VLAN segmentation
Network isolation (management, servers, and user zones)
DHCP, DNS, and internal infrastructure services
Packet capture, traffic analysis, and troubleshooting
Red/Blue team (purple team) validation of network security controls
Secure lab architecture design with full technical documentation
Lab Environment
pfSense (virtualized firewall)
Kali Linux (offensive security testing)
Linux servers (infrastructure and services)
VLAN-based segmented virtual networks
Proxmox / VMware virtualization platforms
Tools & Technologies
pfSense
Kali Linux
Proxmox / VMware
Nmap, Hydra (controlled testing only)
Linux networking tools (tcpdump, ip, netstat)
Git & GitHub (version control and documentation)
Project Structure

Each project includes:

Network architecture design and objectives
Firewall rules and segmentation strategy
Controlled red-team validation scenarios
Blue-team detection, logging, and hardening
Troubleshooting and packet analysis
Lessons learned and improvements
Version-controlled documentation
Project Index
pfSense Multi-VLAN Security Lab
Management plane isolation
Segmentation across Server, Security, and User VLANs
RFC1918 lateral movement restrictions using firewall rules
Controlled red-team testing from isolated VLAN
Blue-team monitoring, logging, and rule refinement

📁 Location: /pfsense

Methodology

Each lab follows a structured engineering workflow:

Design the network and security architecture
Implement VLANs and firewall policies
Validate behavior through controlled testing
Analyze logs and packet-level traffic
Harden configurations based on findings
Document results and improvements in Git
Continuous Development

This repository is actively maintained and expanded as new scenarios, tools, and security techniques are explored.
