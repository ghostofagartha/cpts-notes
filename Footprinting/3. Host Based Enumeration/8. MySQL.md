**Basics**
- SQL relational database
- Controlled using SQL database language
- Works according to client-server principle
- Consists of:
	1. One Server
	2. One or more clients
- Mostly exported as a single `.sql` file
- TCP 3306

**MySQL Databases**
- Often combined with Linux OS, PHP and Apache web server, and sometimes Nginx
- Sensitive data can be stored as plain text but normally, PHP encrypts them by **One-Way Encryption** beforehand

**MySQL Commands**
- Informs users in case of an error, with important information
- Web applications send the generated information to the client after processing correctly
- Commands can `display`, `modify`, `add` or `delete` rows from the tables
- Commands can `change table structures`, `create or delete relationships and indexes` and `manage users`
- **Commands:**
	- mysql -u [username] -p[password] -h [ip address]
	- show databases;
	- use [database];
	- show tables;
	- show colums from [table];
	- select * from [table];
	- select * from [table] where [column] = "[string]";

**Default Configuration**
- Configuration file: /etc/mysql/my.cnf for Debian and /etc/my.cnf for Arch (Depends upon the OS)

**Dangerous Settings**
- `user`: Sets default user for running MySQL service
- `password`: Sets password for the default user
- `admin_address`: IP address on which to listen to TCP/Ip connections on the admin network interface
- `debug`: Indicates current debugging settings
- `sql_warnings`: Toggles if single-row INSERT statements produce an information string in case of warnings
- `secure_file_priv`: Limit the effects of data import and export operations

***Footprinting the Service***
**Scanning MySQL Service**
- `sudo nmap [ip address] -sV -sC -p3306 --script mysql*`
**Interaction with the MySQL Server**
- `mysql -u root -p <password> -h [ip address]`