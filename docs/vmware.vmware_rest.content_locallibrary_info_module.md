# vmware.vmware_rest.content_locallibrary_info

**Returns a given local library.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Status

## Synopsis


* Returns a given local library.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Retrieve the local content library information
  vmware.vmware_rest.content_locallibrary_info:
  register: result

- name: Set test local library id for further testing
  set_fact:
    test_library_id: '{{ result.value[0] }}'

- name: Retrieve the local content library information based upon id check mode
  vmware.vmware_rest.content_locallibrary_info:
    library_id: '{{ test_library_id }}'
  register: result
  check_mode: true

- name: Retrieve the local content library information based upon id
  vmware.vmware_rest.content_locallibrary_info:
    library_id: '{{ test_library_id }}'
  register: result
```

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
