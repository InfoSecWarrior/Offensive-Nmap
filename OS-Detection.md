## **OS Detection**

Nmap’s OS detection feature is a valuable capability used to determine the operating system running on a target machine. This information can aid in identifying OS-specific vulnerabilities and tailoring penetration tests accordingly. Although powerful, OS detection is not always precise and should be verified with supplementary tools for critical assessments.

### **Basic OS Detection**

* Attempts to identify the target's operating system using TCP/IP stack fingerprinting.

  ```
  nmap -O 192.168.1.1
  ```

  ```
  nmap -vv -sT -O -p- 192.168.1.1/24
  ```

  ```
  nmap -v -O example.com -oA scan-results
  ```



### **Limit OS Detection**

* Restricts OS detection to hosts that appear promising, reducing unnecessary effort and time.

  ```
  nmap -O --osscan-limit 192.168.1.0/24
  ```



### **Aggressive OS Detection**

* Makes educated guesses even when the results are not conclusive.

  ```
  nmap -O --osscan-guess 192.168.1.1
  ```



### **Advanced OS Detection**

* Enables OS detection along with version detection, script scanning, and traceroute for comprehensive analysis.

  ```
  nmap -A 192.168.1.1
  ```
