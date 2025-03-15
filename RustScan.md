RustScan is a modern and fast port scanner that integrates with `nmap` . It helps to quickly identify open ports and execute deeper scans using `nmap` .

**Rust-Scan GitHub     →     https://github.com/RustScan/RustScan**

**Rust-Scan Releases   →     https://github.com/RustScan/RustScan/releases**

**Rust Programming Language Installation     →     https://www.rust-lang.org/tools/install**

---

## Installation

### Install Rust

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

- Proceed with standard installation ( Default )

```bash
source "$HOME/.cargo/env"
```

```bash
exit
```

---

### Install Cargo ( Package Manager for Rust )

```bash
apt update
```

```bash
apt install cargo
```

**Verify Cargo Installation**

```bash
cargo -h
```

---

### Install RustScan

```bash
cargo install rustscan
```

```bash
cp -v /root/.cargo/bin/rustscan /usr/local/bin
```

---

### Usage

```bash
rustscan -h
```

### Basic Scan

```bash
rustscan -a 192.168.1.208
```

### Scan All Ports

```bash
rustscan -a 192.168.1.208 -r 0-65535
```

---

### Scan All Ports with Aggressive Nmap Scan

```bash
rustscan -a 192.168.1.208 -r 0-65535 -- -A
```

- `rustscan` → A fast port scanner designed to quickly find open ports before handing off to Nmap.
- `-a 192.168.1.208` → Specifies the target IP address (192.168.1.208).
- `-r 0-65535` → Scans all possible ports (0-65535).
- `--` → Separates RustScan options from the Nmap options.
- `-A` → Runs aggressive scanning mode in Nmap, which includes:
    - OS detection
    - Version detection
    - Script scanning
    - Traceroute
1. **RustScan quickly scans** all 65,536 ports to find which ones are open.
2. **RustScan passes the open ports to Nmap**.
3. **Nmap performs a deeper scan** with `A`, gathering detailed information about the open services.

---

### Scan Specific Ports

```bash
rustscan -a 192.168.1.208 -p 81,300,591,593,832,981,1010,1311,1099,2082,2096,2408,3000,3001,3002,3003,3128,3333,4243,4567,4711,4712,4993,5000
```

---

### Save Output

**Save Output in Greppable Format.**

```bash
rustscan -a 192.168.1.208 -r 1-65535 --greppable > /tmp/1.gnmap
```

**Save Output to File**

```bash
rustscan -a 192.168.1.1 -r 1-65535 -- -oG > /tmp/2.gnmap
```

Here, it is saving output with the help of `nmap` as there is `--` (double hyphens that is used for nmap integration in `rustscan`)

---

### Scan Multiple Hosts

```bash
rustscan -a 192.168.1.1,192.168.1.208 -g > /tmp/2.gnmap
```

Here, it is saving in `.gnmap` format on it’s own, rather than using `nmap` for saving the output in `.gnmap` format.

---

### Scan a Subnet

```bash
 rustscan -a 192.168.1.1/24 -p 81,300,591,593 -- -oG /tmp/192.168.1.1
```

---

### Scan Accessible Hosts

```bash
rustscan -a 192.168.1.1,192.168.1.208 --accessible > /tmp/3.gnmap
```

---

**Conclusion:**

RustScan is a powerful and efficient port scanner that speeds up initial scans before deeper `nmap` analysis. The provided commands and scripts help automate port scanning and service detection effectively.

---
