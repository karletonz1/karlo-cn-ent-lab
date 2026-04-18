# IP Addressing and VLANs

This document defines the logical architecture and IP addressing scheme.

## Subnet Allocation  

### VLANs

| Vlan ID | Vlan Name | Description | Subnet | Members |
| ------- | --------- | ----------- | ------ | ------- |
| 10 | Mgmt | OOB Management/Ansible | 10.0.10.0/24 | All Management IPs |
| 20 | Core | Active Directory, DNS | 10.0.20.0/24 | DC1, DC2 |
| 30 | Corp_users | User Trusted Endpoints | 10.0.30.0/24 | Windows and Linux Clients |
| 40 | Corp_apps | Corporation Application Servers | 10.0.40.0/24 | All Corp Servers |
| 50 | Sec | Security Monitoring Applications | 10.0.50.0/24 | Wazuh, Splunk, Nessus |
| 60 | DMZ_EXT | Public facing Web Servers | 10.0.60.0/24 | Web Servers |
| 70 | Backups | Storage and Backup | 10.0.70.0/24 | Veeam Server |
| 80 | Spine MLAG Peer | 10.0.80.0/30 |
| 666 | Black Hole Sun | - |

> [!NOTE]
> VLAN 666: Used as a "Black Hole" Native VLAN for all trunk ports**

### Router Subnets

| Link | Subnet | Purpose |  
| ---- | ------ | ------- |
| Link 1 | 10.0.71.0/30 | Router-01 to Spine-01 PTP link |
| Link 2 | 10.0.71.4/30 | Router-01 to Spine-02 PTP link |
| Link 3 | 10.0.71.8/30 | Router-02 to Spine-02 PTP link |
| Link 4 | 10.0.71.12/30 | Router-02 to Spine-01 PTP link |
| Sync | 10.0.71.16/30 | Router-01 to Router-02 PTP Bond0 Link |

### Firewall Subnets

| Link | Subnet | Purpose |  
| ---- | ------ | ------- |
| Link 6 | 10.0.72.0/30 | Firewall-01 to Router-01 PTP link |
| Link 7 | 10.0.72.4/30 | Firewall-01 to Router-02 PTP link |
| Link 8 | 10.0.72.8/30 | Firewall-02 to Router-01 PTP link |
| Link 9 | 10.0.72.12/30 | Firewall-02 to Router-02 PTP link |
| Sync | 10.0.72.16/30 | Firewall-01 to Firewall-01 PTP Sync link |
| WAN | 10.0.72.24/29 | Firewall to WAN CARP Gateway link |

## SVI Allocations

### Spine 01 SVI Address

| Hostname | VLAN-ID/SVI | VLAN Name | Network Address | IP Address | VIP (GW) | Virtual MAC | MTU | Description | Primary Nodes |  
| -------- | ----------- | --------- | --------------- | ---------- | -------- | ----------- | --- | ----------- | ------------- |
| karlo-cn-spine-01 | 10 | INFRA-MGMT | 10.0.10.0/24 | 10.0.10.1 | 10.0.10.254 | 00:1c:73:00:00:01 | 1500 | SVI IP address for Infra-MGT Subnet VARP Gateway | VyOS, OPNsense, OVS, Ansible |
| karlo-cn-spine-01 | 11 | SRV-MGMT | 10.0.11.0/24 | 10.0.11.1 | 10.0.11.254 | 00:1c:73:00:00:01 | 1500 | SVI IP address for SRV-MGT Subnet VARP Gateway | Proxmox Host, Veeam, Domain Controllers |
| karlo-cn-spine-01 | 20 | WIN-CLIENTS | 10.0.20.0/24 | 10.0.20.1 | 10.0.20.254 | 00:1c:73:00:00:01 | 1500 | SVI IP address for WIN-CLIENTS Subnet VARP Gateway | Windows 10 Workstations |
| karlo-cn-spine-01 | 21 | LIN-CLIENTS | 10.0.21.0/24 | 10.0.21.1 | 10.0.21.254 | 00:1c:73:00:00:01 | 1500 | SVI IP address for LIN-CLIENTS Subnet VARP Gateway | Rocky Linux Workstations |
| karlo-cn-spine-01 | 30 | SEC-APPS | 10.0.30.0/24 | 10.0.30.1 | 10.0.30.254 | 00:1c:73:00:00:01 | 1500 | SVI IP address for SEC-APPS Subnet VARP Gateway | Wazuh Manager, Splunk, Tenable, Kali Linux |
| karlo-cn-spine-01 | 40 | DMZ | 10.0.40.0/24 | 10.0.40.1 | 10.0.40.254 | 00:1c:73:00:00:01 | 1500 | SVI IP address for DMZ Subnet VARP Gateway | Apache & IIS Web Servers |
| karlo-cn-spine-01 | 50 | PRD-SVRS | 10.0.50.0/24 | 10.0.50.1 | 10.0.50.254 | 00:1c:73:00:00:01 | 1500 | SVI IP address for PRD-SRVS Subnet VARP Gateway | Simulated App Servers (Proxmox) |
| karlo-cn-spine-01 | 60 | BACKUPS | 10.0.60.0/24 | 10.0.60.1 | 10.0.60.254 | 00:1c:73:00:00:01 | 1500 | SVI IP address for BACKUPS Subnet VARP Gateway | Veeam Backups |
| karlo-cn-spine-01 | 70 | SPINE MLAG PEER | 10.0.70.0/30 | 10.0.70.1 | - | - | 1500 | SVI IP for MLAG between the spine switches | Spine-01 and Spine-02 |

