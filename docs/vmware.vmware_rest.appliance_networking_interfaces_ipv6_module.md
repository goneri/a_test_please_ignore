# vmware.vmware_rest.appliance_networking_interfaces_ipv6

**Set IPv6 network configuration for specific interface.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Set IPv6 network configuration for specific interface.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Set the IPv6 network information of nic99 (which does not exist)
  vmware.vmware_rest.appliance_networking_interfaces_ipv6:
    interface_name: nic99
    mode: DHCP
  failed_when:
  - not(result.failed)
  - result.value.messages[0].default_message msg == "The interface is unknown."
  register: result
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
