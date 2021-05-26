# vmware.vmware_rest.appliance_networking_firewall_inbound

**Set the ordered list of firewall rules to allow or deny traffic from
one or more incoming IP addresses**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Examples


* Return Values


* Status

## Synopsis


* Set the ordered list of firewall rules to allow or deny traffic
from one or more incoming IP addresses. This overwrites the
existing firewall rules and creates a new rule list. Within the
list of traffic rules, rules are processed in order of appearance,
from top to bottom. For example, the list of rules can be as
follows: <table> <tr> <th>Address</th><th>Prefix</th><th>Interface
Name</th><th>Policy</th> </tr> <tr>
<td>10.112.0.1</td><td>0</td><td>\*</td><td>REJECT</td> </tr> <tr>
<td>10.112.0.1</td><td>0</td><td>nic0</td><td>ACCEPT</td> </tr>
</table> In the above example, the first rule drops all packets
originating from 10.112.0.1 and<br> the second rule accepts all
packets originating from 10.112.0.1 only on nic0. In effect, the
second rule is always ignored which is not desired, hence the order
has to be swapped. When a connection matches a firewall rule,
further processing for the connection stops, and the appliance
ignores any additional firewall rules you have set.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Examples

```
- name: Ensure the rules parameter is mandatory
  vmware.vmware_rest.appliance_networking_firewall_inbound:
  register: result
  failed_when:
  - not(result.failed)
  - result.msg == 'missing required arguments: rules'

- name: Set a firewall rule
  vmware.vmware_rest.appliance_networking_firewall_inbound:
    rules:
    - address: 1.2.3.4
      prefix: 32
      policy: ACCEPT
  register: result
```

## Return Values

Common return values are documented [here](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html#common-return-values),
the following are the fields unique to this module:

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)
