# Purpose

North Star will align to NIST 800-53 framework as the primary control document with supplementary standards to achieve the controls listed under each section below.

## Scope

**Network infrastructure**  
 Referenced Standards:  

- NIST 800-41 Rev.1 (Guidelines on Firewalls and Firewall Policy)
- NIST 800-53 Rev.5 (Security and Privacy Controls for Information Systems and Organizations)
- NIST 800-92 (Guide to Computer Security Log Management)
- NIST 800-115 (Technical Guide to Information Security Testing and Assessment)
- NIST 800-128 (Guide for Security-Focused Configuration Management of Information Systems)
- NIST 800-70 Rev.4 (National Checklist Program for IT Products: Guidelines for Administrator of Security Configuration Checklists)
- NIST 800-123 Information Security Continuous Monitoring (ISCM) for Federal Information Systems and Organizations
- NIST 800-125 Guide to Security for Full Virtualization Technologies
- NIST 800-137 Guide to General Server Security

### Implementation Summary Table

| Device Type | NIST Standard | Control | Specific Device |
| ----------- | ------------- | ------- | --------------- |
| Baseline - Common | | | |
| Network Devices | | | |
| Hypervisors | | | |
| Endpoints | | | |
| Security | | | |

### Hardening Controls (Expanded)

**Proxmox Windows and Linux host servers with virtual machines**  
Referenced Standards:  

- NIST 800-53 Rev.5 (Security and Privacy Controls for Information Systems and Organizations)

Establish a baseline configuration for all network infrastructure devices including secure configuration checklists  
`Reference: NIST 800-128 (Section 2.3.7 Baseline Configuration)`  
`Reference: NIST 800-53 Rev. 5 (Section 3.5 Configuration Management: CM-2 Baseline Configuration)`  
`Reference: NIST 800-70 Rev. 4 (Section 2.1 Security Configuration Checklists)`  

Default deny-all inbound and outbound traffic that has not been expressly permitted by a firewall policy  
`Reference: NIST 800-41 Rev. 1 (Section 4: Firewall Policy)`  
`Reference: NIST 800-53 Rev. 5 (Section 3.18 System and Communication Protection: SC-7 Boundary Protection)`  

Disable unused ports and services.  
`Reference: NIST 800-53 Rev. 5 (Section 3.5 Configuration Management: CM-7 Least Functionality)`  
`Reference: NIST 800-128 (Appendix F: Best Practices for Establishing Secure Configurations)`  

Implement network zones (LAN, DMZ, WAN)  
`Reference: NIST 800-41 Rev. 1 (Section 3.1 Network Layouts with Firewalls)`  

Enable security system events and audit record logs  
`Reference: NIST 800-92 (Section 2.1.1 Security Software and Section 2.1.2 Operating Systems)`  
`Reference: NIST 800-53 Rev. 5 (Section 3.3 Audit and Accountability: AU-2 Event Logging)`  

Implement Radius authentication with local fallback  
`Reference: NIST 800-53 Rev. 5 (Section 3.7 Identification and Authentication: IA-2 Identification and Authentication`  
`Reference: NIST 800-53 Rev. 5 (Section 3.1 Access Control: AC-2 Account Management)`  

Apply ACLs to allow only the Ansible host IP to SSH into the management plane using a dedicated VRF management VLAN  
`Reference: NIST 800-53 Rev. 5 (Section 3.1 Access Control: AC-6:(1) Least Privilege - Authorized Access to Security Functions)`  
`Reference: NIST 800-53 Rev. 5 (Section 3.18 System and Communication Protection: SC-7:(5) Boundary Protection: Deny by Default-Allow by Exception)`  
`Reference: NIST 800-53 Rev. 5 (Section 3.18 System and Communications Protection: SC-2 Separation of System and User Functionality)`  

