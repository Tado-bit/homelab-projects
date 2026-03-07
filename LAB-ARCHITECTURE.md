# Security Engineering Homelab

This repository documents my **cybersecurity homelab**, designed to practice **network engineering, security automation, attack simulation, and SIEM analysis**.  
It demonstrates a real-world SOC workflow: attackers, detection, automated response, and log analysis.

---

## Homelab Architecture

![Homelab Architecture](diagrams/homelab-network-topology.png)

**Network Segmentation:**

| VLAN       | Subnet        | Purpose                        |
|-----------|---------------|--------------------------------|
| MGMT      | 192.168.1.0/24| Management devices             |
| SECURITY  | 192.168.2.0/24| Attacker / penetration testing |
| SERVER    | 192.168.3.0/24| Services, honeypots, SIEM     |

**Core Infrastructure:**

- pfSense firewall & VLAN segmentation
- DHCP relay & server configuration
- Fail2Ban automated blocking
- Cowrie SSH/Telnet honeypot
- Splunk Enterprise (SIEM)
- Ubuntu / Kali attacker VMs

---

## Security Automation: Fail2Ban → pfSense → Splunk

**Objective:** Detect SSH brute-force attempts and automatically block them.

**Flow:**

Kali attacker → Ubuntu SSH server → Fail2Ban → pfSense firewall table → Splunk SIEM


**Example Splunk search:**

```splunk
index=auth sourcetype=fail2ban*
| stats count by host, action


## Cowrie Honeypot → Splunk Integration

Objective: Capture SSH/Telnet attacks in real time and visualize in Splunk.

Architecture:

Kali attacker VM
     │
     ▼
Cowrie Honeypot (Ubuntu host)
     │
     ▼
JSON logs → Splunk Universal Forwarder → Splunk Enterprise (index=cowrie)

# Splunk search examples:

# Show all attacks
index=cowrie
| table _time src_ip username password eventid

# Count attacks by IP
index=cowrie
| stats count by src_ip

# Attack timeline
index=cowrie
| timechart count by src_ip


## Lessons Learned

- VLAN segmentation is critical for isolating attacker simulations

- Fail2Ban successfully automates SSH blocking

- Cowrie integration provides actionable attack intelligence

- Troubleshooting DHCP/DNS issues improves lab realism


## Skills Demonstrated

# Networking:

- VLAN configuration

- DHCP relay and IP management

- Firewall rules and aliases

# Security:

- SSH hardening and brute-force mitigation

- Honeypot deployment (Cowrie)

- Attack simulation and mitigation

# Monitoring / SIEM:

- Log ingestion into Splunk

- Dashboard creation and event correlation

- Real-time security monitoring

# Automation:

- Fail2Ban → pfSense table automation

- Event-driven response
