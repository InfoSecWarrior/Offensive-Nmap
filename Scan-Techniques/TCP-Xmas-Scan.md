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
