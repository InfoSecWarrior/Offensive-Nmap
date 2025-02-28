# Nmap Scripting Engine (NSE) — Script Scan | (`-sC` or `--script`)

The Nmap Scripting Engine (NSE) allows users to automate scanning tasks, detect vulnerabilities, and gather additional information about a target. It works by running scripts written in Lua that extend Nmap's capabilities.

**Syntax :**

```bash
nmap -sC <target>
```

```bash
nmap --script <script-name> <target>
```

```bash
nmap --script <category> <target>
```

```bash
nmap script "<expression>" <target>
```

When you use `-sC`, Nmap runs a set of default scripts that provide additional information about a target, such as:

- Service detection
- OS fingerprinting
- SSL/TLS information
- Basic vulnerability checks

```bash
nmap -v -Pn -sT -sV -sC -A -O -p- 192.168.1.208
```

**Script Categories in NSE :**

Nmap scripts are organized into different categories:

| **Category** | **Description** |
| --- | --- |
| auth | Authentication bypass & brute-force attacks |
| broadcast | Network discovery scripts |
| brute | Brute-force attack scripts |
| discovery | Identifies hosts, services, and configurations |
| dos | Denial-of-service (DoS) testing |
| exploit | Known vulnerability exploitation |
| external | Uses external services (e.g., WHOIS, Shodan) |
| fuzzer | Sends unexpected data to test robustness |
| intrusive | May affect performance or trigger security alerts |
| malware | Detects malware-infected hosts |
| safe | No harmful impact on targets |
| version | Improves service version detection |
| vuln | Detects vulnerabilities on a target |

Default is also a category

### Locating NSE Scripts :

To see all available scripts :

```bash
ls /usr/share/nmap/scripts/
```

```bash
ls /usr/share/nmap/scripts/ | wc -l
```

Total number of scripts

```bash
nmap --script-updatedb
```

```bash
ls /usr/share/nmap/scripts/ | grep .db$
```

This is the script database file of nmap

```bash
cat /usr/share/nmap/scripts/script.db
```

```bash
cat /usr/share/nmap/scripts/script.db | cut -d "\"" -f 2 | wc -l
```

```bash
grep smb /usr/share/nmap/scripts/script.db
```

Filtering all nmap scripts for **‘smb’**

```bash
ls /usr/share/nmap/scripts/ | grep smb
```

Filtering all nmap scripts for **‘smb’**

```bash
grep ftp /usr/share/nmap/scripts/script.db
```

Filtering all nmap scripts for **‘ftp’**

```bash
ls /usr/share/nmap/scripts/ | grep ftp
```

Filtering all nmap scripts for **‘ftp’**

### Filtering / Searching nmap scripts by ‘category’.

```bash
grep brute /usr/share/nmap/scripts/script.db
```

```bash
grep exploit /usr/share/nmap/scripts/script.db
```

```bash
grep version /usr/share/nmap/scripts/script.db
```

---

### Nmap Default Script | ( `--script=default` )

Runs the default set of Nmap Scripting Engine (NSE) scripts, which provide additional information about open ports, services, and possible vulnerabilities.

```bash
nmap -v -Pn -sV -sT -A -O -p- --script=default 192.168.1.208
```

Performs a scan using Nmap’s built-in default scripts.

Performs a scan using Nmap’s built-in default scripts.

**Key Features:**

- Detects common vulnerabilities
- Retrieves service versions
- Extracts banner information
- Performs light brute-force testing (where applicable)

---

```bash
nmap -v -Pn -sV -sT -A -O -p- --script=default 192.168.1.208
```

  OR

```bash
nmap -v -Pn -sT -sV -sC -A -O -p- 192.168.1.208
```

---

**Syntaxes:**

```bash
nmap --script <script-name> <target>
```

```bash
nmap --script <category> <target>
```

```bash
nmap --script "<expression>" <target>
```

---

**Particular (Each) scripts is applicable on particular ports, like HTTP scripts will work on 80 & 8080**

**Example :**

```bash
cd /usr/share/nmap/scripts
```

```bash
ls -lha | grep http
```

Selecting any script for example

```bash
nmap -v -Pn -sT -sV -A -O -p 80 --script=http-enum.nse 192.168.1.208
```

**Here we are using `http-enum.nse` script and on port `80`**

```bash
nmap -v -Pn -sT -sV -A -O -p 80 --script=http-headers.nse 192.168.1.208
```

---

### Script Category Scan | (`--script=<category>`)

```bash
nmap -v -Pn -sT -sV -A -O -p- --script=discovery 192.168.1.208
```

