## **OUTPUT**

Nmap offers flexible and customizable output formats that suit both human interpretation and automated processing. By default, scan results are shown in the terminal, but using output options, you can save results to files in formats like normal, XML, s|\<rIpt kIddi3, and grepable. These are useful for analysis, reporting, or integration with other tools.



### **Normal Output (-oN)**

* Saves scan results in a human-readable format.

  ```
  nmap --top-ports 100 192.168.1.1/24 -oN scan-results.txt
  ```

  ```
  nmap --top-ports 100 example.com -oN scan-results.txt
  ```

  ```
  nmap -sn -PU 192.168.1.1/24 -oN scan-results.txt
  ```

  ```
  nmap -v -p0-65535 192.168.1.1 -oN scan-results.txt
  ```

  ```
  nmap -v -p T:80,443,U:53,111 192.168.1.1 -oN scan-results.txt
  ```



### **XML Output (-oX)**

* Saves scan results in XML format for machine parsing.

  ```
  nmap --top-ports 100 192.168.1.1/24 -oX scan-results.xml
  ```

  ```
  nmap --top-ports 100 example.com -oX scan-results.xml
  ```

  ```
  nmap -sn -PU 192.168.1.1/24 -oX scan-results.xml
  ```

  ```
  nmap -v -p0-65535 192.168.1.1 -oX scan-results.xml
  ```

  ```
  nmap -v -p T:80,443,U:53,111 192.168.1.1 -oX scan-results.xml
  ```



### **Script Kiddie Output (-oS)**

* Saves scan results in a stylized, readable format mimicking "leet speak."

  ```
  nmap --top-ports 100 192.168.1.1/24 -oS scan-results.txt
  ```

  ```
  nmap --top-ports 100 example.com -oS scan-results.txt
  ```

  ```
  nmap -sn -PU 192.168.1.1/24 -oS scan-results.txt
  ```

  ```
  nmap -v -p0-65535 192.168.1.1 -oS scan-results.txt
  ```

  ```
  nmap -v -p T:80,443,U:53,111 192.168.1.1 -oS scan-results.txt
  ```



### **Grepable Output (-oG)**

* Saves output in a format designed for easy parsing by tools like `grep` or `awk`.

  ```
  nmap --top-ports 100 192.168.1.1/24 -oG scan-results.txt
  ```

  ```
  nmap --top-ports 100 example.com -oG scan-results.txt
  ```

  ```
  nmap -sn -PU 192.168.1.1/24 -oG scan-results.txt
  ```

  ```
  nmap -v -p0-65535 192.168.1.1 -oG scan-results.txt
  ```

  ```
  nmap -v -p T:80,443,U:53,111 192.168.1.1 -oG scan-results.txt
  ```



### **All Formats at Once (-oA)**

* Saves scan results in normal, XML, and grepable formats using a single base filename.

  ```
  nmap --top-ports 100 192.168.1.1/24 -oA filename
  ```

  ```
  nmap --top-ports 100 example.com -oA filename
  ```

  ```
  nmap -sn -PU 192.168.1.1/24 -oA filename
  ```

  ```
  nmap -v -p0-65535 192.168.1.1 -oA filename
  ```

  ```
  nmap -v -p T:80,443,U:53,111 192.168.1.1 -oA filename
  ```



### **Increase Verbosity (-v, -vv)**

* Increases the amount of output detail shown during and after the scan.

  ```
  nmap -v example.com
  ```

  ```
  nmap -vv 192.168.1.1
  ```



### **Increase Debugging (-d, -dd)**

* Displays detailed diagnostic and troubleshooting information.

  ```
  nmap -d example.com
  ```

  ```
  nmap -dd 192.168.1.1
  ```



### **Show Port State Reasons (--reason)**

* Displays the reason why Nmap considers a port to be in a particular state.

  ```
  nmap --top-ports 100 192.168.1.1/24 --reason
  ```

  ```
  nmap --top-ports 100 example.com --reason
  ```

  ```
  nmap -sn -PU 192.168.1.1/24 --reason
  ```

  ```
  nmap -v -p0-65535 192.168.1.1 --reason
  ```

  ```
  nmap -v -p T:80,443,U:53,111 192.168.1.1 --reason
  ```
