# EVPN/VXLAN

This document is the step-by-step guide on how to deploy EVPN/VXLAN in North Star using manual CLI input into all the devices. This document could serve as a training guide for future deployments or a means to verify that the playbooks are automating the correct EVPN/VXLAN configuration.

## Spine 01 (AS 65010)

```text
! enable modern engine needed for EVPN/VXLAN
service routing protocols model multi-agent
!
interface Loopback0
  description EVPN Loopback
  ip address 10.0.71.10/32
!
interface Ethernet1
   description PTP link to karlo-cn-prd-bl01
   no switchport
   mtu 9214
   ip address 10.0.70.16/31
!
interface Ethernet2
   description PTP link to karlo-cn-prd-bl02
   no switchport
   mtu 9214
   ip address 10.0.70.18/31
!
interface Ethernet5
   description PTP link to karlo-cn-prd-al01
   no switchport
   mtu 9214
   ip address 10.0.70.0/31
!
interface Ethernet6
   description PTP link to karlo-cn-prd-al02
   no switchport
   mtu 9214
   ip address 10.0.70.2/31
!
interface Ethernet7
   description PTP link to karlo-cn-prd-al03
   no switchport
   mtu 9214
   ip address 10.0.70.4/31
!
interface Ethernet8
   description PTP link to karlo-cn-prd-al04
   no switchport
   mtu 9214
   ip address 10.0.70.6/31
!
router bgp 65010
   router-id 10.0.71.10
   maximum-paths 4 ecmp 4
   neighbor UNDERLAY peer-group
   neighbor UNDERLAY send-community
   neighbor EVPN_OVERLAY peer-group
   neighbor EVPN_OVERLAY update-source Loopback0
   neighbor EVPN_OVERLAY ebgp-multihop 3
   neighbor EVPN_OVERLAY send-community
   neighbor EVPN_OVERLAY next-hop-unchanged
   !
   neighbor 10.0.70.17 remote-as 65101
   neighbor 10.0.70.17 peer-group UNDERLAY
   neighbor 10.0.70.19 remote-as 65102
   neighbor 10.0.70.19 peer-group UNDERLAY
   neighbor 10.0.70.1 remote-as 65001
   neighbor 10.0.70.1 peer-group UNDERLAY
   neighbor 10.0.70.3 remote-as 65002
   neighbor 10.0.70.3 peer-group UNDERLAY
   neighbor 10.0.70.5 remote-as 65003
   neighbor 10.0.70.5 peer-group UNDERLAY
   neighbor 10.0.70.7 remote-as 65004
   neighbor 10.0.70.7 peer-group UNDERLAY
   !
   neighbor 10.0.71.100 remote-as 65101
   neighbor 10.0.71.100 peer-group EVPN_OVERLAY
   neighbor 10.0.71.200 remote-as 65102
   neighbor 10.0.71.200 peer-group EVPN_OVERLAY
   neighbor 10.0.71.101 remote-as 65001
   neighbor 10.0.71.101 peer-group EVPN_OVERLAY
   neighbor 10.0.71.102 remote-as 65002
   neighbor 10.0.71.102 peer-group EVPN_OVERLAY
   neighbor 10.0.71.103 remote-as 65003
   neighbor 10.0.71.103 peer-group EVPN_OVERLAY
   neighbor 10.0.71.104 remote-as 65004
   neighbor 10.0.71.104 peer-group EVPN_OVERLAY
   !
   address-family ipv4
      neighbor UNDERLAY activate
      network 10.0.71.10/32
   !
   address-family evpn
      neighbor EVPN_OVERLAY activate
```

## Spine 02 (AS 65020)

