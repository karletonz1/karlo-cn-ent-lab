# North Star Lab

## Network Architecture

### Firewall Layer

This layer uses a pair of OPNSense firewalls acting in an HA pair. Due to the constraints of virtualization within GNS3, North Star is simulated as an environment connected to a single ISP via an unmanaged Ethernet 'WAN' switch that will allow both firewalls to connect to the single interface that simulates the internet within GNS3.

The firewalls will share a virtual IP through the use of Common Address Redundancy Protocol. To account for failover, the firewalls also utilize pfsync to synchronize the session state table, and XMLRPC to replicate configuration changes across an HA sync link.

### Core Layer

This layer uses a pair of VyOS routers and they act as the routing backbone for the North Star lab. Each point-to-point (PTP) link uses a VLSM segment of the 10.0.71.0/30 network to connect between the routers and to the spines.  

All networks are propagated via OSPF Area 0.

The routers are fully deployed using the `ansible_connection: network_cli` plugin.

### Distribution and Access Layer

This layer utilizes Arista vEOS switches in order to implement MLAG and VARP to create an active-active redundant network.

The switches are fully deployed using the `ansible_connection: httpapi` plugin.

- **Multi-Chassis Link Aggregation**  

    This lab uses MLAG and VARP in order to eliminate single points of failure. By using MLAG and VARP, the design achieves an active-active network state. This is through the use of resilient Virtual IP gateway for all end-host subnets, which maintains network availability by allowing for seamless fail-over to redundant links.

- **Virtual ARP (VARP)**  

    VARP is used for First-Hop Redundancy within the lab. Unlike VRRP or HSRP, which rely on an Active/Standby redundancies, VARP allows both Arista spines to route traffic simultaneously using the same Virtual MAC and Virtual IP address. This ensures that whichever spine receives a packet from a leaf switch can immediately route it.

    SVI interfaces are configured on the spines which act as the gateways for the North Star VLANs.

- **Multiple Spanning Tree Protocol (MSTP)**  

    While MLAG eliminates the need for STP to block uplinks, MSTP is deployed as a fail-safe mechanism against accidental Layer 2 loops (such as misconfigurations or physical loops on the Arista switches). With MLAG, the spines present a shared MSTP root bridge and leaf-01 and leaf-02 have been given alternate priorities of 12288 and 16384 respectively. All VLANs are mapped to MST instance 0.

- **MTU**  

    Standard MTU 1500 across all interfaces.

### Out-of-band Management

- In order to configure the devices using Ansible, an out-of-band-management (OOBM) network was needed to replicate what would be done in a production environment with a separate OOBM network. This lab simulates this using a separate GNS3 Ethernet switch connected to the network devices via a dedicated VRF management network on vlan 10.  

A bootstrap configuration is needed to configure the required settings so that Ansible can communicate with the switches.

## Security Standards Framework

### Purpose

North Star will align to NIST 800-53 framework as the primary control document with supplementary standards to achieve the controls listed under each section below.

### Scope

**Network infrastructure (VyOS routers, Arista vEOS spines/leafs, and OPNsense firewalls)**  
 Referenced Standards:  

- NIST 800-41 Rev.1 (Guidelines on Firewalls and Firewall Policy)
- NIST 800-53 Rev.5 (Security and Privacy Controls for Information Systems and Organizations)
- NIST 800-92 (Guide to Computer Security Log Management)
- NIST 800-115 (Technical Guide to Information Security Testing and Assessment)
- NIST 800-128 (Guide for Security-Focused Configuration Management of Information Systems)

Hardening Controls:  

Establish a baseline configuration for all network infrastructure devices  
`Reference: NIST 800-128 (Section 2.3.7 Baseline Configuration)`  
`Reference: NIST 800-53 Rev. 5 (Section 3.5 Configuration Management: CM-2 Baseline Configuration)`  

Default deny-all inbound and outbound traffic that has not been expressly permitted by a firewall policy  
`Reference: NIST 800-41 Rev. 1 (Section 4: Firewall Policy)`  
`Reference: NIST 800-53 Rev. 5 (Section 3.18 System and Communication Protection: SC-7 Boundary Protection)`  

Disable unused ports and services.  
`Reference: NIST 800-53 Rev. 5 (Section 3.5 Configuration Management: CM-7 Least Functionality)`  
`Reference: NIST-800-128 (Appendix F: Best Practices for Establishing Secure Configurations)`  

