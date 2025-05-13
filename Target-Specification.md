Target specification is a core aspect of using **Nmap** effectively. It determines which IP addresses, hosts, or networks Nmap will scan. You can define targets in several ways, including single IPs, ranges, CIDR notation, or input files.



### **Scan a specific hostname or IP address**

* Scans a single host using either a domain name or an IP address.

  ```
  nmap -v scanme.nmap.org
  ```

  ```
  nmap -v 192.168.1.1
  ```



### **Scan using CIDR notation**

* Scans an entire subnet using CIDR (Classless Inter-Domain Routing) notation.

  ```
  nmap -v -p 80,443 example.com/24
  ```

  ```
  nmap -v 192.168.1.1/24
  ```



### **Scan a range of IP addresses**

* Scans a specific range of IPs using hyphenated notation.

  ```
  nmap 192.168.0.1-254
  ```

  ```
  nmap 10.0.0-255.1-254
  ```



### **Scan from a list of targets in a file**

* Reads target IPs or hostnames from a file.

  ```
  nmap -v -iL targets.txt
  ```



### **Scan random IP addresses**

* Scans a specified number of randomly selected IP addresses.

  ```
  nmap -iR 3
  ```
* Scans random IPs on specific ports.

  ```
  nmap -v -p 80,443,22,21,445 -iR 3
  ```



### **Exclude specific IPs or hosts from scan**

* Omits specific hosts from the scan results.

  ```
  nmap 192.168.0.0/24 --exclude 192.168.0.1,192.168.0.2
  ```



### **Exclude a list of IPs from a file**

* Excludes all hosts listed in a file from the scan.

  ```
  nmap 192.168.0.0/24 --excludefile exclude.txt
  ```



## **Using `prips` for IP Range Generation**

The `prips` utility is used to generate lists of IP addresses from a given range or subnet. It’s handy when tools don’t support range input directly or for automation.



### **Install `prips`**

* Installs the `prips` tool.

  ```
  apt install prips
  ```



### **Generate a list of IPs from a start and end address**

* Prints all IPs from 192.168.1.1 to 192.168.1.10.

  ```
  prips 192.168.1.1 192.168.1.10
  ```



### **Generate IPs from a subnet**

* Lists all IPs in a /24 subnet.

  ```
  prips 192.168.1.0/24
  ```
* Lists all IPs in a /28 subnet.

  ```
  prips 192.168.1.0/28
  ```
* Lists IPs in another /28 subnet.

  ```
  prips 10.0.0.0/28
  ```
* Lists IPs in a /23 subnet (larger range).

  ```
  prips 10.0.0.0/23
  ```