# vmware.vmware_rest.vcenter_vm

**Creates a virtual machine.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Creates a virtual machine.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Collect the list of the existing VM
  vmware.vmware_rest.vcenter_vm_info:
  register: existing_vms
  until: existing_vms is not failed

- name: Delete some VM
  vmware.vmware_rest.vcenter_vm:
    state: absent
    vm: '{{ item.vm }}'
  with_items: '{{ existing_vms.value }}'

- name: Build a list of all the folders with the type VIRTUAL_MACHINE and called vm
  vmware.vmware_rest.vcenter_folder_info:
    filter_type: VIRTUAL_MACHINE
    filter_names:
    - vm
  register: my_folders

- name: Set my_virtual_machine_folder
  set_fact:
    my_virtual_machine_folder: '{{ my_folders.value|first }}'

- name: We can also use filter to limit the number of result
  vmware.vmware_rest.vcenter_datastore_info:
    filter_names:
    - rw_datastore
  register: my_datastores

- name: Set my_datastore
  set_fact:
    my_datastore: '{{ my_datastores.value|first }}'

- name: Build a list of all the clusters
  vmware.vmware_rest.vcenter_cluster_info:
  register: all_the_clusters

- name: Retrieve details about the first cluster
  vmware.vmware_rest.vcenter_cluster_info:
    cluster: '{{ all_the_clusters.value[0].cluster }}'
  register: my_cluster_info

- name: Create a VM
  vmware.vmware_rest.vcenter_vm:
    placement:
      cluster: '{{ my_cluster_info.id }}'
      datastore: '{{ my_datastore.datastore }}'
      folder: '{{ my_virtual_machine_folder.folder }}'
      resource_pool: '{{ my_cluster_info.value.resource_pool }}'
    name: test_vm1
    guest_OS: DEBIAN_8_64
    hardware_version: VMX_11
    memory:
      hot_add_enabled: true
      size_MiB: 1024
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
