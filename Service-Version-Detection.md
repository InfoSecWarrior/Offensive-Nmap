### Service Version Detection | ( `-sV` )

The Service Version Detection (`-sV`) scan is used to determine the specific software and version running on open ports. Instead of just identifying which ports are open, this scan probes services in more detail to gather information such as:

- The Application Name (e.g., Apache, OpenSSH, MySQL)
- The Software Version (e.g., Apache 2.4.41, OpenSSH 8.4p1)
- The Possible Device type (e.g., printer, router, server)
- Additional details like SSL/TLS support, SSH algorithms, etc.

**Behavior of (`-sV`) Scan :**

Initial Port Scan

- Nmap first scans for open ports using techniques like SYN, TCP, or UDP scans.

Probing Open Ports

- Nmap sends various requests, including:
    - Banner Grabbing (capturing responses to common requests)
    - Protocol-specific queries (e.g., SMTP HELO, HTTP GET /`)
    - TLS/SSL handshakes to identify secure services

**Analyzing Responses**

- Nmap compares received responses service and version signature databases
- If a perfect match isn't found, it makes an educated guess.

**Port Status Identification :**

**Open Port with Identified Service:**

Nmap successfully determines the service and its version.

Port                    Service               Version
22/tcp                  OpenSSH               OpenSSH 8.4p1 Ubuntu
80/tcp                  Apache                Apache httpd 2.4.41
443/tcp                 Ngnix                 Ngnix 1.18.0

**Open Port with Unknown Service:**

- If the response is unrecognized, Nmap labels it as "unknown" but still provides hints.
    
    Port                         Service              Version
    
    8080/tcp                     unknown              Possibly a web server
    

**Closed Port:**

- The port does not respond or actively rejects connections.

**Filtered Port:**

- A firewall or security rule blocks the scan, preventing detection.

**Advantages of Service Version Detection (`-sV`)**

- Accurate Identification — Helps determine running applications and their versions.
- Exposes Vulnerabilities — Version details can be matched against known exploits.
- Supports Custom Probes — Nmap allows creating custom service detection scripts.

**Disadvantages of Service Version Detection (`-sV`)**

- Can Trigger Security Alerts — Some IDS/IPS detect aggressive fingerprinting.
- Not Always 100% Accurate — Services may lie about their versions (banner obfuscation).
- Slower Than Basic Scans — Probing takes additional time, especially with many open ports.

**Use Cases :**

- Identifying running software and potential vulnerabilities.
- Performing penetration tests to map services on a target network.
- Checking for outdated or misconfigured services.

### **Fine-Tuning `-SV` for Accuracy:**

| **Option** | **Description** |
| --- | --- |
| `--version-intensity <0-9>` | Controls probe depth (higher = more accurate, but slower). Default: **7** |
| `--version-light` | Fast but less accurate version detection. |
| `--version-all` | Tries **every possible probe** for maximum accuracy. |
| `--version-trace` | Shows detailed debugging info about version scanning. |

---

```bash
nmap -v -sT -sV -p 22 192.168.1.208
```

```bash
nmap -sV 192.168.1.207
```

```bash
nmap -sV -p 22,80,443 example.com
```

```bash
nmap -sV --version-intensity 9 192.168.1.207
```

```bash
nmap -sV --version-all target.com
```

```bash
nmap -sV --script=banner 192.168.1.207
```

---

```bash
nmap -v -sT -p- 192.168.1.207
```

First perform a port scan of the target

```bash
nmap -v -T4 -sT -p- 192.168.1.207
```

Use this switch (-T4) if the port scanning is taking very long time.

***Now perform version detection on the ports which are found open in the scan, so that you can work on ports when nmap is detecting the version of the services.***

```bash
nmap -v -sV -sT -p 21,22,25,80,139,443,445,512,513,514,666,3306,3632,5091,6001,8080,8443,9080,9443 192.168.1.207
```

---

### `--version-light`

- **Faster but less accurate service detection.**
- Uses **fewer probes**, reducing scan time but **might miss some versions**.

```bash
nmap -sV --version-light 192.168.1.208
```

---

### **`-version-intensity <level>`**

- Controls the **depth** of version detection (range: `0-9`).
- **Higher values** use **more probes**, increasing accuracy but slowing down the scan.

| **Intensity Level** | **Behavior** |
| --- | --- |
| `0` | Only tries the **most likely** probe for each port. |
| `2` | Default level for `--version-light`. |
| `7` | **Default level for `-sV`**, balanced speed & accuracy. |
| `9` | Tries **every probe**, slowest but most accurate. |

```bash
nmap -sV --version-intensity 9 192.168.1.208
```

---

### **`--version-all`**

- **Tries every available probe** for **maximum accuracy**.
- Equivalent to `--version-intensity 9`.
- **Very slow** but ensures Nmap finds the most precise version info.

```bash
nmap -sV --version-all 192.168.1.1
```

---

### **`--version-trace`**

- **Debugging mode** for service detection.
- Shows **detailed logs** of each probe sent and response received.

Useful for troubleshooting when **service detection is failing**.

```bash
nmap -sV --version-trace 192.168.1.1
```

---

### **Comparison Table**

| **Option** | **Purpose** | **Speed** | **Accuracy** |
| --- | --- | --- | --- |
| `--version-light` | Fastest, less accurate | Fast | Lower |
| `--version-intensity <level>` | Fine-tuned control (0-9) | Medium | Medium |
| `--version-all` | Most detailed version scan | Slow | Very High |
| `--version-trace` | Debugging tool | Slow | High |

**Fast scan:** `--version-light`

**Balanced scan (default):** `--version-intensity 7`

**High accuracy (but slow):** `--version-all`

**Debugging issues:** `--version-trace`

---

### **Banner Grabbing | (`--script=banner`)**

```bash
nmap -v -sT --script=banner -p 22,80,443 192.168.1.208
```

**banner information** from open ports.

```bash
nmap -v -Pn -sT --script=banner -p 22,80,443 192.168.1.208
```

```bash
nmap -v -Pn -sT -sV --script=banner 192.168.1.1
```

**Best method**: Uses **both service detection & banner grabbing** to improve accuracy.

---
