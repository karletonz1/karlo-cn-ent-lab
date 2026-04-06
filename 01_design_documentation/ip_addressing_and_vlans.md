# IP Addressing and VLANs

This document defines the logical architecture, IP addressing scheme for the Karlo-CN Infrastructure. All devices within the lab will adhere to these specifications.

## 1. IP Address Management

### Subnets

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
> **VLAN 666:** Used as a "Black Hole" Native VLAN for all trunk ports**

### VARP Gateways

| Vlan ID | Vlan Name | Network | VARP IP (GW) | MTU | Primary Nodes |  
| ------- | --------- | ------- | ------------ | --- | ------------- |
| 10 | INFRA-MGMT | 10.0.10.0/24 | 10.0.10.254 | 1500 | VyOS, OPNsense, OVS, Ansible |
| 11 | SRV-MGMT | 10.0.11.0/24 | 10.0.11.254 | 1500 | Proxmox Host, Veeam, Domain Controllers |
| 20 | WIN-CLIENTS | 10.0.20.0/24 | 10.0.20.254 | 1500 | Windows 10 Workstations |
| 21 | LIN-CLIENTS | 10.0.21.0/24 | 10.0.21.254 | 1500 | Ubuntu Workstations |
| 30 | SEC-APPS | 10.0.30.0/24 | 10.0.30.254 | 1500 | Wazuh Manager, Splunk, Tenable, Kali Linux |
| 40 | DMZ | 10.0.40.0/24 | 10.0.40.254 | 1500 | Apache & IIS Web Servers |
| 50 | PRD-SVRS | 10.0.50.0/24 | 10.0.50.254 | 1500 | Simulated App Servers (Proxmox) |
| 60 | BACKUPS | 10.0.60.0/24 | 10.0.60.254 | 1500 | Veeam Backups |
| 666 | BLACK HOLE SUN | NATIVE | N/A | 1500 | Unused Ports (Security Black hole) |

### Firewall IP Address Allocation

| Hostname | GNS3 Port | IP Address | Vlan ID | Subnet | Gateway | MTU | Role | Link type |
| -------- | --------- | ---------- | ------- | ------ | ------- | --- | ---- | --------- |
| karlo-cn-fw-01 | eth0 | 10.0.10.10 | 10 | 10.0.10.0/24 | 10.0.10.254 | 1500 | Management Port | Access |
| karlo-cn-fw-02 | eth0 | 10.0.10.20 | 10 | 10.0.10.0/24 | 10.0.10.254 | 1500 | Management Port | Access |

### Router 01 PTP Address Allocation

| Hostname | GNS3 Port | Network Address | IP Address | Broadcast Address | MTU | Role | Link type |
| -------- | --------- | --------------- | ---------- | ----------------- | --- | ---- | --------- |
| karlo-cn-rtr-01 | eth1 | 10.0.71.0/30 | 10.0.71.1/30 | 10.0.71.3/30 | 1500 | PTP link between Router-01 and Spine-01 | PTP |
| karlo-cn-rtr-01 | eth2 | 10.0.71.4/30 | 10.0.71.5/30 | 10.0.71.7/30 | 1500 | PTP link between Router-01 and Spine-02 | PTP |
| karlo-cn-rtr-01 | eth8/9:bond0 | 10.0.71.16/30 | 10.0.71.17/30 | 10.0.71.19/30 | 1500 | LAG Link between RTR-01/02 | PTP |

### Spine 01 PTP Address Allocation

| Hostname | GNS3 Port | Network Address | IP Address | Broadcast Address | MTU | Role | Link type |
| -------- | --------- | --------------- | ---------- | ----------------- | ---- | ---- | -------- |
| karlo-cn-spine-01 | eth1 | 10.0.71.0/30 | 10.0.71.2/30 | 10.0.71.3/30 | 1500 | PTP link between Spine-01 and Router-01 | PTP |
| karlo-cn-spine-01 | eth2 | 10.0.71.12/30 | 10.0.71.14/30 | 10.0.71.15/30 | 1500 | PTP link between Spine-01 and Router-02 | PTP |

### Router 02 PTP Address Allocation

| Hostname | GNS3 Port | Network Address | IP Address | Broadcast Address | MTU | Role | Link type |
| -------- | --------- | --------------- | ---------- | ----------------- | --- | ---- | --------- |
| karlo-cn-rtr-02 | eth1 | 10.0.71.8/30 | 10.0.71.9/30 | 10.0.71.11/30 | 1500 | PTP link between Router-01 and Spine-02 | PTP |
| karlo-cn-rtr-02 | eth2 | 10.0.71.12/30 | 10.0.71.13/30 | 10.0.71.15/30 | 1500 | PTP link between Router-01 and Spine-01 | PTP |
| karlo-cn-rtr-02 | eth8/9:bond0 | 10.0.71.16/30 | 10.0.71.18/30 | 10.0.71.19/30 | 1500 | LAG Link between RTR-01/02 | PTP |

