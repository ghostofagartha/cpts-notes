### Detection: Command-line & User Agent

**Command-line detection:**
- Blacklisting: weak, bypassed by case changes (nc → NC → Nc)
- Whitelisting: robust, but time-consuming to set up

**User Agent detection:**
- HTTP clients identified by User-Agent header (curl, PowerShell, Nmap, sqlmap all have defaults)
- Defenders build whitelist of legit UAs (browsers, Windows Update, AV) → flag anomalies in SIEM
- Reference: useragentstring.com (common UA list)

**Attacker implication:**
- Default tool UAs are easily flagged
- Spoof/change UA to blend in during file transfers