# DHCP Scope Leakage – Incident & Resolution

## Overview
During multi-VLAN testing in a pfSense-based homelab, DHCP clients began receiving
IP addresses from incorrect subnets. The issue affected both Server and Security LANs
and caused routing and firewall inconsistencies.

## Environment
- Firewall: pfSense
- VLANs:
  - Management LAN: 192.168.1.0/24
  - Server LAN: 192.168.2.0/24
  - Security LAN: 192.168.3.0/24
- Clients: Ubuntu Server, Kali Linux
- Virtualization: VMware

## Symptoms
- Kali Linux received 192.168.1.x addresses while connected to Security LAN
- Ubuntu Server received DHCP leases despite being intended as a static host
- DHCP offers originated from an unexpected source
- Network segmentation appeared to be broken

## Root Causes
- Deprecated isc-dhcp-server was deployed on Ubuntu
- Multiple DHCP scopes configured on a single interface
- DHCP client (dhclient) was mistakenly run on the DHCP server interface
- No single authoritative DHCP source in the lab

## Troubleshooting Actions
- Verified VLAN placement using MAC address correlation in pfSense
- Inspected DHCP lease tables per interface
- Captured DHCP traffic to identify offer sources
- Confirmed which DHCP configuration file was actively loaded
- Identified scope leakage caused by misconfiguration

## Resolution
- Removed isc-dhcp-server from Ubuntu
- Reset Ubuntu to baseline with static IP only
- Designated pfSense as the single DHCP authority
- Disabled DHCP relay
- Enabled DHCP server per interface on pfSense
- Power-cycled lab in correct startup order

## Lessons Learned
- Only one DHCP authority should exist per broadcast domain
- DHCP servers must use static IP addresses
- Deprecated services increase operational risk
- Firewalls are ideal DHCP termination points in segmented networks
- Running dhclient on a DHCP server interface can cause scope confusion

## Future Improvements
- Evaluate Kea DHCP for centralized DHCP with relay
- Implement monitoring for rogue DHCP servers
- Document startup and shutdown order for infrastructure services
