# vmware.vmware_rest.appliance_ntp_info

**Get the NTP configuration status**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Get the NTP configuration status. If you run the ‘timesync.get’
command, you can retrieve the current time synchronization method
(NTP- or VMware Tools-based). The ‘ntp’ command always returns the
NTP server information, even when the time synchronization mode is
not set to NTP. If the time synchronization mode is not NTP-based,
the NTP server status is displayed as down.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Get the NTP configuration
  vmware.vmware_rest.appliance_ntp_info:

- name: Get the NTP configuration
  vmware.vmware_rest.appliance_ntp_info:
  register: result
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
