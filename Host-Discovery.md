## **Host Discovery in Nmap**

Host discovery is a crucial phase in both internal and external penetration testing. It helps identify live hosts within a network, enabling focused and efficient scanning. Nmap provides multiple techniques for host discovery, which vary depending on the testing environment.

---

### **Internal Host Discovery Techniques**

* **Send ARP requests to discover live hosts**

  ```
  nmap -sn 192.168.1.0/24
  ```

* **Send ICMP echo requests to discover active hosts**

  ```
  nmap -sn -PE 192.168.1.0/24
  ```

* **Send TCP SYN packets to discover hosts with open ports**

  ```
  nmap -sn -PS80,443 192.168.1.0/24
  ```

* **Send UDP packets to discover hosts responding on UDP ports**

  ```
  nmap -sn -PU53 192.168.1.0/24
  ```

---

### **External Host Discovery Techniques**

* **Send ICMP echo requests to external hosts**

  ```
  nmap -sn -PE example.com
  ```

* **Send TCP SYN or ACK packets to detect external hosts**

  ```
  nmap -sn -PS80,443 example.com
  ```

  ```
  nmap -sn -PA80,443 example.com
  ```

* **Send UDP packets to detect UDP services on external targets**

  ```
  nmap -sn -PU53 example.com
  ```

---

## **Additional Host Discovery Options**

* **List all hosts without scanning (list scan)**

  ```
  nmap -sL 192.168.0.0/24
  ```

* **Perform a ping scan (no port scanning)**

  ```
  nmap -sn 192.168.0.0/24
  ```

* **Disable DNS resolution**

  ```
  nmap -n 192.168.1.1
  ```

  ```
  nmap -n 192.168.1.1/24
  ```

  ```
  nmap -n example.com
  ```

* **Treat all hosts as online, skip host discovery**

  ```
  nmap -v -Pn 192.168.1.1/24
  ```

  ```
  nmap -Pn 192.168.1.1
  ```

  ```
  nmap -Pn example.com
  ```

* **Use TCP SYN discovery to specified ports**

  ```
  nmap -v -sn -PS80,443 192.168.1.1/24
  ```

* **Use TCP ACK discovery to specified ports**

  ```
  nmap -v -sn -PA80,443 192.168.1.1/24
  ```

* **Use UDP discovery to specified ports**

  ```
  nmap -sn -PU53 192.168.1.1/24
  ```

* **Send ICMP echo requests for host discovery**

  ```
  nmap -v -sn -PE 192.168.1.1/24
  ```

* **Send ICMP timestamp requests**

  ```
  nmap -v -sn -PP 192.168.1.1/24
  ```

* **Send ICMP netmask requests**

  ```
  nmap -sn -PM 192.168.1.1/24
  ```

* **Use custom DNS servers for name resolution**

  ```
  nmap --dns-servers 8.8.8.8 192.168.1.1
  ```

* **Use system DNS resolver**

  ```
  nmap --system-dns 192.168.1.1
  ```

* **Perform a traceroute to the target**

  ```
  nmap --traceroute 192.168.1.1
  ```