Disable Telnet, enforce SSH-only management access, and session timeouts  
`Reference: NIST 800-53 Rev. 5 (Section 3.18 System and Communications Protection: SC-8 Transmission Confidentiality and Integrity)`  
`Reference: NIST 800-53 Rev. 5 (Section 3.1 Access Control: AC-12 Session Termination)`  

Configure OSPF Authentication  
`Reference: NIST 800-53 Rev. 5 (Section 3.7 Identification and Authentication: IA-3 Device Identification and Authentication)`  

Implement VLAN segmentation  
`Reference: NIST 800-53 Rev. 5 (Section 3.18 System and Communication Protection: SC-7 Boundary Protection)`  

Control plane protections using boundary protection devices  
`Reference: NIST 800-53 Rev. 5 (Section 3.18 System and Communication Protection: SC-5 Denial of Service Protection`  
`Reference: NIST 800-53 Rev. 5 (Section 3.18 System and Communication Protection: SC-7:(9) Boundary Protection: Restrict Threatening Outgoing Communications Traffic)`  

Centralized logging and Restrict Privileged Users to Read-only  
`Reference: NIST 800-92 (Section 3.1 Log Management Infrastructure: Architecture)`  
`Reference: NIST 800-92 (Section 2.3.1 Log Generation and Storage)`  
`Reference: NIST 800-53 Rev. 5 (Section 3.3 Audit and Accountability: AU-9:(6) Protection of Audit Information: Read-only Access)`  

**Proxmox Windows and Linux host servers with virtual machines**  
Referenced Standards:  

- NIST 800-53 Rev.5 (Security and Privacy Controls for Information Systems and Organizations)
- NIST 800-123 (Guide to General Server Security)
- NIST 800-128 (Guide for Security-Focused Configuration Management of Information Systems)
- NIST 800-70 Rev.4 (National Checklist Program for IT Products: Guidelines for Administrator of Security Configuration Checklists)
- NIST 800-125 (Guide to Security for Full Virtualization Technologies)
- NIST 800-92 (Guide to Computer Security Log Management)

Hardening Controls:  
Apply secure baseline configurations  
`Reference: NIST 800-53 Rev. 5 (Section 3.5 Configuration Management: CM-2 Baseline Configuration)`  
`Reference: NIST 800-70 Rev. 4 (Section 2.1 Security Configuration Checklists)`  

Remove unnecessary services  
`Reference: NIST 800-53 Rev. 5 (Section 3.5 Configuration Management: CM-7 Least Functionality)`  
`Reference: NIST 800-123 (Section 4.2 Hardening and Securely Configuring the OS)`  

Enforce least privilege for Administrator roles to configure and access servers  
`Reference: NIST 800-53 Rev. 5 (Section 3.1 Access Control: AC-6 Least Privilege)`  

Apply regular patch management  
`Reference: NIST 800-53 Rev. 5 (Section 3.19 System and Information Integrity: SI-2 Flaw Remediation)`  

Enable System logging  
`Reference: NIST 800-53 Rev. 5 (Section 3.3 Audit and Accountability: AU-2 Event Logging)`  
`Reference: NIST 800-92 (Section 2.1.2 Operating Systems)`  

Forward logs to centralized SIEM  
`Reference: NIST 800-53 Rev. 5 (Section 3.3 Audit and Accountability: AU-9 Protection of Audit Information)`  
`Reference: NIST 800-92 (Section 3.1 Log Management Infrastructure: Architecture)`  

Enforce separate subnets to isolate system functions
`Reference: NIST 800-125 (Section 4.3 Virtualized Infrastructure Security)`  
`Reference: NIST 800-53 Rev. 5 (Section 3.18 System and Communication Protection: SC-7:(29) Boundary Protection - Separate Subnets to isolate Functions )`  

Secure the hypervisor management interface to authorized Administrators only
`Reference: NIST 800-125 (Section 4.1 Hypervisor Management)`  
`Reference: NIST 800-53 Rev. 5 (Section 3.18 System and Communication Protection: SC-2 Separation of System and User Functionality)`  

