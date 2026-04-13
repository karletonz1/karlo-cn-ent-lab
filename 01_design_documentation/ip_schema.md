# IP Schema Addressing

This document defines the logical architecture, IP addressing scheme for the Karlo-CN Infrastructure. All devices within the n will adhere to these specifications.

## EVPN/VXLAN  

### Subnet Zones

| Zone | Purpose | Subnet |
| ---- | ------- | ------ |
| Infrastructure | Loopback, Router-ID, BGP Peer | 10.0.71.0/24 |
| Fabric Mesh | PTP links between network devices | 10.0.70.0/31 - 10.0.70.32/31 |

### Loopback  

| Device | Router ID | Loopback 0 IP | Description |  
| ------ | --------- | ----------- | ----------- |
| karlo-cn-prd-sp01 | 10.0.71.10 | 10.0.71.10 /32 | Spine 01 Router ID |
| karlo-cn-prd-sp02 | 10.0.71.20 | 10.0.71.20 /32 | Spine 02 Router ID |
| karlo-cn-prd-al01 | 10.0.71.101 | 10.0.71.101 /32 | Access Leaf 01 virtual tunnel end point IP |
| karlo-cn-prd-al02 | 10.0.71.102 | 10.0.71.102 /32 | Access Leaf 02 virtual tunnel end point IP |
| karlo-cn-prd-al03 | 10.0.71.103 | 10.0.71.103 /32 | Access Leaf 03 virtual tunnel end point IP |
| karlo-cn-prd-al04 | 10.0.71.104 | 10.0.71.104 /32 | Access Leaf 04 virtual tunnel end point IP |
| karlo-cn-prd-bl01 | 10.0.71.100 | 10.0.71.100 /32 | Border Leaf 01 Router ID |
| karlo-cn-prd-bl02 | 10.0.71.200 | 10.0.71.200 /32 | Border Leaf 02 Router ID |
| karlo-cn-prd-fw01 | 10.0.71.150 | 10.0.71.150 /32 | Firewall 01 Router ID |
| karlo-cn-prd-fw02 | 10.0.71.250 | 10.0.71.250 /32 | Firewall 02 Router ID |

### Fabric Point-to-Point IP Schema

