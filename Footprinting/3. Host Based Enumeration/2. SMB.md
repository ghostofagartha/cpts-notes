**Basics**
- **What:** File sharing protocol, Windows/Samba, network communication
- **Ports:** 139 (NetBIOS), 445 (direct TCP)
- **Protocol:** Application layer, used for file/printer sharing
- **Versions:** SMBv1 (legacy, vulnerable), SMBv2, SMBv3 (modern, secure)
- **Authentication:** NTLM, Kerberos (Active Directory)

**Samba (Linux SMB Server)**
- **What:** Open-source implementation of SMB for Linux
- **Config file:** /etc/samba/smb.conf
- **Service:** smbd (main), nmbd (NetBIOS name service)
- **Users:** Can use system users or Samba-specific users
- **Shares:** Defined in smb.conf with [share_name] sections

**Dangerous Samba Settings**
- **guest ok = yes:** Allow anonymous access to share
- **writable = yes:** Allow write access
- **read only = no:** Allow modifications
- **browseable = yes:** Make share visible in network
- **map to guest = Bad Password:** Treat failed auth as guest
- **public = yes:** Share accessible to everyone
- **security = share:** Share-level security (legacy, vulnerable)

**SMB Null Session**
- **What:** Connect to SMB without credentials
- **Why dangerous:** Allows user enumeration without password
- **How it works:** RPC (Remote Procedure Call) allows queries without auth
- **Impact:** Extract usernames, group info, share list
- **Mitigation:** Disable null sessions in registry/config

**SMB Authentication**
- **NTLM:** Hash-based (user:password hashed, sent over network)
- **Kerberos:** Ticket-based (Active Directory, more secure)
- **Guest access:** No credentials required (if enabled)
- **Pass-the-hash:** Use NTLM hash directly without password (if captured)

***QUICK ENUMERATION***
**Anonymous Share Listing:**
smbclient -N -L //[IP]
- **Result:** Lists shares accessible anonymously
- **Flag:** -N = no password, -L = list shares

**SMB Share Details:**
smbmap -H [IP]
- **Result:** Shows shares, permissions (READ, WRITE, etc), owner
- Better than smbclient for quick overview

**Aggressive Enumeration:**
crackmapexec smb [IP] --shares -u '' -p ''
- **Result:** Shares + guest access status
- More detailed than smbmap

**RPC User Enumeration:**
rpcclient -U "" [IP]
- **Inside rpcclient:**
	- **ssrvinfo:** Server info, OS version, domain
	- **enumdomusers:** List all users (if null session allowed)
	- **queryuser [RID]:** Get user details (RID = Relative ID)
	- **netshareenumall:** All shares on server
	- **netsharegetinfo [share]:** Details of specific share
	- **querygroup [RID]:** Group information
	- **getdompwset:** Domain password policy

**Full Enumeration:**
enum4linux-ng [IP] -A
- Combines all above methods
- **Shows:** users, groups, shares, policy, OS info

***Connecting To SMB Share***
**Anonymous Connection:**
smbclient //[IP]/[share] -N
- **Inside smbclient:**
	- **ls:** List files
	- **ls -R:** Recursive listing
	- **get [file]:** Download file
	- **put [file]:** Upload file (if writable)
	- **exit:** Disconnect

**With Credentials:**
smbclient //[IP]/[share] -U username
- Prompted for password
- Same commands as above

**Bulk Download:**
wget -m --no-passive smb://[IP]/[share]
- Downloads all accessible files

***Common SMB Vulnerabilities***
1. **Null Session Enabled**
	- Find: rpcclient -U "" [IP] connects without password
	- Impact: User enumeration, group info, share listing
	- Exploit: rpcclient → enumdomusers → queryuser [RID]
	- Mitigation: Disable null sessions (registry on Windows)
2. **Anonymous Share Access**
	- Find: smbclient -N -L shows accessible shares
	- Impact: Read sensitive files without credentials
	- Exploit: Connect → ls -R → get [sensitive_file]
	- Mitigation: Set guest ok = no in smb.conf
3. **Writable Shares**
	- Find: smbmap -H [IP] shows WRITE permission
	- Impact: Upload malicious files, modify documents, plant backdoors
	- Exploit: Connect → put [shell.php] → if web-accessible, RCE
	- Mitigation: Remove write permissions, disable if not needed
