# vmware.vmware_rest.appliance_localaccounts

**Create a new local user account.**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Create a new local user account.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Add a local accounts
  vmware.vmware_rest.appliance_localaccounts:
    username: foobar
    config:
      full_name: Foobar
      email: foobar@vsphere.local
      password: Foobar56^$7
      roles:
      - operator
      - admin
      - superAdmin
    state: present
  register: result

- name: Add a local accounts (idempotency)
  vmware.vmware_rest.appliance_localaccounts:
    username: foobar
    config:
      full_name: Foobar
      email: foobar@vsphere.local
      # password: foobar
      roles:
      - operator
      - admin
      - superAdmin
    state: present
  register: result

- name: Change account email address
  vmware.vmware_rest.appliance_localaccounts:
    username: foobar
    config:
      full_name: Foobar
      email: foobar2@vsphere.local
      roles:
      - operator
      - admin
      - superAdmin
    state: present
  register: result

- name: Delete a local accounts
  vmware.vmware_rest.appliance_localaccounts:
    username: foobar
    state: absent
  register: result
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