| Device A | CIDR | Device A IP | Interface | Device B | Device B IP | Interface | Description |
| -------- | ---- | ----------- | --------- | ----------- | --------- | ----------- |
| karlo-cn-prd-sp01 | /31 | 10.0.70.16 | eth1 | karlo-cn-prd-bl01 | 10.0.70.17 | eth1 | PTP link between Spine 01 and Border Leaf 01 |
| karlo-cn-prd-sp01 | /31 | 10.0.70.18 | eth2 | karlo-cn-prd-bl02 | 10.0.70.19 | eth2 | PTP link between Spine 01 and Border Leaf 02 |
| karlo-cn-prd-sp01 | /31 | 10.0.70.0 | eth 5 | karlo-cn-prd-al01 | 10.0.70.1 | eth5 | PTP link between Spine 01 and Access Leaf 01 |
| karlo-cn-prd-sp01 | /31 | 10.0.70.2 | eth 6 | karlo-cn-prd-al02 | 10.0.70.3 | eth6 | PTP link between Spine 01 and Access Leaf 02 |
| karlo-cn-prd-sp01 | /31 | 10.0.70.4 | eth 7 | karlo-cn-prd-al03 | 10.0.70.5 | eth7 | PTP link between Spine 01 and Access Leaf 03 |
| karlo-cn-prd-sp01 | /31 | 10.0.70.6 | eth 8 | karlo-cn-prd-al04 | 10.0.70.7 | eth8 | PTP link between Spine 01 and Access Leaf 04 |
| karlo-cn-prd-sp02 | /31 | 10.0.70.22 | eth1 | karlo-cn-prd-bl02 | 10.0.70.23 | eth1 | PTP link between Spine 02 and Border Leaf 02 |
| karlo-cn-prd-sp02 | /31 | 10.0.70.20 | eth2 | karlo-cn-prd-bl01 | 10.0.70.21 | eth2 | PTP link between Spine 02 and Border Leaf 01 |
| karlo-cn-prd-sp02 | /31 | 10.0.70.8 | eth 9 | karlo-cn-prd-al01 | 10.0.70.9 | eth9 | PTP link between Spine 02 and Access Leaf 01 |
| karlo-cn-prd-sp02 | /31 | 10.0.70.10 | eth 10 | karlo-cn-prd-al02 | 10.0.70.11 | eth10 | PTP link between Spine 02 and Access Leaf 02 |
| karlo-cn-prd-sp02 | /31 | 10.0.70.12 | eth 11 | karlo-cn-prd-al03 | 10.0.70.13 | eth11 | PTP link between Spine 02 and Access Leaf 03 |
| karlo-cn-prd-sp02 | /31 | 10.0.70.14 | eth 12 | karlo-cn-prd-al04 | 10.0.70.15 | eth12 | PTP link between Spine 02 and Access Leaf 04 |
| karlo-cn-prd-fw01 | /24 | DHCP | vtnet0 | ISP-01 | DHCP | eth0 | PTP link between Firewall-01 and ISP 01 |
| karlo-cn-prd-fw01 | /24 | DHCP | vtnet1 | ISP-02 | DHCP | eth1 | PTP link between Firewall-01 and ISP 02 |
| karlo-cn-prd-fw01 | /31 | 10.0.70.24 | vtnet2 | karlo-cn-prd-bl01 | 10.0.70.25 | eth8 | PTP link between Firewall-01 and Border Leaf 01 |
| karlo-cn-prd-fw01 | /31 | 10.0.70.26 | vtnet3 | karlo-cn-prd-bl02 | 10.0.70.27 | eth9 | PTP link between Firewall-01 and Border Leaf 02 |
| karlo-cn-prd-fw02 | /24 | DHCP | vtnet0 | ISP-02 | DHCP | eth0 | PTP link between Firewall-02 and ISP 02 |
| karlo-cn-prd-fw02 | /24 | DHCP | vtnet1 | ISP-01 | DHCP | eth1 | PTP link between Firewall-02 and ISP 01 |
| karlo-cn-prd-fw02 | /31 | 10.0.70.28 | vtnet3 | karlo-cn-prd-bl01 | 10.0.70.29 | eth9 | PTP link between Firewall-02 and Border Leaf 01 |
| karlo-cn-prd-fw02 | /31 | 10.0.70.30 | vtnet2 | karlo-cn-prd-bl02 | 10.0.70.31 | eth8 | PTP link between Firewall-02 and Border Leaf 02 |
| karlo-cn-prd-fw01 | /31 | 10.0.70.32 | sync | karlo-cn-prd-fw02 | 10.0.70.33 | sync | PTP link between Firewall-01 and Firewall-02 |

## ASN

Spine 01: AS 65010
Spine 02: AS 65020
Access Leaf 01: AS 65001
Access Leaf 02: AS 65002
Access Leaf 03: AS 65003
Access Leaf 04: AS 65004
Border Leaf 01: AS 65101
Border Leaf 02: AS 65102

## Anycast Gateways

| VLAN | Name  | Device A IP | Interface | Device B | Device B IP | Interface | Description |

| VLAN | Name | Anycast Gateway IP | vMAC | VNI |
| ---- | ---- | ------------------ | ---- | --- |
| 10 | INFRA-MGMT | 10.0.10.254/24 | 00:1c:73:00:00:99 | 10010 |
| 11 | SRV-MGMT | 10.0.11.254/24 | 00:1c:73:00:00:99 | 10011 |
| 20 | WIN-CLIENTS | 10.0.20.254/24 | 00:1c:73:00:00:99 | 10020 |
| 21 | LIN-CLIENTS | 10.0.21.254/24 | 00:1c:73:00:00:99 | 10021 |
| 30 | SEC-APPS | 10.0.30.254/24 | 00:1c:73:00:00:99 | 10030 |
| 40 | DMZ | 10.0.40.254/24 | 00:1c:73:00:00:99 | 10040 |
| 50 | PRD-SVRS | 10.0.50.254/24 | 00:1c:73:00:00:99 | 10050 |
| 60 | BACKUPS | 10.0.60.254/24 | 00:1c:73:00:00:99 | 10060 |

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
| 666 | Black Hole Sun | - |

> [!NOTE]
> VLAN 666: Used as a "Black Hole" Native VLAN for all trunk ports**