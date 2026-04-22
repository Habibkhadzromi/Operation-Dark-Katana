# Operation-Dark-Katana

Project: Operation Dark Katana (SOC Home-Lab)
🛡️ Executive Summary
This project demonstrates a full-scale security monitoring environment using Wazuh (SIEM). The lab simulates a sophisticated cyber intrusion on a Windows 11 workstation and an Ubuntu server. As a SOC Analyst, I monitor the "Intrusion Lifecycle" from reconnaissance to data exfiltration.

🏗️ The Environment
SIEM Manager: Wazuh 4.14 (Running on VirtualBox)

Victim 1 (The Target): Windows 11 Home (Agent001 - MSI Katana)

Victim 2 (The Diversion): Ubuntu Server 22.04

Attacker: Kali Linux (Hostname: TaskForce-Kali)

📜 The Narrative: A Story of Intrusion
📍 Phase 1: The Diversion (Side Quest)
The story begins with the attacker trying to "noisy-test" the network perimeter. They target the Ubuntu server first to see if the SOC team is active.

The Action: A brute-force SSH attack launched from Kali.

Technique: Password spraying/Brute force.

Wazuh Detection: * Failed SSH Logins: (Rule 5712)

Unusual Account Activity: High-frequency authentication failures.

🔎 Phase 2: The Shadow Scan (Reconnaissance)
Realizing the Ubuntu server is monitored, the attacker pivots to the main prize: the Windows 11 workstation. They look for entry points into the host where sensitive FYP (Final Year Project) data is stored.

The Action: An aggressive Nmap scan (nmap -A).

Technique: Network Service Discovery.

Wazuh Detection: * Anomalous Network Traffic: (Rule 40101) Multiple connections to ports 135, 445, and 9001.

Security Policy Violations: Detecting exposed SMB/RPC services.

🚪 Phase 3: The Backdoor (Persistence)
The attacker exploits a vulnerability and gains a remote shell. Their first goal is "Persistence"—ensuring they can return even after a reboot.

The Action: Creating a hidden user and modifying the Windows Registry.

Technique: Account Creation & Boot/Logon Autostart Execution.

Wazuh Detection: * User Account Creation: (Rule 60109) Detection of net user /add.

Unusual Registry Modifications: Monitoring changes to HKCU\..\Run keys.

⚡ Phase 4: The Climax (Defense Evasion & Takeover)
The "Boss Fight." The attacker attempts to gain SYSTEM-level privileges and "kill the lights" by disabling security software.

The Action: Executing an encoded PowerShell script and stopping the Wazuh/Antivirus services.

Technique: Impair Defenses & Command and Scripting Interpreter.

Wazuh Detection: * Suspicious PowerShell Commands: Detection of -EncodedCommand and -ExecutionPolicy Bypass.

Abnormal Process Execution: Unexpected child processes from cmd.exe.

Privilege Escalation Attempts: Detection of unauthorized token manipulation.

Critical Service Stopped: (Rule 60107) Alerting that security services were manually terminated.

📍 Phase 5: The Data Heist (Side Quest)
With the defenses down, the attacker searches for the "FYP Secrets"—source code for a Lightweight Cryptography (ASCON-128a) project.

The Action: Accessing and modifying files in a restricted project folder.

Technique: Data from Local System.

Wazuh Detection (FIM): * Unauthorized Access to Sensitive Files: File Integrity Monitoring flags the read/write access.

Executable File Modification: Detecting a hash change in a system binary replaced by a trojan.

🚩 Phase 6: The Beacon (Exfiltration)
The final act. The attacker attempts to send the stolen cryptography code to a Command & Control (C2) server.

The Action: Initiating an outbound connection to a "blacklisted" IP address.

Technique: Exfiltration Over C2 Channel.

Wazuh Detection: * Outbound Traffic to Known Malicious IPs: IP matching against a threat-intel blacklist.

🏆 Final Conclusion
By the end of the lab, the MITRE ATT&CK Matrix in Wazuh is populated with the attacker's footprints. The SOC Analyst successfully:

Identified the Initial Access via SSH brute force.

Traced the Lateral Movement to the Windows agent.

Documented the Impact of service termination.

Recovered the system using File Integrity Monitoring logs.
