
# pfSense VLAN Segmentation Lab

## Objective
Implement network segmentation using VLANs to separate users,
servers, and management traffic while enforcing controlled access
through firewall rules.

## Environment
- Firewall: pfSense
- Virtualization: VMware / Proxmox
- Client OS: Kali Linux / Windows
- Switching: Virtual switches

## Network Design
- VLAN 10 – Users
- VLAN 20 – Servers
- VLAN 99 – Management

## Implementation Summary
- Created VLAN interfaces on pfSense
- Configured DHCP per VLAN
- Applied firewall rules between segments

## Challenges Encountered
- DHCP clients receiving APIPA addresses
- Firewall rule order blocking traffic

## Troubleshooting & Fixes
- Verified DHCP service binding per interface
- Corrected firewall rule order
- Confirmed VLAN tagging on virtual switches
- Used packet capture to confirm DHCP DISCOVER packets reached pfSense

## Outcome
Successfully implemented VLAN segmentation with stable DHCP
assignment and controlled inter-VLAN access.

## What I Learned
- Importance of firewall rule order
- Common DHCP misconfiguration issues
- Practical VLAN troubleshooting

## Security Considerations

- Default deny-all firewall posture
- Explicit allow rules for required services only
- Management VLAN restricted to admin hosts
