# VyOS Router Bootstrap

## Purpose

This document serves as the initial configuration checklist for bootstrapping the VyOS routers. It is the minimum configuration required via CLI to allow Ansible to reach the devices before pushing the remaining configuration via automation.

## Prerequisites

:white_check_mark: GNS3 topology is cabled according to the physical design.

:white_check_mark: All routers are powered on.

:white_check_mark: GNS3 console access is working.  

Manual Bootstrap: Minimum configuration required via CLI to allow Ansible to reach the devices before pushing the remaining configuration via automation.

>[!NOTE]
> The default credentials for logging into a new VyOS router is username: vyos, password: vyos

## karlo-cn-router-01

```text
# Enter configuration mode
configure

# 1. Assign the Management IP
set interfaces ethernet eth0 address '10.0.10.102/24'

# 2. Enable the SSH service
set service ssh port '22'

# 3. Set a password for the 'vyos' user
set system login user routeradmin authentication plaintext-password "{{ vault_router_admin_pass }}"

# Apply and save the configuration
commit
save
exit
```

## karlo-cn-router-02

```text
# Enter configuration mode
configure

# 1. Assign the Management IP
set interfaces ethernet eth0 address '10.0.10.103/24'

# 2. Enable the SSH service
set service ssh port '22'

# 3. Set a password for the 'vyos' user
set system login user routeradmin authentication plaintext-password "{{ vault_router_admin_pass }}"

# Apply and save the configuration
commit
save
exit
```
