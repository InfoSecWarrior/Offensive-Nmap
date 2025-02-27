### **Which Scan Type Does Nmap Use by Default?**

By default, **Nmap chooses the scan type based on user privileges**:

**If you run Nmap as a normal user (without root/sudo):**

- Nmap performs a **TCP Connect Scan (`sT`)**
- This is because normal users **cannot** create raw packets required for a stealth scan.

**If you run Nmap as root (with sudo):**

- Nmap performs a **TCP SYN Scan (`sS`)** (Stealth Scan)
- This is because root privileges allow crafting raw packets, which enables a faster and less detectable scan.

---

### **Understanding the Default Scan Types**

**TCP Connect Scan (`-sT`)** → Used when **not running as root**

- Uses the **full** three-way handshake (SYN → SYN/ACK → ACK).
- Slower and more detectable because it fully establishes a connection.
- Logged in target system logs (e.g., firewall and IDS logs).

**TCP SYN Scan (`-sS`)** → Used when **running as root**

- Sends only the **SYN** packet and waits for a response.
- If SYN/ACK is received, it **does not complete the handshake** (stealthier).
- Faster and harder to detect in logs because connections remain half-open.

---

### TCP SYN Stealth (Half Open) Scan | ( `-sS` )

The TCP SYN Stealth Scan ( -sS ) is a widely used scanning technique in penetration testing and network reconnaissance. It works by sending only the initial SYN packet without completing the full three-way handshake.

```bash
nmap -v -sS -p 23 192.168.1.27
```

**Behavior of `-sS` Scan.**

**Open Port:**

- The client sends a SYN packet.
- The server responds with SYN/ACK ( indicating the port is open )
- The client immediately sends RST to terminate the connection before it fully establishes.

**Closed Port:**

- The client sends SYN packet.
- The server responds with RST/ACK, indicating that no service is listening on that port.

**Filtered Port:**

- The client sends a SYN packet.
- No response is received, meaning that port is either filtered by a firewall or a security rule is blocking responses.

**Advantages of SYN Stealth Scan ( `-sS` )**

- Does Not Create  Full Session — Since the connection is never completed, it avoids logging in most application logs.
- Harder to Detect — Many firewalls and intrusion detection systems do not log half-open scans as they do for full TCP connections.

**Disadvantages of SYN Stealth Scan ( `-sS` )**

- Requires Privileged Access — Root/Administrator privileges are needed to send raw packets.
- Produces Many RST Packets — The client resets the connection every time, which can be detected by advanced security monitoring tools

**Use Cases**

- Effective for quiet scanning when trying to identify open ports without triggering firewall alerts.
- Useful for reconnaissance before launching further penetration testing techniques.
- Preferred when scanning hardened targets with logging and detection mechanisms in place.

---

### TCP Connect Scan | ( `-sT` )

The TCP Connect Scan ( `-sT` ) is the default scan method used by Nmap when SYN scans ( `-sS` ) are unavailable ( such as when running without root/administrator privileges ). This method completed the full three-way handshake to establish a connection before terminating it.

```bash
nmap -v -sT -p 23 192.168.1.27
```

**Behavior of `-sT` Scan.**

**Open Port:**

- The client sends a SYN packet.
- The server responds with SYN/ACK ( indicating the port is open ).
- The client sends an ACK, completing the handshake.
- The client then sends a RST to close the connection.

**Closed Port:**

- The client sends a SYN packet.
- The server responds with RST/ACK, indicating no service is listening.

**Filtered Port:**

- The client sends a SYN packet.
- No response is received, likely due to a firewall blocking the request.

**Advantages of TCP Connect Scan ( `-sT` )**

- No Special Privileges Needed — Unlike SYN scans, `-sT` cam be run by any user without root access.

**disadvantages of TCP Connect Scan ( `-sT` )**

- Easily Logged — Since a full connection is established before termination, it is logged by firewalls, IDS/IPS, and system logs.
- Slower than SYN Scan — Because it completes the full handshake, it generates more overhead compares to `-sS`.

**Use Cases**

- Useful when scanning from non-root accounts where `-sS` isn’t possible
- .Can be used to detect services that respond differently to full connection vs. half-open attempts.
- Non stealthy, so use only when logging and detection are not a concern.

---

### TCP Acknowledgement Scan | ( `-sA` )

```bash
nmap -v -sA -p 445 192.168.1.27
```

**Behavior of `-sA` Scan.**

**Unfiltered Port ( Open or Closed ) :**

