# DNS Configuration Overwritten by DHCP (Kali Linux)

## Problem
DNS resolution would fail unless a nameserver was manually added to
`/etc/resolv.conf`. The file was overwritten after reboot or DHCP renewal.

## Symptoms
- DNS stops working after reboot
- `/etc/resolv.conf` loses custom nameserver entries
- Manual edits temporarily fix the issue

## Investigation
- Verified network connectivity was functional
- Confirmed issue persisted across reboots
- Identified NetworkManager as the service managing DNS configuration

## Root Cause
`/etc/resolv.conf` is automatically managed by NetworkManager.
Manual edits are overwritten when the interface reconnects or renews DHCP.

## Resolution
Configured DNS at the NetworkManager connection level and disabled
automatic DNS assignment from DHCP.

Commands used:

```bash
nmcli connection show
nmcli con mod "Wired connection 1" ipv4.ignore-auto-dns yes
nmcli con mod "Wired connection 1" ipv4.dns "8.8.8.8 8.8.4.4"
nmcli con down "Wired connection 1"
nmcli con up "Wired connection 1"
