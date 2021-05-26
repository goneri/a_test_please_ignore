# vmware.vmware_rest.content_locallibrary

**Creates a new local library.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Status

## Synopsis


* Creates a new local library.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Adjust vpxd configuration
  vmware.vmware_rest.appliance_vmon_service:
    service: vpxd
    startup_type: AUTOMATIC
  register: result

- name: Set datastore id
  set_fact:
    datastore_id: '{{ result.value[0].datastore }}'

- name: Define datastore_name and local_library_name
  set_fact:
    datastore_name: rw_datastore
    local_library_name: local_library_001

- name: Create a new content library
  vmware.vmware_rest.content_locallibrary:
    name: '{{ local_library_name }}'
    description: automated
    publish_info:
      published: true
      authentication_method: NONE
    storage_backings:
    - datastore_id: '{{ datastore_id }}'
      type: DATASTORE
    state: present
  register: result

- name: Retrieve the local content library information
  vmware.vmware_rest.content_locallibrary_info:
  register: result

- name: Set test local library id for further testing
  set_fact:
    test_library_id: '{{ result.value[0] }}'

- name: Delete local content library
  vmware.vmware_rest.content_locallibrary:
    library_id: '{{ test_library_id }}'
    state: absent
  register: result
```

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
