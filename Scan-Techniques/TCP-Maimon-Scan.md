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
