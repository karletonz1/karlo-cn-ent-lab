# Physical Interface Mapping

## Spine 01 Interface Mapping

| Device A | Device A Hostname | Device A port | Device B | Device B Hostname | Device B Port |
| --------------- | ----------------- | ------------- | --------------- | ----------------- | ------------- |
| vEOS Spine Switch 01 | karlo-vulcan-sp01 | mgmt | Ethernet Switch | OOBM | eth2 |
| vEOS Spine Switch 01 | karlo-vulcan-sp01 | eth1 | vEOS Spine Switch 02 | karlo-vulcan-sp02 | eth1 |
| vEOS Spine Switch 01 | karlo-vulcan-sp01 | eth2 | OPNsense | karlo-vulcan-fw01 | vnet2 |
| vEOS Spine Switch 01 | karlo-vulcan-sp01 | eth3 | OPNsense | karlo-vulcan-fw02 | vnet3 |
| vEOS Spine Switch 01 | karlo-vulcan-sp01 | eth4 | vEOS Spine Switch 02 | karlo-vulcan-sp02 | eth4 |
| vEOS Spine Switch 01 | karlo-vulcan-sp01 | eth5 | vEOS Leaf Switch 01 | karlo-vulcan-al01 | eth5 |
| vEOS Spine Switch 01 | karlo-vulcan-sp01 | eth6 | vEOS Leaf Switch 02 | karlo-vulcan-al02 | eth6 |
| vEOS Spine Switch 01 | karlo-vulcan-sp01 | eth7 | vEOS Leaf Switch 03 | karlo-vulcan-al03 | eth7 |
| vEOS Spine Switch 01 | karlo-vulcan-sp01 | eth8 | vEOS Leaf Switch 04 | karlo-vulcan-al04 | eth8 |

## Spine 02 Interface Mapping

| Device A | Device A Hostname | Device A port | Device B | Device B Hostname | Device B Port |
| --------------- | ----------------- | ------------- | --------------- | ----------------- | ------------- |
| vEOS Spine Switch 02 | karlo-vulcan-sp02 | mgmt | Ethernet Switch | OOBM | eth3 |
| vEOS Spine Switch 02 | karlo-vulcan-sp02 | eth1 | vEOS Spine Switch 01 | karlo-vulcan-sp01 | eth1 |
| vEOS Spine Switch 02 | karlo-vulcan-sp02 | eth2 | OPNsense | karlo-vulcan-fw02 | vnet2 |
| vEOS Spine Switch 02 | karlo-vulcan-sp02 | eth3 | OPNsense | karlo-vulcan-fw01 | vnet3 |
| vEOS Spine Switch 02 | karlo-vulcan-sp02 | eth4 | vEOS Spine Switch 01 | karlo-vulcan-sp01 | eth4 |
| vEOS Spine Switch 02 | karlo-vulcan-sp02 | eth9 | vEOS Leaf Switch 01 | karlo-vulcan-al01 | eth9 |
| vEOS Spine Switch 02 | karlo-vulcan-sp02 | eth10 | vEOS Leaf Switch 02 | karlo-vulcan-al02 | eth10 |
| vEOS Spine Switch 02 | karlo-vulcan-sp02 | eth11 | vEOS Leaf Switch 03 | karlo-vulcan-al03 | eth11 |
| vEOS Spine Switch 02 | karlo-vulcan-sp02 | eth12 | vEOS Leaf Switch 04 | karlo-vulcan-al04 | eth12 |

## Access Leaf 01 Interface Mapping

| Device A | Device A Hostname | Device A port | Device B | Device B Hostname | Device B Port |
| --------------- | ----------------- | ------------- | --------------- | ----------------- | ------------- |
| vEOS Leaf Switch 01 | karlo-vulcan-al01 | mgmt | Ethernet Switch | OOBM | eth1 |
| vEOS Leaf Switch 01 | karlo-vulcan-al01 | eth1 | Windows 10 Endpoint | karlo-vulcan-win | e0 |
| vEOS Leaf Switch 01 | karlo-vulcan-al01 | eth2 | Windows Server 2022 | karlo-vulcan-security | NIC1 |
| vEOS Leaf Switch 01 | karlo-vulcan-al01 | eth5 | vEOS Spine Switch 01 | karlo-vulcan-sp01 | eth5 |
| vEOS Leaf Switch 01 | karlo-vulcan-al01 | eth9 | vEOS Spine Switch 02 | karlo-vulcan-sp02 | eth9 |
| vEOS Leaf Switch 01 | karlo-vulcan-al01 | eth12 | Linux Ansible Host | karlo-vulcan-ansible | eth0 |

## Access Leaf 02 Interface Mapping

| Device A | Device A Hostname | Device A port | Device B | Device B Hostname | Device B Port |
| --------------- | ----------------- | ------------- | --------------- | ----------------- | ------------- |
| vEOS Leaf Switch 02 | karlo-vulcan-al02 | mgmt | Ethernet Switch | OOBM | eth4 |
| vEOS Leaf Switch 02 | karlo-vulcan-al02 | eth1 | Windows Server 2022 | karlo-vulcan-DC1 | NIC1 |
| vEOS Leaf Switch 02 | karlo-vulcan-al02 | eth6 | vEOS Spine Switch 01 | karlo-vulcan-sp01 | eth6 |
| vEOS Leaf Switch 02 | karlo-vulcan-al02 | eth10 | vEOS Spine Switch 02 | karlo-vulcan-sp02 | eth10 |

## Access Leaf 03 Interface Mapping

| Device A | Device A Hostname | Device A port | Device B | Device B Hostname | Device B Port |
| --------------- | ----------------- | ------------- | --------------- | ----------------- | ------------- |
| vEOS Leaf Switch 03 | karlo-vulcan-al03 | mgmt | Ethernet Switch | OOBM | eth5 |
| vEOS Leaf Switch 03 | karlo-vulcan-al03 | eth1 | Windows Server 2022 | karlo-vulcan-DC2 | NIC1 |
| vEOS Leaf Switch 03 | karlo-vulcan-al03 | eth7 | vEOS Spine Switch 01 | karlo-vulcan-sp01 | eth7 |
| vEOS Leaf Switch 03 | karlo-vulcan-al03 | eth11 | vEOS Spine Switch 02 | karlo-vulcan-sp02 | eth11 |

## Leaf 4 Interface Mapping

| Device A | Device A Hostname | Device A port | Device B | Device B Hostname | Device B Port |
| --------------- | ----------------- | ------------- | --------------- | ----------------- | ------------- |
| vEOS Leaf Switch 04 | karlo-vulcan-al04 | mgmt | Ethernet Switch | OOBM | eth6 |
| vEOS Leaf Switch 04 | karlo-vulcan-al04 | eth1 | Windows Server 2022 | karlo-vulcan-DC2 | NIC1 |
| vEOS Leaf Switch 04 | karlo-vulcan-al04 | eth8 | vEOS Spine Switch 01 | karlo-vulcan-sp01 | eth8 |
| vEOS Leaf Switch 04 | karlo-vulcan-al04 | eth12 | vEOS Spine Switch 02 | karlo-vulcan-sp02 | eth12 |
