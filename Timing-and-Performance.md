Nmap offers a variety of options to optimize scanning speed and accuracy. These include settings to control the number of parallel probes, retries, delays, and timeouts based on the network's performance and the sensitivity of the target.


### **Timing Templates (`-T`)**

The `-T` option in **Nmap** adjusts the **scan speed and timing**, controlling how aggressively Nmap sends probes. It ranges from **paranoid** (slowest) to **insane** (fastest).

- Example

    ```bash
    nmap -v -sT -T0 -p- 192.168.1.50
    ```

    ```bash
    nmap -v -sT -T4 -p- 192.168.1.50
    ```



### **Timing Templates Breakdown**

| **Level** | **Name**   | **Speed** | **Use Case**                                | **Description**                            |
| --------- | ---------- | --------- | ------------------------------------------- | ------------------------------------------ |
| `-T0`     | Paranoid   | Very slow | Avoids IDS detection (one probe at a time)  | Serial scan with long delays (stealthiest) |
| `-T1`     | Sneaky     | Slow      | IDS evasion with slightly better speed      | Very slow scan with long delays            |
| `-T2`     | Polite     | Moderate  | Reduces network load, good for stealth      | Slower scan to reduce bandwidth usage      |
| `-T3`     | Normal     | Default   | Balanced between speed and stealth          | Default timing balance                     |
| `-T4`     | Aggressive | Fast      | Best for quick scanning but more detectable | Faster scan but may trigger IDS/IPS        |
| `-T5`     | Insane     | Very fast | Risk of missing data due to speed           | Fastest scan, likely to cause packet loss  |

**Recommended Use Cases:**

* **`-T3` (Normal):** For general scanning.
* **`-T4` (Aggressive):** For quick results.
* **`-T0` or `-T1`:** For stealthy scans to avoid detection.



### **Parallel Scanning Options**

These options control how many hosts or probes Nmap scans at once.

| **Option**          | **Description**                              |
|  | -------------------------------------------- |
| `--min-hostgroup`   | Minimum number of hosts to scan in parallel  |
| `--max-hostgroup`   | Maximum number of hosts to scan in parallel  |
| `--min-parallelism` | Minimum number of probes to send in parallel |
| `--max-parallelism` | Maximum number of probes to send in parallel |

- **Example:**

    ```bash
    nmap --max-parallelism 20 192.168.1.1/24
    ```

    ```bash
    nmap --min-hostgroup 10 --max-parallelism 20 192.168.1.1/24
    ```




### **Round Trip Time (RTT) Timeout Options**

These options control the maximum and minimum time Nmap waits for a response before retrying or timing out.

| **Option**              | **Description**                     |
| ----------------------- | ----------------------------------- |
| `--min-rtt-timeout`     | Minimum time to wait for a response |
| `--max-rtt-timeout`     | Maximum time to wait for a response |
| `--initial-rtt-timeout` | Initial timeout value for RTT       |

- **Example:**

    ```bash
    nmap --max-rtt-timeout 500ms 192.168.1.1
    ```




### **Retries and Scan Timeouts**

These options help manage how Nmap handles packet loss and slow hosts.

| **Option**         | **Description**                              |
| ------------------ | -------------------------------------------- |
| `--max-retries`    | Maximum number of retries for a failed probe |
| `--host-timeout`   | Abandon scanning a host if it takes too long |
| `--scan-delay`     | Wait at least this time between probes       |
| `--max-scan-delay` | Maximum wait time between probes             |

- **Example:**

    ```bash
    nmap --max-retries 2 --host-timeout 5m 192.168.1.1
    ```




### **Packet Rate Control**

Control the rate at which packets are sent to optimize scanning speed.

| **Option**   | **Description**                                 |
| ------------ | ----------------------------------------------- |
| `--min-rate` | Sends packets no slower than the specified rate |
| `--max-rate` | Sends packets no faster than the specified rate |

- **Example:**

    ```bash
    nmap --min-rate 1000 --max-rate 5000 192.168.1.1
    ```
