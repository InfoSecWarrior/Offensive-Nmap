Nmap provides several timing and performance tuning options to optimize scanning speed and accuract. These options allow users to adjust parallelism, retries, delays, and timeouts based on network conditions and target sensitivity.

### Timing Templates | ( `-T` )

The `-T` option in **Nmap** controls the **scan speed and timing** by adjusting how aggressively Nmap sends probes. It ranges from **paranoid** (slowest) to **insane** (fastest).

```bash
nmap -v -sT -T0 -p- 192.168.1.50
```

```bash
nmap -v -sT -T4 -p- 192.168.1.50
```

Performs a SYN scan with aggressive timing.

Best to use = -T4

**Higher is faster → `-T<0-5>`**

### Timing Templates (`T<level>`)

| **Level** | **Name** | **Speed** | **Use Case** | **Description** |
| --- | --- | --- | --- | --- |
| `-T0` | Paranoid | Very slow | Avoids IDS detection (one probe at a time) | Paranoid mode — Serial scan with long delays (stealthiest) |
| `-T1` | Sneaky | Slow | IDS evasion with slightly better speed | Sneaky mode — Very slow scan with long delays |
| `-T2` | Polite | Moderate | Reduces network load, good for stealth | Polite mode — Slower scan to reduce bandwidth usage |
| `-T3` | Normal | Default | Balanced between speed and stealth | Normal mode — Default timing balance |
| `-T4` | Aggressive | Fast | Best for quick scanning but more detectable | Aggressive mode — Faster scan but may trigger IDS/IPS |
| `-T5` | Insane | Very fast | Risk of missing data due to speed | Insane mode — Fastest scan, likely to cause packet loss |

`-T0` — Paranoid mode — Serial scan with long delays (stealthiest)

`-T1` — Sneaky mode — Very slow scan with long delays

`-T2` — Polite mode — Slower scan to reduce bandwidth usage

`-T3` — Normal mode — Default timing balance

`-T4` — Aggressive mode — Faster scan but may trigger IDS/IPS

`-T5` — Insane mode — Fastest scan, likely to cause packet loss

### Which to use?

- Use **`T3` (Normal)** for general scanning.
- Use **`T4` (Aggressive)** for quick results.
- Use **`T0` or `T1`** for stealthy scans to avoid detection.

---

### Parallel Scanning Options

This options control how many hosts or probes Nmap scans at once.

**`min-hostgroup`**             <size>           **Minimum** number of hosts to scan in parallel.

**`max-hostgroup`**             <size>           **Maximum** number of hosts to scan in parallel.

**`--min-parallelism`**         <numprobes>      **Minimum** number of probes to send in parallel.

**`--max-parallelism`**         <numprobes>      **Maximum** number of probes to send in parallel.

---

### **Nmap Parallel Scanning Options: Boosting Scan Speed**

Parallel scanning in **Nmap** helps increase speed by controlling how many probes are sent at once. Here are the key options:

| **Option** | **Description** |
| --- | --- |
| `-T<0-5>` | Controls overall scan speed (higher = faster) |
| `--min-rate` | Sets a minimum number of packets per second |
| `--max-rate` | Sets a maximum number of packets per second |
| `--min-parallelism` | Minimum concurrent probes sent at once |
| `--max-parallelism` | Maximum concurrent probes sent at once |
| `--min-rtt-timeout` | Minimum time before retrying a probe |
| `--max-rtt-timeout` | Maximum time before retrying a probe |

---

```bash
nmap --max-parallelism 20 192.168.1.1/24
```

```bash
nmap --min-hostgroup 10 --max-parallelism 20 192.168.1.1/24
```

Scan the subnet using up to 20 parallel probes.

---

### Round Trip Time (RTT) Timeout Options

These options control how long Nmap waits for a response before retrying or timing out.

**`--min-rtt-timeout`**             <time>        Minimum time to wait for response

**`--max-rtt-timeout`**             <time>        Maximum time to wait for response

**`--initial-rtt-timeout`**         <time>        Initial timeout value for RTT

Example:

```bash
nmap --max-rtt-timeout 500ms 192.168.1.1
```

Limits the maximum RTT timeout to 500 milliseconds.

---

### Retries and Scan Timeouts

These options help manage how Nmap handles packet loss and slow hosts.

**`--max-retries`**           <tries>        Maximum number of retires for a failed probe.

**`--host-timeout`**          <time>        Abandon scanning a host if it takes too long.

**`--scan-delay`**            <time>        Wait at least this time between probes.

**`--max-scan-delay`**        <time>        Maximum wait time between probes.

Example:

```bash
nmap --max-retries 2 --host-timeout 5m 192.168.1.1
```

Limit retries to 2 attempts and stop scanning a host after 5 minutes.

---

### Packet Rate Control

Control the rate at which packets are sent.

**`--min-rate`**     <num>     Send packets no slower than <num> per second

**`--max-rate`**     <num>     Send packets no faster than <num> per second

Example:

```bash
nmap --min-rate 1000 --max-rate 5000 192.168.1.1
```

Sends packets at a rate between 1,000 and 5,000 packets per second.

---

```bash
nmap -v -Pn -sT -p- --max-rate 10 192.168.1.1
```

10 is considered good here.

---