```text
! enable modern engine needed for EVPN/VXLAN
service routing protocols model multi-agent
!
interface Loopback0
   description EVPN Loopback
   ip address 10.0.71.20/32
!
interface Ethernet1
   description PTP link to karlo-cn-prd-bl02
   no switchport
   mtu 9214
   ip address 10.0.70.22/31
!
interface Ethernet2
   description PTP link to karlo-cn-prd-bl01
   no switchport
   mtu 9214
   ip address 10.0.70.20/31
!
interface Ethernet9
   description PTP link to karlo-cn-prd-al01
   no switchport
   mtu 9214
   ip address 10.0.70.8/31
!
interface Ethernet10
   description PTP link to karlo-cn-prd-al02
   no switchport
   mtu 9214
   ip address 10.0.70.10/31
!
interface Ethernet11
   description PTP link to karlo-cn-prd-al03
   no switchport
   mtu 9214
   ip address 10.0.70.12/31
!
interface Ethernet12
   description PTP link to karlo-cn-prd-al04
   no switchport
   mtu 9214
   ip address 10.0.70.14/31
!
router bgp 65020
   router-id 10.0.71.20
   maximum-paths 4 ecmp 4
   neighbor UNDERLAY peer-group
   neighbor UNDERLAY send-community
   neighbor EVPN_OVERLAY peer-group
   neighbor EVPN_OVERLAY update-source Loopback0
   neighbor EVPN_OVERLAY ebgp-multihop 3
   neighbor EVPN_OVERLAY send-community
   neighbor EVPN_OVERLAY next-hop-unchanged
   !
   neighbor 10.0.70.23 remote-as 65102
   neighbor 10.0.70.23 peer-group UNDERLAY
   neighbor 10.0.70.21 remote-as 65101
   neighbor 10.0.70.21 peer-group UNDERLAY
   neighbor 10.0.70.9 remote-as 65001
   neighbor 10.0.70.9 peer-group UNDERLAY
   neighbor 10.0.70.11 remote-as 65002
   neighbor 10.0.70.11 peer-group UNDERLAY
   neighbor 10.0.70.13 remote-as 65003
   neighbor 10.0.70.13 peer-group UNDERLAY
   neighbor 10.0.70.15 remote-as 65004
   neighbor 10.0.70.15 peer-group UNDERLAY
   !
   neighbor 10.0.71.100 remote-as 65101
   neighbor 10.0.71.100 peer-group EVPN_OVERLAY
   neighbor 10.0.71.200 remote-as 65102
   neighbor 10.0.71.200 peer-group EVPN_OVERLAY
   neighbor 10.0.71.101 remote-as 65001
   neighbor 10.0.71.101 peer-group EVPN_OVERLAY
   neighbor 10.0.71.102 remote-as 65002
   neighbor 10.0.71.102 peer-group EVPN_OVERLAY
   neighbor 10.0.71.103 remote-as 65003
   neighbor 10.0.71.103 peer-group EVPN_OVERLAY
   neighbor 10.0.71.104 remote-as 65004
   neighbor 10.0.71.104 peer-group EVPN_OVERLAY
   !
   address-family ipv4
      neighbor UNDERLAY activate
      network 10.0.71.20/32
   !
   address-family evpn
      neighbor EVPN_OVERLAY activate
```

## Access Leaf 01 (AS 65001)

```text
! enable modern engine needed for EVPN/VXLAN
service routing protocols model multi-agent
!
interface Loopback0
   description EVPN Loopback
   ip address 10.0.71.101/32
!
interface Vxlan1
   vxlan source-interface Loopback0
   vxlan udp-port 4789
!
interface Ethernet5
   description PTP link to karlo-cn-prd-sp01
   no switchport
   mtu 9214
   ip address 10.0.70.1/31
!
interface Ethernet9
   description PTP link to karlo-cn-prd-sp02
   no switchport
   mtu 9214
   ip address 10.0.70.9/31
!
router bgp 65001
   router-id 10.0.71.101
   maximum-paths 4 ecmp 4
   neighbor UNDERLAY peer-group
   neighbor UNDERLAY send-community
   neighbor EVPN_OVERLAY peer-group
   neighbor EVPN_OVERLAY update-source Loopback0
   neighbor EVPN_OVERLAY ebgp-multihop 3
   neighbor EVPN_OVERLAY send-community
   !
   neighbor 10.0.70.0 remote-as 65010
   neighbor 10.0.70.0 peer-group UNDERLAY
   neighbor 10.0.70.8 remote-as 65020
   neighbor 10.0.70.8 peer-group UNDERLAY
   !
   neighbor 10.0.71.10 remote-as 65010
   neighbor 10.0.71.10 peer-group EVPN_OVERLAY
   neighbor 10.0.71.20 remote-as 65020
   neighbor 10.0.71.20 peer-group EVPN_OVERLAY
   !
   address-family ipv4
      neighbor UNDERLAY activate
      network 10.0.71.101/32
   !
   address-family evpn
      neighbor EVPN_OVERLAY activate
```

