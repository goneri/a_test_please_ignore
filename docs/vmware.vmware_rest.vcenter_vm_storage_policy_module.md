# vmware.vmware_rest.vcenter_vm_storage_policy

**Updates the storage policy configuration of a virtual machine and/or
its associated virtual hard disks.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Updates the storage policy configuration of a virtual machine
and/or its associated virtual hard disks.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Look up the VM called test_vm1 in the inventory
  register: search_result
  vmware.vmware_rest.vcenter_vm_info:
    filter_names:
    - test_vm1

- name: Collect information about a specific VM
  vmware.vmware_rest.vcenter_vm_info:
    vm: '{{ search_result.value[0].vm }}'
  register: test_vm1_info

- name: Prepare the disk policy dict
  set_fact:
    vm_disk_policy: "{{ {} | combine({ my_new_disk.id: {'policy': my_storage_policy.policy,\
      \ 'type': 'USE_SPECIFIED_POLICY'} }) }}"

- name: Adjust VM storage policy
  vmware.vmware_rest.vcenter_vm_storage_policy:
    vm: '{{ test_vm1_info.id }}'
    vm_home:
      type: USE_DEFAULT_POLICY
    disks: '{{ vm_disk_policy }}'
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
