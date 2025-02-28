## Firewall, IDS, and IPS Evasion & Spoofing in Nmap

Nmap provides various techniques to bypass firwewalls, intrusion detection systems (IDS), and intrusion prevension systems (IPS) while performing network reconnaissance. These techniques help in stealth scanning, IP spoofing, decoy scanning, and fragmented packet scanning.

### Fragment Packets | (`-f`)

The `-f` switch in Nmap **enables IP packet fragmentation**, breaking scan packets into smaller fragments. This helps evade **Intrusion Detection Systems (IDS)** and **firewalls** that inspect full packets.

Breaks probe packet into small IP fragments, making it harder for firewalls to inspect and block the scan. Sends small fragmented packets to evade detection.

```bash
nmap -v -Pn -f 192.168.1.208
```

### Double Packet Fragmentation | (`-f -f`)

Using **`-f -f`** in Nmap increases the level of **IP fragmentation**, further breaking the scan packets into even **smaller fragments** than using just `-f` once.

```bash
nmap -v -Pn -f -f 192.168.1.208
```

### **Advanced Fragmentation / Increase Fragmentation (Custom Packet Size) | (`--mtu`)**

```
nmap --mtu 16 192.168.1.208
```

```bash
nmap -v -Pn --mtu 8 -p- 192.168.1.208
```

```bash
nmap -v -Pn --mtu 16 -p- 192.168.1.208
```

```bash
nmap -v -Pn --mtu 8 192.168.1.208 -p 80
```

```bash
nmap -v -Pn --mtu 16 192.168.1.208 -p 80
```

```bash
nmap -v -Pn --mtu 16 192.168.1.208
```

---

### **Decoy Scanning | (`-D`)**

The `-D` switch in Nmap **uses decoy IP addresses** to obscure the real source of the scan. This helps **evade Intrusion Detection Systems (IDS)** and **firewall logging**, making it harder for defenders to trace the scan back to you.

Generated fake IP addresses along with the real scan, making it difficult for defenders to pinpoint the actual attacker.

```bash
nmap -D 192.168.1.10,192.168.1.20,ME,192.168.1.30,192.168.1.208
```

- `ME` **represents your real IP address** in the decoy list.

**How `ME` Works :** 

- `ME` ensures **your real IP is included** among the decoys.
- The target **logs all IPs** (real + decoys), making it **difficult to identify the actual attacker**.
- If `ME` is **not included**, your real IP may still be logged, but it won’t be mixed with the decoys.

**Example Scenarios :-** 

**Without `ME`**

```bash
nmap -D 192.168.1.10,192.168.1.20,192.168.1.30,192.168.1.208 192.168.1.1
```

- The scan appears to come from **only the decoy IPs**.
- If the attacker’s real IP sends follow-up requests, they may be detected.

**With `ME` (Your Real IP is Mixed)**

```bash
nmap -D 192.168.1.10,192.168.1.20,ME,192.168.1.30,192.168.1.208 192.168.1.1
```

- Now, your real IP **is part of the decoy list**, making it harder to tell who the real scanner is.

**The command will be executed directly without needing to replace `ME` with your actual IP.**

- The command **runs directly** without modification.
- **`ME` is dynamically replaced** with your current IP.
- No need to manually substitute `ME`—Nmap does it for you!

---

### **Spoof Source IP | (`-S`)**

The `-S` switch in Nmap **spoofs the source IP address**, making scan packets appear as if they are coming from another machine.

Allows scanning while pretending to be a different IP. This can be useful for bypassing firewalls that allow only certain IPs.

**Spoofing the Source IP**

```bash
nmap -S 192.168.1.100 192.168.1.208
```

Makes scan traffic **appear as if it is coming from `192.168.1.100`**, not your real IP.

**Combine with Stealth Scan (`-sS`)**

```bash
nmap -S 192.168.1.100 -sS 192.168.1.208
```

```bash
nmap -Pn -e eth0 -S 192.168.1.100 192.168.1.208
```

```bash
nmap -v -Pn -p- -e eth0 -S 192.168.1.100 192.168.1.208
```

---

### Spoof MAC Address (`--spoof-mac`)

