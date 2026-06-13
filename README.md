# Phishing Email Detection & Awareness System

**Future Interns — Cyber Security Task 2 (2026)**

Prepared by: **Atul** | Roll No: 2023003147
B.Tech CSE — Cyber Security, GITAM University, Hyderabad

---

## 📌 Overview

This repository contains a complete phishing email detection and awareness
analysis. Working in the role of a security analyst, four real-world style
phishing email samples were collected, analyzed for indicators of compromise,
classified by risk level, and documented in a professional client-ready
report — along with prevention and awareness guidelines for employees.

**This is security education and analysis — not hacking.** No real
organizations, individuals, or systems were targeted. All domains and
infrastructure referenced (e.g. `attacker-server.com`, `192.0.2.145`) are
either constructed examples or reserved documentation ranges (RFC 5737
TEST-NET).

---

## 📁 Repository Structure

```
├── report/
│   └── Phishing_Detection_Report_Atul_GITAM.docx   ← Full report (deliverable)
├── samples/
│   ├── sample1_amazon_verification.txt             ← Amazon account scam
│   ├── sample2_sbi_otp_theft.txt                   ← SBI bank OTP scam
│   ├── sample3_it_helpdesk_reset.txt               ← IT helpdesk impersonation
│   └── sample4_wellsfargo_spoofing_headers.txt     ← Wells Fargo header spoofing
├── evidence/
│   └── sample4_google_toolbox_spf_fail.png         ← Header analyzer screenshot
└── README.md
```

---

## 🛠 Tools Used

| Tool | Purpose |
|---|---|
| [Google Admin Toolbox — Messageheader](https://toolbox.googleapps.com/apps/messageheader/) | Parses raw email headers, evaluates SPF/DKIM/DMARC, visualizes delivery route |
| [MXToolbox Email Header Analyzer](https://mxtoolbox.com/EmailHeaders.aspx) | Cross-checks header authentication results and sender IP reputation |
| [PhishTank](https://phishtank.org) / [OpenPhish](https://openphish.com) | Public phishing URL/sample feeds used for indicator research |
| [WHOIS (who.is)](https://who.is) | Domain registration age, registrar, ownership lookups |
| [MXToolbox SuperTool](https://mxtoolbox.com/SuperTool.aspx) | SPF and DMARC record lookups for sender domains |
| [URLScan.io](https://urlscan.io) | Safe sandboxed analysis of suspicious URLs |
| [VirusTotal](https://virustotal.com) | URL/file reputation scanning against 70+ AV engines |
| Kali Linux CLI (`whois`, `dig`, `curl`, `nslookup`) | Command-line domain, DNS, and link investigation |
| Microsoft Word (via `docx` library) | Final report generation and formatting |

---

## 🔍 Analysis Approach

The investigation followed a structured 7-step workflow used by real security
analysts when triaging suspected phishing emails:

1. **Collect samples** — Gathered 4 phishing email samples representing
   common attack patterns (brand impersonation, banking fraud, internal IT
   impersonation, and domain spoofing).

2. **Analyze headers** — Extracted and reviewed `From`, `Reply-To`,
   `Return-Path`, `Received`, and `Authentication-Results` fields using the
   Google Admin Toolbox and MXToolbox header analyzers.

3. **Inspect sender domain & links** — Ran WHOIS, DNS (`dig`), SPF, and
   DMARC lookups on sender domains. Used `curl -IL` to safely trace link
   redirects without visiting pages directly.

4. **Identify phishing indicators** — Applied a standardized checklist
   covering sender/domain red flags, content/language red flags, and
   link/attachment red flags.

5. **Classify risk** — Each sample was scored against the indicator
   checklist and assigned one of three classifications:
   - **Safe** — legitimate, verified sender and content
   - **Suspicious** — some red flags present, requires verification
   - **Phishing** — multiple confirmed indicators, malicious intent

6. **Document findings** — Each sample was documented with the raw
   evidence, indicator table, plain-language explanation of the attack
   ("How This Attack Works"), and final risk classification.

7. **Create awareness guidelines** — Synthesized findings into the
   **SLAM method** (Sender, Links, Attachments, Message), a Do's & Don'ts
   table for employees, an incident response checklist for users who click
   a phishing link, and recommended technical defenses (SPF, DKIM, DMARC,
   MFA) for organizations.

---

## 📊 Summary of Findings

| Sample | Subject | Attack Type | Risk Level |
|---|---|---|---|
| 1 | Amazon Account Verification | Credential Harvesting | **HIGH** |
| 2 | SBI Account Suspended (OTP request) | Banking Fraud / OTP Interception | **CRITICAL** |
| 3 | IT Helpdesk Password Reset | Corporate Credential Theft | **HIGH** |
| 4 | Wells Fargo Security Alert (header spoofing) | Domain Spoofing / Brand Impersonation | **CRITICAL** |

**Common pattern across all 4 samples:**
- Urgency and fear-based language
- Lookalike or spoofed sender domains
- Missing or failed SPF / DKIM / DMARC
- Requests for credentials, OTPs, or clicks on disguised links

---

## 📄 Report Contents

The full report (`report/Phishing_Detection_Report_Atul_GITAM.docx`) includes:

- Introduction to phishing and attack types
- Detailed analysis of all 4 samples (indicators, attack explanation, risk classification)
- Risk classification framework (Safe / Suspicious / Phishing)
- Master phishing indicator checklist
- Prevention & awareness guidelines (SLAM method, Do's & Don'ts, incident response steps)
- Technical defense recommendations for organizations (SPF, DKIM, DMARC, MFA)

---

## ⚠️ Disclaimer

All content in this repository is for **educational and awareness purposes
only**, created as part of the Future Interns Cyber Security Task 2
internship program. No real systems were accessed, attacked, or exploited.