### Spine 02 SVI Address

| Hostname | VLAN-ID/SVI | VLAN Name | Network Address | IP Address | VIP (GW) | Virtual MAC | MTU | Description | Primary Nodes |  
| -------- | ----------- | --------- | --------------- | ---------- | -------- | ----------- | --- | ----------- | ------------- |
| karlo-cn-spine-02 | 10 | INFRA-MGMT | 10.0.10.0/24 | 10.0.10.2 | 10.0.10.254 | 00:1c:73:00:00:01 | 1500 | SVI IP address for Infra-MGT Subnet VARP Gateway | VyOS, OPNsense, OVS, Ansible |
| karlo-cn-spine-02 | 11 | SRV-MGMT | 10.0.11.0/24 | 10.0.11.2 | 10.0.11.254 | 00:1c:73:00:00:01 | 1500 | SVI IP address for SRV-MGT Subnet VARP Gateway | Proxmox Host, Veeam, Domain Controllers |
| karlo-cn-spine-02 | 20 | WIN-CLIENTS | 10.0.20.0/24 | 10.0.20.2 | 10.0.20.254 | 00:1c:73:00:00:01 | 1500 | SVI IP address for WIN-CLIENTS Subnet VARP Gateway | Windows 10 Workstations |
| karlo-cn-spine-02 | 21 | LIN-CLIENTS | 10.0.21.0/24 | 10.0.21.2 | 10.0.21.254 | 00:1c:73:00:00:01 | 1500 | SVI IP address for LIN-CLIENTS Subnet VARP Gateway | Rocky Linux Workstations |
| karlo-cn-spine-02 | 30 | SEC-APPS | 10.0.30.0/24 | 10.0.30.2 | 10.0.30.254 | 00:1c:73:00:00:01 | 1500 | SVI IP address for SEC-APPS Subnet VARP Gateway | Wazuh Manager, Splunk, Tenable, Kali Linux |
| karlo-cn-spine-02 | 40 | DMZ | 10.0.40.0/24 | 10.0.40.2 | 10.0.40.254 | 00:1c:73:00:00:01 | 1500 | SVI IP address for DMZ Subnet VARP Gateway | Apache & IIS Web Servers |
| karlo-cn-spine-02 | 50 | PRD-SVRS | 10.0.50.0/24 | 10.0.50.2 | 10.0.50.254 | 00:1c:73:00:00:01 | 1500 | SVI IP address for PRD-SRVS Subnet VARP Gateway | Simulated App Servers (Proxmox) |
| karlo-cn-spine-02 | 60 | BACKUPS | 10.0.60.0/24 | 10.0.60.2 | 10.0.60.254 | 00:1c:73:00:00:01 | 1500 | SVI IP address for BACKUPS Subnet VARP Gateway | Veeam Backups |
| karlo-cn-spine-02 | 70 | SPINE MLAG PEER | 10.0.70.0/30 | 10.0.70.2 | - | - | 1500 | SVI IP for MLAG between the spine switches | Spine-01 and Spine-02 |

