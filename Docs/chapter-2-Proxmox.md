# Chapter 2 — Proxmox Virtualization Platform

## 2.1 Overview

Proxmox VE is the virtualization platform at the core of the homelab. It provides the compute, virtualization, networking, and local storage layer on which the majority of the lab services run.

The Proxmox host is named `deadpool`.

The environment currently consists of a single Proxmox node rather than a clustered deployment. Virtual machines and Linux containers are used together depending on the requirements of each service.

---

## 2.2 Current Proxmox Version

| Component | Version |
|---|---|
| Proxmox VE | 9.2.0 |
| PVE Manager | 9.2.11 |
| Running Kernel | 7.0.14-8-pve |
| Architecture | amd64 |
| Node | `deadpool` |

The host currently has 8 CPU threads and approximately 13.57 GiB of RAM.

---

## 2.3 Proxmox Host

The current Proxmox node is:

| Property | Value |
|---|---|
| Hostname | `deadpool` |
| Status | Online |
| CPU threads | 8 |
| Memory | ~13.57 GiB |
| Virtualization model | Single-node |
| Root filesystem | ext4 |
| Storage backend | LVM-thin |

The host is currently running approximately 7.67% CPU utilisation and using approximately 9.89 GiB of memory.

Because this is a relatively small homelab host, resource allocation and storage capacity need to be monitored carefully as additional services are deployed.

---

## 2.4 Virtual Machine Inventory

The current Proxmox environment contains two virtual machines.

| VMID | Name | Status | Memory | Disk |
|---:|---|---|---:|---:|
| 100 | pfSense | Running | 1120 MB | 20 GB |
| 101 | OpenMediaVault | Running | 4096 MB | 25 GB |

### VM 100 — pfSense

pfSense provides the primary routing, firewalling, VLAN segmentation, and network security functions for the homelab.

Further documentation:

- [`infrastructure/pfsense/README.md`](../infrastructure/pfsense/README.md)
- [`infrastructure/pfsense/architecture/VLAN-segmentation.md`](../infrastructure/pfsense/architecture/VLAN-segmentation.md)
- [`infrastructure/pfsense/firewall/firewall-rules.md`](../infrastructure/pfsense/firewall/firewall-rules.md)

### VM 101 — OpenMediaVault

OpenMediaVault provides network-attached storage for the homelab.

It supplies NFS and SMB storage used by Proxmox and other services.

---

## 2.5 Linux Container Inventory

The current LXC inventory is:

| CTID | Name | Status | Disk |
|---:|---|---|---:|
| 102 | Portainer | Running | 8 GB |
| 103 | Docker | Running | 20 GB |
| 104 | Vaultwarden | Running | 1 GB |
| 106 | Pi-hole | Running | 8 GB |
| 108 | Influx/Grafana | Running | 10 GB |
| 110 | Docker-LXC | Running | 40 GB |
| 200 | Immich | Running | 20 GB |
| 300 | Caddy | Running | 8 GB |

The lab uses LXCs for lightweight infrastructure and application services where a full virtual machine is not required.

---

## 2.6 VM vs LXC

The lab uses both virtualization models intentionally.

### Virtual Machines

VMs are currently used for:

- pfSense
- OpenMediaVault

VMs provide stronger isolation and a complete virtual hardware environment.

### Linux Containers

LXCs are currently used for:

- Portainer
- Docker
- Vaultwarden
- Pi-hole
- InfluxDB/Grafana
- Docker workloads
- Immich
- Caddy

LXC containers have lower overhead than full VMs and are useful for lightweight Linux services.

---

## 2.7 Proxmox Networking

The host currently has four virtual bridges:

```text
vmbr0
vmbr1
vmbr2
vmbr3
