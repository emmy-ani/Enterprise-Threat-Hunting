# Threat Hunting & Log Analytics Lab

## 📌 Overview
This repository demonstrates practical threat hunting, log analysis, and forensic investigation techniques across Windows and Linux environments. Using enterprise-style telemetry sources—including Windows Security Event Logs, Sysmon, PowerShell ScriptBlock Logging, Apache access logs, and Linux authentication records—the project focuses on identifying attacker behavior, reconstructing attack timelines, and correlating security events commonly investigated in Security Operations Centers (SOC)

## 🛠️ Key Competencies & Technologies
### Core Competencies
* Threat Hunting & Incident Response Investigation
* Security Event Correlation & Digital Forensics
* Windows Event Log & Sysmon Telemetry Analysis
* Linux Log Analysis & Apache Log Investigation
* Privilege Escalation Detection & Authentication Auditing
* Threat Timeline Reconstruction

### Tools & Technologies Used
* **Windows Ecosystem:** PowerShell ScriptBlock Logging, Windows Security Event Logs, Sysmon, Active Directory Auditing
* **Linux Ecosystem:** `grep`, `awk`, `sed`, Regular Expressions (Regex), syslog, `auth.log`
* **Web Services:** Apache Access Logs, SSH Authentication Logs

---

## 📂 Investigation Modules

### 🛡️ Module 1: Windows Domain Controller Compromise Investigation
* **Objective:** Investigate a multi-stage intrusion targeting a Windows Server Domain Controller and reconstruct the attack chain through event log correlation and host telemetry analysis.

#### 📊 Key Findings
* **Initial Access:**
  * Identified 150 failed authentication attempts targeting the local `Administrator` account from source IP `10.10.50.99`.
  * Analyzed Windows Security Event ID `4625` records to establish brute-force activity.
* **Credential Abuse & Lateral Movement:**
  * Detected Logon Type `9` authentication events associated with Pass-the-Hash activity.
  * Tracked lateral movement targeting: `SRV-FILE02`, `SRV-SQL04`, `WS-FINANCE01`, and `WS-MGMT04`.
* **Credential Dumping & Obfuscation:**
  * Identified LSASS access activity consistent with credential dumping techniques.
  * Analyzed Base64-encoded PowerShell payloads and decoded obfuscated commands from ScriptBlock logs.
* **Ransomware Preparation:**
  * Detected shadow copy deletion attempts through execution of `vssadmin.exe delete shadows`.
  * Observed lateral execution activity through `PsExec.exe`.

* **📄 Technical Report:** [View Full Windows Forensic Briefing](./report/Powershell_Soc_script.pdf)

---

### 🛡️ Module 2: Linux Web Server Breach Investigation
* **Objective:** Investigate a compromised Linux web server environment to identify web-based attacks, unauthorized access, and privilege escalation activity.

#### 📊 Key Findings
* **SSH Brute-Force Activity:**
  * Identified 260 failed SSH authentication attempts from source IP `198.51.100.22`.
  * Correlated failed and successful login events to establish attacker access timelines.
* **Web Shell Detection:**
  * Analyzed Apache access logs for suspicious command execution patterns.
  * Detected malicious web requests targeting: `shell.php?cmd=`.
  * Utilized Regex-based filtering to isolate automated web shell actions.
* **Privilege Escalation:**
  * Investigated `sudo` execution logs and kernel process execution records.
  * Identified unauthorized elevation resulting in an interactive `/bin/bash` session running completely as `root`.

* **📄 Technical Report:** [View Full Linux Forensic Briefing](./report/Essential%20Linux%20and_soc_lab_task.pdf)

---

## 📈 Security Recommendations

### 🖥️ Windows Environment
1. **Privilege Restraints:** Monitor and restrict use of high-risk privileges such as `SeDebugPrivilege` and `SeImpersonatePrivilege`.
2. **Endpoint Hardening:** Generate real-time alerts on any unauthorized handle access to `lsass.exe`.
3. **PowerShell Auditing:** Enforce transcription logging and alert on suspicious PowerShell execution parameters or encoded commands.

### 🐧 Linux Environment
1. **Access Control:** Enforce robust SSH key-based authentication and globally disable direct interactive root login options.
2. **Least Privilege:** Implement tight least-privilege boundaries through strictly scrutinized and configured `sudoers` policies.
3. **Telemetry Analysis:** Continuously monitor local authentication logs for sudden brute-force patterns or anomalous privilege escalation attempts.

### 🗄️ Centralized Logging & Strategy
* Aggregate all Windows and Linux infrastructure telemetry into a centralized logging platform or SIEM tool.
* Retain immutable audit trails for authentication events, process creation logs, privilege escalation activity, and administrative actions.
* Establish core correlation rules to drastically accelerate incident response workflows and discovery capabilities.

---

## 📦 Repository Structure
```text
Enterprise-Threat-Hunting/
└── report/
    ├── Powershell_Soc_script.pdf              <-- Windows Forensic Briefing
    └── Essential Linux and_soc_lab_task.pdf   <-- Linux Forensic Briefing