4. **Guest Account Enabled**
	- Find: crackmapexec shows guest access allowed
	- Impact: Full read access without knowing password
	- Exploit: Connect as guest → download all files
	- Mitigation: Disable guest account
5. **SMBv1 Running**
	- Find: nmap -sV -p445 shows SMBv1
	- Impact: Vulnerable to EternalBlue (CVE-2017-0144), RCE possible
	- Exploit: Use Metasploit exploit/windows/smb/ms17_010_eternalblue
	- Mitigation: Upgrade to SMBv2/v3
6. **Weak Permissions**
	- Find: Share allows EVERYONE:FULL or GUEST:WRITE
	- Impact: Anyone can read/modify files
	- Exploit: Connect → modify sensitive files → impact business
	- Mitigation: Restrict permissions to specific users/groups
7. **Credentials in Shares**
	- Find: Download all files from accessible share
	- Impact: Find passwords, SSH keys, API tokens
	- Exploit: grep -r "password" [downloaded_files]
	- Mitigation: Don't store credentials in shares

***NmapSMB Scanning***
**Basic:**
nmap -sV -p139,445 [IP]
- Detects SMB service and version

**With NSE Scripts:**
nmap -sV -p139,445 --script=smb* [IP]

**Key SMB NSE Scripts:**
- smb-enum-shares: Enumerate shares
- smb-enum-users: Enumerate users (if null session allowed)
- smb-os-discovery: OS version, NetBIOS names
- smb-vuln-ms17-010: Check for EternalBlue
- smb-protocols: Supported SMB versions
- smb-security-mode: Encryption, signing info

***Exploitation Workflow***
**Step 1: Enumerate**
- nmap -sV -p445 -sC [IP]
- smbmap -H [IP]
- crackmapexec smb [IP] --shares

**Step 2: Check Null Session**
- rpcclient -U "" [IP]
- If connects → null session enabled

**Step 3: Enumerate Users (if null session)**
- rpcclient → enumdomusers
- rpcclient → queryuser [RID]

**Step 4: List Shares**
- smbclient -N -L //[IP]
- smbmap -H [IP]

**Step 5: Check Permissions**
- smbmap -H [IP] → look for READ, WRITE, ADMIN
- which shares accessible?
- which writable?

**Step 6: Download Files**
- smbclient //[IP]/[share] -N
- ls -R → find interesting files
- get [file] → download

**Step 7: Check for Credentials**
- grep -r "password" [downloaded_files]
- grep -r "secret" [downloaded_files]
- Check for: .conf, .txt, .xlsx (password docs)

**Step 8: If Write Access**
- put [shell.php] → upload shell
- Check if /uploads maps to web directory
- Access via: http://[IP]/uploads/shell.php
- Possible RCE

**Step 9: Use Found Credentials**
- Credentials found on share?
- Try SSH with those creds
- Try RDP if Windows
- Try other services

***Escalation Path***

SMB Share → Credentials Found
		↓
Use credentials for SSH/RDP
		↓
System Access as user
		↓
Privilege Escalation (if needed)
		↓
Full system compromise

**OR**

SMB Share → Upload Shell (if writable + web-accessible)
		↓
Web RCE via shell
		↓
System access as web user
		↓
Privilege Escalation
		↓
Full system compromise

***Report Template***
- **Vulnerability:** Anonymous SMB Share Access
- **Severity:** HIGH
- **Description:** SMB shares on [IP] are accessible without authentication. Users can connect and download sensitive files.

**Affected Service:** SMB (port 445)
**Authentication:** None required

**Steps to Reproduce:**
1. Run: smbclient -N -L //[IP]
2. Connect to accessible share: smbclient //[IP]/[share] -N
3. Download files: ls -R, get [file]
4. Review downloaded files for sensitive data

**Impact:** Unauthorized access to confidential files, information disclosure, data theft

**Evidence:**
- Screenshots of accessible shares
- List of files downloaded
- Sensitive data found (if any)

**Remediation:**
1. Set "guest ok = no" in /etc/samba/smb.conf
2. Restrict share permissions to authorized users only
3. Remove world-readable/writable permissions
4. Disable SMBv1 if running
5. Enable SMB signing and encryption