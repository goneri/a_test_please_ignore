# vmware.vmware_rest.appliance_access_shell

**Set enabled state of BASH, that is, access to BASH from within the
controlled CLI.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Set enabled state of BASH, that is, access to BASH from within the
controlled CLI.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Disable the Shell
  vmware.vmware_rest.appliance_access_shell:
    enabled: false
    timeout: 600

- name: Enable the Shell with a timeout
  vmware.vmware_rest.appliance_access_shell:
    enabled: true
    timeout: 600
  register: result
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
