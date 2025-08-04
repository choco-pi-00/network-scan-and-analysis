# 🔍 Port & Service Analysis

## 🧠 What Are Ports in Networking?
Think of ports as doors on your device. When data wants to come in or go out of your computer (like visiting a website or sending a message), it uses a specific port number to know which application or service should handle that data.

Every internet-connected device has 65,535 ports (numbered from 0 to 65535). These ports don’t physically exist — they’re logical communication channels managed by your operating system.

🛎 Example Analogy:
Let’s say you live in a big building (your computer). There are many rooms (apps/services) like:

📞 A phone room for calls (Port 5060 for SIP)

📧 A mailroom for emails (Port 25 for SMTP)

🌐 A lobby for visitors (Port 80 for HTTP websites)

🔐 A private office with a security guard (Port 22 for SSH)



When someone sends you a message, the message must go to the correct room (service) through the right door (port).

--- 

📬 Two Types of Ports:


TCP (Transmission Control Protocol) :  	Reliable and ensures delivery :  	Web, SSH, FTP


UDP (User Datagram Protocol) : 	Faster but less reliable :  	Streaming, DNS

--- 

🎯 Why Are Ports Important in Cybersecurity?


🕵️‍♂️ Hackers scan open ports to find weak services.

🔓 Misconfigured or forgotten ports can leak data or be exploited.

💡 Tools like Nmap let you check which ports are open and what they’re doing.



This document provides a manual analysis of the ports discovered during the Nmap scan. 
Understanding what services run on open ports helps evaluate the risk and identify potential vulnerabilities in your network.

---

## 🛠 Ports Identified in Scan

From our TCP SYN scan (using `nmap -sS`), we observed the following open ports:

| Port | Protocol | Service        | Description                                         |
|------|----------|----------------|-----------------------------------------------------|
| 135  | TCP      | msrpc          | Microsoft RPC - Remote communication between apps   |
| 139  | TCP      | netbios-ssn    | NetBIOS Session Service - Legacy file/printer share |
| 445  | TCP      | microsoft-ds   | SMB over TCP - File sharing and network logins      |

---
## 🔍 What This Port Table Means:
Think of your computer (or any device) as a building with many doors. Each port is like a door that allows certain types of communication to enter or leave the system.

This table tells you:

| Column                                    | What It Means                                                                                                            |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| **Port**                                  | The number assigned to a "door" on a device. Each port handles a specific service (like email, web, file sharing, etc.). |
| **Protocol**                              | The type of communication used — usually **TCP** (reliable) or **UDP** (faster but no guarantee).                        |
| **Service**                               | The name of the service usually running on that port — like **HTTP** for websites or **SSH** for remote login.           |
| **Description / Vulnerability Potential** | Describes if the port/service is risky, outdated, or commonly attacked — and why you should be careful with it.          |


🎓 Example:


Let’s take one row and explain it:

| **Port** | **Protocol** | **Service** | **Description / Vulnerability Potential** |
| -------- | ------------ | ----------- | ----------------------------------------- |
| 22       | TCP          | SSH         | Target for brute-force attacks            |

- Port 22 is like a door to your system.

- It uses TCP to ensure the messages are sent properly.

- The service running here is SSH — used to remotely log in to your computer.

- But it's also a common target for hackers trying to guess your password!


So, if you're not using SSH, you should close this door (block or disable it) to stay safe.


--- 
## 🔎 Port-by-Port Breakdown

### 🔹 **Port 135/tcp – msrpc**
- **Name**: Microsoft Remote Procedure Call (MSRPC)
- **Purpose**: Enables communication between applications/services across systems.
- **Risks**:
  - Often exploited by malware and attackers.
  - Can be used for remote code execution (e.g., DCOM).
- **Recommendation**:
  - Block externally via firewall.
  - Only allow internally if required by Windows services.

---

### 🔹 **Port 139/tcp – netbios-ssn**
- **Name**: NetBIOS Session Service
- **Purpose**: Legacy protocol used for file and printer sharing in Windows.
- **Risks**:
  - Can expose system name, user names, and more.
  - Can be used in LLMNR/NBT-NS poisoning attacks.
- **Recommendation**:
  - Disable NetBIOS over TCP/IP if not needed.
  - Use SMBv2/v3 and modern alternatives.

---

### 🔹 **Port 445/tcp – microsoft-ds**
- **Name**: Microsoft Directory Services (SMB)
- **Purpose**: Handles file sharing, network logins, and Active Directory communication.
- **Risks**:
  - Target for ransomware like WannaCry and EternalBlue.
  - Exposes file shares to the network.
