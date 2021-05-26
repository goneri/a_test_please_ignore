# vmware.vmware_rest.vcenter_datastore_info

**Retrieves information about the datastore indicated by {@param.name
datastore}.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Retrieves information about the datastore indicated by
mailto:[{@param.name](mailto:{@param.name) datastore}.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Retrieve a list of all the datastores
  vmware.vmware_rest.vcenter_datastore_info:
  register: my_datastores

- name: We can also use filter to limit the number of result
  vmware.vmware_rest.vcenter_datastore_info:
    filter_names:
    - rw_datastore
  register: my_datastores
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