## IP Address Allocations

### Firewall IP Address

| Hostname | GNS3 Port | Network Address | IP Address | Broadcast Address | MTU | Role | Link type |
| -------- | --------- | --------------- | ---------- | ----------------- | --- | ---- | --------- |
| karlo-cn-fw-01 | vtnet0 | 10.0.72.24/29 | 10.0.72.25/29 | 10.0.72.31/29 | 1500 | Firewall-01 WAN Link | PTP |
| karlo-cn-fw-01 | vtnet1 | 10.0.72.16/30 | 10.0.72.17/30 | 10.0.72.19/30 | 1500 | PTP Sync link between Firewall-01 and Firewall-02 | PTP |
| karlo-cn-fw-01 | vtnet2 | 10.0.72.0/30 | 10.0.72.1/30 | 10.0.72.3/30 | 1500 | PTP link between Firewall-01 and Router-01 | PTP |
| karlo-cn-fw-01 | vtnet3 | 10.0.72.4/30 | 10.0.72.5/30 | 10.0.72.7/30 | 1500 | PTP link between Firewall-01 and Router-02 | PTP |
| karlo-cn-fw-02 | vtnet0 | 10.0.72.24/29 | 10.0.72.26/29 | 10.0.72.31/29 | 1500 | Firewall-02 WAN Link | PTP |
| karlo-cn-fw-02 | vtnet1 | 10.0.72.16/30 | 10.0.72.18/30 | 10.0.72.19/30 | 1500 | PTP Sync link between Firewall-02 and Firewall-01 | PTP |
| karlo-cn-fw-02 | vtnet2 | 10.0.72.8/30 | 10.0.72.9/30 | 10.0.72.11/30 | 1500 | PTP link between Firewall-02 and Router-01 | PTP |
| karlo-cn-fw-02 | vtnet3 | 10.0.72.12/30 | 10.0.72.13/30 | 10.0.72.15/30 | 1500 | PTP link between Firewall-02 and Router-02 | PTP |

### Router PTP Address

| Hostname | GNS3 Port | Network Address | IP Address | Broadcast Address | MTU | Role | Link type |
| -------- | --------- | --------------- | ---------- | ----------------- | --- | ---- | --------- |
| karlo-cn-rtr-01 | eth1 | 10.0.71.0/30 | 10.0.71.1/30 | 10.0.71.3/30 | 1500 | PTP link between Router-01 and Spine-01 | PTP |
| karlo-cn-rtr-01 | eth2 | 10.0.71.4/30 | 10.0.71.5/30 | 10.0.71.7/30 | 1500 | PTP link between Router-01 and Spine-02 | PTP |
| karlo-cn-rtr-01 | eth4 | 10.0.72.0/30 | 10.0.72.2/30 | 10.0.72.3/30 | 1500 | PTP link between Router-01 and firewall-01 | PTP |
| karlo-cn-rtr-01 | eth5 | 10.0.72.8/30 | 10.0.72.10/30 | 10.0.72.11/30 | 1500 | PTP link between Router-01 and firewall-02 | PTP |
| karlo-cn-rtr-01 | eth8/9:bond0 | 10.0.71.16/30 | 10.0.71.17/30 | 10.0.71.19/30 | 1500 | LAG Link between RTR-01/02 | PTP |
| karlo-cn-rtr-02 | eth1 | 10.0.71.8/30 | 10.0.71.9/30 | 10.0.71.11/30 | 1500 | PTP link between Router-02 and Spine-02 | PTP |
| karlo-cn-rtr-02 | eth2 | 10.0.71.12/30 | 10.0.71.13/30 | 10.0.71.15/30 | 1500 | PTP link between Router-02 and Spine-01 | PTP |
| karlo-cn-rtr-02 | eth4 | 10.0.72.12/30 | 10.0.72.14/30 | 10.0.72.15/30 | 1500 | PTP link between Router-02 and Firewall-02 | PTP |
| karlo-cn-rtr-02 | eth5 | 10.0.72.4/30 | 10.0.72.6/30 | 10.0.72.7/30 | 1500 | PTP link between Router-02 and Firewall-01 | PTP |
| karlo-cn-rtr-02 | eth8/9:bond0 | 10.0.71.16/30 | 10.0.71.18/30 | 10.0.71.19/30 | 1500 | LAG Link between RTR-01/02 | PTP |

