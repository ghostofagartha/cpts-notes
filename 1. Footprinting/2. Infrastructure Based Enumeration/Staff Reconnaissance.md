**Purpose:** Identify employee skills, tech stack, infrastructure

**LinkedIn - Job Posts**
- Shows: Languages (Java, Python, C++), databases (PostgreSQL, MySQL), 
  frameworks (Flask, Django, Spring), tools (Git, Atlassian)
- Example: Job post reveals company uses Python/Django → search for 
  Django security vulns

**LinkedIn - Employee Profiles**
- Skills section: Shows tech they use (React, Kafka, Elastic, etc.)
- About section: Portfolio links (GitHub) → find leaked creds, hardcoded 
  keys, personal info
- Career section: Job history → identify weak areas, strengths
- Search tip: Filter by "Security" + "Development" roles → find security 
  posture

**GitHub (via employee profiles)**
- Public repos = exposed: personal emails, API keys, JWT tokens, 
  hardcoded secrets
- Often follows standard tutorials → predictable file names/structure
- Source code = understand their stack + find vulns

***KEY FINDINGS TO LOOK FOR:***
- Programming languages (what they use)
- Frameworks (Flask, Django, Spring = know their patterns)
- Cloud services (AWS, Azure mentioned in stack)
- Security tools (CompTIA Sec+, etc. = what they defend against)
- Leaked credentials (emails, keys in public repos)