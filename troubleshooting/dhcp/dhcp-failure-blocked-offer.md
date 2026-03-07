## Evidence
Packet capture showed repeated DHCP DISCOVER packets from 0.0.0.0 to
255.255.255.255 with no DHCPOFFER or DHCPACK responses observed.

## Lessons Learned
- DHCP DISCOVER without OFFER indicates server-side failure
- When pfSense is the DHCP server, firewall rules may not block DHCP replies
- Disabling the DHCP service is the correct way to simulate DHCP outages
- Packet capture is essential to distinguish client vs server failures