### Spine PTP Address

| Hostname | GNS3 Port | Network Address | IP Address | Broadcast Address | MTU | Role | Link type |
| -------- | --------- | --------------- | ---------- | ----------------- | ---- | ---- | -------- |
| karlo-cn-spine-01 | eth1 | 10.0.71.0/30 | 10.0.71.2/30 | 10.0.71.3/30 | 1500 | PTP link between Spine-01 and Router-01 | PTP |
| karlo-cn-spine-01 | eth2 | 10.0.71.12/30 | 10.0.71.14/30 | 10.0.71.15/30 | 1500 | PTP link between Spine-01 and Router-02 | PTP |
| karlo-cn-spine-02 | eth1 | 10.0.71.8/30 | 10.0.71.10/30 | 10.0.71.11/30 | 1500 | PTP link between Spine-02 and Router-02 | PTP |
| karlo-cn-spine-02 | eth2 | 10.0.71.4/30 | 10.0.71.6/30 | 10.0.71.7/30 | 1500 | PTP link between Spine-02 and Router-01 | PTP |

## Port-Channel Allocations

### Spine Port-Channel

| Hostname | GNS3 Port | Logical Interface | Allowed Vlan | Role | Link type |
| -------- | --------- | ----------------- | ------------ | ---- | --------- |
| karlo-cn-spine-01 | eth3 | Port-channel 70 | 10,11,20,21,30,40,50,60,666,70 | MLAG peer link to secondary | Trunk |
| karlo-cn-spine-01 | eth4 | Port-channel 70 | 10,11,20,21,30,40,50,60,666,70 | MLAG peer link to secondary | Trunk |
| karlo-cn-spine-01 | eth5 | Port-channel 10 | 10,11,20,21,30,40,50,60,666 | Downlink to karlo-cn-leaf-01 | Trunk |
| karlo-cn-spine-01 | eth6 | Port-channel 20 | 10,11,20,21,30,40,50,60,666 | Downlink to karlo-cn-leaf-02 | Trunk |
| karlo-cn-spine-02 | eth3 | Port-channel 70 | 10,11,20,21,30,40,50,60,666,70 | MLAG peer link to primary | Trunk |
| karlo-cn-spine-02 | eth4 | Port-channel 70 | 10,11,20,21,30,40,50,60,666,70 | MLAG peer link to primary | Trunk |
| karlo-cn-spine-02 | eth5 | Port-channel 20 | 10,11,20,21,30,40,50,60,666 | Downlink to karlo-cn-leaf-02 | Trunk |
| karlo-cn-spine-02 | eth6 | Port-channel 10 | 10,11,20,21,30,40,50,60,666 | Downlink to karlo-cn-leaf-01 | Trunk |

### Leaf Port-Channel

| Hostname | GNS3 Port | Logical Interface | Allowed Vlan | Role | Link type |
| -------- | --------- | ----------------- | ------------ | ---- | --------- |
| karlo-cn-leaf-01 | eth5 | Port-channel 10 | 10,11,20,21,30,40,50,60,666 | Port-Channel uplink to karlo-spine-01 | Trunk |
| karlo-cn-leaf-01 | eth6 | Port-channel 10 | 10,11,20,21,30,40,50,60,666 | Port-Channel uplink to karlo-spine-02 | Trunk |
| karlo-cn-leaf-01 | eth12 | e0 | 20 | Access uplink to karlo-leaf-01 | Access |
| karlo-cn-leaf-02 | eth5 | Port-channel 20 | 10,11,20,21,30,40,50,60,666 | Port-Channel uplink to karlo-spine-02 | Trunk |
| karlo-cn-leaf-02 | eth6 | Port-channel 20 | 10,11,20,21,30,40,50,60,666 | Port-Channel uplink to karlo-spine-01 | Trunk |

## Endpoint Layer 3 IP Addressing

### Server IP Address

