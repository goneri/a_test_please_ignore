# vmware.vmware_rest.appliance_monitoring_query

**Get monitoring data.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Get monitoring data.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Query the monitoring backend
  vmware.vmware_rest.appliance_monitoring_query:
    end_time: 2021-04-14T09:34:56Z
    start_time: 2021-04-14T08:34:56Z
    names:
    - mem.total
    interval: MINUTES5
    function: AVG
  register: result
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
