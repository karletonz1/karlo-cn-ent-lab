# EVPN/VXLAN

This document is the step-by-step guide on how to deploy EVPN/VXLAN in North Star using manual CLI input into all the devices. This document could serve as a training guide for future deployments or a means to verify that the playbooks are automating the correct EVPN/VXLAN configuration.

## Phase 1: Configure the Physical Interfaces (Underlay)

The goal of the underlay is to establish loopback-to-loopback reachability.

1. Configure on all switches

### karlo-cn-prd-sp01

```text
enable
configure terminal

! Configure the Loopback Interfaces
interface Loopback0
   description EVPN Loopback
   ip address 10.0.71.10/32
   ip ospf area 0.0.0.0
exit

! Configure Transit Interfaces
interface Ethernet1
   description PTP link to border leaf 01
   no switchport
   mtu 9214
   ip address 10.0.70.16/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
exit

! Configure Transit Interfaces
interface Ethernet2
   description PTP link to border leaf 02
   no switchport
   mtu 9214
   ip address 10.0.70.18/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
exit

! Configure Transit Interfaces
interface Ethernet5
   description PTP link to access leaf 01
   no switchport
   mtu 9214
   ip address 10.0.70.0/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
exit

! Configure Transit Interfaces
interface Ethernet6
   description PTP link to access leaf 02
   no switchport
   mtu 9214
   ip address 10.0.70.2/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
exit

! Configure Transit Interfaces
interface Ethernet7
   description PTP link to access leaf 03
   no switchport
   mtu 9214
   ip address 10.0.70.4/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
exit

! Configure Transit Interfaces
interface Ethernet8
   description PTP link to access leaf 04
   no switchport
   mtu 9214
   ip address 10.0.70.6/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
exit

! Configure the Global Routing Process
router ospf 1
   router-id 10.0.71.10
   passive-interface default
   no passive-interface Ethernet1
   no passive-interface Ethernet2
   no passive-interface Ethernet5
   no passive-interface Ethernet6
   no passive-interface Ethernet7
   no passive-interface Ethernet8
   ! (Repeat 'no passive-interface' for all active PTP links)
   max-lsa 12000

exit
write memory
```

### karlo-cn-prd-sp02

```text
enable
configure terminal

! Configure the Loopback Interfaces
interface Loopback0
   description EVPN Loopback
   ip address 10.0.71.20/32
   ip ospf area 0.0.0.0
exit

! Configure Transit Interfaces
interface Ethernet1
   description PTP link to border leaf 02
   no switchport
   mtu 9214
   ip address 10.0.70.22/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
exit

! Configure Transit Interfaces
interface Ethernet2
   description PTP link to border leaf 01
   no switchport
   mtu 9214
   ip address 10.0.70.20/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
exit

! Configure Transit Interfaces
interface Ethernet9
   description PTP link to access leaf 01
   no switchport
   mtu 9214
   ip address 10.0.70.8/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
exit

! Configure Transit Interfaces
interface Ethernet10
   description PTP link to access leaf 02
   no switchport
   mtu 9214
   ip address 10.0.70.10/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
exit

! Configure Transit Interfaces
interface Ethernet11
   description PTP link to access leaf 03
   no switchport
   mtu 9214
   ip address 10.0.70.12/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
exit

! Configure Transit Interfaces
interface Ethernet12
   description PTP link to access leaf 04
   no switchport
   mtu 9214
   ip address 10.0.70.14/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
exit

! Configure the Global Routing Process
router ospf 1
   router-id 10.0.71.20
   passive-interface default
   no passive-interface Ethernet1
   no passive-interface Ethernet2
   no passive-interface Ethernet5
   no passive-interface Ethernet6
   no passive-interface Ethernet7
   no passive-interface Ethernet8
   ! (Repeat 'no passive-interface' for all active PTP links)
   max-lsa 12000
   
exit
write memory
```

### karlo-cn-prd-al01

