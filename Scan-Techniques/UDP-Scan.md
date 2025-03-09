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
