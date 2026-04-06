# Network Architecture Decisions

## Distribution/Access Layer

This layer utilizes Arista vEOS switches in order to implement MLAG and VARP to create an active-active redundant network.

- ### Multi-Chassis Link Aggregation  

    This lab uses MLAG and VARP in order to eliminate single points of failure. By using MLAG and VARP, the design achieves an active-active network state. This provides a resilient Virtual IP gateway for all end-host subnets, which allows for fail-over to the redundant links seamlessly.

- ### Virtual ARP (VARP)  

    VARP is used for First-Hop Redundancy within the lab. Unlike VRRP or HSRP which rely on an Active/Standby redundancies, VARP allows both Arista spines to route traffic simultaneously using the same Virtual MAC and Virtual IP address. This ensures that whichever spine receives a packet from a leaf switch can immediately route it.

- ### Multiple Spanning Tree Protocol (MSTP)  

    While MLAG eliminates the need for STP to block uplinks, MSTP is deployed as a fail-safe mechanism against accidental Layer 2 loops (e.g., misconfigurations or physical loops on the Arista switches). Due to MLAG, the Spines are presenting as a shared MSTP root bridge and leaf-01 and leaf-02 have been given alternate priorities of 12288 and 16384 respectively. All VLANs are mapped to MST instance 0.

## Core Layer

This layer uses a pair of VyOS routers and acts as the routing backbone for the North Star lab. It uses a routed PTP /30 links to connect between the routers and the spine switches.  

The spine switches are also configured with SVIs and act as the gateways for the VLANS needed by the downstream devices. 

All networks are propagated via OSPF Area 0.

### Out-of-band Management

In order to provision the devices using Ansible with only a bootstrap configuration, an out-of-band-management (OOBM) network was needed to replicate what would be done in a production environment with a separate OOBM network. This lab simulates this using a separate GNS3 Ethernet switch connected to the network devices via a dedicated VRF management network on vlan 10.  

### MTU & Performance Policy

Standard MTU (1500):

- Applied to all Management (OOB) interfaces.

- Applied to all WAN/Internet-facing interfaces.

- Applied to the Inter-Router LACP Backbone.

- Applied to Servers connections.  

>[!NOTE]  
>In a production environment, there would be specific instances where an MTU higher than 1500 would be needed to accommodate jumbo frames. Accounting for specific storage devices have not been applied to this lab.

### Security Considerations

- Unused network ports on the Arista switches will be placed in the Black Hole VLAN and shutdown. ***Arista enables all ports by default***

- All network devices have been configured with their own unique user for local authentication. This will be used as the fallback method once Radius has been configured in the lab.