## Access Leaf 02 (AS 65002)

```text
! enable modern engine needed for EVPN/VXLAN
service routing protocols model multi-agent
!
interface Loopback0
   description EVPN Loopback
   ip address 10.0.71.102/32
!
interface Vxlan1
   vxlan source-interface Loopback0
   vxlan udp-port 4789
!
interface Ethernet6
   description PTP link to karlo-cn-prd-sp01
   no switchport
   mtu 9214
   ip address 10.0.70.3/31
!
interface Ethernet10
   description PTP link to karlo-cn-prd-sp02
   no switchport
   mtu 9214
   ip address 10.0.70.11/31
!
router bgp 65002
   router-id 10.0.71.102
   maximum-paths 4 ecmp 4
   neighbor UNDERLAY peer-group
   neighbor UNDERLAY send-community
   neighbor EVPN_OVERLAY peer-group
   neighbor EVPN_OVERLAY update-source Loopback0
   neighbor EVPN_OVERLAY ebgp-multihop 3
   neighbor EVPN_OVERLAY send-community
   !
   neighbor 10.0.70.2 remote-as 65010
   neighbor 10.0.70.2 peer-group UNDERLAY
   neighbor 10.0.70.10 remote-as 65020
   neighbor 10.0.70.10 peer-group UNDERLAY
   !
   neighbor 10.0.71.10 remote-as 65010
   neighbor 10.0.71.10 peer-group EVPN_OVERLAY
   neighbor 10.0.71.20 remote-as 65020
   neighbor 10.0.71.20 peer-group EVPN_OVERLAY
   !
   address-family ipv4
      neighbor UNDERLAY activate
      network 10.0.71.102/32
   !
   address-family evpn
      neighbor EVPN_OVERLAY activate

5. Access Leaf 03 (AS 65003)
Plaintext

! enable modern engine needed for EVPN/VXLAN
service routing protocols model multi-agent
!
interface Loopback0
   description EVPN Loopback
   ip address 10.0.71.103/32
!
interface Vxlan1
   vxlan source-interface Loopback0
   vxlan udp-port 4789
!
interface Ethernet7
   description PTP link to karlo-cn-prd-sp01
   no switchport
   mtu 9214
   ip address 10.0.70.5/31
!
interface Ethernet11
   description PTP link to karlo-cn-prd-sp02
   no switchport
   mtu 9214
   ip address 10.0.70.13/31
!
router bgp 65003
   router-id 10.0.71.103
   maximum-paths 4 ecmp 4
   neighbor UNDERLAY peer-group
   neighbor UNDERLAY send-community
   neighbor EVPN_OVERLAY peer-group
   neighbor EVPN_OVERLAY update-source Loopback0
   neighbor EVPN_OVERLAY ebgp-multihop 3
   neighbor EVPN_OVERLAY send-community
   !
   neighbor 10.0.70.4 remote-as 65010
   neighbor 10.0.70.4 peer-group UNDERLAY
   neighbor 10.0.70.12 remote-as 65020
   neighbor 10.0.70.12 peer-group UNDERLAY
   !
   neighbor 10.0.71.10 remote-as 65010
   neighbor 10.0.71.10 peer-group EVPN_OVERLAY
   neighbor 10.0.71.20 remote-as 65020
   neighbor 10.0.71.20 peer-group EVPN_OVERLAY
   !
   address-family ipv4
      neighbor UNDERLAY activate
      network 10.0.71.103/32
   !
   address-family evpn
      neighbor EVPN_OVERLAY activate
```

