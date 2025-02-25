### Verbose Mode | ( `-v` , `-vv` , `-vvv` )

**The ‘verbosity’ of the nmap output can be increased by 2 or 3 time by using `-vv` or `-vvv` ; use `-vv` or more for greater effect**

```bash
nmap -vv -sT -p- 192.168.1.207
```

```bash
nmap -vvv -sT -p- 192.168.1.207
```

---

### Debugging level | ( `-d` )

**The ‘debugging’ level of the nmap output can be increased by 2 or 3 time by using `-dd` or `-ddd` ; use `-dd` or more for greater effect**

```bash
nmap -d -sT -p- 192.168.1.207
```

```bash
nmap -dd -sT -p- 192.168.1.207
```

```bash
nmap -v -d -sT -p- 192.168.1.207
```

```bash
nmap -vv -dd -sT -p- 192.168.1.207
```

---

### Reason | ( `--reason` )

The `--reason` option in Nmap provides additional details about why a port is classified as **open**, **closed**, or **filtered**. It explains the reason behind each port's state based on the response received from the target.

→ When you run an Nmap scan with `--reason`, Nmap will display an extra column explaining why each port was assigned its specific state.

```bash
nmap -v -sT -p 80 --reason 192.168.1.207
```

**When the port is allowed in firewall or firewall is off and the port is open then it will show [Three way handshake completed]                     → open**

**When the firewall is off or port is allowed but in this case the web-server is off then it will show [client send SYN, server replies with RST] → closed**

**When the firewall is on or the port is not allowed in the firewall then it will show [client send SYN; server does not replies]                          → filtered**

**Open Port:**

- The client sends a SYN packet to initiate the connection.
- The server responds with a SYN/ACK packet, indicating that it is listening.
- The client then sends a RST (reset) instead of completing the handshake, which is a common technique used in port scanning to avoid establishing a full connection.

**Closed Port:**

- The client sends a SYN packet to the server.
- The server responds with RST/ACK, indicating that no service is listening on that port.

**Filtered Port:**

- The client sends a SYN packet.
- There is no response from the server, which suggests that the packet might have been dropped by a firewall or other security mechanism.

---

### Only displays ‘Open Ports’ | ( `--open` )

```bash
nmap -v -sT --top-ports 50 --reason --open 192.168.1.207
```

---

### Tracing Packets | ( `--packet-trace` )

The `--packet-trace` option in Nmap enables **detailed packet-level logging**, showing each packet sent and received during the scan. This is useful for debugging, analyzing network behavior, and understanding how Nmap interacts with the target system.

```bash
nmap -v -sT --top-ports 50 --reason --open --packet-trace 192.168.1.207
```

---

### Display Interface list | ( `--iflist` )

```bash
nmap --iflist
```

Shows interface list.

---

## Outputs in nmap:

### Save nmap output in plain text form | ( `-oN` )

The `-oN` option in Nmap is used to **save scan results in a normal/plain text format**.

```bash
nmap -v -sT -p- 192.168.1.27 -oN nmap-output.nmap
```

This will save the output of this command in plain text ( as-it-is ) with the extension `.nmap` ; here ‘scan-output’ is the name of the file in which we are creating to save nmap’s output.

---

### Save nmap output in XML form | ( `-oX` )

```bash
nmap -v -sT -p- 192.168.1.27 -oX nmap-output.xml
```

This will save the output of nmap in XML format; extension will be `.xml` 

---

```bash
file nmap-output.nmap
```

```bash
file nmap-output.xml
```

---

### Save nmap output in script kiddies form | ( `-oS` )

No useful at all

```bash
nmap -v -sT -p- 192.168.1.27 -oS nmap-output.script
```

---

### Save nmap output in Grepable format | ( `-oG` )

```bash
nmap -v -sT -p- 192.168.1.27 -oG nmap-output.gnmap
```

Saved nmap output in grepable format

---

### Save nmap output in all formats | ( `-oA` )

```bash
nmap -v -sT -p- 192.168.1.27 -oA nmap-output
```

**This will save the output if nmap in all three major useful formats ( Plain text, grepable & XML )→ `.nmap` , `.gnmap` , `.xml`** 

---

### Resume the nmap scan

`--resume` option should reference the previously saved scan file (`.gnmap` or `.nmap`)

```bash
nmap -v -sT -p- 192.168.1.0/24 -oA mynetwork
```

```bash
nmap --resume mynetwork.nmap
```

This will **continue the scan** from where it was interrupted, using the data stored in `mynetwork.nmap`.

- This continues the scan using the saved progress in `mynetwork.nmap`.
- **No need to specify the target IP again** — Nmap already knows from the file.

---

### Add nmap output to already existing nmap output file | ( `--append-output` )

The `--append-output` option in Nmap allows you to **append** scan results to an existing output file instead of overwriting it. This is useful when running multiple scans and keeping all results in a single file for later analysis.

```bash
nmap -v -sT -p- 192.168.1.207 -oA nmap-output
```

This will scan the target IP and save the output in all majorly used format types.

Now we will scan the new target, and add the output of the new target scan to already existing nmap output file

```bash
nmap -v -sT -sV -sC -A -p- 192.168.1.207 -oA nmap-output --append-output
```

Here we have added some switched to observe in the file that there are changes in the output of the targets in the same file.

---

### Lock the Keyboard Interaction | ( `--noninteractive` )

The `--noninteractive` option in Nmap is used to **disable interactive features**, ensuring the scan runs without requiring user input. This is useful when running Nmap in **scripts, automated tasks, or non-interactive environments** like cron jobs or remote servers.

```bash
nmap -v -sT -p- 192.168.1.207 --noninteractive -oA nmap-output
```

---

### Nmap output in WebXML format | ( `--webxml` )

The `--webxml` option in Nmap modifies the **XML output format** to be more compatible with web-based applications. This is useful when integrating Nmap scan results into web tools, dashboards, or reporting systems.

```bash
nmap -v -sT -sV -sC -A -p- 192.168.1.207 --webxml -oX nmap-output.xml
```

This is very different than the switches ‘`-oX filename.xml`’ Because it saves the output in web friendly way, opening this file in web browser says it all.

---

### No Style-Sheet | ( `--webxml --no-stylesheet` )

These two options modify **Nmap’s XML output** for better compatibility with web-based applications and XML parsers.

- By default, Nmap's XML output contains a reference to an **XSL stylesheet** that allows for better viewing in a web browser:
- **`--no-stylesheet` removes this line**, making the XML file cleaner and ensuring it's parsed purely as XML data.

```bash
nmap -v -sT -sV -sC -A -p- 192.168.1.207 --webxml --no-stylesheet -oX nmap-output2.xml
```

Generates an XML file that is **cleaner**, optimized for web tools and parsers.

---

### Customize XML output | ( `--stylesheet` )

The `--stylesheet <URL>` option allows you to **customize the XML output's appearance** by specifying a custom XSL stylesheet. When opening the XML file in a web browser, the stylesheet will format the data into a readable, user-friendly report.

```bash
nmap -v -sT -sV -sC -A -p- 192.168.1.0/24 --webxml --stylesheet https://raw.githubusercontent.com/honze-net/nmap-bootstrap-xsl/master/nmap-bootstrap.xsl -oX nmap-output3.xml
```

This will create the output of nmap in the format of the given URL, you can see that output in the web-browser when opening that nmap output file.

---