Using ‘discovery’ category here

---

```bash
nmap -v -Pn -sT -sV -A -O -p- --script=auth 192.168.1.208
```

Runs scripts to check for authentication weaknesses, such as:

- ftp-anon — Checks for anonymous FTP login.
- smb-brute — Attempts SMB login with brute force.
- http-auth — Detect HTTP authentication methods.

---

```bash
nmap -v -Pn -sT -sV -A -O -p- --script=default 192.168.1.208
```

Default is also a category

---

```bash
nmap -v -Pn -sT -sV -A -O -p- --script=brute 192.168.1.208
```

Runs brute-force attack scripts, including:

- ssh-brute — SSH password cracking.
- ftp-brute — FTP brute-force attack.
- http-brute — HTTP login brute-force

---

```bash
nmap -v -Pn -sT -sV -A -O -p- --script=exploit 192.168.1.208
```

Runs script that exploit vulnerabilities, including:

- smb-psexec — Executes commands on Windows via SMB.
- http-shellshock — Test for Shellshock vulnerability.

---

```bash
nmap -v -Pn -sT -sV -A -O -p- --script=malware 192.168.1.208
```

Runs scripts that check for malware-infected services, including:

- http-malware-host — Detects if a web host is serving malware.
- smb-vuln-conficker — Detects Conficker worm.

---

```bash
nmap -v -Pn -sT -sV -A -O -p- --script=dos 192.168.1.208
```

Runs scripts that check for DoS vulnerabilities, such as:

- http-slowloris — Checks for Slowloris DoS vulnerability.
- snmp-hh3c-dos — Checks SNMP DoS vulnerability.

---

```bash
nmap -v -Pn -sT -sV -A -O -p- --script=discovery 192.168.1.208
```

Runs scripts that father network and host information, such as:

- snmp-brute — Brute-force SNMP community strings.
- nbstat — Retrieves NetBIOS info.
- dns-brute — Attempts DNS subdomain enumeration

---

```bash
nmap -v -Pn -sT -sV -A -O -p- --script=vuln 192.168.1.208
```

Runs scripts that detect known vulnerabilities, including:

- smb-vuln-ms-08-067 — Checks for the MS08-067 vulnerability.
- http-shellshock — Tests for Shellshock vulnerability.
- ssl-heartbleed — Checks for Heartbleed.

---

```bash
nmap -v -Pn -sT -sV -A -O -p- --script=scada 192.168.1.208
```

Runs scripts that test SCADA/ICS (Industrial Control Systems) devices, such as:

- modbus-discover — Enumerates Modbus devices.
- iec-identify — Identifies IEC 60870-5-104 SCADA devices.

---

| **Category** | **Nmap Command** |
| --- | --- |
| Default Scripts | nmap --script=default 192.168.1.100 |
| Authentication | nmap --script=auth 192.168.1.100 |
| Vulnerability | nmap --script=vuln 192.168.1.100 |
| Discovery | nmap --script=discovery 192.168.1.100 |
| Web Application | nmap --script=http 192.168.1.100 |
| Brute-Force | nmap --script=brute  192.168.1.100 |
| Exploitation | nmap --script=exploit  192.168.1.100 |
| Malware Detection | nmap --script=malware  192.168.1.100 |
| DOS Testing | nmap --script=dos  192.168.1.100 |
| SCADA/ICS | nmap --script=scada  192.168.1.100 |

---

### Script Expression Scan | (`--script "<expression>"`)

Allows you to run Nmap Scripting Engine (NSE) scripts using specific expressions, such as wildcard (`*`), Lua patterns, or logical operators. This helps target specific scripts instead of entire categories.

**To run a specific script :**

```bash
nmap --script "ftp-anon" 192.168.1.208
```

**Run multiple scripts by separating them with commas:**

```bash
nmap --script "ssh-brute,ftp-brute,smb-brute" 192.168.1.208
```

**Using Wildcard (`*`) to run multiple scripts matching a pattern:**

```bash
nmap --script "http-*" 192.168.1.208
```

**Running Scripts Based on a Keyword**

```bash
nmap --script "*smb" 192.168.1.208
```

**Running Scripts on Regular Expressions:**

```bash
nmap --script "^(smb|ftp)-.*" 192.168.1.208
```

**Exclude certain scripts using ! :**

```bash
nmap --script "dafault and not safe" 192.168.1.208
```

```bash
nmap --script "vuln and not http-shellshock" 192.168.1.208
```

**Run a Custom Script File :**

```bash
nmap --script "/path/to/custom-script.nse" 192.168.1.208
```

---
