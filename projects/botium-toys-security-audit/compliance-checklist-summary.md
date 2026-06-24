# Compliance Checklist Summary: Botium Toys

## Overview

This document summarises the controls and compliance checklist completed as part of the mock Botium Toys security audit.

The checklist was used to review whether key security controls and compliance best practices were currently in place, based on the provided scope, goals and risk assessment report.

This summary has been rewritten in my own words for portfolio purposes.

---

## Controls Assessment

### Controls in Place

| Control                       | Category               | Summary                                                                         |
| ----------------------------- | ---------------------- | ------------------------------------------------------------------------------- |
| Firewall                      | Technical              | A firewall was in place to filter traffic using defined security rules.         |
| Antivirus software            | Technical              | Antivirus software was installed and monitored by the IT department.            |
| Locks                         | Physical / Operational | Physical locks were in place across the offices, storefront and warehouse.      |
| CCTV surveillance             | Physical / Operational | CCTV was in place to support physical security monitoring.                      |
| Fire detection and prevention | Physical / Operational | Fire detection and prevention systems were in place to protect physical assets. |

---

### Controls Not Fully in Place

| Control                                              | Category       | Risk / Impact                                                                                                                  |
| ---------------------------------------------------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| Least privilege                                      | Administrative | All employees had broad access to internally stored data, increasing the risk of unauthorised access to sensitive information. |
| Disaster recovery plans                              | Administrative | No disaster recovery plan was in place, creating a business continuity risk if systems or data were lost.                      |
| Password policies                                    | Administrative | A password policy existed, but the requirements were weak and not aligned with stronger password complexity expectations.      |
| Separation of duties                                 | Administrative | Duties were not clearly separated, increasing the risk of misuse, error or unchecked access.                                   |
| Intrusion detection system                           | Technical      | No IDS was installed, limiting the ability to detect suspicious or malicious network activity.                                 |
| Backups                                              | Technical      | Critical data was not backed up, increasing the risk of permanent data loss.                                                   |
| Manual monitoring and maintenance for legacy systems | Technical      | Legacy systems were monitored, but there was no regular schedule and intervention methods were unclear.                        |
| Encryption                                           | Technical      | Encryption was not being used to protect sensitive customer and payment information.                                           |
| Password management system                           | Technical      | No centralised password management system was in place, increasing security and productivity risks.                            |

---

## Compliance Assessment

### PCI DSS

| Best Practice                                                                   | Status  | Summary                                                                                                                    |
| ------------------------------------------------------------------------------- | ------- | -------------------------------------------------------------------------------------------------------------------------- |
| Only authorised users have access to customers’ credit card information         | Not met | Access to sensitive data was too broad, meaning cardholder data may have been accessible to employees who did not need it. |
| Credit card information is stored, accepted, processed and transmitted securely | Not met | Credit card information was stored, processed and transmitted internally without sufficient protection.                    |
| Data encryption is used to secure credit card transaction touchpoints and data  | Not met | Encryption was not in place for payment data.                                                                              |
| Secure password management policies are adopted                                 | Not met | Password requirements were weak and there was no centralised password management system.                                   |

---

### GDPR

| Best Practice                                                 | Status  | Summary                                                                                  |
| ------------------------------------------------------------- | ------- | ---------------------------------------------------------------------------------------- |
| EU customers’ data is kept private and secure                 | Not met | Customer data was not fully protected due to weak access control and lack of encryption. |
| EU customers are notified within 72 hours of a breach         | Met     | A breach notification plan was in place for EU customers.                                |
| Data is properly classified and inventoried                   | Not met | Asset and data classification needed improvement to support stronger security controls.  |
| Privacy policies and procedures are documented and maintained | Met     | Privacy policies, procedures and processes were developed and enforced.                  |

---

### SOC Type 1 / SOC Type 2

| Best Practice                              | Status  | Summary                                                                                      |
| ------------------------------------------ | ------- | -------------------------------------------------------------------------------------------- |
| User access policies are established       | Not met | Access policies were not sufficiently established or enforced.                               |
| Sensitive data is confidential and private | Not met | Sensitive data protection was not strong enough due to broad access and lack of encryption.  |
| Data integrity is maintained               | Met     | Controls were in place to support the consistency, accuracy and validation of data.          |
| Data is available to authorised users      | Not met | Availability controls were incomplete due to missing backups and disaster recovery planning. |

---

## Main Gaps Identified

The main gaps identified from the checklist were:

* No least privilege model
* No clear separation of duties
* Weak password policy requirements
* No centralised password management system
* No intrusion detection system
* No encryption for sensitive or payment data
* No disaster recovery plan
* No backups of critical data
* Legacy system monitoring lacked a clear schedule
* Asset and data classification needed improvement
* Broad internal access to customer and cardholder data
* Compliance gaps relating to PCI DSS, GDPR and SOC-type controls

---

## Priority Recommendations

Based on the checklist, the highest priority recommendations were:

1. Implement least privilege and role-based access control
2. Introduce separation of duties for sensitive processes
3. Strengthen password policy requirements
4. Implement a centralised password management system
5. Encrypt sensitive customer and payment data
6. Install and configure an intrusion detection system
7. Create and test a disaster recovery plan
8. Implement regular backups for critical data
9. Establish a scheduled process for legacy system monitoring and maintenance
10. Improve asset inventory and data classification
11. Review PCI DSS, GDPR and SOC-related compliance requirements
12. Regularly review access policies and security procedures

---

## Reflection

This checklist helped me understand how security controls connect to wider business risk.

The exercise showed that individual control gaps often combine to create larger risks. For example, broad access to internal data, weak password requirements and no encryption all increase the chance and impact of sensitive data exposure. Similarly, no backups and no disaster recovery plan create a serious risk to business continuity.

It also reinforced that security is not only technical. Policies, procedures, access reviews, documentation, physical controls and compliance requirements all play an important role in improving an organisation’s security posture.
