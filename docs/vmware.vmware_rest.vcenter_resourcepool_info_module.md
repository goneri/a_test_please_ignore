# vmware.vmware_rest.vcenter_resourcepool_info

**Retrieves information about the resource pool indicated by
{@param.name resourcePool}.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Retrieves information about the resource pool indicated by
mailto:[{@param.name](mailto:{@param.name) resourcePool}.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Get the existing resource pools
  vmware.vmware_rest.vcenter_resourcepool_info:
  register: resource_pools

- name: Get the existing resource pool
  vmware.vmware_rest.vcenter_resourcepool_info:
    resource_pool: '{{ resource_pools.value[0].resource_pool }}'
  register: my_resource_pool

- name: Create a generic resource pool
  vmware.vmware_rest.vcenter_resourcepool:
    name: my_resource_pool
    parent: '{{ resource_pools.value[0].resource_pool }}'
  register: my_resource_pool

- name: Read details from a specific resource pool
  vmware.vmware_rest.vcenter_resourcepool_info:
    resource_pool: '{{ my_resource_pool.id }}'
  register: my_resource_pool
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
