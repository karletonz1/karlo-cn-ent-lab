# EVPN/VXLAN

## karlo-cn-prd-al01

```text
vrf instance PROD-VRF
   rd 10.0.71.101:5000
!
vlan 10,11,20,21,30,40,50,60,666
!
interface Vlan10
   vrf PROD-VRF
   ip address virtual 10.0.10.254/24
interface Vlan11
   vrf PROD-VRF
   ip address virtual 10.0.11.254/24
interface Vlan20
   vrf PROD-VRF
   ip address virtual 10.0.20.254/24
interface Vlan21
   vrf PROD-VRF
   ip address virtual 10.0.21.254/24
interface Vlan30
   vrf PROD-VRF
   ip address virtual 10.0.30.254/24
interface Vlan40
   vrf PROD-VRF
   ip address virtual 10.0.40.254/24
interface Vlan50
   vrf PROD-VRF
   ip address virtual 10.0.50.254/24
interface Vlan60
   vrf PROD-VRF
   ip address virtual 10.0.60.254/24
!
interface Vxlan1
   vxlan source-interface Loopback0
   vxlan udp-port 4789
   vxlan vrf PROD-VRF vni 5000
   vxlan vlan 10 vni 10000
   vxlan vlan 11 vni 11000
   vxlan vlan 20 vni 20000
   vxlan vlan 21 vni 21000
   vxlan vlan 30 vni 30000
   vxlan vlan 40 vni 40000
   vxlan vlan 50 vni 50000
   vxlan vlan 60 vni 60000
   vxlan vlan 666 vni 666000
!
router bgp 65001
   router-id 10.0.71.101
   vlan 10
      rd 10.0.71.101:10000
      route-target both 10000:10000
      redistribute learned
   vlan 11
      rd 10.0.71.101:11000
      route-target both 11000:11000
      redistribute learned
   vlan 20
      rd 10.0.71.101:20000
      route-target both 20000:20000
      redistribute learned
   vlan 21
      rd 10.0.71.101:21000
      route-target both 21000:21000
      redistribute learned
   vlan 30
      rd 10.0.71.101:30000
      route-target both 30000:30000
      redistribute learned
   vlan 40
      rd 10.0.71.101:40000
      route-target both 40000:40000
      redistribute learned
   vlan 50
      rd 10.0.71.101:50000
      route-target both 50000:50000
      redistribute learned
   vlan 60
      rd 10.0.71.101:60000
      route-target both 60000:60000
      redistribute learned
   vlan 666
      rd 10.0.71.101:666000
      route-target both 666000:666000
      redistribute learned
   vrf PROD-VRF
      rd 10.0.71.101:5000
      route-target import evpn 5000:5000
      route-target export evpn 5000:5000
      redistribute connected
```

## karlo-cn-prd-al02

```text
vrf instance PROD-VRF
   rd 10.0.71.102:5000
!
vlan 10,11,20,21,30,40,50,60,666
!
interface Vlan10
   vrf PROD-VRF
   ip address virtual 10.0.10.254/24
interface Vlan11
   vrf PROD-VRF
   ip address virtual 10.0.11.254/24
interface Vlan20
   vrf PROD-VRF
   ip address virtual 10.0.20.254/24
interface Vlan21
   vrf PROD-VRF
   ip address virtual 10.0.21.254/24
interface Vlan30
   vrf PROD-VRF
   ip address virtual 10.0.30.254/24
interface Vlan40
   vrf PROD-VRF
   ip address virtual 10.0.40.254/24
interface Vlan50
   vrf PROD-VRF
   ip address virtual 10.0.50.254/24
interface Vlan60
   vrf PROD-VRF
   ip address virtual 10.0.60.254/24
!
interface Vxlan1
   vxlan source-interface Loopback0
   vxlan udp-port 4789
   vxlan vrf PROD-VRF vni 5000
   vxlan vlan 10 vni 10000
   vxlan vlan 11 vni 11000
   vxlan vlan 20 vni 20000
   vxlan vlan 21 vni 21000
   vxlan vlan 30 vni 30000
   vxlan vlan 40 vni 40000
   vxlan vlan 50 vni 50000
   vxlan vlan 60 vni 60000
   vxlan vlan 666 vni 666000
!
router bgp 65002
   router-id 10.0.71.102
   vlan 10
      rd 10.0.71.102:10000
      route-target both 10000:10000
      redistribute learned
   vlan 11
      rd 10.0.71.102:11000
      route-target both 11000:11000
      redistribute learned
   vlan 20
      rd 10.0.71.102:20000
      route-target both 20000:20000
      redistribute learned
   vlan 21
      rd 10.0.71.102:21000
      route-target both 21000:21000
      redistribute learned
   vlan 30
      rd 10.0.71.102:30000
      route-target both 30000:30000
      redistribute learned
   vlan 40
      rd 10.0.71.102:40000
      route-target both 40000:40000
      redistribute learned
   vlan 50
      rd 10.0.71.102:50000
      route-target both 50000:50000
      redistribute learned
   vlan 60
      rd 10.0.71.102:60000
      route-target both 60000:60000
      redistribute learned
   vlan 666
      rd 10.0.71.102:666000
      route-target both 666000:666000
      redistribute learned
   vrf PROD-VRF
      rd 10.0.71.102:5000
      route-target import evpn 5000:5000
      route-target export evpn 5000:5000
      redistribute connected
```

