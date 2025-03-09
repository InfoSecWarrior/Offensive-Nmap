### SCTP INIT Scan | ( **`-sY` )**

```bash
nmap -v -sY 192.168.1.207
```

The SCTP INIT Scan ( **`-sY` )** is a specialized scanning technique used to identify open SCTP (Stream Control Transmission Protocol) ports. It works by sending SCTP INIT packets to determine the port status without completing a full SCTP handshake. This is useful for assessing systems that use SCTP, such as telephony and industrial control networks.

**Behavior of ( `-sY` ) Scan**

**Open Port :**

- The client sends an SCTP INIT packet.
- The server responds with an INIT-ACK, indicating the port is open.
- The client does not proceed further, preventing a full SCTP association.
    
    Client                 INIT         →     Server
    
    Client             ←   INIT-ACK           Server
    

**Closed Port :**

- The client sends an SCTP INIT packet.
- The server responds with an ABORT, indicating no service is listening on that port.
    
    Client              INIT      →     Server
    
    Client          ←   ABORT           Server
    

**Filtered Port :**

- The client sends n SCTP INIT packet.
- No response is received, meaning the port is either filtered by a firewall or a security rule is blocking responses.
    
    Client                INIT                →     Server
    
     no-response
    

**Advantages of SCTP INIT Scan ( `-sY` )**

- Non-intrusive — The scan does not complete the SCTP handshake, making it stealthy.
- Effective for detecting SCTP services — Ideal for applications like VoIP, SS7 signaling, and industrial systems.

**Disadvantages of SCTP INIT Scan ( `-sY` )**

- Requires Privileged Access — Root/Administrator privileges are needed to send raw SCTP packets.
- Limited Use — Only useful in environments where SCTP is in use.

**Use Cases :**

- Detecting SCTP-based services in VoIP, telecom, and security appliances.
- Assessing SCTP firewall rules and security measures.
- Performing reconnaissance on targets using non-TCP/UDP protocols.

```bash
nmap -v -sY 192.168.1.207
```

```bash
nmap -v -sY -p 2095 192.168.1.207
```

```bash
nmap -v -sY -p 36412 192.168.1.207
```

```bash
nmap -v -sY -p 2095,36412 192.168.1.207
```

---
