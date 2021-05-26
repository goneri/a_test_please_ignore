# vmware.vmware_rest.appliance_networking_noproxy

**Sets servers for which no proxy configuration should be applied**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Sets servers for which no proxy configuration should be applied.
This operation sets environment variables. In order for this
operation to take effect, a logout from appliance or a service
restart is required. If IPv4 is enabled, “127.0.0.1” and
“localhost” will always bypass the proxy (even if they are not
explicitly configured).

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Set HTTP noproxy configuration
  vmware.vmware_rest.appliance_networking_noproxy:
    servers:
    - redhat.com
    - ansible.com
  register: result

- name: Set HTTP noproxy configuration (again)
  vmware.vmware_rest.appliance_networking_noproxy:
    servers:
    - redhat.com
    - ansible.com
  register: result

- name: Remove the noproxy entries
  vmware.vmware_rest.appliance_networking_noproxy:
    servers: []
  register: result
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
