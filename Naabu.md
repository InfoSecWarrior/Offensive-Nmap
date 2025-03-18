# Naabu

## Naabu - Fast Port Scanner

Naabu is a fast port scanner written in `Go` that allows scanning multiple hosts for open ports efficiently.

  →     https://github.com/projectdiscovery/naabu

### Installation

**Installing dependencies**

```bash
apt update
apt install -y libpcap-dev
```

```bash
go install -v github.com/projectdiscovery/naabu/v2/cmd/naabu@latest
```

Installation via Binary Release

```bash
wget https://github.com/projectdiscovery/naabu/releases/download/v2.0.3/naabu-linux-amd64.tar.gz -O naabu-linux-amd64.tar.gz
```

```bash
tar -xvf naabu-linux-amd64.tar.gz
```

- Extracting the archive

```bash
cp naabu-linux-amd64 /usr/local/bin/naabu
```

- Move the binary to `/usr/local/bin` for system-wide access:

---

## Naabu - Usage

```bash
naabu -h
```

### Update Naabu

```bash
naabu -up
```

---

### Basic Network Scans

Scan a Network (Ping Scan)

```bash
naabu -sn -host 102.168.1.1/24
```

---

### Silent Mode Scan

```bash
naabu -sn -slient -host 102.168.1.1/24
```

---

### **Host Discovery**

Naabu optionally supports multiple options to perform host discovery. Host discovery is optional and can be enabled with the `-wn` flag. `-sn` flag instructs the tool to perform host discovery only.

Available options to perform host discovery:

- **ARP** ping (`-arp`)
- TCP **SYN** ping (`-ps 80`)
- TCP **ACK** ping (`-pa 443`)
- ICMP **echo** ping (`-pe`)
- ICMP **timestamp** ping (`-pp`)
- ICMP **address mask** ping (`-pm`)
- IPv6 **neighbor discovery** (`-nd`)

---

## Scan Specific TCP Flags

### SYN Ping | (`-ps`)

```bash
naabu -sn -ps -host 102.168.1.1/24
```

### ACK Ping | (`-pa`)

```bash
naabu -sn -pa -host 102.168.1.1/24
```

---

## Scan Specific Hosts/Domains

### Scan a Single Host

```bash
naabu -host example.com
```

---

### Scan all Ports (1-65535)

```bash
naabu -p - -host example.com
```

  OR

```bash
naabu -p - -host 192.168.1.208
```

---

### Scan Specific Ports

```bash
naabu -p 22,80,443 -host example.com
```

---

### Scan Multiple Hosts

**Using a file containing multiple IPs (Targets)**

```bash
naabu -silent -Pn -scan-type c -c 1000 -p 80,443 -iL targets.txt
```

**Passing multiple targets directly in the command line**

```bash
naabu -silent -Pn -scan-type c -c 1000 -p 80,443 -host 192.168.1.208 192.168.1.209 192.168.1.210
```

---

### Set Concurrency for Faster Scanning

The `-c` flag is used to specify the number of concurrent hosts to scan at the same time. This helps control the load when scanning multiple targets.

```bash
naabu -c 50 -p - -host 192.168.1.1/24
```

- Scans up to 50 hosts concurrently
- Useful when scanning large lists to speed up the process.

```bash
naabu -list targets.txt -c 50
```

- **Scans up to 50 hosts concurrently** from `targets.txt`.
- Useful when scanning large lists to speed up the process.

```bash
naabu -list targets.txt -c 100 -rate 10000 -p 80,443
```

- Scans 100 hosts at a time with a **rate of 10,000 packets per second**, checking **ports 80 and 443**.

---

### Saving the Output  | (`-o`) / (`-j`) / (`-csv`)

```bash
naabu -host example.com -o scan-report.txt
```

```bash
naabu -host example.com -j scan-report.json
```

```bash
naabu -host example.com -csv scan-report.csv
```

```bash
naabu -host 192.168.1.208 -o 192.168.1.208-naabu.txt
```

---

### Verify | (`-verify`)

The `-verify` flag in **Naabu** is used to **confirm whether the detected open ports are actually open** by performing an additional connection check. This helps reduce false positives.

```bash
naabu -p - -verify -host example.com
```

---

### Scan Rate | (`-rate`)

**Increasing Scan Rate / Increase Speed (Adjust Rate)**

```bash
naabu -p - -host 192.168.1.208 -rate 10000
```

- Scans with a **rate of 10,000 packets per second.**
- **Higher `-rate` increases speed but may cause packet loss.**

---

### Scanning from Input Files

**Echo a Single IP**

```bash
echo 192.168.1.208 | naabu
```

---

**Scan a Subnet**

```bash
echo 192.168.1.1/24 | naabu
```

---

**Scan a Single IP Address**

```bash
naabu -host 192.168.1.1
```

---

## Advanced Scanning Options

### Scan with Specific Flags and Types

**Skip Host Discovery / Don’t Ping | (`-Pn`)**

```bash
naabu -v -Pn -scan-type c -p - -host 192.168.1.208
```

### **Explanation of Each Flag:**

| **Flag** | **Meaning** |
| --- | --- |
| `-v` | Verbose mode – shows detailed output during scanning. |
| `-Pn` | No host discovery – treats the host as alive without pinging first (useful for firewalled hosts). |
| `-scan-type c` | Uses a **CONNECT scan**, which establishes a full TCP connection (similar to Nmap’s `-sT` scan). |
| `-p -` | Scans **all 65,535 TCP ports**. |
| `-host 192.168.1.208` | Specifies the target IP address to scan. |

---

### Silent mode with maximum concurrency

```bash
naabu -silent -Pn -scan-type c -c 1000 -p - -host 192.168.1.208
```

---

### Scan from a List | (`-l` / `-list`)

```bash
naabu -l ip-list.txt -c 100 -rate 1000
```

- `ip-list.txt` should contain one IP address per line.

```bash
naabu -list domains.txt
```

- `domains.txt` should contain one domain per line.

```bash
cat ip-list.txt | naabu -silent -Pn -scan-type c -p -
```

- Here we are passing a list of IP addresses.

---

### Integrate with Nmap

```bash
naabu -host example.com -nmap-cli 'nmap -sV -sC'
```

---
