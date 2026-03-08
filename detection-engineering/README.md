Cowrie Honeypot Integration with Splunk
Objective
Integrate Cowrie SSH/Telnet honeypot logs into Splunk for real-time attack monitoring.

Architecture
Kali attacker VM
│ ▼ Cowrie Honeypot (Ubuntu host)
│ ▼ JSON log files
│ ▼ Splunk Universal Forwarder
│ ▼ Splunk Enterprise (index=cowrie)

Cowrie Logging
Logs located at: /home/cowrie/cowrie/log
Format: JSON
Forwarded to Splunk via Universal Forwarder
Index: cowrie
Show all SSH/Telnet attacks
index=cowrie | table _time src_ip username password eventid

Count attacks by IP
index=cowrie | stats count by src_ip

Show timeline of bans
index=cowrie | timechart count by src_ip

Result
Cowrie logs ingested into Splunk

Attack attempts visible in real-time

IPs and credentials captured for analysis

Combined with Fail2Ban, the lab demonstrates active defense + monitoring
