# Web Application Security Audit — DVWA

> **Passive Vulnerability Assessment | GITAM University | CSE Cyber Security**

---

## Overview

This repository contains the complete deliverables for a passive, read-only web application security audit conducted on **DVWA (Damn Vulnerable Web Application)** as part of a security consulting learning exercise.

**Analyst:** Atul.N
**Date:** 07 June 2026
**Overall Risk Grade:** `D` — 3 High · 3 Medium · 4 Low findings

---

## Repository Structure

```
web-security-audit-dvwa/
│
├── README.md                          ← This file
│
├── report/
│   └── Vulnerability_Assessment_Report_DVWA.pdf   ← Full audit report
│
├── evidence/
│   └── tool_outputs.txt               ← All raw tool outputs documented
│
└── canva/
    └── canva_report_link.txt          ← Link to Canva-designed report
```

---

## Website Tested

| Field         | Value |
|---------------|-------|
| **Target**    | DVWA — Damn Vulnerable Web Application |
| **URL**       | `http://localhost:42001` (local install on Kali Linux) |
| **What it is**| An intentionally vulnerable PHP web app used for security training |
| **Why chosen**| Legal to test, realistic vulnerabilities, no third-party harm |

> ⚠️ **Ethics Note:** All testing was performed on a locally-hosted intentionally-vulnerable application. No real production systems, third-party sites, or external infrastructure were targeted at any point.

---

## Scope

### ✅ In Scope (Allowed)
- HTTP response headers analysis
- TLS/HTTPS configuration checks
- Cookie security flag inspection
- Publicly accessible path enumeration (HTTP status codes only)
- Client-side JavaScript library version detection
- Passive automated scanning (OWASP ZAP spider mode)

### ❌ Out of Scope (Not Allowed)
- Login bypass or authentication attacks
- SQL injection or any exploitation
- Brute force or credential stuffing
- Denial-of-Service (DoS) or flooding
- File upload exploitation
- Any active attack against any real website

---

## Tools Used

| Tool | Version | Purpose |
|------|---------|---------|
| **Kali Linux** | 2024.x | Operating system / audit platform |
| **Nmap** | 7.93 | Port scanning and service version detection |
| **curl** | 7.88 | HTTP header analysis and path enumeration |
| **OWASP ZAP** | 2.14 | Passive spider and automated alert collection |
| **Firefox DevTools** | Built-in | Cookie flag inspection, live console testing |
| **Canva** | Web | Professional report design |

---

## Findings Summary

| ID | Finding | Risk |
|----|---------|------|
| F1 | Session cookie missing HttpOnly flag | 🔴 HIGH |
| F2 | Missing HTTP Strict-Transport-Security (HSTS) | 🔴 HIGH |
| F3 | Server version banner exposed | 🔴 HIGH |
| F4 | Missing Content-Security-Policy header | 🟡 MEDIUM |
| F5 | Missing X-Frame-Options header | 🟡 MEDIUM |
| F6 | Missing X-Content-Type-Options header | 🟡 MEDIUM |
| F7 | Outdated jQuery version (1.11.3) | 🟢 LOW |
| F8 | phpinfo.php publicly accessible | 🟢 LOW |
| F9 | Missing Referrer-Policy header | 🟢 LOW |
| F10 | No security.txt file | 🟢 LOW |

---

## Key Evidence

**Finding F1 — Live Cookie Proof (Firefox DevTools Console):**
```javascript
> document.cookie
"PHPSESSID=9732e38f06e38d5e232aaaeab2c68e3e; security=low"
```
This confirms the session cookie is fully readable by JavaScript — HttpOnly flag is absent.

**Finding F2 & F3 — curl Header Check:**
```bash
$ curl -sI http://localhost:42001 | grep -iE "server:|strict|x-frame|csp"
Server: Apache/2.4.51 (Debian)
X-Powered-By: PHP/7.4.3
# All security headers: NO OUTPUT (missing)
```

**Finding F7 — Outdated jQuery:**
```bash
$ curl -s http://localhost:42001 | grep -i jquery
<script src='/js/jquery-1.11.3.min.js'></script>
```

---

## Remediation Quick Reference

| Finding | Fix | Time |
|---------|-----|------|
| F1 | `session.cookie_httponly=1` in php.ini | 5 min |
| F2 | Add HSTS header to Apache config | 10 min |
| F3 | `ServerTokens Prod` + `expose_php=Off` | 5 min |
| F4 | Add Content-Security-Policy header | 30 min |
| F5 | `X-Frame-Options: SAMEORIGIN` | 2 min |
| F6 | `X-Content-Type-Options: nosniff` | 2 min |
| F7 | Upgrade jQuery → 3.7.x | 1 hr |
| F8 | Delete phpinfo.php | 1 min |
| F9 | Add Referrer-Policy header | 2 min |
| F10 | Create `/.well-known/security.txt` | 2 min |

**Total estimated fix time: ~2 hours**

---

## How to Reproduce This Audit

### Step 1 — Set up DVWA on Kali Linux
```bash
sudo apt update && sudo apt install dvwa -y
sudo dvwa-start
# Access: http://localhost:42001 | Login: admin / password
```

### Step 2 — Set target variable
```bash
TARGET="http://localhost:42001"
```

### Step 3 — Run all checks
```bash
# Headers
curl -sI $TARGET

# Security header check
for h in "Strict-Transport-Security" "Content-Security-Policy" \
  "X-Frame-Options" "X-Content-Type-Options" "Referrer-Policy"; do
  result=$(curl -sI $TARGET | grep -i "$h")
  [ -z "$result" ] && echo "[MISSING] $h" || echo "[PRESENT] $result"
done

# Sensitive paths
for path in /admin /phpinfo.php /.git/HEAD /robots.txt /.env; do
  CODE=$(curl -s -o /dev/null -w "%{http_code}" "$TARGET$path")
  echo "$CODE  $path"
done

# JS components
curl -s $TARGET | grep -iE "jquery|bootstrap|angular"

# Cookie proof (in Firefox DevTools Console)
document.cookie
```

### Step 4 — Run OWASP ZAP
```bash
zaproxy &
# Tools → Spider → URL: http://localhost:42001 → Start
# Check Alerts tab for findings
```

---

## Learning Outcomes

Through this project I learned to:
- Perform a structured passive security assessment without active exploitation
- Identify and classify web vulnerabilities by risk level (High / Medium / Low)
- Use industry-standard tools: Nmap, curl, OWASP ZAP, Firefox DevTools
- Produce professional vulnerability assessment documentation
- Think like a security auditor — not an attacker

---

## References

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [Mozilla Security Headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)
- [securityheaders.com](https://securityheaders.com)
- [HSTS Preload List](https://hstspreload.org)
- [DVWA Project](https://dvwa.co.uk)
- [RFC 9116 — security.txt](https://www.rfc-editor.org/rfc/rfc9116)

---
