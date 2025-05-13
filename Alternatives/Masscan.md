# Masscan

Masscan is an extremely fast network scanner capable of scanning the entire Internet in under six minutes. It functions similarly to `nmap` but is significantly faster, utilizing asynchronous transmission for high-performance scanning.



## Installation

**Install dependencies**

* Updates packages and installs necessary build tools

```
apt update
apt install git make gcc
```

**Install Masscan using the package manager**

* Installs Masscan from the system’s package repositories

```
apt install -y masscan
```

**Access the manual and help pages**

```
man masscan
```

```
masscan -h
```

```
masscan --help
```



## Examples


* **Scans all 65535 TCP ports on a target**

    ```
    masscan -p- 192.168.1.208
    ```


* **Controls scan speed to avoid detection or congestion**

    ```
    masscan -p- 192.168.1.208 --max-rate 1000
    ```

- **Scan specific ports on a target**

    ```
    masscan -p 80,443,8080,8443 192.168.1.208 --max-rate 1000
    ```

- **Scan an entire subnet**

    ```
    masscan -p0-65535 192.168.1.1/24 --max-rate 2000
    ```

* **Attempts to grab banners from open services**

    ```
    masscan -p- --max-rate 2000 --banners 192.168.1.208
    ```


- **Save results in Grepable format (`.gnmap`)**

    ```
    masscan -p- --max-rate 2000 --banners 192.168.1.208 -oG /tmp/scan-report.gnmap
    ```

- **Save results in JSON format**

    ```
    masscan -p- --max-rate 2000 --banners 192.168.1.208 -oJ /tmp/scan-report.json
    ```

- **Check `nmap`-compatible output support**

    ```
    masscan --nmap
    ```


- **Scan a wide range of common ports**

    ```
    masscan -p 21,22,25,53,80,110,135,139,143,443,465,587,993,995,1080,1433,1521,1723,3306,3389,5900,8080,8443,10000 192.168.1.208 --max-rate 2000
    ```

- **Scan all ports and save output**

    ```
    masscan -p- --max-rate 2000 192.168.1.208 -oG /tmp/scan-report.gnmap
    ```

- **Scan all TCP and UDP ports**

    ```
    masscan -pU:0-65535,T:0-65535 192.168.1.208 --max-rate 2000
    ```