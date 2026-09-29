## Mediroza General Hospital — Penetration Testing Project
## NetworkWalks Internship | Batch B083 | Week 4

A full black-box penetration test conducted against a simulated hospital web application as part of my cybersecurity internship with NetworkWalks. This repo documents the methodology, findings, and full report for the engagement.

⚠ Disclaimer: This project was conducted in a controlled, authorised educational environment against a target explicitly set up by NetworkWalks for security testing, with written permission granted for all activities described here. These techniques must never be applied to any system without explicit written authorisation from the owner.


## 📋 Project Overview

Field	Details

Client	      Mediroza General Hospital (simulated)

Target	      https://medirozahospital.com

Scope	Target  domain only — no social engineering, no DoS

The engagement was broken into 4 milestones:

1.	M1 — Initial Access: Attack the website and retrieve 3 confidential PDF patient lab reports.

2.	M2 — Data Extraction: Crack the encryption on all 3 retrieved files.

3.	M3 — Critical Exposure: Find and document a further critical data exposure on the server.

4.	M4 — Reporting: Produce a full professional penetration testing report.
 
## 🛠 Tools Used
Tool	Purpose

whois

Domain registration recon

nslookup / dnsrecon	DNS resolution & records

whatweb

Web server / tech stack fingerprinting

theHarvester

Subdomain & email enumeration

wafw00f

WAF detection

Firefox (manual browsing)	Site mapping, robots.txt / sitemap.xml review

NetworkWalks Password Cracker	Offline PDF password cracking (dictionary attack)
 
## 🔍 Methodology
1.	Reconnaissance — passive info-gathering (WHOIS, DNS, subdomains, WAF fingerprinting) to map the target before any direct interaction.

2.	Enumeration — manually reviewed robots.txt and sitemap.xml , which disclosed undocumented directories ( /patient/ , /staff/ , /old/ ) not linked anywhere on the live site.

3.	Exploitation — used the enumerated paths to identify and exploit an exposed backup and an authentication bypass vulnerability.

4.	Post-Exploitation — extracted and cracked the confidential files retrieved during exploitation.

5.	Reporting — consolidated all findings into a professional report with reproduction steps, evidence, risk ratings, and remediation guidance.
 
## 🚨 Findings Summary

#	Finding	Risk Rating

1	 Sensitive paths disclosed via robots.txt	Low

2	 Publicly accessible historic database backup ( /old/ )	Critical

3	 SQL Injection — authentication bypass on Patient Portal	Critical

4	 Weak, dictionary-crackable password protection on confidential PDFs	High

# Finding 2 — Exposed Database Backup

An unauthenticated, publicly downloadable .sql backup ( mediroza_db_backup_2019.sql ) exposed full staff PII (national ID numbers, phone numbers, salaries) for 30 employees, plus confidential shareholder equity data — with no authentication required to access it.

# Finding 3 — SQL Injection Authentication Bypass

The Patient Portal login form ( /patient/login.php ) passed unsanitised user input directly into a SQL query. Submitting admin'-- as the username bypassed the password check entirely, granting full access to confidential patient lab reports with no valid credentials.

# Finding 4 — Weak PDF Password Protection

The three retrieved lab reports were passwordprotected, but weakly: two were cracked within the first couple of attempts using a small default wordlist ( 123456 , password ), while the third required a larger custom wordlist ( !@#$%^& , recovered after 3,546 attempts). (Full details, reproduction steps, and evidence for each finding are in the penetration testing report.)

## 🧠 Key Takeaways

Small, "boring" misconfigurations — a robots.txt entry, an old backup left in a public folder — are often what actually break an application open, not exotic zero-days.
Unsanitized user input in a single login form was enough to compromise the entire patient portal.  	Encryption is only as strong as the password behind it — strong algorithms mean nothing paired with weak, guessable passwords.Vulnerabilities rarely matter in isolation; it's the chain of small issues that gives an attacker full compromise.
 
## 📄 Report
The full professional penetration testing report — including all findings, screenshots, risk ratings, and remediation recommendations — is included in this repo: Mediroza_Pentest_Report.docx

## 🙏 Acknowledgements
Thanks to Mr. Waqas Karim and the NetworkWalks team for building a lab environment that mirrors a real-world engagement rather than a simplified CTF puzzle.
