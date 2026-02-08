BEFORE YOU DO ANYTHING MAKE SURE TO SYNC YOUR CLOCK WITH THE DOMAIN CONTROLLER

## Initial (Uncredentialed) Enumeration

### Host Identification
1. Create a document for all discovered hosts (hostnames and IP addresses).

2. Start Wireshark and listen for Layer 2 (ARP, MDNS, NBNS) traffic to discover IP addresses and hostnames.

3. Start Responder in Analyze mode to discover IP addresses and hostnames.

4. Perform an Fping ICMP Sweep to find all hosts on your subnet that respond to an ICMP echo request. 

5. Perform an NMAP identification scan in case it picks up anything we missed.

6. With all of the discovered hosts, perform an NMAP scan to determine the services running on each host. 

7. Save to a file all ports/services running on each discovered host, but pay special attention to Domain Controllers, RDP, Naming conventions, etc.

8. Look for any quick wins that can give us an initial foothold, like outdated software or OS.

### User Identification
1. Create a document for all discovered users.

2. Attempt to abuse an SMB Null Session against the domain controller with a tool like rpcclient or enum4linux to grab all users

3. Attempt to abuse an anonymous ldap search against the domain controller with a tool like ldapsearch or windapsearch to grab all users

4. Try Impacket's lookupsid.py for discovering users with SID/RID brute forcing.

5. Use Kerbrute and common wordlists (that match the naming convention of the organization) to brute force usernames against a discovered DC.

6. Check for users that do not require Kerberos pre-auth (ASREP-ROASTING) to get their password hash.

7. Look for systems that can be exploited to gain SYSTEM level access. This is essentially like getting AD credentials.

### User Foothold
1. Start Responder/Inveigh on network interface to listen for NTLM users and hashes. Attempt to crack them with Hashcat/John

2. Attempt a password spray on users identified during user identification    

3. Attempt to gather the password policy of the organization first

4. Password spray against the discovered users using a common password like "Welcome1", "ChangeMe1", "SeasonYear (Spring2025)",username as the password, etc.
## Credentialed Enumeration / Exploitation

### Host Identification
1. Run Bloodhound with discovered credentials (mark anything we have controlled over as owned)
2. Use ldapdomaindump to identify all domain joined computers
3. Enumerate accessible shares on servers with NetExec, SMBMap, PowerView, or Snaffler
### User Identification
1. Run Bloodhound with discovered credentials (mark anything we have controlled over as owned)

2. Bloodhound.py

3. Bloodhound from Windows

4. Gather the domain password policy using the discovered credentials

5. Use the discovered credentials and a tool like NetExec to get all users, groups, and logged-on users against the server you have credentials for (ultimate goal is DC)

6. Gather a list of Domain Admins or Privileged users using the following tools:    
	- Windapsearch
	- PowerView
	- Bloodhound
	- AD powershell module

7. Credentialed Enumeration - from Linux

8. Credentialed Enumeration - from Windows

### Foothold Enumeration
1. Run Bloodhound with discovered credentials
	-  Bloodhound.py
	- Bloodhound from Windows	

2. Enumerate security controls in place (Defender, AppLocker, PowerShell Constraints, LAPS)

3. Look for other logged-on users using CME to see if we can dump any credentials with Mimikatz or Rubeus

4. Look for kerberoastable accounts through Bloodhound, PowerView, GetUserSPNs.py

5. Look at owned users for abusable ACL entries (ForceChangePassword, AddMember, GenericAll, etc.). Easiest to do in BloodHound.
	- ACL Enumeration

6. Check bloodhound for CanRDP, CanPSRemote, or SQLAdmin abilities to move laterally onto other machines.

### Pivoting
1. Run chisel for windows on the Windows pivot host
	- Connect with the linux chisel client on the attack host
	- Modify the proxychains.conf file to match the proxy that is established
	- Run commands with proxychains

2. Check bloodhound for CanRDP, CanPSRemote, or SQLAdmin abilities to move laterally onto other machines.  

### Exploitation
1. Look for kerberoastable accounts through Bloodhound, PowerView, GetUserSPNs.py

2. Grab all TGS tickets with GetUserSPNs.py (or Windows equivalent) and save to a file. Attempt to crack with Hashcat (on a GPU rig preferably)

3. Abuse any over permissive ACL entries to gain control of more users and move laterally throughout the network.    

4. ForceChangePassword to change a user's password to one we know

5. AddMember to add a member we control to a privileged group

6. GenericAll/GenericWrite to create an SPN for account and kerberoast their password hash (to potentially crack)

7. GenericAll to change the user's password

8. DS-Replication-Get-Changes-All to perform a DCSync attack

9. AddKeyCredentialLink to get user's NTLM Hash

10. ACL Abuse from Linux

11. Check bloodhound for CanRDP, CanPSRemote, or SQLAdmin abilities to move laterally onto other machines. Abuse these rights and look for sensitive info on the new machines

12. Check for common vulnerabilities to escalate privileges or move laterally:    

13. NoPac

14. PrintNightmare

15. PetitPotam

16. Check for common misconfigurations to escalate privileges or move laterally:    

17. Exchange group permissions

18. MS-RPRN Printer bug

19. MS14-068

20. Sniff for LDAP credentials

21. Enumerate DNS records for interesting servers

22. Look for user passwords and other notes in AD user descriptions

23. Check for PASSWD_NOTREQD Field on users and test for weak/no passwords

24. Look for credentials and other interesting files on SMB shares manually or with Snaffler

25. Check for DONT_REQ_PREAUTH field and ASREPRoasting any discovered users

26. Check for GPOs that we have write access over to gain administrator rights or move laterally (Can be checked with BloodHound)

27. Resource Based Constrained Delegation, Constrained Delegation, Unconstrained Delegation

28. Active Directory Certificate Services Attacks

29. Check for Group Policy Preferences (GPP) Passwords

30. Check for Active Directory Certificate Services (AD CS) attacks
    

### Additional Auditing
1. Create a snapshot of the AD database with AD Explorer for offline analysis

2. Use PingCastle to discover additional AD misconfigurations and vulnerabilities

3. Run Group3r to uncover vulnerabilities in AD Group Policy

4. Run ADRecon.ps1 to discover additional AD misconfigurations and vulnerabilities that we may have missed
## Attacking AD Trusts (Parent Domain)
1. Discover any current domain trusts with other domains using Get-ADTrust, Get-DomainTrust (PowerView), or Bloodhound

2. From a Windows or Linux machine with Domain Admin privileges, attempt an ExtraSIDs attack to create an Enterprise Admin user in the parent domain

3. Domain Trusts Overview
   
4. Child -> Parent Attacks - Windows

5. Child -> Parent Attacks - Linux    

## Attacking AD Trusts (Cross Forest)
1. Discover any current domain trusts with other domains using Get-ADTrust, Get-DomainTrust (PowerView), or Bloodhound

2. Attempt cross-forest kerberoasting

3. If admin accounts share names across domains, and one is compromised, try reused credentials.

4. Check for SIDHistory abuse

5. Cross-Forest Trust Abuse - Windows

6. Cross-Forest Trust Abuse - Linux

### Additional Noteworthy Methods
1. Access a host via RDP or WinRM as a local user or a local admin

2. Authenticate to a remote host as an admin using a tool such as PsExec

3. Gain access to a sensitive file share

4. Gain MSSQL access to a host as a DBA user, which can then be leveraged to escalate privileges