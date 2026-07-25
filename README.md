# Defensive Security: LetsDefend SOC Alert Triage Portfolio

**Author:** John Gabriel Abitria | Abu Dhabi, UAE  
**Focus:** Incident Triage, Log Analysis, Privilege Escalation & MFA Bypass Investigations

---

## 📌 Executive Summary
This repository contains end-to-end investigation reports for live simulated security alerts on the LetsDefend platform. Each report details the initial trigger, log analysis (Windows Event Logs / Network Telemetry), framework mapping (MITRE ATT&CK), and final incident verdicts.

---

## 📑 Quick Navigation
* [Alert 1: SOC225 - Compromised VPN Connection & MFA Bypass](#-alert-1-soc225---compromised-vpn-connection--mfa-bypass)
* [Alert 2: SOC313 - CVE-2024-39138 Exploitation & Privilege Escalation](#-alert-2-soc313---cve-2024-39138-exploitation--privilege-escalation)

---

## 🚨 Alert 1: SOC225 - Compromised VPN Connection & MFA Bypass

### 1. Incident Summary
* **Date Investigated:** Feb 13, 2024
* **Category:** Unauthorized Access / Identity Compromise
* **Overview:** An alert was triggered for an unauthorized VPN connection originating from a restricted geolocation (Vietnam) targeting the user account `Monica`. The threat actor successfully authenticated via the corporate VPN gateway (`33.33.33.33`) by bypassing multi-factor authentication (MFA) security controls.

### 2. Investigation & Log Triage
* **Timeline Analysis:** The legitimate user was last observed active on the endpoint on Feb 12, 2024, at 04:41 PM.
* **Endpoint Activity:** On Feb 13, 2024, endpoint activity logs indicated suspicious behavior, confirming an interactive session was established while the legitimate user was away.
* **Reconnaissance Execution:** Telemetry confirmed the threat actor actively executed system reconnaissance commands on the internal endpoint (`172.16.17.163`) immediately following the unauthorized VPN authentication.

### 3. Framework Mapping (MITRE ATT&CK)
* **Tactic:** Initial Access, Discovery
* **Technique:** T1133 (External Remote Services), T1087 (Account Discovery)

### 4. Remediation & Conclusion
* **Verdict:** True Positive
* **Action Taken:** The endpoint was successfully contained. No critical infrastructure was compromised, and no successful lateral movement was observed. Forensic backups were completed, and a full system wipe was ordered.

---

## 🚨 Alert 2: SOC313 - CVE-2024-39138 Exploitation & Privilege Escalation

### 1. Incident Summary
* **Date Investigated:** [Insert Date]
* **Category:** Privilege Escalation / Execution
* **Overview:** A security event was generated regarding a malicious executable masquerading as a legitimate binary. The threat actor, operating from a malicious IP address (`185.107.56.141` - Netherlands), targeted the internal endpoint `172.16.17.207` belonging to the user `Victor`, culminating in a successful local privilege escalation exploit.

### 2. Investigation & Log Triage
* **Authentication Brute Force:** The legitimate user logged out at 12:00 PM. At 2:35 PM, logs indicated an automated brute-force attack generating multiple authentication failures showing error code `0xC000006D` (Unknown user name or bad password).
* **Lateral Movement & Access:** Within a minute, the threat actor utilized a compromised Guest account to gain access to `Victor`'s primary account. Windows Security Event ID `4624` was recorded with a Logon Type `10` (RemoteInteractive), indicating successful RDP or terminal access.
* **Failed Privilege Escalation Attempts:** At 2:35 PM, the threat actor attempted to brute-force administrative credentials but failed. Subsequent attempts to probe RDP ports were successfully dropped by the host firewall.
* **Successful Exploitation:** Following the failed brute-force attempts, the threat actor executed a local privilege escalation exploit targeting vulnerability CVE-2024-39138. The exploit successfully granted the attacker administrative execution rights on the local machine.


### 3. Framework Mapping (MITRE ATT&CK)
* **Tactic:** Privilege Escalation, Lateral Movement, Defense Evasion
* **Technique:** T1068 (Exploitation for Privilege Escalation), T1021.001 (Remote Desktop Protocol), T1110 (Brute Force)

### 4. Remediation & Conclusion
* **Verdict:** True Positive
* **Action Taken:** The exploitation was confirmed and the endpoint was immediately isolated from the network. No critical infrastructure was damaged. The incident was escalated to senior analysts for further forensic review.
* **Security Recommendations:** Enforce strict password complexity requirements across the domain, implement continuous monitoring of endpoint configurations for exposed credentials, and strictly enforce the principle of least privilege for all Guest accounts.