### Spine 02 PTP Address Allocation

| Hostname | GNS3 Port | Network Address | IP Address | Broadcast Address | MTU | Role | Link type |
| -------- | --------- | --------------- | ---------- | ----------------- | --- | ---- | --------- |
| karlo-cn-spine-02 | eth1 | 10.0.71.8/30 | 10.0.71.10/30 | 10.0.71.11/30 | 1500 | PTP link between Spine-02 and Router-02 | PTP |
| karlo-cn-spine-02 | eth2 | 10.0.71.4/30 | 10.0.71.6/30 | 10.0.71.7/30 | 1500 | PTP link between Spine-02 and Router-01 | PTP |

### Spine 01 SVI Address Allocation

| Hostname | SVI | Network Address | IP Address | VIP (GW) | Virtual MAC | MTU | Description |
| -------- | --- | --------------- | ---------- | -------- | ----------- | --- | ----------- |
| karlo-cn-spine-01 | 10 | 10.0.10.0/24 | 10.0.10.1 | 10.0.10.254 | 00:11:11:00:00:01 | 1500 | SVI IP address for Infra-MGT Subnet VARP Gateway |
| karlo-cn-spine-01 | 11 | 10.0.11.0/24 | 10.0.11.1 | 10.0.11.254 | 00:11:11:00:00:01 | 1500 | SVI IP address for SRV-MGT Subnet VARP Gateway |
| karlo-cn-spine-01 | 20 | 10.0.20.0/24 | 10.0.20.1 | 10.0.20.254 | 00:11:11:00:00:01 | 1500 | SVI IP address for WIN-CLIENTS Subnet VARP Gateway |
| karlo-cn-spine-01 | 21 | 10.0.21.0/24 | 10.0.21.1 | 10.0.21.254 | 00:11:11:00:00:01 | 1500 | SVI IP address for LIN-CLIENTS Subnet VARP Gateway |
| karlo-cn-spine-01 | 30 | 10.0.30.0/24 | 10.0.30.1 | 10.0.30.254 | 00:11:11:00:00:01 | 1500 | SVI IP address for SEC-APPS Subnet VARP Gateway |
| karlo-cn-spine-01 | 40 | 10.0.40.0/24 | 10.0.40.1 | 10.0.40.254 | 00:11:11:00:00:01 | 1500 | SVI IP address for DMZ Subnet VARP Gateway |
| karlo-cn-spine-01 | 50 | 10.0.50.0/24 | 10.0.50.1 | 10.0.50.254 | 00:11:11:00:00:01 | 1500 | SVI IP address for PRD-SRVS Subnet VARP Gateway |
| karlo-cn-spine-01 | 60 | 10.0.60.0/24 | 10.0.60.1 | 10.0.60.254 | 00:11:11:00:00:01 | 1500 | SVI IP address for BACKUPS Subnet VARP Gateway |
| karlo-cn-spine-01 | 70 | 10.0.70.0/30 | 10.0.70.1 | - | - | 1500 | MLAG Peer Link to Secondary |

### Spine 02 SVI Address Allocation

| Hostname | SVI | Network Address | IP Address | VIP (GW) | Virtual MAC | MTU | Description |
| -------- | --- | --------------- | ---------- | -------- | ----------- | --- | ----------- |
| karlo-cn-spine-02 | 10 | 10.0.10.0/24 | 10.0.10.2 | 10.0.10.254 | 00:11:11:00:00:01 | 1500 | SVI IP address for Infra-MGT Subnet VARP Gateway |
| karlo-cn-spine-02 | 11 | 10.0.11.0/24 | 10.0.11.2 | 10.0.11.254 | 00:11:11:00:00:01 | 1500 | SVI IP address for SRV-MGT Subnet VARP Gateway |
| karlo-cn-spine-02 | 20 | 10.0.20.0/24 | 10.0.20.2 | 10.0.20.254 | 00:11:11:00:00:01 | 1500 | SVI IP address for WIN-CLIENTS Subnet VARP Gateway |
| karlo-cn-spine-02 | 21 | 10.0.21.0/24 | 10.0.21.2 | 10.0.21.254 | 00:11:11:00:00:01 | 1500 | SVI IP address for LIN-CLIENTS Subnet VARP Gateway |
| karlo-cn-spine-02 | 30 | 10.0.30.0/24 | 10.0.30.2 | 10.0.30.254 | 00:11:11:00:00:01 | 1500 | SVI IP address for SEC-APPS Subnet VARP Gateway |
| karlo-cn-spine-02 | 40 | 10.0.40.0/24 | 10.0.40.2 | 10.0.40.254 | 00:11:11:00:00:01 | 1500 | SVI IP address for DMZ Subnet VARP Gateway |
| karlo-cn-spine-02 | 50 | 10.0.50.0/24 | 10.0.50.2 | 10.0.50.254 | 00:11:11:00:00:01 | 1500 | SVI IP address for PRD-SRVS Subnet VARP Gateway |
| karlo-cn-spine-02 | 60 | 10.0.60.0/24 | 10.0.60.2 | 10.0.60.254 | 00:11:11:00:00:01 | 1500 | SVI IP address for BACKUPS Subnet VARP Gateway |
| karlo-cn-spine-02 | 70 | 10.0.70.0/30 | 10.0.70.2 | - | - | 1500 | MLAG Peer Link to Primary |

