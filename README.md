# ansible-role-common

---

## TODO

[ ] Add download of slurm exporter

## General

This role is used to set up the basic packages and tools required for each server/instance at SANBI.

In it's currect state this role will set up:
1) Packages for Ceph filesystem/RBD access
2) FreeIPA client and join the SANBI domain
3) Install node_exporter for monitoring
4) Install consul and join the master

## Requirements

- Python must be installed on clients
- FreeIPA server installed and available
- Servers have all-lowercase FQDN as their hostnames
- Consul server available on network

## Variables

### Genearl Variables

| Variable                  | Type | Description                                        |
|---------------------------|------|----------------------------------------------------|
| `timezone`                | str  | Sets the timezone of the host.                     |
| `force_hostname_override` | bool | Sets the hostname of the targetted host with the ansible `inventory_hostname`. |

### FreeIPA Related

| Variable                  | Type | Description                                         |
|---------------------------|------|-----------------------------------------------------|
| `freeipa_user`            | str   | The username of an admin for the FreeIPA server.   |
| `freeipa_pass`            | str   | The password for above mentioned user.             |
| `freeipa_domain`          | str   | The domain for the target server/instance to join. |
| `freeipa_server_hostname` | str   | The fqdn of the server with FreeIPA installed.     |
| `freeipa_realm`           | str   |The realm you want the target to join.              |
| `freeipa_force_join`      | bool  |Forces jouning of the realm in the case that host is already registered.|

### Monitoring Related

| Variable                | Type | Description                                          |
|-------------------------|------|------------------------------------------------------|
| `node_exporter_version` | str  | The version of node_exporter to install to target.   |
| `enable_monitoring`     | bool | Globally enable or disable install of monitoring.    |
| `enable_node_exporter`  | bool | Globally enable or disable install of node_exporter. |
| `enable_consul_agent`   | bool | Globally enable or disable install of consul agent. |


## Usage

1. Set the appropriate variables (listed above).
2. Create a playbook that includes this role. Following is an example.
   ```yaml
   ---
   - hosts: all
     become: true
     roles:
       - role: ansible-role-common
   ```