- **Recommendation**:
  - Disable SMBv1 protocol.
  - Use firewalls to limit exposure.
  - Never expose port 445 to the internet.

---

## 🛡️ Security Tips Summary

| Port | Risk Level | Safe Practice                        |
|------|------------|--------------------------------------|
| 135  | ⚠️ High    | Block externally, allow internal use |
| 139  | ⚠️ High    | Disable unless legacy systems exist  |
| 445  | 🔥 Critical | Block internet access, disable SMBv1 |

---
## 🚨 Real-World Attack Examples

- **WannaCry Ransomware (Port 445)**: Exploited SMB vulnerability (EternalBlue).
- **Mirai Botnet (Ports 23/2323)**: Targeted insecure Telnet services on IoT devices.
- **DNS Poisoning (Port 53)**: Altered DNS responses to redirect traffic to malicious sites.
--- 
## 🧾 Extended Port Cheat Sheet

Here’s a wider list of common ports, what they’re used for, and why they matter for security analysis.


| Port       | Protocol | Service                     | Notes / Vulnerability Potential                          |
|------------|----------|-----------------------------|----------------------------------------------------------|
| 20         | TCP      | FTP (Data)                  | Unencrypted, old protocol                                |
| 21         | TCP      | FTP (Control)               | Often misconfigured (anonymous login)                    |
| 22         | TCP      | SSH                         | Target for brute-force attacks                           |
| 23         | TCP      | Telnet                      | Plaintext login, never use in production                 |
| 25         | TCP      | SMTP                        | Email sending; often abused for spam                     |
| 53         | TCP/UDP  | DNS                         | DNS poisoning, cache attacks                             |
| 67/68      | UDP      | DHCP                        | Dynamic IP allocation                                    |
| 69         | UDP      | TFTP                        | Insecure; used in embedded systems                       |
| 80         | TCP      | HTTP                        | Unencrypted traffic, XSS/SQLi vulnerabilities            |
| 110        | TCP      | POP3                        | Insecure email retrieval                                 |
| 123        | UDP      | NTP                         | Used in amplification DDoS attacks                       |
| 135        | TCP      | Microsoft RPC               | DCOM/WMI; vulnerable to old worms                        |
| 137-139    | TCP/UDP  | NetBIOS                     | Info leaks, file sharing vulnerabilities                 |
| 143        | TCP      | IMAP                        | Email fetching protocol                                  |
| 161/162    | UDP      | SNMP                        | Often exposed accidentally; info disclosure              |
| 389        | TCP/UDP  | LDAP                        | Used in AD; can be abused in privilege escalation        |
| 443        | TCP      | HTTPS                       | Encrypted; SSL/TLS attacks possible                      |
| 445        | TCP      | SMB (Microsoft-DS)          | EternalBlue, WannaCry — critical attack vector           |
| 465/587    | TCP      | SMTP over SSL/TLS           | Secure email sending                                     |
| 514        | UDP      | Syslog                      | Can be poisoned for log injection                        |
| 631        | TCP      | IPP                         | May expose printer interfaces publicly                   |
| 993        | TCP      | IMAPS                       | Secure IMAP                                              |
| 995        | TCP      | POP3S                       | Secure POP3                                              |
| 1433       | TCP      | Microsoft SQL Server        | Brute-force and SQLi target                              |
| 1521       | TCP      | Oracle DB                   | Oracle connections; attack vector                        |
| 1723       | TCP      | PPTP                        | Insecure VPN protocol                                    |
| 3306       | TCP      | MySQL                       | Common SQL injection/brute-force target                  |
| 3389       | TCP      | RDP                         | Remote Desktop — ransomware and brute-force risk         |
| 5432       | TCP      | PostgreSQL                  | Database exposure risk                                   |
| 5900       | TCP      | VNC                         | Remote desktop — often no password                       |
| 5985/5986  | TCP      | WinRM                       | PowerShell-based remote access (use with care)           |
| 6379       | TCP      | Redis                       | Often exposed with no auth                               |
| 8080       | TCP      | HTTP (Alternate)            | Dev environments, proxy targets                          |
| 8443       | TCP      | HTTPS (Alternate)           | Secure web traffic on non-standard ports                 |
| 9000       | TCP      | PHP-FPM                     | Exploitable in misconfigured setups                      |


---

**Author**: Me :)  
*🧠 Compiled with curiosity, 📡 scanned using Nmap, and 🔐 focused on security.*
