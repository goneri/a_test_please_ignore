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
