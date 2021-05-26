# vmware.vmware_rest.vcenter_vm_tools_installer

**Connects the VMware Tools CD installer as a CD-ROM for the guest
operating system**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Status

## Synopsis


* Connects the VMware Tools CD installer as a CD-ROM for the guest
operating system. On Windows guest operating systems with autorun,
this should cause the installer to initiate the Tools installation
which will need user input to complete. On other (non-Windows)
guest operating systems this will make the Tools installation
available, and a a user will need to do guest-specific actions.  On
Linux, this includes opening an archive and running the installer.
To monitor the status of the Tools install, clients should check
the mailto:[{@name](mailto:{@name) vcenter.vm.Tools.Info#versionStatus} and
mailto:[{@name](mailto:{@name) vcenter.vm.Tools.Info#runState} from mailto:[{@link](mailto:{@link)
vcenter.vm.Tools#get}

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
