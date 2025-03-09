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
