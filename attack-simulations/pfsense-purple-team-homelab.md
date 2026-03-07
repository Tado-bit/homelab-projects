# pfSense Multi-VLAN Purple Team Homelab

## Overview

This project demonstrates a segmented pfSense homelab designed to simulate real-world enterprise security architecture using multiple VLANs.  
Both offensive (red team) and defensive (blue team) techniques were implemented to validate firewall controls and management plane protection.

The lab focuses on:

- VLAN segmentation
- Management plane isolation
- Brute-force simulation
- Firewall logging and detection
- Security hardening

---

## Network Architecture

Management LAN: 192.168.1.0/24  
Server LAN: 192.168.2.0/24  
Security LAN (Kali): 192.168.3.0/24  
User LAN: 192.168.4.0/24  

pfSense Gateway IPs: *.254

Security VLAN is used as attacker network.

---

## Objectives

- Restrict pfSense GUI access to Management LAN only
- Prevent lateral movement between VLANs
- Simulate brute-force attempts from Security VLAN
- Observe firewall telemetry
- Implement mitigation controls

---

## Tools Used

- pfSense
- Kali Linux
- Nmap
- Hydra
- VMware

---

## Red Team Activities

- Host discovery
- Port scanning
- Service enumeration
- Controlled credential testing
- Pivot attempts between VLANs

---

## Blue Team Controls

- Interface-based GUI restriction
- Firewall segmentation rules
- RFC1918 blocking
- Login protection
- Firewall logging
- Temporary attack exposure for testing

---

## Key Outcomes

- Management plane successfully isolated
- Brute-force attempts detected via firewall logs
- Lateral movement blocked
- Attack surface reduced after hardening

---

## Lessons Learned

- VLAN segmentation is critical for limiting attack spread
- Management interfaces must never be exposed to user networks
- Logging provides essential visibility during attacks
- Red/Blue simulation improves defensive design

---

## Future Improvements

- Suricata IDS
- Central syslog server
- Fail2ban on Linux servers
- Dashboard visualization
