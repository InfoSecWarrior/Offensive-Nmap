**Use don’t ping switch in FTP bounce (`-b` ) and in Idle / Zombie Scan | ( `-sI` ) otherwise your real IP will be logged in the target device logs**

### FTP Bounce Scan | (`-b` )

The FTP Bounce Scan (`-b`) is an advanced network scanning technique that exploits the FTP server's proxy feature (PORT command) to scan other hosts indirectly. This allows an attacker to bypass network restrictions and scan targets from the FTP server's perspective, potentially hiding the attacker's real IP address.
The FTP Bounce attack is an old vulnerability, and most modern FTP servers disable PORT-based relaying by default. However, it is still a useful technique for testing legacy FTP servers and misconfigured environments.

**Behavior of (`-b` )Scan**

**Open Port:**

- The attacker connects to an FTP server with an anonymous or valid login.
- The attacker issues a PORT command to instruct the FTP server to connect to the target on a specific port.
- If the connection is successful, the port is open.
    
    Attacker            CONNECT                 →     FTP Server
    
    Attacker            PORT Target : Port      →     FTP Server
    
    FTP Server          CONNECT Target : Port   →     Target
    
    FTP Server      ←   Success (Port Open)           Target
    

**Closed Port:** 

- The attacker connects to an FTP server and issues the PORT command for the target.
- The FTP server tries to connect to the target port but receives a rejection (RST).
    
    Attacker            CONNECT                               →     FTP Server
    
    Attacker            PORT Target : Port                    →     FTP Server
    
    FTP Server         CONNECT Target : Port                  →     Target
    
    FTP Server     ←   Connection Refused (Port Closed)             Target
    

**Filtered Port:**

- The attacker connects to an FTP server and issues the PORT command for the target.
- The FTP server tries to connect but receives no response (possibly filtered by a firewall).
    
    Attacker       CONNECT                →     FTP Server
    
    Attacker       PORT Target : Port     →    FTP Server
    
    FTP Server     CONNECT Target: Port   →    Target
    
     no-response
    

**Advantages of FTP Bounce Scan (`-b`)**

- Bypasses Firewalls Can scan internal networks if an exposed FTP server is available.
- Hides Attacker's IP The scan appears to originate from the FTP server, not the attacker.
- Exploits Misconfigured FTP Servers Useful for assessing security flaws in legacy systems.

**Disadvantages of FTP Bounce Scan (`-b`)**

- Requires an Open FTP Server Only works if an FTP server allows proxying through the PORT command.
- Slow Scan Speed Relaying traffic through an FTP server can be inefficient.
- Mostly Patched Modern FTP servers disable this feature by default.

**Use Cases**

- Testing FTP server security to ensure FTP bounce vulnerabilities are patched.
- Performing indirect scans on internal networks using misconfigured FTP servers.
- Bypassing IP-based access control restrictions in firewalled environments.

```bash
nmap -Pn -b anonymous@ftp.example.com -p 21,22,80 192.168.1.207
```

```bash
nmap -Pn -b user:pass@ftp.example.com -p 1-1000 192.168.1.207
```

```bash
nmap -Pn -b anonymous@ftp.example.com -p 21,22,80 target.com
```

---
