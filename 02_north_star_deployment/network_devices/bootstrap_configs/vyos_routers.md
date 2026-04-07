# Router Bootstrap Configuration

Manual Bootstrap: Minimum configuration required via CLI to allow Ansible to reach the devices before pushing the remaining configuration via automation.

>[!NOTE]
> The default credentials for logging into a new VyOS router is username: vyos, password: vyos

## karlo-cn-router-01

```text
# Enter configuration mode
configure

# 1. Assign the Management IP
set interfaces ethernet eth0 address '192.168.10.102/24'

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
set interfaces ethernet eth0 address '192.168.10.103/24'

# 2. Enable the SSH service
set service ssh port '22'

# 3. Set a password for the 'vyos' user
set system login user routeradmin authentication plaintext-password "{{ vault_router_admin_pass }}"

# Apply and save the configuration
commit
save
exit
```
