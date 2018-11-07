# ansible-role-common

---

This role is used to set up the basic packages and tools required for each server/instance at SANBI.

In it's currect state this role will set up:
1) Packages for Ceph filesystem/RBD access
2) FreeIPA client and join the SANBI domain
3) Install node_exporter for monitoring

## Requirements

- FreeIPA server installed and available
- Servers have all-lowercase FQDN as their hostnames

## Variables

### FreeIPA Related

| Variable                  | Type | Description                                        |
|---------------------------|------|----------------------------------------------------|
| `freeipa_user`            | str  | The username of an admin for the FreeIPA server.   |
| `freeipa_pass`            | str  | The password for above mentioned user.             |
| `freeipa_domain`          | str  | The domain for the target server/instance to join. |
| `freeipa_server_hostname` | str  | The fqdn of the server with FreeIPA installed.     |
| `freeipa_realm`           | str  |The realm you want the target to join.              |

### Monitoring Related

| Variable                | Type | Description                                          |
|-------------------------|------|------------------------------------------------------|
| `node_exporter_version` | str  | The version of node_exporter to install to target.   |
| `enable_monitoring`     | bool | Globally enable or disable install of monitoring.    |
| `enable_node_exporter`  | bool | Globally enable or disable install of node_exporter. |

## Usage

1. Set the appropriate variables (listed above).
2. Create a playbook that includes this role. Following is an example.
   ```shell
   - hosts: all
     become: true
     # The pre_tasks section will disable gathering of
     # facts to first install python2 if it is not
     # already installed and then gather facts.
     gather_facts: false
     pre_tasks:
     - name: Install python2 for Ansible
       raw: bash -c "test -e /usr/bin/python || (apt -qqy update && apt install -qqy python-minimal)"
       register: output
       changed_when: output.stdout != ""
     - name: Gathering Facts
       setup:
     roles:
       # This is actually where this role
       # (ansible-role-common) is being included.
       - role: ansible-role-common
   ```