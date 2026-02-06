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
1. Alert details (EventID 320)  
   <img width="975" height="566" alt="Image" src="https://github.com/user-attachments/assets/09893231-10d2-4573-a185-191a56aadb56f" />  
2. IP reputation (AbuseIPDB)  
   <img width="975" height="649" alt="Image" src="https://github.com/user-attachments/assets/dd44ea61-365f-418c-954a-c12a2674e212" />  
3. Process chain showing w3wp.exe → PowerShell spawn  
   <img width="975" height="641" alt="Image" src="https://github.com/user-attachments/assets/49457b87-a0b6-4244-99c6-1a872c65b83e" />  
4. Terminal history with hidden PowerShell and cmd echo  
   <img width="816" height="310" alt="Image" src="https://github.com/user-attachments/assets/5c439298-52f1-44d9-b732-a163ac1821ce" />  
5. Containment enabled on host  
   <img width="975" height="566" alt="Image" src="https://github.com/user-attachments/assets/d7cd2682-c6c2-4292-badd-b38f381e1017" />  
6. File search confirming spinstall0.aspx  
   <img width="975" height="566" alt="Image" src="https://github.com/user-attachments/assets/849a6db1-cec5-46ae-94cc-ca55db5517a2" />  
7. Closed alert – True Positive confirmation  
   <img width="975" height="566" alt="Image" src="https://github.com/user-attachments/assets/another-asset-id-here" />  <!-- Add if you have a 7th -->

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
