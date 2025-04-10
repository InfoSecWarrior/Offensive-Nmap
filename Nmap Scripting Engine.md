### **Nmap Scripting Engine (NSE) - Definition**

The **Nmap Scripting Engine (NSE)** is a flexible and powerful feature of Nmap that enables users to write and execute custom scripts to automate a wide variety of networking tasks. These scripts are written in the Lua programming language and are used to extend Nmap's capabilities beyond basic port scanning.

NSE allows for:
- **Advanced service detection**
- **Vulnerability detection and exploitation**
- **Brute-force authentication testing**
- **Network discovery**
- **Configuration auditing**
- **Malware detection**

NSE scripts are organized into categories like `auth`, `brute`, `discovery`, `vuln`, and `safe`, among others. This modular system helps both security professionals and administrators perform complex scanning tasks easily and efficiently.


## Nmap NSE (Nmap Scripting Engine) Usage Notes

---

###  Update Nmap Script Database

```bash
nmap --script-updatedb
```

---

###  Location of Nmap Scripts

```bash
/usr/share/nmap/scripts
```

####  Working with the Script Database
```bash
# List all scripts
ls /usr/share/nmap/scripts

# Show database file
cat /usr/share/nmap/scripts/script.db

# Extract script names
cat /usr/share/nmap/scripts/script.db | cut -d "\"" -f 2

# Search specific service scripts
ls /usr/share/nmap/scripts | grep ftp
ls /usr/share/nmap/scripts | grep smb

# Search keywords in script.db
grep smb /usr/share/nmap/scripts/script.db
grep ftp /usr/share/nmap/scripts/script.db
grep brute /usr/share/nmap/scripts/script.db
grep exploit /usr/share/nmap/scripts/script.db
grep version /usr/share/nmap/scripts/script.db
```

---

###  Default Script Scans

```bash
nmap -v -Pn -sT -sV -sC -A -O -p- 192.168.1.236
nmap -v -Pn -sT -sV -A -O -p- --script=default 192.168.1.236
```

---

###  Scan Using Specific Scripts

```bash
nmap -v -Pn -sT -sV -A -O -p 80 --script=http-title.nse 192.168.1.236
nmap -v -Pn -sT -sV -A -O -p 80 --script=http-server-header.nse 192.168.1.236
```

---

###  Nmap Script Categories

| Category     | Description |
|--------------|-------------|
| `auth`       | Auth bypass, brute-force |
| `broadcast`  | LAN service discovery |
| `brute`      | Brute-force login attempts |
| `discovery`  | Host/service discovery |
| `dos`        | Denial of Service checks |
| `exploit`    | Exploits known vulnerabilities |
| `external`   | Uses external services like WHOIS |
| `fuzzer`     | Fuzz testing for protocol robustness |
| `intrusive`  | May cause performance issues or alerts |
| `malware`    | Malware detection |
| `safe`       | Non-intrusive scripts |
| `version`    | Enhances version detection |
| `vuln`       | Detects vulnerabilities |
| `scada`      | Targets ICS/SCADA systems |

####  Category Examples
```bash
nmap -v -Pn -sT -sV -A -O -p- --script=discovery 192.168.1.236
nmap -v -Pn -sT -sV -A -O -p- --script=auth 192.168.1.236
nmap -v -Pn -sT -sV -A -O -p- --script=vuln 192.168.1.236
nmap -v -Pn -sT -sV -A -O -p- --script=brute 192.168.1.236
```

---

###  Advanced Script Usage

####  Run a Specific Script:
```bash
nmap --script "ftp-anon" 192.168.1.236
```

####  Run Multiple Scripts:
```bash
nmap --script "ssh-brute,ftp-brute,smb-brute" 192.168.1.236
```

####  Use Wildcards:
```bash
nmap --script "http-*" 192.168.1.236
```

####  Keyword Match:
```bash
nmap --script "*smb*" 192.168.1.236
```

####  Regex Match:
```bash
nmap --script "^(smb|ftp)-.*" 192.168.1.236
```

####  Exclude Specific Scripts:
```bash
nmap --script "default and not safe" 192.168.1.236
nmap --script "vuln and not http-shellshock" 192.168.1.236
```

####  Run a Custom Script File:
```bash
nmap --script "/path/to/custom-script.nse" 192.168.1.236
```
>  *Remember to run `nmap --script-updatedb` after placing custom scripts in the default script directory.*

