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