### Spine 01 Port-Channel Allocation

| Hostname | GNS3 Port | Logical Interface | Allowed Vlan | Role | Link type |
| -------- | --------- | ----------------- | ------------ | ---- | --------- |
| karlo-cn-spine-01 | eth3 | Port-channel 70 | 10,11,20,21,30,40,50,60,666,70 | MLAG peer link to secondary | Trunk |
| karlo-cn-spine-01 | eth4 | Port-channel 70 | 10,11,20,21,30,40,50,60,666,70 | MLAG peer link to secondary | Trunk |
| karlo-cn-spine-01 | eth5 | Port-channel 10 | 10,11,20,21,30,40,50,60,666 | Downlink to karlo-cn-leaf-01 | Trunk |
| karlo-cn-spine-01 | eth6 | Port-channel 20 | 10,11,20,21,30,40,50,60,666 | Downlink to karlo-cn-leaf-02 | Trunk |

### Spine 02 Port-Channel Allocation

| Hostname | GNS3 Port | Logical Interface | Allowed Vlan | Role | Link type |
| -------- | --------- | ----------------- | ------------ | ---- | --------- |
| karlo-cn-spine-02 | eth3 | Port-channel 70 | 10,11,20,21,30,40,50,60,666,70 | MLAG peer link to primary | Trunk |
| karlo-cn-spine-02 | eth4 | Port-channel 70 | 10,11,20,21,30,40,50,60,666,70 | MLAG peer link to primary | Trunk |
| karlo-cn-spine-02 | eth5 | Port-channel 20 | 10,11,20,21,30,40,50,60,666 | Downlink to karlo-cn-leaf-02 | Trunk |
| karlo-cn-spine-02 | eth6 | Port-channel 10 | 10,11,20,21,30,40,50,60,666 | Downlink to karlo-cn-leaf-01 | Trunk |

### Server IP Address Allocation

| Hostname | GNS3 Port | IP Address | Vlan ID | Subnet | Gateway | MTU | Role | Link type |
| -------- | --------- | ---------- | ------- | ------ | ------- | --- | ---- | -------- |
| karlo-cn-esx-01 | eth0/vmk0 | 10.0.50.1 | 50 | 10.0.50.0/24 | 10.0.50.254 | 1500 | Uplink to karlo-cn-leaf-01 | Access |
| karlo-cn-esx-02 | eth0/vmk0 | 10.0.50.2 | 50 | 10.0.50.0/24 | 10.0.50.254 | 1500 | Uplink to karlo-cn-leaf-02 | Access |

### Client IP Address Allocation

| Hostname | GNS3 Port | IP Address | Vlan ID | Subnet | Gateway | MTU | Role | Link type |
| -------- | --------- | ---------- | ------- | ------ | ------- | --- | ---- | -------- |
| karlo-cn-ansible | eth0 | 10.0.10.253 | 10 | 10.0.10.0/24 | 10.0.10.254 | 1500 | Uplink to karlo-cn-leaf-01 | Access |

### Management Address Allocation

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

### Service & Port Map

| Source | Destination | Protocol/Port | Service |
| ------ | ----------- | ------------- | ------- |
| ANY | DMZ | (50) TCP 80, 443 | Web Traffic (External) |
| ALL SUBNETS | SEC-APPS (30) | UDP 1514, 1515 | Wazuh Agent Logs |
| ALL SUBNETS | SEC-APPS (30) | TCP 9997 | Splunk Data Ingest |
| NET-MGMT (10) | ALL SUBNETS | TCP 22 | Ansible SSH Management |
