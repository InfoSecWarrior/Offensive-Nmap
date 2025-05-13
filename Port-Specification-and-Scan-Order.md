## **Port Specification and Scan Order**

Nmap, by default, scans the 1000 most common ports. However, customizing the ports to scan and the scan order allows more efficient and targeted reconnaissance. These options help avoid scanning irrelevant ports and can significantly speed up or fine-tune the process.



## **Scan All 65535 Ports**

* **Scan all TCP ports (0–65535)**

  ```
  nmap -v -p0-65535 192.168.1.1
  ```



## **Scan Specific Ports**

* **Scan a single port**

  ```
  nmap -v -p 22 192.168.1.1
  ```

* **Scan multiple specified ports**

  ```
  nmap -v -p 22,80,443 192.168.1.1
  ```

* **Scan ports using protocol-specific notation**

  ```
  nmap -v -p T:80,443,U:53,111 192.168.1.1
  ```

* **Scan all ports with shorthand**

  ```
  nmap -v -p- example.com
  ```



## **Exclude Specific Ports**

* **Exclude a single port**

  ```
  nmap -v --exclude-ports 80 example.com
  ```

* **Exclude a port range**

  ```
  nmap -v --exclude-ports 100-400 192.168.1.1/24
  ```

* **Exclude multiple ranges**

  ```
  nmap --exclude-ports 1-1000,3389-3390
  ```



## **Fast Scan (Top 100 Ports)**

* **Scan only the top 100 most common ports**

  ```
  nmap -F 192.168.1.1
  ```

* **Verbose fast scan**

  ```
  nmap -v -F 192.168.1.1
  ```



## **Sequential Port Scan**

* **Scan all ports in order instead of randomizing**

  ```
  nmap -r -p- 192.168.1.1-10
  ```

* **Use sequential order with fast scan**

  ```
  nmap -v -r -F example.com
  ```



## **Scan Top N Most Common Ports**

* **Scan top 100 ports**

  ```
  nmap --top-ports 100 192.168.1.1/24
  ```

* **Scan top 100 ports on a domain**

  ```
  nmap --top-ports 100 example.com
  ```