# vmware.vmware_rest.vcenter_vm_hardware_adapter_sata

**Adds a virtual SATA adapter to the virtual machine.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Adds a virtual SATA adapter to the virtual machine.

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

- name: Create a SATA adapter at PCI slot 34
  vmware.vmware_rest.vcenter_vm_hardware_adapter_sata:
    vm: '{{ test_vm1_info.id }}'
    pci_slot_number: 34

- name: Remove SATA adapter at PCI slot 34
  vmware.vmware_rest.vcenter_vm_hardware_adapter_sata:
    vm: '{{ test_vm1_info.id }}'
    pci_slot_number: 34
    state: absent
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
