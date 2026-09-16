# RAVEN
Offensive Security — CTF1 Iron Raven
# CIP-A104: Offensive Security — CTF1 Iron Raven

Student: Habeebah Adeyinka Olugbogi  
Reg No: c11.ehit2617337@icdfa.edu.ng  
Cohort: Ethical Hacker Internship  
Course: CIP-A104: Offensive Security  

Target: 192.168.44.129 (Raven / OPFOR-01)  
Attacker: 192.168.44.128 (Kali Linux)  

## Overview

This was a penetration test on the Raven CTF box.  
I finished recon and host checks.  
After that, I tried several routes to get in.  
I could not get a working foothold before the time limit.

## What Was Done

### Phase 1 — Reconnaissance

- Found the target with `nmap -sn 192.168.44.0/24`  
- Checked exposed services and got these results:  
  - 22 (SSH)  
  - 80 (HTTP)  
  - 111 (rpcbind)  
  - 43212 (RPC status)

Phase 2: Enumeration

- Verified Apache 2.4.10 on Debian.
- Located WordPress 4.8.7 under `/wordpress/`.
- Pulled out WordPress usernames: `michael` and `steven`.
- Checked XML-RPC status. It was on.
- Noted that uploads directory listing was open.
- Looked over the RPC related services.

Phase 3: Vulnerability review

- WordPress 4.8.7 was not current.
- XML-RPC being enabled can help with password guessing.
- `/contact.php` looked like a PHPMailer input page at first.
- Later it turned out `/contact.php` was just static HTML.

Phase 4: First access test (tried)

I attempted these steps:

- PHPMailer argument injection tied to CVE-2016-10033.
- Used the Metasploit `phpmailer_arg_injection` module.
- Tried a sendmail `-X` style injection with `${IFS}` and tabs.
- Ran XML-RPC brute force for `michael` and `steven`.

Outcome: I did not get a shell.  
I did write backdoor files into the web root.  
Those files still did not run.

## Tools Used

- nmap
- curl
- wpscan
- Metasploit Framework
- netcat (nc)
- Python 3


## Next Steps (If Resumed)

- Run full `rockyou.txt` brute force against `michael` and `steven` via XML-RPC
- Investigate `/wp-login.php` and `/wp-admin/` for plugin-based vulnerabilities
- Check `/wordpress/wp-content/uploads/` for leftover files
- Investigate RPC services on ports 111 and 43212


## Cleanup

These files were written to the target and must be removed manually:
/var/www/html/JonzbrGv.php
/var/www/html/kveXdJQK.php
/var/www/html/qsTPUbOB.php
/var/www/html/vesVd5rI.php
/var/www/html/r97Ur6uI.php
/var/www/html/pflYTUZd.php
/var/www/html/32YMKKy1.php
/var/www/html/r3.php
/var/www/html/raven.php
/var/www/html/raven_shell.php



## Status

| Phase | Status |
|---|---|
| 0 – Preparation | Complete |
| 1 – Reconnaissance | Complete |
| 2 – Enumeration | Complete |
| 3 – Vulnerability Analysis | Complete |
| 4 – Initial Access | Not Achieved |
| 5 – Post-Exploitation | Not Reached |
| 6 – Objective Collection | Not Reached |
| 7 – Withdrawal | Partial |



*For educational use only. Do not use against systems you do not own or have permission to test.*

