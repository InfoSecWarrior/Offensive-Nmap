### Custom TCP Scan Flags | (**`--scanflags`)**

The `--scanflags` option in Nmap allows you to manually specify custom TCP flags to craft a custom scan packet. This gives you full control over the TCP scanning behavior.

Normally, Nmap uses predefined scan types like SYN scan ( -sS ), FIN scan (-sF), or Xmas scan ( -sX ), However, with `--scanflags`, you can manually define which TCP flags should be set in the probe packets.

```bash
nmap -v -p 80 **--scanflags=SYN** 192.168.1.207
```

Scanning **port 80** using a **custom SYN flag**, similar to a **SYN scan (`-sS`)**, but manually set.

- If the port is open, the server responds with SYN/ACK.
- If the port is closed, the server responds with RST/ACK.
- If the port is filtered, there is no response.

RST Scan ( **`--scanflags=RST`** )

```bash
nmap -v -p 80 **--scanflags=RST** 192.168.1.207
```

scanning port 80 by custom RST flag.

Uncommon and usually ineffective.

- Since RST is used to close a connection immediately, most systems will ignore or drop the packet.
- This scan may not reveal useful information.

ACK Scan ( **`--scanflags=ACK`** )

```bash
nmap -v -p 80 **--scanflags=ACK** 192.168.1.207
```

scanning port 80 by custom FIN flag.

Used to map out firewall rules.

- If the port is unfiltered, the server responds with RST ( whether the port is open or closed ).
- If the port is filtered, there is no response.
- Can help detect stateful firewall, which drop packets that don’t belong to an established connection.

### **Common TCP Flags**

| **Flag** | **Meaning** |
| --- | --- |
| **SYN** | Initiates a TCP connection |
| **ACK** | Acknowledges received data |
| **FIN** | Closes a connection |
| **RST** | Resets a connection |
| **PSH** | Forces data delivery |
| **URG** | Marks packet as urgent |
| **ECE** | Congestion notification |
| **CWR** | Congestion window reduced |

---
