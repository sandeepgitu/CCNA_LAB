VLAN – Basic Configuration
What is VLAN?

VLAN (Virtual Local Area Network) logically divides a physical switch into multiple Layer 2 broadcast domains.

Example:
VLAN 10 → SALES
VLAN 20 → IT

Devices in different VLANs are logically separated and require Inter-VLAN Routing to communicate.

Why Use VLANs?

* Network segmentation
* Reduced broadcast traffic
* Improved isolation and security
* Better network organization
* Efficient use of switching infrastructure

Configuration

Create VLANs

cisco
Switch(config)# vlan 10
Switch(config-vlan)# name SALES
Switch(config-vlan)# exit

Switch(config)# vlan 20
Switch(config-vlan)# name IT
Switch(config-vlan)# exit


Assign Ports

cisco
Switch(config)# interface range fa0/1 - 2
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 10
Switch(config-if-range)# exit

Switch(config)# interface range fa0/3 - 4
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 20
Switch(config-if-range)# exit


Verification

cisco
show vlan brief
show interfaces switchport


show vlan brief verifies VLANs and the ports assigned to them.

Key Concepts

* VLAN → Logical Layer 2 network
* Access Port → Carries traffic for a single VLAN
* Broadcast Domain → Each VLAN is a separate broadcast domain
* Inter-VLAN Routing → Allows communication between different VLANs
