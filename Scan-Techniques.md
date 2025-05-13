## **Scan Techniques**

Scan techniques define how Nmap probes target systems to discover open ports and services. Each method has unique characteristics in terms of stealth, speed, and reliability, making them suitable for different scanning scenarios such as reconnaissance, firewall evasion, or vulnerability assessment.



### **SYN Scan - Half-open TCP scan**

* Sends a SYN packet and waits for SYN/ACK (open) or RST (closed). Does not complete TCP handshake.
* Fast and stealthy; widely used.

  ```
  nmap -sS -p- 192.168.1.1/24
  ```
  ```
  nmap -v -sS -p 21,22,25,80,443 example.com
  ```
  ```
  nmap -v -Pn -sS example.com/24
  ```



### **Connect Scan - Full TCP connection scan**

* Completes full TCP handshake; easily detected by firewalls and IDS.

  ```
  nmap -v -sT -p- 192.168.1.1/24
  ```
  ```
  nmap -v -sT -p 1-100 example.com
  ```
  ```
  nmap -v -sT example.com/24
  ```
  ```
  nmap -v -sT -p 80,443,22,21,25,445,69,53 example.com
  ```



### **ACK Scan - Firewall detection**

* Sends ACK packets to determine if ports are filtered or unfiltered.

  ```
  nmap -v -sA -p- 192.168.1.1/24
  ```
  ```
  nmap -v -sA -Pn example.com
  ```
  ```
  nmap -v -sA example.com/24
  ```



### **Window Scan - Based on TCP window size**

* Sends ACK packets with zero window size; rarely used, relies on TCP stack behavior.

  ```
  nmap -sW 192.168.1.1/24
  ```
  ```
  nmap -v -sW example.com
  ```
  ```
  nmap -v -sW example.com/24
  ```



### **Maimon Scan - Uses TCP URG flag**

* Similar to Window scan; sends TCP packets with the URG flag.

  ```
  nmap -sM 192.168.1.1/24
  ```
  ```
  nmap -v -sM example.com
  ```
  ```
  nmap -v -sM example.com/24
  ```



### **Custom TCP Flags Scan - Set specific flags manually**

* Enables granular TCP scan behavior by defining custom flags.

  ```
  nmap -Pn -v --scanflags SYN -p 80,443 example.com
  ```



### **UDP Scan - Scan for open UDP ports**

* Scans UDP services, slower and less reliable due to lack of response.

  ```
  nmap -sU 192.168.1.1
  ```
  ```
  nmap -v -sU example.com
  ```



### **Idle Scan - Stealth scan using a zombie host**

* Uses a third-party "zombie" host to scan the target indirectly.

  ```
  nmap -sI zombie_host target_host
  ```



### **FTP Bounce Scan - Using FTP server as proxy**

* Leverages misconfigured FTP servers to perform indirect scans.

  ```
  nmap -b ftp_server target_host
  ```



### **TCP Null Scan - No TCP flags set**

* Sends TCP packets with no flags; no response implies port is open.

  ```
  nmap -v -sN -p 80,443 192.168.1.1
  ```
  ```
  nmap -v -sN -p 80,443 example.com
  ```
  ```
  nmap -v -Pn -sN example.com
  ```



### **TCP FIN Scan - FIN flag set only**

* Sends FIN packets; closed ports respond with RST, open ports do not respond.

  ```
  nmap -v -sF -p- 192.168.1.1
  ```
  ```
  nmap -v -sF -p 80,443 example.com
  ```



### **TCP Xmas Scan - FIN, PSH, and URG flags set**

* Sends a “Christmas tree” packet; open ports stay silent, closed ports send RST.

  ```
  nmap -v -sX -p 80,443 192.168.1.1
  ```
  ```
  nmap -v -sX -p 80,443 example.com
  ```