- The client sends an ACK packet.
- The server responds with RST, meaning the port is not being actively filtered by a firewall.
- Since ACK packets are expected only after an established connection, the server resets the connection.

**Filtered Port :**

- The client sends an ACK packet.
- There is no response, indicating that a firewall or filtering mechanism is dropping the packet.

**Advantages of TCP ACK Scan ( `-sA` )**

- Firewall Detection — It helps map firewall rules by checking which ports are actively filtered
- Stealthier Than SYN/Connect Scans — Since it doesn’t try to establish a connection, it is less likely to trigger alerts in IDS/IPS systems.
- Bypass Some Stateful Firewalls — Certain firewalls only block SYN packets but allow ACK packets through, revealing hidden firewall configurations.

**Disadvantages of TCP ACK Scan ( `-sA` )**

- Does Not Detect Open Ports — It only tells whether a port is filtered or unfiltered, not whether a service is actually running.
- May be Blocked by Stateful Firewall — Some firewalls drop unexpected ACK packets, making it harder to get meaningful results

**Use Cases**

- Useful in firewall penetration testing to detrmine which ports are filtered and which are unprotected.
- Helps in mapping firewall rules to identify how a network allows or blocks traffic.
- Often combined with other scans ( `-sS` , `-sT` , `-sN` ) to cross-check results.

---

### **TCP Window Scan | ( `-sW` )**

```bash
nmap -v -sW -p 445 192.168.1.27
```

The TCP Window Scan ( `-sW` ) is a variation of the ACK scan that attempts to differentiate between open and closed ports by analyzing window size values in the RST packets from the target. It is useful for detecting firewall rules and determining which ports are open without triggering IDS/IPS alerts.

**Port Detection Logic**

| **Response** | **Port State** |
| --- | --- |
| ACK with non-zero TCP Window size | Open |
| ACK with zero TCP Window size | Closed |
| No response / ICMP unreachable | Filtered |

**Behavior of `-sW` Scan.**

**Unfiltered Port ( Open or Closed ) :**

- The client sends ACK packet.
- The server responds with an RST packet.
- If the TCP window size in the RST packet is nonzero, the port is considered open.

**Closed Port:** 

- The client sends an ACK packet.
- The server responds with an RST packet.
- If the TCP window size is zero, the port is considered closed.

**Filtered Port :**

- The client sends an ACK packet.
- There is no response, indicating that a firewall is blocking the packets.

**Advantages of TCP Window Scan ( `-sW` )**

- Stealthier than SYN/Connect Scans — Since it doesn’t initiate a connection, it is less likely to be logged or trigger IDS/IPS alerts.
- Firewall Detection — Can be used to detect which ports are actively filtered.
- More Reliable than ACK Scan — Uses TCP window size analysis for better accuracy in distinguishing open vs. closed ports.

**Disadvantages of TCP Window Scan ( `-sW` )**

- Not Always Effective — Some systems always send RST packets with a fixed window size, making it unreliable in those cases.
- Requires Special OS Conditions — Works best on systems where TCP window size behavior is predictable, which isn't always the case.
- May be Blocked by Firewalls — Like ACK scans, some firewalls drop unexpected ACK packets, leading to false positives.

---

### **TCP Maimon Scan | ( `-sM` )**

```bash
nmap -v -sM -p 445 192.168.1.27
```

The TCP Maimon Scan ( `-sM` ) is a lesser-known stealth scan technique based on a quirk in certain TCP/IP stack implementations. It was discovered by Uriel Maimon and works similarly to FIN, NULL, and Xmas scans, but with a slight variation.

**Port Detection Logic**

| **Response** | **Port State** |
| --- | --- |
| RST (Reset) packet received | Closed |
| No response | Open/Filtered |

**Behavior of `-sM`Scan.**

**Open Port :**

- The client sends a FIN/ACK packet.
- The server ignores the packet ( no response ).
- Indicates the port is open or filtered.

**Closed Port :**

- The client sends a FIN/ACK packet.
- The server responds with an RST packet.
- Indicates the port is closed.

**Filtered Port :**

- The client sends a FIN/ACK packet.
- There is no response, indicating a firewall is blocking the packet.

**Advantages of TCP Maimon Scan ( `-sM` )**

- Stealthy Scan — Since it doesn’t use SYN packets, it’s less likely to be logged or trigger IDS/IPS
- Firewall Evasion — Can bypass some firewalls and packet filters that block standard SYN scans.
- Works Against Certain TCP/IP Stacks — Exploits a flaw in some systems handle FIN/ACK packet, which can reveal open ports.

