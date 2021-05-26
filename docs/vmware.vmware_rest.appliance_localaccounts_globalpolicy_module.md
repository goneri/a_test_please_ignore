# vmware.vmware_rest.appliance_localaccounts_globalpolicy

**Set the global password policy.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Set the global password policy.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Update the global policy of the local accounts
  vmware.vmware_rest.appliance_localaccounts_globalpolicy:
    warn_days: 5

- name: Update the global policy of the local accounts (idempotency)
  vmware.vmware_rest.appliance_localaccounts_globalpolicy:
    warn_days: 5
  register: result
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
