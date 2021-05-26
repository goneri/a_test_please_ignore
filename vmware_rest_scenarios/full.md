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
# Configure the console and SSH access

## Introduction

This section show you how to manage the console and SSH access of the
vCenter Server Appliance (VCSA).

## Scenario requirements

You’ve got an up and running vCenter Server Appliance.

### Manage the shell access

Detect if the Shell is enabled.

```
- name: Check if the Shell is enabled
  vmware.vmware_rest.appliance_access_shell_info:
```

response

```
{
    "changed": false,
    "value": {
        "enabled": true,
        "timeout": 446
    }
}
```

Or turn on the Shell access with a timeout:

```
- name: Disable the Shell
  vmware.vmware_rest.appliance_access_shell:
    enabled: False
    timeout: 600
```

response

```
{
    "changed": true,
    "value": {}
}
```

### Manage the Direct Console User Interface (DCUI)

You can use [vmware.vmware_rest.appliance_access_dcui_info](../../docs/vmware.vmware_rest.appliance_access_dcui_info_module.rst#vmware-vmware-rest-appliance-access-dcui-info-module)
to get the current state of the configuration:

```
- name: Check if the Direct Console User Interface is enabled
  vmware.vmware_rest.appliance_access_dcui_info:
```

response

```
{
    "changed": false,
    "value": false
}
```

You can enable or disable the interface with appliance_access_dcui:

```
- name: Disable the Direct Console User Interface
  vmware.vmware_rest.appliance_access_dcui:
    enabled: False
```

response

```
{
    "changed": false,
    "value": false
}
```

### Manage the SSH interface

You can also get the status of the SSH interface with
appliance_access_ssh_info:

```
- name: Check is the SSH access is enabled
  vmware.vmware_rest.appliance_access_ssh_info:
```

response

```
{
    "changed": false,
    "value": true
}
```

And to enable the SSH interface:

```
- name: Ensure the SSH access ie enabled
  vmware.vmware_rest.appliance_access_ssh:
    enabled: true
```

response

```
{
    "changed": false,
    "value": true
}
```
# Get the health state of the VCSA components

## Introduction

The collection provides several modules that you can use to know the
state of the different components of the VCSA.

## Scenario requirements

You’ve got an up and running vCenter Server Appliance.

### Health state per component

The database:

```
- name: Get the database heath status
  vmware.vmware_rest.appliance_health_database_info:
```

response

```
{
    "changed": false,
    "value": {
        "messages": [
            {
                "message": {
                    "args": [],
                    "default_message": "DB state is Degraded",
                    "id": "desc"
                },
                "severity": "WARNING"
            }
        ],
        "status": "DEGRADED"
    }
}
```

The database storage:

```
- name: Get the database storage heath status
  vmware.vmware_rest.appliance_health_databasestorage_info:
```

response

```
{
    "changed": false,
    "value": "gray"
}
```

The system load:

```
- name: Get the system load status
  vmware.vmware_rest.appliance_health_load_info:
```

response

```
{
    "changed": false,
    "value": "gray"
}
```

The memory usage:

```
- name: Get the system mem status
  vmware.vmware_rest.appliance_health_mem_info:
```

response

```
{
    "changed": false,
    "value": "gray"
}
```

The system status:

```
- name: Get the system health status
  vmware.vmware_rest.appliance_health_system_info:
```

response

```
{
    "changed": false,
    "value": "gray"
}
```

The package manager:

```
- name: Get the health of the software package manager
  vmware.vmware_rest.appliance_health_softwarepackages_info:
```

response

```
{
    "changed": false,
    "value": "gray"
}
```

The storage system:

```
- name: Get the health of the storage system
  vmware.vmware_rest.appliance_health_storage_info:
```

response

```
{
    "changed": false,
    "value": "gray"
}
```

The swap usage:

```
- name: Get the health of the swap
  vmware.vmware_rest.appliance_health_swap_info:
```

response

```
{
    "changed": false,
    "value": "gray"
}
```

### Monitoring

You can also retrieve information from the VCSA monitoring backend.
First you need the name of the item. To get a full list of these
items, run:

```
- name: Get the list of the monitored items
  vmware.vmware_rest.appliance_monitoring_info:
  register: result
```

response

```
{
    "changed": false,
    "value": [
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.read.rate.dm-10",
            "id": "disk.read.rate.dm-10",
            "instance": "dm-10",
            "name": "com.vmware.applmgmt.mon.name.disk.read.rate.dm-10",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.read.rate.dm-11",
            "id": "disk.read.rate.dm-11",
            "instance": "dm-11",
            "name": "com.vmware.applmgmt.mon.name.disk.read.rate.dm-11",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.read.rate.dm-12",
            "id": "disk.read.rate.dm-12",
            "instance": "dm-12",
            "name": "com.vmware.applmgmt.mon.name.disk.read.rate.dm-12",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.read.rate.dm-13",
            "id": "disk.read.rate.dm-13",
            "instance": "dm-13",
            "name": "com.vmware.applmgmt.mon.name.disk.read.rate.dm-13",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.read.rate.dm-14",
            "id": "disk.read.rate.dm-14",
            "instance": "dm-14",
            "name": "com.vmware.applmgmt.mon.name.disk.read.rate.dm-14",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.read.rate.dm-2",
            "id": "disk.read.rate.dm-2",
            "instance": "dm-2",
            "name": "com.vmware.applmgmt.mon.name.disk.read.rate.dm-2",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.read.rate.dm-3",
            "id": "disk.read.rate.dm-3",
            "instance": "dm-3",
            "name": "com.vmware.applmgmt.mon.name.disk.read.rate.dm-3",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.read.rate.dm-4",
            "id": "disk.read.rate.dm-4",
            "instance": "dm-4",
            "name": "com.vmware.applmgmt.mon.name.disk.read.rate.dm-4",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.read.rate.dm-5",
            "id": "disk.read.rate.dm-5",
            "instance": "dm-5",
            "name": "com.vmware.applmgmt.mon.name.disk.read.rate.dm-5",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.read.rate.dm-6",
            "id": "disk.read.rate.dm-6",
            "instance": "dm-6",
            "name": "com.vmware.applmgmt.mon.name.disk.read.rate.dm-6",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.read.rate.dm-7",
            "id": "disk.read.rate.dm-7",
            "instance": "dm-7",
            "name": "com.vmware.applmgmt.mon.name.disk.read.rate.dm-7",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.read.rate.dm-8",
            "id": "disk.read.rate.dm-8",
            "instance": "dm-8",
            "name": "com.vmware.applmgmt.mon.name.disk.read.rate.dm-8",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.read.rate.dm-9",
            "id": "disk.read.rate.dm-9",
            "instance": "dm-9",
            "name": "com.vmware.applmgmt.mon.name.disk.read.rate.dm-9",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.write.rate.dm-10",
            "id": "disk.write.rate.dm-10",
            "instance": "dm-10",
            "name": "com.vmware.applmgmt.mon.name.disk.write.rate.dm-10",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.write.rate.dm-11",
            "id": "disk.write.rate.dm-11",
            "instance": "dm-11",
            "name": "com.vmware.applmgmt.mon.name.disk.write.rate.dm-11",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.write.rate.dm-12",
            "id": "disk.write.rate.dm-12",
            "instance": "dm-12",
            "name": "com.vmware.applmgmt.mon.name.disk.write.rate.dm-12",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.write.rate.dm-13",
            "id": "disk.write.rate.dm-13",
            "instance": "dm-13",
            "name": "com.vmware.applmgmt.mon.name.disk.write.rate.dm-13",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.write.rate.dm-14",
            "id": "disk.write.rate.dm-14",
            "instance": "dm-14",
            "name": "com.vmware.applmgmt.mon.name.disk.write.rate.dm-14",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.write.rate.dm-2",
            "id": "disk.write.rate.dm-2",
            "instance": "dm-2",
            "name": "com.vmware.applmgmt.mon.name.disk.write.rate.dm-2",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.write.rate.dm-3",
            "id": "disk.write.rate.dm-3",
            "instance": "dm-3",
            "name": "com.vmware.applmgmt.mon.name.disk.write.rate.dm-3",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.write.rate.dm-4",
            "id": "disk.write.rate.dm-4",
            "instance": "dm-4",
            "name": "com.vmware.applmgmt.mon.name.disk.write.rate.dm-4",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.write.rate.dm-5",
            "id": "disk.write.rate.dm-5",
            "instance": "dm-5",
            "name": "com.vmware.applmgmt.mon.name.disk.write.rate.dm-5",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.write.rate.dm-6",
            "id": "disk.write.rate.dm-6",
            "instance": "dm-6",
            "name": "com.vmware.applmgmt.mon.name.disk.write.rate.dm-6",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.write.rate.dm-7",
            "id": "disk.write.rate.dm-7",
            "instance": "dm-7",
            "name": "com.vmware.applmgmt.mon.name.disk.write.rate.dm-7",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.write.rate.dm-8",
            "id": "disk.write.rate.dm-8",
            "instance": "dm-8",
            "name": "com.vmware.applmgmt.mon.name.disk.write.rate.dm-8",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.write.rate.dm-9",
            "id": "disk.write.rate.dm-9",
            "instance": "dm-9",
            "name": "com.vmware.applmgmt.mon.name.disk.write.rate.dm-9",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.latency.rate.dm-10",
            "id": "disk.latency.rate.dm-10",
            "instance": "dm-10",
            "name": "com.vmware.applmgmt.mon.name.disk.latency.rate.dm-10",
            "units": "com.vmware.applmgmt.mon.unit.msec_per_io"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.latency.rate.dm-11",
            "id": "disk.latency.rate.dm-11",
            "instance": "dm-11",
            "name": "com.vmware.applmgmt.mon.name.disk.latency.rate.dm-11",
            "units": "com.vmware.applmgmt.mon.unit.msec_per_io"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.latency.rate.dm-12",
            "id": "disk.latency.rate.dm-12",
            "instance": "dm-12",
            "name": "com.vmware.applmgmt.mon.name.disk.latency.rate.dm-12",
            "units": "com.vmware.applmgmt.mon.unit.msec_per_io"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.latency.rate.dm-13",
            "id": "disk.latency.rate.dm-13",
            "instance": "dm-13",
            "name": "com.vmware.applmgmt.mon.name.disk.latency.rate.dm-13",
            "units": "com.vmware.applmgmt.mon.unit.msec_per_io"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.latency.rate.dm-14",
            "id": "disk.latency.rate.dm-14",
            "instance": "dm-14",
            "name": "com.vmware.applmgmt.mon.name.disk.latency.rate.dm-14",
            "units": "com.vmware.applmgmt.mon.unit.msec_per_io"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.latency.rate.dm-2",
            "id": "disk.latency.rate.dm-2",
            "instance": "dm-2",
            "name": "com.vmware.applmgmt.mon.name.disk.latency.rate.dm-2",
            "units": "com.vmware.applmgmt.mon.unit.msec_per_io"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.latency.rate.dm-3",
            "id": "disk.latency.rate.dm-3",
            "instance": "dm-3",
            "name": "com.vmware.applmgmt.mon.name.disk.latency.rate.dm-3",
            "units": "com.vmware.applmgmt.mon.unit.msec_per_io"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.latency.rate.dm-4",
            "id": "disk.latency.rate.dm-4",
            "instance": "dm-4",
            "name": "com.vmware.applmgmt.mon.name.disk.latency.rate.dm-4",
            "units": "com.vmware.applmgmt.mon.unit.msec_per_io"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.latency.rate.dm-5",
            "id": "disk.latency.rate.dm-5",
            "instance": "dm-5",
            "name": "com.vmware.applmgmt.mon.name.disk.latency.rate.dm-5",
            "units": "com.vmware.applmgmt.mon.unit.msec_per_io"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.latency.rate.dm-6",
            "id": "disk.latency.rate.dm-6",
            "instance": "dm-6",
            "name": "com.vmware.applmgmt.mon.name.disk.latency.rate.dm-6",
            "units": "com.vmware.applmgmt.mon.unit.msec_per_io"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.latency.rate.dm-7",
            "id": "disk.latency.rate.dm-7",
            "instance": "dm-7",
            "name": "com.vmware.applmgmt.mon.name.disk.latency.rate.dm-7",
            "units": "com.vmware.applmgmt.mon.unit.msec_per_io"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.latency.rate.dm-8",
            "id": "disk.latency.rate.dm-8",
            "instance": "dm-8",
            "name": "com.vmware.applmgmt.mon.name.disk.latency.rate.dm-8",
            "units": "com.vmware.applmgmt.mon.unit.msec_per_io"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.latency.rate.dm-9",
            "id": "disk.latency.rate.dm-9",
            "instance": "dm-9",
            "name": "com.vmware.applmgmt.mon.name.disk.latency.rate.dm-9",
            "units": "com.vmware.applmgmt.mon.unit.msec_per_io"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.swap.util",
            "id": "swap.util",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.swap.util",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.used.filesystem.swap",
            "id": "storage.used.filesystem.swap",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.storage.used.filesystem.swap",
            "units": "com.vmware.applmgmt.mon.unit.percent"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.swap",
            "id": "swap",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.swap",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.totalsize.filesystem.swap",
            "id": "storage.totalsize.filesystem.swap",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.storage.totalsize.filesystem.swap",
            "units": "com.vmware.applmgmt.mon.unit.percent"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.util.filesystem.swap",
            "id": "storage.util.filesystem.swap",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.storage.util.filesystem.swap",
            "units": "com.vmware.applmgmt.mon.unit.percent"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.cpu",
            "description": "com.vmware.applmgmt.mon.descr.cpu.totalfrequency",
            "id": "cpu.totalfrequency",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.cpu.totalfrequency",
            "units": "com.vmware.applmgmt.mon.unit.mhz"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.cpu",
            "description": "com.vmware.applmgmt.mon.descr.cpu.systemload",
            "id": "cpu.systemload",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.cpu.systemload",
            "units": "com.vmware.applmgmt.mon.unit.load_per_min"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.memory",
            "description": "com.vmware.applmgmt.mon.descr.mem.util",
            "id": "mem.util",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.mem.util",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.memory",
            "description": "com.vmware.applmgmt.mon.descr.mem.total",
            "id": "mem.total",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.mem.total",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.memory",
            "description": "com.vmware.applmgmt.mon.descr.mem.usage",
            "id": "mem.usage",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.mem.usage",
            "units": "com.vmware.applmgmt.mon.unit.percent"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.network",
            "description": "com.vmware.applmgmt.mon.descr.net.rx.error.eth0",
            "id": "net.rx.error.eth0",
            "instance": "eth0",
            "name": "com.vmware.applmgmt.mon.name.net.rx.error.eth0",
            "units": "com.vmware.applmgmt.mon.unit.errors_per_sample"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.network",
            "description": "com.vmware.applmgmt.mon.descr.net.rx.error.lo",
            "id": "net.rx.error.lo",
            "instance": "lo",
            "name": "com.vmware.applmgmt.mon.name.net.rx.error.lo",
            "units": "com.vmware.applmgmt.mon.unit.errors_per_sample"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.network",
            "description": "com.vmware.applmgmt.mon.descr.net.tx.error.eth0",
            "id": "net.tx.error.eth0",
            "instance": "eth0",
            "name": "com.vmware.applmgmt.mon.name.net.tx.error.eth0",
            "units": "com.vmware.applmgmt.mon.unit.errors_per_sample"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.network",
            "description": "com.vmware.applmgmt.mon.descr.net.tx.error.lo",
            "id": "net.tx.error.lo",
            "instance": "lo",
            "name": "com.vmware.applmgmt.mon.name.net.tx.error.lo",
            "units": "com.vmware.applmgmt.mon.unit.errors_per_sample"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.totalsize.directory.vcdb_hourly_stats",
            "id": "storage.totalsize.directory.vcdb_hourly_stats",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.storage.totalsize.directory.vcdb_hourly_stats",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.totalsize.directory.vcdb_daily_stats",
            "id": "storage.totalsize.directory.vcdb_daily_stats",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.storage.totalsize.directory.vcdb_daily_stats",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.totalsize.directory.vcdb_monthly_stats",
            "id": "storage.totalsize.directory.vcdb_monthly_stats",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.storage.totalsize.directory.vcdb_monthly_stats",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.totalsize.directory.vcdb_yearly_stats",
            "id": "storage.totalsize.directory.vcdb_yearly_stats",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.storage.totalsize.directory.vcdb_yearly_stats",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.totalsize.directory.vcdb_stats",
            "id": "storage.totalsize.directory.vcdb_stats",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.storage.totalsize.directory.vcdb_stats",
            "units": "com.vmware.applmgmt.mon.unit.percent"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.totalsize.filesystem.seat",
            "id": "storage.totalsize.filesystem.seat",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.storage.totalsize.filesystem.seat",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.util.directory.vcdb_stats",
            "id": "storage.util.directory.vcdb_stats",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.storage.util.directory.vcdb_stats",
            "units": "com.vmware.applmgmt.mon.unit.percent"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.totalsize.directory.vcdb_events",
            "id": "storage.totalsize.directory.vcdb_events",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.storage.totalsize.directory.vcdb_events",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.util.directory.vcdb_events",
            "id": "storage.util.directory.vcdb_events",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.storage.util.directory.vcdb_events",
            "units": "com.vmware.applmgmt.mon.unit.percent"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.totalsize.directory.vcdb_alarms",
            "id": "storage.totalsize.directory.vcdb_alarms",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.storage.totalsize.directory.vcdb_alarms",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.util.directory.vcdb_alarms",
            "id": "storage.util.directory.vcdb_alarms",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.storage.util.directory.vcdb_alarms",
            "units": "com.vmware.applmgmt.mon.unit.percent"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.totalsize.directory.vcdb_tasks",
            "id": "storage.totalsize.directory.vcdb_tasks",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.storage.totalsize.directory.vcdb_tasks",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.util.directory.vcdb_tasks",
            "id": "storage.util.directory.vcdb_tasks",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.storage.util.directory.vcdb_tasks",
            "units": "com.vmware.applmgmt.mon.unit.percent"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.used.filesystem.seat",
            "id": "storage.used.filesystem.seat",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.storage.used.filesystem.seat",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.util.filesystem.seat",
            "id": "storage.util.filesystem.seat",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.storage.util.filesystem.seat",
            "units": "com.vmware.applmgmt.mon.unit.percent"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.used.filesystem.db",
            "id": "storage.used.filesystem.db",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.storage.used.filesystem.db",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.totalsize.filesystem.db",
            "id": "storage.totalsize.filesystem.db",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.storage.totalsize.filesystem.db",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.util.filesystem.db",
            "id": "storage.util.filesystem.db",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.storage.util.filesystem.db",
            "units": "com.vmware.applmgmt.mon.unit.percent"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.used.filesystem.dblog",
            "id": "storage.used.filesystem.dblog",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.storage.used.filesystem.dblog",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.totalsize.filesystem.dblog",
            "id": "storage.totalsize.filesystem.dblog",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.storage.totalsize.filesystem.dblog",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.util.filesystem.dblog",
            "id": "storage.util.filesystem.dblog",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.storage.util.filesystem.dblog",
            "units": "com.vmware.applmgmt.mon.unit.percent"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.totalsize.filesystem.root",
            "id": "storage.totalsize.filesystem.root",
            "instance": "/",
            "name": "com.vmware.applmgmt.mon.name.storage.totalsize.filesystem.root",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.util.filesystem.root",
            "id": "storage.util.filesystem.root",
            "instance": "/",
            "name": "com.vmware.applmgmt.mon.name.storage.util.filesystem.root",
            "units": "com.vmware.applmgmt.mon.unit.percent"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.totalsize.filesystem.boot",
            "id": "storage.totalsize.filesystem.boot",
            "instance": "/boot",
            "name": "com.vmware.applmgmt.mon.name.storage.totalsize.filesystem.boot",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.util.filesystem.boot",
            "id": "storage.util.filesystem.boot",
            "instance": "/boot",
            "name": "com.vmware.applmgmt.mon.name.storage.util.filesystem.boot",
            "units": "com.vmware.applmgmt.mon.unit.percent"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.totalsize.filesystem.archive",
            "id": "storage.totalsize.filesystem.archive",
            "instance": "/storage/archive",
            "name": "com.vmware.applmgmt.mon.name.storage.totalsize.filesystem.archive",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.util.filesystem.archive",
            "id": "storage.util.filesystem.archive",
            "instance": "/storage/archive",
            "name": "com.vmware.applmgmt.mon.name.storage.util.filesystem.archive",
            "units": "com.vmware.applmgmt.mon.unit.percent"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.totalsize.filesystem.autodeploy",
            "id": "storage.totalsize.filesystem.autodeploy",
            "instance": "/storage/autodeploy",
            "name": "com.vmware.applmgmt.mon.name.storage.totalsize.filesystem.autodeploy",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.util.filesystem.autodeploy",
            "id": "storage.util.filesystem.autodeploy",
            "instance": "/storage/autodeploy",
            "name": "com.vmware.applmgmt.mon.name.storage.util.filesystem.autodeploy",
            "units": "com.vmware.applmgmt.mon.unit.percent"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.totalsize.filesystem.core",
            "id": "storage.totalsize.filesystem.core",
            "instance": "/storage/core",
            "name": "com.vmware.applmgmt.mon.name.storage.totalsize.filesystem.core",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.util.filesystem.core",
            "id": "storage.util.filesystem.core",
            "instance": "/storage/core",
            "name": "com.vmware.applmgmt.mon.name.storage.util.filesystem.core",
            "units": "com.vmware.applmgmt.mon.unit.percent"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.totalsize.filesystem.imagebuilder",
            "id": "storage.totalsize.filesystem.imagebuilder",
            "instance": "/storage/imagebuilder",
            "name": "com.vmware.applmgmt.mon.name.storage.totalsize.filesystem.imagebuilder",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.util.filesystem.imagebuilder",
            "id": "storage.util.filesystem.imagebuilder",
            "instance": "/storage/imagebuilder",
            "name": "com.vmware.applmgmt.mon.name.storage.util.filesystem.imagebuilder",
            "units": "com.vmware.applmgmt.mon.unit.percent"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.totalsize.filesystem.lifecycle",
            "id": "storage.totalsize.filesystem.lifecycle",
            "instance": "/storage/lifecycle",
            "name": "com.vmware.applmgmt.mon.name.storage.totalsize.filesystem.lifecycle",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.util.filesystem.lifecycle",
            "id": "storage.util.filesystem.lifecycle",
            "instance": "/storage/lifecycle",
            "name": "com.vmware.applmgmt.mon.name.storage.util.filesystem.lifecycle",
            "units": "com.vmware.applmgmt.mon.unit.percent"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.totalsize.filesystem.log",
            "id": "storage.totalsize.filesystem.log",
            "instance": "/storage/log",
            "name": "com.vmware.applmgmt.mon.name.storage.totalsize.filesystem.log",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.util.filesystem.log",
            "id": "storage.util.filesystem.log",
            "instance": "/storage/log",
            "name": "com.vmware.applmgmt.mon.name.storage.util.filesystem.log",
            "units": "com.vmware.applmgmt.mon.unit.percent"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.totalsize.filesystem.netdump",
            "id": "storage.totalsize.filesystem.netdump",
            "instance": "/storage/netdump",
            "name": "com.vmware.applmgmt.mon.name.storage.totalsize.filesystem.netdump",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.util.filesystem.netdump",
            "id": "storage.util.filesystem.netdump",
            "instance": "/storage/netdump",
            "name": "com.vmware.applmgmt.mon.name.storage.util.filesystem.netdump",
            "units": "com.vmware.applmgmt.mon.unit.percent"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.totalsize.filesystem.updatemgr",
            "id": "storage.totalsize.filesystem.updatemgr",
            "instance": "/storage/updatemgr",
            "name": "com.vmware.applmgmt.mon.name.storage.totalsize.filesystem.updatemgr",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.util.filesystem.updatemgr",
            "id": "storage.util.filesystem.updatemgr",
            "instance": "/storage/updatemgr",
            "name": "com.vmware.applmgmt.mon.name.storage.util.filesystem.updatemgr",
            "units": "com.vmware.applmgmt.mon.unit.percent"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.totalsize.filesystem.vtsdb",
            "id": "storage.totalsize.filesystem.vtsdb",
            "instance": "/storage/vtsdb",
            "name": "com.vmware.applmgmt.mon.name.storage.totalsize.filesystem.vtsdb",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.util.filesystem.vtsdb",
            "id": "storage.util.filesystem.vtsdb",
            "instance": "/storage/vtsdb",
            "name": "com.vmware.applmgmt.mon.name.storage.util.filesystem.vtsdb",
            "units": "com.vmware.applmgmt.mon.unit.percent"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.totalsize.filesystem.vtsdblog",
            "id": "storage.totalsize.filesystem.vtsdblog",
            "instance": "/storage/vtsdblog",
            "name": "com.vmware.applmgmt.mon.name.storage.totalsize.filesystem.vtsdblog",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.util.filesystem.vtsdblog",
            "id": "storage.util.filesystem.vtsdblog",
            "instance": "/storage/vtsdblog",
            "name": "com.vmware.applmgmt.mon.name.storage.util.filesystem.vtsdblog",
            "units": "com.vmware.applmgmt.mon.unit.percent"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.used.filesystem.root",
            "id": "storage.used.filesystem.root",
            "instance": "/",
            "name": "com.vmware.applmgmt.mon.name.storage.used.filesystem.root",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.used.filesystem.boot",
            "id": "storage.used.filesystem.boot",
            "instance": "/boot",
            "name": "com.vmware.applmgmt.mon.name.storage.used.filesystem.boot",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.used.filesystem.archive",
            "id": "storage.used.filesystem.archive",
            "instance": "/storage/archive",
            "name": "com.vmware.applmgmt.mon.name.storage.used.filesystem.archive",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.used.filesystem.autodeploy",
            "id": "storage.used.filesystem.autodeploy",
            "instance": "/storage/autodeploy",
            "name": "com.vmware.applmgmt.mon.name.storage.used.filesystem.autodeploy",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.used.filesystem.core",
            "id": "storage.used.filesystem.core",
            "instance": "/storage/core",
            "name": "com.vmware.applmgmt.mon.name.storage.used.filesystem.core",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.used.filesystem.imagebuilder",
            "id": "storage.used.filesystem.imagebuilder",
            "instance": "/storage/imagebuilder",
            "name": "com.vmware.applmgmt.mon.name.storage.used.filesystem.imagebuilder",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.used.filesystem.lifecycle",
            "id": "storage.used.filesystem.lifecycle",
            "instance": "/storage/lifecycle",
            "name": "com.vmware.applmgmt.mon.name.storage.used.filesystem.lifecycle",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.used.filesystem.log",
            "id": "storage.used.filesystem.log",
            "instance": "/storage/log",
            "name": "com.vmware.applmgmt.mon.name.storage.used.filesystem.log",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.used.filesystem.netdump",
            "id": "storage.used.filesystem.netdump",
            "instance": "/storage/netdump",
            "name": "com.vmware.applmgmt.mon.name.storage.used.filesystem.netdump",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.used.filesystem.updatemgr",
            "id": "storage.used.filesystem.updatemgr",
            "instance": "/storage/updatemgr",
            "name": "com.vmware.applmgmt.mon.name.storage.used.filesystem.updatemgr",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.used.filesystem.vtsdb",
            "id": "storage.used.filesystem.vtsdb",
            "instance": "/storage/vtsdb",
            "name": "com.vmware.applmgmt.mon.name.storage.used.filesystem.vtsdb",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.storage",
            "description": "com.vmware.applmgmt.mon.descr.storage.used.filesystem.vtsdblog",
            "id": "storage.used.filesystem.vtsdblog",
            "instance": "/storage/vtsdblog",
            "name": "com.vmware.applmgmt.mon.name.storage.used.filesystem.vtsdblog",
            "units": "com.vmware.applmgmt.mon.unit.kb"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.cpu",
            "description": "com.vmware.applmgmt.mon.descr.cpu.util",
            "id": "cpu.util",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.cpu.util",
            "units": "com.vmware.applmgmt.mon.unit.percent"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.cpu",
            "description": "com.vmware.applmgmt.mon.descr.cpu.steal",
            "id": "cpu.steal",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.cpu.steal",
            "units": "com.vmware.applmgmt.mon.unit.percent"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.memory",
            "description": "com.vmware.applmgmt.mon.descr.swap.pageRate",
            "id": "swap.pageRate",
            "instance": "",
            "name": "com.vmware.applmgmt.mon.name.swap.pageRate",
            "units": "com.vmware.applmgmt.mon.unit.pages_per_sec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.read.rate.dm-0",
            "id": "disk.read.rate.dm-0",
            "instance": "dm-0",
            "name": "com.vmware.applmgmt.mon.name.disk.read.rate.dm-0",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.read.rate.dm-1",
            "id": "disk.read.rate.dm-1",
            "instance": "dm-1",
            "name": "com.vmware.applmgmt.mon.name.disk.read.rate.dm-1",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.write.rate.dm-0",
            "id": "disk.write.rate.dm-0",
            "instance": "dm-0",
            "name": "com.vmware.applmgmt.mon.name.disk.write.rate.dm-0",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.write.rate.dm-1",
            "id": "disk.write.rate.dm-1",
            "instance": "dm-1",
            "name": "com.vmware.applmgmt.mon.name.disk.write.rate.dm-1",
            "units": "com.vmware.applmgmt.mon.unit.num_of_io_per_msec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.latency.rate.dm-0",
            "id": "disk.latency.rate.dm-0",
            "instance": "dm-0",
            "name": "com.vmware.applmgmt.mon.name.disk.latency.rate.dm-0",
            "units": "com.vmware.applmgmt.mon.unit.msec_per_io"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.disk",
            "description": "com.vmware.applmgmt.mon.descr.disk.latency.rate.dm-1",
            "id": "disk.latency.rate.dm-1",
            "instance": "dm-1",
            "name": "com.vmware.applmgmt.mon.name.disk.latency.rate.dm-1",
            "units": "com.vmware.applmgmt.mon.unit.msec_per_io"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.network",
            "description": "com.vmware.applmgmt.mon.descr.net.rx.activity.eth0",
            "id": "net.rx.activity.eth0",
            "instance": "eth0",
            "name": "com.vmware.applmgmt.mon.name.net.rx.activity.eth0",
            "units": "com.vmware.applmgmt.mon.unit.kb_per_sec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.network",
            "description": "com.vmware.applmgmt.mon.descr.net.rx.activity.lo",
            "id": "net.rx.activity.lo",
            "instance": "lo",
            "name": "com.vmware.applmgmt.mon.name.net.rx.activity.lo",
            "units": "com.vmware.applmgmt.mon.unit.kb_per_sec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.network",
            "description": "com.vmware.applmgmt.mon.descr.net.rx.packetRate.eth0",
            "id": "net.rx.packetRate.eth0",
            "instance": "eth0",
            "name": "com.vmware.applmgmt.mon.name.net.rx.packetRate.eth0",
            "units": "com.vmware.applmgmt.mon.unit.packets_per_sec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.network",
            "description": "com.vmware.applmgmt.mon.descr.net.rx.packetRate.lo",
            "id": "net.rx.packetRate.lo",
            "instance": "lo",
            "name": "com.vmware.applmgmt.mon.name.net.rx.packetRate.lo",
            "units": "com.vmware.applmgmt.mon.unit.packets_per_sec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.network",
            "description": "com.vmware.applmgmt.mon.descr.net.rx.drop.eth0",
            "id": "net.rx.drop.eth0",
            "instance": "eth0",
            "name": "com.vmware.applmgmt.mon.name.net.rx.drop.eth0",
            "units": "com.vmware.applmgmt.mon.unit.drops_per_sample"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.network",
            "description": "com.vmware.applmgmt.mon.descr.net.rx.drop.lo",
            "id": "net.rx.drop.lo",
            "instance": "lo",
            "name": "com.vmware.applmgmt.mon.name.net.rx.drop.lo",
            "units": "com.vmware.applmgmt.mon.unit.drops_per_sample"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.network",
            "description": "com.vmware.applmgmt.mon.descr.net.tx.activity.eth0",
            "id": "net.tx.activity.eth0",
            "instance": "eth0",
            "name": "com.vmware.applmgmt.mon.name.net.tx.activity.eth0",
            "units": "com.vmware.applmgmt.mon.unit.kb_per_sec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.network",
            "description": "com.vmware.applmgmt.mon.descr.net.tx.activity.lo",
            "id": "net.tx.activity.lo",
            "instance": "lo",
            "name": "com.vmware.applmgmt.mon.name.net.tx.activity.lo",
            "units": "com.vmware.applmgmt.mon.unit.kb_per_sec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.network",
            "description": "com.vmware.applmgmt.mon.descr.net.tx.packetRate.eth0",
            "id": "net.tx.packetRate.eth0",
            "instance": "eth0",
            "name": "com.vmware.applmgmt.mon.name.net.tx.packetRate.eth0",
            "units": "com.vmware.applmgmt.mon.unit.packets_per_sec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.network",
            "description": "com.vmware.applmgmt.mon.descr.net.tx.packetRate.lo",
            "id": "net.tx.packetRate.lo",
            "instance": "lo",
            "name": "com.vmware.applmgmt.mon.name.net.tx.packetRate.lo",
            "units": "com.vmware.applmgmt.mon.unit.packets_per_sec"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.network",
            "description": "com.vmware.applmgmt.mon.descr.net.tx.drop.eth0",
            "id": "net.tx.drop.eth0",
            "instance": "eth0",
            "name": "com.vmware.applmgmt.mon.name.net.tx.drop.eth0",
            "units": "com.vmware.applmgmt.mon.unit.drops_per_sample"
        },
        {
            "category": "com.vmware.applmgmt.mon.cat.network",
            "description": "com.vmware.applmgmt.mon.descr.net.tx.drop.lo",
            "id": "net.tx.drop.lo",
            "instance": "lo",
            "name": "com.vmware.applmgmt.mon.name.net.tx.drop.lo",
            "units": "com.vmware.applmgmt.mon.unit.drops_per_sample"
        }
    ]
}
```

With this information, you can access the information for a given time
frame:

```
- name: Query the monitoring backend
  vmware.vmware_rest.appliance_monitoring_query:
    end_time: 2021-04-14T09:34:56.000Z
    start_time: 2021-04-14T08:34:56.000Z
    names:
      - mem.total
    interval: MINUTES5
    function: AVG
  register: result
```

response

```
{
    "changed": false,
    "value": [
        {
            "data": [
                "",
                "",
                "",
                "",
                "",
                "",
                "",
                "",
                "",
                "",
                "",
                "",
                ""
            ],
            "end_time": "2021-04-14T09:34:56.000Z",
            "function": "AVG",
            "interval": "MINUTES5",
            "name": "mem.total",
            "start_time": "2021-04-14T08:34:56.000Z"
        }
    ]
}
```
# Network managment

## IP configuration

You can also use Ansible to get and configure the network stack of the
VCSA.

### Global network information

The appliance_networking_info exposes the state of the global network
configuration:

```
- name: Get network information
  vmware.vmware_rest.appliance_networking_info:
```

response

```
{
    "changed": false,
    "value": {
        "dns": {
            "hostname": "vcenter.test",
            "mode": "DHCP",
            "servers": [
                "192.168.123.1"
            ]
        },
        "interfaces": {
            "nic0": {
                "ipv4": {
                    "address": "192.168.123.8",
                    "configurable": true,
                    "default_gateway": "192.168.123.1",
                    "mode": "DHCP",
                    "prefix": 24
                },
                "mac": "52:54:00:05:a4:c8",
                "name": "nic0",
                "status": "up"
            }
        },
        "vcenter_base_url": "https://vcenter.test:443"
    }
}
```

And you can adjust the parameters with the appliance_networking
module.

```
- name: Set network information
  vmware.vmware_rest.appliance_networking:
    ipv6_enabled: False
```

response

```
{
    "changed": true,
    "id": null,
    "value": {}
}
```

### Network Interface configuration

The appliance_networking_interfaces_info returns a list of the Network
Interface of the system:

```
- name: Get a list of the network interfaces
  vmware.vmware_rest.appliance_networking_interfaces_info:
```

response

```
{
    "changed": false,
    "value": [
        {
            "ipv4": {
                "address": "192.168.123.8",
                "configurable": true,
                "default_gateway": "192.168.123.1",
                "mode": "DHCP",
                "prefix": 24
            },
            "mac": "52:54:00:05:a4:c8",
            "name": "nic0",
            "status": "up"
        }
    ]
}
```

You can also use the `interface_name` parameter to just focus on one
single entry:

```
- name: Get details about one network interfaces
  vmware.vmware_rest.appliance_networking_interfaces_info:
    interface_name: nic0
```

response

```
{
    "changed": false,
    "id": "nic0",
    "value": {
        "ipv4": {
            "address": "192.168.123.8",
            "configurable": true,
            "default_gateway": "192.168.123.1",
            "mode": "DHCP",
            "prefix": 24
        },
        "mac": "52:54:00:05:a4:c8",
        "name": "nic0",
        "status": "up"
    }
}
```

You can adjust the IPv4 network configuration of a NIC with with
appliance_networking_interfaces_ipv4:

```
- name: Enforce the use of DHCP for nic0
  vmware.vmware_rest.appliance_networking_interfaces_ipv4:
    interface_name: nic0
    mode: DHCP
```

response

```
{
    "changed": false,
    "value": {
        "address": "192.168.123.8",
        "configurable": true,
        "default_gateway": "192.168.123.1",
        "mode": "DHCP",
        "prefix": 24
    }
}
```

The appliance_networking_interfaces_ipv6 and
appliance_networking_interfaces_ipv6_info allow you do the same with
IPv6.

## DNS configuration

### The hostname configuration

The appliance_networking_dns_hostname_info module can be use to
retrieve the hostname of the VCSA:

```
- name: Get the hostname configuration
  vmware.vmware_rest.appliance_networking_dns_hostname_info:
```

response

```
{
    "changed": false,
    "value": "vcenter.test"
}
```

### The DNS servers

Use the appliance_networking_dns_servers_info to get DNS servers
currently in use:

```
- name: Get the DNS servers
  vmware.vmware_rest.appliance_networking_dns_servers_info:
  ignore_errors: True  # May be failing because of the CI set-up
```

response

```
{
    "changed": false,
    "value": {
        "mode": "dhcp",
        "servers": [
            "192.168.123.1"
        ]
    }
}
```

The appliance_networking_dns_servers can be used to set a different
name server.

```
- name: Set the DNS servers
  vmware.vmware_rest.appliance_networking_dns_servers:
    servers:
      - 192.168.123.1
```

response

```
{
    "changed": false,
    "value": {
        "mode": "dhcp",
        "servers": [
            "192.168.123.1"
        ]
    }
}
```

You can test a list of servers if you set `state=test`:

```
- name: Test the DNS servers
  vmware.vmware_rest.appliance_networking_dns_servers:
    state: test
    servers:
      - var
```

response

```
{
    "changed": false,
    "value": {
        "messages": [
            {
                "message": "Failed to reach 'var'.",
                "result": "failure"
            }
        ],
        "status": "red"
    }
}
```

### The search domain configuration

The search domain configuration can be done with
appliance_networking_dns_domains and
appliance_networking_dns_domains_info. The second module returns a
list of domains:

```
- name: Get DNS domains configuration
  vmware.vmware_rest.appliance_networking_dns_domains_info:
```

response

```
{
    "changed": false,
    "value": [
        "foobar",
        "barfoo"
    ]
}
```

There is two way to set the search domain. By default the value you
pass in `domains` will overwrite the existing domain:

```
- name: Update the domain configuration
  vmware.vmware_rest.appliance_networking_dns_domains:
    domains:
      - foobar
```

response

```
{
    "changed": true,
    "value": {}
}
```

If you instead use the `state=add` parameter, the `domain` value
will complet the existing list of domains.

```
- name: Add another domain configuration
  vmware.vmware_rest.appliance_networking_dns_domains:
    domain: barfoo
    state: add
```

response

```
{
    "changed": false,
    "value": {}
}
```

## Firewall settings

You can also configure the VCSA firewall. You can add new ruleset with
the appliance_networking_firewall_inbound module. In this example, we
reject all the traffic coming from the `1.2.3.0/24` subnet:

```
- name: Set a firewall rule
  vmware.vmware_rest.appliance_networking_firewall_inbound:
    rules:
      - address: 1.2.3.0
        prefix: 24
        policy: REJECT
```

response

```
{
    "changed": true,
    "value": {}
}
```

The appliance_networking_firewall_inbound_info module returns a list
of the inbound ruleset:

```
- name: Get the firewall inbound configuration
  vmware.vmware_rest.appliance_networking_firewall_inbound_info:
```

response

```
{
    "changed": false,
    "value": [
        {
            "address": "1.2.3.0",
            "interface_name": "*",
            "policy": "REJECT",
            "prefix": 24
        }
    ]
}
```

## HTTP proxy

You can also configurre the VCSA to go through a HTTP proxy. The
collection provides a set of modules to configure the proxy server and
manage the noproxy filter.

In this example, we will set up a proxy and configure the `noproxy`
for `redhat.com` and `ansible.com`:

```
- name: Set the HTTP proxy configuration
  vmware.vmware_rest.appliance_networking_proxy:
    enabled: true
    server: http://47.244.50.194
    port: 8081
    protocol: http
- name: Set HTTP noproxy configuration
  vmware.vmware_rest.appliance_networking_noproxy:
    servers:
      - redhat.com
      - ansible.com
```

response

```
{
    "changed": false,
    "value": {
        "enabled": false,
        "port": -1,
        "server": ""
    }
}
```

```
{
    "changed": true,
    "value": {}
}
```

We can validate the configuration with the associated _info modules:

```
- name: Get the HTTP proxy configuration
  vmware.vmware_rest.appliance_networking_proxy_info:
- name: Get HTTP noproxy configuration
  vmware.vmware_rest.appliance_networking_noproxy_info:
```

response

```
{
    "changed": false,
    "value": {
        "ftp": {
            "enabled": false,
            "port": -1,
            "server": ""
        },
        "http": {
            "enabled": false,
            "port": -1,
            "server": ""
        },
        "https": {
            "enabled": false,
            "port": -1,
            "server": ""
        }
    }
}
```

```
{
    "changed": false,
    "value": [
        "redhat.com",
        "ansible.com",
        "localhost",
        "127.0.0.1"
    ]
}
```

And we finally reverse the configuration:

```
- name: Delete the HTTP proxy configuration
  vmware.vmware_rest.appliance_networking_proxy:
    config: {}
    protocol: http
    state: absent
- name: Remove the noproxy entries
  vmware.vmware_rest.appliance_networking_noproxy:
    servers: []
```

response

```
{
    "changed": true,
    "value": {}
}
```

```
{
    "changed": true,
    "value": {}
}
```
# Services managment

## Handle your VCSA services with Ansible

You can use Ansible to control the VCSA services. To get a view of all
the known services, you can use the appliance_services_info module:

```
- name: List all the services
  vmware.vmware_rest.appliance_services_info:
```

response

```
{
    "changed": false,
    "value": {
        "appliance-shutdown": {
            "description": "/etc/rc.local.shutdown Compatibility",
            "state": "STOPPED"
        },
        "atftpd": {
            "description": "The tftp server serves files using the trivial file transfer protocol.",
            "state": "STOPPED"
        },
        "auditd": {
            "description": "Security Auditing Service",
            "state": "STOPPED"
        },
        "cloud-config": {
            "description": "Apply the settings specified in cloud-config",
            "state": "STARTED"
        },
        "cloud-init": {
            "description": "Initial cloud-init job (metadata service crawler)",
            "state": "STARTED"
        },
        "cloud-init-local": {
            "description": "Initial cloud-init job (pre-networking)",
            "state": "STARTED"
        },
        "crond": {
            "description": "Command Scheduler",
            "state": "STARTED"
        },
        "dbus": {
            "description": "D-Bus System Message Bus",
            "state": "STARTED"
        },
        "dm-event": {
            "description": "Device-mapper event daemon",
            "state": "STOPPED"
        },
        "dnsmasq": {
            "description": "A lightweight, caching DNS proxy",
            "state": "STARTED"
        },
        "dracut-cmdline": {
            "description": "dracut cmdline hook",
            "state": "STOPPED"
        },
        "dracut-initqueue": {
            "description": "dracut initqueue hook",
            "state": "STOPPED"
        },
        "dracut-mount": {
            "description": "dracut mount hook",
            "state": "STOPPED"
        },
        "dracut-pre-mount": {
            "description": "dracut pre-mount hook",
            "state": "STOPPED"
        },
        "dracut-pre-pivot": {
            "description": "dracut pre-pivot and cleanup hook",
            "state": "STOPPED"
        },
        "dracut-pre-trigger": {
            "description": "dracut pre-trigger hook",
            "state": "STOPPED"
        },
        "dracut-pre-udev": {
            "description": "dracut pre-udev hook",
            "state": "STOPPED"
        },
        "dracut-shutdown": {
            "description": "Restore /run/initramfs on shutdown",
            "state": "STARTED"
        },
        "emergency": {
            "description": "Emergency Shell",
            "state": "STOPPED"
        },
        "getty@tty2": {
            "description": "DCUI",
            "state": "STARTED"
        },
        "haveged": {
            "description": "Entropy Daemon based on the HAVEGE algorithm",
            "state": "STARTED"
        },
        "initrd-cleanup": {
            "description": "Cleaning Up and Shutting Down Daemons",
            "state": "STOPPED"
        },
        "initrd-parse-etc": {
            "description": "Reload Configuration from the Real Root",
            "state": "STOPPED"
        },
        "initrd-switch-root": {
            "description": "Switch Root",
            "state": "STOPPED"
        },
        "initrd-udevadm-cleanup-db": {
            "description": "Cleanup udevd DB",
            "state": "STOPPED"
        },
        "irqbalance": {
            "description": "irqbalance daemon",
            "state": "STARTED"
        },
        "kmod-static-nodes": {
            "description": "Create list of required static device nodes for the current kernel",
            "state": "STARTED"
        },
        "lsassd": {
            "description": "Likewise Security and Authentication Subsystem",
            "state": "STARTED"
        },
        "lvm2-activate": {
            "description": "LVM2 activate volume groups",
            "state": "STARTED"
        },
        "lvm2-lvmetad": {
            "description": "LVM2 metadata daemon",
            "state": "STARTED"
        },
        "lvm2-pvscan@253:2": {
            "description": "LVM2 PV scan on device 253:2",
            "state": "STARTED"
        },
        "lvm2-pvscan@253:4": {
            "description": "LVM2 PV scan on device 253:4",
            "state": "STARTED"
        },
        "lwsmd": {
            "description": "Likewise Service Control Manager Service",
            "state": "STARTED"
        },
        "ntpd": {
            "description": "Network Time Service",
            "state": "STARTED"
        },
        "observability": {
            "description": "VMware Observability Service",
            "state": "STARTED"
        },
        "rc-local": {
            "description": "/etc/rc.d/rc.local Compatibility",
            "state": "STARTED"
        },
        "rescue": {
            "description": "Rescue Shell",
            "state": "STOPPED"
        },
        "rsyslog": {
            "description": "System Logging Service",
            "state": "STARTED"
        },
        "sendmail": {
            "description": "Sendmail Mail Transport Agent",
            "state": "STARTED"
        },
        "sshd": {
            "description": "OpenSSH Daemon",
            "state": "STARTED"
        },
        "sshd-keygen": {
            "description": "Generate sshd host keys",
            "state": "STOPPED"
        },
        "syslog-ng": {
            "description": "System Logger Daemon",
            "state": "STOPPED"
        },
        "sysstat": {
            "description": "Resets System Activity Logs",
            "state": "STARTED"
        },
        "sysstat-collect": {
            "description": "system activity accounting tool",
            "state": "STOPPED"
        },
        "sysstat-summary": {
            "description": "Generate a daily summary of process accounting",
            "state": "STOPPED"
        },
        "systemd-ask-password-console": {
            "description": "Dispatch Password Requests to Console",
            "state": "STOPPED"
        },
        "systemd-ask-password-wall": {
            "description": "Forward Password Requests to Wall",
            "state": "STOPPED"
        },
        "systemd-binfmt": {
            "description": "Set Up Additional Binary Formats",
            "state": "STOPPED"
        },
        "systemd-fsck-root": {
            "description": "File System Check on Root Device",
            "state": "STARTED"
        },
        "systemd-hostnamed": {
            "description": "Hostname Service",
            "state": "STARTED"
        },
        "systemd-hwdb-update": {
            "description": "Rebuild Hardware Database",
            "state": "STARTED"
        },
        "systemd-initctl": {
            "description": "initctl Compatibility Daemon",
            "state": "STOPPED"
        },
        "systemd-journal-catalog-update": {
            "description": "Rebuild Journal Catalog",
            "state": "STARTED"
        },
        "systemd-journal-flush": {
            "description": "Flush Journal to Persistent Storage",
            "state": "STARTED"
        },
        "systemd-journald": {
            "description": "Journal Service",
            "state": "STARTED"
        },
        "systemd-logind": {
            "description": "Login Service",
            "state": "STARTED"
        },
        "systemd-machine-id-commit": {
            "description": "Commit a transient machine-id on disk",
            "state": "STOPPED"
        },
        "systemd-modules-load": {
            "description": "Load Kernel Modules",
            "state": "STARTED"
        },
        "systemd-networkd": {
            "description": "Network Service",
            "state": "STARTED"
        },
        "systemd-networkd-wait-online": {
            "description": "Wait for Network to be Configured",
            "state": "STARTED"
        },
        "systemd-quotacheck": {
            "description": "File System Quota Check",
            "state": "STOPPED"
        },
        "systemd-random-seed": {
            "description": "Load/Save Random Seed",
            "state": "STARTED"
        },
        "systemd-remount-fs": {
            "description": "Remount Root and Kernel File Systems",
            "state": "STARTED"
        },
        "systemd-resolved": {
            "description": "Network Name Resolution",
            "state": "STARTED"
        },
        "systemd-sysctl": {
            "description": "Apply Kernel Variables",
            "state": "STARTED"
        },
        "systemd-tmpfiles-clean": {
            "description": "Cleanup of Temporary Directories",
            "state": "STOPPED"
        },
        "systemd-tmpfiles-setup": {
            "description": "Create Volatile Files and Directories",
            "state": "STARTED"
        },
        "systemd-tmpfiles-setup-dev": {
            "description": "Create Static Device Nodes in /dev",
            "state": "STARTED"
        },
        "systemd-udev-trigger": {
            "description": "udev Coldplug all Devices",
            "state": "STARTED"
        },
        "systemd-udevd": {
            "description": "udev Kernel Device Manager",
            "state": "STARTED"
        },
        "systemd-update-done": {
            "description": "Update is Completed",
            "state": "STARTED"
        },
        "systemd-update-utmp": {
            "description": "Update UTMP about System Boot/Shutdown",
            "state": "STARTED"
        },
        "systemd-update-utmp-runlevel": {
            "description": "Update UTMP about System Runlevel Changes",
            "state": "STOPPED"
        },
        "systemd-user-sessions": {
            "description": "Permit User Sessions",
            "state": "STARTED"
        },
        "systemd-vconsole-setup": {
            "description": "Setup Virtual Console",
            "state": "STOPPED"
        },
        "vami-lighttp": {
            "description": "vami-lighttp.service",
            "state": "STARTED"
        },
        "vgauthd": {
            "description": "VGAuth Service for open-vm-tools",
            "state": "STOPPED"
        },
        "vmafdd": {
            "description": "LSB: Authentication Framework Daemon",
            "state": "STARTED"
        },
        "vmcad": {
            "description": "LSB: Start and Stop vmca",
            "state": "STARTED"
        },
        "vmdird": {
            "description": "LSB: Start and Stop vmdir",
            "state": "STARTED"
        },
        "vmtoolsd": {
            "description": "Service for virtual machines hosted on VMware",
            "state": "STOPPED"
        },
        "vmware-firewall": {
            "description": "VMware Firewall service",
            "state": "STARTED"
        },
        "vmware-pod": {
            "description": "VMware Pod Service.",
            "state": "STARTED"
        },
        "vmware-vdtc": {
            "description": "VMware vSphere Distrubuted Tracing Collector",
            "state": "STARTED"
        },
        "vmware-vmon": {
            "description": "VMware Service Lifecycle Manager",
            "state": "STARTED"
        }
    }
}
```

If you need to target a specific service, you can pass its name
through the `service` parameter.

```
- name: Get information about ntpd
  vmware.vmware_rest.appliance_services_info:
    service: ntpd
```

response

```
{
    "changed": false,
    "id": "ntpd",
    "value": {
        "description": "ntpd.service",
        "state": "STARTED"
    }
}
```

Use the appliance_services module to stop a service:

```
- name: Stop the ntpd service
  vmware.vmware_rest.appliance_services:
    service: ntpd
    state: stop
```

response

```
{
    "changed": false,
    "value": {}
}
```

or to start a service:

```
- name: Start the ntpd service
  vmware.vmware_rest.appliance_services:
    service: ntpd
    state: start
```

response

```
{
    "changed": false,
    "value": {}
}
```

## VMON services

The VMON services can also be managed from Ansible. For instance to
get the state of the `vpxd` service:

```
- name: Get information about a VMON service
  vmware.vmware_rest.appliance_vmon_service_info:
    service: vpxd
```

response

```
{
    "changed": false,
    "value": [
        {
            "key": "analytics",
            "value": {
                "description_key": "cis.analytics.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [],
                "name_key": "cis.analytics.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "applmgmt",
            "value": {
                "description_key": "cis.applmgmt.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [],
                "name_key": "cis.applmgmt.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "certificateauthority",
            "value": {
                "description_key": "cis.certificateauthority.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [
                    {
                        "args": [
                            "GREEN"
                        ],
                        "default_message": "Health is GREEN",
                        "id": "certificateathority.health.statuscode"
                    }
                ],
                "name_key": "cis.certificateauthority.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "certificatemanagement",
            "value": {
                "description_key": "cis.certificatemanagement.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [
                    {
                        "args": [
                            "GREEN"
                        ],
                        "default_message": "Health is GREEN",
                        "id": "certificatemanagement.health.statuscode"
                    }
                ],
                "name_key": "cis.certificatemanagement.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "cis-license",
            "value": {
                "description_key": "cis.cis-license.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [
                    {
                        "args": [],
                        "default_message": "The License Service is operational.",
                        "id": "cis.license.health.ok"
                    }
                ],
                "name_key": "cis.cis-license.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "content-library",
            "value": {
                "description_key": "cis.content-library.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [
                    {
                        "args": [],
                        "default_message": "Database server connection is GREEN.",
                        "id": "com.vmware.vdcs.vsphere-cs-lib.db_health_green"
                    }
                ],
                "name_key": "cis.content-library.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "eam",
            "value": {
                "description_key": "cis.eam.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [
                    {
                        "args": [],
                        "default_message": "",
                        "id": "cis.eam.statusOK"
                    }
                ],
                "name_key": "cis.eam.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "envoy",
            "value": {
                "description_key": "cis.envoy.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [],
                "name_key": "cis.envoy.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "hvc",
            "value": {
                "description_key": "cis.hvc.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [
                    {
                        "args": [
                            "GREEN"
                        ],
                        "default_message": "Health is GREEN",
                        "id": "hvc.health.statuscode"
                    }
                ],
                "name_key": "cis.hvc.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "imagebuilder",
            "value": {
                "description_key": "cis.imagebuilder.ServiceDescription",
                "name_key": "cis.imagebuilder.ServiceName",
                "startup_type": "MANUAL",
                "state": "STOPPED"
            }
        },
        {
            "key": "infraprofile",
            "value": {
                "description_key": "cis.infraprofile.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [
                    {
                        "args": [
                            "GREEN"
                        ],
                        "default_message": "Health is GREEN",
                        "id": "infraprofile.health.statuscode"
                    }
                ],
                "name_key": "cis.infraprofile.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "lookupsvc",
            "value": {
                "description_key": "cis.lookupsvc.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [],
                "name_key": "cis.lookupsvc.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "netdumper",
            "value": {
                "description_key": "cis.netdumper.ServiceDescription",
                "name_key": "cis.netdumper.ServiceName",
                "startup_type": "MANUAL",
                "state": "STOPPED"
            }
        },
        {
            "key": "observability-vapi",
            "value": {
                "description_key": "cis.observability-vapi.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [
                    {
                        "args": [
                            "GREEN"
                        ],
                        "default_message": "Health is GREEN",
                        "id": "observability.health.statuscode"
                    }
                ],
                "name_key": "cis.observability-vapi.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "perfcharts",
            "value": {
                "description_key": "cis.perfcharts.ServiceDescription",
                "health": "DEGRADED",
                "health_messages": [
                    {
                        "args": [],
                        "default_message": "health.statsReoptInitalizer.illegalStateEx",
                        "id": "health.statsReoptInitalizer.illegalStateEx"
                    }
                ],
                "name_key": "cis.perfcharts.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "pschealth",
            "value": {
                "description_key": "cis.pschealth.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [],
                "name_key": "cis.pschealth.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "rbd",
            "value": {
                "description_key": "cis.rbd.ServiceDescription",
                "name_key": "cis.rbd.ServiceName",
                "startup_type": "MANUAL",
                "state": "STOPPED"
            }
        },
        {
            "key": "rhttpproxy",
            "value": {
                "description_key": "cis.rhttpproxy.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [],
                "name_key": "cis.rhttpproxy.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "sca",
            "value": {
                "description_key": "cis.sca.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [],
                "name_key": "cis.sca.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "sps",
            "value": {
                "description_key": "cis.sps.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [],
                "name_key": "cis.sps.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "statsmonitor",
            "value": {
                "description_key": "cis.statsmonitor.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [
                    {
                        "args": [],
                        "default_message": "Appliance monitoring service is healthy.",
                        "id": "com.vmware.applmgmt.mon.health.healthy"
                    }
                ],
                "name_key": "cis.statsmonitor.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "sts",
            "value": {
                "description_key": "cis.sts.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [],
                "name_key": "cis.sts.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "topologysvc",
            "value": {
                "description_key": "cis.topologysvc.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [
                    {
                        "args": [
                            "GREEN"
                        ],
                        "default_message": "Health is GREEN",
                        "id": "topologysvc.health.statuscode"
                    }
                ],
                "name_key": "cis.topologysvc.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "trustmanagement",
            "value": {
                "description_key": "cis.trustmanagement.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [
                    {
                        "args": [
                            "GREEN"
                        ],
                        "default_message": "Health is GREEN",
                        "id": "trustmanagement.health.statuscode"
                    }
                ],
                "name_key": "cis.trustmanagement.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "updatemgr",
            "value": {
                "description_key": "cis.updatemgr.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [],
                "name_key": "cis.updatemgr.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "vapi-endpoint",
            "value": {
                "description_key": "cis.vapi-endpoint.ServiceDescription",
                "health": "HEALTHY_WITH_WARNINGS",
                "health_messages": [
                    {
                        "args": [
                            "498abd68-65d4-4272-ad63-1918298aafc8\\com.vmware.cis.ds"
                        ],
                        "default_message": "Failed to connect to 498abd68-65d4-4272-ad63-1918298aafc8\\com.vmware.cis.ds vAPI provider.",
                        "id": "com.vmware.vapi.endpoint.failedToConnectToVApiProvider"
                    },
                    {
                        "args": [
                            "2021-05-18T23:51:42UTC",
                            "2021-05-18T23:51:43UTC"
                        ],
                        "default_message": "Configuration health status is created between 2021-05-18T23:51:42UTC and 2021-05-18T23:51:43UTC.",
                        "id": "com.vmware.vapi.endpoint.healthStatusProducedTimes"
                    }
                ],
                "name_key": "cis.vapi-endpoint.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "vcha",
            "value": {
                "description_key": "cis.vcha.ServiceDescription",
                "name_key": "cis.vcha.ServiceName",
                "startup_type": "DISABLED",
                "state": "STOPPED"
            }
        },
        {
            "key": "vlcm",
            "value": {
                "description_key": "cis.vlcm.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [],
                "name_key": "cis.vlcm.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "vmcam",
            "value": {
                "description_key": "cis.vmcam.ServiceDescription",
                "name_key": "cis.vmcam.ServiceName",
                "startup_type": "MANUAL",
                "state": "STOPPED"
            }
        },
        {
            "key": "vmonapi",
            "value": {
                "description_key": "cis.vmonapi.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [],
                "name_key": "cis.vmonapi.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "vmware-postgres-archiver",
            "value": {
                "description_key": "cis.vmware-postgres-archiver.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [
                    {
                        "args": [],
                        "default_message": "VMware Archiver service is healthy.",
                        "id": "cis.vmware-postgres-archiver.health.healthy"
                    }
                ],
                "name_key": "cis.vmware-postgres-archiver.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "vmware-vpostgres",
            "value": {
                "description_key": "cis.vmware-vpostgres.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [
                    {
                        "args": [],
                        "default_message": "Service vmware-vpostgres is healthy.",
                        "id": "cis.vmware-vpostgres.health.healthy"
                    }
                ],
                "name_key": "cis.vmware-vpostgres.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "vpxd",
            "value": {
                "description_key": "cis.vpxd.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [
                    {
                        "args": [
                            "vCenter Server",
                            "GREEN"
                        ],
                        "default_message": "{0} health is {1}",
                        "id": "vc.health.statuscode"
                    },
                    {
                        "args": [
                            "VirtualCenter Database",
                            "GREEN"
                        ],
                        "default_message": "{0} health is {1}",
                        "id": "vc.health.statuscode"
                    }
                ],
                "name_key": "cis.vpxd.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "vpxd-svcs",
            "value": {
                "description_key": "cis.vpxd-svcs.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [
                    {
                        "args": [],
                        "default_message": "Tagging service is in a healthy state",
                        "id": "cis.tagging.health.status"
                    }
                ],
                "name_key": "cis.vpxd-svcs.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "vsan-health",
            "value": {
                "description_key": "cis.vsan-health.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [],
                "name_key": "cis.vsan-health.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "vsm",
            "value": {
                "description_key": "cis.vsm.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [],
                "name_key": "cis.vsm.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "vsphere-ui",
            "value": {
                "description_key": "cis.vsphere-ui.ServiceDescription",
                "name_key": "cis.vsphere-ui.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STOPPED"
            }
        },
        {
            "key": "vstats",
            "value": {
                "description_key": "cis.vstats.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [],
                "name_key": "cis.vstats.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "vtsdb",
            "value": {
                "description_key": "cis.vtsdb.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [
                    {
                        "args": [],
                        "default_message": "Service vtsdb is healthy.",
                        "id": "cis.vtsdb.health.healthy"
                    }
                ],
                "name_key": "cis.vtsdb.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        },
        {
            "key": "wcp",
            "value": {
                "description_key": "cis.wcp.ServiceDescription",
                "health": "HEALTHY",
                "health_messages": [],
                "name_key": "cis.wcp.ServiceName",
                "startup_type": "AUTOMATIC",
                "state": "STARTED"
            }
        }
    ]
}
```

And to ensure it starts `automatically`:

```
- name: Adjust vpxd configuration
  vmware.vmware_rest.appliance_vmon_service:
    service: vpxd
    startup_type: AUTOMATIC
```

response

```
{
    "changed": false,
    "id": "vpxd",
    "value": {
        "description_key": "cis.vpxd.ServiceDescription",
        "health": "HEALTHY",
        "health_messages": [
            {
                "args": [
                    "vCenter Server",
                    "GREEN"
                ],
                "default_message": "{0} health is {1}",
                "id": "vc.health.statuscode"
            },
            {
                "args": [
                    "VirtualCenter Database",
                    "GREEN"
                ],
                "default_message": "{0} health is {1}",
                "id": "vc.health.statuscode"
            }
        ],
        "name_key": "cis.vpxd.ServiceName",
        "startup_type": "AUTOMATIC",
        "state": "STARTED"
    }
}
```
# System managment

## How to reboot or shutdown the VCSA

You can use Ansible to trigger or cancel a shutdown. The
appliance_shutdown_info module is useful to know if a shutdown is
already scheduled.

```
- name: Check if there is a shutdown scheduled
  vmware.vmware_rest.appliance_shutdown_info:
```

response

```
{
    "changed": false,
    "value": {
        "action": "",
        "reason": ""
    }
}
```

When you trigger a shutdown, you can also specify a `reason`. The
information will be exposed to the other users:

```
- name: Shutdown the appliance
  vmware.vmware_rest.appliance_shutdown:
    state: poweroff
    reason: this is an example
    delay: 600
```

response

```
{
    "changed": false,
    "value": {}
}
```

To cancel a shutdown, you must set the `state` to `cancel`:

```
- name: Abort the shutdown of the appliance
  vmware.vmware_rest.appliance_shutdown:
    state: cancel
```

response

```
{
    "changed": false,
    "value": {}
}
```

# FIPS mode

## Federal Information Processing Standards (FIPS)

The appliance_system_globalfips_info module will tell you if FIPS is
enabled.

```
- name: "Get the status of the Federal Information Processing Standard mode"
  vmware.vmware_rest.appliance_system_globalfips_info:
```

response

```
{
    "changed": false,
    "value": {
        "enabled": false
    }
}
```

You can turn the option on or off with appliance_system_globalfips:

Warning: The VCSA will silently reboot itself if you change the FIPS

    configuration.

```
- name: Turn off the FIPS mode and reboot
  vmware.vmware_rest.appliance_system_globalfips:
    enabled: false
```

response

```
{
    "changed": false,
    "id": null,
    "value": {
        "enabled": false
    }
}
```

# Time and Timezone configuration

## Timezone

The appliance_system_time_timezone and
ppliance_system_time_timezone_info modules handle the Timezone
configuration. You can get the current configuration with:

```
- name: Get the timezone configuration
  vmware.vmware_rest.appliance_system_time_timezone_info:
```

response

```
{
    "changed": false,
    "value": "UTC"
}
```

And to adjust the system’s timezone, just do:

```
- name: Use the UTC timezone
  vmware.vmware_rest.appliance_system_time_timezone:
    name: UTC
```

response

```
{
    "changed": false,
    "value": "UTC"
}
```

In this example we set the `UTC` timezone, you can also pass a
timezone in the `Europe/Paris` format.

## Current time

If you want to get the current time, use appliance_system_time_info:

```
- name: Get the current time
  vmware.vmware_rest.appliance_system_time_info:
```

response

```
{
    "changed": false,
    "value": {
        "date": "Tue 05-18-2021",
        "seconds_since_epoch": 1621382295.21046,
        "time": "11:58:15 PM",
        "timezone": "UTC"
    }
}
```

## Time Service (NTP)

The VCSA can get the time from a NTP server:

```
- name: Get the NTP configuration
  vmware.vmware_rest.appliance_ntp_info:
```

response

```
{
    "changed": false,
    "value": [
        "time.google.com"
    ]
}
```

You can use the appliance_ntp module to adjust the system NTP servers.
The module accepts one or more NTP servers:

```
- name: Adjust the NTP configuration
  vmware.vmware_rest.appliance_ntp:
    servers:
      - time.google.com
```

response

```
{
    "changed": false,
    "value": [
        "time.google.com"
    ]
}
```

If you set `state=test`, the module will validate the servers are
rechable.

```
- name: Test the NTP configuration
  vmware.vmware_rest.appliance_ntp:
    state: test
    servers:
      - time.google.com
```

response

```
{
    "changed": false,
    "value": [
        {
            "message": {
                "args": [],
                "default_message": "NTP Server is reachable.",
                "id": "com.vmware.appliance.ntp_sync.success"
            },
            "server": "time.google.com",
            "status": "SERVER_REACHABLE"
        }
    ]
}
```

You can check the clock synchronization with appliance_timesync_info:

```
- name: Get information regarding the clock synchronization
  vmware.vmware_rest.appliance_timesync_info:
```

response

```
{
    "changed": false,
    "value": "NTP"
}
```

Or also validate the system use NTP with:

```
- name: Ensure we use NTP
  vmware.vmware_rest.appliance_timesync:
    mode: NTP
```

response

```
{
    "changed": false,
    "value": "NTP"
}
```

# Storage system

The collection also provides modules to manage the storage system.
appliance_system_storage_info will list the storage partitions:

```
- name: Get the appliance storage information
  vmware.vmware_rest.appliance_system_storage_info:
```

response

```
{
    "changed": false,
    "value": []
}
```

You can use the `state=resize_ex` option to extend an existing
partition:

```
- name: Resize the first partition and return the state of the partition before and after the operation
  vmware.vmware_rest.appliance_system_storage:
    state: resize_ex
```

response

```
{
    "changed": false,
    "value": {}
}
```

Note: `state=resize` also works, but you won’t get as much information

    as with `resize_ex`.