**Disadvantages of TCP Maimon Scan ( `-sM` )**

- Not Universal — Only works on systems with specific TCP stack behavior (e.g., BSD-based systems) .
- May be Blocked by Stateful Firewalls — Modern firewalls often drop unexpected FIN/ACK packets.
- Can be Inconsistent — Some operating systems treat FIN/ACK packets the same as FIN packets, affecting reliability.

---

### UDP Scan | ( `-sU` )

The UDP Scan ( `-sU` ) is used to detect open UDP ports on a target system. Unlike TCP, UDP is a connectionless, meaning there is no SYN, SYN/ACK, ACK handshake like in TCP scans. Instead, it relies on responses on lack of responses to determine the port status.

```bash
nmap -v -sU -p 53 192.168.1.207
```

```bash
nmap -v -sU -p 69 192.168.1.207
```

**Behavior of `-sU` Scan**

**Open Port.**

- The client sends a UDP Request to a port (e.g., port 53 for DNS).
- The server replies with a valid UDP response.
- Port is OPEN.

**Closed Port.**

- The client sends a UDP request to a port.
- The server responds with an ICMP Port Unreachable ( Type 3, Code 3 ) message.
- Port is CLOSED.

**Open | Filtered Port.**

- The client sends a UDP request to a port.
- No response is received ( firewall may be dropping packets ).
- Port is either OPEN or FILTERED.

**Advantages of UDP Scan ( `-sU` )**

- Lower Overhead — Since UDP does not have a connection handshake like TCP, it is more lightweight.
- No Connection Limit — Unlike TCP, UDP scans are not affected by TCP connections limits.
- Works Well on Microsoft Devices — Windows systems often have important services running on UDP, such as DNS (53), NetBIOS (137), and SNMP (161).

**Disadvantages of UDP Scan ( `-sU` )**

- Privileged Access Required — Root/admin privileges are needed to send raw packets.
- Slow Scans — Many UDP services do not respond, making UDP scanning much slower than TCP.
- ICMP Rate Limiting — Firewalls or rate-limiting settings on the target may drop ICMP Port Unreachable replies, causing many false open/filtered results.

**Use Cases :**

- Finding Open UDP services — Identifies UDP-based services like DNS, DHCP, SNMP, NTP, and TFTP.
- Firewall Evasion — Since many firewalls primarily block TCP, UDP scanning can reveal hidden services.
- Pentesting Windows Networks — UDP scans help discover Windows-specific services, such as NetBIOS (137) and MSRPC (135).

---

### TCP Null Scan | ( `-sN` )

```bash
nmap -v -sN -p 23 192.168.1.207
```

The TCP Null Scan ( `-sN` ) is a stealthy scan technique used to identify open, closed, and filtered ports. It works by sending a TCP packet with no flags set (Null packet) and analyzing the response from the target.

**Behavior of `-sN` Scan:**

**Open Port.**

- The client sends a TCP packet with no flags.
- The server does not respond ( RFC-compliant TCP behavior ).
- Port is OPEN ( or FILTERED ).

**Closed Port.**

- The client sends a TCP packet with no flags.
- The server replies with a RST/ACK packet.
- Port is CLOSED.

**Filtered Port.**

- The client sends a TCP packet with no flags.
- No response is received ( a firewall may be dropping packets ).
- Port is FILTERED.

**Advantages of NULL Scan ( `-sN` )**

- Stealthy — Unlike a full TCP connect ( `-sT` ) or SYN scan ( `-sS` ), it does not trigger logs on some firewalls and IDS.
- Useful for Firewall Bypass — If a firewall only blocks SYN packets, this scan might bypass it.
- Works on Non-Windows Targets — Most UNIX-based systems follow RFC 793, allowing this scan to work effectively.

**Disadvantages of NULL Scan ( `-sN` )**

- Does Not Work on Windows — Windows automatically marks all closed ports as filtered, making this scan ineffective.
- Can be Dropped by Firewalls — Many modern firewalls detect Null scans and block them.
- Unreliable for Open Ports — Since no response is received for open ports, distinguishing between open and filtered ports is difficult.

**Use Cases :**

- Evading IDS/IPS Detection — Since it doesn’t use SYN packets, it might bypass logging on some firewalls.
- Scanning Unix/Linux Systems — This method works well on Linux, BSD, and other Unix-based systems.
- Testing for Firewall Rules — Helps check is firewall is filtering SYN packets but allowing other traffic.

---

### **TCP FIN Scan | ( `-sF` )**

