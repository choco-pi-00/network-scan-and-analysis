# network-scan-and-analysis
This project performs a comprehensive scan of a local network using **Nmap** and analyzes the resulting packet traffic with **Wireshark**. 
It is part of a cybersecurity learning task designed to understand port visibility, service identification, and packet-level behavior of active scanning.

🎯 Objective

- Discover devices and open ports in a local subnet
- Identify common and potentially vulnerable services
- Capture and analyze packet-level behavior using Wireshark
- Document findings and suggest basic security measures

🛠️ Tools Used

- **Nmap** – for network and port scanning
- **Wireshark** – for live packet capture and protocol analysis 
- **Kali Linux** – as the operating system for running tools

📁 Project Structure

network-scan-and-analysis/
├── results/
│   └── redacted_scan.txt              # Cleaned Nmap output
├── analysis/
│   ├── scanning_with_nmap.md          # Using Nmap 
│   └── port_analysis.md               # Manual service analysis
│   └── wireshark_analysis.md          # Packet behavior explanation
├── screenshots/
│   └── nmap_scan_capture.pcapng       # Saved Wireshark capture (optional)
├── scripts/
│   └── scan.sh                        # Bash script to run Nmap scan 
└── README.md


Author - Me :) 
Made with 🔍, 🧠, and Nmap.