## Access Leaf 04 (AS 65004)

```text
! enable modern engine needed for EVPN/VXLAN
service routing protocols model multi-agent
!
interface Loopback0
   description EVPN Loopback
   ip address 10.0.71.104/32
!
interface Vxlan1
   vxlan source-interface Loopback0
   vxlan udp-port 4789
!
interface Ethernet8
   description PTP link to karlo-cn-prd-sp01
   no switchport
   mtu 9214
   ip address 10.0.70.7/31
!
interface Ethernet12
   description PTP link to karlo-cn-prd-sp02
   no switchport
   mtu 9214
   ip address 10.0.70.15/31
!
router bgp 65004
   router-id 10.0.71.104
   maximum-paths 4 ecmp 4
   neighbor UNDERLAY peer-group
   neighbor UNDERLAY send-community
   neighbor EVPN_OVERLAY peer-group
   neighbor EVPN_OVERLAY update-source Loopback0
   neighbor EVPN_OVERLAY ebgp-multihop 3
   neighbor EVPN_OVERLAY send-community
   !
   neighbor 10.0.70.6 remote-as 65010
   neighbor 10.0.70.6 peer-group UNDERLAY
   neighbor 10.0.70.14 remote-as 65020
   neighbor 10.0.70.14 peer-group UNDERLAY
   !
   neighbor 10.0.71.10 remote-as 65010
   neighbor 10.0.71.10 peer-group EVPN_OVERLAY
   neighbor 10.0.71.20 remote-as 65020
   neighbor 10.0.71.20 peer-group EVPN_OVERLAY
   !
   address-family ipv4
      neighbor UNDERLAY activate
      network 10.0.71.104/32
   !
   address-family evpn
      neighbor EVPN_OVERLAY activate
```

## Border Leaf 01 (VyOS - AS 65101)

```text
set interfaces dummy dum0 address '10.0.71.100/32'

set interfaces ethernet eth1 address '10.0.70.17/31'
set interfaces ethernet eth1 description 'PTP link to karlo-cn-prd-sp01'
set interfaces ethernet eth1 mtu '9214'

set interfaces ethernet eth2 address '10.0.70.21/31'
set interfaces ethernet eth2 description 'PTP link to karlo-cn-prd-sp02'
set interfaces ethernet eth2 mtu '9214'

set interfaces ethernet eth8 address '10.0.70.25/31'
set interfaces ethernet eth8 description 'PTP link to karlo-cn-prd-fw01'
set interfaces ethernet eth8 mtu '9214'

set interfaces ethernet eth9 address '10.0.70.29/31'
set interfaces ethernet eth9 description 'PTP link to karlo-cn-prd-fw02'
set interfaces ethernet eth9 mtu '9214'

set protocols bgp system-as 65101
set protocols bgp parameters router-id '10.0.71.100'

set protocols bgp peer-group UNDERLAY remote-as external
set protocols bgp neighbor 10.0.70.16 peer-group UNDERLAY
set protocols bgp neighbor 10.0.70.20 peer-group UNDERLAY

set protocols bgp peer-group EVPN_OVERLAY ebgp-multihop 3
set protocols bgp peer-group EVPN_OVERLAY update-source 10.0.71.100
set protocols bgp neighbor 10.0.71.10 remote-as 65010
set protocols bgp neighbor 10.0.71.10 peer-group EVPN_OVERLAY
set protocols bgp neighbor 10.0.71.20 remote-as 65020
set protocols bgp neighbor 10.0.71.20 peer-group EVPN_OVERLAY

set protocols bgp address-family ipv4-unicast network 10.0.71.100/32
set protocols bgp address-family l2vpn-evpn neighbor EVPN_OVERLAY activate
```