```bash
nmap -v -sF -p 23 192.168.1.207
```

The TCP FNI Scan **( `-sF` )** is a stealth scanning techniques used to identify open, closed, and filtered ports, It works by sending a TCP packet with only the FIN flag set and analyzing the response from the target.

**Port Detection Logic**

| **Response from Target** | **Port State** |
| --- | --- |
| RST (Reset) packet received | Closed |
| No response | Open/Filtered |

**Behavior of `-sF` Scan:**

**Open Port.**

- The client sends a TCP FIN packet.
- The server does not respond ( RFC-complaint TCP behavior ).
- Port is OPEN ( or FILTERED ).

**Closed Port.**

- The client sends a TCP FIN packet.
- The server replies with a RST/ACK packet.
- Port is CLOSED.

**Filtered Port.**

- The client sends a TCP FIN packet.
- No response is received  ( a firewall may be dropping packets ).
- Port is FILTERED.

**Advantages of FIN Scan ( `-sF` )**

- Stealthy — Unlike SYN or Connect scans, it does not establish a session, making it harder to detect.
- Useful for Firewall Evasion — If a firewall blocks SYN packets but allows other traffic, this scan might bypass it.
- Effective on UNIX/Linux Systems — Works well on Linux, BSD, and other Unix-based systems that follow RFC 793.

**Disadvantages of FIN Scan ( `-sF` )**

- Does Not Work on Windows — Windows automatically marks all closed ports as filtered, making this scan ineffective.
- Can be Dropped by Firewalls — Many modern firewalls detect FIN scans and block them.
- Unreliable for Open Ports — Since no response is received for open ports, distinguishing between open and filtered ports is difficult.

**Use Cases :**

- Evading IDS/IPS Detection — Since it doesn’t use SYN packets, it might bypass logging on some firewalls.
- Scanning Unix/Linux Systems — This method works well on Linux, BSD, and other Unix-based systems.
- Testing for Firewall Rules — Helps check is firewall is filtering SYN packets but allowing other traffic.

---

### TCP Xmas Scan | ( `-sX` )

```bash
nmap -v -sX -p 80 192.168.1.207
```

The Xmas Scan ( `-sX` ) is a stealthy and advanced scanning technique that sends TCP packets with FIN, PUSH, and URG flags set, resembling a ‘lit-up’ Christmas tree-hence the name “Xmas Scan.”

**Port Detection Logic**

| **Response from Target** | **Port State** |
| --- | --- |
| RST (Reset) packet received | Closed |
| No response | Open/Filtered |

**Behavior of `-sX` Scan:**

**Open Port.**

- The client sends a TCP packet with FIN, PUSH and URG flags set.
- The server does not respond ( RFC-complaint TCP behavior ).
- Port is OPEN ( or FILTERED ).

**Closed Port.**

- The client sends a TCP Xmas packet.
- The server replies with a RST/ACK packet.
- Port is CLOSED.

**Filtered Port.**

- The client sends a TCP Xmas packet.
- No response is received  ( a firewall may be dropping packets ).
- Port is FILTERED.

**Advantages of Xmas Scan ( `-sX` )**

- Stealthy — Unlike SYN or Connect scans, it does not establish a session, making it harder to detect in logs.
- Firewall Evasion — May bypass stateless firewalls that only filter SYN packets.
- Effective on UNIX/Linux Systems — Works well on Linux, BSD, and other Unix-based systems.

**Disadvantages of Xmas Scan ( `-sX` )**

- Ineffective on Windows — Windows automatically marks all closed ports as filtered, making this scan unreliable.
- Easily Detected by Modern Firewalls — Many modern firewalls and Intrusion detection systems recognize Xmas scans.
- Unreliable for Open Ports — Since no response is received for open ports, distinguishing between open and filtered ports is difficult.

**Use Cases :**

- Bypass IDS/IPS Detection — Since it doesn’t use SYN packets, it might bypass logging on some firewalls.
- Scanning Unix/Linux Systems — This method works well on Linux, BSD, and other Unix-based systems.
- Testing for Firewall Rules — Helps check is firewall is filtering SYN packets but allowing other traffic.

---

### Comparison: FIN Scan vs. Null Scan vs. Xmas Scan

| **Scan Type** | **Flags Sent** | **Open Port Response** | **Close Port Response** | **Works on Windows?** | **Stealth Level** |
| --- | --- | --- | --- | --- | --- |
| FIN Scan ( -sF ) | FIN | No response | RST/ACK | No | High |
| Null Scan ( -sN ) | No Flags | No response | RST/ACK | No | High |
| Xmas Scan ( -sX ) | FIN, PSH, URG | No response | RST/ACK | No | High |

