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
