# vmware.vmware_rest.vcenter_host_info

**Returns information about at most 2500 visible (subject to
permission checks) hosts in vCenter matching the {@link FilterSpec}.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Returns information about at most 2500 visible (subject to
permission checks) hosts in vCenter matching the mailto:[{@link](mailto:{@link)
FilterSpec}.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Get a list of the hosts
  vmware.vmware_rest.vcenter_host_info:
  register: my_hosts
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
