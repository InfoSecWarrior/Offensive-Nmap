```bash
cat /usr/share/nmap/nmap-services | head -n 50
```

*Show the frequency of the used ports (how often the port is found open in real life case scenarios*

Service name                  —    The name of the service typically associated with this port.

Port number/protocol          —    The port number and transport protocol (TCP/UDP).

Open-frequency                —    A decimal value indicating how often Nmap has observed this port open in real-world scans.

Optional comments             —    Additional information about the service.

**Meaning of Open-Frequency:**

— It is a statical measure based on scans conducted by Nmap across a wide range of networks.

— A higher value indicated that the port is commonly found open.

— A lower value suggests that the port is rarely open.

---

### Nmap Fast Scan | ( `-F` )

```bash
nmap -F 192.168.1.207
```

-F for fast scan, scans Top Used 100 Ports.

---

### Scan Top most used ports | ( `--top-ports` )

```bash
nmap --top-ports 10 192.168.1.207
```

‘10’ after the ‘--top-ports’ will scan the top most used 10 ports

```bash
nmap --top-ports 50 192.168.1.207
```

This will scan top most used 50 ports

---

### Custom port(s) Scanning | ( `-p` )

```bash
nmap -p 21 192.168.1.207
```

This will only scan port number 21

```bash
nmap -p 21 192.168.1.207
```

**Ports in CSV (comma-separated values)**

```bash
nmap -p 21,22,23,25,80,443,445 192.168.1.207
```

This will scan the given ports in the command

**Ports in Range**

```bash
nmap -p 80-800 192.168.1.207
```

This will scan all ports between 80 to 800 including them too

```bash
nmap -p 21,22,23,25,80,443,445,8000-8999 192.168.1.207
```

This will scan the mentioned ports as well as all ports in the range of 8000 to 8999

**Scanning All ports**

```bash
nmap -p 0-65535 192.168.1.207
```

This will scan all ports including the port number ‘0’

  OR

```bash
nmap -p - 192.168.1.207
```

  OR

```bash
nmap -p- 192.168.1.207
```

### Scan TCP Ports | ( `-p T:` )

```bash
nmap -p T:100-900 192.168.1.207
```

This will scan TCP ports from range 100 to 900

### Scan UDP Ports | ( `-p U:` )

```bash
nmap -sU -sT -p T:100-900,U:53,69,67,68 192.168.1.207
```

This will scan the UDP ports entered in the command.

### Scan UDP Ports | ( `-sU:` )

```bash
nmap -sU 192.168.1.207
```

This will scan top 1000 most used UDP ports.

```bash
nmap -sU --top-ports 10 192.168.1.207
```

Scans 10 top most used UDP ports.

---

### Scanning both TCP & UDP ports ( Custom )

```bash
nmap -sU -sS -p t:21,23,22,25,80,443,445,U:53,68,67 192.168.1.207
```

---

### Exclude Ports | ( `--exclude-ports` )

**The service on port 9100/TCP is jetdirect ( printers use that )**

This is used when you want to exclude some ports from being scanned e.g., Ports used by printers etc.

```bash
nmap -v -p- --exclude-ports 9100 192.168.1.105
```

This will scan all ports of the target IP except port number 9100/TCP ( This port is used by printers )

```bash
nmap -v -p- --exclude-ports 9100,515 192.168.1.105
```

```bash
nmap -v -p- --exclude-ports 9000-9999 192.168.1.105
```

### Scanning ports by service name

```bash
nmap -v -p http 192.168.1.207
```

This will scan all ports running the ‘HTTP’ protocol.

```bash
nmap -v -p http* 192.168.1.207
```

Scanning All HTTP-related Ports with Wildcard (`http*`)

```bash
nmap -v -p ssh* 192.168.1.207
```

Scanning All SSH-related Ports with Wildcard

```bash
nmap -v -p ftp* 192.168.1.207
```

Scanning All FTP-related Ports with Wildcard

```bash
nmap -v -p *ftp 192.168.1.207
```

Scanning All Ports with Names Ending in 'FTP' (`*ftp`)

```bash
nmap -v -p *ftp* 192.168.1.207
```

Scanning All Ports with 'FTP' in Their Name (`*ftp*`)

---

### Don’t randomize port scan | ( `-r` )

**"By default, Nmap scans ports in a random order. Using the `-r` switch disables randomization, meaning the ports will be scanned in numerical order."**

```bash
nmap -v -r -p 1-10 192.168.1.207
```

---

### Scanning ports by ratio | ( `--port-ratio` )

### `-port-ratio` Switch in Nmap

The `--port-ratio` option in Nmap is used to filter and scan only those ports that are commonly open on hosts, based on statistical data. This switch is useful when you want to scan only frequently used ports instead of scanning all possible ports, which can save time and reduce network load.

The `--port-ratio` option is used to filter ports based on their open-frequency ratio, which is derived from the nmap-services file.

### **How It Works**

- Nmap maintains statistical data on the likelihood of ports being open across different scans.
- The `-port-ratio` switch allows you to specify a threshold (between 0.0 and 1.0), where:
    - **`-port-ratio 0.9`** scans only ports that have at least a **90% chance** of being open.
    - **`-port-ratio 0.5`** scans ports that have at least a **50% chance** of being open.
    - **`-port-ratio 0.1`** scans ports that have at least a **10% chance** of being open.

### **Example Usage**

```bash
nmap --port-ratio 0.8 192.168.1.207
```

- This will scan only ports that are open at least 80% of the time in Nmap's dataset.
- It reduces unnecessary scanning of rarely open ports.

### **When to Use `-port-ratio`**

- When performing a **quick scan** to check only the most commonly open ports.
- To **reduce scan time** and **network footprint** when scanning large networks.
- When focusing on **high-probability attack surfaces** rather than scanning all 65,535 ports.

```bash
awk '$2~/udp$/' /usr/share/nmap/nmap-services | sort -r -k3 | head -n 100
```

There will be the ports with their frequency; e.g., ‘`0.060743`’

---
