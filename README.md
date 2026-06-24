# Rob Barnes - Cyber Security Portfolio

<a href="https://www.linkedin.com/in/robertbarnes1990/"><img src="https://img.shields.io/badge/-LinkedIn-0072b1?&style=for-the-badge&logo=linkedin&logoColor=white" /></a>

## About This Portfolio

This portfolio documents my practical learning as I work towards a career move into cyber security, with a focus on SOC analysis, defensive security, log analysis and incident investigation.

My background includes hands-on technical troubleshooting, system configuration, client communication and structured problem solving across Windows, Linux and macOS environments. Alongside my current role, I have been building my cyber security knowledge through structured study, hands-on labs and a virtual home lab environment.

The aim of this portfolio is to show not only what I am learning, but how I approach technical problems, document my work and apply security concepts in practical scenarios.

---

## Objective

My goal is to move into a SOC or cyber security analyst role where I can apply my technical troubleshooting experience, continue developing practical security skills and contribute to detection, investigation and response work.

I am still early in my cyber security career, but I am committed to learning properly, building hands-on experience and documenting my progress clearly.

---

## Current Study

| Area                                          | Status                               |
| --------------------------------------------- | ------------------------------------ |
| Google Cybersecurity Professional Certificate | In progress                          |
| CompTIA Security+                             | Studying                             |
| CompTIA Network+                              | Studying                             |
| TryHackMe                                     | Ongoing practical learning           |
| Virtual home lab                              | Ongoing build and detection practice |

---

## Skills

| Skill Area                                                  | Evidence                                                                                                |
| ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| Security auditing and control review                        | [Mock Security Audit: Botium Toys](./projects/botium-toys-security-audit/)                              |
| Risk assessment and compliance awareness                    | [Mock Security Audit: Botium Toys](./projects/botium-toys-security-audit/)                              |
| Identifying administrative, technical and physical controls | [Mock Security Audit: Botium Toys](./projects/botium-toys-security-audit/)                              |
| PCI DSS, GDPR and SOC control awareness                     | [Mock Security Audit: Botium Toys](./projects/botium-toys-security-audit/)                              |
| Network traffic analysis                                    | [Network Traffic Analysis: DNS and ICMP Incident Report](./projects/network-traffic-analysis-dns-icmp/) |
| DNS, UDP and ICMP protocol analysis                         | [Network Traffic Analysis: DNS and ICMP Incident Report](./projects/network-traffic-analysis-dns-icmp/) |
| Interpreting tcpdump output                                 | [Network Traffic Analysis: DNS and ICMP Incident Report](./projects/network-traffic-analysis-dns-icmp/) |
| Incident report writing                                     | [Network Traffic Analysis: DNS and ICMP Incident Report](./projects/network-traffic-analysis-dns-icmp/) |
| Virtual lab build and documentation                         | [Virtual Home Lab: pfSense, Active Directory and Splunk SIEM](./projects/virtual-home-lab/)             |
| Network segmentation and firewall configuration             | [Virtual Home Lab: pfSense, Active Directory and Splunk SIEM](./projects/virtual-home-lab/)             |
| Active Directory setup and basic policy management          | [Virtual Home Lab: pfSense, Active Directory and Splunk SIEM](./projects/virtual-home-lab/)             |
| SIEM setup and log analysis                                 | [Virtual Home Lab: pfSense, Active Directory and Splunk SIEM](./projects/virtual-home-lab/)             |
| Splunk installation and configuration                       | [Virtual Home Lab: pfSense, Active Directory and Splunk SIEM](./projects/virtual-home-lab/)             |
| Sysmon deployment and Windows log forwarding                | [Virtual Home Lab: pfSense, Active Directory and Splunk SIEM](./projects/virtual-home-lab/)             |
| Basic SPL searching and field extraction                    | [Virtual Home Lab: pfSense, Active Directory and Splunk SIEM](./projects/virtual-home-lab/)             |
| Nmap reconnaissance detection                               | [Virtual Home Lab: pfSense, Active Directory and Splunk SIEM](./projects/virtual-home-lab/)             |
| Technical troubleshooting and documentation                 | [Virtual Home Lab: pfSense, Active Directory and Splunk SIEM](./projects/virtual-home-lab/)             |

---

## Tools and Technologies

