# Network Traffic Analysis: DNS and ICMP Incident Report

## Objective

This project was completed as part of the Google Cybersecurity Professional Certificate.

The objective was to analyse DNS and ICMP traffic from a simulated cybersecurity incident and produce a short incident report. The scenario involved customers being unable to access a website and receiving a `destination port unreachable` error.

The activity focused on identifying which network protocols were involved, interpreting the tcpdump output, understanding the meaning of the ICMP error response, and explaining the likely cause of the incident in a clear report format.

---

## Scenario

In this scenario, customers reported that they were unable to access the website:

`www.yummyrecipesforme.com`

When attempting to load the website, users received a `destination port unreachable` error.

To investigate the issue, tcpdump was used to capture and review network traffic. The traffic showed that DNS requests were being sent using UDP, but ICMP responses were returned with the error:

`udp port 53 unreachable`

Because port 53 is commonly used for DNS traffic, this indicated a likely issue with the DNS server, the DNS service, or access to port 53.

---

## Skills Learned

- Analysing network traffic using tcpdump output
- Identifying DNS, UDP and ICMP traffic
- Understanding the role of DNS in resolving domain names to IP addresses
- Recognising that UDP port 53 is associated with DNS
- Interpreting ICMP error messages
- Understanding how network errors can affect web access
- Writing a clear cybersecurity incident report
- Explaining technical findings in a structured format
- Identifying possible next steps for escalation or further investigation

---

## Tools and Resources Used

- tcpdump packet capture output
- DNS protocol analysis
- UDP traffic review
- ICMP error message analysis
- Cybersecurity incident report template
- Google Cybersecurity Professional Certificate course materials

---

## Protocols Identified

| Protocol | Role in the Incident |
|---|---|
| DNS | Used to resolve the domain name `www.yummyrecipesforme.com` into an IP address. |
| UDP | Used to send the DNS query to the DNS server on port 53. |
| ICMP | Used to return the error message indicating that UDP port 53 was unreachable. |

---

## Network Analysis Evidence

The tcpdump output showed repeated DNS lookup attempts from the client machine `192.51.100.15` to the DNS server `203.0.113.2`.

The client attempted to query the domain `yummyrecipesforme.com` using UDP to DNS port 53. Each request received an ICMP response from the DNS server stating:

`udp port 53 unreachable`

This repeated pattern indicated that DNS resolution was failing because the DNS service was unreachable.

<img src="/projects/network-traffic-analysis-dns-icmp/images/tcpdump-dns-icmp-log.png.png" alt="tcpdump output showing UDP DNS queries and ICMP port unreachable responses" width="900">

*Ref 1: tcpdump output showing repeated UDP DNS queries to port 53 followed by ICMP `udp port 53 unreachable` responses.*

---

## Incident Report

### Part 1: Summary of the problem found in the DNS and ICMP traffic log

UDP requests to port 53 were returned as unreachable when contacting the website `www.yummyrecipesforme.com`.

This was based on the results of the network analysis, which showed ICMP error responses returning the message:

`udp port 53 unreachable`

Port 53 is used to send UDP requests to the DNS server in order to obtain the IP address for the requested web address.

This port returning an unreachable status indicates that the DNS server was not accepting or responding to UDP requests on this port. This suggests an issue with DNS resolution, which may have been caused by the DNS server being offline, the DNS service not running correctly, or firewall rules blocking traffic to port 53.

---

### Part 2: Analysis of the data and possible cause of the incident

The incident occurred shortly before 13:24.

Customers reported that they were unable to access the client’s website, `www.yummyrecipesforme.com`.

The network security team responded by analysing traffic to the website using a network analysis tool. The results showed that UDP requests to the DNS server on port 53 returned an ICMP error message:

`udp port 53 unreachable`

This happened when attempting to resolve the website IP address.

Port 53 is used for DNS requests. When a user attempts to visit a website, a DNS request is sent to resolve the web address into an IP address before the browser can connect to the web server. Because port 53 was unreachable, the browser could not complete DNS resolution and therefore could not load the website.

The likely cause of the incident is that the DNS server is offline, the DNS service is unavailable, or traffic to port 53 is being blocked by a firewall rule or misconfiguration. It could also potentially be the result of a denial of service attack affecting the DNS server, but further investigation would be needed to confirm the root cause.

Recommended next steps would be to check whether the DNS server is online, confirm that the DNS service is running, review firewall rules affecting port 53, and inspect DNS/server logs for errors or unusual traffic.

---

## Key Findings

The tcpdump output showed that DNS lookup requests were being sent to the DNS server using UDP.

Instead of receiving a successful DNS response, the system received ICMP error messages stating that UDP port 53 was unreachable.

The repeated requests at 13:24, 13:26 and 13:28 showed that the issue was persistent rather than a single failed packet.

This indicated that the DNS request could not be completed. As a result, the browser could not resolve the website domain name to an IP address, meaning the website could not be loaded.

The issue appeared to be related to DNS resolution rather than the website itself.

---

## Possible Causes

Possible causes included:

- DNS server outage
- DNS service not running
- Firewall blocking traffic to port 53
- Firewall misconfiguration affecting DNS traffic
- Denial of Service attack affecting the DNS server

At this stage of the investigation, further analysis would be needed to confirm the root cause.

---

## Recommended Next Steps

Recommended next steps included:

1. Confirm whether the DNS server is online and reachable
2. Check whether the DNS service is running correctly
3. Review firewall rules for any changes affecting port 53
4. Check DNS server logs for errors or unusual activity
5. Review network traffic for signs of excessive DNS requests
6. Escalate to security engineers or network administrators for remediation
7. Continue monitoring for repeated DNS failures or suspicious activity

---

## What I Learned

This activity helped me understand how protocol analysis can be used during incident investigation.

The main takeaway was that network traffic can provide important clues about where a failure is happening. In this case, the website itself may not have been the first issue to investigate. The tcpdump output showed that DNS resolution was failing before the browser could reach the web server.

This project also helped reinforce the importance of reading logs carefully, identifying the protocols involved, and explaining findings clearly enough that they can be escalated to the right technical team.

It also showed why analysts should avoid jumping to a single conclusion too early. The log showed that UDP port 53 was unreachable, but further checks would be needed to confirm whether the cause was a DNS server issue, firewall misconfiguration, or a denial of service condition.
