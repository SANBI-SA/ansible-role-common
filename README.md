# ansible-role-common

In it's currect state this role will set up:
  - Packages for Ceph filesystem/RBD access
  - FreeIPA client and join the SANBI domain

## Requirements:
- FreeIPA server installed and available
- Servers have FQDN as their hostnames

## To use:

1. Set `freeipa_pass` in `Freeipa.yml` to admin pass for your FreeIPA server OR add `freeipa_pass` to your playbook.
2. Create a playbook that includes this role. E.g.:
   ```shell
   - hosts: all
     become: true
     gather_facts: false
     pre_tasks:
     - name: Install python2 for Ansible
       raw: bash -c "test -e /usr/bin/python || (apt -qqy update && apt install -qqy python-minimal)"
       register: output
       changed_when: output.stdout != ""
     - name: Gathering Facts
       setup:
     roles:
       - role: ansible-role-common
   ```