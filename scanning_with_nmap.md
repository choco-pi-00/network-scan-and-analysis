

## 🕵️‍♀️ What is Nmap?

Imagine you're playing hide and seek 👀 in your neighborhood. You want to know:

🏠 Who's home?

🚪 Which doors are open?

🧱 Which ones are locked?

🧠 What kind of rooms are behind those doors?

Nmap is like a magic flashlight 🔦 that lets you look at all the houses (computers or devices) on a street (network) and tells you:
"This house is awake!"
"This door (port) is open!"
"Behind this door is a kitchen (a service like web server or file sharing)!"

It helps cybersecurity folks find vulnerabilities so they can fix them — kind of like checking if any door is open by mistake.



*This document outlines the process of performing a basic TCP SYN scan of the local network using Nmap. 
It serves as a step-by-step guide for identifying open ports, understanding the services running, and interpreting Nmap output.*



## 🛠 Tools Used

- **Nmap** – Open-source network scanner
- **Kali Linux** – Linux distro used to run Nmap



## ✅ Pre-requisites

  ⭐Check if nmap is installed in kali.
  
       **Bash**
            nmap --version
            
       **Output(Something like this, if nmap already installed)**
            Nmap version 7.94SVN ( https://nmap.org )
            Platform: x86_64-pc-linux-gnu
            Compiled with: liblua-5.4.6 openssl-3.4.0 libssh2-1.11.0 libz-1.3.1 libpcre2-10.42 libpcap-1.10.4 nmap-libdnet-1.12 ipv6
            Compiled without:
            Available nsock engines: epoll poll select
  
  Otherwise Install nmap via:
  
      **Bash** 
          sudo apt update
          sudo apt install nmap
  
  ⭐Find the Local IP address 
  
      **Bash** 
          ip a




## 🔍 Scanning with nmap 

     **Bash**
          sudo nmap -sS <target-ip-address>
     **Output**
          PORT      STATE    SERVICE
          135/tcp   open      msrpc
          139/tcp   open      netbios-ssn
          445/tcp   open      microsoft-ds

(full output in results/redacted_scan.txt file)


## 🕵️‍♀️Analysis

   - **sudo**: stands for "SuperUser DO", 
        It lets you run a command as a superuser (administrator) on your system
        It's like saying: “Hey system, I have permission to do something powerful or sensitive
   - **-sS**: This is used to do TCP SYN scan (also called a "half-open" or stealth scan), which checks which ports are open without completing a full connection — it's faster and less likely to be detected.    
   - **PORT**: The network port number and protocol (tcp).
   - **STATE**: Whether the port is open (accepting connections).
   - **SERVICE**: What kind of service usually runs on that port.



## 🔍 Detailed Port Analysis


🔹 **Port 135/tcp – msrpc**


Name: Microsoft RPC (Remote Procedure Call)
Used For: Letting software on different computers talk to each other.
Security Note: This is often targeted by malware or attackers. If unused, block or restrict access.


🔹 **Port 139/tcp – netbios-ssn**


Name: NetBIOS Session Service
Used For: Sharing files and printers in Windows networks.
Security Note: Very old protocol. Can leak system info or be abused. Best to disable on modern networks.


🔹 **Port 445/tcp – microsoft-ds**


Name: Microsoft Directory Services (SMB over TCP)
Used For: File sharing, Windows network logins.
Security Note: Often used in ransomware attacks (like WannaCry). Exposing this port to the internet is a big risk.




## 🛡️ Suggested Actions


| Port | If You're Not Using It | What to Do                     |
| ---- | ---------------------- | ------------------------------ |
| 135  | Not needed remotely    | Block it in firewall           |
| 139  | Legacy protocol        | Disable NetBIOS                |
| 445  | File sharing only      | Restrict access or disable SMB |




## 🔧 How to Block or Disable Ports on Windows


🔹 **Option 1: Block in Windows Firewall**

✅ Steps:

1. Open Start Menu → Search “Windows Defender Firewall”
2. Click “Advanced Settings” (left panel)
3. Click Inbound Rules → New Rule
4. Choose Port → Next
5. Select TCP
6. Type: 135, 139, 445
7. Choose Block the connection → Next
8. Apply to all profiles (Domain, Private, Public) → Next
9. Name it something like: Block SMB & RPC Ports
10. Click Finish


🎯 Done! These ports are now blocked from accepting incoming connections.

---


 ## 📘 Nmap Command Cheatsheet 

| 🧪 Task / Scan Type              | 💻 Command                                            | 📋 Purpose                                                                 |
|----------------------------------|------------------------------------------------------|----------------------------------------------------------------------------|
| Ping Scan (Live Hosts)           | `nmap -sn <target>`                                  | Find active devices on the network (host discovery only)                  |
| Basic Port Scan                  | `nmap <target>`                                      | Scan top 1000 TCP ports                                                   |
| Stealth SYN Scan (Root)          | `sudo nmap -sS <target>`                             | Fast & quiet half-open TCP scan                                           |
| Full TCP Connect Scan            | `nmap -sT <target>`                                  | Full TCP connection (used if not root)                                    |
| UDP Scan                         | `sudo nmap -sU <target>`                             | Scan open UDP ports (e.g., DNS, SNMP)                                     |
| Aggressive Scan                  | `nmap -A <target>`                                   | OS detection, version detection, script scan, traceroute                  |
| OS Detection                     | `sudo nmap -O <target>`                              | Attempt to determine target OS                                            |
| Service Version Detection        | `nmap -sV <target>`                                  | Detect services and their versions                                        |
| Vulnerability Detection          | `nmap --script vuln <target>`                        | Use NSE scripts to check for known vulnerabilities                        |
| Scan Specific Ports              | `nmap -p 22,80,443 <target>`                         | Scan only specific ports                                                  |
| Scan All 65535 TCP Ports         | `nmap -p- <target>`                                  | Scan all ports (0-65535)                                                  |
| Fast Scan (Top 100 Ports)        | `nmap -F <target>`                                   | Quick scan of top 100 most common ports                                   |
| Scan IP Range                    | `nmap 192.168.1.1-50`                                | Scan IPs from `.1` to `.50`                                               |
| Scan Subnet                      | `nmap 192.168.1.0/24`                                | Scan entire subnet                                                        |
| Only Show Open Ports             | `nmap --open <target>`                               | Hide closed/filtered ports from output                                    |
| Save Output to Text File         | `nmap -oN result.txt <target>`                       | Human-readable output saved to file                                       |
| Save in All Formats              | `nmap -oA scan_result <target>`                      | Save output in .nmap, .gnmap, and .xml formats                            |
| DNS Brute Force (Zone Transfer)  | `nmap --script dns-zone-transfer <target>`           | Attempt DNS zone transfer                                                 |
| HTTP Service Script Scan         | `nmap -p 80 --script "http-*" <target>`              | Run all HTTP-related NSE scripts on port 80                               |
| SSH Brute-force (⚠️ Legal Test Only) | `nmap -p 22 --script ssh-brute <target>`        | Brute-force SSH login (authorized testing only)                           |
| Firewall/Packet Filter Detection | `nmap -sA <target>`                                  | ACK scan to determine if firewall is blocking ports                       |
| Traceroute to Host               | `nmap --traceroute <target>`                         | Show route packets take to reach host                                     |
| Scan with Spoofed MAC Address    | `nmap --spoof-mac 00:11:22:33:44:55 <target>`        | Send scan using a fake MAC address                                        |
| Decoy Scan (Hide Source IP)      | `nmap -D RND:10 <target>`                            | Mask your real IP by adding decoy IPs                                     |
| Use Timing Option                | `nmap -T4 <target>`                                  | Increase scan speed (T0 = slowest, T5 = fastest)                          |
| Scan via Specific Interface      | `nmap -e eth0 <target>`                              | Use a specific network interface                                          |
| Send Fragmented Packets          | `nmap -f <target>`                                   | Send fragmented packets (bypass basic IDS)                                |
| Heartbleed Vulnerability Check   | `nmap -p 443 --script ssl-heartbleed <target>`       | Check for OpenSSL Heartbleed bug on HTTPS                                 |


🧠 Summary or Learnings
---
**Author:** Me :)  
*Made with 🔍, 🔐, and curiosity.*

