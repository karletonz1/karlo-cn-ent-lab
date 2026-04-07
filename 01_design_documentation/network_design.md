# North Star Lab Design Decisions

## Distribution/Access Layer

This layer utilizes Arista vEOS switches in order to implement MLAG and VARP to create an active-active redundant network.

The switches are fully deployed using Ansible via Arista eAPI.

- ### Multi-Chassis Link Aggregation  

    This lab uses MLAG and VARP in order to eliminate single points of failure. By using MLAG and VARP, the design achieves an active-active network state. This is through the use of resilient Virtual IP gateway for all end-host subnets, which maintains network availability by allowing for seamless fail-over to redundant links.

- ### Virtual ARP (VARP)  

    VARP is used for First-Hop Redundancy within the lab. Unlike VRRP or HSRP, which rely on an Active/Standby redundancies, VARP allows both Arista spines to route traffic simultaneously using the same Virtual MAC and Virtual IP address. This ensures that whichever spine receives a packet from a leaf switch can immediately route it.

    SVI interfaces are configured on the spines which act as the gateways for the North Star VLANs.

- ### Multiple Spanning Tree Protocol (MSTP)  

    While MLAG eliminates the need for STP to block uplinks, MSTP is deployed as a fail-safe mechanism against accidental Layer 2 loops (such as misconfigurations or physical loops on the Arista switches). With MLAG, the spines present a shared MSTP root bridge and leaf-01 and leaf-02 have been given alternate priorities of 12288 and 16384 respectively. All VLANs are mapped to MST instance 0.

- ### MTU

    Standard MTU (1500):

    1. Applied to all Management (OOB) interfaces.
    2. Applied to all WAN/Internet-facing interfaces.
    3. Applied to the Inter-Router LACP Backbone.
    4. Applied to Servers connections.  

>[!NOTE]  
>In a production environment, there would be specific instances where an MTU higher than 1500 would be needed to accommodate jumbo frames. Accounting for specific storage devices have not been applied to this lab.

## Core Layer

- This layer uses a pair of VyOS routers and acts as the routing backbone for the North Star lab. Each point-to-point (PTP) link uses a VLSM segment of the 10.0.71.0/30 network to connect between the routers and to the spines.  

- All networks are propagated via OSPF Area 0.

## Out-of-band Management

- In order to configure the devices using Ansible, an out-of-band-management (OOBM) network was needed to replicate what would be done in a production environment with a separate OOBM network. This lab simulates this using a separate GNS3 Ethernet switch connected to the network devices via a dedicated VRF management network on vlan 10.  

A bootstrap configuration is needed to configure the required settings so that Ansible can communicate with the switches.

## Network Security Considerations

- Unused network ports on the Arista switches will be placed in the Black Hole VLAN and shutdown. ***Arista enables all ports by default***

- All network devices have been configured with their own unique user for local authentication. This will be used as the fallback method once Radius has been configured in the lab.

- The following will be configured on all devices:  

    **Banner/MOTD:**  
    Mandatory legal warning for unauthorized access.

    **Management ACLs:**  
    Restrict SSH and SNMP traffic so that only the Ansible node's IP can connect. The end goal is to simulate a dedicated windows machine to simulate an authorized Administrator/Engineer's workstation being the only device able to manage into the routers.

    **SSH Hardening:**  
    :white_check_mark: Disable Telnet.  
    :white_check_mark: Disable SSH Version 1.  
    :white_check_mark: Set a Timeout Policy: Automatically kick idle sessions after 5 or 10 minutes.  

    **Administrative Security:**  
    :white_check_mark: Individual Accounts: Every admin has their own username.  
    :white_check_mark: Role-Based Access Control (RBAC):  Admins: Full config rights  |  Operators/Auditors: "Show" commands only.  

    **External Authentication:**  
    An authentication order will be configured:  
    :white_check_mark: RADIUS as primary (running on the Domain Controllers)  
    :white_check_mark: local as fallback.  

    **Logging & Accountability**  
    :white_check_mark: NTP  
    :white_check_mark: Syslog configuration  
    :white_check_mark: SNMPv3 only

## Systems Security Considerations

- Wazuh agents will be installed on clients for endpoint monitoring, and logs will be forwarded to a central Splunk dashboard.

- Nessus essentials will be deployed to scan targeting endpoints due to the limit of five IP addresses that can be scanned using this version.

- Kali Linux will be used to assist with simulating common attacks on the network, and to verify that monitoring and to determine the effectiveness of the security measures implemented within the lab.

- A DMZ will be established that will contain a windows IIS server and a Debian Apache web server. An OPNsense firewall will be used for access control, packet inspection, policy enforcement, and NAT as the edge device for internet connectivity.

## Backups

- Veeam Community edition has been chosen for backups of all the North Star servers. A second VM will be configured to act as the secondary storage target for Veeam backups.
