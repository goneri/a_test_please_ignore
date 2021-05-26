# vmware.vmware_rest.appliance_networking_dns_hostname_info

**Get the Fully Qualified Doman Name.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Get the Fully Qualified Doman Name.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Get the hostname configuration
  vmware.vmware_rest.appliance_networking_dns_hostname_info:
  register: result
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