## karlo-cn-prd-al03

```text
vrf instance PROD-VRF
   rd 10.0.71.103:5000
!
vlan 10,11,20,21,30,40,50,60,666
!
interface Vlan10
   vrf PROD-VRF
   ip address virtual 10.0.10.254/24
interface Vlan11
   vrf PROD-VRF
   ip address virtual 10.0.11.254/24
interface Vlan20
   vrf PROD-VRF
   ip address virtual 10.0.20.254/24
interface Vlan21
   vrf PROD-VRF
   ip address virtual 10.0.21.254/24
interface Vlan30
   vrf PROD-VRF
   ip address virtual 10.0.30.254/24
interface Vlan40
   vrf PROD-VRF
   ip address virtual 10.0.40.254/24
interface Vlan50
   vrf PROD-VRF
   ip address virtual 10.0.50.254/24
interface Vlan60
   vrf PROD-VRF
   ip address virtual 10.0.60.254/24
!
interface Vxlan1
   vxlan source-interface Loopback0
   vxlan udp-port 4789
   vxlan vrf PROD-VRF vni 5000
   vxlan vlan 10 vni 10000
   vxlan vlan 11 vni 11000
   vxlan vlan 20 vni 20000
   vxlan vlan 21 vni 21000
   vxlan vlan 30 vni 30000
   vxlan vlan 40 vni 40000
   vxlan vlan 50 vni 50000
   vxlan vlan 60 vni 60000
   vxlan vlan 666 vni 666000
!
router bgp 65003
   router-id 10.0.71.103
   vlan 10
      rd 10.0.71.103:10000
      route-target both 10000:10000
      redistribute learned
   vlan 11
      rd 10.0.71.103:11000
      route-target both 11000:11000
      redistribute learned
   vlan 20
      rd 10.0.71.103:20000
      route-target both 20000:20000
      redistribute learned
   vlan 21
      rd 10.0.71.103:21000
      route-target both 21000:21000
      redistribute learned
   vlan 30
      rd 10.0.71.103:30000
      route-target both 30000:30000
      redistribute learned
   vlan 40
      rd 10.0.71.103:40000
      route-target both 40000:40000
      redistribute learned
   vlan 50
      rd 10.0.71.103:50000
      route-target both 50000:50000
      redistribute learned
   vlan 60
      rd 10.0.71.103:60000
      route-target both 60000:60000
      redistribute learned
   vlan 666
      rd 10.0.71.103:666000
      route-target both 666000:666000
      redistribute learned
   vrf PROD-VRF
      rd 10.0.71.103:5000
      route-target import evpn 5000:5000
      route-target export evpn 5000:5000
      redistribute connected
```

## karlo-cn-prd-al04

```text
vrf instance PROD-VRF
   rd 10.0.71.104:5000
!
vlan 10,11,20,21,30,40,50,60,666
!
interface Vlan10
   vrf PROD-VRF
   ip address virtual 10.0.10.254/24
interface Vlan11
   vrf PROD-VRF
   ip address virtual 10.0.11.254/24
interface Vlan20
   vrf PROD-VRF
   ip address virtual 10.0.20.254/24
interface Vlan21
   vrf PROD-VRF
   ip address virtual 10.0.21.254/24
interface Vlan30
   vrf PROD-VRF
   ip address virtual 10.0.30.254/24
interface Vlan40
   vrf PROD-VRF
   ip address virtual 10.0.40.254/24
interface Vlan50
   vrf PROD-VRF
   ip address virtual 10.0.50.254/24
interface Vlan60
   vrf PROD-VRF
   ip address virtual 10.0.60.254/24
!
interface Vxlan1
   vxlan source-interface Loopback0
   vxlan udp-port 4789
   vxlan vrf PROD-VRF vni 5000
   vxlan vlan 10 vni 10000
   vxlan vlan 11 vni 11000
   vxlan vlan 20 vni 20000
   vxlan vlan 21 vni 21000
   vxlan vlan 30 vni 30000
   vxlan vlan 40 vni 40000
   vxlan vlan 50 vni 50000
   vxlan vlan 60 vni 60000
   vxlan vlan 666 vni 666000
!
router bgp 65004
   router-id 10.0.71.104
   vlan 10
      rd 10.0.71.104:10000
      route-target both 10000:10000
      redistribute learned
   vlan 11
      rd 10.0.71.104:11000
      route-target both 11000:11000
      redistribute learned
   vlan 20
      rd 10.0.71.104:20000
      route-target both 20000:20000
      redistribute learned
   vlan 21
      rd 10.0.71.104:21000
      route-target both 21000:21000
      redistribute learned
   vlan 30
      rd 10.0.71.104:30000
      route-target both 30000:30000
      redistribute learned
   vlan 40
      rd 10.0.71.104:40000
      route-target both 40000:40000
      redistribute learned
   vlan 50
      rd 10.0.71.104:50000
      route-target both 50000:50000
      redistribute learned
   vlan 60
      rd 10.0.71.104:60000
      route-target both 60000:60000
      redistribute learned
   vlan 666
      rd 10.0.71.104:666000
      route-target both 666000:666000
      redistribute learned
   vrf PROD-VRF
      rd 10.0.71.104:5000
      route-target import evpn 5000:5000
      route-target export evpn 5000:5000
      redistribute connected
```
