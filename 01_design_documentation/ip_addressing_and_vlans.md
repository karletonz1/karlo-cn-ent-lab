# IP Addressing and VLANs

This document defines the logical architecture, IP addressing scheme for the Karlo-CN Infrastructure. All devices within the n will adhere to these specifications.

## Subnet Allocation  

### VLANs

| Vlan ID | Vlan Name | Subnet |  
| ------- | --------- | ------ |
| 10 | Network Management | 10.0.10.0/24 |
| 11 | Server Management | 10.0.11.0/24 |
| 20 | Windows Clients | 10.0.20.0/24 |
| 21 | Linux Clients | 10.0.21.0/24 |
| 30 | Security Applications | 10.0.30.0/24 |
| 40 | DMZ | 10.0.40.0/24 |
| 50 | Production Servers | 10.0.50.0/24 |
| 60 | Backups | 10.0.60.0/24 |
| 70 | Spine MLAG Peer | 10.0.70.0/30 |
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

### Firewall 01 IP Address

| Hostname | GNS3 Port | Network Address | IP Address | Broadcast Address | MTU | Role | Link type |
| -------- | --------- | --------------- | ---------- | ----------------- | --- | ---- | --------- |
| karlo-cn-fw-01 | vtnet0 | 10.0.72.24/29 | 10.0.72.25/29 | 10.0.72.31/29 | 1500 | Firewall-01 WAN Link | PTP |
| karlo-cn-fw-01 | vtnet1 | 10.0.72.16/30 | 10.0.72.17/30 | 10.0.72.19/30 | 1500 | PTP Sync link between Firewall-01 and Firewall-02 | PTP |
| karlo-cn-fw-01 | vtnet2 | 10.0.72.0/30 | 10.0.72.1/30 | 10.0.72.3/30 | 1500 | PTP link between Firewall-01 and Router-01 | PTP |
| karlo-cn-fw-01 | vtnet3 | 10.0.72.4/30 | 10.0.72.5/30 | 10.0.72.7/30 | 1500 | PTP link between Firewall-01 and Router-02 | PTP |

### Firewall 02 IP Address

| Hostname | GNS3 Port | Network Address | IP Address | Broadcast Address | MTU | Role | Link type |
| -------- | --------- | --------------- | ---------- | ----------------- | --- | ---- | --------- |
| karlo-cn-fw-02 | vtnet0 | 10.0.72.24/29 | 10.0.72.26/29 | 10.0.72.31/29 | 1500 | Firewall-02 WAN Link | PTP |
| karlo-cn-fw-02 | vtnet1 | 10.0.72.16/30 | 10.0.72.18/30 | 10.0.72.19/30 | 1500 | PTP Sync link between Firewall-02 and Firewall-01 | PTP |
| karlo-cn-fw-02 | vtnet2 | 10.0.72.8/30 | 10.0.72.9/30 | 10.0.72.11/30 | 1500 | PTP link between Firewall-02 and Router-01 | PTP |
| karlo-cn-fw-02 | vtnet3 | 10.0.72.12/30 | 10.0.72.13/30 | 10.0.72.15/30 | 1500 | PTP link between Firewall-02 and Router-02 | PTP |

### Router 01 PTP Address

| Hostname | GNS3 Port | Network Address | IP Address | Broadcast Address | MTU | Role | Link type |
| -------- | --------- | --------------- | ---------- | ----------------- | --- | ---- | --------- |
| karlo-cn-rtr-01 | eth1 | 10.0.71.0/30 | 10.0.71.1/30 | 10.0.71.3/30 | 1500 | PTP link between Router-01 and Spine-01 | PTP |
| karlo-cn-rtr-01 | eth2 | 10.0.71.4/30 | 10.0.71.5/30 | 10.0.71.7/30 | 1500 | PTP link between Router-01 and Spine-02 | PTP |
| karlo-cn-rtr-01 | eth4 | 10.0.72.0/30 | 10.0.72.2/30 | 10.0.72.3/30 | 1500 | PTP link between Router-01 and firewall-01 | PTP |
| karlo-cn-rtr-01 | eth5 | 10.0.72.8/30 | 10.0.72.10/30 | 10.0.72.11/30 | 1500 | PTP link between Router-01 and firewall-02 | PTP |
| karlo-cn-rtr-01 | eth8/9:bond0 | 10.0.71.16/30 | 10.0.71.17/30 | 10.0.71.19/30 | 1500 | LAG Link between RTR-01/02 | PTP |

### Router 02 PTP Address

