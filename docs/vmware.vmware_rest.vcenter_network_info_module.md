# vmware.vmware_rest.vcenter_network_info

**Returns information about at most 1000 visible (subject to
permission checks) networks in vCenter matching the {@link
FilterSpec}.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Returns information about at most 1000 visible (subject to
permission checks) networks in vCenter matching the mailto:[{@link](mailto:{@link)
FilterSpec}.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Get a list of the networks
  vmware.vmware_rest.vcenter_network_info:
  register: my_network_value

- name: Get a list of the networks with a filter
  vmware.vmware_rest.vcenter_network_info:
    filter_types: STANDARD_PORTGROUP
  register: my_standard_portgroup_value
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
