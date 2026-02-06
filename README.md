Hi, I'm Chris (@chrisalee27 on X) – building hands-on SOC analyst skills through LetsDefend labs.
# SOC Incident Response Portfolio  
**LetsDefend Labs – Real-World Simulation**

This repository showcases hands-on Security Operations Center (SOC) investigations from LetsDefend labs. Each case includes alert triage, enrichment, endpoint analysis, IOC confirmation, containment decisions, and remediation planning.

## SOC342 – ToolShell SharePoint Zero-Day Exploitation  
**CVE-2025-53770 | Critical Web Attack | True Positive**

**Summary**  
Detected and fully investigated exploitation of the **ToolShell** zero-day vulnerability (CVE-2025-53770) in on-premises SharePoint Server 2019.  
- Attack vector: Unauthenticated POST to /_layouts/15/ToolPane.aspx → unsafe deserialization + remote code execution (RCE)  
- Result: Successful compromise with webshell deployment and ASP.NET MachineKey theft  

**Outcome**: True Positive – Host compromised (successful RCE + persistence)  
**Key Skills Demonstrated**:
- Alert triage and initial enrichment
- External IP reputation analysis (AbuseIPDB, WHOIS)
- Endpoint investigation (process tree, terminal history, file artifacts)
- Recognition of webshell IOCs and post-exploitation chains
- Containment and remediation planning

### Key Findings & IOCs
- **Source IP**: 107.191.58.76 (Vultr / Constant Company VPS hosting)  
  - 26 historical abuse reports clustered in July 2025  
  - Tied to SharePoint CVE-2025-53770 exploitation attempts
- **Target Host**: SharePoint01 (172.16.20.17, Windows Server 2019)
- **Webshell**: spinstall0.aspx (dropped in LAYOUTS path – confirmed classic ToolShell IOC)
- **Exploitation Chain** (from terminal history & process tree):  
  w3wp.exe (IIS worker) → powershell.exe (-nop -w hidden) → csc.exe (compile payload) → cmd.exe (echo malicious ASPX content) → powershell.exe (finalize / extract MachineKey from web.config)

### Evidence  
All screenshots are in the `/screenshots/` folder:  
<img width="1669" height="844" alt="1_alert_details png" src="https://github.com/user-attachments/assets/33d3ea8f-ff46-4ba3-a06c-86db4de4dd1b" />
<img width="891" height="524" alt="2_ip_ownership" src="https://github.com/user-attachments/assets/359112cc-923c-429d-aec4-fe16897e9422" />
<img width="819" height="549" alt="3_ip_reputation_abuseipdb png" src="https://github.com/user-attachments/assets/56137bbc-0dba-49c2-b8b1-5f1f73d63809" />
<img width="817" height="597" alt="4_ip_reputation_abuseipd2_png" src="https://github.com/user-attachments/assets/0010cffd-0b47-4c7c-8ad3-4bd5b25594f9" />
<img width="1104" height="426" alt="5_mailicious_process png" src="https://github.com/user-attachments/assets/094c3b82-e97c-4c22-885a-6a857cf129b5" />
<img width="1225" height="631" alt="6_spinstall_aspx_found png" src="https://github.com/user-attachments/assets/b7d4f437-6567-414b-83fe-9f7971ce2323" />
<img width="998" height="525" alt="7_closed_aler_true_postive png" src="https://github.com/user-attachments/assets/7033d5c4-805c-4f78-bf9d-871f98a2866d" />

### Remediation Recommendations (from lab notes)
- Immediate containment (enabled in simulation)
- Apply Microsoft July 2025 out-of-band patches for SharePoint Server 2019
- Rotate ASP.NET MachineKeys in web.config & restart IIS
- Delete spinstall0.aspx (and variants) from LAYOUTS folders
- Rebuild host from clean backup (assume keys stolen)
- Block 107.191.58.0/24 range & hunt for similar IOCs org-wide

### Files in this Case
- `EventID_320_SOC342.docx` – Full incident report template
- `analyst_notes.md` – Raw investigation notes  
- `/screenshots/` – Visual proof of each investigation step  

This lab exercise closely mirrors Tier 1/Tier 2 SOC analyst responsibilities for critical alerts.
