# vmware.vmware_rest.appliance_localaccounts_info

**Get the local user account information.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Get the local user account information.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: List the local accounts
  vmware.vmware_rest.appliance_localaccounts_info:
  register: result

- name: Get the information about the new account
  vmware.vmware_rest.appliance_localaccounts_info:
    username: foobar
  register: result
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
