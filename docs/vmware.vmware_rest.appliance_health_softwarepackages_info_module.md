# vmware.vmware_rest.appliance_health_softwarepackages_info

**Get information on available software updates available in the
remote vSphere Update Manager repository**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Get information on available software updates available in the
remote vSphere Update Manager repository. Red indicates that
security updates are available. Orange indicates that non-security
updates are available. Green indicates that there are no updates
available. Gray indicates that there was an error retreiving
information on software updates.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Get the health of the software package manager
  vmware.vmware_rest.appliance_health_softwarepackages_info:
  register: result
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