| Hostname | GNS3 Port | Network Address | IP Address | Broadcast Address | MTU | Role | Link type |
| -------- | --------- | --------------- | ---------- | ----------------- | --- | ---- | --------- |
| karlo-cn-rtr-02 | eth1 | 10.0.71.8/30 | 10.0.71.9/30 | 10.0.71.11/30 | 1500 | PTP link between Router-02 and Spine-02 | PTP |
| karlo-cn-rtr-02 | eth2 | 10.0.71.12/30 | 10.0.71.13/30 | 10.0.71.15/30 | 1500 | PTP link between Router-02 and Spine-01 | PTP |
| karlo-cn-rtr-02 | eth4 | 10.0.72.12/30 | 10.0.72.14/30 | 10.0.72.15/30 | 1500 | PTP link between Router-02 and Firewall-02 | PTP |
| karlo-cn-rtr-02 | eth5 | 10.0.72.4/30 | 10.0.72.6/30 | 10.0.72.7/30 | 1500 | PTP link between Router-02 and Firewall-01 | PTP |
| karlo-cn-rtr-02 | eth8/9:bond0 | 10.0.71.16/30 | 10.0.71.18/30 | 10.0.71.19/30 | 1500 | LAG Link between RTR-01/02 | PTP |

### Spine 01 PTP Address

| Hostname | GNS3 Port | Network Address | IP Address | Broadcast Address | MTU | Role | Link type |
| -------- | --------- | --------------- | ---------- | ----------------- | ---- | ---- | -------- |
| karlo-cn-spine-01 | eth1 | 10.0.71.0/30 | 10.0.71.2/30 | 10.0.71.3/30 | 1500 | PTP link between Spine-01 and Router-01 | PTP |
| karlo-cn-spine-01 | eth2 | 10.0.71.12/30 | 10.0.71.14/30 | 10.0.71.15/30 | 1500 | PTP link between Spine-01 and Router-02 | PTP |

### Spine 02 PTP Address

| Hostname | GNS3 Port | Network Address | IP Address | Broadcast Address | MTU | Role | Link type |
| -------- | --------- | --------------- | ---------- | ----------------- | --- | ---- | --------- |
| karlo-cn-spine-02 | eth1 | 10.0.71.8/30 | 10.0.71.10/30 | 10.0.71.11/30 | 1500 | PTP link between Spine-02 and Router-02 | PTP |
| karlo-cn-spine-02 | eth2 | 10.0.71.4/30 | 10.0.71.6/30 | 10.0.71.7/30 | 1500 | PTP link between Spine-02 and Router-01 | PTP |

## Port-Channel Allocations

### Spine 01 Port-Channel

| Hostname | GNS3 Port | Logical Interface | Allowed Vlan | Role | Link type |
| -------- | --------- | ----------------- | ------------ | ---- | --------- |
| karlo-cn-spine-01 | eth3 | Port-channel 70 | 10,11,20,21,30,40,50,60,666,70 | MLAG peer link to secondary | Trunk |
| karlo-cn-spine-01 | eth4 | Port-channel 70 | 10,11,20,21,30,40,50,60,666,70 | MLAG peer link to secondary | Trunk |
| karlo-cn-spine-01 | eth5 | Port-channel 10 | 10,11,20,21,30,40,50,60,666 | Downlink to karlo-cn-leaf-01 | Trunk |
| karlo-cn-spine-01 | eth6 | Port-channel 20 | 10,11,20,21,30,40,50,60,666 | Downlink to karlo-cn-leaf-02 | Trunk |

### Spine 02 Port-Channel

| Hostname | GNS3 Port | Logical Interface | Allowed Vlan | Role | Link type |
| -------- | --------- | ----------------- | ------------ | ---- | --------- |
| karlo-cn-spine-02 | eth3 | Port-channel 70 | 10,11,20,21,30,40,50,60,666,70 | MLAG peer link to primary | Trunk |
| karlo-cn-spine-02 | eth4 | Port-channel 70 | 10,11,20,21,30,40,50,60,666,70 | MLAG peer link to primary | Trunk |
| karlo-cn-spine-02 | eth5 | Port-channel 20 | 10,11,20,21,30,40,50,60,666 | Downlink to karlo-cn-leaf-02 | Trunk |
| karlo-cn-spine-02 | eth6 | Port-channel 10 | 10,11,20,21,30,40,50,60,666 | Downlink to karlo-cn-leaf-01 | Trunk |

### Leaf 01 Port-Channel

| Hostname | GNS3 Port | Logical Interface | Allowed Vlan | Role | Link type |
| -------- | --------- | ----------------- | ------------ | ---- | --------- |
| karlo-cn-leaf-01 | eth5 | Port-channel 10 | 10,11,20,21,30,40,50,60,666 | Port-Channel uplink to karlo-spine-01 | Trunk |
| karlo-cn-leaf-01 | eth6 | Port-channel 10 | 10,11,20,21,30,40,50,60,666 | Port-Channel uplink to karlo-spine-02 | Trunk |
| karlo-cn-leaf-01 | eth12 | e0 | 20 | Access uplink to karlo-leaf-01 | Access |

### Leaf 02 Port-Channel

| Hostname | GNS3 Port | Logical Interface | Allowed Vlan | Role | Link type |
| -------- | --------- | ----------------- | ------------ | ---- | --------- |
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

## Managment IP Allocations

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
