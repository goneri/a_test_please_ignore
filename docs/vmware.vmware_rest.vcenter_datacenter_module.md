# vmware.vmware_rest.vcenter_datacenter

**Create a new datacenter in the vCenter inventory**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Create a new datacenter in the vCenter inventory

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

- name: Force delete the existing DC
  vmware.vmware_rest.vcenter_datacenter:
    state: absent
    datacenter: '{{ item.datacenter }}'
    force: true
  with_items: '{{ existing_datacenters.value }}'

- name: Build a list of all the folders
  vmware.vmware_rest.vcenter_folder_info:
  register: my_folders

- name: Set my_datacenter_folder
  set_fact:
    my_datacenter_folder: '{{ my_folders.value|selectattr("type", "equalto", "DATACENTER")|first
      }}'

- name: Create datacenter my_dc
  vmware.vmware_rest.vcenter_datacenter:
    name: my_dc
    folder: '{{ my_datacenter_folder.folder }}'
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
