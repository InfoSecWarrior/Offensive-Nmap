
## IP,TCP,UDP,ICMP Header Fields & Their Role in Nmap Scans

1. **Version**  
   - Always set to 4 for IPv4 packets.
   - Nmap primarily works with IPv4 but supports IPv6 with the `-6` option.
   - Example: `nmap -6 2001:db8:3333:4444:5555:6666:7777:8888`

2. **IP Header Length (IHL)**  
   - Specifies the length of the IP header in 32-bit words (default is 5 words = 20 bytes).
   - If options are used, the length increases.
   - Nmap allows modification of headers in raw packet scans (`-s0` for protocol scan).

3. **Type of Service (ToS) / Differentiated Services Code Point (DSCP)**  
   - Used for Quality of Service (QoS) in routing.
   - Nmap allows setting ToS using `--ip-options`.
   - Example: `nmap --ip-options 'Ix03' 192.168.1.1`

4. **Size of Datagram**  
   - Total length (header + data).
   - Large packets may be fragmented, which Nmap can manipulate (`-f` for fragmentation).
   - Used for firewall evasion.
   - Example: `nmap -f 192.168.1.1`

5. **Identification**  
   - Unique 16-bit ID for fragmented packets.
   - Used in packet reassembly.
   - Can be observed using `tcpdump` when analyzing Nmap traffic.

6. **Flags**  
   - **DF (Don't Fragment):** Prevents packet fragmentation.
   - **MF (More Fragments):** Indicates more fragments are coming.
   - Nmap can manipulate fragmentation to bypass IDS.
   - Example: `nmap -f 192.168.1.1`

7. **Fragmentation Offset**  
   - Indicates where a fragmented packet belongs in the original datagram.
   - Helps in evading firewalls when used with `-f`.

8. **Time to Live (TTL)**  
   - Decrements at each hop; prevents infinite loops.
   - Can be set in Nmap to control traceability.

9. **Protocol**  
   - Defines which Transport Layer protocol is used:
     - `1 = ICMP`
     - `6 = TCP`
     - `17 = UDP`
   - Nmap IP Protocol Scan (`-s0`) identifies supported protocols.
   - Example: `nmap -s0 192.168.1.1`

10. **Header Checksum**  
    - Ensures header integrity (modified if the header changes).
    - Calculated automatically; no manual Nmap control over this.

11. **Source Address**  
    - IP address of the sender.
    - Useful for stealth scans.
    - Nmap can spoof the source IP.
    - Example: `nmap -S 192.168.1.100 192.168.1.1`

12. **Destination Address**  
    - IP of the target system.
    - Can be set manually or by CIDR notation (`/24` for subnet scans).

13. **Options**  
    - Rarely used, but allows features like Record Route (RR).
    - Can be set in Nmap with `--ip-options`.
    - Example: `nmap --ip-options 'Ix07\x27\x08\x08\x08\x08' 192.168.1.1`

---

## TCP Header Fields & Their Role in Nmap Scans

1. **Source Port (16 bits)**  
   - Port used by the sender to initiate communication.
   - Can be specified in Nmap to evade firewalls.
   - Example: `nmap --source-port 53 192.168.1.1`

2. **Destination Port (16 bits)**  
   - Target port on the destination machine.
   - Example: `nmap -p 22,80,443 192.168.1.1`

3. **Sequence Number (32 bits)**  
   - Used for ordering TCP segments.
   - Nmap uses ISNs in OS fingerprinting (`-O`).

4. **Acknowledgment Number (32 bits)**  
   - Indicates received data up to this sequence number.
   - Used in TCP three-way handshake.

5. **Data Offset (Header Length) (4 bits)**  
   - Defines TCP header length (usually 5 words = 20 bytes).
   - If options are present, the length increases.

6. **TCP Flags (9 bits)**  
   - Used in different Nmap stealth scans:

   | Flag | Description | Nmap Usage |
   |------|-------------|------------|
   | URG  | Urgent data | Rarely used |
   | ACK  | Acknowledgment | Used in ACK scans (`-sA`) |
   | PSH  | Push | Rarely used |
   | RST  | Reset connection | Used in RST responses |
   | SYN  | Synchronize | Used in SYN scans (`-sS`) |
   | FIN  | Finish | Used in FIN scans (`-sF`) |

7. **Window Size (16 bits)**  
   - Determines the amount of data the sender can receive.
   - Used in OS fingerprinting.

8. **Checksum (16 bits)**  
   - Ensures data integrity.
   - Automatically calculated by Nmap.

9. **Options (Variable Length)**  
   - Used for Maximum Segment Size (MSS), timestamps, etc.
   - Example: `nmap --tcp-options 02:04:4000 192.168.1.1`

---

## UDP Header Fields & Their Role in Nmap Scans

| UDP Header Field | Nmap Usage |
|------------------|------------|
| **Source Port** | Can be set using `--source-port` to evade firewalls |
| **Destination Port** | Specifies target service for scanning (`-p 53,161,500`) |
| **Length** | Used for fragmentation evasion techniques |
| **Checksum** | Validates packet integrity; sometimes intentionally incorrect for evasion |

### Key UDP Scanning Techniques in Nmap (`-sU`)

1. **UDP Scan (`-sU`)**
   - Sends an empty UDP packet to the target port.
   - Relies on ICMP Port Unreachable (Type 3, Code 3) responses.
   - Example: `nmap -sU -p 53,161 192.168.1.1`
   - If **ICMP Type 3, Code 3** is received → Port is **closed**.
   - If **no response** → Port is **open or filtered**.
   - If **ICMP Type 3, Code 1, 2, 9, 10, or 13** → Port is **filtered (firewall blocking)**.

---

## Internet Control Message Protocol (ICMP) Header Fields
 - ICMP (Internet Control Message Protocol) is a network layer protocol used for diagnostics, error reporting, and operational  queries.
	Unlike TCP and UDP, ICMP is not used for data transmission but rather for network troubleshooting and communication between network devices.

| ICMP Type | Message Name | Description |
|-----------|--------------|-------------|
| **0** | Echo Reply | Response to an Echo Request (ping). |
| **3** | Destination Unreachable | Indicates an unreachable destination. |
| **5** | Redirect Message | Informs a host of a better route. |
| **8** | Echo Request | Used to check network connectivity. |
| **11** | Time Exceeded | TTL expired before reaching the destination. |
| **12** | Parameter Problem | Indicates a malformed packet header. |

**Advantage of ICMP**

Error Reporting: ICMP conveys information to the participating network devices regarding the problems taking place, like unreachable destinations, etc.

Diagnostic Tools: They assist in diagnosing network connectivity and performance issues, for example, ping and traceroute.

Network Management: ICMP helps in network traffic management by providing feedback on the network conditions and problems.


