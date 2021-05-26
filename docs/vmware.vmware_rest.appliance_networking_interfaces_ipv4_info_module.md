# vmware.vmware_rest.appliance_networking_interfaces_ipv4_info

**Get IPv4 network configuration for specific NIC.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Get IPv4 network configuration for specific NIC.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Collect the IPv4 network information
  vmware.vmware_rest.appliance_networking_interfaces_ipv4_info:
    interface_name: nic0
  register: result
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
