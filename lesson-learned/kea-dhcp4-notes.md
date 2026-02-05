# Kea DHCP – Initial Study Notes

## Why Kea
- Replacement for isc-dhcp-server
- Actively maintained
- API-driven
- Designed for relay-based deployments

## Key Differences from ISC DHCP
- JSON configuration
- Modular architecture
- External database support
- REST control interface

## Intended Use in Lab
- Central DHCP server
- pfSense as DHCP relay
- Multiple VLANs served from one authority

## Status
- Installed but disabled
- Studying configuration and design only
