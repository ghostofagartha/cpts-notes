***SSH***
- Port 22
**Footprinting the Service**
- **SSH-Audit**
	- `git clone https://github.com/jtesta/ssh-audit.git && cd ssh-audit`
	- `./ssh-audit.py [ip address]`
- **Change Authentication Method**
	- ssh -v [username]@[ip address]
	- ssh -v [username]@[ip address] -o PreferredAuthentications=password

***Rsync***
- Port 873
- Can be configured to use ssh for transfer
**Probing For Accessible Shares**
- `nc -nv [ip address] [port]`
**Enumerating On Open Share**
- rsync -av --list-only rsync:// [ip address]/[share name from previous command]