## If Webserver Exists

1. Check web configuration files and source code for vulnerabilities, hard coded credentials, etc.
	- Enumeration: Credential Hunting
	- Check the source code of all pages, including index
	- For Apache (httpd):
		- /var/www/html — default root for Apache on most distros (e.g., Ubuntu, Debian).
		- /var/www/ — base directory for multiple sites or vhosts.
		- /srv/http/ — default on Arch Linux.
		- /usr/share/httpd/ — sometimes used on RHEL/CentOS.
		- /etc/apache2/sites-available/000-default.conf
	- For Nginx:
		- /usr/share/nginx/html — default on CentOS/RHEL.
		- /var/www/html — common if configured similarly to Apache.
		- /etc/nginx/sites-available/ and /etc/nginx/sites-enabled/ — config files, not content.
	- Other possible locations:
		- /opt/web/ — custom installations.
		- /home/user/public_html/ — sometimes used for user web directories.
		- ~/www/ or ~/html/ — user-specific web roots.
## Default Methodology

1. Run Linux Priv Esc Automation Scripts. Save output to a file and transfer it to Kali Box to examine in a text editor.
	- LinPeas
	- LinEnum

2. Perform basic box enumeration once a foothold is established
	- First user checks / network checks
	- Checking Sudo Privileges
	- Check OS/Kernel Version
	- Check $PATH variable
	- List Processes running as root
	- Discover other users on the machine
	- Check for SSH keys
	- Check current user bash history
	- Check if Shadow is readable or Passwd is writable
		- Additionally check for hashes in /etc/passwd
	- Enumerate Existing Groups
	- Check for running Cron Jobs
	- Check for Unmounted File Systems and Additional Drives
		- Weak NFS Privileges
	- Find Writable Directories and Files
	- Enumerate All Hidden Files and directories
	- Enumerate Services and Versions and binaries
	- Check for accessible configuration files
	- Check for accessible scripts

3. Enumerate for hardcoded / cleartext credentials on the system (quickly check config files)  
	- BE ESPECIALLY ATTENTIVE TO OUT OF THE ORDINARY SERVICES
	- Look for things in /opt
	- Look for web config files
	- Enumeration: Credential Hunting

4. Look at access rights of the user we gained a foothold with (Sudo/SUID/GUID)
	- Checking for Sudo privileges
	- Checking SUID/GUID Privileges
	- Check these rights in GTFOBins

5. Check for unique files owned by the user or by the group that the user is in
	- Unique Files Owned by User

6. If our user has sudo privileges over a binary not in GTFOBins, try the LD_PRELOAD Privilege Escalation Exploit

7. If custom binaries exist with the SETUID bit set, try Shared Object Hacking
  
8. Check the groups the user is a part of and see if any of them are privileged groups
	- Enumerate Existing Groups
	- LXC / LXD Group Membership
	- Docker Group Membership
	- Disk Group Membership
	- ADM Group Membership

9. Check for PATH abuse (unlikely)  

10. Check for Wildcard abuse in cron jobs or custom scripts

11. Look for services running on internal ports that were not accessible from the outside with netstat
	- Checking for Internal Listening Ports/Services

12. Check for additional NICs using commands like ifconfig to see if there are any other sub networks
	- Enumerate Network Interfaces
	- Enumerate Other Hostnames

13. Look for cronjobs running writable scripts / services running as root or another privileged user

14. Look for abusable linux capabilities

15. Look for vulnerable application / service versions
	- Vulnerable Services
	- Enumerate Services and Versions
	- Enumerating Binaries

16. Check for Logrotate exploit

17. Check for kernel exploits
	- Search for exploits on GitHub, ExploitDB, and Metasploit

18. If Python script exists, check for Python Library Hijacking

19. Check for vulnerable versions of Netfilter  

20. Check for hijackable tmux sessions    

21. If tcpdump exists on the machine, try capturing cleartext traffic for credentials

22. Check for recent exploits and zero days
	- Recent Zero Days (From CPTS Modules)
23. Check /etc/bash.bashrc for actions taken on login for any user

Attempt to brute force root user with password file and sucrack

## Linux Container Privilege Escalation

1. Linux Containers (LXC/LXD)

2. Docker Privilege Escalation

3. Kubernetes Privilege Escalation 

## Documentation and Reporting
Follow the Bruno Rocha Moura Method for documenting findings  


