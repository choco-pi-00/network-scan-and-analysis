## 📌 What is Wireshark?

Wireshark is like a microscope for your network. 🎯  


It captures everything — every website visit, every file download, every DNS lookup — and shows you exactly what’s happening behind the scenes.


Whether you're:
- Diagnosing a slow connection,
- Hunting malware,
- Or learning how internet traffic flows...


Wireshark is one of the most powerful tools you can use.


## 🛠️ Tools & Requirements

- Wireshark (Pre-installed on Kali Linux or installable)


- Active network connection (wired or wireless)

## 🛡️ Security Use Cases

Wireshark helps in:

-Monitoring for malicious traffic (e.g., beaconing, port scans)

-Detecting plaintext passwords in HTTP, FTP, Telnet

-Capturing suspicious DNS queries (e.g., to shady domains)

-Inspecting malware callbacks or C2 channels

-Verifying firewall or IDS/IPS behavior

--- 
📘


Wireshark doesn't open or close ports — but it gives you eyes on the wire to see what's using them, who's knocking on them, and whether traffic is suspicious, expected, or misrouted. When used with tools like Nmap, it gives a full picture of who's doing what on which port — essential for any cybersecurity task.

---

## ✅ Step-by-Step: Capture Nmap Scan with Wireshark
1. Start Wireshark
        Launch Wireshark in Kali:
        
               sudo wireshark

2. Choose the Right Interface

  
        Look for the active one:
            Usually eth0, wlan0, or enp0s3
        
        Double-click it to start capture


3. Apply a Display Filter (Optional)


        Before or after capture, you can filter packets:
          ip.addr == 192.168.35.X
          or
          tcp.port == 135 || tcp.port == 139 || tcp.port == 445



4. Run Your Nmap Scan (While Capture Is Running)
   
        From terminal:
        ``nmap -sS <target-ip>``
        You’ll start seeing SYN packets, responses, resets, etc.
          

6. Stop Capture After Scan Completes
   
      Click the red square in Wireshark to stop.
      Then, Save the capture.


---

## 🔍 Filtering Packets (Display Filters)

Wireshark captures everything. Use display filters to focus on what matters:

- `host 192.168.1.1` → Only traffic to/from that IP
  
- `tcp port 80` → Only HTTP traffic
  
- `net 192.168.1.0/24` → Only traffic in your subnet
  
- `port not 22` → Exclude SSH traffic
  
- `tcp.port == 445` → Show all TCP traffic on port 445 (SMB)

- `udp.port == 53` → Show DNS traffic

- `tcp.flags.syn == 1 and tcp.flags.ack == 0` → SYN packets (used to initiate connection)

- `tcp.analysis.retransmission` → Look for retransmissions (possible port or network issues)

- `tcp.dstport == 22` → Who is trying to connect to SSH

- `tcp.srcport == 80` → Responses from a web server


---

## 🧠 Protocol Examples to Explore


| Protocol | Use Case                                  | What to Look For                                                           | Common Port(s) | Vulnerability Notes                                                                 |
|----------|--------------------------------------------|----------------------------------------------------------------------------|----------------|--------------------------------------------------------------------------------------|
| HTTP     | Web traffic                                | Requests, responses, URLs, cookies, headers                                | 80, 8080       | Can expose sensitive info, XSS/SQLi vulnerabilities, plaintext data               |
| HTTPS    | Secure web traffic                         | SSL/TLS handshake, encrypted payloads, cert details                        | 443, 8443      | Check for outdated/weak TLS versions, expired certs                              |
| DNS      | Domain name resolution                     | Hostname lookups, TXT records, suspicious queries                          | 53             | Can leak internal domains, used in data exfiltration (DNS tunneling)             |
| ARP      | Local network resolution                    | ARP requests/replies, duplicate IPs                                        | N/A            | ARP spoofing/poisoning, man-in-the-middle attacks                                |
| TCP      | General communication, connection setup     | 3-way handshake, retransmissions, SYN floods                               | Various        | Port scanning (SYN/FIN), DoS via connection exhaustion                          |
| UDP      | Lightweight, connectionless communication   | Quick exchanges, VoIP, DNS, SNMP                                           | Various        | Harder to trace, susceptible to spoofing and amplification DDoS                  |
| ICMP     | Ping, traceroute, error messaging           | Echo requests/replies, unreachable messages                                | N/A            | Network scanning, covert channels                                               |
| SMB      | Windows file sharing                        | File names, access logs, user auth attempts                                | 445            | SMBv1 vulnerable to EternalBlue, data exposure risks                             |
| FTP      | File transfer                              | Usernames/passwords, file uploads/downloads                                | 20, 21         | Plaintext credentials, anonymous login, command injection                       |
| SSH      | Secure remote access                       | Session initiation, key exchange, login attempts                           | 22             | Brute force risk, weak keys, misconfigured access                               |
| Telnet   | Remote login (legacy)                      | Login credentials, shell interactions (plaintext)                          | 23             | Fully insecure, deprecated                                                      |
| TLS/SSL  | Encrypted sessions                         | Cipher suite negotiation, certificate chain, alerts                        | Varies         | Weak cipher suites, downgrade attacks, expired certificates                     |
| NTP      | Time synchronization                       | Time requests, responses, timestamps                                       | 123            | Amplification attacks for DDoS                                                  |
| DHCP     | IP address assignment                      | Offers, requests, IP allocation data                                       | 67/68          | Rogue DHCP server attacks                                                       |