<img src="https://img.shields.io/badge/-Splunk-000000?&style=for-the-badge&logo=Splunk&logoColor=white" />
<img src="https://img.shields.io/badge/-pfSense-212121?&style=for-the-badge&logo=pfsense&logoColor=white" />
<img src="https://img.shields.io/badge/-Windows_Server-0078D6?&style=for-the-badge&logo=windows&logoColor=white" />
<img src="https://img.shields.io/badge/-Windows-0078D6?&style=for-the-badge&logo=windows&logoColor=white" />
<img src="https://img.shields.io/badge/-Linux-FCC624?&style=for-the-badge&logo=linux&logoColor=black" />
<img src="https://img.shields.io/badge/-Ubuntu-E95420?&style=for-the-badge&logo=ubuntu&logoColor=white" />
<img src="https://img.shields.io/badge/-Active_Directory-0078D4?&style=for-the-badge&logo=microsoft&logoColor=white" />
<img src="https://img.shields.io/badge/-VirtualBox-183A61?&style=for-the-badge&logo=virtualbox&logoColor=white" />
<img src="https://img.shields.io/badge/-Kali_Linux-557C94?&style=for-the-badge&logo=kalilinux&logoColor=white" />
<img src="https://img.shields.io/badge/-Nmap-004170?&style=for-the-badge&logoColor=white" />

---

## Portfolio Projects

### [Virtual Home Lab: pfSense, Active Directory and Splunk SIEM](./projects/virtual-home-lab/)

A practical virtual cyber security lab built using Oracle VirtualBox, pfSense, Windows Server, Active Directory, Splunk Enterprise, Sysmon and Kali Linux.

This project involved designing a segmented virtual network, configuring pfSense as the central firewall/router, building a Windows domain environment, forwarding Windows and Sysmon logs into Splunk, and using Nmap reconnaissance traffic to validate detection in the SIEM.

**Key areas covered:**

* Virtual network design
* pfSense firewall and routing
* Network segmentation using VirtualBox host-only networks
* Windows Server 2022 and Active Directory
* Basic Group Policy and audit policy configuration
* Splunk Enterprise setup
* Splunk Universal Forwarder configuration
* Sysmon deployment
* Windows Security, System and Sysmon log forwarding
* Nmap reconnaissance simulation
* SPL queries and detection analysis
* Troubleshooting and documentation

**Detailed build notes:**

* [Network Architecture](./projects/virtual-home-lab/01-network-architecture.md)
* [pfSense Firewall Configuration](./projects/virtual-home-lab/02-pfsense-firewall.md)
* [Active Directory Domain Setup](./projects/virtual-home-lab/03-active-directory.md)
* [Splunk SIEM Setup](./projects/virtual-home-lab/04-splunk-siem.md)
* [Sysmon and Log Forwarding](./projects/virtual-home-lab/05-sysmon-and-log-forwarding.md)
* [Attack Simulation and Detection](./projects/virtual-home-lab/06-attack-simulation-and-detection.md)
* [Troubleshooting Notes](./projects/virtual-home-lab/07-troubleshooting-notes.md)

---

### [Network Traffic Analysis: DNS and ICMP Incident Report](./projects/network-traffic-analysis-dns-icmp/)

A network traffic analysis project completed as part of the Google Cybersecurity Professional Certificate.

This project involved reviewing simulated tcpdump output from an incident where customers were unable to access a website. The analysis identified that DNS requests were being sent using UDP to port 53, but ICMP responses were being returned with a `udp port 53 unreachable` error.

The report explains the protocols involved, the likely cause of the issue, and recommended next steps such as checking DNS server availability, reviewing firewall rules and investigating possible denial of service activity.

**Key areas covered:**

* tcpdump log analysis
* DNS, UDP and ICMP traffic
* Port 53 troubleshooting
* Network incident reporting
* Identifying possible root causes
* Recommending next investigation steps

---

### [Mock Security Audit: Botium Toys](./projects/botium-toys-security-audit/)

A mock internal security audit completed as part of the Google Cybersecurity Professional Certificate.

This project involved reviewing the organisation’s scope, assets, existing controls and compliance gaps. The work included identifying missing administrative, technical and physical controls, reviewing risks linked to customer data and payment information, and producing recommendations to improve the organisation’s security posture.

**Key areas covered:**

* Security control assessment
* Risk assessment
* Compliance awareness
* PCI DSS, GDPR and SOC control considerations
* Asset and data classification
* Security recommendations and documentation

---

## Current Focus

I am currently focusing on:

* SOC analyst fundamentals
* Networking and security concepts
* Windows and Linux administration
* Splunk and SIEM workflows
* Log analysis and detection
* Active Directory fundamentals
* Incident investigation and reporting
* Practical lab documentation

---

## Next Steps

Planned next steps include:

* Add screenshots of the final Nmap detection and Splunk visualisation
* Add more detection examples from the virtual home lab
* Generate and investigate failed logon activity
* Create simple Splunk dashboards
* Document additional mock SOC investigations
* Continue progressing through the Google Cybersecurity Professional Certificate
* Continue studying towards CompTIA Security+ and Network+
* Build familiarity with other SIEM tools, such as Microsoft Sentinel