---

### Custom TCP Scan Flags | (**`--scanflags`)**

The `--scanflags` option in Nmap allows you to manually specify custom TCP flags to craft a custom scan packet. This gives you full control over the TCP scanning behavior.

Normally, Nmap uses predefined scan types like SYN scan ( -sS ), FIN scan (-sF), or Xmas scan ( -sX ), However, with `--scanflags`, you can manually define which TCP flags should be set in the probe packets.

```bash
nmap -v -p 80 --scanflags=SYN 192.168.1.207
```

Scanning **port 80** using a **custom SYN flag**, similar to a **SYN scan (`-sS`)**, but manually set.

- If the port is open, the server responds with SYN/ACK.
- If the port is closed, the server responds with RST/ACK.
- If the port is filtered, there is no response.

RST Scan ( **`--scanflags=RST`** )

```bash
nmap -v -p 80 --scanflags=RST 192.168.1.207
```

scanning port 80 by custom RST flag.

Uncommon and usually ineffective.

- Since RST is used to close a connection immediately, most systems will ignore or drop the packet.
- This scan may not reveal useful information.

ACK Scan ( **`--scanflags=ACK`** )

```bash
nmap -v -p 80 --scanflags=ACK 192.168.1.207
```

scanning port 80 by custom FIN flag.

Used to map out firewall rules.

- If the port is unfiltered, the server responds with RST ( whether the port is open or closed ).
- If the port is filtered, there is no response.
- Can help detect stateful firewall, which drop packets that don’t belong to an established connection.

### **Common TCP Flags**

| **Flag** | **Meaning** |
| --- | --- |
| **SYN** | Initiates a TCP connection |
| **ACK** | Acknowledges received data |
| **FIN** | Closes a connection |
| **RST** | Resets a connection |
| **PSH** | Forces data delivery |
| **URG** | Marks packet as urgent |
| **ECE** | Congestion notification |
| **CWR** | Congestion window reduced |

---

### **Comparison of Stealth Scans in Nmap**

| **Scan Type** | **Flags Sent** | **Open Port Response** | **Closed Port Response** | **Works on Windows?** | **Stealth Level** | **Best Used For** |
| --- | --- | --- | --- | --- | --- | --- |
| **SYN Scan (`-sS`)** | SYN | SYN/ACK (half-open) | RST | Yes | High | Fast & stealthy scans |
| **FIN Scan (`-sF`)** | FIN | No response | RST/ACK | No | High | Bypassing SYN filters |
| **Null Scan (`-sN`)** | No Flags | No response | RST/ACK | No | High | Evading firewalls & IDS |
| **Xmas Scan (`-sX`)** | FIN, PSH, URG | No response | RST/ACK | No | High | Firewall evasion |
| **ACK Scan (`-sA`)** | ACK | No response (filtered) | RST (unfiltered) | Yes | Medium | Checking firewall rules |
| **Maimon Scan (`-sM`)** | FIN + ACK | No response | RST | No | High | Bypassing firewalls |

---

### **IP Protocol Scan | (`-sO`)**

```bash
nmap -v -sO 192.168.1.207
```

The **`-sO` scan** is used to **identify supported IP protocols** on a target system, rather than scanning for open ports.

The `-sO` ( IP Protocol Scan ) allows you to scan for supported IP protocols on target system. Instead of scanning for open ports, this scan checks which network protocols ( TCP, UDP, ICMP, IGMP, etc. ) are supported and responding.

The IP Protocol Scan (`-sO`) is a specialized scanning technique that detects which IP protocols are supported by a target system. Instead of scanning ports, it scans for supported protocol numbers (such as ICMP, TCP, UDP, GRE, ESP, etc.), helping identify firewall rules, security configurations, and available network services.

**How It Works**

- Instead of scanning specific TCP/UDP ports, the IP Protocols Scans raw IP packets with different protocol numbers.
- If the system responds, it means the protocol is supported.
- If the system sends ICMP “Protocol Unreachable”, it means the protocol is not supported.

**Behavior of `-sO` Scan**

**Open Protocol:**

- The client sends an IP packet with a specific protocol number (e.g., ICMP, TCP, UDP).
- The server responds appropriately, confirming that the protocol is supported.
    
    Client                 IP Protocol                          →     Server
    
    Client           ←   Response (valid protocol)           Server
    

**Closed Protocol:**

