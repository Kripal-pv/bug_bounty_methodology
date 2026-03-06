# 🐞 Bug Bounty Hunting Methodology (Step‑by‑Step)

A practical **GitHub‑friendly methodology** for bug bounty hunters and
web application penetration testers.

> ⚠️ **Use only on systems you have permission to test.**

------------------------------------------------------------------------

# 🧭 Workflow Overview

    Scope → Recon → Subdomain Enumeration → Live Host Discovery → Port Scan
    → Technology Detection → Directory Fuzzing → Parameter Discovery
    → JavaScript Analysis → Vulnerability Scanning → Manual Testing
    → Exploitation → Reporting

------------------------------------------------------------------------

# 1️⃣ Scope Identification

🔎 Always check what is allowed.

**Tasks**

-   Read bug bounty program rules
-   Identify allowed domains
-   Check restricted actions

**Example Scope**

    *.target.com
    api.target.com
    app.target.com

------------------------------------------------------------------------

# 2️⃣ Reconnaissance (Information Gathering)

🎯 Goal: Gather information about the target.

### Tools

-   Amass
-   theHarvester
-   Whois
-   crt.sh

### Copy‑Paste Command

``` bash
amass enum -passive -d target.com
```

------------------------------------------------------------------------

# 3️⃣ Subdomain Enumeration

🎯 Goal: Discover hidden subdomains.

### Tools

-   Subfinder
-   Assetfinder
-   Amass

### Copy‑Paste Command

``` bash
subfinder -d target.com -o subdomains.txt
```

**Example Output**

    admin.target.com
    api.target.com
    dev.target.com
    test.target.com

------------------------------------------------------------------------

# 4️⃣ Live Host Discovery

Check which subdomains are active.

### Tool

httpx

### Copy‑Paste Command

``` bash
cat subdomains.txt | httpx -o live.txt
```

------------------------------------------------------------------------

# 5️⃣ Port Scanning

Discover open ports and services.

### Tool

Nmap

### Copy‑Paste Command

``` bash
nmap -sC -sV target.com
```

**Example Result**

    80   HTTP
    443  HTTPS
    22   SSH

------------------------------------------------------------------------

# 6️⃣ Technology Detection

Identify technologies used by the website.

### Tools

-   WhatWeb
-   Wappalyzer
-   BuiltWith

### Command

``` bash
whatweb https://target.com
```

------------------------------------------------------------------------

# 7️⃣ Directory & Endpoint Discovery

Find hidden directories.

### Tools

-   FFUF
-   Dirsearch
-   Gobuster

### Copy‑Paste Command

``` bash
ffuf -u https://target.com/FUZZ -w /usr/share/wordlists/dirb/common.txt
```

**Possible Findings**

    /admin
    /login
    /dashboard
    /backup
    /api

------------------------------------------------------------------------

# 8️⃣ Parameter Discovery

Find URL parameters for testing.

### Tools

-   ParamSpider
-   Waybackurls
-   gau

### Copy‑Paste Command

``` bash
waybackurls target.com
```

Example:

    target.com/product?id=
    target.com/search?q=
    target.com/login?redirect=

------------------------------------------------------------------------

# 9️⃣ JavaScript Analysis

JavaScript files often contain hidden endpoints.

### Tools

-   LinkFinder
-   SecretFinder

Look for:

    API keys
    tokens
    hidden endpoints
    internal URLs

------------------------------------------------------------------------

# 🔟 Vulnerability Scanning

Automated vulnerability scanning.

### Tool

Nuclei

### Copy‑Paste Command

``` bash
nuclei -u https://target.com
```

Detects:

-   exposed admin panels
-   outdated software
-   misconfigurations

------------------------------------------------------------------------

# 1️⃣1️⃣ Manual Testing

Most vulnerabilities are found manually.

### Tool

Burp Suite

Use Burp Suite to:

-   intercept requests
-   modify parameters
-   analyze authentication
-   test business logic

------------------------------------------------------------------------

# 1️⃣2️⃣ SQL Injection Testing

### Tool

SQLMap

### Copy‑Paste Command

``` bash
sqlmap -u "https://target.com/product?id=1" --dbs
```

------------------------------------------------------------------------

# 1️⃣3️⃣ Cross‑Site Scripting (XSS)

### Tool

Dalfox

### Copy‑Paste Command

``` bash
dalfox url https://target.com/search?q=test
```

**Example Payload**

    <script>alert(1)</script>

------------------------------------------------------------------------

# 1️⃣4️⃣ IDOR Testing

Example request:

    GET /api/user/1001

Modify:

    GET /api/user/1002

If another user's data is accessible → **IDOR vulnerability**.

------------------------------------------------------------------------

# 1️⃣5️⃣ File Upload Testing

Try uploading test files:

    shell.php
    shell.jsp
    shell.php5

If executed by server → possible **Remote Code Execution**.

------------------------------------------------------------------------

# 1️⃣6️⃣ Authentication Testing

Check for:

-   weak password policies
-   broken authentication
-   password reset flaws
-   session fixation

------------------------------------------------------------------------

# 1️⃣7️⃣ Vulnerability Reporting

A proper report includes:

    Title
    Description
    Steps to Reproduce
    Proof of Concept
    Impact
    Fix Recommendation

Example:

    Title: SQL Injection in Product Page

    Steps:
    1. Visit https://target.com/product?id=1
    2. Add ' after the parameter
    3. SQL error appears

    Impact:
    Database information exposure.

------------------------------------------------------------------------

# 🧰 Tools Summary

  Step                  Tool
  --------------------- -------------
  Recon                 Amass
  Subdomain Enum        Subfinder
  Live Host             httpx
  Port Scan             Nmap
  Tech Detection        WhatWeb
  Directory Fuzzing     FFUF
  Parameter Discovery   ParamSpider
  JS Analysis           LinkFinder
  Vulnerability Scan    Nuclei
  Manual Testing        Burp Suite
  SQL Injection         SQLMap
  XSS Testing           Dalfox

------------------------------------------------------------------------

# ⚠️ Disclaimer

This repository is **for educational and authorized testing only**.\
Do not test systems without permission.
