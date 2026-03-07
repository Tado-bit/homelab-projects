# Homelab Architecture

## Network Overview

This homelab simulates a segmented enterprise network with multiple VLANs and security monitoring components.

### VLANs

MGMT VLAN      192.168.1.0/24  
SECURITY VLAN  192.168.2.0/24  
SERVER VLAN    192.168.3.0/24  

## Core Infrastructure

- pfSense firewall
- VLAN segmentation
- DHCP relay
- SSH hardened management access

## Security Automation

Fail2Ban monitors SSH authentication logs and automatically pushes malicious IPs to the pfSense firewall block table.

Flow:

Fail2Ban → pfSense table → firewall block rule

## Attack Simulation

Attack scenarios are executed from a Kali Linux attacker VM.

Examples:

- SSH brute-force attacks
- firewall rule testing
- packet capture analysis

## Troubleshooting

Real network problems encountered during lab setup:

- DHCP offer blocked by firewall
- DNS overwritten by DHCP
- DHCP relay misconfiguration
