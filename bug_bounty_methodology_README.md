# Bug Bounty Hunting Methodology

This document describes a complete workflow used in bug bounty hunting
and web application penetration testing.

Workflow: Scope → Recon → Subdomain Enumeration → Live Host Discovery →
Port Scanning → Technology Detection → Directory Fuzzing → Parameter
Discovery → JS Analysis → Vulnerability Scanning → Manual Testing →
Exploitation → Reporting

------------------------------------------------------------------------

## 1. Scope Identification

Before testing any target always verify the program scope.

Tasks: - Read bug bounty program rules - Identify allowed domains - Note
restricted testing activities

Example scope: \*.target.com api.target.com app.target.com

------------------------------------------------------------------------

## 2. Reconnaissance (Information Gathering)

Goal: Collect basic information about the target.

Tools: - Amass - theHarvester - Whois - crt.sh

Example: amass enum -passive -d target.com

------------------------------------------------------------------------

## 3. Subdomain Enumeration

Goal: Discover hidden subdomains.

Tools: - Subfinder - Assetfinder - Amass

Example: subfinder -d target.com -o subdomains.txt

Example Output: admin.target.com api.target.com dev.target.com
test.target.com

------------------------------------------------------------------------

## 4. Live Host Discovery

Check which subdomains are active.

Tool: httpx

Example: cat subdomains.txt \| httpx -o live.txt

------------------------------------------------------------------------

## 5. Port Scanning

Discover open ports.

Tool: Nmap

Example: nmap -sC -sV target.com

Example: 80 HTTP 443 HTTPS 22 SSH

------------------------------------------------------------------------

## 6. Technology Detection

Identify technologies used by the website.

Tools: - WhatWeb - Wappalyzer - BuiltWith

Example: whatweb https://target.com

------------------------------------------------------------------------

## 7. Directory Fuzzing

Find hidden directories.

Tools: - FFUF - Dirsearch - Gobuster

Example: ffuf -u https://target.com/FUZZ -w
/usr/share/wordlists/dirb/common.txt

Possible findings: /admin /login /dashboard /api

------------------------------------------------------------------------

## 8. Parameter Discovery

Find URL parameters.

Tools: - ParamSpider - Waybackurls - gau

Example: waybackurls target.com

Example: target.com/product?id= target.com/search?q=

------------------------------------------------------------------------

## 9. JavaScript Analysis

Analyze JS files for secrets.

Tools: - LinkFinder - SecretFinder

Look for: API keys tokens hidden endpoints

------------------------------------------------------------------------

## 10. Vulnerability Scanning

Automated vulnerability scanning.

Tool: Nuclei

Example: nuclei -u https://target.com

------------------------------------------------------------------------

## 11. Manual Testing

Most vulnerabilities are discovered manually.

Tool: Burp Suite

Use it to: - intercept requests - modify parameters - test
authentication logic

------------------------------------------------------------------------

## 12. SQL Injection Testing

Tool: SQLMap

Example: sqlmap -u "https://target.com/product?id=1" --dbs

------------------------------------------------------------------------

## 13. XSS Testing

Tool: Dalfox

Example: dalfox url https://target.com/search?q=test

Payload:
```{=html}
<script>alert(1)</script>
```

------------------------------------------------------------------------

## 14. IDOR Testing

Example request: GET /api/user/1001

Change to: GET /api/user/1002

If another user's data is accessible → IDOR vulnerability.

------------------------------------------------------------------------

## 15. Reporting

A proper vulnerability report includes:

-   Title
-   Description
-   Steps to reproduce
-   Proof of Concept
-   Impact
-   Fix recommendation

------------------------------------------------------------------------

## Tools Summary

Recon → Amass\
Subdomain Enumeration → Subfinder\
Live Host Discovery → httpx\
Port Scanning → Nmap\
Technology Detection → WhatWeb\
Directory Fuzzing → FFUF\
Parameter Discovery → ParamSpider\
JS Analysis → LinkFinder\
Vulnerability Scan → Nuclei\
Manual Testing → Burp Suite\
SQL Injection → SQLMap\
XSS Testing → Dalfox

------------------------------------------------------------------------

Disclaimer: Only perform security testing on systems you have permission
to test.