| Hostname | GNS3 Port | IP Address | Vlan ID | Subnet | Gateway | MTU | Role | Link type |
| -------- | --------- | ---------- | ------- | ------ | ------- | --- | ---- | -------- |
| karlo-cn-kvm-01 | eth0 | 10.0.50.100 | 50 | 10.0.50.0/24 | 10.0.50.254 | 1500 | Uplink to karlo-cn-leaf-01 | Access |
| karlo-cn-kvm-02 | eth0 | 10.0.50.200 | 50 | 10.0.50.0/24 | 10.0.50.254 | 1500 | Uplink to karlo-cn-leaf-02 | Access |

### Client IP Address

| Hostname | GNS3 Port | IP Address | Vlan ID | Subnet | Gateway | MTU | Role | Link type |
| -------- | --------- | ---------- | ------- | ------ | ------- | --- | ---- | -------- |
| karlo-cn-ansible | eth0 | 10.0.10.253 | 10 | 10.0.10.0/24 | 10.0.10.254 | 1500 | Uplink to karlo-cn-leaf-01 | Access |
| win10 | NIC1 | 10.0.20.100 | 20 | 10.0.20.0/24 | 10.0.20.254 | 1500 | Uplink to karlo-cn-leaf-01 | Access |

## Management IP Allocations

| Hostname | IP Address | Vlan ID | Subnet | Gateway | MTU | Role |
| -------- | ---------- | ------- | ------ | ------- | --- | ---- |
| karlo-cn-fw-01 | 10.0.10.100 | 10 | 10.0.10.0/24 | 10.0.10.254 | 1500 | VRF Management |
| karlo-cn-fw-02 | 10.0.10.101 | 10 | 10.0.10.0/24 | 10.0.10.254 | 1500 | VRF Management |
| karlo-cn-rtr-01 | 10.0.10.102 | 10 | 10.0.10.0/24 | 10.0.10.254 | 1500 | VRF Management |
| karlo-cn-rtr-02 | 10.0.10.103 | 10 | 10.0.10.0/24 | 10.0.10.254 | 1500 | VRF Management |
| karlo-cn-spine-01 | 10.0.10.104 | 10 | 10.0.10.0/24 | 10.0.10.254 | 1500 | VRF Management |
| karlo-cn-spine-02 | 10.0.10.105 | 10 | 10.0.10.0/24 | 10.0.10.254 | 1500 | VRF Management |
| karlo-cn-leaf-01 | 10.0.10.106 | 10 | 10.0.10.0/24 | 10.0.10.254 | 1500 | VRF Management |
| karlo-cn-leaf-02 | 10.0.10.107 | 10 | 10.0.10.0/24 | 10.0.10.254 | 1500 | VRF Management |
| karlo-cn-ansible | 10.0.10.253 | 10 | 10.0.10.0/24 | 10.0.10.254 | 1500 | VRF Management |

## Service & Port Map

| Source | Destination | Protocol/Port | Service |
| ------ | ----------- | ------------- | ------- |
| ANY | DMZ | (50) TCP 80, 443 | Web Traffic (External) |
| ALL SUBNETS | SEC-APPS (30) | UDP 1514, 1515 | Wazuh Agent Logs |
| ALL SUBNETS | SEC-APPS (30) | TCP 9997 | Splunk Data Ingest |
| NET-MGMT (10) | ALL SUBNETS | TCP 22 | Ansible SSH Management |

## HQ Core Addressing

### Subnet Zones

| Zone | Purpose | Subnet |
| ---- | ------- | ------ |
| Infrastructure Loopback | Router-IDs, BGP Peers | 10.0.71.0/24 |
| Fabric Mesh | PTP links between network devices | 10.0.70.0/24 |
| Remote Site Zone | Remote Site allocation block | 10.20.0.0/16 |

### Loopback & Router IDs

| Device | Router ID / Loopback 0 | Description |
| ------ | ---------------------- | ----------- |
| karlo-hq-spine-01 | 10.0.71.10/32 | HQ Spine 01 |
| karlo-hq-spine-02 | 10.0.71.20/32 | HQ Spine 02 |
| karlo-hq-leaf-01 | 10.0.71.101/32 | HQ Access Leaf 01 |
| karlo-hq-leaf-02 | 10.0.71.102/32 | HQ Access Leaf 02 |
| karlo-hq-leaf-03 | 10.0.71.103/32 | HQ Access Leaf 03 |
| karlo-hq-leaf-04 | 10.0.71.104/32 | HQ Access Leaf 04 |
| karlo-rm-leaf-01 | 10.20.71.101/32 | Remote Access Leaf 01 |

