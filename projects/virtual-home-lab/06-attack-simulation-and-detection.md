# Attack Simulation and Detection

## Purpose

This page documents the attack simulation and detection stage of my virtual cyber security home lab.

The aim of this part of the project was to validate the full monitoring pipeline by generating controlled reconnaissance traffic from Kali Linux and detecting the resulting activity in Splunk.

This helped me practise a basic SOC-style workflow: generate test activity, collect logs, search the data, identify patterns and explain what happened.

---

## Scenario

To test the lab, I used Kali Linux from the isolated attacker zone to run a network reconnaissance scan against the corporate LAN.

The target network contained the Windows Server 2022 domain controller:

`192.168.211.10`

The attacker machine was placed in a separate network zone:

`192.168.100.0/24`

This allowed me to simulate traffic coming from outside the corporate LAN while still keeping the activity inside the isolated virtual lab.

---

## Systems Involved

| System     | Role                                  | IP Address                           |
| ---------- | ------------------------------------- | ------------------------------------ |
| Kali Linux | Attacker / test machine               | `192.168.100.50`                     |
| pfSense    | Firewall / router                     | `192.168.100.254`, `192.168.211.254` |
| Lab-DC01   | Windows Server 2022 domain controller | `192.168.211.10`                     |
| Lab-Splunk | Splunk Enterprise SIEM server         | `192.168.56.50`                      |

---

## Attack Simulation

Kali Linux was used to run an Nmap scan against the corporate LAN.

Example scan command:

```bash
nmap -sV -O -p- 192.168.211.0/24
```

The purpose of the scan was to simulate automated network reconnaissance.

The options used were:

| Option             | Meaning                                                     |
| ------------------ | ----------------------------------------------------------- |
| `-sV`              | Attempts to identify service versions running on open ports |
| `-O`               | Attempts operating system detection                         |
| `-p-`              | Scans all TCP ports                                         |
| `192.168.211.0/24` | Targets the corporate LAN subnet                            |

---

## Expected Behaviour

The scan was expected to generate a large number of connection attempts against hosts in the corporate LAN.

Because the domain controller was running common Windows and Active Directory services, the scan was expected to identify ports linked to domain services.

Examples of expected services included:

| Port  | Service  |
| ----- | -------- |
| `53`  | DNS      |
| `88`  | Kerberos |
| `135` | RPC      |
| `139` | NetBIOS  |
| `389` | LDAP     |
| `445` | SMB      |

This type of activity is useful for detection practice because port scanning and service discovery are common early-stage reconnaissance techniques.

---

## Telemetry Sources

The detection relied on Windows logs being generated and forwarded into Splunk.

The main telemetry sources were:

| Source                            | Purpose                                          |
| --------------------------------- | ------------------------------------------------ |
| Windows Security logs             | Captured security and audit-related events       |
| Windows Filtering Platform events | Captured allowed and blocked connection activity |
| Sysmon Operational logs           | Provided enhanced endpoint telemetry             |
| Splunk Universal Forwarder        | Sent Windows logs to Splunk                      |
| Splunk Enterprise                 | Indexed and searched the collected events        |

The relevant Windows Filtering Platform event codes were:

| Event Code | Meaning                                             |
| ---------- | --------------------------------------------------- |
| `5156`     | The Windows Filtering Platform allowed a connection |
| `5157`     | The Windows Filtering Platform blocked a connection |

---

## Splunk Detection Query

After running the Nmap scan, I searched Splunk for Windows Filtering Platform events.

The following SPL query was used:

```spl
index="windows" (EventCode=5156 OR EventCode=5157)
| rex field=_raw "DestPort=(?<Destination_Port>\d+)"
| stats count by Destination_Port
| sort - count
```

This query searched the `windows` index for allowed or blocked connection events, extracted the destination port from the raw event data, counted the number of events per destination port, and sorted the results by the highest count.

---

## Query Breakdown

