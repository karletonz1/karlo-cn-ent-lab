# Physical Interface Mapping

## Spine 01 Interface Mapping

| Device A Device | Device A Hostname | Device A port | Device B Device | Device B Hostname | Device B Port |
| --------------- | ----------------- | ------------- | --------------- | ----------------- | ------------- |
| vEOS Spine Switch 01 | karlo-cn-spine-01 | mgmt | Ethernet Switch | OOBM | eth2 |
| vEOS Spine Switch 01 | karlo-cn-spine-01 | eth1 | VyOS Core router 01 | karlo-cn-rtr-01 | eth1 |
| vEOS Spine Switch 01 | karlo-cn-spine-01 | eth2 | VyOS Core router 02 | karlo-cn-rtr-02 | eth2 |
| vEOS Spine Switch 01 | karlo-cn-spine-01 | eth3 | vEOS Spine Switch 02 | karlo-cn-spine-02 | eth3 |
| vEOS Spine Switch 01 | karlo-cn-spine-01 | eth4 | vEOS Spine Switch 02 | karlo-cn-spine-02 | eth4 |
| vEOS Spine Switch 01 | karlo-cn-spine-01 | eth5 | vEOS Leaf Switch 01 | karlo-cn-leaf-01 | eth5 |
| vEOS Spine Switch 01 | karlo-cn-spine-01 | eth6 | vEOS Leaf Switch 02 | karlo-cn-leaf-02 | eth6 |

## Spine 02 Interface Mapping

| Device A Device | Device A Hostname | Device A port | Device B Device | Device B Hostname | Device B Port |
| --------------- | ----------------- | ------------- | --------------- | ----------------- | ------------- |
| vEOS Spine Switch 02 | karlo-cn-spine-02 | mgmt | Ethernet Switch | OOBM | eth3 |
| vEOS Spine Switch 02 | karlo-cn-spine-02 | eth1 | VyOS Core router 02 | karlo-cn-rtr-02 | eth1 |
| vEOS Spine Switch 02 | karlo-cn-spine-02 | eth2 | VyOS Core router 01 | karlo-cn-rtr-01 | eth2 |
| vEOS Spine Switch 02 | karlo-cn-spine-02 | eth3 | vEOS Spine Switch 01 | karlo-cn-spine-01 | eth3 |
| vEOS Spine Switch 02 | karlo-cn-spine-02 | eth4 | vEOS Spine Switch 01 | karlo-cn-spine-01 | eth4 |
| vEOS Spine Switch 02 | karlo-cn-spine-02 | eth5 | vEOS Leaf Switch 02 | karlo-cn-leaf-02 | eth5 |
| vEOS Spine Switch 02 | karlo-cn-spine-02 | eth6 | vEOS Leaf Switch 01 | karlo-cn-leaf-01 | eth6 |

## Leaf 01 Interface Mapping

| Device A Device | Device A Hostname | Device A port | Device B Device | Device B Hostname | Device B Port |
| --------------- | ----------------- | ------------- | --------------- | ----------------- | ------------- |
| vEOS Leaf Switch 01 | karlo-cn-leaf-01 | mgmt | Ethernet Switch | OOBM | eth1 |
| vEOS Leaf Switch 01 | karlo-cn-leaf-01 | eth5 | vEOS Spine Switch 01 | karlo-cn-spine-01 | eth5 |
| vEOS Leaf Switch 01 | karlo-cn-leaf-01 | eth6 | vEOS Spine Switch 02 | karlo-cn-spine-02 | eth6 |
| vEOS Leaf Switch 01 | karlo-cn-leaf-01 | eth12 | Linux Ansible Host | karlo-cn-ansible | eth0 |

## Leaf 02 Interface Mapping

| Device A Device | Device A Hostname | Device A port | Device B Device | Device B Hostname | Device B Port |
| --------------- | ----------------- | ------------- | --------------- | ----------------- | ------------- |
| vEOS Leaf Switch 02 | karlo-cn-leaf-02 | mgmt | Ethernet Switch | OOBM | eth4 |
| vEOS Leaf Switch 02 | karlo-cn-leaf-02 | eth5 | vEOS Spine Switch 02 | karlo-cn-spine-02 | eth5 |
| vEOS Leaf Switch 02 | karlo-cn-leaf-02 | eth6 | vEOS Spine Switch 01 | karlo-cn-spine-01 | eth6 |

## Router 01 Interface Mapping

| Device A Device | Device A Hostname | Device A port | Device B Device | Device B Hostname | Device B Port |
| --------------- | ----------------- | ------------- | --------------- | ----------------- | ------------- |
| VyOS Core router 01 | karlo-cn-rtr-01 | eth0 | Ethernet Switch | OOBM Mgmt | eth5 |
| VyOS Core router 01 | karlo-cn-rtr-01 | eth1 | Arista Switch 01 | karlo-cn-spine-01 | eth1 |
| VyOS Core router 01 | karlo-cn-rtr-01 | eth2 | Arista Switch 02 | karlo-cn-spine-02 | eth2 |
| VyOS Core router 01 | karlo-cn-rtr-01 | eth8 | VyOS Core router 02 | karlo-cn-rtr-02 | eth8 |
| VyOS Core router 01 | karlo-cn-rtr-01 | eth9 | VyOS Core router 02 | karlo-cn-rtr-02 | eth9 |

## Router 02 Interface Mapping

| Device A Device | Device A Hostname | Device A port | Device B Device | Device B Hostname | Device B Port |
| --------------- | ----------------- | ------------- | --------------- | ----------------- | ------------- |
| VyOS Core router 02 | karlo-cn-rtr-02 | eth0 | Ethernet Switch | OOBM Mgmt | eth6 |
| VyOS Core router 02 | karlo-cn-rtr-02 | eth1 | Arista Switch 02 | karlo-cn-spine-02 | eth1 |
| VyOS Core router 02 | karlo-cn-rtr-02 | eth2 | Arista Switch 01 | karlo-cn-spine-01 | eth2 |
| VyOS Core router 02 | karlo-cn-rtr-02 | eth8 | VyOS Core router 01 | karlo-cn-rtr-01 | eth8 |
| VyOS Core router 02 | karlo-cn-rtr-02 | eth9 | VyOS Core router 01 | karlo-cn-rtr-01 | eth9 |