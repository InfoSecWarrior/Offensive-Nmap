**Firewall, IDS, and IPS Evasion & Spoofing in Nmap**
Nmap provides powerful techniques to bypass firewalls, intrusion detection systems (IDS), and intrusion prevention systems (IPS) during reconnaissance. These include stealth scanning, IP spoofing, decoy scanning, packet fragmentation, and more.



### **Fragment Packets**

* Breaks probe packets into small IP fragments, making it harder for firewalls and IDS to inspect and block the scan.

  ```
  nmap -v -Pn -f 192.168.1.208
  ```

### **Double Packet Fragmentation**

* Further increases fragmentation level by using the `-f` flag twice.

  ```
  nmap -v -Pn -f -f 192.168.1.208
  ```

###  **Advanced Fragmentation with Custom MTU**

* Controls the packet size to fine-tune fragmentation, helping evade detection.

  ```
  nmap --mtu 16 192.168.1.208
  ```
  ```
  nmap -v -Pn --mtu 8 -p- 192.168.1.208
  ```
  ```
  nmap -v -Pn --mtu 16 -p- 192.168.1.208
  ```
  ```
  nmap -v -Pn --mtu 8 192.168.1.208 -p 80
  ```
  ```
  nmap -v -Pn --mtu 16 192.168.1.208 -p 80
  ```

###  **Decoy Scanning**

* Launches scans using decoy IPs, obscuring the real source and confusing IDS and logging systems.

  ```
  nmap -D 192.168.1.10,192.168.1.20,ME,192.168.1.30,192.168.1.208
  ```

**Without ME**

* Your IP is not mixed among the decoys.

  ```
  nmap -D 192.168.1.10,192.168.1.20,192.168.1.30,192.168.1.208 192.168.1.1
  ```

**With ME**

* Includes your real IP in the decoy list, making detection harder.

  ```
  nmap -D 192.168.1.10,192.168.1.20,ME,192.168.1.30,192.168.1.208 192.168.1.1
  ```



###  **Spoof Source IP**

* Pretends the scan is coming from another IP.

  ```
  nmap -S 192.168.1.100 192.168.1.208
  ```

###  **Combine with Stealth Scan**

```
nmap -S 192.168.1.100 -sS 192.168.1.208
```
```
nmap -Pn -e eth0 -S 192.168.1.100 192.168.1.208
```
```
nmap -v -Pn -p- -e eth0 -S 192.168.1.100 192.168.1.208
```



###  **Spoof MAC Address**

* Modifies the MAC address to impersonate another device or bypass MAC-based filters.

  ```
  nmap --spoof-mac 00:11:22:33:44:55 192.168.1.208
  ```
  ```
  nmap --spoof-mac Apple 192.168.1.208
  ```
  ```
  nmap -Pn --spoof-mac Cisco 192.168.1.208
  ```
  ```
  nmap -Pn --spoof-mac 0 192.168.1.208
  ```



###  **Specify Source Port**

* Uses a specific source port to appear as legitimate traffic.

  ```
  nmap -g 53 192.168.1.208
  ```
  ```
  nmap -Pn -g 80 192.168.1.208
  ```



###  **Use Proxies**

* Routes scans through HTTP/SOCKS proxies to hide your real IP.

  ```
  nmap --proxies http://proxy.example.com:8080 192.168.1.208
  ```
  ```
  nmap -Pn --proxies http://proxy1:8080,http://proxy2:8080 192.168.1.208
  ```



### **Idle Scan**

* Uses a zombie host to relay scans, hiding your identity.

  ```
  nmap -sI 192.168.1.50 192.168.1.208
  ```
  ```
  nmap -Pn -sI 192.168.1.50 192.168.1.208
  ```



### **Randomize Scan Order**

* Prevents detection from scanning IPs in sequence.

  ```
  nmap -Pn --randomize-hosts 192.168.1.208
  ```



### **Cloak Nmap Signature**

* Adds random bytes to packets to bypass signature-based IDS.

  ```
  nmap -Pn --data-length 50 192.168.1.208
  ```



### **Avoid Ping**

* Skips host discovery to avoid ICMP filtering.

  ```
  nmap -Pn 192.168.1.208
  ```



###  **Custom User-Agent String**

* Modifies HTTP requests to appear as legitimate browser traffic.

  ```
  nmap --script http-useragent --script-args http.useragent="Mozilla/5.0" example.com
  ```



### **Custom Data in Probes**

* Sends raw or crafted data to simulate real service requests or confuse IDS.

  ```
  nmap -v -Pn 192.168.1.208 -p 80 --data 4142434445
  ```
  ```
  nmap --data "\x50\x49\x4e\x47" -p 80 192.168.1.10
  ```
  ```
  nmap --data "GET / HTTP/1.1\r\nHost: example.com\r\n\r\n" -p 80 example.com
  ```



###  **Custom String Payloads**

* Sends readable string data instead of hex.

  ```
  nmap -v -Pn 192.168.1.40 -p 80 --data-string test
  ```
  ```
  nmap -p 80 --data-string "GET / HTTP/1.1\r\nHost: example.com\r\n\r\n" example.com
  ```



###  **Packet Padding**

* Adds extra data to packets for evasion or signature alteration.

  ```
  nmap -v -Pn 192.168.1.40 -p 80 --data-length 128
  ```



### **Set Time-To-Live (TTL)**

* Adjusts TTL to emulate different OS or bypass TTL-based detection.

  ```
  nmap --ttl 64 -p 80 192.168.1.10
  ```
  ```
  nmap --ttl 50 -p 80 192.168.1.10
  ```



### **Customize IP Header Options**

* Adds custom IP header options to test firewall behavior or evade detection.

  ```
  nmap --ip-options "R" -p 80 192.168.1.10
  ```
  ```
  nmap --ip-options "\x83\x03\x10\x10\x00" 192.168.1.20
  ```
  ```
  nmap --ip-options "L192.168.1.1" -p 22 192.168.1.20
  ```
  ```
  nmap --ip-options "S192.168.1.1,192.168.1.2" -p 443 192.168.1.30
  ```