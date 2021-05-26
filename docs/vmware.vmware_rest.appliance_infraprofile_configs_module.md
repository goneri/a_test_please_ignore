# vmware.vmware_rest.appliance_infraprofile_configs

**Exports the desired profile specification.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Exports the desired profile specification.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Export the ApplianceManagement profile
  vmware.vmware_rest.appliance_infraprofile_configs:
    state: export
    profiles:
    - ApplianceManagement
  register: result
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