```text
enable
configure terminal

! Configure the Loopback Interfaces
interface Loopback0
   description EVPN Loopback
   ip address 10.0.71.101/32
   ip ospf area 0.0.0.0
exit

! Configure Transit Interfaces
interface Ethernet5
   description PTP link to spine 01
   no switchport
   mtu 9214
   ip address 10.0.70.1/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
exit

! Configure Transit Interfaces
interface Ethernet9
   description PTP link to spine 02
   no switchport
   mtu 9214
   ip address 10.0.70.9/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
exit

! Configure the Global Routing Process
router ospf 1
   router-id 10.0.71.101
   passive-interface default
   no passive-interface Ethernet5
   no passive-interface Ethernet9
   ! (Repeat 'no passive-interface' for all active PTP links)
   max-lsa 12000
   
exit
write memory
```

### karlo-cn-prd-al02

```text
enable
configure terminal

! Configure the Loopback Interfaces
interface Loopback0
   description EVPN Loopback
   ip address 10.0.71.102/32
   ip ospf area 0.0.0.0
exit

! Configure Transit Interfaces
interface Ethernet6
   description PTP link to spine 01
   no switchport
   mtu 9214
   ip address 10.0.70.3/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
exit

! Configure Transit Interfaces
interface Ethernet10
   description PTP link to spine 02
   no switchport
   mtu 9214
   ip address 10.0.70.11/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
exit

! Configure the Global Routing Process
router ospf 1
   router-id 10.0.71.102
   passive-interface default
   no passive-interface Ethernet6
   no passive-interface Ethernet10
   ! (Repeat 'no passive-interface' for all active PTP links)
   max-lsa 12000
   
exit
write memory
```

### karlo-cn-prd-al03

```text
enable
configure terminal

! Configure the Loopback Interfaces
interface Loopback0
   description EVPN Loopback
   ip address 10.0.71.103/32
   ip ospf area 0.0.0.0
exit

! Configure Transit Interfaces
interface Ethernet7
   description PTP link to spine 01
   no switchport
   mtu 9214
   ip address 10.0.70.5/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
exit

! Configure Transit Interfaces
interface Ethernet11
   description PTP link to spine 02
   no switchport
   mtu 9214
   ip address 10.0.70.13/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
exit

! Configure the Global Routing Process
router ospf 1
   router-id 10.0.71.103
   passive-interface default
   no passive-interface Ethernet7
   no passive-interface Ethernet11
   ! (Repeat 'no passive-interface' for all active PTP links)
   max-lsa 12000
   
exit
write memory
```

### karlo-cn-prd-al04

```text
enable
configure terminal

! Configure the Loopback Interfaces
interface Loopback0
   description EVPN Loopback
   ip address 10.0.71.104/32
   ip ospf area 0.0.0.0
exit

! Configure Transit Interfaces
interface Ethernet8
   description PTP link to spine 01
   no switchport
   mtu 9214
   ip address 10.0.70.7/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
exit

! Configure Transit Interfaces
interface Ethernet12
   description PTP link to spine 02
   no switchport
   mtu 9214
   ip address 10.0.70.15/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
exit

! Configure the Global Routing Process
router ospf 1
   router-id 10.0.71.104
   passive-interface default
   no passive-interface Ethernet8
   no passive-interface Ethernet12
   ! (Repeat 'no passive-interface' for all active PTP links)
   max-lsa 12000
   
exit
write memory
```

2. Configure on all VyOS Border Leafs  

### karlo-cn-prd-bl01

```text
# Configure Router-ID
set interfaces dummy dum0 address '10.0.71.100/32'

# 2. Configure PTP Interfaces
set interfaces ethernet eth1 address '10.0.70.17/31'
set interfaces ethernet eth1 description 'PTP link to Spine 01'
set interfaces ethernet eth1 mtu '9214'
set interfaces ethernet eth2 address '10.0.70.21/31'
set interfaces ethernet eth2 description 'PTP link to Spine 02'
set interfaces ethernet eth2 mtu '9214'
set interfaces ethernet eth8 address '10.0.70.25/31'
set interfaces ethernet eth8 description 'PTP link to firewall 01'
set interfaces ethernet eth8 mtu '9214'
set interfaces ethernet eth9 address '10.0.70.29/31'
set interfaces ethernet eth9 description 'PTP link to firewall 02'
set interfaces ethernet eth9 mtu '9214'

# 3. Configure the Global OSPF Routing
set protocols ospf parameters router-id '10.0.71.100'
set protocols ospf interface dum0 area '0'

# 4. Attach Interfaces to OSPF
set protocols ospf interface eth1 area '0'
set protocols ospf interface eth1 network 'point-to-point'
set protocols ospf interface eth2 area '0'
set protocols ospf interface eth2 network 'point-to-point'
set protocols ospf interface eth8 area '0'
set protocols ospf interface eth8 network 'point-to-point'
set protocols ospf interface eth9 area '0'
set protocols ospf interface eth9 network 'point-to-point'

commit
save
exit
```