**Endpoints including the Windows 10 and Linux clients**  
Referenced Standards:

- NIST 800-53 Rev.5 (Security and Privacy Controls for Information Systems and Organizations)
- NIST 800-92 (Guide to Computer Security Log Management)
- NIST 800-115 (Technical Guide to Information Security Testing and Assessment)
- NIST 800-137 (Information Security Continuous Monitoring for Federal Information Systems and Organizations)

Hardening Controls:  
Apply regular patch management  
`Reference: NIST 800-53 Rev. 5 (Section 3.19 System and Information Integrity: SI-2 Flaw Remediation)`  

Enable continuous monitoring for endpoints  
`Reference: NIST 800-53 Rev. 5 (Section 3.19 System and Information Integrity: SI-4 System Monitoring)`  
`Reference: NIST 800-137 (Section 3.3 Implement an ISCM Program)`  

Forward logs to centralized SIEM  
`Reference: NIST 800-92 (Section 2.1.3 Applications)`  
`Reference: NIST 800-53 Rev. 5 (Section 3.3 Audit and Accountability: AU-9:(2) Protection of Audit Information - Store on Separate Physical Systems or Components)`  

Conduct regular vulnerability scanning  
`Reference: NIST 800-115 (Section 4.3 Vulnerability Scanning)`  
`Reference: NIST 800-53 Rev. 5 (Section 3.16 Risk Assessment: RA-5 Vulnerability Monitoring and Scanning)`  

**Security Monitoring and Logging Systems**  
Referenced Standards:

- NIST 800-137 (Information Security Continuous Monitoring for Federal Information Systems and Organizations)
- NIST 800-92 (Guide to Computer Security Log Management)
- NIST 800-53 Rev.5 (Security and Privacy Controls for Information Systems and Organizations)

Hardening Controls:  
Enable continuous monitoring across the entire lab  
`Reference: NIST 800-53 Rev. 5 (Section 3.19 System and Information Integrity: SI-4 System Monitoring)`  
`Reference: NIST 800-137 (Section 3.3 Implement an ISCM Program)`  

Correlate logs across ntopng, Grafana, and Splunk dashboards  
`Reference: NIST 800-92 (Section 3.1 Log Management Infrastructure: Architecture)`  
`Reference: NIST 800-53 Rev. 5 (Section 3.3 Audit and Accountability: AU-9:(2) Protection of Audit Information - Store on Separate Physical Systems or Components)`  
`Reference: NIST 800-53 Rev. 5 (Section 3.3 Audit and Accountability: AU-6:(3) Audit Record Review, Analysis, and Reporting - Correlate Audit Record Repositories)`  

Monitor authentication events  
`Reference: NIST 800-53 Rev. 5 (Section 3.3 Audit and Accountability: AU-2 Event Logging)`  
`Reference: NIST 800-53 Rev. 5 (Section 3.3 Audit and Accountability: AU-3 Content of Audit Records)`  
`Reference: NIST 800-53 Rev. 5 (Section 3.3 Audit and Accountability: AU-6 Audit Record Review, Analysis, and Reporting)`  

Monitor network traffic anomalies  
`Reference: NIST 800-53 Rev. 5 (Section 3.19 System and Information Integrity: SI-4:(4) System Monitoring: Inbound and Outbound Communications Traffic)`

Retain logs for investigation purposes or information retention requirements  
`Reference: NIST 800-53 Rev. 5 (Section 3.3 Audit and Accountability: AU-11 Audit Record Retention)`  
`Reference: NIST 800-92 (Section 3.2 Functions - Storage)`  

Deploy security applications in their own subnet  
`Reference: NIST 800-53 Rev. 5 (Section 3.18 System and Communication Protection: SC-7:(13) Boundary Protection: Isolation of Security Tools, Mechanisms, and Support Components)`  
