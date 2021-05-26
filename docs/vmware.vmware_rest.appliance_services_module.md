# vmware.vmware_rest.appliance_services

**Restarts a service**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Restarts a service

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Stop the ntpd service
  vmware.vmware_rest.appliance_services:
    service: ntpd
    state: stop
  register: result

- name: Start the ntpd service
  vmware.vmware_rest.appliance_services:
    service: ntpd
    state: start
  register: result

- name: Restart the ntpd service
  vmware.vmware_rest.appliance_services:
    service: ntpd
    state: restart
  register: result
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
