```bash
nmap domain1.com domain2.org
```

Scanning domain names

```bash
nmap 192.168.1.35 192.168.1.1 192.168.1.27
```

Scanning IPs

```bash
nmap -v 192.168.1.27
```

-v for verbose mode, gives detailed output of the scan

```bash
nmap -v domain1.com domain2.com
```

**If you have executed the command and forgot to add ‘ -v ’ ( for verbose mode ), then press ‘ v ’ and press enter on the terminal where the command is executed and still in running, this will add the verbose in the command, multiple ‘ v ’s can be pressed for more and more detailed verbose output.**

**Still the ‘ -v ‘ switch in the command is always preferred for better output**

*( Verbosity Increased to 1 ) then ( Verbosity Increased to 2 ) then ( Verbosity Increased to 3 ) and so on.*

```bash
nmap 192.168.1.0/24
```

Scanning the IP range / Subnet

---

### Scanning Selected IPs

```bash
nmap -v 192.168.1.1,5,27,100,207
```

Scanning selected IPs in Comma Separated Values ( CSV )

```bash
nmap -v 192.168.1.1 192.168.1.5 192.168.1.27 192.168.1.100 192.168.1.207
```

---

### Scanning in Range

```bash
nmap -v 192.168.1.1-100
```

```bash
nmap -v 192.168.1.1-51,250-254
```

```bash
nmap 192.168.1.1-10,50-60,100,200,240-254
```

```bash
nmap 192.168.1.1,10,50,60,100,200,244,231,31,61
```

---

### Scanning in Range in Multiple Networks | Scanning Multiple Networks with Range

```bash
nmap 192.168.1,2.1-100
```

Here we are scanning two networks 192.168.1.0 and 192.168.2.0 and in these networks we are scanning from IPs 1 to 100 of both. ( Total 200 IPs )

---

### Scanning Multiple Networks

```bash
nmap -v 192.168.1-3.1/24
```

Here we are scanning whole networks of 192.168.1.1/24 then 192.168.2.1/24 and then 192.168.3.1/24

---

### Scanning IP Lists in Nmap

```bash
vim ip.txt
```

```bash
192.168.1.1
192.168.1.10
192.168.1.31
192.168.1.61
192.168.1.100
192.168.1.102
192.168.1.105
192.168.1.123
```

*Save and Exit!*

```bash
nmap -v -iL ip.txt
```

Here -i is for input and L is for list, it makes input list and then the location of the list of the IP addresses.

---

### Scanning Random Targets

```bash
nmap -v -iR 10
```

Nmap will scan 10 Random Public IPs.

```bash
nmap -v -iR 21
```

Here Nmap will scan 21 Random Public IPs.

---

### Excluding IPs in Nmap & Excluding in IP list in Nmap

```bash
nmap -v 192.168.1.1/24 --exclude 192.168.1.1-10
```

Nmap will scan the whole network of 192.168.1.1/24 and will exclude the IPs from range 1 to 10 of the same network.

```bash
nmap -v 192.168.1.1/24 --excludefile ip.txt
```

This will exclude the IPs of the given file in the following network 192.168.1.1/24

---

### prips

prips — Prips is a tool that can be used to print all of the IP address on a given range. It can enhance the usability of tools that are made to work on only one host at a time (e.g. whois).

The `prips` tool is used in Linux to generate a list of IP addresses within a given range. It's helpful in networking tasks like scanning, subnetting, and automating IP address assignments.

```bash
apt install prips
```

```bash
prips 192.168.1.1 192.168.1.10
```

```bash
prips 192.168.1.0/24
```

```bash
prips 192.168.1.0/28
```

```bash
prips 10.0.0.0/28
```

```bash
prips 10.0.0.0/23
```

---
