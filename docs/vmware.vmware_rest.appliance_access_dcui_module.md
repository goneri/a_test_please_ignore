# vmware.vmware_rest.appliance_access_dcui

**Set enabled state of Direct Console User Interface (DCUI TTY2).**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Set enabled state of Direct Console User Interface (DCUI TTY2).

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Disable the Direct Console User Interface
  vmware.vmware_rest.appliance_access_dcui:
    enabled: false
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
