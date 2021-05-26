# vmware.vmware_rest.vcenter_storage_policies_info

**Returns information about at most 1024 visible (subject to
permission checks) storage solicies availabe in vCenter**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Returns information about at most 1024 visible (subject to
permission checks) storage solicies availabe in vCenter. These
storage policies can be used for provisioning virtual machines or
disks.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: List existing storage policies
  vmware.vmware_rest.vcenter_storage_policies_info:
  register: storage_policies
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
