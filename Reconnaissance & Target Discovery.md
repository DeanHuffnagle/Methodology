Identify target scope (IP ranges, domains, subnets)
## Passive Recon
1. Gather public info (DNS, WHOIS, certificates, company structure)
2. Search public data leaks (Google Dorks, GitHub, Pastebin, etc.)

## Active Recon
1. Discover all active hosts on the target network/IP range/subnet(s). Document all active hosts to Obsidian notes
	- NMAP Host Discovery Scan
	- ICMP sweep (ping or fping)
	- TCP/UDP host discovery (nmap -sn, masscan)
	- ARP scanning (if on same subnet)
	- Add discovered hostnames to /etc/hosts file

2. For each active host, Scan ALL TCP / UDP ports. Document each open port for the respective host in Obsidian.
	- NMAP TCP port scanning
	- NMAP UDP port scanning

3. For each open port, run service version scan on discovered ports with scripts and OS detection.
	- NMAP service enumeration and OS detection
	- Netcat banner grabbing (manual confirmation)
	- Document services and service versions in Obsidian

4. For each detected service, do individual service enumeration to look for more information and vulnerabilities

5. Check for vulnerabilities in discovered services / service versions
	- Search metasploit for service exploits with the discovered version
	- Search ExploitDB for service exploits with the discovered version
	- Look at NMAP script output for discovered vulnerabilities or misconfigurations (ex: anonymous login)
	- Look for OS version exploits
	- Document discovered vulnerabilities in Obsidian
	- Search Google for "{Service} {Version} Exploit GitHub"

6. Check file share services (FTP, SMB, etc.) for anonymous logon and credential files
	- FTP Enumeration
	- SMB Enumeration

7. Check for write access over a share (SMB, FTP)
	- Capturing Hashes with Malicious .lnk File
	- Capture hashes with SCF on a File Share (no longer works on Windows Server 2019 or greater)

8. If webserver(s) exist, see Webserver Enumeration Methodology  
    

## Documentation and Reporting
Follow the Bruno Rocha Moura Method for documenting findings  

---
---

