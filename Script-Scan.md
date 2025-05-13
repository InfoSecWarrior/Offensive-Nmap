## **Nmap Scripting Engine (NSE) — Script Scan | (`-sC` or `--script`)**

The Nmap Scripting Engine (NSE) is a powerful tool that allows users to automate scanning tasks, detect vulnerabilities, and gather additional information about a target. NSE uses Lua scripts to extend Nmap's capabilities, enabling more advanced network reconnaissance and security testing.



### **Basic Script Scan**

* Use the `-sC` option to run the default set of scripts for gathering basic information about the target, such as service version, OS, and security vulnerabilities.

  ```bash
  nmap -sC <target>
  ```

* Alternatively, use the `--script` option with a specific script or category:

  ```bash
  nmap --script <script-name> <target>
  ```

  ```bash
  nmap --script <category> <target>
  ```

  ```bash
  nmap --script "<expression>" <target>
  ```



### **Example with Multiple Options**

* Perform a comprehensive scan using multiple options like version detection, OS fingerprinting, and the default script set:

  ```bash
  nmap -v -Pn -sT -sV -sC -A -O -p- 192.168.1.208
  ```



### **Script Categories in NSE**

NSE scripts are organized into various categories to help streamline the scanning process. Some common categories include:

| **Category** | **Description**                                   |
|  ------------ | ------------- |
| auth         | Authentication bypass & brute-force attacks       |
| broadcast    | Network discovery scripts                         |
| brute        | Brute-force attack scripts                        |
| discovery    | Identifies hosts, services, and configurations    |
| dos          | Denial-of-service (DoS) testing                   |
| exploit      | Known vulnerability exploitation                  |
| external     | Uses external services (e.g., WHOIS, Shodan)      |
| fuzzer       | Sends unexpected data to test robustness          |
| intrusive    | May affect performance or trigger security alerts |
| malware      | Detects malware-infected hosts                    |
| safe         | No harmful impact on targets                      |
| version      | Improves service version detection                |
| vuln         | Detects vulnerabilities on a target               |



### **Locating NSE Scripts**

To list all available NSE scripts:

```bash
ls /usr/share/nmap/scripts/
```

To count the total number of scripts:

```bash
ls /usr/share/nmap/scripts/ | wc -l
```

Update the NSE script database:

```bash
nmap --script-updatedb
```

To filter scripts based on specific terms:

```bash
ls /usr/share/nmap/scripts/ | grep smb
```

To find a specific script for a service (e.g., FTP):

```bash
grep ftp /usr/share/nmap/scripts/script.db
```

To search for scripts by category:

```bash
grep brute /usr/share/nmap/scripts/script.db
```



### **Running Default Scripts (`--script=default`)**

The `default` script category runs a set of essential scripts that provide additional insights about open ports, services, and potential vulnerabilities:

```bash
nmap -v -Pn -sV -sT -A -O -p- --script=default 192.168.1.208
```

**Key Features of the Default Scripts:**

* Detects common vulnerabilities
* Retrieves service versions
* Extracts banner information
* Performs light brute-force testing where applicable



### **Script Category Scans (`--script=<category>`)**

You can use a specific script category to focus the scan on particular tasks.

* **Discovery category**:

  ```bash
  nmap -v -Pn -sT -sV -A -O -p- --script=discovery 192.168.1.208
  ```

* **Authentication category** (checks for authentication weaknesses):

  ```bash
  nmap -v -Pn -sT -sV -A -O -p- --script=auth 192.168.1.208
  ```

* **Brute-force category** (attempts brute-force attacks):

  ```bash
  nmap -v -Pn -sT -sV -A -O -p- --script=brute 192.168.1.208
  ```

* **Exploit category** (attempts known exploits):

  ```bash
  nmap -v -Pn -sT -sV -A -O -p- --script=exploit 192.168.1.208
  ```

* **Malware category** (checks for malware):

  ```bash
  nmap -v -Pn -sT -sV -A -O -p- --script=malware 192.168.1.208
  ```

* **DoS category** (checks for DoS vulnerabilities):

  ```bash
  nmap -v -Pn -sT -sV -A -O -p- --script=dos 192.168.1.208
  ```



### **Script Expression Scan (`--script "<expression>"`)**

NSE allows for the use of Lua expressions to target specific scripts or match patterns. This feature gives flexibility when running scripts on multiple targets or filtering by keywords.

**To run a specific script**:

```bash
nmap --script "ftp-anon" 192.168.1.208
```

**Run multiple scripts**:

```bash
nmap --script "ssh-brute,ftp-brute,smb-brute" 192.168.1.208
```

**Use a wildcard (`*`) to run multiple scripts matching a pattern**:

```bash
nmap --script "http-*" 192.168.1.208
```

**Run scripts using regular expressions**:

```bash
nmap --script "^(smb|ftp)-.*" 192.168.1.208
```

**Exclude certain scripts using `!`**:

```bash
nmap --script "vuln and not http-shellshock" 192.168.1.208
```

**Run a custom script**:

```bash
nmap --script "/path/to/custom-script.nse" 192.168.1.208
```
