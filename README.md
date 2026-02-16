# wazuh-soc-lab-rdp-bruteforce-detection

Wazuh SIEM Home Lab – RDP Brute Force Detection

Project Overview

This project demonstrates the deployment of a multi-VM cybersecurity home lab environment designed to simulate and detect RDP brute-force attacks using Wazuh SIEM.

The objective of this lab is to understand authentication-based attacks, analyze Windows Security logs, and implement defensive monitoring strategies using SIEM tools in a controlled virtual environment.

---

Lab Architecture

Virtual Machines Used:

Ubuntu Server – Wazuh Manager
Windows 10– Target Endpoint (Wazuh Agent Installed)
Kali Linux – Attack Simulation Machine

Virtualization Platform:
VMware Workstation (Isolated Network Configuration)

---

Environment Setup

Wazuh Manager Installation (Ubuntu)

Installed Wazuh Manager on Ubuntu Server
Verified services:

  ```bash
  sudo systemctl status wazuh-manager
  ```
* Confirmed Wazuh Indexer and Dashboard services running

---

Wazuh Agent Installation (Windows 10)

Installed Wazuh Agent
Configured Manager IP address
Started service:

  ```powershell
  NET START WazuhSvc
  ```
* Verified agent connection in Wazuh Dashboard

---

Attack Simulation (Kali Linux)

Simulated RDP brute-force attempts using Hydra:

```bash
hydra -l socuser -P rockyou.txt rdp://192.168.102.130
```

Purpose:

Generate multiple failed login attempts
Trigger Windows Security Event ID 4625
Validate SIEM detection capability

---

Log Analysis

Windows Security Events Monitored

Event ID 4625 – Failed Logon Attempt
Event ID 4624** – Successful Logon

Wazuh Discover Query Example

```
agent.name:"DESKTOP-RH3HGQS" AND data.win.system.eventID:4625
```

---

Custom Rule Creation

Created custom rule inside:

```
/var/ossec/etc/rules/local_rules.xml
```

Example:

```xml
<group name="local,windows,authentication">
  <rule id="100100" level="12">
    <if_sid>60122</if_sid>
    <description>HIGH: Windows failed login detected (Custom Override)</description>
    <group>authentication_failed,bruteforce</group>
    <options>no_full_log</options>
  </rule>
</group>
```

Restarted Wazuh Manager:

```bash
sudo systemctl restart wazuh-manager
```

---

Skills Demonstrated

SIEM Deployment and Configuration
Wazuh Agent Management
Windows Authentication Log Analysis
RDP Brute Force Simulation (Controlled Lab)
Threat Hunting in Wazuh Dashboard
Custom Detection Rule Creation
Log Correlation
VMware Network Isolation Setup
Troubleshooting SIEM Configuration Errors

---

Security Note

All attack simulations were performed in a controlled and isolated virtual lab environment for educational and defensive security learning purposes only.

---

Key Takeaways

* Understanding attacker techniques improves defensive detection capability.
* Windows Event ID 4625 is critical for brute-force monitoring.
* Custom SIEM rules enhance detection visibility.
* Proper log correlation is essential in security operations.

---

Future Improvements

* Implement alert escalation workflows
* Integrate email alert notifications
* Simulate additional attack vectors (e.g., privilege escalation)
* Build dashboard visualizations for authentication anomalies

---

Author

Miguel Angelo Batain



