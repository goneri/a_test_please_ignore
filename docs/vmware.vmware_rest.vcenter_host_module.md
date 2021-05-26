# vmware.vmware_rest.vcenter_host

**Add a new standalone host in the vCenter inventory**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Add a new standalone host in the vCenter inventory. The newly
connected host will be in connected state. The vCenter Server will
verify the SSL certificate before adding the host to its inventory.
In the case where the SSL certificate cannot be verified because
the Certificate Authority is not recognized or the certificate is
self signed, the vCenter Server will fall back to thumbprint
verification mode as defined by mailto:[{@link](mailto:{@link)
CreateSpec.ThumbprintVerification}.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Build a list of all the folders
  vmware.vmware_rest.vcenter_folder_info:
  register: my_folders

- name: Look up the different folders
  set_fact:
    my_host_folder: '{{ my_folders.value|selectattr("type", "equalto", "HOST")|first
      }}'

- name: Connect the host(s)
  vmware.vmware_rest.vcenter_host:
    hostname: "{{ lookup('env', 'ESXI1_HOSTNAME') }}"
    user_name: "{{ lookup('env', 'ESXI1_USERNAME') }}"
    password: "{{ lookup('env', 'ESXI1_PASSWORD') }}"
    thumbprint_verification: NONE
    folder: '{{ my_host_folder.folder }}'
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