---

## 🔍 What This Wireshark Protocol Table Means


The table you generated is a quick reference guide that tells you:

| Column                  | What It Tells You                                                                                           |
| ----------------------- | ----------------------------------------------------------------------------------------------------------- |
| **Protocol**            | The type of network traffic you're analyzing (like HTTP for websites, DNS for domain lookups, etc.)         |
| **Use Case**            | What that protocol is generally used for (e.g., FTP is for file transfer, DNS for resolving domain names)   |
| **What to Look For**    | In Wireshark, these are the fields or activities you should pay attention to when analyzing this traffic    |
| **Common Port(s)**      | The network ports usually associated with the protocol (e.g., HTTP uses port 80)                            |
| **Vulnerability Notes** | Security risks or attacks commonly related to this protocol (e.g., DNS tunneling, FTP password leaks, etc.) |


---

## 🧠 Why This Is Useful in Wireshark

When you open Wireshark and see a lot of traffic flying around, you’ll often ask:

“Is this safe?”

“What is this protocol doing here?”

“Could this be an attack?”



##  Example from the Table

💡Let’s say you see a lot of FTP traffic (port 21) in Wireshark:

**Use Case**: File transfers

**What to Look For**: Is someone sending or receiving files? Look at USER and PASS commands.

**Vulnerability**: FTP sends passwords in plaintext, which anyone on the network can read 😱

**Action**: Raise a flag — someone might be sending sensitive info unencrypted!



💡**Another Example** — **DNS**
If you see DNS (port 53) traffic doing something odd (like constant lookups for strange domains):

It could be DNS tunneling — a way to sneak data out of a network (used by malware)

Or a misconfigured client leaking internal hostnames


---

## 🔍 Use Cases: Port-Level Analysis in Wireshark


| 🔧 What You Can Do                    | 🔍 How Wireshark Helps                                                                                                  |
| ------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| **Identify open ports**               | Look for SYN-ACK replies from destination ports (3-way TCP handshake) — it means the port is open.                      |
| **Detect port scanning**              | Multiple SYN requests from one IP to many ports (common behavior of Nmap scans).                                        |
| **Check what services run on a port** | Follow packets to a port (e.g., 80, 443, 445) and inspect payloads — e.g., HTTP requests on port 80.                    |
| **Monitor unauthorized services**     | Unexpected open ports in the packet stream (e.g., port 23 for Telnet) may indicate a misconfigured or backdoor service. |
| **Analyze protocols per port**        | Wireshark decodes protocols running over ports (e.g., DNS over port 53, SMB over 445).                                  |
| **Catch malware communication**       | Malicious software might communicate over unusual ports — Wireshark reveals these through traffic inspection.           |
| **Validate firewall rules**           | Check if traffic to blocked ports is being dropped or allowed accidentally.                                             |


---

## ✅ Why It's Useful in Cybersecurity

- Helps confirm real-time port usage

- Detects unauthorized access attempts

- Identifies malicious scans and port misuse

- Assists in troubleshooting service communication

- Gives evidence of exploitation or misconfiguration (e.g., EternalBlue via port 445)

---


🧠 *"Learning to analyze the traffic is like learning to speak the language of the network — Wireshark is your dictionary."*

--

**Author:** Me :)
