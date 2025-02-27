### Operating System Detection | ( `-O` )

The OS Detection (`-O`) scan is used to identify the operating system running on a target machine. It works by analyzing how a system responds to specially crafted network packets and comparing these responses against a vast database of OS fingerprints.

**Behavior of `-O` Scan :**

Initial Port Scan

- Nmap first performs a scan to find open and closed ports, as OS detection requires at least **one open** and **one closed** port to function properly.

**Sending Probes**

- Nmap sends various TCP, UDP, and ICMP probes to analyze packet structure, TTL values, window sizes, and response behaviors.

**Matching Fingerprints**

- Responses are compared against Nmap's extensive OS fingerprint database.
- If an exact match is found, the OS is identified with high confidence.
- If only partial matches exist, Nmap provides a best guess with a confidence score.

---

**Example Output :**

```bash
nmap -O 192.168.1.46
```

`Starting Nmap 7.94 (https://nmap.org) at 2025-02-14 14:32 IST`

`Nmap scan report for 192.168.1.46`

`Host is up (0.0050s latency).`

`Not shown: 998 filtered ports`

`PORT       STATE    SERVICE`

`22/tcp     open     ssh`

`OS details: Linux 5.4 - 5.11 (Ubuntu), Windows Server 2019, or FreeBSD 12`

`Network Distance: 1 hop`

---

**Port Status and OS Detection:**

**OS Detected Successfully:**

- If sufficient open and closed ports are found, Nmap will display the exact OS or a short list of possibilities.

**Example:**

— OS details: Windows 10 (build 19044), Linux 5.10

**OS Guessing:**

- If no exact match is found, Nmap provides a guess with a confidence score.

**Example:**

— OS details: Linux kernel 4.x (87% confidence)

**OS Detection Failed:**

- If insufficient data is available (e.g., all ports filtered), OS detection fails.

**Example:**

— Too many fingerprint conflicts, OS detection may be unreliable.

**Advantages of OS Detection (`-O`)**

- Identifies 05 Fingerprints — Helps in penetration testing by mapping network assets.
- Useful for Asset Management — Helps organizations track device types and OS versions.
- Assists in Vulnerability Analysis — Knowing the 05 helps in choosing relevant exploits.

**Disadvantages of OS Detection (`-O`)**

- Requires at Least One Open nd One Closed Port — May fail if all ports are filtered.
- Can Trigger Security Alerts — Some IDS/IPS detect and block OS fingerprinting.
- Not Always 100% Accurate — Fingerprinting can fail if the target custom network stacks.

**Use Cases :**

- Performing reconnaissance to identify OS versions before launching further attacks.
- Assessing network security by identifying outdated or vulnerable operating systems.
- Testing firewall and intrusion detection evasion techniques.

```bash
nmap -O 192.168.1.50
```

```bash
nmap -O --osscan-guess example.com
```

```bash
nmap -O --osscan-limit 10.10.10.10
```

```bash
nmap -O -v 192.168.1.1
```

```bash
nmap -v -Pn -sT -sV -O -p 80 192.168.1.208
```

---

### `--osscan-guess`

The `--osscan-guess` option in **Nmap** is used to **increase the accuracy of OS detection** when the results are uncertain. If Nmap cannot confidently determine the operating system, this option forces it to make an educated guess based on the closest matching OS fingerprint.

```bash
nmap -v -Pn -sT -sV -O --osscan-guess -p 80 192.168.1.208
```

---

### `--osscan-limit`

The `--osscan-limit` option in **Nmap** is used to **restrict OS detection** to only those hosts that respond with enough data for accurate fingerprinting.

```bash
nmap -v -Pn -sT -sV -O --osscan-limit -p 80 192.168.1.208
```

---

### Aggressive Scan | ( `-A` )

The `-A` option in **Nmap** enables **aggressive scanning**, which gathers **detailed information** about the target. It combines multiple advanced scanning features.

When you use `nmap -A <target>`, it enables:

1. **OS Detection (`-O`)** → Tries to determine the target’s operating system.
2. **Version Detection (`-sV`)** → Identifies running services and their versions. 
3. **Script Scanning (`--script=default`)** → Runs common NSE scripts for extra details.
4. **Traceroute (`--traceroute`)** → Maps the network path to the target.

```bash
nmap -v -Pn -sT -sV -O -A -p 80 192.168.1.208
```

---

**Port No. → Service → Kernel → OS**

---

**Protocol No.     →     Service     →     Kernel     →     OS**
---

1    →   ICMP             Linux Kernel 5.x          →   Linux (Ubuntu, Debian, CentOS)

6    →   TCP              Windows NT Kernel         →   Windows 10, Windows Server 2019

17   →   UDP              FreeBSD Kernel            →   FreeBSD 12.x

47   →   GRE              Linux Kernel 4.x          →   VPN Tunnels, Cisco Routers

50   →   ESP              Linux Kernel 5.x          →   IPSec VPN (Linux)

51   →   AH               OpenBSD Kernel            →   OpenBSD 6.x (Firewall Systems)

58   →   ICMPv6           Linux Kernel 6.x          →   IPv6 Networking (Ubuntu, RHEL)

89   →   OSPF             Cisco IOS Kernel          →   Cisco Routers / Switches

**Explanation:**

Protocol Number — Identifies the network protocol used.

Service — The service or functionality associated with the protocol.

Kernel — The underlaying operating system kernel handling the service.

OS — The detected operating system running that kernel.

---

```bash
nmap -v -Pn -sT -sV -O -p 21,80,445,1978,1979,1980,5985,27865 192.168.1.50
```

-O for OS Detection

---

### When there are multiple OSs came in the output of nmap

When there are multiple OSs came in the output of nmap, then scan the target OS on Individual ports for best accuracy

```bash
nmap -v -Pn -sT -sV -O -p 445 192.168.1.50
```

```bash
nmap -v -Pn -sT -sV -O -p 80 192.168.1.50
```

---
