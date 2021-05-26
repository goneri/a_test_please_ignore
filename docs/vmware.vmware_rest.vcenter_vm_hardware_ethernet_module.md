# vmware.vmware_rest.vcenter_vm_hardware_ethernet

**Adds a virtual Ethernet adapter to the virtual machine.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Adds a virtual Ethernet adapter to the virtual machine.

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

- name: Retrieve details about the portgroup
  community.vmware.vmware_dvs_portgroup_info:
    validate_certs: no
    datacenter: my_dc
  register: my_portgroup_info

- name: Attach a VM to a dvswitch
  vmware.vmware_rest.vcenter_vm_hardware_ethernet:
    vm: '{{ test_vm1_info.id }}'
    pci_slot_number: 4
    backing:
      type: DISTRIBUTED_PORTGROUP
      network: '{{ my_portgroup_info.dvs_portgroup_info.dvswitch1[0].key }}'
    start_connected: false
  register: vm_hardware_ethernet_1

- name: Turn the NIC's start_connected flag on
  vmware.vmware_rest.vcenter_vm_hardware_ethernet:
    nic: '{{ vm_hardware_ethernet_1.id }}'
    start_connected: true
    vm: '{{ test_vm1_info.id }}'
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
