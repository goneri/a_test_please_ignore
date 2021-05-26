# vmware.vmware_rest.appliance_networking_firewall_inbound_info

**Get the ordered list of firewall rules**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Get the ordered list of firewall rules. Within the list of traffic
rules, rules are processed in order of appearance, from top to
bottom. When a connection matches a firewall rule, further
processing for the connection stops, and the appliance ignores any
additional firewall rules you have set.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Get the firewall inbound configuration
  vmware.vmware_rest.appliance_networking_firewall_inbound_info:
  register: result
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
