**Basics**
- **What:** Application layer protocol for file transfer (TCP/IP)
- **Ports:** 21 (control/commands), 20 (data transfer)
- **Modes:** Active (server initiates, no firewall) | Passive (client initiates, firewall present)
- **Authentication:** Usually requires credentials OR anonymous access
- **Risk:** Clear-text protocol (can be sniffed)

**Anonymouse FTP**
- **What:** Anyone can access without password
- **Username:** anonymous
- **When allowed:** Companies allow internal file sharing
- **Risk:** Download company files without credentials, sometimes can upload files
- **How to test:** ftp [IP] → Name: anonymous → ls -R

***TFTP (Trivial File Transfer Protocol)***
- **What:** Simpler than FTP, no authentication
- **Protocol:** UDP (unreliable, no error checking)
- **Use:** Local networks only (no firewall), device configuration (routers, printers)
- **Commands:** connect [ip] [port], get [file], put [file], quit, status, verbose
- **Limitation:** NO directory listing (can't browse)

***vsFTPd (VSFTPD) - COMMON FTP SERVER***
**Configuration Files:**
- **/etc/vsftpd.conf:** Main config file
- **/etc/ftpusers:** List of blocked users

**Key Settings**
- **anonymous_enable=YES/NO:** Allow anonymous access
- **anon_upload_enable=YES:** Anonymous can upload **(Dangerous)**
- **anon_mkdir_write_enable=YES:** Anonymous can create directories **(Dangerous)**
- **no_anon_password=YES:** Don't ask for password **(Dangerous)**
- **write_enable=YES:** Allow STOR, DELE, RNFR, RNTO, MKD, RMD commands **(Dangerous)**
- **local_enable=YES:** Local users can login
- **chroot_local_user=YES:** Restrict users to home directory
- **hide_ids=YES:** Show "ftp" instead of UID/GID **(hides file ownership)**
- **ls_recurse_enable=YES:** Allow recursive directory listing

**Dangerous Configuration Example**
- anonymous_enable=YES
- anon_upload_enable=YES
- anon_mkdir_write_enable=YES
- no_anon_password=YES
- write_enable=YES
- **Result:** Anyone can upload/download/delete files without password

**Banner & Version**
- FTP shows banner on connect (response code 220)
- Banner often contains FTP server version
- Example: "220 vsFTPd 3.0.3"
- Use this to identify version and known vulnerabilities

***FTP Commands (Client)***
**Connection**
- **ftp [IP]:** Connect to FTP server
- **open [IP]:** Open connection
- **close:** Close connection
- **quit/exit:** Exit FTP

**Navigation & Listing**
- **ls:** List files (non-recursive)
- **ls -R:** List files recursively (all subdirectories)
- **pwd:** Print working directory
- **cd [dir]:** Change directory

**File Transfer**
- **get [file]:** Download single file
- **mget [files]:** Download multiple files
- **put [file]:** Upload single file
- **mput [files]:** Upload multiple files

**Status & Debugging**
- **status:** Show FTP connection status
- **debug:** Turn on debug mode (show packet info)
- **trace:** Turn on tracing (show detailed packet data)

***Downloading Files***
**1. Manual Method:**
- ftp [IP]
- Name: anonymous
- Password: (leave blank)
- ls -R (see all files)
- get [filename] (download)
- exit

**2. Bulk Download (Linux):**
- wget -m --no-passive ftp://anonymous:anonymous@[IP]
- Result: Creates directory with IP name, downloads all accessible files

***Uploading Files***
**How**
- ftp [IP]
- Name: anonymous
- put [filename] (upload file)
- ls (verify upload)
- exit
**Risk:** If FTP directory is accessible via web server = RCE possible
**Example:** Upload shell.php to /var/www/html via FTP → Execute via browser

***Nmap Scanning FTP***
**Basic Scan**
- nmap -sV -p21 [IP]
- Output: FTP version

**Complete Scan**
- nmap -sV -p21 -sC -A [IP]
	**Options**
	- **-sV:** Service version detection
	- **-sC:** Default NSE scripts
	- **-A:** Aggressive (OS detection, version, script scan)

**NSE Scripts for FTP**
- **ftp-anon:** Check if anonymous login allowed, show directory contents
- **ftp-syst:** Execute STAT command, show server info and version
- **ftp-vsftpd-backdoor:** Check for vsftpd backdoor (CVE-2011-2523)
- **ftp-proftpd-backdoor:** Check for ProFTPd backdoor
- **ftp-bounce:** Check FTP bounce vulnerability
- **ftp-brute:** Brute force FTP credentials
- **ftp-libopie:** Check for OPIE authentication weakness

**Update Scripts**
- sudo nmap --script-updatedb

***SSL/TLS Encrypted FTP***
**How to Connect (openssl)**
- openssl s_client -connect [IP]:21 -starttls ftp

**Info Extracted**
- SSL certificate (reveals hostname, email, organization)
- Certificate CN (Common Name): actual server hostname
- Email in cert: admin/company email
- Organization info

**Example Cert Info**
- CN = master.inlanefreight.htb
- O = Inlanefreight
- OU = Dev
- emailAddress = admin@inlanefreight.htb

***Enumeration Workflow***
1. **Nmap scan:** nmap -sV -p21 -sC [IP]
   → Identify FTP version
   → Check anonymous access
   → Get directory listing

2. **Check version** Search for known vulnerabilities

3. **If anonymous access**
   → ls -R (explore all files)
   → wget -m (download everything)
   → Look for: config files, credentials, source code, backups

4. **If upload possible**
   → Upload test file to verify write access
   → If web server uses same directory: upload shell.php = RCE

5. **If SSL/TLS**
   → openssl s_client -connect [IP]:21 -starttls ftp
   → Extract certificate info (hostname, email, organization)

***Common Misconfigurations***
**Vulnerability: Anonymous Access Enabled**
- **Risk:** Download all company files
- **Detection:** Nmap ftp-anon script
- **Exploitation:** ftp [IP] → anonymous → ls -R → get [files]

**Vulnerability: Anonymous Upload Enabled**
- **Risk:** Upload malicious files, replace existing files
- **Detection:** Check write permissions in nmap output ([NSE: writeable])
- **Exploitation:** put [shell.php] → access via web if accessible

**Vulnerability: FTP Directory = Web Server Directory**
- **Risk:** Upload shell → Execute via browser
- **Detection:** Check file paths in nmap output
- **Exploitation:** upload shell.php → curl http://[IP]/shell.php

**Vulnerability: Credentials in FTP Files**
- **Risk:** Find usernames/passwords in readable files
- **Detection:** Download and read all files
- **Exploitation:** Use credentials for SSH, other services

***Tools Summary***
**Manual Testing:**
- **ftp:** Built-in FTP client
- **nc/netcat:** Connect to port 21 manually
- **telnet:** Connect to port 21 (if available)
- **openssl:** For SSL/TLS connections

**Automated Scanning:**
- **nmap:** Version detection, NSE scripts
- **wget:** Bulk download with -m flag

***Commands to Remember:***
- ftp [IP]
- nmap -sV -p21 -sC [IP]
- wget -m --no-passive ftp://anonymous:anonymous@[IP]
- openssl s_client -connect [IP]:21 -starttls ftp