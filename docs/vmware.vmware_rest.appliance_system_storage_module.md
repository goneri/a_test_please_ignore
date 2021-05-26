# vmware.vmware_rest.appliance_system_storage

**Resize all partitions to 100 percent of disk size.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Resize all partitions to 100 percent of disk size.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Resize the first partition and return the state of the partition before and
    after the operation
  vmware.vmware_rest.appliance_system_storage:
    state: resize_ex
  register: result

- name: Resize the first partition
  vmware.vmware_rest.appliance_system_storage:
    state: resize
  register: result
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
