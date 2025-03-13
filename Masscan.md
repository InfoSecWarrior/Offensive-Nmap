Masscan is a high-speed network scanner that can scan the entire internet in under 6 minutes. It is similar to `nmap` , but much faster, leveraging asynchronous scanning techniques.

### Installation :

**Install dependencies**

```bash
apt update
apt install git make gcc
```

**Installing Masscan from package manager**

```bash
apt install -y masscan
```

- Installs `masscan` from the package manager (if available in the repositories). This version may not be the latest.

---

**Cloning the Masscan Repository**

```bash
git clone **https://github.com/robertdavidgraham/masscan**
```

- Clones the official Masscan repository form the GitHub into the `/opt/masscan` directory

**Navigate to the cloned directory**

```bash
cd /opt/masscan/
```

**Compile Masscan**

```bash
make -j$(nproc)
```

**Install Masscan**

```bash
make install
```

```bash
man masscan
```

```bash
masscan -h
masscan --help
```

---

### Basic Usage :

Scan all ports on a single IP

```bash
masscan -p- 192.168.1.208
```

---

### Defining Rate Limit

```bash
masscan -p- 192.168.1.208 --max-rate 1000
```

---

### Scan specific ports on a host

```bash
masscan -p 80,443,8080,8443 192.168.1.208 --max-rate 1000
```

### Scan an entire subnet

```bash
masscan -p0-65535 192.168.1.1/24 --max-rate 2000
```

---

### Scan with banner garbbing enabled

```bash
masscan -p- --max-rate 2000 --banners 192.168.1.208
```

---

### Saving the Output

```bash
masscan -p- --max-rate 2000 --banners 192.168.1.208 **-oG** /tmp/scan-report.gnmap
```

- Saved in `.gnmap` grepable nmap format.

```bash
masscan -p- --max-rate 2000 --banners 192.168.1.208 -oJ /tmp/scan-report.json
```

- Saved in `.json` format.

---

### Check `nmap` compatibility.

```bash
masscan --nmap
```

---

### Advanced Usage

```bash
masscan -p 21,22,25,53,80,110,135,139,143,443,465,587,993,995,1080,1433,1521,1723,3306,3389,5900,8080,8443,10000 192.168.1.208 --max-rate 2000
```

- Scanning with a large set of ports

---

### Scanning all ports and save results

```bash
masscan -p- --max-rate 2000 192.168.1.208 **-oG** /tmp/scan-report.gnmap
```

---

### Scan all TCP & UDP ports:

```bash
masscan -pU:0-65535,T:0-65535 192.168.1.208 --max-rate 2000
```

---

- `Masscan` uses raw sockets, so it requires root privileges.
- The `--max-rate` option controls how aggressive the scan is.
- To avoid detection, adjust scan speeds carefully.
- **Masscan** does not support scanning domains directly. It only works with **IP addresses** or **IP ranges**.
- First resolve the domain to an IP address using a tool like `nslookup` or `dig`, and then use Masscan on the resolved IP.
    
    ```bash
    nslookup example.com
    ```
    
    OR
    
    ```bash
    dig +short example.com
    ```
    

---
