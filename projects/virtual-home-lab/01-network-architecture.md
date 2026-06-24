# Network Architecture

## Purpose

This page documents the network design for my virtual cyber security home lab.

The aim of the network architecture was to create an isolated lab environment that could simulate a small corporate network while keeping testing activity separate from my home network.

The lab uses pfSense as the central firewall and router, with separate virtual network zones for management, corporate systems and attacker testing.

---

## Overview

The lab was built on a Windows 11 host using Oracle VirtualBox.

Instead of placing every virtual machine on the same flat network, the lab was split into separate network zones. This allowed me to practise network segmentation, routing, firewall rule configuration and controlled traffic flow between different parts of the environment.

The three main internal zones are:

| Zone               | Purpose                                                            |
| ------------------ | ------------------------------------------------------------------ |
| Management Network | Used for management access and the Splunk SIEM server              |
| Corporate LAN      | Used for the Active Directory domain controller and Windows client |
| Attacker Zone      | Used for Kali Linux and controlled attack simulation               |

pfSense sits between these networks and acts as the gateway for each zone.

---

## Network Zones

| Zone               | Network            | Gateway           | Purpose                                                                |
| ------------------ | ------------------ | ----------------- | ---------------------------------------------------------------------- |
| WAN / NAT          | VirtualBox NAT     | N/A               | Provides controlled outbound internet access for updates and downloads |
| Management Network | `192.168.56.0/24`  | `192.168.56.254`  | Used for pfSense management and the Splunk SIEM server                 |
| Corporate LAN      | `192.168.211.0/24` | `192.168.211.254` | Internal corporate network containing Windows domain systems           |
| Attacker Zone      | `192.168.100.0/24` | `192.168.100.254` | Isolated testing network for Kali Linux                                |

---

## Host Addressing

| Host          | Role                                  | Network            | IP Address                                                |
| ------------- | ------------------------------------- | ------------------ | --------------------------------------------------------- |
| pfSense       | Firewall / router                     | Management Network | `192.168.56.254`                                          |
| pfSense       | Firewall / router                     | Corporate LAN      | `192.168.211.254`                                         |
| pfSense       | Firewall / router                     | Attacker Zone      | `192.168.100.254`                                         |
| Splunk Server | SIEM server                           | Management Network | `192.168.56.50`                                           |
| Lab-DC01      | Windows Server 2022 domain controller | Corporate LAN      | `192.168.211.10`                                          |
| Lab-Client01  | Windows client workstation            | Corporate LAN      | DHCP from `192.168.211.50-150`                            |
| Kali Linux    | Attacker/test machine                 | Attacker Zone      | DHCP from `192.168.100.50-150` or static `192.168.100.50` |

---

## VirtualBox Network Design

The lab uses multiple VirtualBox network adapters to separate traffic between different virtual networks.

| VirtualBox Network   | Connected Systems                                  | Purpose                                               |
| -------------------- | -------------------------------------------------- | ----------------------------------------------------- |
| NAT                  | pfSense WAN interface                              | Allows pfSense to reach the internet through the host |
| Host-Only Adapter #1 | pfSense management interface, Splunk server        | Management and monitoring network                     |
| Host-Only Adapter #2 | pfSense CORP_LAN interface, Windows domain systems | Corporate network                                     |
| Host-Only Adapter #3 | pfSense ATTACKER_ZONE interface, Kali Linux        | Isolated attacker/testing network                     |

This design allowed the lab systems to communicate through pfSense rather than being placed directly on the same network.

---

## High-Level Traffic Flow

The intended traffic flow was:

1. Internal lab systems use pfSense as their default gateway.
2. Corporate systems in `192.168.211.0/24` can communicate with required lab services.
3. The Splunk server receives forwarded logs from Windows systems.
4. Kali Linux generates controlled reconnaissance traffic from the attacker zone.
5. pfSense routes traffic between zones according to configured firewall rules.
6. Windows systems generate logs from network activity.
7. The Splunk Universal Forwarder sends Windows and Sysmon logs to Splunk.
8. Splunk is used to search and analyse the resulting telemetry.

---

## Simplified Network Diagram

```text
                         Internet / Updates
                                |
                         VirtualBox NAT
                                |
                            pfSense
              -------------------------------------
              |                 |                 |
      Management Network   Corporate LAN     Attacker Zone
       192.168.56.0/24    192.168.211.0/24  192.168.100.0/24
              |                 |                 |
       Splunk SIEM        Lab-DC01           Kali Linux
       192.168.56.50      192.168.211.10     192.168.100.50
                            |
                       Lab-Client01
                    DHCP 192.168.211.50-150
```

---

## Security Design Decisions

The network was designed with separation in mind.

The attacker machine was placed in its own network zone rather than directly inside the corporate LAN. This allowed me to simulate reconnaissance traffic from a separate network and observe how that traffic appeared in logs.

The Splunk server was placed in the management network to separate monitoring infrastructure from the corporate systems being monitored.

pfSense was used as the central routing and firewall point so that traffic between networks could be controlled, tested and adjusted.

---

## Why This Matters

This setup helped me practise concepts that are relevant to both IT support and cyber security roles, including:

* Subnetting
* Default gateways
* Static IP addressing
* DHCP scopes
* Network segmentation
* Firewall routing
* Lab isolation
* Controlled attack simulation
* Centralised log collection

It also helped show how network design affects security monitoring. The way systems are segmented and routed has a direct impact on what traffic can be seen, logged and investigated.

---

## Notes

The original intention was to practise a VLAN-style segmented network design. Because this lab was built using VirtualBox virtual machines, I used separate VirtualBox host-only networks connected through pfSense interfaces instead of traditional VLAN tagging.

This still allowed me to practise the same core concepts: separating network zones, assigning gateways, routing traffic through a firewall, creating firewall rules, and controlling communication between management, corporate and attacker networks.

Throughout the documentation I refer to these as network zones rather than VLANs, because no VLAN tagging was configured in this build.
This is a more accurate description of how the lab is currently built.
