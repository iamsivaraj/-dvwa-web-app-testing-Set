# DVWA Web Application Security Testing

## Overview
Hands-on lab project testing common web application vulnerabilities using DVWA (Damn Vulnerable Web Application), a deliberately vulnerable app built for learning security testing. Conducted in an isolated local environment (Kali Linux, DVWA via Docker) for educational purposes.

## Tools Used
- **DVWA** (Damn Vulnerable Web Application) — target application
- **Docker** — used to run DVWA locally
- **Firefox** — manual testing and payload injection

## Environment
- DVWA run locally via Docker on Kali Linux (`127.0.0.1`)
- Security level set to "Low" to clearly demonstrate each vulnerability
- Isolated local environment — no external or production systems involved

## Vulnerabilities Tested

### 1. SQL Injection
- **What was done:** Submitted a crafted input (`1' OR '1'='1`) into the DVWA SQL Injection form's User ID field instead of a normal numeric value.
- **Result:** The application returned data for multiple users instead of just one, because the input was inserted directly into the underlying SQL query without sanitization.
- **Impact:** An attacker could extract data they should not have access to, or manipulate the query to bypass authentication entirely.

### 2. Cross-Site Scripting (XSS – Reflected)
- **What was done:** Submitted `<script>alert('XSS')</script>` into the DVWA Reflected XSS form's Name field instead of plain text.
- **Result:** The browser executed the injected script, displaying a popup alert — proving the application reflects user input back into the page without sanitizing it.
- **Impact:** In a real attack, this could be used to steal session cookies, hijack accounts, or redirect users to malicious sites.

### 3. Weak Credentials / Brute Force
- **What was done:** Tested the DVWA Brute Force login form with the username `admin` and password `password`.
- **Result:** Login succeeded immediately, confirming the application accepts a weak, easily guessable password with no lockout or rate-limiting in place.
- **Impact:** An attacker could use automated tools to try common passwords rapidly and gain unauthorized access.

## Findings Summary
| Finding | Severity | Description |
|---|---|---|
| SQL Injection | High | User input is inserted directly into SQL queries without sanitization or parameterization, allowing data extraction/manipulation. |
| Reflected XSS | Medium-High | User input is reflected into the page without output encoding, allowing arbitrary script execution in a victim's browser. |
| Weak Credentials | Medium | The application accepts common/weak passwords with no account lockout or rate-limiting. |

## Remediation Recommendations
- Use parameterized queries / prepared statements instead of directly inserting user input into SQL queries.
- Sanitize and encode all user input before rendering it back on a page (output encoding) to prevent XSS.
- Enforce strong password policies and implement account lockout or rate-limiting after repeated failed login attempts.

## Disclaimer
This assessment was performed exclusively against DVWA, an application intentionally built to be vulnerable for training purposes, running in an isolated local environment. No production systems or third-party applications were involved.
