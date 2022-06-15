# Ansible Network Utilities

Ansible Collection for Network utilities, roles and so on

Collection Name: `ginigangadharan.network_utilities`

Install this collection locally:

```shell
ansible-galaxy collection install ginigangadharan.network_utilities
```

## Testing Modules

Pass appropriate hostname or group name for `nw_devices`

```shell
$ ansible-playbook playbooks/network-test.yaml -e "nw_devices=asa"
```

## Sample Playbooks

Sample playbooks are available in [playbooks](playbooks) directory.

```yaml
---
- name: Network Connection Test
  hosts: "{{ nw_devices }}"
  gather_facts: no
  
  tasks:
    - name: Set network os
      ansible.builtin.set_fact:
        network_os: "{{ ansible_network_os | replace('.','-') }}"
    - debug:
        msg: "{{ network_os }}"

    - name: Check Device Connection
      include_role:
        name: network-connection-test
```
## Publishing Ansible Collection

### Configure `ANSIBLE_GALAXY_TOKEN`

```shell
$ export ANSIBLE_GALAXY_TOKEN='YOUR_ANSIBLE_GALAXY_API_TOKEN'
```
### Using Ansible Playbook

The Ansible playbook will build and publish the Ansible Collection with proper version tag. 

```shell
$ ansible-playbook utilities/update-collection.yaml -e "tag=1.0.10"
```

### Manual Method


**Building the collection archive**

`ansible-galaxy collection build` command will create the collection archive with version information which you can publish to Ansible Galaxy.

```shell
$ ansible-galaxy collection build
# --force to overwrite if any existing archive with same version 
```

**Publish the Collection to Ansible Galaxy**

Make sure you have created the Ansible Galaxy API Token and exported as environment variable `ANSIBLE_GALAXY_TOKEN` before you call the publish command.

```shell
$ ansible-galaxy collection publish \
  ./NAMESPACE_COLLECTIONNAME-{{ tag }}.tar.gz \
  --api-key $ANSIBLE_GALAXY_TOKEN \
  --ignore-certs
```