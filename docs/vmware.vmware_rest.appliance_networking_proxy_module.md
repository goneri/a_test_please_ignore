# vmware.vmware_rest.appliance_networking_proxy

**Configures which proxy server to use for the specified protocol**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Configures which proxy server to use for the specified protocol.
This operation sets environment variables for using proxy. In order
for this configuration to take effect a logout / service restart is
required.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Set the HTTP proxy configuration
  vmware.vmware_rest.appliance_networking_proxy:
    enabled: true
    server: http://47.244.50.194
    port: 8081
    protocol: http
  register: result

- name: Set the HTTP proxy configuration (again)
  vmware.vmware_rest.appliance_networking_proxy:
    enabled: true
    server: http://47.244.50.194
    port: 8081
    protocol: http
  register: result

- name: Delete the HTTP proxy configuration
  vmware.vmware_rest.appliance_networking_proxy:
    protocol: http
    state: absent
  register: result
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
