# Endpoint Incident Response and Lateral Movement Analysis (MDR Simulation)

## Overview
This project presents a full incident response investigation conducted in a simulated Managed Detection and Response (MDR) environment. The analysis focuses on identifying a multi-stage malware infection, tracing lateral movement, and assessing impact across systems.

The investigation was based on Process Monitor (Procmon) telemetry capturing approximately 22 minutes of system activity during the incident window.

---

## Scenario
A potential security incident was reported involving a non-corporate device used by an executive's family member. There was concern that malicious activity on this system may have impacted sensitive files and spread across the network.

A Process Monitor (PML) file was provided for analysis, covering activity from:
- **Date:** November 28, 2016  
- **Time Window:** 11:29 AM – 11:51 AM  

---

## Investigation Objectives
- Identify the infection chain  
- Determine the initial infection vector  
- Confirm lateral movement  
- Assess system and data impact  
- Provide remediation recommendations  

---

## Key Findings

### Initial Infection
- Infection began with manual execution of `gift-card.exe` by a local administrator via `Explorer.exe`  
- The executable functioned as a **dropper**, deploying a second-stage payload  
- Administrative privileges enabled unrestricted system access  

### Second-Stage Malware
- Payload: `hwvbgrmymssj.exe` written to the Windows directory  
- Performed:
  - System reconnaissance  
  - Network scanning  
  - File creation and modification  
- Generated high volumes of activity in:
  - `C:\Python27\Lib\` (indicating staging/unpacking behavior)

---

## Malicious Activity Observed
- Execution of dropper and second-stage payload  
- Authentication to remote host **ADMIN-21139**  
- Traversal of sensitive directories:
  - `.bash_history`
  - `.atom`
  - `.azure`
  - `.gitconfig`
- Creation of ransomware-style files:
  - `_ReCoVeRy_+ergfw.*`
- Interaction with Sysinternals metadata files (`Eula.txt`, `psversion.txt`)  

These behaviors indicate a full attack lifecycle including execution, persistence, reconnaissance, lateral movement, and data impact.

---

## Lateral Movement
- Successful authentication to remote host **ADMIN-21139**  
- Accessed administrator profile directories containing potential credentials and tokens  
- Performed directory traversal and file modifications  
- Created ransomware-style artifacts across multiple directories  

This confirms **full lateral movement and compromise of a secondary host**.

---

## Impact Assessment

### Local System
- Malware executed with administrative privileges  
- Second-stage payload established persistent system access  
- Evidence of staging activity via Python-related file operations  

### Remote System
- Confirmed unauthorized access to sensitive directories  
- Creation of ransomware-style files across multiple locations  
- Data modification and potential credential exposure  

---

## Attack Chain Summary
1. User executes `gift-card.exe`  
2. Dropper deploys `hwvbgrmymssj.exe`  
3. Malware performs reconnaissance and staging  
4. Authenticates to remote host `ADMIN-21139`  
5. Traverses sensitive directories  
6. Creates ransomware-style files and modifies data  

---

## MITRE ATT&CK Mapping
- T1078 – Valid Accounts  
- T1021 – Remote Services (Lateral Movement)  
- T1105 – Ingress Tool Transfer  
- T1059 – Command Execution  
- T1486 – Data Encrypted for Impact  

---

## Tools Used
- Sysinternals Process Monitor (Procmon)  
- Windows endpoint forensic analysis  
- Manual log and artifact analysis  

---

## Remediation Recommendations
- Reimage affected systems to remove persistence  
- Reset all credentials associated with compromised accounts  
- Rotate exposed API keys, tokens, and developer secrets  
- Restrict or remove local administrator privileges  
- Deploy Endpoint Detection and Response (EDR) solutions  
- Enforce least privilege access controls  
- Provide security awareness training for high-risk users  

---

## Outcome
This investigation confirmed a multi-stage malware infection involving lateral movement and ransomware-style file impact. The project demonstrates practical SOC and MDR capabilities, including attack chain reconstruction, endpoint analysis, and risk-based remediation.

---

## Author
**Maxine Jones**  
Cybersecurity Analyst | SOC | Incident Response | Threat Detection
