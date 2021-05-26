# vmware.vmware_rest.vcenter_vm_hardware_boot_device_info

**Returns an ordered list of boot devices for the virtual machine**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Returns an ordered list of boot devices for the virtual machine. If
the mailto:[{@term](mailto:{@term) list} is empty, the virtual machine uses a
default boot sequence.

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

- name: Get boot device info
  vmware.vmware_rest.vcenter_vm_hardware_boot_device_info:
    vm: '{{ test_vm1_info.id }}'

- name: Get information about the boot device
  vmware.vmware_rest.vcenter_vm_hardware_boot_device_info:
    vm: '{{ test_vm1_info.id }}'
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
