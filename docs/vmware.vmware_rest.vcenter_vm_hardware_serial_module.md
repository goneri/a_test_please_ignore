# vmware.vmware_rest.vcenter_vm_hardware_serial

**Adds a virtual serial port to the virtual machine.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Adds a virtual serial port to the virtual machine.

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

- name: Create a new serial port
  vmware.vmware_rest.vcenter_vm_hardware_serial:
    vm: '{{ test_vm1_info.id }}'
    label: Serial port 2
    allow_guest_control: true

- name: Create another serial port with a label
  vmware.vmware_rest.vcenter_vm_hardware_serial:
    vm: '{{ test_vm1_info.id }}'
    label: Serial port 2
    allow_guest_control: true

- name: Create an existing serial port (label)
  vmware.vmware_rest.vcenter_vm_hardware_serial:
    vm: '{{ test_vm1_info.id }}'
    label: Serial port 1
    allow_guest_control: true

- name: Get an existing serial port (label)
  vmware.vmware_rest.vcenter_vm_hardware_serial_info:
    vm: '{{ test_vm1_info.id }}'
    label: Serial port 1
  register: serial_port_1

- name: Delete an existing serial port (port id)
  vmware.vmware_rest.vcenter_vm_hardware_serial:
    vm: '{{ test_vm1_info.id }}'
    port: '{{ serial_port_1.id }}'
    state: absent

- name: Delete an existing serial port (label)
  vmware.vmware_rest.vcenter_vm_hardware_serial:
    vm: '{{ test_vm1_info.id }}'
    label: Serial port 2
    state: absent
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
