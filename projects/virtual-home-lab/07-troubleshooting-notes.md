# Troubleshooting Notes

## Purpose

This page summarises the main troubleshooting issues encountered while building my virtual cyber security home lab.

The aim of this section is to document not just what was built, but the problems that came up during the process, how they were investigated, and how they were resolved.

This is important because practical cyber security and IT work often involves diagnosing configuration issues, understanding how systems interact, and working through problems methodically.

---

## Summary of Issues

| Issue                                                        | Area Affected                 | Resolution                                                                            |
| ------------------------------------------------------------ | ----------------------------- | ------------------------------------------------------------------------------------- |
| VLAN-style segmentation not directly available in VirtualBox | Network design                | Used separate VirtualBox host-only networks routed through pfSense                    |
| Keeping test activity isolated from my home network          | Lab safety / network design   | Used VirtualBox NAT for outbound access and host-only networks for internal lab zones |
| Poor network performance through pfSense                     | Virtual networking / firewall | Disabled pfSense hardware offloading options                                          |
| DHCP failure after changing LAN subnet                       | pfSense / DHCP                | Manually configured Ubuntu networking, then corrected DHCP scope                      |
| Splunk service not available through systemd                 | Ubuntu / Splunk               | Reconfigured Splunk boot-start using systemd                                          |
| Nmap scan not visible in Splunk                              | Windows firewall / telemetry  | Created a firewall rule to allow lab scan traffic to generate logs                    |

---

## 1. VirtualBox VLAN Limitation and Network Zone Workaround

### Symptom / Design Challenge

The original aim was to practise a VLAN-style segmented network design, with separate areas for management, corporate systems and attacker testing.

However, because the lab was built using Oracle VirtualBox, I was not working with a physical managed switch or a full VLAN tagging setup. This meant traditional VLAN tagging was not the most practical approach for this build.

### Investigation

The main goal was not specifically to configure VLAN tags, but to practise the underlying concepts that VLANs are often used for:

* Separating different types of systems
* Controlling traffic between networks
* Routing traffic through a firewall
* Keeping attacker and test traffic away from normal systems
* Building a more realistic network layout than a flat subnet

VirtualBox host-only networks could provide this separation at the virtual networking layer, even without true VLAN tagging.

### Resolution

Instead of using traditional VLANs, I created separate VirtualBox host-only networks and connected each one to a dedicated pfSense interface.

The lab was split into separate network zones:

| Zone               | Network            | Purpose                              |
| ------------------ | ------------------ | ------------------------------------ |
| Management Network | `192.168.56.0/24`  | pfSense management and Splunk SIEM   |
| Corporate LAN      | `192.168.211.0/24` | Domain controller and Windows client |
| Attacker Zone      | `192.168.100.0/24` | Kali Linux test machine              |

pfSense then acted as the router and firewall between these zones.

This gave me a practical way to build a segmented lab, practise routing and firewall rules, and control traffic between different parts of the environment.

### What I Learned

This helped me understand the difference between VLANs and separate routed network zones.

Although this lab does not use VLAN tagging, it still allowed me to practise the same core design principles: segmentation, gateway assignment, routing, firewall control and controlled traffic flow between networks.

It also reinforced the importance of being accurate in documentation. Throughout this project, I refer to these as network zones rather than VLANs, because that better reflects how the lab was actually built.

---

## 2. Lab Isolation and Safe Testing

### Design Consideration

Because the lab included Kali Linux and controlled reconnaissance testing, it was important to keep the environment isolated from my home network.

The goal was to practise security monitoring safely without allowing test traffic to affect personal devices or other systems outside the lab.

### Implementation

The lab used VirtualBox networking and pfSense to keep the environment contained.

The pfSense WAN interface used VirtualBox NAT for controlled outbound internet access, while the internal lab systems were placed on separate host-only networks.

This meant the lab could download updates and packages when needed, but the internal corporate and attacker networks remained separate from the wider home network.

### What I Learned

This reinforced that lab safety matters.

Even when testing basic tools like Nmap, it is important to know where the traffic is going and to make sure testing is limited to systems you own and control.

This also made the lab more realistic, because the attacker system was placed in a separate zone rather than directly on the same network as the domain controller.

---

## 3. pfSense Hardware Offloading and Poor Network Performance

### Symptom

During the early build stages, network performance through pfSense was extremely poor.

Ubuntu package updates timed out, and large downloads for Windows evaluation media stalled or failed. The issue only appeared when traffic was routed through the pfSense gateway.

### Investigation

pfSense runs on FreeBSD and includes hardware offloading features that are normally used to improve performance on physical network cards.

In this lab, pfSense was running as a virtual machine inside Oracle VirtualBox. The network adapters were virtual devices rather than physical NICs.

The issue appeared to be caused by pfSense attempting to use hardware offloading features with VirtualBox virtual network adapters. This caused packets to be dropped or processed incorrectly, resulting in severe network slowdown.

### Resolution

I accessed the pfSense web interface and disabled the following hardware offloading options:

* Hardware checksum offloading
* Hardware TCP segmentation offloading
* Hardware large receive offloading

After saving the changes and rebooting pfSense, network performance returned to normal.

### What I Learned

This showed that network performance issues in a virtual lab are not always caused by the guest operating system or internet connection.

Sometimes the issue is caused by how the virtualisation layer handles network traffic. It also reinforced the importance of understanding the difference between physical and virtual network adapters.

