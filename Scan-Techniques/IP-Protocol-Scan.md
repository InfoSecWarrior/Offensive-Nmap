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
