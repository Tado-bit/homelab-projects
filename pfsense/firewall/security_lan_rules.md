# SECURITY_LAN Firewall Rules

Rules are evaluated top-down (first match wins).

1. Block SECURITY_LAN → This Firewall
2. Block SECURITY_LAN → MGMT_LAN
3. Block SECURITY_LAN → SERVER_LAN_NET
4. Allow SECURITY_LAN → Internet (any)

Logging enabled on all block rules.
