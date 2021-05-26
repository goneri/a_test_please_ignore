# vmware.vmware_rest.vcenter_folder_info

**Returns information about at most 1000 visible (subject to
permission checks) folders in vCenter matching the {@link
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
permission checks) folders in vCenter matching the mailto:[{@link](mailto:{@link)
FilterSpec}.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Build a list of all the folders
  vmware.vmware_rest.vcenter_folder_info:
  register: my_folders

- name: Build a list of the folders, with a filter
  vmware.vmware_rest.vcenter_folder_info:
    filter_type: DATASTORE

- name: Build a list of all the folders with the type VIRTUAL_MACHINE and called vm
  vmware.vmware_rest.vcenter_folder_info:
    filter_type: VIRTUAL_MACHINE
    filter_names:
    - vm
  register: my_folders
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
