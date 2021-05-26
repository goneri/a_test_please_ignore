# vmware.vmware_rest.vcenter_vm_guest_filesystem

**Initiates an operation to transfer a file to or from the guest**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Status

## Synopsis


* Initiates an operation to transfer a file to or from the guest. <p>
If the power state of the Virtual Machine is changed when the file
transfer is in progress, or the Virtual Machine is migrated, then
the transfer operation is aborted. <p> When transferring a file
into the guest and overwriting an existing file, the old file
attributes are not preserved. <p> In order to ensure a secure
connection to the host when transferring a file using HTTPS, the
X.509 certificate for the host must be used to authenticate the
remote end of the connection. The certificate of the host that the
virtual machine is running on can be retrieved using the following
fields: XXX insert link to certificate in Host config XXX <p>

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
