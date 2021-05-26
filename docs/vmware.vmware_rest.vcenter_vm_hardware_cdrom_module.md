# vmware.vmware_rest.vcenter_vm_hardware_cdrom

**Adds a virtual CD-ROM device to the virtual machine.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Adds a virtual CD-ROM device to the virtual machine.

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

- name: Attach an ISO image to a guest VM
  vmware.vmware_rest.vcenter_vm_hardware_cdrom:
    vm: '{{ test_vm1_info.id }}'
    type: SATA
    sata:
      bus: 0
      unit: 2
    start_connected: true
    backing:
      iso_file: '[ro_datastore] fedora.iso'
      type: ISO_FILE
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
