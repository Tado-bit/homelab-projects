# Proxmox Virtualization Layer

This section documents the deployment and configuration of the Proxmox hypervisor used to host the homelab environment.

The hypervisor provides isolation between network segments, supports virtual firewall deployment (pfSense), and enables controlled testing environments for both offensive and defensive security scenarios.

---

## Key Responsibilities

- Hosting pfSense firewall (VM)
- Running Linux-based services (LXC containers)
- Providing virtual networking bridges (vmbr interfaces)
- Enforcing traffic flow through pfSense (no bypass)

---

## Core Configuration

### vmbr0 (WAN Bridge)
- Connected to upstream home router
- Provides internet access to pfSense WAN interface

### vmbr1 (LAN Bridge)
- Internal lab network
- Connected to pfSense LAN interface

All virtual machines and containers are connected to **vmbr1**, ensuring all traffic flows through pfSense.

---

## Security Design Considerations

- Proxmox is not directly exposed to the internet
- Management access restricted to management network
- No routing performed by Proxmox
- All traffic inspection handled by pfSense

---

## Related Documentation

- networking.md
- architecture.md
- troubleshooting.md
