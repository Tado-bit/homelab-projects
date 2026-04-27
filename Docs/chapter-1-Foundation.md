# Chapter 1: Lab Foundation & Environment Setup

This chapter covers the initial setup of the homelab environment, including Proxmox deployment and early networking configuration.

---

## Objectives

- Install and configure Proxmox
- Understand virtual networking (vmbr interfaces)
- Establish initial connectivity
- Identify and resolve foundational networking issues

---

## Initial Setup

- Installed Proxmox VE on dedicated hardware
- Configured primary network bridge (vmbr0)
- Attempted initial network access and routing setup

---

## Key Challenges

### Loss of Proxmox Access

- Incorrect network configuration resulted in loss of web UI access
- Gateway misconfiguration caused routing failures

### Routing Issues

- Invalid gateway errors when attempting manual route configuration
- Misunderstanding of network boundaries between host and virtual network

---

## Troubleshooting Approach

- Used Proxmox console for recovery
- Verified IP addressing and subnet alignment
- Reconfigured network interfaces manually
- Restarted networking services

---

## Key Lessons Learned

- The hypervisor must not function as a router
- Proper gateway configuration is critical
- Console access is essential for recovery
- Always validate network design before applying changes

---

## Outcome

- Stable Proxmox installation
- Reliable access to management interface
- Foundation ready for firewall deployment
