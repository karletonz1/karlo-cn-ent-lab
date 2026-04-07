# Arista vEOS Bootstrap Configuration

Manual Bootstrap: Minimum configuration required via CLI to allow Ansible to reach the devices before pushing the remaining full configuration.  

> [!TIP]  
> For all switches, run `#zerotouch cancel` first to stop Arista ZTP and enter manual configuration mode. This will trigger an immediate switch reload. 

## karlo-cn-leaf-01 Bootstrap Config  

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
   ip address 10.0.10.106/24
   no shutdown
exit

copy run start
```

## karlo-cn-leaf-02 Bootstrap Config  

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
   ip address 10.0.10.107/24
   no shutdown
exit

copy run start
```

## karlo-cn-spine-01 Bootstrap Config  

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
   ip address 10.0.10.104/24
   no shutdown
exit

copy run start
```

## karlo-cn-spine-02 Bootstrap Config  

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
   ip address 10.0.10.105/24
   no shutdown
exit

copy run start
```
