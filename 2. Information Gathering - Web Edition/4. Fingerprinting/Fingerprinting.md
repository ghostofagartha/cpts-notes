- Focuses on extracting technical details about the technologies powering a website or web application
- Used in web reconnaissance for following reasons:
	1. **Targeted Attacks**
	2. **Identifying Misconfigurations**
	3. **Prioritising Targets**
	4. **Building a Comprehensive Profile**

**Fingerprinting Techniques**
- **Banner Grabbing**
- **Analyzing HTTP Headers**
- **Probing for Specific Responses**
- **Analyzing Page Content**

**Automated Tools**
- `Wappalyzer`
- `BuiltWith`
- `WhatWeb`
- `Nmap`
- `Netcraft`
- `wafw00f`: For detecting and identifying web firewalls

***Fingerprinting a Target***
**Banner Grabbing**
- `curl -I [web address]`
**Wafw00f**
- `wafw00f [web address]`
**Nikto**
- `nikto -h [web address] -Tuning b`
**WhatWeb**
- `whatweb -H "Host: [VHost]" http://[ip address]`