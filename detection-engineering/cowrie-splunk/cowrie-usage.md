# Cowrie Utility Commands

Cowrie includes several built-in utilities used to manage the honeypot, modify the fake environment, and analyze attacker sessions.

These tools help simulate realistic systems and investigate attacker behavior captured by the honeypot.

---

# cowrie

The `cowrie` command is used to control the honeypot service.

Start Cowrie:

```
cowrie start
```

Stop Cowrie:

```
cowrie stop
```

Restart Cowrie:

```
cowrie restart
```

Check status:

```
cowrie status
```

Typical usage example:

```
cowrie restart
```

This reloads the configuration and restarts the honeypot service.

---

# fsctl

`fsctl` is used to modify Cowrie’s **fake filesystem**.

Cowrie presents attackers with a simulated Linux environment called `honeyfs`.

This command allows administrators to manipulate files visible to attackers.

Example:

```
fsctl ls /
```

List files in the fake filesystem.

Example:

```
fsctl cat /etc/passwd
```

View simulated system files.

Use cases include:

* modifying fake system banners
* adding fake files
* creating more realistic environments

---

# createfs

`createfs` generates a **fake filesystem from a real Linux system**.

This allows the honeypot to closely resemble a real server.

Example usage:

```
createfs /
```

This scans the system and builds a filesystem template.

The generated filesystem can be placed inside:

```
cowrie/honeyfs
```

This improves honeypot realism and attacker engagement.

---

# playlog

`playlog` replays attacker sessions captured by Cowrie.

Cowrie records interactive SSH sessions in the `tty` directory.

Example:

```
playlog var/lib/cowrie/tty/<session-id>
```

This recreates the attacker terminal session.

Example output:

```
root@server:~# uname -a
Linux server 5.4.0
```

Use cases:

* analyzing attacker behavior
* investigating commands executed
* training and demonstrations

---

# asciinema

The `asciinema` utility converts Cowrie session logs into **asciinema recordings**.

These recordings can be replayed in the terminal or shared online.

Example:

```
asciinema var/lib/cowrie/tty/<session-id>
```

The output can be uploaded to:

```
https://asciinema.org
```

This produces a **terminal recording of the attacker session**, useful for:

* incident reports
* demonstrations
* security training

---

# Cowrie Log Locations

Important directories used by Cowrie.

Session logs:

```
var/lib/cowrie/tty
```

JSON event logs:

```
var/log/cowrie/cowrie.json
```

Text logs:

```
var/log/cowrie/cowrie.log
```

Downloaded attacker files:

```
var/lib/cowrie/downloads
```

---

# Example Analysis Workflow

1. Attacker connects to honeypot

```
ssh root@honeypot
```

2. Cowrie records session.

3. Investigate commands used:

```
playlog var/lib/cowrie/tty/<session-id>
```

4. Check downloaded malware:

```
ls var/lib/cowrie/downloads
```

5. Analyze activity in Splunk.

```
index=honeypot
```

---

# Why These Tools Matter

These utilities allow defenders to:

* replay attacker sessions
* analyze intrusion techniques
* capture malware downloads
* simulate realistic systems

This makes Cowrie a valuable tool for **threat intelligence and security monitoring labs**.