- The client sends an IP packet with a specific protocol number.
- The server responds with an **ICMP Protocol Unreachable (Type 3, Code 2)** message, indicating that the protocol is not supported.
    
    Client                 IP Protocol                              →     Server
    
    Client           ←   ICMP Protocol Unreachable           Server
    

**Filtered Protocol:**

- The client sends an IP packet with a specific protocol number.
- No response is received, meaning the protocol is either **filtered by a firewall* or blocked by security rules.
    
    Client                    IP Protocol      →     Server
    
     no-response
    

**Advantages of IP Protocol Scan (`-sO`)**

- Enumerates Supported Protocols Helps identify network services beyond just TCP/UDP ports.
- Useful for Firewall & Security Testing Can detect which protocols are allowed or blocked.
- Works on Firewalled Hosts Even if all TCP/UDP ports are closed, some protocols might still be open.

**Disadvantages of IP Protocol Scan (`-sO`)**

- Requires Privileged Access — Root/Administrator privileges are needed to send raw IP packets.
- Limited Information — Only tells if a protocol is open/closed, not detailed service information.
- May Trigger Security Alerts — Some firewalls detect and block IP protocol scanning attempts.

**Use Cases :**

- Testing firewall rules for allowed/disallowed protocols.
- Identifying non-standard communication channels in a network.
- Detecting VPN, tunneling, and encrypted traffic(e.g., GRE, ESP, Ah).
- Performing reconnaissance on restricted or hardened targets.

```bash
nmap -v -sO 192.168.1.207
```

```bash
nmap -v -sO --packet-trace 192.168.1.207
```

```bash
nmap -v -sO --disable-arp-ping 192.168.1.207
```

```bash
nmap -v -sO -Pn 192.168.1.207
```

Basic IP protocol Scan

```bash
nmap -v -sO 192.168.1.207
```

This is can checks which IP protocol the target supports and response to.

Scan for Specific Protocol (e.g., ICMP)

```bash
nmap -v -sO --packet-trace 192.168.1.207
```

The `--packet-trace` option shows detailed packet information during the scan.

Interpreting Scan Result

**Supported Protocols**

- If a protocol is open, the system responds to it.

**Example output:**

Proto (1) open

Proto (6) open

Proto (17) open

**This means:**

Proto 1 → ICMP is supported.

Proto 6 → TCP is supported.

Proto 17 → UDP is supported.

**Common IP Protocol Numbers:**

| **Protocol Name** | **Protocol Number** |
| --- | --- |
| ICMP (Ping) | 1 |
| IGMP | 2 |
| TCP | 6 |
| UDP | 17 |
| GRE | 47 |
| ESP (IPSec) | 50 |

---

### SCTP INIT Scan | ( **`-sY` )**

```bash
nmap -v -sY 192.168.1.207
```

The SCTP INIT Scan ( **`-sY` )** is a specialized scanning technique used to identify open SCTP (Stream Control Transmission Protocol) ports. It works by sending SCTP INIT packets to determine the port status without completing a full SCTP handshake. This is useful for assessing systems that use SCTP, such as telephony and industrial control networks.

**Behavior of ( `-sY` ) Scan**

**Open Port :**

- The client sends an SCTP INIT packet.
- The server responds with an INIT-ACK, indicating the port is open.
- The client does not proceed further, preventing a full SCTP association.
    
    Client             INIT       →     Server
    
    Client        ←   INIT-ACK          Server
    

**Closed Port :**

- The client sends an SCTP INIT packet.
- The server responds with an ABORT, indicating no service is listening on that port.
    
    Client              INIT       →     Server
    
    Client          ←   ABORT            Server
    

**Filtered Port :**

- The client sends n SCTP INIT packet.
- No response is received, meaning the port is either filtered by a firewall or a security rule is blocking responses.
    
    Client                INIT                →     Server
    
    no-response
    

**Advantages of SCTP INIT Scan ( `-sY` )**

- Non-intrusive — The scan does not complete the SCTP handshake, making it stealthy.
- Effective for detecting SCTP services — Ideal for applications like VoIP, SS7 signaling, and industrial systems.

**Disadvantages of SCTP INIT Scan ( `-sY` )**

- Requires Privileged Access — Root/Administrator privileges are needed to send raw SCTP packets.
- Limited Use — Only useful in environments where SCTP is in use.

**Use Cases :**

- Detecting SCTP-based services in VoIP, telecom, and security appliances.
- Assessing SCTP firewall rules and security measures.
- Performing reconnaissance on targets using non-TCP/UDP protocols.