## Border Leaf 02 (VyOS - AS 65102)

```text
set interfaces dummy dum0 address '10.0.71.200/32'

set interfaces ethernet eth1 address '10.0.70.23/31'
set interfaces ethernet eth1 description 'PTP link to karlo-cn-prd-sp02'
set interfaces ethernet eth1 mtu '9214'

set interfaces ethernet eth2 address '10.0.70.19/31'
set interfaces ethernet eth2 description 'PTP link to karlo-cn-prd-sp01'
set interfaces ethernet eth2 mtu '9214'

set interfaces ethernet eth8 address '10.0.70.31/31'
set interfaces ethernet eth8 description 'PTP link to karlo-cn-prd-fw02'
set interfaces ethernet eth8 mtu '9214'

set interfaces ethernet eth9 address '10.0.70.27/31'
set interfaces ethernet eth9 description 'PTP link to karlo-cn-prd-fw01'
set interfaces ethernet eth9 mtu '9214'

set protocols bgp system-as 65102
set protocols bgp parameters router-id '10.0.71.200'

set protocols bgp peer-group UNDERLAY remote-as external
set protocols bgp neighbor 10.0.70.18 peer-group UNDERLAY
set protocols bgp neighbor 10.0.70.22 peer-group UNDERLAY

set protocols bgp peer-group EVPN_OVERLAY ebgp-multihop 3
set protocols bgp peer-group EVPN_OVERLAY update-source 10.0.71.200
set protocols bgp neighbor 10.0.71.10 remote-as 65010
set protocols bgp neighbor 10.0.71.10 peer-group EVPN_OVERLAY
set protocols bgp neighbor 10.0.71.20 remote-as 65020
set protocols bgp neighbor 10.0.71.20 peer-group EVPN_OVERLAY

set protocols bgp address-family ipv4-unicast network 10.0.71.200/32
set protocols bgp address-family l2vpn-evpn neighbor EVPN_OVERLAY activate
```

## Verification Tests

>[!NOTE] Use the following OSPF neighbor commands to verify devices can see their neighbors

| Device | Command | Success Criteria |
| ------ | ------- | ----- |
| All switches | show interfaces : include MTU | Success Criteria: Your physical transit interfaces (e.g., Ethernet5, eth1) must show an MTU of 9214. |
| | Ping { Destination Loopback IP } source Loopback0 size 9000 | 100% Success. This proves that large packets can traverse the physical underlay without fragmentation |
| VyOS border leaf | show interfaces ethernet { eth } MTU | Success Criteria: Your physical transit interfaces (e.g., Ethernet5, eth1) must show an MTU of 9214 |
| | ping { Destination Loopback IP } interface dum0 size 9000 | 100% Success |

>[!NOTE] Use the following route commands once neighbors in 'full' status to indicate they can now exchange their databases.  

| Device | Command | Success Criteria |
| ------ | ------- | ----- |
| All switches | show ip route ospf | Success Criteria: You should see a list of /32 routes for every single switch and router in the fabric. We specifically want to see everyone's Loopback0 IP |
| VyOS border leafs | show ip route ospf | Success Criteria: You should see a list of /32 routes for every single switch and router in the fabric. We specifically want to see everyone's Loopback0 IP |

>[!NOTE] Ensure the each device is able to ping using the Router's loopback address.  

| Device | Command | Success Criteria |
| ------ | ------- | ----- |
| All switches | show bgp evpn summary | The neighbor IPs should be the Loopback of the other switches. The State/PfxRcd must be a number |
| Border Leaf | show bgp l2vpn evpn summary | State a number not active or connect |

>[!NOTE] We need to verify that Jumbo frame configuration has been applied successfully. This is critical for EVPN/VXLAN.

| Device | Command | Success Criteria |
| ------ | ------- | ----- |
| All Access Leafs | show interfaces Vxlan 1 | Status must be Vxlan1 is up, line protocol is up. The source interface must be listed as Loopback0 with the correct IP |