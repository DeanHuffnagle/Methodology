## If Webserver Exists

1. Check web configuration files and source code for vulnerabilities, hard coded credentials, etc.  
	- Enumeration: Credential Hunting
	- Check the source code of all pages, including index
	- Potential Locations
	- C:\xampp\htdocs (common with Apache)
	- C:\inetpub

2. Run Windows Priv Esc Automation Scripts. Save the output to a file and transfer it to the attack box in the appropriate CTF f older. Examine in text editor for easier readability.
	- winPEAS
	- Things to check
	- Seatbelt
	- PowerUp / SharpUp
	- JAWS - Recommended by Ippsec. Uses PowerShell. Could be worth trying if custom executables are blocked.
	- SessionGopher
	- Full List
	- Bloodhound

3. Perform basic box enumeration once a foothold is established
	- Gathering Network Information
	- Gathering System Information
	- Gathering Process Enumeration
	- User and Group Enumeration

4. Look at access rights of user we have a foothold with (user privileges)
	- Windows Privileges Overview
		-   `whoami /all`
		- `whoami /priv`
	- SeImpersonate / SeAssignPrimaryToken (JuicyPotato / RoguePotato / PrintSpoofer)
	- SeDebugPrivilege
	- SeTakeOwnershipPrivilege

5. Check if user is a member of privileged groups
	- `whoami /groups`
	- Backup Operators
	- Event Log Readers
	- DnsAdmins
	- Hyper-V Admins
	- Print Operators
	- Server Operators
	- WSUS Administrators

6. Check for Weak File / Service Permissions
	- Permissive File System ACLs
	- Weak Service Permissions
	- Unquoted Service Path
	- Permissive Registry ACLs
	- Modifiable Registry Autorun Binary

7. Check for saved credentials
	-  Cmdkey Saved Credentials
	- `cmdkey /list`
	- If we get access this way, run mimikatz to try to extract their plaintext password

8. Look for services running on internal ports that were not accessible from the outside with netstat
	- Databases we can connect to?
	- netstat -ano
	- Vulnerable Services
 
9. Check for additional NICs using commands like ipconfig    

10. Look for vulnerable applications and services
	- wmic product get name  
	- Vulnerable Services

11. Look for interesting files on the server that may have credentials or other sensitive info
	- Credential Hunting
	- Credential Hunting Other Files
	- Further Credential Theft
	- Dumping Hashes / Credentials
	- Mimikatz

12. Pillage for credentials or other interesting information
	- Pillaging Overview
	- Pillaging Applications
	- Accessing Instant Messaging Clients Through Cookies
	- Pillaging the Clipboard + Keylogging
	- Roles and Services (pillaging backups)

13. Look for scheduled tasks that we can modify
	- Scheduled Tasks

14. Look for credentials in process command line
	- Process Command Line

15. Check for 'always install elevated' setting and exploit with MSI package
	- Always Install Elevated

16. Capture hashes with a Malicious LNK file or SCF file
	- ESPECIALLY USEFUL IF WE THINK USERS WILL VISIT A SHARE THAT WE HAVE UPLOADED TO
	- Capturing Hashes with Malicious .lnk File
	- Capture hashes with SCF on a File Share (no longer works on Windows Server 2019 or greater)

17. Look for Kernel / OS exploits
	- Kernel Exploits
		-  Zero logon
		-  PrintNightmare
	- EOL Systems and their exploits (Windows Server 2008, Windows 7, etc.)

18. Attempt to bypass UAC Controls if present
	- User Account Control (UAC) Attacks

19. DLL Injection
	- DLL Injection
  
20. Common Vulnerabilities and Vulnerable Programs (Talked about in the CPTS)  
	- Docker Desktop Community Edition before 2.1.0.1 (CVE-2019-15752)
	- Windows Certificate Dialog (CVE-2019-1388)

21. Attempt to Capture Network Traffic with Inveigh/Responder, Wireshark, or Snaffler
	- Traffic Capture
	- Snaffler - SMB Share Enumeration Tool
	- LLMNR/NBT-NS Poisoning From Windows
	- LLMNR/NBT-NS Poisoning From Linux

22. Enumerate User/Computer Description Fields for cleartext credentials or other useful information
	- User/Computer Description Field

23. If the system has .vhd, .vhdx, and .vmdk files, we can mount them to potentially dump the machine hashes
	- Mount VHDX/VMDK

24. Check for Active Directory Certificate Services (AD CS) attacks
    

If trying to be evasive or if custom executables are locked down, check out LOLBAS for alternative methods
	- Living Off The Land Binaries and Scripts (LOLBAS)
## Once we are Admin

1. Attempt to recover all of the user passwords or NTLM hashes on the system
	- Dump the LSASS process to try and recover more user passwords or NTLM hashes
	- Stealing NTLM Hashes from an LSASS.DMP Memory Dump
	- Dumping the SAM Database to recover password hashes  

---
---