```bash
nmap -v -sY 192.168.1.207
```

```bash
nmap -v -sY -p 2095 192.168.1.207
```

```bash
nmap -v -sY -p 36412 192.168.1.207
```

```bash
nmap -v -sY -p 2095,36412 192.168.1.207
```

---

### SCTP COOKIE ECHO Scan | ( **`-sZ` )**

```bash
nmap -v -sZ 192.168.1.207
```

The SCTP COOKIE ECHO Scan **( `-sZ` )** is a advanced SCTP scanning method that verifies open ports by sending COOKIE ECHO packets. This method helps confirm whether a port is truly open by ensuring the service responds correctly to the SCTP association process.

**Behavior of ( `-sZ` ) Scan**

**Open Port :**

- The client sends an SCTP INIT packet
- The server responds with an INIT-ACK, signaling an open port.
- The client sends a COOKIE ECHO packet to continue the association.
- If the server responds with a COOKIE ACK, the port is confirmed as open.
    
    Client                INIT         →     Server
    
    Client            ←   INIT-ACK           Server
    
    Client                COOKIE ECHO   →    Server
    
    Client            ←   COOKIE ACK         Server
    

**Closed Port :**

- The client sends an SCTP INIT packet.
- The server responds with an ABORT, indicating that no service is listening.
    
    Client                      INIT   →                Server
    
    Client                  ←   ABORT                   Server
    

**Filtered Port :**

- The client sends an SCTP INIT packet.
- No response is received, meaning the port is either filtered by a firewall or blocked.
    
    Client                INIT                →            Server
    
    no-response
    

**Advantages of SCTP INIT Scan ( `-sY` )**

- More Accurate — Confirms open ports beyond just an INIT-ACK response.
- Useful for Penetration Testing — Helps in assessing SCTP-based security implementations.

**Disadvantages of SCTP INIT Scan ( `-sY` )**

- Requires Privileged Access — Needs root permissions to craft raw packets.
- May Trigger Security Alerts — Some intrusion detection systems monitor for COOKIE ECHO packets.

**Use Cases :**

- Confirming SCTP service availability beyond INIT responses.
- Testing security mechanisms in SCTP-enabled environments.
- Performing deeper reconnaissance on SCTP-enabled targets.

```bash
nmap -v -sZ 192.168.1.207
```

```bash
nmap -v -sZ -p 2095 192.168.1.207
```

```bash
nmap -v -sZ -p 36412 192.168.1.207
```

```bash
nmap -v -sZ -p 2095,36412 192.168.1.207
```

---
Use don’t ping switch in FTP bounce (-b ) and in Idle Scan / Zombie Scan | ( -sI ) otherwise your real IP will be logged in the target device logs  

### FTP Bounce Scan | (`-b` )

The FTP Bounce Scan (`-b`) is an advanced network scanning technique that exploits the FTP server's proxy feature (PORT command) to scan other hosts indirectly. This allows an attacker to bypass network restrictions and scan targets from the FTP server's perspective, potentially hiding the attacker's real IP address.
The FTP Bounce attack is an old vulnerability, and most modern FTP servers disable PORT-based relaying by default. However, it is still a useful technique for testing legacy FTP servers and misconfigured environments.

**Behavior of (`-b` )Scan**

**Open Port:**

- The attacker connects to an FTP server with an anonymous or valid login.
- The attacker issues a PORT command to instruct the FTP server to connect to the target on a specific port.
- If the connection is successful, the port is open.
    
    Attacker              CONNECT                     →     FTP Server
    
    Attacker              PORT Target : Port          →     FTP Server
    
    FTP Server            CONNECT Target : Port       →     Target
    
    FTP Server       ←    Success (Port Open)               Target
    

**Closed Port:** 

- The attacker connects to an FTP server and issues the PORT command for the target.
- The FTP server tries to connect to the target port but receives a rejection (RST).
    
    Attacker            CONNECT                                 →     FTP Server
    
    Attacker            PORT Target : Port                      →     FTP Server
    
    FTP Server          CONNECT Target : Port                   →     Target
      
    FTP Server      ←   Connection Refused (Port Closed)              Target
    

**Filtered Port:**

- The attacker connects to an FTP server and issues the PORT command for the target.
- The FTP server tries to connect but receives no response (possibly filtered by a firewall).
    
    Attacker       CONNECT                       →     FTP Server
    
    Attacker       PORT Target : Port             →    FTP Server
    
    FTP Server     CONNECT Target: Port           →    Target
    
     no-response
    

**Advantages of FTP Bounce Scan (`-b`)**

