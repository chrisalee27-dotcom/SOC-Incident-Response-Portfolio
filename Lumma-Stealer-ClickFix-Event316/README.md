SOC Analyst Report: Investigated Alert - Lumma Stealer Infection via ClickFix Phishing

**Executive Summary**

On March 13, 2025, at approximately 09:44 AM, an alert was triggered (Event ID: 316, Rule: SOC338 - Lumma Stealer - DLL Side-Loading via Click Fix Phishing) indicating a potential malware infection on the host associated with user Dylan (dylan@letsdefend.io, IP: 172.16.17.216). The incident involved a phishing email masquerading as a free Windows 11 Pro upgrade, leading to a fake update website that tricked the user into executing obfuscated PowerShell and mshta.exe commands. These commands downloaded and executed a malicious payload from a suspicious domain, resulting in connections to a command-and-control (C2) server associated with Lumma Stealer, an information-stealing malware.

The host was successfully contained via Endpoint Detection and Response (EDR) tools. Investigation confirmed the involvement of Lumma Stealer, a Malware-as-a-Service (MaaS) infostealer targeting credentials, browser data, cryptocurrency wallets, and other sensitive information. No evidence of data exfiltration was observed in the logs, but the malware's capabilities pose a high risk. Recommendations include user awareness training, enhanced email filtering, and full system remediation.

**Severity:** Critical
**Status:** Resolved (Host Contained) 
**Analyst:** Christopher Lee
**Alert Details**

**•	Event Time:** March 13, 2025, 09:44 AM
**•	Rule Triggered:** SOC338 - Lumma Stealer - DLL Side-Loading via Click Fix Phishing
<img width="1600" height="452" alt="Tag 061617" src="https://github.com/user-attachments/assets/a65194b6-8d24-47b3-8dc9-f540e1f5345a" />

**•	Affected User/Host:** Dylan (dylan@letsdefend.io), Internal IP: 172.16.17.216
<img width="1619" height="752" alt="Email Located 055402" src="https://github.com/user-attachments/assets/0e6ac8f8-aeef-477e-a0e3-579b1c296987" />

**•	Source: **Phishing email from update@windows-update.site (SMTP Address: 132.232.40.201) 
<img width="1914" height="965" alt="URL 064719" src="https://github.com/user-attachments/assets/28578d11-74cd-402d-8ff3-a8eef67d9e83" />

**•	Subject:** "Upgrade your system to Windows 11 Pro for FREE"

**•	Action Taken:** Email allowed; host contained post-execution

**•	Initial Indicators:**

o	Browser access to https://windows-update.site/

o	Execution of suspicious commands via PowerShell and mshta.exe

<img width="611" height="190" alt="Malicious Command 060027" src="https://github.com/user-attachments/assets/9510c613-96f0-4b85-9e47-85c333d91f0e" />

<img width="616" height="146" alt="Malicious Command 060011" src="https://github.com/user-attachments/assets/8de96c83-4c89-4f28-94e5-e4e000630206" />

o	Network connections to 132.232.40.201 (C2 IP)

<img width="895" height="515" alt="IP Matched Time 055546" src="https://github.com/user-attachments/assets/a55a607f-3fa9-4098-ad96-17948be3f4a7" />

The email was received and opened, leading to the fake website displaying a convincing Windows update interface with a countdown timer and an "UPDATE NOW" button. This is consistent with ClickFix campaigns, where users are prompted to paste and run malicious commands to "complete" the update.

**Investigation Steps and Findings**

**Step 1:** Email Security Review

•	The phishing email was flagged but allowed through filters.

•	Sender: update@windows-update.site (spoofed Microsoft branding)

•	Content: Lured the user to a fake upgrade page for Windows 11 Pro, claiming no downloads or complex installations required.

•	Similar emails observed in logs, but this was the only one targeting Dylan.

**Step 2:** Endpoint and Command Line Analysis

