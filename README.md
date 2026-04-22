---

# 🗡️ Operation: Dark Katana 🗡️
### *A Full-Stack SOC Home-Lab Simulation*

---

## 🛡️ Executive Summary
**Operation: Dark Katana** is a comprehensive security monitoring simulation designed to demonstrate the power of **Wazuh (SIEM)** in a hybrid environment. By mapping a sophisticated cyber-attack against the **MITRE ATT&CK®** framework, this lab provides hands-on experience in threat detection, behavioral analysis, and incident response. 

As the **SOC Analyst**, I monitor the entire "Intrusion Lifecycle"—from the first aggressive scan to the final data exfiltration.

---

## 🏗️ Lab Infrastructure
| Component | Device/OS | Role |
| :--- | :--- | :--- |
| **SIEM Manager** | Wazuh 4.14 (VirtualBox) | Centralized Logging & Alerting |
| **Victim 1 (Target)** | Windows 11 Home (Agent001) | **MSI Katana** - Primary Workstation |
| **Victim 2 (Diversion)** | Ubuntu Server 22.04 | Web/App Server |
| **Attacker** | Kali Linux (`TaskForce-Kali`) | Threat Actor Machine |

---

## 📜 The Narrative: A Story of Intrusion

### 📍 Phase 1: The Diversion
The story begins with the attacker "noisy-testing" the perimeter. They target the Ubuntu server first to see if the SOC team is awake.

* **The Action:** A rapid-fire SSH brute-force attack from Kali.
* **Technique:** Password Spraying (T1110.003).
* **Wazuh Detection:**
    * `Rule 5712`: Failed SSH Logins.
    * `Behavioral Alert`: Unusual Account Activity (High-frequency authentication failures).



### 🔎 Phase 2: The Shadow Scan (Reconnaissance)
Realizing the Linux server is monitored, the attacker pivots to the main prize: the MSI Katana. They hunt for entry points into the host where sensitive **FYP (Lightweight Cryptography)** data is stored.

* **The Action:** Aggressive Nmap discovery (`nmap -A`).
* **Technique:** Network Service Discovery (T1046).
* **Wazuh Detection:**
    * `Rule 40101`: Anomalous Network Traffic (Multiple connections to ports 135, 445, 9001).
    * `SCA Scan`: Security Policy Violations (Exposed SMB/RPC services).

### 🚪 Phase 3: The Backdoor (Persistence)
The attacker exploits a vulnerability and gains a remote shell. To ensure they aren't kicked out after a reboot, they dig in.

* **The Action:** Creating a hidden "backdoor" user and modifying the Registry.
* **Technique:** Account Creation (T1136) & Registry Run Keys (T1547.001).
* **Wazuh Detection:**
    * `Rule 60109`: User account created via `net user /add`.
    * `FIM Alert`: Unusual Registry Modifications in `HKCU\..\Run` keys.

### ⚡ Phase 4: The Climax (Defense Evasion & Takeover)
The "Boss Fight." The attacker attempts to gain SYSTEM-level privileges and "kill the lights" to hide their footprints.

* **The Action:** Running encoded PowerShell scripts to terminate security services.
* **Technique:** Impair Defenses (T1562) & Deobfuscate/Decode Files (T1140).
* **Wazuh Detection:**
    * **Suspicious PowerShell**: Detection of `-EncodedCommand` and `-ExecutionPolicy Bypass`.
    * **Privilege Escalation**: Unauthorized token manipulation detected.
    * `Rule 60107`: **Critical Service Stopped** (SIEM agent/Antivirus termination).



### 📍 Phase 5: The Data Heist (Side Quest)
With the defenses down, the attacker raids the "FYP Secrets"—the source code for the ASCON-128a project.

* **The Action:** Modifying files in a restricted project folder and swapping system binaries.
* **Technique:** Data from Local System (T1005).
* **Wazuh Detection (FIM):**
    * **Unauthorized Access**: File Integrity Monitoring flags read/write access to restricted directories.
    * **Binary Integrity**: Detection of a hash change in a system binary replaced by a trojan.

### 🚩 Phase 6: The Beacon (Exfiltration)
The final act. The attacker attempts to send the stolen cryptography code to a Command & Control (C2) server.

* **The Action:** Initiating an outbound connection to a "blacklisted" C2 IP address.
* **Technique:** Exfiltration Over C2 Channel (T1041).
* **Wazuh Detection:**
    * **Threat Intel Match**: Outbound traffic to known malicious IPs via threat-intel blacklist.

---

## 🏆 Final Conclusion
By the end of the lab, the **MITRE ATT&CK Matrix** in Wazuh is fully populated with the attacker's footprints. As a SOC Analyst, I successfully:

1.  **Identified** the initial access point and blocked the brute-force IP.
2.  **Traced** the lateral movement from Linux to the Windows agent.
3.  **Documented** the impact of service termination and privilege escalation.
4.  **Recovered** the integrity of the system using **File Integrity Monitoring (FIM)** logs.

---
**Author:** Habib (Lil B)  
**Project:** SOC Home-Lab Simulation  
**Tools:** Wazuh, Kali Linux, Ubuntu, Windows 11
