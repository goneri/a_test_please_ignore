# vmware.vmware_rest.appliance_shutdown

**Cancel pending shutdown action.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Cancel pending shutdown action.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Shutdown the appliance
  vmware.vmware_rest.appliance_shutdown:
    state: poweroff
    reason: this is an example
    delay: 600
  register: result

- name: Abort the shutdown of the appliance
  vmware.vmware_rest.appliance_shutdown:
    state: cancel
  register: result

- name: Reboot the appliance
  vmware.vmware_rest.appliance_shutdown:
    state: reboot
    reason: this is an example
    delay: 600
  register: result

- name: Abort the reboot
  vmware.vmware_rest.appliance_shutdown:
    state: cancel
  register: result

- name: Abort the reboot (again)
  vmware.vmware_rest.appliance_shutdown:
    state: cancel
  register: result
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
