### **Which Scan Type Does Nmap Use by Default?**

By default, **Nmap chooses the scan type based on user privileges**:

**If you run Nmap as a normal user (without root/sudo):**

- Nmap performs a **TCP Connect Scan (`sT`)**
- This is because normal users **cannot** create raw packets required for a stealth scan.

**If you run Nmap as root (with sudo):**

- Nmap performs a **TCP SYN Scan (`sS`)** (Stealth Scan)
- This is because root privileges allow crafting raw packets, which enables a faster and less detectable scan.

---

### **Understanding the Default Scan Types**

**TCP Connect Scan (`-sT`)** → Used when **not running as root**

- Uses the **full** three-way handshake (SYN → SYN/ACK → ACK).
- Slower and more detectable because it fully establishes a connection.
- Logged in target system logs (e.g., firewall and IDS logs).

**TCP SYN Scan (`-sS`)** → Used when **running as root**

- Sends only the **SYN** packet and waits for a response.
- If SYN/ACK is received, it **does not complete the handshake** (stealthier).
- Faster and harder to detect in logs because connections remain half-open.

---
