### List Scan | ( `-sL` )

```bash
nmap -v -sL 192.168.1.0/24
```

---

### Ping Scan & no Port Scan | ( `-sn` )

Not using verbose mode is recommended, as then it’ll only show UP hosts, otherwise all hosts will be listed in the output of the command

```bash
nmap -sn 192.168.1.0/24
```

```bash
nmap -v -sn 192.168.1.0/24 -oA mynetwork
```

By not giving the extension in the saving output file of nmap, the nmap will save the file in 3 different formats (extensions) ; which are as → `.nmap` `.gnmap` & `.xml` 

```bash
cat mynetwork.nmap | grep "scan report" | grep -v "down" | cut -d ' ' -f 5
```

This command will give you all the ‘UP’ hosts IP, but there might be names of the IPs too in there, therefore we have to scan again with the ‘ `-n` ’ switch for not resolving in the names.
**OR JUST USE “ .gnmap ” FILE FOR THIS PUROSE**

```bash
cat mynetwork.gnmap | grep Up | cut -d ' ' -f 2
```

```bash
cat mynetwork.gnmap | grep Up | cut -d ' ' -f 2 > Live-hosts-ip.txt
```

```bash
cat mynetwork.gnmap | grep Down | cut -d ' ' -f 2 > Down-hosts-ip.txt
```

Now you can scan again the down hosts to check on them again

```bash
nmap -sn -iL Down-hosts-ip.txt
```

### Not resolving in name i.e., stopping name resolution of Hosts ( `-n` )

```bash
nmap -v -n -sn 192.168.1.0/24 -oA mynetwork2
```

```bash
cat mynetwork2.nmap | grep "scan report" | grep -v "down" | cut -d ' ' -f 5
```

---

**It is always recommended to save the output of the nmap as over the LAN (WLAN) it will take alot time to scan multiple times, therefore save the output of the scan**

---

### Treat all hosts online / Don’t ping directly scans ports | ( `-Pn` )

*This switch is used when the ICMP requests are blocked at the target, then we tell Nmap to don’t ping, directly scan the targets assuming all target IPs are live*

```bash
nmap -v -Pn 192.168.1.21
```

---

### Pinging Target from SYN Packet / TCP SYN Ping | ( `-PS` )

*This command will work with Public IP as the target otherwise Nmap will do ARP Ping to the target in private network*

```bash
nmap -v -sn -PS 192.168.1.21
```

```bash
nmap -v -PS443 -sn 192.168.1.21
```

Now the Nmap will SYN Ping at port TCP-443, by default it does it port 80.

```bash
nmap -v -PS21,22,23,25,80,110,139,443,445,3389 -sn 192.168.1.21
```

Here we are using top used 10 ports to SYN Ping the target IP.

---

### Scanning Top Used Ports in Nmap | ( `--top-ports` )

```bash
nmap --top-ports 10 192.168.1.21
```

This will scan the top used 10 ports on the given IP address / IP Range

```bash
nmap --top-ports 100 192.168.1.21
```

This will scan the top used 100 ports on the given IP address / IP Range

---

```bash
cat /usr/share/nmap/nmap-services
```

This file has the information of all the 65536 ports from `0-65535` and their frequency, means how frequently are they used.

```bash
awk '$2~/tcp$/' /usr/share/nmap/nmap-services
```

This commands show all the TCP Ports which are there are frequently used.

```bash
awk '$2~/tcp$/' /usr/share/nmap/nmap-services | sort -r -k3 | head -n 20
```

Top used 20 ports reverse sort in the following file.

```bash
awk '$2~/tcp$/' /usr/share/nmap/nmap-services | sort -r -k3 | head -n 100 | awk -F '/tcp' '{print $1}' | awk '{print $2}' | tr '\n' ','
```

Top 100 used ports.

---

### Pinging Target from ACK Packet / TCP ACK Ping | ( `-PA` )

The target will reply from `RST` Flag.

*This command will work with Public IP as the target otherwise Nmap will do ARP Ping to the target in private network*

```bash
nmap -v -sn -PA 192.168.1.21
```

```bash
nmap -v -PA21,22,23,25,80,110,139,443,445,3389 -sn 192.168.1.21
```

Here we are using top used 10 ports to ACK Ping the target IP.

---

### Pinging Target from UDP Ping | ( `-PU` )

```bash
nmap -v -sn -PU 192.168.1.21
```

```bash
awk '$2~/udp$/' /usr/share/nmap/nmap-services | sort -r -k3 | head -n 20 | awk -F '/udp' '{print $1}' | awk '{print $2}' | tr '\n' ','
```

UDP 20 Top Most used ports.

```bash
nmap -v -PU31,161,137,123,138,1434,445,135,67,53,139,500,68,520,1900,4500,514,49152,162,63,69 -sn 192.168.1.21
```

Pinging top used 20 UDP Ports.

---

### SCTP Protocol Ping | ( `-PY` )

```bash
nmap -v -PY -sn 192.168.1.21
```

```bash
awk '$2~/sctp$/' /usr/share/nmap/nmap-services | sort -r -k3 | head -n 20 | awk -F '/sctp' '{print $1}' | awk '{print $2}' | tr '\n' ','
```

```bash
nmap -v -PY36422,80,11999,11997,11998,5675,5061,9,7626,2905,2904,2049 -sn 192.168.1.21
```

---

### ICMP Ping through Nmap | ( `-PE` )

```bash
nmap -v -PE -sn 192.168.1.21
```

-PE is for ICMP Echo Ping request.

```bash
nmap -v -PP -sn 192.168.1.21
```

-PE is for ICMP timestamp request.

```bash
nmap -v -PM -sn 192.168.1.21
```

-PM is for ICMP mask/netmask request.

```bash
nmap -v -PO -sn 192.168.1.21
```

-PO is for IP Protocol Ping. Sends RAW Packet, packet only has header.

---

### Don’t resolve in name / Keep it in IP | ( `-n` )

```bash
nmap -v -n -sn 192.168.1.21
```

-n Don’t do DNS Resolution, don’t resolve in name.

```bash
nmap -v -n -sn 192.168.1.21
```

```bash
nmap -v -sn 8.8.8.8
```

---

### Use specific DNS Server | ( `--dns-servers` )

This will use the given DNS Server in the command to resolve the target IP

```bash
nmap -v -sn 8.8.8.8 --dns-servers 1.1.1.1
```

```bash
nmap -v -sn 8.8.8.8 --dns-servers 8.8.4.4
```

---

### Resolve the Name of the input / target IPs | ( `-R` )

*Even without using this switch, the name resolution is on by default*

```bash
nmap -v -R -sn 8.8.8.8
```

---

### Use system DNS | ( `--system-dns` )

This will resolve the target IPs from the system DNS ( `/etc/resolv.conf` )

```bash
nmap -v -R -sn 8.8.8.8 --system-dns
```

---

### Tracing the route | ( `--traceroute` )

Defines the nodes between you can the entered target IP

```bash
nmap -v -R -sn 8.8.8.8 --traceroute
```

---
