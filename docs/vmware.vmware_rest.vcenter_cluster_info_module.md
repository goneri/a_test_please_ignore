# vmware.vmware_rest.vcenter_cluster_info

**Retrieves information about the cluster corresponding to
{@param.name cluster}.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Retrieves information about the cluster corresponding to
mailto:[{@param.name](mailto:{@param.name) cluster}.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: get all the clusters called my_cluster
  vmware.vmware_rest.vcenter_cluster_info:
    filter_names:
    - my_cluster
  register: my_cluster

- name: Build a list of all the clusters
  vmware.vmware_rest.vcenter_cluster_info:
  register: all_the_clusters

- name: Retrieve details about the first cluster
  vmware.vmware_rest.vcenter_cluster_info:
    cluster: '{{ all_the_clusters.value[0].cluster }}'
  register: my_cluster_info
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