Implement network zones (LAN, DMZ, WAN)  
`Reference: NIST 800-41 Rev. 1 (Section 3.1 Network Layouts with Firewalls)`  

Enable security system events and audit record logs  
`Reference: NIST 800-92 (Section 2.1.1 Security Software/Section 2.1.2 Operating Systems)`  
`Reference: NIST 800-53 Rev. 5 (Section 3.3 Audit and Accountability: AU-2 Event Logging)`  

Implement Radius authentication with local fallback  
`Reference: NIST 800-53 Rev. 5 (Section 3.7 Identification and Authentication: IA-2 Identification and Authentication`  
`Reference: NIST 800-53 Rev. 5 (Section 3.1 Access Control: AC-2 Account Management)`  

Apply ACLs to allow only the Ansible host IP to SSH into the management plane using a dedicated VRF management VLAN  
`Reference: NIST 800-41 Rev. 1 (Section 4.1 Policies Based on IP addresses and Protocols)`  
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
`Reference: NIST 800-92 (Section 2.3.1 Introduction to Computer Security Log Management: Log Generation and Storage)`  
`Reference: NIST 800-53 Rev. 5 (Section 3.3 Audit and Accountability: AU-9:(6) Protection of Audit Information: Read-only Access)`  

**Proxmox Windows and Linux host servers with VMS**  
Additional Referenced Standards:  

- NIST 800-53 Rev.5 (Security and Privacy Controls for Information Systems and Organizations)
- NIST 800-123 (Guide to General Server Security)
- NIST 800-128 (Guide for Security-Focused Configuration Management of Information Systems)
- NIST 800-70 Rev.4 (National Checklist Program for IT Products: Guidelines for Administrator of Security Configuration Checklists)
- NIST 800-125A (Security Recommendations for Server-based Hypervisor Platforms)
- NIST 800-92 (Guide to Computer Security Log Management)

Hardening Controls:  

Apply secure baseline configurations  

Remove unnecessary services  

Enforce least privilege  

Apply regular patch management  
`Reference: NIST 800-53 Rev. 5 (Section 3.19 System and Information Integrity: SI-2 Flaw Remediation)`  

Enable System logging  
`Reference: NIST 800-53 Rev. 5 (Section 3.3 Audit and Accountability: AU-2 Event Logging)`  

Forward logs to centralized SIEM  

Enforce virtual machine network isolation  

Secure hypervisor management interface  

**Endpoints including the Windows 10 and Linux clients**  
Referenced Standards:

- NIST 800-53 Rev.5 (Security and Privacy Controls for Information Systems and Organizations)
- NIST 800-137 (Information Security Continuous Monitoring for Federal Information Systems and Organizations)
- NIST 800-92 (Guide to Computer Security Log Management)
- NIST 800-115 (Technical Guide to Information Security Testing and Assessment)

Hardening Controls:  

Apply regular patch management  
`Reference: NIST 800-53 Rev. 5 (Section 3.19 System and Information Integrity: SI-2 Flaw Remediation)`   

Enable continuous monitoring  
`Reference: NIST 800-53 Rev. 5 (Section 3.29 System and Information Integrity: SI-4 System Monitoring)`  

Forward logs to SIEM  
Reference: NIST 800-92 (Section 2.1.3 Applications)  

Conduct vulnerability scanning  
`Reference: NIST 800-115 (Section 4.3 Vulnerability Scanning)`  
`Reference: NIST 800-53 Rev. 5 (Section 3.16 Risk Assessment: RA-5 Vulnerability Monitoring and Scanning)`  

**Security Monitoring and Logging Systems**  
Referenced Standards:

- NIST 800-137 (Information Security Continuous Monitoring for Federal Information Systems and Organizations)
- NIST 800-92 (Guide to Computer Security Log Management)
- NIST 800-53 Rev.5 (Security and Privacy Controls for Information Systems and Organizations)

Hardening Controls:  

Centralize logs in SIEM  

Monitor authentication events  

Monitor network traffic anomalies  

Retain logs per policy  

Deploy security applications in their own subnet  
`Reference: NIST 800-53 Rev. 5 (Section 3.18 System and Communication Protection: SC-7:(13) Boundary Protection: Isolation of Security Tools, Mechanisms, and Support Components)`  
