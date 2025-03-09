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
