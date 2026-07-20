- Leverages pre-defined lists of potential subdomain name to test these names against the target domain name

**Process**
1. **Wordlist Selection**
2. **Iteration and Query**
3. **DNS Lookup**
4. **Filtering and Validation**

**Tools**
1. dnsenum
2. fierce
3. dnsrecon
4. amass
5. assetfinder
6. puredns
- **DNSEnum:**
	- For DNS reconnaissance, provides information about a target domain's DNS infrastructure and potential subdomains
	- **Features:**
		- DNS Record Enumeration
		- Zone Transfer Attempts
		- Subdomain Brute-Forcing
		- Google Scraping
		- Reverse Lookup
		- WHOIS Lookups
	- **Command**
		- `dnsenum --enum [domain name] -f [seclist] -r`
		- 