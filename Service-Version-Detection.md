## **Service Version Detection**

The **Service Version Detection** feature in Nmap allows you to identify the specific services and their versions running on open ports of a target system. This insight is critical for vulnerability analysis, patch management, and overall security auditing.

### **Basic Service Version Detection**

* Attempts to identify services and versions on open ports.

  ```
  nmap -sV 192.168.1.1
  ```



### **Set Version Intensity**

* Controls how aggressive Nmap is when attempting version detection. Values range from 0 (light) to 9 (intense).

  ```
  nmap -sV --version-intensity 5 192.168.1.1
  ```

  ```
  nmap -v -n -sV --version-intensity 5 192.168.1.1
  ```

  ```
  nmap -v -n -sT -sV --version-intensity 5 192.168.1.0/24
  ```



### **Use Version Light**

* Limits probes to those most likely to succeed. Equivalent to intensity level 2.

  ```
  nmap -sV --version-light 192.168.1.1
  ```



### **Use Version All**

* Uses all available probes to detect service versions. Equivalent to intensity level 9.

  ```
  nmap -sV --version-all 192.168.1.1
  ```
