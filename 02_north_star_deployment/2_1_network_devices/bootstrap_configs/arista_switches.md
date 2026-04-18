# Arista vEOS Bootstrap

## Purpose

This document serves as the initial configuration checklist for bootstrapping the Arista switches. It is the minimum configuration required via CLI to allow Ansible to reach the devices before pushing the remaining full configuration.

## Prerequisites  

:white_check_mark: GNS3 topology is cabled according to the physical design.

:white_check_mark: All switches are powered on.

:white_check_mark: GNS3 console access is working.  

> [!TIP]  
> For all switches, run `#zerotouch cancel` first to stop Arista ZTP and enter manual configuration mode. This will trigger an immediate switch reload.
> [!NOTE]
> Default credentials for a new vEOS switch is admin, no password

## karlo-cn-prd-al01 Bootstrap Config  

```text
enable
config

! Management User
username leafadmin privilege 15 secret "{{ vault_leaf_admin_pass }}"
enable password "{{ vault_leaf_enable_pass }}"

! Enable eAPI
management api http-commands
   protocol https
   no protocol HTTP
   no shutdown
   vrf management
   no shutdown
   exit

! Create the management VRF
vrf instance management

! Configure the physical management port
interface Management1
   description OOBM-TO-ANSIBLE
   vrf management
   ip address 10.10.10.106/24
   no shutdown
exit

copy run start
```

## karlo-cn-prd-al02 Bootstrap Config  

```text
enable
config

! Management User
username leafadmin privilege 15 secret "{{ vault_leaf_admin_pass }}"
enable password "{{ vault_leaf_enable_pass }}"

! Enable eAPI
management api http-commands
   protocol https
   no protocol HTTP
   no shutdown
   vrf management
   no shutdown
   exit

! Create the management VRF
vrf instance management

! Configure the physical management port
interface Management1
   description OOBM-TO-ANSIBLE
   vrf management
   ip address 10.10.10.107/24
   no shutdown
exit

copy run start
```

## karlo-cn-prd-al03 Bootstrap Config  

```text
enable
config

! Management User
username leafadmin privilege 15 secret "{{ vault_leaf_admin_pass }}"
enable password "{{ vault_leaf_enable_pass }}"

! Enable eAPI
management api http-commands
   protocol https
   no protocol HTTP
   no shutdown
   vrf management
   no shutdown
   exit

! Create the management VRF
vrf instance management

! Configure the physical management port
interface Management1
   description OOBM-TO-ANSIBLE
   vrf management
   ip address 10.10.10.108/24
   no shutdown
exit

copy run start
```

## karlo-cn-prd-al04 Bootstrap Config  

```text
enable
config

! Management User
username leafadmin privilege 15 secret "{{ vault_leaf_admin_pass }}"
enable password "{{ vault_leaf_enable_pass }}"

! Enable eAPI
management api http-commands
   protocol https
   no protocol HTTP
   no shutdown
   vrf management
   no shutdown
   exit

! Create the management VRF
vrf instance management

! Configure the physical management port
interface Management1
   description OOBM-TO-ANSIBLE
   vrf management
   ip address 10.10.10.109/24
   no shutdown
exit

copy run start
```

## karlo-cn-prd-sp01 Bootstrap Config  

```text
enable
config

! Management User
username spineadmin privilege 15 secret "{{ vault_spine_admin_pass }}"
enable password "{{ vault_spine_enable_pass }}"

! Enable eAPI
management api http-commands
   protocol https
   no protocol http
   no shutdown
   vrf management
   no shutdown
   exit

! Create the management VRF
vrf instance management

! Configure the physical management port
interface Management1
   description OOBM-TO-ANSIBLE
   vrf management
   ip address 10.10.10.104/24
   no shutdown
exit

copy run start
```

## karlo-cn-prd-sp02 Bootstrap Config  

```text
enable
config

! Management User
username spineadmin privilege 15 secret "{{ vault_spine_admin_pass }}"
enable password "{{ vault_spine_enable_pass }}"

! Enable eAPI
management api http-commands
   protocol https
   no protocol http
   no shutdown
   vrf management
   no shutdown
   exit

! Create the management isolation container
vrf instance management

! Configure the physical management port
interface Management1
   description OOBM-TO-ANSIBLE
   vrf management
   ip address 10.10.10.105/24
   no shutdown
exit

copy run start
```
