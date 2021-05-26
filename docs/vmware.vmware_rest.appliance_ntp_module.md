# vmware.vmware_rest.appliance_ntp

**Set NTP servers**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Set NTP servers. This method updates old NTP servers from
configuration and sets the input NTP servers in the configuration.
If NTP based time synchronization is used internally, the NTP
daemon will be restarted to reload given NTP configuration. In case
NTP based time synchronization is not used, this method only
replaces servers in the NTP configuration.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Adjust the NTP configuration
  vmware.vmware_rest.appliance_ntp:
    servers:
    - time.google.com

- name: Adjust the NTP configuration (again)
  vmware.vmware_rest.appliance_ntp:
    servers:
    - time.google.com
  register: result

- name: Test the NTP configuration
  vmware.vmware_rest.appliance_ntp:
    state: test
    servers:
    - time.google.com
  register: result
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
