# Naabu

Naabu is a fast and efficient port scanning tool developed by ProjectDiscovery. It is designed to quickly enumerate open ports on one or more hosts. Written in Go, Naabu is optimized for speed, making it ideal for scanning large networks.



## Installation

- You can install Naabu using Go:

    ```
    go install -v github.com/projectdiscovery/naabu/v2/cmd/naabu@latest
    ```

    Alternatively, download the latest precompiled binary from the [Naabu GitHub releases](https://github.com/projectdiscovery/naabu/releases).



## Basic Syntax

```
naabu [flags] -host <target>
```


## Examples

- **Scan a single target for open ports**

    ```
    naabu -host example.com
    ```

- **Scan multiple targets from a list file**

    ```
    naabu -list targets.txt
    ```

    (Each target should be on a new line in `targets.txt`.)

- **Scan specific ports on a target**

    ```
    naabu -host example.com -p 21,22,80,443
    ```

- **Scan the top 1000 commonly used ports**

    ```
    naabu -host example.com -top-ports 1000
    ```

- **Scan all 65535 ports on a target**

    ```
    naabu -host example.com -p -
    ```

- **Scan a target with a custom rate limit**

    ```
    naabu -host example.com -rate 5000
    ```

    (Useful for evasion and avoiding rate-based detection.)

- **Save scan results to a file**

    ```
    naabu -host example.com -o output.txt
    ```

- **Perform a service version detection scan**

    ```
    naabu -host example.com -sV
    ```

- **Scan while skipping host discovery**

    ```
    naabu -host example.com -Pn
    ```

    (Useful when the host is known to be online or behind a firewall.)

- **Fetch passive port data without active scanning**

    ```
    naabu -host example.com -passive
    ```

- **Pipe results into Nmap for detailed scanning**

    ```
    naabu -host example.com -nmap
    ```

- **Exclude specific IPs or networks**

    ```
    naabu -host example.com -exclude 192.168.1.1/24
    ```

- **Scan all IPs associated with a domain**

    ```
    naabu -host example.com -scan-all-ips
    ```