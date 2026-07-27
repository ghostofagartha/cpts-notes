### Prominent Windows Exploits (know these - appear in CPTS labs)
- **MS08-067:** SMB flaw, used by Conficker + Stuxnet
- **EternalBlue (MS17-010):** SMBv1 RCE, used in WannaCry/NotPetya, affects Windows 2008-2016
- **PrintNightmare:** Windows Print Spooler RCE, low-priv shell → SYSTEM
- **BlueKeep (CVE 2019-0708):** RDP RCE, affects Windows 2000 to Server 2008 R2
- **Zerologon (CVE 2020-1472):** Netlogon cryptographic flaw → Domain Admin in ~256 guesses

****
### Enumerating Windows & Fingerprinting Methods
**TTL fingerprinting (ping):**
- Windows TTL = 128
- Linux TTL = 64

**Banner Grabbing:**
- sudo nmap -v [target] --script banner.nse

****
### Bats, DLLs, & MSI Files, Oh My!
**Payload types:**
- **DLL:** inject/hijack → SYSTEM or UAC bypass
- **.bat:** automated CMD commands (open ports, connect back)
- **VBS:** phishing/macro-based execution
- **.msi:** Windows Installer payload → run with msiexec → elevated shell
- **PowerShell:** .NET-based scripting, most flexible

****
### Tools, Tactics, and Procedures for Payload Generation, Transfer, and Execution
**Payload generation tools:**
- MSFVenom + Metasploit (primary)
- Mythic C2 (alternative C2)
- Nishang (offensive PowerShell scripts)
- Darkarmour (obfuscated binaries for AV evasion)

**Transfer methods:**
- Impacket (psexec, smbclient, SMB server)
- SMB shares (C$, admin$)
- HTTP/S, FTP, TFTP

****
### Example Compromise Walkthrough
**Flow (mental model, not commands):**
1. nmap -v -A [target] → identify OS, services, versions
2. Research vulnerabilities based on OS version + open services
3. Validate with MSF auxiliary scanner
4. Select exploit + payload in MSF
5. Set RHOSTS, LHOST, LPORT → run → get shell

**EternalBlue example:**
- use auxiliary/scanner/smb/smb_ms17_010 → validate vuln
- use exploit/windows/smb/ms17_010_psexec → exploit
- payload: windows/meterpreter/reverse_tcp

****
### CMD-Prompt and PowerShells for Fun and Profit
#### Use CMD when:
- Older host (pre-Windows 7, no PowerShell)
- Need stealth (CMD doesn't log commands)
- Simple tasks, batch files, net commands
- Execution policy or UAC might block PowerShell

#### Use PowerShell when:
- Need cmdlets or custom scripts
- Interacting with .NET objects
- Stealth is less important
- Cloud services or aliases needed

#### WSL and PowerShell For Linux
- WSL network traffic bypasses Windows Firewall and Defender (blind spot)
- PowerShell Core on Linux carries over normal PS functions
- Both used by malware to avoid AV/EDR detection
- Advanced topic — just know it exists