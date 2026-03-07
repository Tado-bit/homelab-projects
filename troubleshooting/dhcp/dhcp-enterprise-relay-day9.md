# Enterprise DHCP with Relay (Day 9)

## Overview
This lab implements enterprise-style DHCP by moving the DHCP service off
pfSense and using DHCP relay to serve multiple VLANs from a centralized
DHCP server.

The goal was to validate correct DHCP relay behavior using packet capture
and to understand how subnet selection occurs in a routed environment.

---

## Network Design
- Firewall / Router: pfSense
- DHCP Server: Linux (isc-dhcp-server) on SERVER_LAN
- Client Networks:
  - MGMT_LAN (192.168.1.0/24)
  - SECURITY_LAN (192.168.3.0/24)

pfSense acts only as a router and DHCP relay. It does not provide DHCP
services on any interface.

---

## Configuration Summary

### pfSense
- DHCP Server: Disabled on all interfaces
- DHCP Relay: Enabled
- Downstream Interfaces:
  - MGMT_LAN
  - SECURITY_LAN
- Upstream DHCP Server:
  - SERVER_LAN DHCP server IP

### DHCP Server
- isc-dhcp-server running on SERVER_LAN
- DHCP scopes defined for:
  - MGMT_LAN (192.168.1.0/24)
  - SECURITY_LAN (192.168.3.0/24)
- SERVER_LAN subnet defined without an address pool
- Subnets grouped using `shared-network` to support relay

---

## Validation and Packet Capture

### Client Behavior
Clients broadcast DHCP DISCOVER messages on their local VLAN.

### Relay Behavior
pfSense relays DHCP requests to the DHCP server and inserts the gateway
IP (GIADDR) corresponding to the interface on which the request was
received.

### Server Behavior
The DHCP server selects the address scope based on the GIADDR value,
not on the physical location of the server.

---

## Observed Behavior
- Clients on MGMT_LAN received addresses from 192.168.1.0/24
- Clients on SECURITY_LAN received addresses from 192.168.3.0/24
- Packet capture confirmed DISCOVER, OFFER, REQUEST, and ACK exchanges
  across VLANs via the relay

This behavior confirms correct enterprise DHCP relay operation.

---

## Lessons Learned
- DHCP relay assigns addresses based on the interface where the request
  is received
- The DHCP server uses GIADDR to select the correct subnet
- Firewalls should not act as long-term DHCP servers in enterprise designs
- Packet capture is essential to validate relay behavior
