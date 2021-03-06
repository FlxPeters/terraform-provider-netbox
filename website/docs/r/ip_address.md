---
# generated by https://github.com/hashicorp/terraform-plugin-docs
page_title: "netbox_ip_address Resource - terraform-provider-netbox"
subcategory: ""
description: |-
  
---

# netbox_ip_address (Resource)

Per [the docs](https://netbox.readthedocs.io/en/stable/models/ipam/ipaddress/):

> An IP address comprises a single host address (either IPv4 or IPv6) and its subnet mask. Its mask should match exactly how the IP address is configured on an interface in the real world.
> Like a prefix, an IP address can optionally be assigned to a VRF (otherwise, it will appear in the "global" table). IP addresses are automatically arranged under parent prefixes within their respective VRFs according to the IP hierarchy.
>
> Each IP address can also be assigned an operational status and a functional role. Statuses are hard-coded in NetBox and include the following:
>
> - Active
> - Reserved
> - Deprecated
> - DHCP
> - SLAAC (IPv6 Stateless Address Autoconfiguration)

## Example Usage

### Marking an IP reserved
```terraform
resource "netbox_ip_address" "ip" {
    ip_address = "10.0.0.50/24"
    status = "reserved"
    dns_name = "dc-west-myvm-20.mydomain.local"
}
```

### Marking an IP active and assigning to interface

```terraform
// Assumes Netbox already has a VM whos name matches 'dc-west-myvm-20'
data "netbox_virtual_machine" "myvm" {
    name_regex = "dc-west-myvm-20"
}

resource "netbox_interface" "myvm-eth0" {
    name = "eth0"
    virtual_machine_id = data.netbox_virtual_machine.myvm.id
}

resource "netbox_ip_address" "myvm-ip" {
    ip_address = "10.0.0.60/24"
    status = "active"
    interface_id = netbox_interface.myvm-eth0.id
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Required

- **ip_address** (String)
- **status** (String)

### Optional

- **dns_name** (String)
- **id** (String) The ID of this resource.
- **interface_id** (Number)
- **tags** (Set of String)
- **tenant_id** (Number)
- **vrf_id** (Number)


