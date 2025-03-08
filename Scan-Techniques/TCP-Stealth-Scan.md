### TCP SYN Stealth (Half Open) Scan | ( `-sS` )

The TCP SYN Stealth Scan ( `-sS` ) is a widely used scanning technique in penetration testing and network reconnaissance. It works by sending only the initial SYN packet without completing the full three-way handshake.

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