- Bypasses Firewalls Can scan internal networks if an exposed FTP server is available.
- Hides Attacker's IP The scan appears to originate from the FTP server, not the attacker.
- Exploits Misconfigured FTP Servers Useful for assessing security flaws in legacy systems.

**Disadvantages of FTP Bounce Scan (`-b`)**

- Requires an Open FTP Server Only works if an FTP server allows proxying through the PORT command.
- Slow Scan Speed Relaying traffic through an FTP server can be inefficient.
- Mostly Patched Modern FTP servers disable this feature by default.

**Use Cases**

- Testing FTP server security to ensure FTP bounce vulnerabilities are patched.
- Performing indirect scans on internal networks using misconfigured FTP servers.
- Bypassing IP-based access control restrictions in firewalled environments.

```bash
nmap -Pn -b anonymous@ftp.example.com -p 21,22,80 192.168.1.207
```

```bash
nmap -Pn -b user:pass@ftp.example.com -p 1-1000 192.168.1.207
```

```bash
nmap -Pn -b anonymous@ftp.example.com -p 21,22,80 target.com
```

---

### Idle Scan / Zombie Scan | ( `-sI` )

The Idle Scan (`-sI`) is a highly stealthy and advanced port scanning technique that allows an attacker to scan a target without directly communicating with it. Instead, it leverages a "zombie" host to relay packets, making the scan appear as if it originated from the zombie, effectively hiding the attacker's identity.

**Behavior of (`-sI`) Scan**

Find a Suitable Zombie Host:

- The attacker selects a host (the "zombie") that has a predictable IP ID sequence and minimal network activity.

**Determine the Zombie's IP ID Value:**

- The attacker sends a SYN/ACK or other probe to the zombie.
- The zombie responds with an **IP ID number**, which is used to track changes in future interactions.

**Send a SYN Packet to the Target via the Zombie:**

- The attacker instructs the zombie to send a SYN packet to the target on a specific port.
- The attacker's IP never appears in the target's logs.

**Analyze the Zombie's IP ID Changes:**

- If the target port is open, the target replies with SYN/ACK, causing the zombie to send an RST.
- If the target port is closed, the target responds with RST, and the zombie's IP ID remains unchanged.
- The attacker sends another probe to the zombie to check how much its IP ID has increased.

**Port Status Identification:**

**Open Port:**

- The zombie's **IP ID increases by 2** (one for the SYN/ACK from the target, one for the zombie's RST).
- This confirms the target port is open.
    
    Attacker          Probe Zombie ID=100
    
    Attacker           Ask Zombie to SYN > Target
    
    Target               < SYN/ACK Target (Open)
    
    Zombie              < RST Zombie
    
    Attacker              Probe Zombie ID=102 (Increase by 2)
    

**Closed Port:**

- The target sends an **RST** immediately.
- The zombie's **IP ID increases by 1** or remains the same.
    
    Attacker          Probe Zombie ID=100
    
    Attacker           Ask Zombie to SYN > Target
    
    Target               < RST Target (Closed)
    
    Attacker            Probe Zombie ID=101 (Increased by 1)
    

**Filtered Port :**

- If no response is received from the target, the firewall is likely blocking traffic.
- The zombie's IP ID remains unchanged (or increases due to other reasons).
    
    Attacker             Probe Zombie ID=100
    
    Attacker             Ask Zombie to SYN > Target
    
    No Response
    
    Attacker             Probe Zombie ID=100 (No Change)
    

**Advantages of Idle Scan (`-sI`)**

- Complete Anonymity — The attacker's IP address never interacts with the target.
- Bypasses Firewalls — Scans can be relayed through internal or trusted hosts.
- No Direct Connection to Target — This makes it extremely difficult to detect.

**Disadvantages of Idle Scan (`-sI`)**

- Requires a Suitable Zombie — The technique only works if a predictable IP ID host is available.
- Slow Scan Speed — Relies on indirect communication, making scans inefficient.
- Not Always Reliable — If the zombie host is active, its IP ID sequence may be unpredictable.

**Use Cases :**

- Performing stealth reconnaissance against highly monitored networks.
- Bypassing intrusion detection systems (IDS) that log direct scans.
- Using internal poorly configured hosts to scan restricted networks.

```bash
nmap -Pn -sI 192.168.1.100 192.168.1.207
```

```bash
nmap -Pn -sI zombie.example.com -p 80 target.com
```

```bash
nmap -sI zombie.example.com -Pn -p 22,443,8080 10.10.10.10
```

---
