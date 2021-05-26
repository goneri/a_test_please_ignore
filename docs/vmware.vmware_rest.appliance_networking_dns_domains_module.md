# vmware.vmware_rest.appliance_networking_dns_domains

**Set DNS search domains.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Set DNS search domains.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Update the domain configuration
  vmware.vmware_rest.appliance_networking_dns_domains:
    domains:
    - foobar
  register: result

- name: Update the domain configuration (again)
  vmware.vmware_rest.appliance_networking_dns_domains:
    domains:
    - foobar
  register: result

- name: Add another domain configuration
  vmware.vmware_rest.appliance_networking_dns_domains:
    domain: barfoo
    state: add
  register: result
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
