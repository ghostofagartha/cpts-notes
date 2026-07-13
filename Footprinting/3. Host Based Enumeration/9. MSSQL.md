**Basics**
- Microsoft SQL-based relational database management
- Usually on windows but can be on Linux or MacOS

**MSSQL Clients**
Many clients can be used including:
- mssql-cli
- SQL Server Powershell
- HeidiSQL
- SQLPro
- Impacket's mssqlclient.py

**MSSQL Databases**
- **master:** Tracks system info for an SQL server instance
- **model:** Template database for new databases created
- **msdb:** To schedule job & alert
- **tempdb:** Stores temporary objects
- **resource:** Read-only containing system objects

**Dangerous Settings**
- MSSQL clients not using encryption
- Use of self-signed certificates
- Use of named pipes
- Weak and default `sa` credentials

***Footprinting the Service***
**Nmap MSSQL Script Scan** 
````
sudo nmap --script ms-sql-info,ms-sql-empty-password,ms-sql-xp-cmdshell,ms-sql-config,ms-sql-ntlm-info,ms-sql-tables,ms-sql-hasdbaccess,ms-sql-dac,ms-sql-dump-hashes --script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=,mssql.instance-name=MSSQLSERVER -sV -p 1433 [ip address]
````
**Metasploit MSSQL Ping**
```
msf6 auxillary(scanner/mssql/mssql_ping) > set rhosts [ip address]
rhosts => [ip address]
msf6 auxillary(scanner/mssql/mssql_ping) > run
```
**Connecting with Mssqlclient.py**
````
python3 mssqlclient.py Administrator@[ip address] -windows-auth
````
