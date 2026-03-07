# Brute Force Validation Lab

## Objective

Simulate brute-force style traffic from the SECURITY_LAN toward the pfSense
management interface to validate firewall behavior, logging, and management
plane isolation.

This lab focuses on detection and defensive validation, not credential compromise.

---

## Network Context

MGMT_LAN: 192.168.1.0/24 (pfSense GUI on custom port 445)  
SECURITY_LAN: 192.168.2.0/24 (Kali attacker)  
SERVER_LAN: 192.168.3.0/24  
USER_LAN: 192.168.4.0/24  

---

## Method

1. Temporarily allowed TCP 445 from SECURITY_LAN to pfSense (floating/interface rule).
2. Temporarily enabled WebConfigurator listening on SECURITY_LAN.
3. Generated controlled HTTPS traffic using Hydra with small credential lists.
4. Observed firewall logs for repeated connection attempts.
5. Removed temporary rules and restored GUI binding to MGMT_LAN only.

Hydra command used:

hydra -L users.txt -P pass.txt -t 2 -W 3 https-get://192.168.1.254:445


---

## Observations

- Repeated TCP 445 connections from SECURITY_LAN were visible in pfSense firewall logs.
- Management plane exposure was confirmed only while temporary rules were active.
- After cleanup, SECURITY_LAN could no longer reach the pfSense GUI.

Hydra results produced HTTP-level false positives due to pfSense CSRF protection,
which is expected behavior.

---

## Outcome

- Management plane successfully isolated after testing
- Firewall logging confirmed attack-style traffic
- Temporary access removed and segmentation restored

---

## Lessons Learned

- GUI binding must match firewall rules for management access
- Floating and interface rules must be carefully ordered
- Custom GUI ports must be reflected in testing tools
- Brute-force simulation is useful for validating telemetry, not cracking pfSense

This lab validates proper management plane protection and segmentation design.

