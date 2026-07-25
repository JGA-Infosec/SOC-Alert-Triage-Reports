# Defensive Security: LetsDefend SOC Alert Triage Portfolio

**Author:** John Gabriel Abitria | Abu Dhabi, UAE  
**Focus:** Incident Triage, Log Analysis, Privilege Escalation & MFA Bypass Investigations

---

## Executive Summary
This repository contains investigation reports for live simulated security alerts on the LetsDefend platform. Each report details the initial trigger, log analysis (Windows Event Logs / Network Telemetry), and final incident verdicts based on manual log triage.

---

## Quick Navigation
* [Alert 1: Event ID 225 - Compromised VPN Connection (MFA Bypass)](#-alert-1-event-id-225---compromised-vpn-connection-mfa-bypass)
* [Alert 2: Event ID 313 - CVE-2024-39138 Malicious Executable](#-alert-2-event-id-313---cve-2024-39138-malicious-executable)

---

## Alert 1: Event ID 225 - Compromised VPN Connection (MFA Bypass)

### 1. Alert Overview
* **Incident:** Established Compromised VPN connection from unauthorized country (Vietnam).
* **Target Endpoint:** Monica (Internal IP: `172.16.17.163`)
* **Attacker Source:** VPN gateway `33.33.33.33`

### 2. Investigation & Log Triage
* **MFA Bypass:** The attacker managed to get unauthorized access with the VPN gateway and was logged bypassing multi-factor authentication security.
* **Timeline Analysis:** The logs show that Monica was last seen active on Feb 12, 2024, at 04:41 PM. 
* **Suspicious Activity:** Endpoint activity logs showed suspicious activity on Feb 13, 2024, with the attacker running reconnaissance commands on the endpoint's machine.

### 3. Conclusion & Remediation
* **Verdict:** True Positive Incident.
* **Action Taken:** The endpoint has now been contained. No critical infrastructure was compromised, and no successful lateral movement was done. Backups have been done and a full wipe was ordered.

---

## Alert 2: Event ID 313 - CVE-2024-39138 Malicious Executable

### 1. Alert Overview
* **Incident:** CVE-2024-39138 Malicious executable masquerading as a binary execution detected.
* **Target Endpoint:** Victor (Internal IP: `172.16.17.207`)
* **Attacker Source:** `185.107.56.141` (Netherlands)

### 2. Investigation Findings
* **Authentication Brute Force:** The threat actor was logged bruteforcing or using an automation tool to bypass user login, giving error codes at around 2:35 PM (Victor was already logged out of the endpoint at 12 PM). Error generated: `0xC000006D` (Unknown user name or bad password).
* **Lateral Movement:** Not even a minute after, the guest account that the threat actor was in already gained access to Victor's account via lateral movement. **EventID 4624** (An account was successfully logged on) triggered with a **Logon Type of 10 (RemoteInteractive)**, suggesting the threat actor had RDP access or terminal login.
* **Privilege Escalation Attempts:** In the same 2:35 PM time frame, the threat actor tried to escalate privileges into an admin account but was unable to crack the correct credentials (generating more `0xC000006D` errors).
* **Network Probing:** The source IP probed RDP ports after failing to gain an admin account but was dropped by the firewall.
* **Successful Exploitation:** The threat actor was able to execute local privilege escalation exploit code targeting the **CVE-2024-39138** vulnerability, executing admin exploits via local vulnerability exploitation. 
* **Containment:** The victim's machine showed indicators of compromise and was immediately isolated from the network.

### 3. Conclusion & Recommendations
* **Verdict:** True Positive.
* **Action Taken:** Exploitation was confirmed, endpoint is now isolated, and no critical infrastructure was damaged. The alert has been escalated to seniors.
* **Recommendations:** 
  * Require employees to have strong passwords.
  * Consistent monitoring of endpoint configurations to ensure no exposed passwords or misconfigurations are evident.
  * Enforce least privilege for guest users.