•	Browser history confirmed access to https://windows-update.site/ at 2025-03-13 23:26:08. 

<img width="1213" height="300" alt="Browser History Time Stamp055258" src="https://github.com/user-attachments/assets/a56acf08-0c73-4823-a023-a451be1c6138" />

**•	Executed Commands:**

<img width="611" height="190" alt="Malicious Command 060027" src="https://github.com/user-attachments/assets/9510c613-96f0-4b85-9e47-85c333d91f0e" />

<img width="616" height="146" alt="Malicious Command 060011" src="https://github.com/user-attachments/assets/8de96c83-4c89-4f28-94e5-e4e000630206" />

**•	Analysis:**The PowerShell command uses obfuscation (e.g., replacing ']' to form "mshta.exe") to bypass detection and download/execute a payload disguised as "maloy.mp4" (likely an HTA file or DLL for side-loading). mshta.exe is abused to run remote scripts, a common tactic in ClickFix attacks.

•	The payload is associated with Lumma Stealer, confirmed by threat intelligence tagging in logs.

**Step 3: Network Activity Review**

•	Connections logged to: 132.232.40.201 C2 communication; tagged as Lumma stealer

<img width="895" height="515" alt="IP Matched Time 055546" src="https://github.com/user-attachments/assets/17d8df58-9e43-4ffa-bec7-7b0f785e699f" />

**•	Primary suspicious connection: **132.232.40.201 (Tencent Cloud, China; ASN: AS45090). This IP has a history of abuse, including IoCs reported by the Government of India and brute-force attempts. Confidence of abuse: 0%, but flagged in multiple threat feeds as Lumma C2.
**Step 4: Threat Intelligence Enrichment**
**•	Domain Analysis:**

<img width="1316" height="568" alt="Virustotal 062334" src="https://github.com/user-attachments/assets/eb2f4d64-dd64-4176-bf30-8ca772f4274c" />

**o	windows-update.site:** Flagged malicious by 8/94 vendors on VirusTotal (e.g., BitDefender, Kaspersky, Sophos as Malware). Part of fake Windows update scams in ClickFix waves.

**o	overcoatpassably.shop:** Hosts malware payloads; linked to Lumma distribution via ANY.RUN and Joe Sandbox analyses (malicious activity confirmed).

**•	Malware Profile:** Lumma Stealer (aka LummaC2) is a C++/ASM-based infostealer sold as MaaS since 2022. Capabilities include stealing browser credentials, crypto wallets, 2FA extensions, and exfiltrating data to C2 servers. It uses evasion techniques like ChaCha20 encryption and frequent updates. Distributed via phishing, fake CAPTCHAs, and cracked software.

•	No active site content retrieved (503 error), but historical data confirms fake update interface

**Step 5: Sandbox and Containment Verification**

•	URL/Attachment analyzed in third-party sandboxes (e.g., VirusTotal, URLhaus): Confirmed malicious.

•	Host containment successful; no further processes or persistence observed.

Recommendations

**1.	Immediate Actions:**

o	Reset credentials for affected user (Dylan) and scan for exfiltrated data.

o	Full malware scan and wipe/reimage the contained host.

**2.	Preventive Measures:**

o	Enhance email filters to block spoofed Microsoft domains and suspicious attachments.

o	Implement user training on phishing recognition, especially fake updates and CAPTCHAs.

o	Enable automatic updates and block execution of mshta.exe/PowerShell from untrusted sources.

o	Monitor for similar IoCs across the network.

**3.	Long-Term:**

o	Deploy advanced threat protection (e.g., behavioral analysis) to detect ClickFix tactics.

o	Regularly review abuse reports for hosted IPs.

**Conclusion**

This incident highlights the evolving tactics of Lumma Stealer distributors using ClickFix phishing to bypass traditional defenses. Prompt containment mitigated potential data loss, but underscores the need for vigilance against social engineering attacks. No ongoing threat detected; case closed pending remediation verification.
