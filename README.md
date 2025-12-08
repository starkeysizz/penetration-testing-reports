# penetration testing reports
Pentest Project — Internal Network Assessment (Target: 192.168.0.43)

Objective: Enumerate attack surface, identify vulnerabilities, obtain system access, and assess privilege escalation paths on an internal Ubuntu host.

1. Reconnaissance & Enumeration

Network Discovery

Identified active hosts on the local subnet using ARP-based scanning.

Confirmed target host (192.168.0.43) running Apple NIC hardware.

Port & Service Enumeration

Performed SYN scan on target. Key exposed services:

22/tcp – SSH (OpenSSH 7.2p2 on Ubuntu 16.04)

25/tcp – SMTP (Postfix)

80/tcp – Apache/2.4.18

Web Enumeration

Used Gobuster to brute-force web directories and file extensions.

Discovered:

/index.html (200 OK)

Multiple restricted Apache config/password files (.htaccess, .htpasswd) → 403 Forbidden

/server-status exposed but access-restricted

Server fingerprint: Apache/2.4.18 on outdated Ubuntu 16.04.

2. Vulnerability Assessment

Nessus Basic Scan Findings

Critical: Unsupported OS (Ubuntu 16.04 EoL) → No security patches, high risk.

Medium: SSH Terrapin Prefix Truncation Weakness (CVE-2023-48795).

Low: ICMP timestamp disclosure.

Additional Observations

Local MySQL server exposed on 127.0.0.1:3306 only.

FTP service technically listening (::21), though no banner retrieved.

SMTP misconfiguration warnings shown in logs.

3. Attack Execution

SSH Assessment

Enumerated SSH version compatibility and confirmed weak configuration.

Credential Attack

Generated a custom wordlist using crunch.

Performed a targeted SSH brute force with Hydra.

Successful login:

User: sharky

Password: sharky123

This demonstrates weak password policy and lack of brute-force protection.

4. Post-Exploitation

Privilege Escalation

Verified sudo rights: the sharky user had full sudo access (ALL:ALL) → immediate root potential.

Enumerated SUID binaries present on the system; findings consistent with an unpatched Ubuntu 16.04 install.

Service/Process Review

Apache, SSH, SMTP, FTP all running.

No anomalous processes, but logs show repeated service restarts and misconfigurations.

Log Analysis

Apache logs confirmed directory enumeration attempts were recorded.

Postfix logs revealed multiple configuration overrides and potential administrative issues.

System logs showed outdated packages, AppArmor resets, and OS-level maintenance problems.

5. Key Takeaways / Impact

Host was running an end-of-life OS, vulnerable by design.

Weak SSH credentials allowed direct access.

Excessive sudo privileges turned any user compromise into full system compromise.

Web server misconfigurations exposed sensitive file patterns (even if protected).

SMTP configuration errors suggested broader security hygiene issues.

6. Portfolio-Ready Summary (What You Actually Showcase)

Performed a full-stack internal assessment against an Ubuntu server. Conducted host discovery, port scanning, web enumeration, and vulnerability scanning. Exploited weak SSH credentials via controlled brute force to gain shell access. Identified misconfigurations, outdated OS, and unrestricted sudo privileges leading to full system compromise. Delivered actionable remediation steps including OS upgrade paths, SSH hardening, and credential policy enforcement.
