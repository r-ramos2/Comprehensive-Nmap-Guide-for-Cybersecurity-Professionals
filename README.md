# **Comprehensive Nmap Guide for Cybersecurity Professionals**

## **Table of Contents**
1. [Introduction](#introduction)
   - [What is Nmap?](#what-is-nmap)
   - [Importance of Nmap in Cybersecurity](#importance-of-nmap-in-cybersecurity)
   - [Disclaimer](#disclaimer)
2. [Nmap Installation](#nmap-installation)
   - [Installation on Windows](#installation-on-windows)
3. [Basic Nmap Commands](#basic-nmap-commands)
   - [Scanning a Public Server](#scanning-a-public-server)
   - [Scanning a Range of IP Addresses](#scanning-a-range-of-ip-addresses)
   - [Random IP Scanning](#random-ip-scanning)
   - [Saving Scan Results](#saving-scan-results)
4. [Advanced Nmap Scanning Techniques](#advanced-nmap-scanning-techniques)
   - [TCP Connect and SYN Scans](#tcp-connect-and-syn-scans)
   - [OS and Service Version Detection](#os-and-service-version-detection)
   - [Host Discovery with Ping Sweep](#host-discovery-with-ping-sweep)
5. [Enumeration Techniques](#enumeration-techniques)
   - [FTP, DNS, and SMB Enumeration](#ftp-dns-and-smb-enumeration)
   - [Using nslookup for Domain Name and IP Resolution](#using-nslookup-for-domain-name-and-ip-resolution)
6. [Firewall and IDS/IPS Evasion Techniques](#firewall-and-idsips-evasion-techniques)
7. [Vulnerability Scanning with Nmap](#vulnerability-scanning-with-nmap)
   - [Vulners NSE Script](#vulners-nse-script)
   - [Nmap Scripting Engine (NSE)](#nmap-scripting-engine-nse)
8. [Using Wireshark for Nmap Traffic Analysis](#using-wireshark-for-nmap-traffic-analysis)
9. [Ethical and Legal Considerations](#ethical-and-legal-considerations)
10. [Conclusion](#conclusion)
11. [References](#references)

---

## **Introduction**

### **What is Nmap?**
Nmap (Network Mapper) is a powerful, open-source tool used for network discovery and security auditing. It helps cybersecurity professionals map networks, identify open ports, and gather detailed information about the systems and services running on target machines.

### **Importance of Nmap in Cybersecurity**
- **Network Discovery**: Quickly identifies hosts on a network, which is crucial for asset management and security analysis.
- **Vulnerability Assessment**: Nmap can detect security vulnerabilities using its scripting engine (NSE).
- **Penetration Testing**: Widely used in security audits to uncover weaknesses.
- **Compliance Audits**: Helps organizations meet regulatory requirements by ensuring network security configurations.

> **Disclaimer**: Always ensure you have permission before scanning any network or device. Unauthorized scanning is illegal and against ethical standards.

---

## **Nmap Installation**

### **Installation on Windows**
1. Visit the [Nmap website](https://nmap.org/download.html) and download the appropriate installer for Windows.
2. After installation, open the terminal and verify the installation by entering:
   ```bash
   nmap --help
   ```
3. This command lists available options and confirms that Nmap is installed correctly.

---

## **Basic Nmap Commands**

### **Scanning a Public Server**
To scan a public server such as `scanme.nmap.org`, enter:
```bash
nmap -v -A scanme.nmap.org
```
- **Explanation**: The `-A` option enables advanced scanning, including OS and version detection.

### **Scanning a Range of IP Addresses**
To scan a range of IP addresses:
```bash
nmap -v -sn [start_IP_range]/[end_IP_range]
```
- **Explanation**: This performs a ping scan without scanning ports, identifying live hosts in the range.

### **Random IP Scanning**
To scan random IP addresses for open ports:
```bash
nmap -v -iR 10000 -Pn -p 80
```
- **Explanation**: Scans 10,000 random IP addresses and targets port 80 without performing ping scans.

### **Saving Scan Results**
To save the results of your scan into a file:
```bash
nmap -oG [your_IP_address] -vv > Desktop/results.txt
```
- **Explanation**: This saves the scan results in a greppable format, useful for future analysis.

---

## **Advanced Nmap Scanning Techniques**

### **TCP Connect and SYN Scans**
- **TCP Connect Scan**: Establishes a full connection (slower, but more reliable):
  ```bash
  nmap -sT [target_IP] -p 21-8080
  ```
- **SYN (Stealth) Scan**: Faster, doesn’t complete the TCP handshake, so it’s less likely to be detected:
  ```bash
  nmap -sS [target_IP]
  ```
- **Comprehensive Service Scan**: To detect service versions:
  ```bash
  nmap -sS -sV [target_IP] -p 21-8080
  ```

### **OS and Service Version Detection**
- Detect OS and services:
  ```bash
  nmap -O [device_IP_address]
  ```

- For a more in-depth scan, use the `-A` flag:
  ```bash
  nmap -A [device_IP_address] -p-
  ```

### **Host Discovery with Ping Sweep**
To scan your own network and discover active hosts:
```bash
nmap -sn [your_IP_address/subnet]
```
- **Explanation**: This identifies live devices within your network without scanning ports.

---

## **Enumeration Techniques**

### **FTP, DNS, and SMB Enumeration**

#### **FTP Enumeration**
- Check for FTP vulnerabilities:
  ```bash
  sudo nmap -p 21 --script ftp-anon,ftp-vsftpd-backdoor [device_IP]
  ```

#### **DNS Enumeration**
- Perform a DNS brute-force scan:
  ```bash
  sudo nmap --script dns-brute [target_domain]
  ```

- Conduct a DNS zone transfer (with permission):
  ```bash
  sudo nmap --script dns-zone-transfer --script-args 'dns-zone-transfer.domain=zonetransfer.me'
  ```

#### **SMB Enumeration**
- Identify SMB vulnerabilities:
  ```bash
  sudo nmap -p 445 --script smb-vuln-ms17-010 [device_IP]
  ```

### **Using nslookup for Domain Name and IP Resolution**
- To get the IP address of a domain:
  ```bash
  nslookup scanne.nmap.org
  ```
  
- To perform a reverse lookup and resolve an IP address to a domain:
  ```bash
  nslookup [IP_address]
  ```

---

## **Firewall and IDS/IPS Evasion Techniques**
Use evasion techniques to avoid detection by firewalls or intrusion detection systems. For example:
```bash
nmap -D RND:10 [target_IP]
```
This sends decoy packets, making it harder for a firewall to detect the actual scanning source.

---

## **Vulnerability Scanning with Nmap**

### **Vulners NSE Script**
To scan for vulnerabilities using the Vulners script:
1. Download the script from [GitHub](https://github.com/vulnersCom/nmap-vulners).
2. Run the vulnerability scan:
   ```bash
   nmap -sV --script vulners [target_IP]
   ```
This will provide CVE information and vulnerability ratings.

### **Nmap Scripting Engine (NSE)**
The Nmap Scripting Engine allows for extended scanning capabilities, including vulnerability detection, backdoor discovery, and exploitation.
- To list all available scripts:
  ```bash
  ls -al /usr/share/nmap/scripts/
  ```

- Example of using NSE for HTTP enumeration:
  ```bash
  nmap --script http-enum [target_IP]
  ```

---

## **Using Wireshark for Nmap Traffic Analysis**
Use Wireshark to analyze the packets Nmap sends during scans:
1. Run a SYN scan:
   ```bash
   nmap -sS [your_IP_address] -p 21
   ```
2. In Wireshark, filter traffic by the SYN flag to monitor the behavior.

For stealth scans, Wireshark will display the RST flag, indicating that the TCP connection wasn't completed.

---

## **Ethical and Legal Considerations**
- **Permission**: Always obtain permission before scanning any network you don’t own.
- **Compliance**: Adhere to legal regulations such as **GDPR** and **HIPAA** when performing scans on sensitive networks.

---

## **Conclusion**
This guide provides a comprehensive overview of Nmap’s capabilities, from basic scans to advanced vulnerability detection. With Nmap, cybersecurity professionals can efficiently perform network assessments, identify vulnerabilities, and conduct penetration tests in a lawful and ethical manner.

---

## **References**
1. [Nmap Official Documentation](https://nmap.org)
2. [Wireshark User Guide](https://www.wireshark.org/docs/)
3. [Vulners Nmap Script](https://github.com/vulnersCom/nmap-vulners)
4. [SecLists on GitHub](https://github.com/danielmiessler/SecLists)
