# IP Schema Addressing

This document defines the logical architecture, IP addressing scheme for the Karlo-CN Infrastructure. All devices within the n will adhere to these specifications.

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
| 666 | Black Hole Sun | - |

> [!NOTE]
> VLAN 666: Used as a "Black Hole" Native VLAN for all trunk ports**