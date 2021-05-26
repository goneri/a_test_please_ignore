# vmware.vmware_rest.vcenter_datacenter_info

**Retrieves information about the datacenter corresponding to
{@param.name datacenter}.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Retrieves information about the datacenter corresponding to
mailto:[{@param.name](mailto:{@param.name) datacenter}.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Get a list of all the datacenters
  register: existing_datacenters
  vmware.vmware_rest.vcenter_datacenter_info:

- name: collect a list of the datacenters
  vmware.vmware_rest.vcenter_datacenter_info:
  register: my_datacenters
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
