### SCTP COOKIE ECHO Scan | ( **`-sZ` )**

```bash
nmap -v -sZ 192.168.1.207
```

The SCTP COOKIE ECHO Scan **( `-sZ` )** is a advanced SCTP scanning method that verifies open ports by sending COOKIE ECHO packets. This method helps confirm whether a port is truly open by ensuring the service responds correctly to the SCTP association process.

**Behavior of ( `-sZ` ) Scan**

**Open Port :**

- The client sends an SCTP INIT packet
- The server responds with an INIT-ACK, signaling an open port.
- The client sends a COOKIE ECHO packet to continue the association.
- If the server responds with a COOKIE ACK, the port is confirmed as open.
    
    Client                INIT            →     Server
    
    Client            ←   INIT-ACK              Server
    
    Client                COOKIE ECHO     →     Server
    
    Client            ←   COOKIE ACK            Server
    

**Closed Port :**

- The client sends an SCTP INIT packet.
- The server responds with an ABORT, indicating that no service is listening.
    
    Client                 INIT   →          Server
    
    Client             ←   ABORT             Server
    

**Filtered Port :**

- The client sends an SCTP INIT packet.
- No response is received, meaning the port is either filtered by a firewall or blocked.
    
    Client          INIT         →        Server
    
    no-response
    

**Advantages of SCTP INIT Scan ( `-sY` )**

- More Accurate — Confirms open ports beyond just an INIT-ACK response.
- Useful for Penetration Testing — Helps in assessing SCTP-based security implementations.

**Disadvantages of SCTP INIT Scan ( `-sY` )**

- Requires Privileged Access — Needs root permissions to craft raw packets.
- May Trigger Security Alerts — Some intrusion detection systems monitor for COOKIE ECHO packets.

**Use Cases :**

- Confirming SCTP service availability beyond INIT responses.
- Testing security mechanisms in SCTP-enabled environments.
- Performing deeper reconnaissance on SCTP-enabled targets.

```bash
nmap -v -sZ 192.168.1.207
```

```bash
nmap -v -sZ -p 2095 192.168.1.207
```

```bash
nmap -v -sZ -p 36412 192.168.1.207
```

```bash
nmap -v -sZ -p 2095,36412 192.168.1.207
```

---
