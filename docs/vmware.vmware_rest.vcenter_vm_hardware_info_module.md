# vmware.vmware_rest.vcenter_vm_hardware_info

**Returns the virtual hardware settings of a virtual machine.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Returns the virtual hardware settings of a virtual machine.

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

- name: Collect the hardware information
  vmware.vmware_rest.vcenter_vm_hardware_info:
    vm: '{{ search_result.value[0].vm }}'
  register: my_vm1_hardware_info
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
