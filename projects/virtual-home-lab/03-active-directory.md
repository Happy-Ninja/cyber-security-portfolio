# Active Directory Domain Setup

## Purpose

This page documents the Active Directory domain environment used in my virtual cyber security home lab.

The aim of this part of the project was to gain practical experience with Active Directory, Windows Server administration and basic policy management in a small domain environment.

This included building a Windows Server 2022 domain controller, creating a new `corp.local` forest, managing users and organisational units, joining a Windows client workstation to the domain, and applying Group Policy updates.

The domain environment also provided realistic Windows infrastructure for log generation, domain authentication, endpoint telemetry and later security monitoring in Splunk.

---

## Role of Active Directory in the Lab

Active Directory was used to simulate a small corporate identity environment.

In this lab, the domain controller provides:

* Centralised user account management
* Domain authentication
* DNS services for the corporate LAN
* Group Policy management
* A realistic Windows logging source
* A target system for security monitoring and detection testing

---

## Lab Domain Overview

| Item                       | Configuration                                   |
| -------------------------- | ----------------------------------------------- |
| Domain controller hostname | `Lab-DC01`                                      |
| Operating system           | Windows Server 2022 Standard Desktop Experience |
| Static IP address          | `192.168.211.10`                                |
| Default gateway            | `192.168.211.254`                               |
| Domain name                | `corp.local`                                    |
| Network zone               | Corporate LAN                                   |
| Corporate LAN subnet       | `192.168.211.0/24`                              |

The domain controller was placed on the corporate LAN behind pfSense.

The pfSense gateway for this network was:

`192.168.211.254`

---

## Windows Server Deployment

Windows Server 2022 was installed as a virtual machine in Oracle VirtualBox.

After installation, the server was configured with:

* A clear hostname: `Lab-DC01`
* Static IPv4 addressing
* Correct default gateway
* DNS settings suitable for Active Directory
* Server Manager access for adding roles and features

The server was renamed before being promoted to a domain controller so that the hostname was clear and consistent across the lab.

---

## Static Network Configuration

The domain controller was configured with a static IP address so that other systems could reliably use it for DNS and domain services.

| Setting         | Value              |
| --------------- | ------------------ |
| IP address      | `192.168.211.10`   |
| Subnet          | `192.168.211.0/24` |
| Default gateway | `192.168.211.254`  |
| Preferred DNS   | `192.168.211.10`   |

Using a static IP was important because the Windows client workstation needed to point to the domain controller for DNS before joining the domain.

---

## Active Directory Domain Services Installation

The Active Directory Domain Services role was installed using Server Manager.

The server was then promoted to a domain controller and used to create a new forest:

`corp.local`

This created the core identity infrastructure for the lab.

The domain controller was configured as the main server for:

* Active Directory Domain Services
* DNS
* Domain authentication
* Group Policy processing

---

## Organisational Unit and Test User

After the domain was created, Active Directory Users and Computers was used to create a basic structure for test users.

A custom organisational unit was created:

`Corporate_Users`

A mock employee account was then created:

| User         | Details                                                             |
| ------------ | ------------------------------------------------------------------- |
| Username     | `jsmith`                                                            |
| Display name | John Smith                                                          |
| Purpose      | Low-privileged test user for workstation login and event generation |

Creating a standard test user allowed me to practise logging in as a non-administrator account and later generate realistic Windows authentication events.

---

## Windows Client Workstation

A Windows client workstation was built as a domain-joined endpoint.

During installation, the network connection was temporarily disconnected so that a local account could be created without needing to sign in with a Microsoft account.

The local setup account used was:

`localadmin`

After the operating system setup was complete, the client was connected to the corporate LAN.

---

## Client DNS Configuration

Before joining the domain, the Windows client workstation needed to use the domain controller as its DNS server.

The preferred DNS server was set to:

`192.168.211.10`

This was required because Active Directory domain joining relies on DNS to locate the domain controller and domain services.

---

## Domain Join

The Windows client workstation was successfully joined to the domain:

`corp.local`

Domain join authentication was completed using domain administrator credentials.

After rebooting, the workstation was tested by logging in as the low-privileged domain user:

`CORP\jsmith`

This confirmed that the domain controller was working correctly and that the client could authenticate against the domain.

---

## Group Policy Update

Group Policy was refreshed on the domain controller using:

`gpupdate /force`

<img src="./images/gpupdate-success-dc01.png" alt="Windows Server command prompt showing gpupdate force completed successfully" width="900">

*Ref 1: Group Policy update completed successfully on the Windows Server domain controller.*

This confirmed that Group Policy processing was working and allowed later audit policy changes to be applied during the logging and detection stages of the project.

---

## Validation Checks

After configuring the domain environment, I validated that:

* `Lab-DC01` had a static IP address
* The server could communicate through the corporate LAN gateway
* Active Directory Domain Services was installed
* The `corp.local` forest was created
* DNS was available from the domain controller
* The `Corporate_Users` OU was created
* The `jsmith` test user was created
* The Windows client could use the domain controller for DNS
* The Windows client could join the `corp.local` domain
* The `jsmith` user could log in to the domain-joined workstation
* Group Policy could be refreshed successfully

---

## Why This Matters for Security Monitoring

Active Directory is a key part of many corporate environments, so building a small domain helped make the lab more realistic.

The domain controller and client workstation can generate useful security events, including:

* Successful logons
* Failed logons
* Account changes
* Group Policy updates
* Service activity
* DNS activity
* Windows security events
* Sysmon endpoint telemetry

These logs can then be forwarded into Splunk for monitoring and investigation.

---

## Skills Practised

This part of the project helped me practise:

* Windows Server installation
* Static IP configuration
* Server renaming
* Active Directory Domain Services installation
* Domain controller promotion
* New forest creation
* DNS configuration for Active Directory
* Organisational unit creation
* User account creation
* Basic Active Directory administration
* Windows client setup
* Domain joining
* Logging in as a domain user
* Group Policy refresh
* Basic policy management
* Windows administration troubleshooting

---

## What I Learned

This part of the lab helped me understand how important DNS is to Active Directory.

The Windows client could only join the domain once it was using the domain controller as its DNS server. This reinforced that domain issues are often linked to name resolution and network configuration rather than just user credentials.

I also learned how useful even a simple domain can be for security practice. Once the domain controller and client were working, the lab could produce realistic Windows activity that could later be forwarded into Splunk and analysed as part of a SOC-style workflow.
