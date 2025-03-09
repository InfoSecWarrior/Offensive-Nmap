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
