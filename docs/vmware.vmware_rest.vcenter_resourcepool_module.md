# vmware.vmware_rest.vcenter_resourcepool

**Creates a resource pool.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Creates a resource pool.

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

- name: Create an Ad hoc resource pool
  vmware.vmware_rest.vcenter_resourcepool:
    name: my_resource_pool
    parent: '{{ resource_pools.value[0].resource_pool }}'
    cpu_allocation:
      expandable_reservation: true
      limit: 40
      reservation: 0
      shares:
        level: NORMAL
    memory_allocation:
      expandable_reservation: false
      limit: 2000
      reservation: 0
      shares:
        level: NORMAL
  register: my_resource_pool

- name: Remove a resource pool
  vmware.vmware_rest.vcenter_resourcepool:
    resource_pool: '{{ my_resource_pool.id }}'
    state: absent

- name: Create a generic resource pool
  vmware.vmware_rest.vcenter_resourcepool:
    name: my_resource_pool
    parent: '{{ resource_pools.value[0].resource_pool }}'
  register: my_resource_pool

- name: Modify a resource pool
  vmware.vmware_rest.vcenter_resourcepool:
    resource_pool: '{{ my_resource_pool.id }}'
    cpu_allocation:
      expandable_reservation: true
      limit: -1
      reservation: 0
      shares:
        level: NORMAL
    memory_allocation:
      expandable_reservation: false
      limit: 1000
      reservation: 0
      shares:
        level: NORMAL
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
