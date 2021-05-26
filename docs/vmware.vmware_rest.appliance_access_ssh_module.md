# vmware.vmware_rest.appliance_access_ssh

**Set enabled state of the SSH-based controlled CLI.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Set enabled state of the SSH-based controlled CLI.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Ensure the SSH access ie enabled
  vmware.vmware_rest.appliance_access_ssh:
    enabled: true
  register: result
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
