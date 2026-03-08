# Splunk SIEM Integration – Cowrie, Fail2Ban, pfSense (with Universal Forwarder)

## Overview

This project documents the deployment of a **SIEM monitoring pipeline** in a cybersecurity homelab using Splunk. The environment simulates attacker activity and collects telemetry from multiple security controls for centralized analysis.

The lab integrates:

* **Cowrie Honeypot** – captures attacker interaction
* **Fail2Ban** – detects and blocks brute-force attempts
* **pfSense Firewall** – network-level filtering
* **Splunk Enterprise** – SIEM platform
* **Splunk Universal Forwarder** – log forwarding agent

Attack traffic is generated from a **Kali Linux machine** to simulate real adversary behavior.

---

## Lab Architecture

```
Kali Attacker
      │
      ▼
   pfSense Firewall
      │
      ▼
 Ubuntu Server
 ├── Cowrie Honeypot
 ├── Fail2Ban
 └── Splunk Universal Forwarder
      │
      ▼
 Splunk Enterprise SIEM
      │
      ▼
 Security Dashboards
```

---

# 1. Install Splunk Enterprise (SIEM)

Download Splunk Enterprise:

https://www.splunk.com/en_us/download/splunk-enterprise.html

Install:

```bash
sudo dpkg -i splunk*.deb
```

Start Splunk:

```bash
sudo /opt/splunk/bin/splunk start --accept-license --run-as-root
```

Verify service:

```bash
sudo /opt/splunk/bin/splunk status
```

Access Splunk web interface:

```
http://<server-ip>:8000
```

Example:

```
http://192.168.3.50:8000
```

---

# 2. Create Splunk Indexes

Indexes organize logs by data source.

Create indexes:

```bash
sudo /opt/splunk/bin/splunk add index honeypot --run-as-root
sudo /opt/splunk/bin/splunk add index auth --run-as-root
sudo /opt/splunk/bin/splunk add index firewall --run-as-root
sudo /opt/splunk/bin/splunk add index system --run-as-root
```

Verify:

```bash
sudo /opt/splunk/bin/splunk list index --run-as-root
```

---

# 3. Install Splunk Universal Forwarder

The **Universal Forwarder** sends logs from endpoints to the Splunk server.

Download:

https://www.splunk.com/en_us/download/universal-forwarder.html

Install:

```bash
sudo dpkg -i splunkforwarder*.deb
```

Start forwarder:

```bash
sudo /opt/splunkforwarder/bin/splunk start --accept-license
```

Enable at boot:

```bash
sudo /opt/splunkforwarder/bin/splunk enable boot-start
```

---

# 4. Connect Forwarder to Splunk Server

Configure the forwarder to send logs to Splunk.

```bash
sudo /opt/splunkforwarder/bin/splunk add forward-server 192.168.3.50:9997
```

Verify connection:

```bash
sudo /opt/splunkforwarder/bin/splunk list forward-server
```

Expected:

```
Active forwards:
192.168.3.50:9997
```

---

# 5. Enable Receiving on Splunk Server

On the Splunk server:

```
Settings → Forwarding and Receiving
```

Enable receiving on:

```
Port: 9997
```

---

# 6. Configure Log Inputs

Configure the Universal Forwarder to monitor security logs.

Add inputs:

### Cowrie Honeypot Logs

```bash
sudo /opt/splunkforwarder/bin/splunk add monitor /home/thato/cowrie/var/log/cowrie/cowrie.json -index honeypot
```

---

### Fail2Ban Logs

```bash
sudo /opt/splunkforwarder/bin/splunk add monitor /var/log/fail2ban.log -index auth
```

---

### System Logs

```bash
sudo /opt/splunkforwarder/bin/splunk add monitor /var/log/auth.log -index system
```

---

### pfSense Logs (via syslog)

Forward firewall logs to the Splunk server and monitor:

```
/var/log/syslog
```

Add monitor:

```bash
sudo /opt/splunkforwarder/bin/splunk add monitor /var/log/syslog -index firewall
```

---

# 7. Restart the Forwarder

```bash
sudo /opt/splunkforwarder/bin/splunk restart
```

---

# 8. Verify Log Ingestion

Open Splunk Search.

### Cowrie events

```
index=honeypot
```

### Failed SSH attempts

```
index=honeypot eventid=cowrie.login.failed
```

### Attacker commands

```
index=honeypot eventid=cowrie.command.input
```

### Fail2Ban bans

```
index=auth Ban
```

---

# 9. Cowrie Honeypot Deployment

Clone repository:

```bash
git clone https://github.com/cowrie/cowrie.git
cd cowrie
```

Create virtual environment:

```bash
python3 -m venv cowrie-env
source cowrie-env/bin/activate
```

Install dependencies:

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

Copy configuration:

```bash
cp etc/cowrie.cfg.dist etc/cowrie.cfg
```

Install Cowrie package:

```bash
pip install -e .
```

Start honeypot:

```bash
cowrie start
```

Check status:

```bash
cowrie status
```

---

# 10. Enable Malware Download Capture

Edit configuration:

```
~/cowrie/etc/cowrie.cfg
```

Enable downloads:

```
[download]
enabled = true
download_path = var/lib/cowrie/downloads
```

Captured files stored in:

```
~/cowrie/var/lib/cowrie/downloads
```

---

# 11. Attack Simulation

Generate SSH brute-force traffic from Kali:

```bash
hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://192.168.3.50 -s 2222
```

Observed activity:

* repeated login attempts
* password guessing
* command execution
* file download attempts

---

# 12. Example Splunk Searches

### Top attacking IPs

```
index=honeypot
| stats count by src_ip
| sort -count
```

---

### Most attempted passwords

```
index=honeypot eventid=cowrie.login.failed
| stats count by password
```

---

### Attacker commands

```
index=honeypot eventid=cowrie.command.input
| stats count by input
```

---

### Fail2Ban bans

```
index=auth Ban
```

---

# 13. Splunk Dashboard Panels

Recommended dashboard components:

Top Attacking IPs

```
index=honeypot
| stats count by src_ip
```

Most Used Passwords

```
index=honeypot eventid=cowrie.login.failed
| stats count by password
```

Attacker Commands

```
index=honeypot eventid=cowrie.command.input
| stats count by input
```

Attack Timeline

```
index=honeypot
| timechart count
```

---

# 14. Security Monitoring Pipeline

```
Attacker (Kali)
      │
      ▼
SSH brute force
      │
      ▼
Cowrie Honeypot logs attacker behavior
      │
      ▼
Fail2Ban detects brute force
      │
      ▼
pfSense blocks attacker IP
      │
      ▼
Splunk Universal Forwarder sends logs
      │
      ▼
Splunk SIEM visualizes activity
```

---

# Skills Demonstrated

This lab demonstrates hands-on experience with:

* SIEM deployment and configuration
* log ingestion and parsing
* honeypot monitoring
* brute-force detection
* network security monitoring
* threat investigation workflows

---

# Future Enhancements

Planned improvements include:

* MITRE ATT&CK detection mapping
* GeoIP attacker visualization
* automated alerting in Splunk
* threat intelligence enrichment
* Wazuh integration