### karlo-cn-prd-bl02

```text
# Configure Router-ID
set interfaces dummy dum0 address '10.0.71.200/32'

# 2. Configure PTP Interfaces 
set interfaces ethernet eth1 address '10.0.70.23/31'
set interfaces ethernet eth1 description 'PTP link to Spine 02'
set interfaces ethernet eth1 mtu '9214'
set interfaces ethernet eth2 address '10.0.70.19/31'
set interfaces ethernet eth2 description 'PTP link to Spine 01'
set interfaces ethernet eth2 mtu '9214'
set interfaces ethernet eth8 address '10.0.70.31/31'
set interfaces ethernet eth8 description 'PTP link to firewall 02'
set interfaces ethernet eth8 mtu '9214'
set interfaces ethernet eth9 address '10.0.70.27/31'
set interfaces ethernet eth9 description 'PTP link to firewall 01'
set interfaces ethernet eth9 mtu '9214'

# 3. Configure the Global OSPF Routing
set protocols ospf parameters router-id '10.0.71.200'
set protocols ospf interface dum0 area '0'

# 4. Attach Interfaces to OSPF 
set protocols ospf interface eth1 area '0'
set protocols ospf interface eth1 network 'point-to-point'
set protocols ospf interface eth2 area '0'
set protocols ospf interface eth2 network 'point-to-point'
set protocols ospf interface eth8 area '0'
set protocols ospf interface eth8 network 'point-to-point'
set protocols ospf interface eth9 area '0'
set protocols ospf interface eth9 network 'point-to-point'

commit
save
exit
```

## Verification Tests

>[!NOTE] Use the following OSPF neighbor commands to verify devices can see their neighbors

| Device | Command | Success Criteria |
| ------ | ------- | ----- |
| All switches | show ip ospf neighbor | Success Criteria: You should see the Router IDs of the directly connected neighbors. The state must say FULL. If it says INIT, 2-WAY, or EXSTART, your MTU might be mismatched or packets are dropping. |
| VyOS border leafs | show ip ospf neighbor | Success Criteria: You should see the Router IDs of the directly connected neighbors. The state must say FULL. If it says INIT, 2-WAY, or EXSTART, your MTU might be mismatched or packets are dropping. |

>[!NOTE] Use the following route commands once neighbors in 'full' status to indicate they can now exchange their databases.  

| Device | Command | Success Criteria |
| ------ | ------- | ----- |
| All switches | show ip route ospf | Success Criteria: You should see a list of /32 routes for every single switch and router in the fabric. We specifically want to see everyone's Loopback0 IP |
| VyOS border leafs | show ip route ospf | Success Criteria: You should see a list of /32 routes for every single switch and router in the fabric. We specifically want to see everyone's Loopback0 IP |

>[!NOTE] Ensure the each device is able to ping using the Router's loopback address.  

| Device | Command | Success Criteria |
| ------ | ------- | ----- |
| All switches | ping { IP address } source { Loopback address of testing switch } | Success Criteria: 100% success rate |
| VyOS border leaf | ping { IP address } interface dum0 | Success Criteria: 100% success rate |

>[!NOTE] We need to verify that Jumbo frame configuration has been applied successfully. This is critical for EVPN/VXLAN.

| Device | Command | Success Criteria |
| ------ | ------- | ----- |
| All switches | show interfaces : include MTU | Success Criteria: Your physical transit interfaces (e.g., Ethernet5, eth1) must show an MTU of 9214. |
| VyOS border leaf | show interfaces ethernet { eth } MTU | Success Criteria: Your physical transit interfaces (e.g., Ethernet5, eth1) must show an MTU of 9214 |
