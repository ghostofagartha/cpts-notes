**Summary**
- Resolves domain names to IPs, port 53
- Hierarchical: root → TLD → authoritative nameservers
- Usually unencrypted (vulnerable to interception)

**How It Works**
- Client asks resolver "what's the IP of example.com?"
- Resolver queries nameservers up the chain
- Returns IP address
- **Why important:** Everything on internet depends on DNS

**Server Types (know these concepts)**
- **Root servers:** Top level, know where TLDs are
- **Authoritative:** Owns the actual zone data
- **Recursive resolver:** Does the lookup for you
- **Caching server:** Stores previous lookups

**Dns Records (what information is stored)**
- **A:** IPv4 address (what domain points to)
- **AAAA:** IPv6 address
- **MX:** Mail server (where emails go)
- **NS:** Nameserver (who authorizes this domain)
- **TXT:** Text records (verification, SPF, DKIM)
- **CNAME:** Alias to another domain
- **SOA:** Zone information, admin email
- **PTR:** IP to domain (reverse DNS)

**Why This Matters**
- **A/AAAA** = tells you the actual IP
- **MX** = tells you email infrastructure
- **NS** = tells you who manages the domain
- **TXT** = tell you external services (Atlassian, Google, etc)
- **Each record = reconnaissance data**

***Dangerous Configurations***
**Zone Transfer Allowed**
- What: dig @[nameserver] [domain] AXFR returns all records
- Why dangerous: Anyone gets entire zone (all subdomains, mail servers)
- Impact: Information disclosure
- Test: dig @[nameserver] [domain] AXFR

**Open Recursive Resolver**
- What: DNS server answers queries from anyone
- Why dangerous: Can be used for DDoS amplification attacks
- Test: Query from external IP, see if it responds

**Subdomain Rakeover Risk**
- What: Subdomain points to deleted service (expired domain/service)
- Why dangerous: Attacker registers that service, takes over subdomain
- Impact: Phishing, malware distribution
- Test: Enumerate subdomains, check if they're valid

***Quick Enumeration Commands***
**All Records:**
dig [domain] ANY
- Shows: A, AAAA, MX, NS, TXT, SOA
- Fast overview of DNS setup

**Specific Record:**
dig [domain] A           (IPv4)
dig [domain] MX          (mail servers)
dig [domain] NS          (nameservers)
dig [domain] TXT         (text records, SPF, DKIM)

**Zone Transfer (try to get all records):**
dig @[nameserver] [domain] AXFR
- If works = you get entire zone
- Usually blocked = still worth trying

**Reverse DNS:**
dig -x [IP]
- What domain is this IP?

**Nameserver Info:**
nslookup [domain]
- Shows: A records, MX records, NS records

**Subdomain Enumeration:**
fierce -dns [domain]     (brute force + zone transfer)
dnsenum [domain]         (comprehensive enum)
dig [domain] NS           (find nameservers)

**Nmap:**
nmap -sV -p53 [IP]
- Detects DNS service

***Workflow (how to hunt DNS vulns)***
**Step 1: Enumerate**
- dig [domain] ANY
- Look for: Subdomains, mail servers, nameservers

**Step 2: Find Nameservers**
- dig [domain] NS
- Note down each nameserver IP

**Step 3: Try Zone Transfer (main vuln to test)**
- dig @[nameserver] [domain] AXFR
- If succeeds = vulnerable, extract all records

**Step 4: Subdomain Enumeration**
- fierce -dns [domain] **OR** dnsenum [domain]
- Find all subdomains (hidden ones)

**Step 5: Check Subdomains**
For each subdomain found:
- Does it point to valid service?
- Can it be taken over?
- Does it have different IP than main domain?

**Step 6: Extract Info**
From all above:
- Mail servers (email infrastructure)
- Subdomains (attack surface)
- Nameservers (who manages it)
- External services (from TXT records)

**Remember**
- DNS = reconnaissance mainly (not RCE)
- Zone transfer = biggest DNS vulnerability
- Subdomains = often reveal attack surface
- TXT records = tell you what external services they use
- Commands: dig is your main tool, learn its options
- In labs: Practice dig commands, understand what each record means