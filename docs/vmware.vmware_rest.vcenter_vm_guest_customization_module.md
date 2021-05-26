# vmware.vmware_rest.vcenter_vm_guest_customization

**Applies a customization specification in {@param.name spec} on the
virtual machine in {@param.name vm}**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Status

## Synopsis


* Applies a customization specification in mailto:[{@param.name](mailto:{@param.name) spec}
on the virtual machine in mailto:[{@param.name](mailto:{@param.name) vm}. This
mailto:[{@term](mailto:{@term) operation} only sets the specification settings for
the virtual machine. The actual customization happens inside the
guest when the virtual machine is powered on. If
mailto:[{@param.name](mailto:{@param.name) spec} has mailto:[{@term](mailto:{@term) unset} values, then any
pending customization settings for the virtual machine are cleared.
If there is a pending customization for the virtual machine and
mailto:[{@param.name](mailto:{@param.name) spec} has valid content, then the existing
customization setting will be overwritten with the new settings.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
