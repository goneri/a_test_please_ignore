# How to configure the vmware_rest collection

## Introduction

The vcenter_rest modules need to be authenticated. There are two
different

## Environment variables

Note: This solution requires that you call the modules from the local

    machine.

You need to export some environment variables in your shell before you
call the modules.

```
$ export VMWARE_HOST=vcenter.test
$ export VMWARE_USER=myvcenter-user
$ export VMWARE_password=mypassword
$ ansible-playbook my-playbook.yaml
```

## Module parameters

All the vcenter_rest modules accept the following arguments:


* `vcenter_host`


* `vcenter_username`


* `vcenter_password`

## Ignore SSL certificate error

It’s common to run a test environment without a proper SSL certificate
configuration.

To ignore the SSL error, you can use the `vcenter_validate_certs:
no` argument or `export VMWARE_VALIDATE_CERTS=no` to set the
environment variable.
# How to install the vmware_rest collection

## Requirements

The collection depends on:


* Ansible >=2.9.10 or greater


* Python 3.6 or greater

## aiohttp

[aiohttp](https://docs.aiohttp.org/en/stable/) is the only
dependency of the collection. You can install it with `pip` if you
use a virtualenv to run Ansible.

```
$ pip install aiohttp
```

Or using an RPM.

```
$ sudo dnf install python3-aiohttp
```

## Installation

The best option to install the collection is to use the
`ansible-galaxy` command:

```
$ ansible-galaxy collection install vmware.vmware_rest
```
# How to collect information about your environment

## Introduction

This section shows you how to utilize Ansible to collect information
about your environment. This information is useful for the other
tutorials.

## Scenario requirements

In this scenario we’ve got a vCenter with an ESXi host.

Our environment is pre-initialized with the following elements:


* A datacenter called `my_dc`


* A cluster called `my_cluser`


* A cluster called `my_cluser`


* An ESXi host called `esxi1` is in the cluster


* Two datastores on the ESXi: `rw_datastore` and `ro_datastore`


* A dvswitch based guest network

Finally, we use the environment variables to authenticate ourselves as
explained in [How to configure the vmware_rest collection](1_authentication.rst#vmware-rest-authentication).

## How to collect information

In these examples, we use the `vcenter_\*_info` module to collect
information about the associated resources.

All these modules return a `value` key. Depending on the context,
this `value` key will be either a list or a dictionary.

### Datacenter

Here we use the `vcenter_datacenter_info` module to list all the
datacenters. As expected, the `value` key of the output is a list.

```
- name: collect a list of the datacenters
  vmware.vmware_rest.vcenter_datacenter_info:
  register: my_datacenters
```

response

```
{
    "changed": false,
    "value": [
        {
            "datacenter": "datacenter-1416",
            "name": "my_dc"
        }
    ]
}
```

### Cluster

Here we do the same with `vcenter_cluster_info` module:

```
- name: Build a list of all the clusters
  vmware.vmware_rest.vcenter_cluster_info:
  register: all_the_clusters
```

response

```
{
    "changed": false,
    "value": [
        {
            "cluster": "domain-c1422",
            "drs_enabled": true,
            "ha_enabled": false,
            "name": "my_cluster"
        }
    ]
}
```

And we can also fetch the details about a specific cluster, with the
`cluster` parameter:

```
- name: Retrieve details about the first cluster
  vmware.vmware_rest.vcenter_cluster_info:
    cluster: "{{ all_the_clusters.value[0].cluster }}"
  register: my_cluster_info
```

response

```
{
    "changed": false,
    "id": "domain-c1422",
    "value": {
        "name": "my_cluster",
        "resource_pool": "resgroup-1423"
    }
}
```

And the `value` key of the output is this time a dictionary.

### Datastore

Here we use `vcenter_datastore_info` to get a list of all the
datastore called `rw_datastore`:

```
- name: Retrieve a list of all the datastores
  vmware.vmware_rest.vcenter_datastore_info:
    filter_names:
    - rw_datastore
  register: my_datastores
```

response

```
{
    "changed": false,
    "value": [
        {
            "capacity": 26831990784,
            "datastore": "datastore-1431",
            "free_space": 24406040576,
            "name": "rw_datastore",
            "type": "NFS"
        }
    ]
}
```

We save the first datastore in *my_datastore* fact for later use.

```
- name: Set my_datastore
  set_fact:
     my_datastore: '{{ my_datastores.value|first }}'
```

response

```
{
    "ansible_facts": {
        "my_datastore": {
            "capacity": 26831990784,
            "datastore": "datastore-1431",
            "free_space": 24406040576,
            "name": "rw_datastore",
            "type": "NFS"
        }
    },
    "changed": false
}
```

### Folder

And here again, you use the `vcenter_folder_info` module to retrieve
a list of all the folders.

```
- name: Build a list of all the folders
  vmware.vmware_rest.vcenter_folder_info:
  register: my_folders
```

response

```
{
    "changed": false,
    "value": [
        {
            "folder": "group-d1",
            "name": "Datacenters",
            "type": "DATACENTER"
        },
        {
            "folder": "group-h1418",
            "name": "host",
            "type": "HOST"
        },
        {
            "folder": "group-n1420",
            "name": "network",
            "type": "NETWORK"
        },
        {
            "folder": "group-s1419",
            "name": "datastore",
            "type": "DATASTORE"
        },
        {
            "folder": "group-v1417",
            "name": "vm",
            "type": "VIRTUAL_MACHINE"
        },
        {
            "folder": "group-v1426",
            "name": "Discovered virtual machine",
            "type": "VIRTUAL_MACHINE"
        },
        {
            "folder": "group-v1433",
            "name": "vCLS",
            "type": "VIRTUAL_MACHINE"
        }
    ]
}
```

Most of the time, you will just want one type of folder. In this case
we can use filters to reduce the amount to collect. Most of the
`_info` modules come with similar filters.

```
- name: Build a list of all the folders with the type VIRTUAL_MACHINE and called vm
  vmware.vmware_rest.vcenter_folder_info:
    filter_type: VIRTUAL_MACHINE
    filter_names:
      - vm
  register: my_folders
```

response

```
{
    "changed": false,
    "value": [
        {
            "folder": "group-v1417",
            "name": "vm",
            "type": "VIRTUAL_MACHINE"
        }
    ]
}
```

We register the first folder for later use with `set_fact`.

```
- name: Set my_virtual_machine_folder
  set_fact:
    my_virtual_machine_folder: '{{ my_folders.value|first }}'
```

response

```
{
    "ansible_facts": {
        "my_virtual_machine_folder": {
            "folder": "group-v1417",
            "name": "vm",
            "type": "VIRTUAL_MACHINE"
        }
    },
    "changed": false
}
```
# How to create a Virtual Machine


* Introduction


* Scenario requirements


* How to create a virtual machine

## Introduction

This section shows you how to use Ansible to create a virtual machine.

## Scenario requirements

You’ve already followed [How to collect information about your
environment](2_collect_information.rst#vmware-rest-collect-info) and
you’ve got the following variables defined:


* `my_cluster_info`


* `my_datastore`


* `my_virtual_machine_folder`


* `my_cluster_info`

## How to create a virtual machine

In this example, we will use the `vcenter_vm` module to create a new
guest.

```
- name: Create a VM
  vmware.vmware_rest.vcenter_vm:
    placement:
      cluster: "{{ my_cluster_info.id }}"
      datastore: "{{ my_datastore.datastore }}"
      folder: "{{ my_virtual_machine_folder.folder }}"
      resource_pool: "{{ my_cluster_info.value.resource_pool }}"
    name: test_vm1
    guest_OS: DEBIAN_8_64
    hardware_version: VMX_11
    memory:
      hot_add_enabled: true
      size_MiB: 1024
  register: _result
```

response

```
{
    "changed": true,
    "id": "vm-1439",
    "value": {
        "boot": {
            "delay": 0,
            "enter_setup_mode": false,
            "retry": false,
            "retry_delay": 10000,
            "type": "BIOS"
        },
        "boot_devices": [],
        "cdroms": {},
        "cpu": {
            "cores_per_socket": 1,
            "count": 1,
            "hot_add_enabled": false,
            "hot_remove_enabled": false
        },
        "disks": {
            "2000": {
                "backing": {
                    "type": "VMDK_FILE",
                    "vmdk_file": "[rw_datastore] test_vm1_12/test_vm1.vmdk"
                },
                "capacity": 17179869184,
                "label": "Hard disk 1",
                "scsi": {
                    "bus": 0,
                    "unit": 0
                },
                "type": "SCSI"
            }
        },
        "floppies": {},
        "guest_OS": "DEBIAN_8_64",
        "hardware": {
            "upgrade_policy": "NEVER",
            "upgrade_status": "NONE",
            "version": "VMX_11"
        },
        "identity": {
            "bios_uuid": "4226c316-6256-9306-68d4-e0b04b0e9a4e",
            "instance_uuid": "5026d333-4a51-eb05-6e8e-84ace3966ccf",
            "name": "test_vm1"
        },
        "instant_clone_frozen": false,
        "memory": {
            "hot_add_enabled": true,
            "size_MiB": 1024
        },
        "name": "test_vm1",
        "nics": {},
        "nvme_adapters": {},
        "parallel_ports": {},
        "power_state": "POWERED_OFF",
        "sata_adapters": {},
        "scsi_adapters": {
            "1000": {
                "label": "SCSI controller 0",
                "scsi": {
                    "bus": 0,
                    "unit": 7
                },
                "sharing": "NONE",
                "type": "PVSCSI"
            }
        },
        "serial_ports": {}
    }
}
```

Note: `vcenter_vm` accepts more parameters, however you may prefer to

    start with a simple VM and use the `vcenter_vm_hardware` modules
    to tune it up afterwards. It’s easier this way to identify a
    potential problematical step.
# Retrieve information from a specific VM


* Introduction


* Scenario requirements


* How to collect virtual machine information


    * List the VM


    * Collect the details about a specific VM


    * Get the hardware version of a specific VM


    * List the SCSI adapter(s) of a specific VM


    * List the CDROM drive(s) of a specific VM


    * Get the memory information of the VM


        * Get the storage policy of the VM


        * Get the disk information of the VM

## Introduction

This section shows you how to use Ansible to retrieve information
about a specific virtual machine.

## Scenario requirements

You’ve already followed [How to create a Virtual Machine](3_create_vm.rst#vmware-rest-create-vm) and you’ve got create a new
VM called `test_vm1`.

## How to collect virtual machine information

### List the VM

In this example, we use the `vcenter_vm_info` module to collect
information about our new VM.

In this example, we start by asking for a list of VMs. We use a filter
to limit the results to just the VM called `test_vm1`. So we are in
a list context, with one single entry in the `value` key.

```
- name: Look up the VM called test_vm1 in the inventory
  vmware.vmware_rest.vcenter_vm_info:
    filter_names:
      - test_vm1
  register: search_result
```

response

```
{
    "changed": false,
    "value": [
        {
            "cpu_count": 1,
            "memory_size_MiB": 1024,
            "name": "test_vm1",
            "power_state": "POWERED_OFF",
            "vm": "vm-1439"
        }
    ]
}
```

As expected, we get a list. And thanks to our filter, we just get one
entry.

### Collect the details about a specific VM

For the next steps, we pass the ID of the VM through the `vm`
parameter. This allow us to collect more details about this specific
VM.

```
- name: Collect information about a specific VM
  vmware.vmware_rest.vcenter_vm_info:
    vm: '{{ search_result.value[0].vm }}'
  register: test_vm1_info
```

response

```
{
    "changed": false,
    "id": "vm-1439",
    "value": {
        "boot": {
            "delay": 0,
            "enter_setup_mode": false,
            "retry": false,
            "retry_delay": 10000,
            "type": "BIOS"
        },
        "boot_devices": [],
        "cdroms": {},
        "cpu": {
            "cores_per_socket": 1,
            "count": 1,
            "hot_add_enabled": false,
            "hot_remove_enabled": false
        },
        "disks": {
            "2000": {
                "backing": {
                    "type": "VMDK_FILE",
                    "vmdk_file": "[rw_datastore] test_vm1_12/test_vm1.vmdk"
                },
                "capacity": 17179869184,
                "label": "Hard disk 1",
                "scsi": {
                    "bus": 0,
                    "unit": 0
                },
                "type": "SCSI"
            }
        },
        "floppies": {},
        "guest_OS": "DEBIAN_8_64",
        "hardware": {
            "upgrade_policy": "NEVER",
            "upgrade_status": "NONE",
            "version": "VMX_11"
        },
        "identity": {
            "bios_uuid": "4226c316-6256-9306-68d4-e0b04b0e9a4e",
            "instance_uuid": "5026d333-4a51-eb05-6e8e-84ace3966ccf",
            "name": "test_vm1"
        },
        "instant_clone_frozen": false,
        "memory": {
            "hot_add_enabled": true,
            "size_MiB": 1024
        },
        "name": "test_vm1",
        "nics": {},
        "nvme_adapters": {},
        "parallel_ports": {},
        "power_state": "POWERED_OFF",
        "sata_adapters": {},
        "scsi_adapters": {
            "1000": {
                "label": "SCSI controller 0",
                "scsi": {
                    "bus": 0,
                    "unit": 7
                },
                "sharing": "NONE",
                "type": "PVSCSI"
            }
        },
        "serial_ports": {}
    }
}
```

The result is a structure with all the details about our VM. You will
note this is actually the same information that we get when we created
the VM.

### Get the hardware version of a specific VM

We can also use all the `vcenter_vm_\*_info` modules to retrieve a
smaller amount of information. Here we use
`vcenter_vm_hardware_info` to know the hardware version of the VM.

```
- name: Collect the hardware information
  vmware.vmware_rest.vcenter_vm_hardware_info:
    vm: '{{ search_result.value[0].vm }}'
  register: my_vm1_hardware_info
```

response

```
{
    "changed": false,
    "value": {
        "upgrade_policy": "NEVER",
        "upgrade_status": "NONE",
        "version": "VMX_11"
    }
}
```

### List the SCSI adapter(s) of a specific VM

Here for instance, we list the SCSI adapter(s) of the VM:

```
- name: List the SCSI adapter of a given VM
  vmware.vmware_rest.vcenter_vm_hardware_adapter_scsi_info:
    vm: '{{ test_vm1_info.id }}'
  register: _result
```

response

```
{
    "changed": false,
    "value": [
        {
            "label": "SCSI controller 0",
            "scsi": {
                "bus": 0,
                "unit": 7
            },
            "sharing": "NONE",
            "type": "PVSCSI"
        }
    ]
}
```

You can do the same for the SATA controllers with
`vcenter_vm_adapter_sata_info`.

### List the CDROM drive(s) of a specific VM

And we list its CDROM drives.

```
- name: List the cdrom devices on the guest
  vmware.vmware_rest.vcenter_vm_hardware_cdrom_info:
    vm: '{{ test_vm1_info.id }}'
  register: _result
```

response

```
{
    "changed": false,
    "value": []
}
```

### Get the memory information of the VM

Here we collect the memory information of the VM:

```
- name: Retrieve the memory information from the VM
  vmware.vmware_rest.vcenter_vm_hardware_memory_info:
    vm: '{{ test_vm1_info.id }}'
  register: _result
```

response

```
{
    "changed": false,
    "value": {
        "hot_add_enabled": true,
        "size_MiB": 1024
    }
}
```

#### Get the storage policy of the VM

We use the `vcenter_vm_storage_policy_info` module for that:

```
- name: Get VM storage policy
  vmware.vmware_rest.vcenter_vm_storage_policy_info:
    vm: '{{ test_vm1_info.id }}'
  register: _result
```

response

```
{
    "changed": false,
    "value": {
        "disks": {}
    }
}
```

#### Get the disk information of the VM

We use the `vcenter_vm_hardware_disk_info` for this operation:

```
- name: Retrieve the disk information from the VM
  vmware.vmware_rest.vcenter_vm_hardware_disk_info:
    vm: '{{ test_vm1_info.id }}'
  register: _result
```

response

```
{
    "changed": false,
    "value": [
        {
            "backing": {
                "type": "VMDK_FILE",
                "vmdk_file": "[rw_datastore] test_vm1_12/test_vm1.vmdk"
            },
            "capacity": 17179869184,
            "label": "Hard disk 1",
            "scsi": {
                "bus": 0,
                "unit": 0
            },
            "type": "SCSI"
        }
    ]
}
```
# How to modify a virtual machine


* Introduction


* Scenario requirements


* How to add a CDROM drive to a virtual machine


    * Add a new SATA adapter


    * Add a CDROM drive


* How to attach a VM to a network


    * Attach a new NIC


    * Adjust the configuration of the NIC


* Increase the memory of the VM


* Upgrade the hardware version of the VM


* Adjust the number of CPUs of the VM


* Remove a SATA controller


* Attach a floppy drive


* Attach a new disk

## Introduction

This section shows you how to use Ansible to modify an existing
virtual machine.

## Scenario requirements

You’ve already followed [How to create a Virtual Machine](3_create_vm.rst#vmware-rest-create-vm) and created a VM.

## How to add a CDROM drive to a virtual machine

In this example, we use the `vcenter_vm_hardware_\*` modules to add a
new CDROM to an existing VM.

### Add a new SATA adapter

First we create a new SATA adapter. We specify the
`pci_slot_number`. This way if we run the task again it won’t do
anything if there is already an adapter there.

```
- name: Create a SATA adapter at PCI slot 34
  vmware.vmware_rest.vcenter_vm_hardware_adapter_sata:
    vm: '{{ test_vm1_info.id }}'
    pci_slot_number: 34
  register: _sata_adapter_result_1
```

response

```
{
    "changed": true,
    "id": "15000",
    "value": {
        "bus": 0,
        "label": "SATA controller 0",
        "pci_slot_number": 34,
        "type": "AHCI"
    }
}
```

### Add a CDROM drive

Now we can create the CDROM drive:

```
- name: Attach an ISO image to a guest VM
  vmware.vmware_rest.vcenter_vm_hardware_cdrom:
    vm: '{{ test_vm1_info.id }}'
    type: SATA
    sata:
      bus: 0
      unit: 2
    start_connected: true
    backing:
      iso_file: '[ro_datastore] fedora.iso'
      type: ISO_FILE
  register: _result
```

response

```
{
    "changed": true,
    "id": "16002",
    "value": {
        "allow_guest_control": false,
        "backing": {
            "iso_file": "[ro_datastore] fedora.iso",
            "type": "ISO_FILE"
        },
        "label": "CD/DVD drive 1",
        "sata": {
            "bus": 0,
            "unit": 2
        },
        "start_connected": true,
        "state": "NOT_CONNECTED",
        "type": "SATA"
    }
}
```

## How to attach a VM to a network

### Attach a new NIC

Here we attach the VM to the network (through the portgroup). We
specify a `pci_slot_number` for the same reason.

The second task adjusts the NIC configuration.

```
- name: Attach a VM to a dvswitch
  vmware.vmware_rest.vcenter_vm_hardware_ethernet:
    vm: '{{ test_vm1_info.id }}'
    pci_slot_number: 4
    backing:
      type: DISTRIBUTED_PORTGROUP
      network: "{{ my_portgroup_info.dvs_portgroup_info.dvswitch1[0].key }}"
    start_connected: false
  register: vm_hardware_ethernet_1
```

response

```
{
    "changed": true,
    "id": "4000",
    "value": {
        "allow_guest_control": false,
        "backing": {
            "connection_cookie": 2145785799,
            "distributed_port": "2",
            "distributed_switch_uuid": "50 26 86 58 79 40 4f fd-07 54 ae 4d 0c 79 36 e9",
            "network": "dvportgroup-1438",
            "type": "DISTRIBUTED_PORTGROUP"
        },
        "label": "Network adapter 1",
        "mac_address": "00:50:56:a6:e5:13",
        "mac_type": "ASSIGNED",
        "pci_slot_number": 4,
        "start_connected": false,
        "state": "NOT_CONNECTED",
        "type": "VMXNET3",
        "upt_compatibility_enabled": false,
        "wake_on_lan_enabled": false
    }
}
```

### Adjust the configuration of the NIC

```
- name: Turn the NIC's start_connected flag on
  vmware.vmware_rest.vcenter_vm_hardware_ethernet:
    nic: '{{ vm_hardware_ethernet_1.id }}'
    start_connected: true
    vm: '{{ test_vm1_info.id }}'
```

response

```
{
    "changed": true,
    "id": "4000",
    "value": {}
}
```

## Increase the memory of the VM

We can also adjust the amount of memory that we dedicate to our VM.

```
- name: Increase the memory of a VM
  vmware.vmware_rest.vcenter_vm_hardware_memory:
    vm: '{{ test_vm1_info.id }}'
    size_MiB: 1080
  register: _result
```

response

```
{
    "changed": true,
    "id": null,
    "value": {}
}
```

## Upgrade the hardware version of the VM

Here we use the `vcenter_vm_hardware` module to upgrade the version
of the hardware:

```
- name: Upgrade the VM hardware version
  vmware.vmware_rest.vcenter_vm_hardware:
    upgrade_policy: AFTER_CLEAN_SHUTDOWN
    upgrade_version: VMX_13
    vm: '{{ test_vm1_info.id }}'
  register: _result
```

response

```
{
    "changed": true,
    "id": null,
    "value": {}
}
```

## Adjust the number of CPUs of the VM

You can use `vcenter_vm_hardware_cpu` for that:

```
- name: Dedicate one core to the VM
  vmware.vmware_rest.vcenter_vm_hardware_cpu:
    vm: '{{ test_vm1_info.id }}'
    count: 1
  register: _result
```

response

```
{
    "changed": false,
    "id": null,
    "value": {
        "cores_per_socket": 1,
        "count": 1,
        "hot_add_enabled": false,
        "hot_remove_enabled": false
    }
}
```

## Remove a SATA controller

In this example, we remove the SATA controller of the PCI slot 34.

```
- name: Dedicate one core to the VM
  vmware.vmware_rest.vcenter_vm_hardware_cpu:
    vm: '{{ test_vm1_info.id }}'
    count: 1
  register: _result
```

response

```
{
    "changed": false,
    "id": null,
    "value": {
        "cores_per_socket": 1,
        "count": 1,
        "hot_add_enabled": false,
        "hot_remove_enabled": false
    }
}
```

## Attach a floppy drive

Here we attach a floppy drive to a VM.

```
- name: Add a floppy disk drive
  vmware.vmware_rest.vcenter_vm_hardware_floppy:
    vm: '{{ test_vm1_info.id }}'
    allow_guest_control: true
  register: my_floppy_drive
```

response

```
{
    "changed": true,
    "id": "8000",
    "value": {
        "allow_guest_control": true,
        "backing": {
            "auto_detect": true,
            "host_device": "",
            "type": "HOST_DEVICE"
        },
        "label": "Floppy drive 1",
        "start_connected": false,
        "state": "NOT_CONNECTED"
    }
}
```

## Attach a new disk

Here we attach a tiny disk to the VM. The `capacity` is in bytes.

```
- name: Create a new disk
  vmware.vmware_rest.vcenter_vm_hardware_disk:
    vm: '{{ test_vm1_info.id }}'
    type: SATA
    new_vmdk:
      capacity: 320000
  register: my_new_disk
```

response

```
{
    "changed": true,
    "id": "16000",
    "value": {
        "backing": {
            "type": "VMDK_FILE",
            "vmdk_file": "[rw_datastore] test_vm1_12/test_vm1_1.vmdk"
        },
        "capacity": 320000,
        "label": "Hard disk 2",
        "sata": {
            "bus": 0,
            "unit": 0
        },
        "type": "SATA"
    }
}
```
# How to run a virtual machine


* Introduction


* Power information


* How to start a virtual machine


* How to wait until my virtual machine is ready

## Introduction

This section covers the power management of your virtual machine.

## Power information

Use `vcenter_vm_power_info` to know the power state of the VM.

```
- name: Get guest power information
  vmware.vmware_rest.vcenter_vm_power_info:
    vm: '{{ test_vm1_info.id }}'
  register: _result
```

response

```
{
    "changed": false,
    "value": {
        "clean_power_off": true,
        "state": "POWERED_OFF"
    }
}
```

## How to start a virtual machine

Use the `vcenter_vm_power` module to start your VM:

```
- name: Turn the power of the VM on
  vmware.vmware_rest.vcenter_vm_power:
    state: start
    vm: '{{ test_vm1_info.id }}'
```

response

```
{
    "changed": false,
    "value": {}
}
```

## How to wait until my virtual machine is ready

If your virtual machine runs VMware Tools, you can build a loop around
the `center_vm_tools_info` module:

```
- name: Wait until my VM is ready
  vmware.vmware_rest.vcenter_vm_tools_info:
    vm: '{{ test_vm1_info.id }}'
  register: vm_tools_info
  until:
  - vm_tools_info is not failed
  - vm_tools_info.value.run_state == "RUNNING"
  retries: 60
  delay: 5
```

response

```
{
    "attempts": 7,
    "changed": false,
    "value": {
        "auto_update_supported": false,
        "install_attempt_count": 0,
        "install_type": "OPEN_VM_TOOLS",
        "run_state": "RUNNING",
        "upgrade_policy": "MANUAL",
        "version": "10346",
        "version_number": 10346,
        "version_status": "UNMANAGED"
    }
}
```
# How to get information from a running virtual machine


* Introduction


* Scenario requirements


* How to collect information


    * Filesystem


    * Guest identity


    * Network


    * Network interfaces


    * Network routes

## Introduction

This section shows you how to collection information from a running
virtual machine.

## Scenario requirements

You’ve already followed [How to run a virtual machine](6_run_a_vm.rst#vmware-rest-run-a-vm) and your virtual machine runs
VMware Tools.

## How to collect information

In this example, we use the `vcenter_vm_guest_\*` module to collect
information about the associated resources.

### Filesystem

Here we use `vcenter_vm_guest_localfilesystem_info` to retrieve the
details about the filesystem of the guest. In this example we also use
a `retries` loop. The VMware Tools may take a bit of time to start
and by doing so, we give the VM a bit more time.

```
- name: Get guest filesystem information
  vmware.vmware_rest.vcenter_vm_guest_localfilesystem_info:
    vm: '{{ test_vm1_info.id }}'
  register: _result
  until:
  - _result is not failed
  retries: 60
  delay: 5
```

response

```
{
    "attempts": 1,
    "changed": false,
    "value": {
        "error_type": "SERVICE_UNAVAILABLE",
        "messages": [
            {
                "args": [
                    "vm-1439:059dd233-dedf-4960-bba8-ab6710e6aeb4"
                ],
                "default_message": "VMware Tools in the virtual machine with identifier 'vm-1439:059dd233-dedf-4960-bba8-ab6710e6aeb4' provided no information.",
                "id": "com.vmware.api.vcenter.vm.guest.information_not_available"
            }
        ]
    }
}
```

### Guest identity

You can use `vcenter_vm_guest_identity_info` to get details like the
OS family or the hostname of the running VM.

```
- name: Get guest identity information
  vmware.vmware_rest.vcenter_vm_guest_identity_info:
    vm: '{{ test_vm1_info.id }}'
  register: _result
```

response

```
{
    "changed": false,
    "value": {
        "error_type": "SERVICE_UNAVAILABLE",
        "messages": [
            {
                "args": [
                    "vm-1439:059dd233-dedf-4960-bba8-ab6710e6aeb4"
                ],
                "default_message": "VMware Tools in the virtual machine with identifier 'vm-1439:059dd233-dedf-4960-bba8-ab6710e6aeb4' provided no information.",
                "id": "com.vmware.api.vcenter.vm.guest.information_not_available"
            }
        ]
    }
}
```

### Network

`vcenter_vm_guest_networking_info` will return the OS network
configuration.

```
- name: Get guest networking information
  vmware.vmware_rest.vcenter_vm_guest_networking_info:
    vm: '{{ test_vm1_info.id }}'
  register: _result
```

response

```
{
    "changed": false,
    "value": {}
}
```

### Network interfaces

`vcenter_vm_guest_networking_interfaces_info` will return a list of
NIC configurations.

See also [How to attach a VM to a network](5_vm_hardware_tuning.rst#vmware-rest-attach-a-network).

```
- name: Get guest network interfaces information
  vmware.vmware_rest.vcenter_vm_guest_networking_interfaces_info:
    vm: '{{ test_vm1_info.id }}'
  register: _result
```

response

```
{
    "changed": false,
    "value": []
}
```

### Network routes

Use `vcenter_vm_guest_networking_routes_info` to explore the route
table of your vitual machine.

```
- name: Get guest network routes information
  vmware.vmware_rest.vcenter_vm_guest_networking_routes_info:
    vm: '{{ test_vm1_info.id }}'
  register: _result
```

response

```
{
    "changed": false,
    "value": []
}
```
# How to configure the VMware tools of a running virtual machine


* Introduction


* Scenario requirements


* How to change the upgrade policy


    * Change the upgrade policy to MANUAL


    * Change the upgrade policy to UPGRADE_AT_POWER_CYCLE

## Introduction

This section show you how to collection information from a running
virtual machine.

## Scenario requirements

You’ve already followed [How to run a virtual machine](6_run_a_vm.rst#vmware-rest-run-a-vm) and your virtual machine runs
VMware Tools.

## How to change the upgrade policy

### Change the upgrade policy to MANUAL

You can adjust the VMware Tools upgrade policy with the
`vcenter_vm_tools` module.

```
- name: Change vm-tools upgrade policy to MANUAL
  vmware.vmware_rest.vcenter_vm_tools:
    vm: '{{ test_vm1_info.id }}'
    upgrade_policy: MANUAL
  register: _result
```

response

```
{
    "changed": false,
    "id": null,
    "value": {
        "auto_update_supported": false,
        "install_attempt_count": 0,
        "install_type": "OPEN_VM_TOOLS",
        "run_state": "RUNNING",
        "upgrade_policy": "MANUAL",
        "version": "10346",
        "version_number": 10346,
        "version_status": "UNMANAGED"
    }
}
```

### Change the upgrade policy to UPGRADE_AT_POWER_CYCLE

```
- name: Change vm-tools upgrade policy to UPGRADE_AT_POWER_CYCLE
  vmware.vmware_rest.vcenter_vm_tools:
    vm: '{{ test_vm1_info.id }}'
    upgrade_policy: UPGRADE_AT_POWER_CYCLE
  register: _result
```

response

```
{
    "changed": true,
    "id": null,
    "value": {}
}
```
