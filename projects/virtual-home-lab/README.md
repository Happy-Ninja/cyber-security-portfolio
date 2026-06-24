# Virtual Home Lab: pfSense, Active Directory and Splunk SIEM

## Aim

The aim of this project was to build an isolated virtual home lab that simulates a small corporate network and allows me to practise cyber security, networking and system administration skills in a safe environment.

The lab was designed to give me hands-on experience with network segmentation, firewall configuration, Active Directory, Windows logging, Sysmon, Splunk SIEM ingestion and basic attack detection workflows.

A key goal was to validate the full path from activity generation to detection: generating network reconnaissance traffic from an attacker machine, forwarding Windows and Sysmon logs into Splunk, and using SPL queries to identify suspicious activity.

---

## Scope

This project covers the design, build and validation of an isolated virtual cyber security lab using Oracle VirtualBox.

The lab includes a segmented network built around pfSense, a Windows Active Directory environment, a Splunk SIEM server, Windows log forwarding, Sysmon telemetry and a Kali Linux test machine used to generate controlled reconnaissance traffic.

The focus of this project is defensive monitoring and detection. It does not include malware execution, real-world offensive activity, or testing outside the isolated lab environment.

---

## Lab Summary

The lab consists of a pfSense firewall/router connected to separate virtual network zones for management, corporate systems and attacker testing.

The main systems in the lab are:

| System              | Role                                      |
| ------------------- | ----------------------------------------- |
| pfSense             | Firewall, router and network segmentation |
| Ubuntu Server       | Splunk Enterprise SIEM server             |
| Windows Server 2022 | Active Directory domain controller        |
| Windows Client      | Domain-joined workstation                 |
| Kali Linux          | Isolated attacker/test machine            |

---

## Detailed Build Notes

This project has been split into separate build notes so each part of the lab can be documented clearly without turning the overview into a wall of text.

The sections below cover the technical setup, configuration decisions, evidence gathered and troubleshooting steps from the lab build.

| Section                                                                    | Description                                                                                                                                                                              |
| -------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Network Architecture](./01-network-architecture.md)                       | Documents the overall lab design, including VirtualBox host-only networks, pfSense routing, network zones, IP ranges and host roles.                                                     |
| [pfSense Firewall Configuration](./02-pfsense-firewall.md)                 | Covers the pfSense virtual firewall setup, interface assignments, DHCP configuration, firewall rules and the network performance issue fixed by disabling hardware offloading.           |
| [Active Directory Domain Setup](./03-active-directory.md)                  | Covers the Windows Server 2022 domain controller build, the `corp.local` forest, organisational unit creation, test user setup and Windows client domain join.                           |
| [Splunk SIEM Setup](./04-splunk-siem.md)                                   | Documents the Ubuntu Server setup, Splunk Enterprise installation, creation of the `windows` index and configuration of the Splunk receiving port.                                       |
| [Sysmon and Log Forwarding](./05-sysmon-and-log-forwarding.md)             | Covers Sysmon installation, audit policy tuning, Splunk Universal Forwarder setup and forwarding Windows Security, System and Sysmon logs into Splunk.                                   |
| [Attack Simulation and Detection](./06-attack-simulation-and-detection.md) | Documents the Kali Linux reconnaissance test using Nmap, the traffic generated against the corporate LAN and the Splunk search used to detect and analyse the scan activity.             |
| [Troubleshooting Notes](./07-troubleshooting-notes.md)                     | Summarises the main technical problems encountered during the build and how they were resolved, including networking, Splunk service persistence and Windows firewall visibility issues. |