Change the MAC address to evade detection or impersonate a trusted device.

The `--spoof-mac` option in Nmap **modifies the MAC address** of the network interface before scanning. This is useful for:

- **Bypassing MAC-based access restrictions**
- **Hiding the real MAC address**
- **Mimicking trusted devices (Apple, Cisco, etc.)**

```bash
nmap --spoof-mac 00:11:22:33:44:55 192.168.1.208
```

- **Forces the MAC address** to `00:11:22:33:44:55` before scanning.

```bash
nmap --spoof-mac <MAC_Address | Vendor_Name | 0> <Target>
```

| **Option** | **Effect** |
| --- | --- |
| Specific MAC Address | Manually sets a custom MAC address. |
| Vendor Name | Generates a MAC address matching a specific manufacturer (e.g., Apple, Cisco, Intel). |
| 0 (Zero) | Assigns a completely random MAC address. |

**Spoof a MAC Address from a Specific Vendor**

```bash
nmap --spoof-mac Apple 192.168.1.208
```

Generates a random Apple MAC address (useful for bypassing MAC filters).

```bash
nmap -Pn --spoof-mac Cisco 192.168.1.208
```

Generates a Cisco-based MAC address to appear as a networking device.

```bash
nmap -Pn --spoof-mac 0 192.168.1.208
```

Assigns a fully random MAC before scanning.

---

### **Specify Source Port | (`-g <port>`**or `--source-port`**)**

The `-g` (or `--source-port`)  option in Nmap allows you to **set the source port** for outgoing packets. This can be useful for:

Use a random source port to make detection more difficult.

**Syntax :**

```bash
nmap -g <port> <target>
```

```bash
nmap -Pn -g 53 102.168.1.208
```

**Tries to disguise the scan as DNS traffic** (some firewalls allow DNS packets through).

- **Bypassing firewalls** that allow traffic from trusted ports (e.g., `53` for DNS, `80` for HTTP).
- **Evading intrusion detection systems (IDS)** by imitating legitimate traffic.
- **Testing firewall rules** by checking how they handle packets from different ports.

```bash
nmap -Pn -g 80 192.168.1.208
```

**Tries to make the scan appear as regular web traffic**.

---

### Proxies | (`--proxies`)

Routes scans through an HTTP/SOCKS proxy to hide the attacker’s real IP.

```bash
nmap --proxies http://proxy.example.com:8080 192.168.1.208
```

```bash
nmap -Pn --proxies <proxy1>,<proxy2>,<proxy3> <target>
```

---

### Idle Scan | (`-sI`)

Uses a “zombie host” to relay the scan, making it appear as if the zombie host is performing the scan instead of the attacker.

**Syntax :**

```bash
nmap -sI <zombie_IP> <target_IP>
```

```bash
nmap -Pn -sI 192.168.1.50 192.168.1.208
```

- **Completely Stealthy** → The scan looks like it's coming from the **zombie**, not you.
- **No Direct Connection to Target** → Your IP never interacts with the target.
- **Bypasses Firewalls & IDS** → If the target blocks your IP, this method avoids detection.

---

### Randomize Scan Order | (`--randomize-hosts`)

Prevent IDS from detecting sequential scanning patterns.

The `--randomize-hosts` option **randomizes the order of target IPs** during a scan. This helps avoid **detection, rate limiting, or IP-based blocking** by firewalls or intrusion detection systems (IDS).

**Syntax :**

```bash
nmap -Pn --randomize-hosts <target_range>
```

```bash
nmap -Pn --randomize-hosts 192.168.1.208
```

---

### Cloaking Nmap Signature | (`--data-length`)

Adds extra random payload data to scans to bypass simple signature-based IDS detection.

**Syntax :**

```bash
nmap --data-length <number> <target>
```

<number>= Number of random bytes to add to each packet.

```bash
nmap -Pn --data-length 50 192.168.1.208
```

Adds 50 bytes of random data to each packet.

---

### Avoid Ping | (`-Pn`)

Some firewalls block ICMP ping requests, preventing host discovery, The `-Pn` option skips host discovery and assumes the host is online.

Useful when firewalls block pings but still allow TCP/UDP traffic.

