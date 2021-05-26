# vmware.vmware_rest.appliance_networking_dns_servers

**Set the DNS server configuration**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Set the DNS server configuration. If you set the mode argument to
“DHCP”, a DHCP refresh is forced.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Set the DNS servers
  vmware.vmware_rest.appliance_networking_dns_servers:
    servers:
    - 1.1.1.1
  register: result
  ignore_errors: true  # May be failing because of the CI set-up

- name: Set the DNS servers (again)
  vmware.vmware_rest.appliance_networking_dns_servers:
    servers:
    - 1.1.1.1
  register: result
  ignore_errors: true  # May be failing because of the CI set-up

- name: Test the DNS servers
  vmware.vmware_rest.appliance_networking_dns_servers:
    state: test
    servers:
    - var
  register: result
  #ignore_errors: True  # May be failing because of the CI set-up
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
