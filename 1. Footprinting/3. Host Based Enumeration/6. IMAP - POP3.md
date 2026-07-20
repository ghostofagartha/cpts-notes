**Basics**
- Internet Message Access Protocol
- Post Office Protocol
- SMTP is used for IMAP and pop3 for POP3
- Sends emails in text based in ASCII format

**IMAP**
- Manage emails directly on the server
- Port 143, alternatively 993
- Unencrypted
- Can be encrypted by using SSL/TLS
- Supports folder structure / tree
- Client-server based
- Synchronization between local and remote server mailbox
- Some clients allow offline managing of local mailbox and synchronize on connection establishment

**POP3**
- No synchronization
- Port 110, alternatively 995
- Manually list, retrieve and delete emails
- No sending functionality
- Less features than IMAP

**Working**
- Commands can be sent in succession without any confirmation from the other side
- Sent emails copied into IMAP folder, lets all clients have access to emails sent by anyother client

***IMAP COMMANDS***
1. `1 LOGIN username password`
2. `1 LIST "" *`: List all directories
3. `1 CREATE "INBOX"`
4. `1 DELETE "INBOX"`
5. `1 RENAME "ToRead" "Important"`
6. `1 LSUB "" *`: Return subset of names from the set of names that the User has declared as being **active** or **subscribed**
7. `1 SELECT INBOX`
8. `1 UNSELECT INBOX`
9. `1 FETCH <ID> id`: Retrieves data associated with a message in the mailbox
10. `1 CLOSE`: Removes all messages with the **Deleted** flag set
11. `1 LOGOUT`

***POP3 COMMANDS***
1. `USER username`
2. `PASS password`
3. `STAT`: Requests number of saved emails on the server
4. `LIST`
5. `RETR id`
6. `DELE id`
7. `CAPA`: Requests the server to display server capabilities
8. `RSET`: Requests the server to reset the transmitted information
9. `QUIT`

**Dangerous Settings**
1. `auth_debug`: Enables all authentication debug logging
2. `auth_debug_password`: Adjusts log verbosity, the submitted passwords, and scheme gets logged
3. `auth_verbose`: Logs unsuccessful authentication attempts and their reasons
4. `auth_verbose_password`: Logs passwords used for authentication and they can be truncated
5. `auth_anonymous_username`: Specifies username to be used when to be logged in as anonymous

**Footprinting IMAP via cURL**
- curl -k imaps://[ip address] --user user:password
	- -v can be used for verbose

**Encrypted Interaction**
- **openssl** or **ncat** for interacting with IMAP and POP3 over SSL

**Footprinting POP3 via OpenSSL (TLS Encrypted)**
- openssl s_client -connect [ip address]:pop3

**Footprinting IMAP via OpenSSL (TLS Encrypted)**
- openssl s_client -connect [ip address]:imap