```bash
nmap -Pn 192.168.1.208
```

---

### Using a Custom User-Agent | (`--script http-useragent`)

Some firewalls block default Nmap scans. Modifying the User-Agent string can help bypass such filters.

The `--script http-useragent` option in Nmap allows you to **spoof or modify the User-Agent string** when scanning web services. This helps in:

- **Bypassing basic security filters** that block certain scanners.
- **Emulating real browsers** to avoid detection.
- **Testing how different User-Agents interact** with a web server.

```bash
nmap --script http-useragent --script-args http.useragent="<custom_agent>" <target>
```

```bash
nmap --script http-useragent --script-args http.useragent="<Mozilla/5.0>" <target>
```

---

### **Custom Data in Probes | (`--data`)**

The --data option in Nmap allows you to **send custom payload data** in probes when scanning specific ports or services. This is useful for:

- **Bypassing Intrusion Detection Systems (IDS)** by modifying packet content.
- **Triggering specific responses** from services that require special input.
- **Customizing probes for non-standard protocols** or **proprietary services**.

**Syntax :**

```bash
nmap --data "<custom_payload>" -p <port> <target>
```

```bash
nmap --data "GET / HTTP/1.1\r\nHost: [example.com](http://example.com/)\r\n\r\n" -p 80 [example.com](http://example.com/)
```

Sends a **custom HTTP request** to port **80**, simulating a real browser request.

```bash
nmap -v -Pn 192.168.1.208 -p 80 --data 4142434445
```

**sends custom raw data (`4142434445`) (ABCDE) to port 80** of `192.168.1.208`, without performing host discovery.

```bash
nmap --data "\x50\x49\x4e\x47" -p 80 192.168.1.10
```

Sends the **hexadecimal data equivalent of "PING"** to the target.

---

### **Custom String Data in Probes | (`-data-string`)**

The `--data-string` option in Nmap allows you to **send a custom string** as payload data in probes. It is similar to `--data`, but instead of using **hexadecimal**, you can send a **plain text string**.

**Syntax :**

```bash
nmap --data-string "<custom_string>" -p <port> <target>
```

```bash
nmap -v -Pn 192.168.1.40 -p 80 --data-string test
```

```bash
nmap -p 80 --data-string "GET / HTTP/1.1\r\nHost: example.com\r\n\r\n" example.com
```

Sends an **HTTP GET request** to simulate a web browser accessing the server.

---

### Padding Packets with Extra Data | (`--data-length`)

```bash
nmap -v -Pn 192.168.1.40 -p 80 --data-length 128
```

---

### **Setting Time-To-Live (TTL) for Packets | (`--ttl`)**

**Syntax :**

```bash
nmap --ttl <value> -p <port> <target>
```

```bash
nmap -Pn -p 80 192.168.1.10 --ttl 64
```

Scans **port 80** on `192.168.1.10` while setting the TTL value to **64** (default for Linux systems).

```bash
nmap -Pn -p 80 192.168.1.10 --ttl 50
```

---

### Customizing IP Header Options | (`--ip-options`)

The `--ip-options` flag in Nmap allows users to set custom IP header options in packets. This can be useful for bypassing firewalls, evading intrusion detection systems (IDS/IPS), or testing how a system handles specific IP options.

The `--ip-options` switch in Nmap allows you to **manually set custom IP header options** in probe packets. This is useful for:

- **Evading firewalls & IDS** (by adding unexpected options)
- **Testing network responses** (how a system handles specific IP options)
- **Simulating different packet behaviors**

**Syntax:**

```bash
nmap --ip-options "<options>" -p <port> <target>
```

```bash
nmap --ip-options "R" -p 80 192.168.1.10
```

Sends **record route (RR) option**, storing the IPs of up to 9 routers along the packet's path.

```bash
nmap --ip-options "\x83\x03\x10\x10\x00" 192.168.1.20
```

**Loose Source Routing (LSRR)**

```bash
nmap --ip-options "L192.168.1.1" -p 22 192.168.1.20
```

**Strict Source Routing (SSRR)**

```bash
nmap --ip-options "S192.168.1.1,192.168.1.2" -p 443 192.168.1.30
```

---
