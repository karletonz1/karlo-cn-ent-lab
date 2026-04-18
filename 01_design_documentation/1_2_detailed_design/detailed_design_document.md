## Network

### Firewall Layer

This layer uses a pair of OPNSense firewalls acting in an HA pair. Due to the constraints of virtualization within GNS3, North Star is simulated as an environment connected to a single ISP via an unmanaged Ethernet 'WAN' switch that will allow both firewalls to connect to the single interface that simulates the internet within GNS3.

The firewalls will share a virtual IP through the use of Common Address Redundancy Protocol. To account for failover, the firewalls also utilize pfsync to synchronize the session state table, and XMLRPC to replicate configuration changes across an HA sync link.

### Core Layer

This layer uses a pair of VyOS routers and they act as the routing backbone for the North Star lab. There are five point-to-point (PTP) links in total and they each use a separate /30 subnet, using the 10.0.71.0 network, to connect between the routers and to the spines.  

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

In order to configure the devices using Ansible, an out-of-band-management (OOBM) network was needed to replicate what would be done in a production environment with a separate OOBM network. This lab simulates this using a separate GNS3 Ethernet switch connected to the network devices via a dedicated VRF management network on vlan 10.  