---

## 4. DHCP Failure After Changing the Management Subnet

### Symptom

After changing the pfSense LAN interface from the default subnet to the custom management network, new virtual machines connected to that network could not receive an automatic IPv4 address.

The Ubuntu Server installer failed during automatic network discovery because no DHCP lease was being provided.

### Investigation

The pfSense LAN interface had been changed to:

`192.168.56.254/24`

However, the DHCP service was still tied to the old default subnet configuration.

This meant the DHCP pool no longer matched the active LAN network, so pfSense was not handing out valid addresses to new hosts on the management network.

### Resolution

To complete the Ubuntu Server installation, I manually configured the network settings during setup:

| Setting   | Value                |
| --------- | -------------------- |
| Subnet    | `192.168.56.0/24`    |
| Static IP | `192.168.56.50`      |
| Gateway   | `192.168.56.254`     |
| DNS       | `1.1.1.1`, `8.8.8.8` |

Once Ubuntu was installed and the pfSense web interface was available, I corrected the DHCP scope under the pfSense DHCP server settings.

The management DHCP range was updated to:

`192.168.56.50-192.168.56.150`

After this change, DHCP worked correctly for future systems on the management network.

### What I Learned

Changing an interface address can affect supporting services such as DHCP.

The interface IP, subnet and DHCP pool need to align. If they do not, new hosts may fail to obtain network configuration automatically.

This was a useful reminder to check both the interface configuration and related services when troubleshooting network issues.

---

## 5. Splunk systemd Service Issue

### Symptom

After installing Splunk Enterprise on Ubuntu Server, the expected Splunk service was not available through `systemctl`.

Running the expected service command produced an error showing that the Splunk service unit could not be found.

### Investigation

Modern Ubuntu systems use `systemd` to manage services.

Splunk needed a valid systemd service file to run reliably in the background and persist across reboots. The initial installation did not create the expected service configuration in a usable way.

There also appeared to be a conflict with older boot-start configuration behaviour.

### Resolution

I cleared the old boot-start configuration and then enabled Splunk using systemd-managed settings.

The process involved:

* Disabling the existing boot-start configuration
* Reloading the systemd daemon
* Re-enabling Splunk using systemd-managed service settings
* Enabling and starting the Splunk service
* Checking service status to confirm it was running correctly

After this, Splunk could be managed as a persistent service and would survive server reboots.

### What I Learned

Installing a service is not the same as making it reliable.

For a SIEM platform like Splunk, service persistence matters because log collection depends on the server being available. This issue helped me better understand Linux service management and the importance of validating services after installation.

---

## 6. Windows Firewall Visibility Gap During Nmap Testing

### Symptom

After configuring Sysmon, Windows audit policy and Splunk forwarding, I ran an Nmap scan from Kali Linux against the corporate LAN.

The scan traffic was expected to generate Windows Filtering Platform events, but Splunk showed no useful `5156` or `5157` events from the domain controller.

### Investigation

The Kali Linux machine was in the attacker zone:

`192.168.100.50`

The domain controller was in the corporate LAN:

`192.168.211.10`

The traffic was crossing from one subnet into another. Windows Defender Firewall treated the incoming traffic from the attacker zone as untrusted and silently dropped it before it generated the audit events I wanted to analyse.

This created a visibility gap. The activity was happening, but the logging pipeline was not seeing the expected events.

### Resolution

For the purposes of controlled lab testing, I created a custom inbound Windows Defender Firewall rule on the domain controller.

The rule allowed traffic from the Kali Linux lab IP:

`192.168.100.50`

After applying the rule, I refreshed policy using:

`gpupdate /force`

I then re-ran the Nmap scan.

This time, Windows generated the expected Filtering Platform events and Splunk was able to receive and analyse them.

### What I Learned

This was one of the most useful troubleshooting lessons in the project.

It showed that missing SIEM events do not always mean nothing happened. Sometimes the environment is not generating the right telemetry because traffic is being blocked or dropped before it reaches the logging stage.

It reinforced the importance of understanding the full detection path:

`Activity -> Logging -> Forwarding -> Indexing -> Searching -> Detection`

If any part of that chain is missing or misconfigured, the detection can fail.

---

## Key Takeaways

The main troubleshooting lessons from this project were:

* VLAN-style segmentation can be practised in VirtualBox using separate host-only networks and routed firewall interfaces
* Safe lab design matters when using attacker tools, even for controlled testing
* Virtual networking issues can come from the hypervisor layer
* Firewall/router configuration affects every part of a segmented lab
* DHCP issues often come from mismatched interface and scope settings
* Linux services need to be validated after installation
* SIEM visibility depends on endpoint logging and forwarding being configured correctly
* Missing logs do not automatically mean missing activity
* Detection depends on the full telemetry pipeline working end to end

---

## Reflection

This project was valuable because the troubleshooting was just as important as the successful setup.

Building the lab required more than following installation steps. I had to adapt the network design to fit the limits of VirtualBox, keep the lab isolated, diagnose network performance issues, repair DHCP configuration, make Splunk run reliably and fix a detection visibility gap caused by Windows firewall behaviour.

These problems helped me build more practical confidence because they forced me to understand why things were not working, not just copy commands until they did.

The biggest takeaway was that cyber security tooling depends heavily on the underlying infrastructure. Firewalls, routing, DNS, DHCP, services, logging policy and forwarding configuration all affect what an analyst can see during an investigation.