| SPL Component                                        | Purpose                                                                     |
| ---------------------------------------------------- | --------------------------------------------------------------------------- |
| `index="windows"`                                    | Searches the Windows telemetry index                                        |
| `(EventCode=5156 OR EventCode=5157)`                 | Filters for allowed or blocked Windows Filtering Platform connection events |
| `rex field=_raw "DestPort=(?<Destination_Port>\d+)"` | Extracts the destination port from the raw log data                         |
| `stats count by Destination_Port`                    | Counts events grouped by destination port                                   |
| `sort - count`                                       | Sorts results from highest to lowest count                                  |

---

## Detection Outcome

The scan generated a clear increase in Windows Filtering Platform events.

In Splunk, the query grouped the activity by destination port, showing repeated connection attempts against multiple ports on the target network.

This confirmed that:

* Reconnaissance traffic was generated from Kali Linux
* The traffic reached the Windows domain environment
* Windows generated relevant network connection events
* The Splunk Universal Forwarder sent the logs to Splunk
* Splunk indexed the logs successfully
* SPL could be used to extract and analyse the activity

This validated the end-to-end telemetry pipeline from attack simulation to detection.

---

<!--- ## Evidence

Add screenshots here once available:

```md
<img src="./images/nmap-scan-output.png" alt="Kali Linux Nmap scan output against the corporate LAN" width="900">

*Ref 1: Nmap scan output showing open services discovered on the corporate LAN.*

<img src="./images/splunk-port-scan-detection.png" alt="Splunk query showing destination port counts from Windows Filtering Platform events" width="900">

*Ref 2: Splunk query showing destination port counts from network scan activity.*
```
--->
---

## Troubleshooting Notes

### Windows firewall visibility issue

During testing, the initial Nmap scans did not produce the expected Windows Filtering Platform events in Splunk.

The scan was reaching the lab network, but Splunk showed no useful `5156` or `5157` events from the domain controller.

After investigation, I found that Windows Defender Firewall was silently dropping traffic from the attacker subnet before the activity generated the expected audit events.

The Kali Linux machine was in the attacker zone:

`192.168.100.50`

The domain controller was in the corporate LAN:

`192.168.211.10`

Because the traffic came from a different subnet, Windows treated it as untrusted traffic and dropped it before it produced the telemetry I wanted to analyse.

To fix this for the purposes of lab testing, I created a custom inbound Windows Defender Firewall rule on the domain controller to allow traffic from the Kali Linux lab IP:

`192.168.100.50`

After applying the firewall rule and refreshing policy with:

```cmd
gpupdate /force
```

I re-ran the Nmap scan.

This time, the traffic generated Windows Filtering Platform events that were forwarded into Splunk, allowing the scan activity to be detected and analysed.

---

## Why This Matters

This troubleshooting step was important because it showed that detection depends on visibility.

If traffic is dropped before useful logs are generated, the SIEM may not show the activity, even when something is happening on the network.

This helped me understand that analysts need to think about the full path:

```text
Activity -> Logging -> Forwarding -> Indexing -> Searching -> Detection
```

If any part of that chain is missing or misconfigured, the detection may fail.

---

## Skills Practised

This part of the project helped me practise:

* Running controlled Nmap scans in a lab
* Understanding reconnaissance traffic
* Identifying common Active Directory service ports
* Reviewing Windows Filtering Platform event codes
* Searching Windows logs in Splunk
* Writing basic SPL queries
* Extracting fields with `rex`
* Aggregating event data with `stats`
* Sorting results for analysis
* Troubleshooting missing telemetry
* Connecting offensive activity to defensive evidence

---

## Security Concepts Practised

This part of the lab helped reinforce:

* Network reconnaissance
* Port scanning
* Service discovery
* Defensive monitoring
* SIEM detection logic
* Windows firewall behaviour
* Event log visibility
* Telemetry pipelines
* SOC investigation workflow

---

## What I Learned

This part of the project helped me understand how security monitoring works in practice.

Running the scan was only one part of the exercise. The more important part was proving that the activity could be seen, logged, forwarded, searched and explained.

I also learned that missing logs do not always mean nothing happened. In this case, the Windows firewall behaviour created a visibility gap until the lab was adjusted to allow the traffic to generate useful events.

The main takeaway was that detection engineering depends on understanding both the attack activity and the logging pipeline. To detect something reliably, the environment must be configured to generate the right telemetry and the SIEM must be able to search it effectively.
