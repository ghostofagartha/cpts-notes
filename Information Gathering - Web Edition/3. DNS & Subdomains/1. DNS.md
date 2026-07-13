**How DNS Works**
1. **DNS Query:** Your Computer Asks for Directions
2. **Recursive Lookup:** The DNS Resolver Checks its Map
3. **Root Name Server Points the Way**
4. **TLD  Name Server Narrows It Down**
5. **Authoritative Name Server Delivers the Address**
6. **The DNS Resolver Returns the Information**
7. **Your Computer Connects**

**The Hosts File**
- Used to map hostnames to IP addresses inside a text file
- /etc/hosts

**Key DNS Concepts**
- **Zone:** A distinct part of the domain namespaces that a specific entity or admin manages
- **Domain Name:** A human-readable label for a website or other internet resource
- **IP Address:** A unique numerical identifier of any device connected to internet
- **DNS Resolver:** A server that translates domain names into IP addresses
- **Root Name Server:** The top-level server in the DNS hierarchy
- **TLD Name Server:** Servers responsible for specific top-level domains
- **Authoritative Name Server:** The server that holds the actual IP address for a domain
- **DNS Record Types:** Different types of information stored in DNS

**Record Type**
1. **A:** Address Record **(IPv4)**
2. **AAAA:** IPv6 Address Record
3. **CNAME:** Canonical Name Record **(Creates aliases between hostnames)**
4. **MX:** Mail Exchange Record **(Specifies mail servers for handling a domain's emails)**
5. **NS:** Name Server Record **(Delegates DNS zones to authoritative name servers)**
6. **TXT:** Text Record **(Stores arbitrary text information)**
7. **SOA:** Start of Authority Record **(Specifies administrative info about a DNS zone)**
8. **SRV:** Service Record **(Defines the hostname and port number for specific services)**
9. **PTR:** Pointer Record **(Used for reverse DNS lookups)**
- ***Note:*** **"IN"** in the records stand for "Internet"

**Why DNS Matters for Web Recon**
- **Uncovering Assets**
- **Mapping the Network Infrastructure**
- **Monitoring for Changes**