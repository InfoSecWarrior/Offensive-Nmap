# RustScan

RustScan is a modern, high-speed port scanner built with Rust. It is designed to rapidly identify open ports and then hand them off to Nmap for more in-depth scanning. This hybrid approach combines RustScan's speed with Nmap's versatility.

* **GitHub Repository:** [https://github.com/RustScan/RustScan](https://github.com/RustScan/RustScan)


## Installation

### Install Rust

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

* Follow the default installation steps.
* Then activate the environment:

```bash
source "$HOME/.cargo/env"
```

* Optionally, restart your terminal or run:

```bash
exit
```

### Install Cargo (Rust's package manager)

```bash
apt update
apt install cargo
```

**Verify installation:**

```bash
cargo -h
```



### Install RustScan

```bash
cargo install rustscan
```

**Move the binary to system path:**

```bash
cp -v ~/.cargo/bin/rustscan /usr/local/bin
```



## Basic Usage

- **Show help menu**

    ```bash
    rustscan -h
    ```

- **Scan a single host**

    ```bash
    rustscan -a 192.168.1.208
    ```

- **Scan all ports**

    ```bash
    rustscan -a 192.168.1.208 -r 0-65535
    ```


## Integrate with Nmap

- Full port scan followed by Nmap aggressive scan

    ```bash
    rustscan -a 192.168.1.208 -r 0-65535 -- -A
    ```

- Scan specific ports

    ```bash
    rustscan -a 192.168.1.208 -p 81,300,591,593,832,981,1010,1311,1099,2082,2096,2408,3000,3001,3002,3003,3128,3333,4243,4567,4711,4712,4993,5000
    ```

- Save output in greppable format (RustScan only)

    ```bash
    rustscan -a 192.168.1.208 -r 1-65535 --greppable > /tmp/1.gnmap
    ```

- Save output using Nmap

    ```bash
    rustscan -a 192.168.1.1 -r 1-65535 -- -oG /tmp/2.gnmap
    ```

- Scan multiple IPs

    ```bash
    rustscan -a 192.168.1.1,192.168.1.208 -g > /tmp/2.gnmap
    ```

    `-g` saves output in `.gnmap` format (RustScan-native).

- Scan a subnet

    ```bash
    rustscan -a 192.168.1.1/24 -p 81,300,591,593 -- -oG /tmp/192.168.1.1
    ```

- Scan only accessible (live) hosts

    ```bash
    rustscan -a 192.168.1.1,192.168.1.208 --accessible > /tmp/3.gnmap
    ```