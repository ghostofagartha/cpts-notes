***RDP***
- Remote Desktop Protocol
- Port 3389 (TCP for as transport protocol or UDP for connectionless remote administration)
- **Network Address Translation (NAT)**
- **Network Level Authentication (NLA):** Authenticating user before session
**Footprinting the Service**
- **Nmap**
	- `nmap -sV -sC [ip address] -p3389 --script rdp*`
	- `nmap -sV -sC [ip address] -p3389 --script rdp* --packet-trace --disable-arp-ping -n`
**RDP Security Check**
- **Installation**
	- `sudo cpan`
- **RDP Security Check**
	- `git clone https://github.com/CiscoCXSecurity/rdp-sec-check.git && cd rdp-sec-check`
	- `./rdp-sec-check.pl [ip address]`
**Connecting To RDP From Linux**
- **Tools:** `xfreerdp`, `rdesktop` and `Remmina`
- **Initiate an RDP Session**
	- `xfreerdp /u:[username] /p:"[password]" /v:[ip address]`

***WinRM***
- Windows Remote Management
- Port 5985 and 5986 where 5986 using HTTPS
- **Windows Remote Shell (WinRS)**
**Footprinting the Service**
- **Nmap**
	- `nmap -sV -sC [ip address] -p5985,5986 --disable-arp-ping -n`
- **Evil-WinRM**
	- `evil-winrm -i [ip address] -u [username] -p [password]`

***WMI***
- Windows Management Instrumentation
- TCP 135, after communication moved to random port
**Footprinting the Service**
- **WMIExec.py**
	- `/usr/share/doc/python3-impacket/examples/wmiexec.py [username]:"[password]"@[ip address] [hostname]`