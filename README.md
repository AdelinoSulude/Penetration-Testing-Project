# Penetration-Testing-Project
Mediroza General Hospital

# Mediroza Hospital — Authorized Penetration Testing Project

## NetworkWalks Cybersecurity Internship

---

## Project Overview

This project documents my work on an authorized penetration testing lab against the Mediroza Hospital web application.

**Assessment Type:** Authorized penetration testing lab

The assessment focused on reconnaissance, application enumeration, authentication testing, and validation of input-handling vulnerabilities.

> **Important:** No patient records, passwords, session cookies, or other sensitive data are included in this repository.

---

## Objectives

The main objectives of the assessment were to:

- Conduct reconnaissance against the target.
- Identify exposed services and application entry points.
- Enumerate publicly accessible web resources.
- Identify the Patient Portal.
- Analyse the authentication mechanism.
- Test user input handling for SQL injection.
- Attempt to validate an authentication bypass.
- Document findings and limitations.

---

## Tools Used

The following tools and techniques were used:

- WHOIS
- `nslookup`
- `curl`
- WhatWeb
- wafw00f
- dnsrecon
- Nmap / Zenmap
- GHDB search queries

---

# Methodology

The assessment followed a progressive approach:

1. Passive and active reconnaissance
2. DNS and technology enumeration
3. Web resource discovery
4. Application entry-point identification
5. Documentation of evidence and limitations

---

# 1. Reconnaissance

## 1.1 WHOIS Enumeration

Command:

```bash
whois medirozahospital.com
```

### Observed Information

- Domain: `MEDIROZAHOSPITAL.COM`
- Registrar: NameCheap, Inc.
- Creation date: `2026-08-14`
- Updated date: `2026-08-14`
- Expiry date: `2027-08-14`
- Nameservers:
  - `dns1.namecheaphosting.com`
  - `dns2.namecheaphosting.com`

This established the domain registration and authoritative DNS infrastructure.

---

## 1.2 DNS Enumeration

Command:

```bash
nslookup medirozahospital.com
```

### Result

The domain resolved to:

```text
199.188.201.16
```

Additional DNS enumeration was performed with:

```bash
dnsrecon -d medirozahospital.com
```

### Observed Records

- A record: `199.188.201.16`
- NS:
  - `dns1.namecheaphosting.com`
  - `dns2.namecheaphosting.com`
- MX:
  - `mx1-hosting.jellyfish.systems`
  - `mx2-hosting.jellyfish.systems`
  - `mx3-hosting.jellyfish.systems`
- SPF record was identified.
- DMARC was present with policy `p=none`.
- CalDAV/CardDAV SRV records were identified.

---

## 1.3 Technology Enumeration

Command:

```bash
whatweb medirozahospital.com
```

### Observed Technologies

- LiteSpeed
- HTML5
- HTTPS redirection
- `x-turbo-charged-by: LiteSpeed`

HTTP requests also showed intermediary/proxy behavior, including OpenResty in some responses.

---

## 1.4 HTTP Header Analysis

Command:

```bash
curl -I https://medirozahospital.com
```

Observed headers included:

```text
server: openresty/1.31.1.1
content-type: text/html
cache-control: private, no-cache, no-store, must-revalidate, max-age=0
cf-edge-cache: no-cache
```

The server behavior was different in some authenticated/application requests, where LiteSpeed and PHP were exposed.

---

## 1.5 WAF Detection

Command:

```bash
wafw00f https://medirozahospital.com
```

No generic WAF was detected by wafw00f.

However, browser and HTTP behavior showed an intermediary challenge/protection mechanism. Therefore, the wafw00f result was not treated as proof that the application had no protection layer.

---

# 2. Sitemap Enumeration

The following resource was checked:

```text
https://medirozahospital.com/sitemap.xml
```

The sitemap returned HTTP 200 and exposed the following pages:

```text
/index.html
/about.html
/doctors.html
/contact.html
```

These pages were subsequently confirmed to be accessible.

---


# GHDB Reconnaissance

The following searches were performed:

```text
site:medirozahospital.com filetype:pdf
```

```text
site:medirozahospital.com (filetype:bak OR filetype:sql OR filetype:zip)
```

```text
site:medirozahospital.com intitle:"index of"
```

No useful publicly indexed patient PDFs, database backups, or directory listings were identified through these searches.

---

# Network Scanning

Nmap was used to perform a top-port scan against the identified IP address:

```bash
nmap -T4 --top-ports 20 199.188.201.16
```

### Observed Results

```text
80/tcp   open     http
443/tcp  open     https
```

The remaining tested top ports were reported as filtered.

This established that the primary externally accessible services identified during the scan were HTTP and HTTPS.

---

# Limitations

The main limitation encountered was the authentication stage.

The SQL injection vulnerability was confirmed, but I was unable to achieve a reliable authentication bypass during the assessment.

Because the restricted portal could not be accessed successfully:

- The three confidential PDF reports were not retrieved.
- PDF password recovery was not performed.
- Metadata analysis of the retrieved reports was not performed.
- The further critical server exposure was not investigated.
- Hospital employee salary data was not obtained.
- Shareholder information was not obtained.

These items were therefore **not claimed as completed findings**.

---

# Milestone Progress

| Milestone | Status | Result |
|---|---|---|
| M1 — Reconnaissance & Authentication Testing | Partially completed | Reconnaissance, application enumeration and SQL injection validation completed |
| M2 — PDF Encryption Recovery | Not completed | PDFs were not retrieved |
| M3 — Critical Data Exposure | Not completed | Dependent on successful retrieval/access to further data |
| M4 — Penetration Test Report | Documentation prepared | Based only on evidence actually obtained |

---

---

# Final Conclusion

The assessment successfully identified the Mediroza Hospital Patient Portal and allowed analysis of its authentication mechanism.

However, an authentication bypass was not successfully achieved. As a result, the subsequent milestones requiring access to protected PDF reports and further server-side data exposure could not be completed.

This repository therefore documents only the work and findings that were actually achieved during the assessment.

## 👤 Author

This CyberLab was created and documented by **Adelino Sulude**
for hands-on cybersecurity practice.

**LinkedIn:** [Adelino Sulude](https://www.linkedin.com/in/adelino-sulude/)

## 🙏 Credits

The training and lab concepts were learned from:

- **Waqas Karim** — Cybersecurity Professional, CCIE
  - Instructor of the cybersecurity training used as a learning reference.
  - **LinkedIn:** [Waqas Karim](https://www.linkedin.com/in/waqaskarim/)

All lab configurations, testing, documentation, and practical experimentation
were performed by me in my own virtual lab environment.

## 📌 Project Information
Program Name: Cybersecurity at Networkwalks | Week: 04 | Project: Penetration-Testing-Project | Repository: GitHub