### HQ Fabric Point-to-Point

| Device A | IP | Port | Device B | IP | Port |
| -------- | -- | ---- | -------- | -- | ---- |
| karlo-hq-spine-01 | 10.0.70.0/31 | eth5 | karlo-hq-leaf-01 | 10.0.70.1/31 | eth5 |
| karlo-hq-spine-01 | 10.0.70.2/31 | eth6 | karlo-hq-leaf-02 | 10.0.70.3/31 | eth6 |
| karlo-hq-spine-01 | 10.0.70.4/31 | eth7 | karlo-hq-leaf-03 | 10.0.70.5/31 | eth7 |
| karlo-hq-spine-01 | 10.0.70.6/31 | eth8 | karlo-hq-leaf-04 | 10.0.70.7/31 | eth8 |
| karlo-hq-spine-02 | 10.0.70.8/31 | eth9 | karlo-hq-leaf-01 | 10.0.70.9/31 | eth9 |
| karlo-hq-spine-02 | 10.0.70.10/31 | eth10 | karlo-hq-leaf-02 | 10.0.70.11/31 | eth10 |
| karlo-hq-spine-02 | 10.0.70.12/31 | eth11 | karlo-hq-leaf-03 | 10.0.70.13/31 | eth11 |
| karlo-hq-spine-02 | 10.0.70.14/31 | eth12 | karlo-hq-leaf-04 | 10.0.70.15/31 | eth12 |

### Collapsed Spine to Firewall

| Device | Interface | IP Address | Role |
| ------ | --------- | ---------- | ---- |
| karlo-hq-fw-vip | CARP VIP | 10.0.70.1/24 | Default Gateway for Spines |
| karlo-hq-spine-01 | Vlan70 | 10.0.70.2/24 | HQ Spine 01 |
| karlo-hq-spine-02 | Vlan70 | 10.0.70.3/24 | HQ Spine 02 |
| karlo-hq-fw-01 | vtnet2 | 10.0.70.4/24 | Active Firewall LAN Node |
| karlo-hq-fw-02 | vtnet2 | 10.0.70.5/24 | Passive Firewall LAN Node |
| karlo-hq-fw-01 | sync | 10.0.70.32/31 | pfSync State Synchronization |
| karlo-hq-fw-02 | sync | 10.0.70.33/31 | pfSync State Synchronization |

Layer 3 Client Gateways (SVIs)

| VLAN | Name | HQ Subnet | Remote Branch Subnet |
| ---- | ---- | --------- | -------------------- |
| 10 | INFRA-MGMT | 10.0.10.0/24 | 10.20.10.0/24 |
| 11 | SRV-MGMT | 10.0.11.0/24 | 10.20.11.0/24 |
| 20 | WIN-CLIENTS | 10.0.20.0/24 | 10.20.20.0/24 |
| 21 | LIN-CLIENTS | 10.0.21.0/24 | 10.20.21.0/24 |
| 30 | SEC-APPS | 10.0.30.0/24 | 10.20.30.0/24 |
| 40 | DMZ | 10.0.40.0/24 | 10.20.40.0/24 |
| 50 | PRD-SVRS | 10.0.50.0/24 | 10.20.50.0/24 |
| 60 | BACKUPS | 10.0.60.0/24 | 10.20.60.0/24 |
| 100 | BRANCH-USERS | - | 10.20.100.0/24 |
| 200 | BRANCH-GUEST | - | 10.20.200.0/24 |
| 666 | BLACK-HOLE | Native Trunk Void | Native Trunk Void |

Autonomous Systems (BGP / WAN Scope)

| Domain | ASN | Description |
| ------ | --- | ----------- |
| HQ | AS 65010 | Spines / HQ OPNsense Cluster |
| HQ Access Leafs | AS 65001-65004 | Internal Fabric |
| MPLS Provider | AS 65050 | Simulated Provider MPLS Network (VyOS) |
| Remote Branch | AS 65020 | Remote Branch Firewall |
