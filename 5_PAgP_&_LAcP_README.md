LACP and PAgP EtherChannel

Overview

EtherChannel is a Cisco technology that combines multiple physical links
between network devices into a single logical link called a
Port-Channel.

LACP and PAgP are negotiation protocols used to dynamically form and
manage EtherChannel bundles.

LACP is an open standard protocol defined by IEEE 802.3ad and is widely
supported by network vendors.

PAgP is a Cisco proprietary EtherChannel negotiation protocol.

LACP

LACP stands for Link Aggregation Control Protocol.

LACP dynamically negotiates the formation of an EtherChannel between
compatible devices.

LACP Modes

Active

The interface actively sends LACP negotiation packets and attempts to
form an EtherChannel.

Passive

The interface waits for LACP packets from the other device and responds
when negotiation is initiated.

LACP can form an EtherChannel when one side is Active and the other side
is Active or Passive.

PAgP

PAgP stands for Port Aggregation Protocol.

PAgP is a Cisco proprietary protocol used to dynamically negotiate
EtherChannel.

PAgP Modes

Desirable

The interface actively attempts to negotiate and establish an
EtherChannel.

Auto

The interface waits for the neighboring device to initiate PAgP
negotiation.

PAgP can form an EtherChannel when one side is Desirable and the other
side is Desirable or Auto.

Features

Combines multiple physical Ethernet links into one logical Port-Channel.

Provides increased bandwidth by using multiple physical links.

Provides link redundancy between network devices.

Uses negotiation protocols to dynamically establish EtherChannel.

Supports Layer 2 EtherChannel configurations.

Can also be used with Layer 3 EtherChannel configurations depending on
the platform and configuration.

Distributes traffic across multiple physical links.

Provides continued connectivity when one member link fails, provided
other member links remain operational.

LACP is based on an open IEEE standard.

PAgP is a Cisco proprietary protocol.

LACP supports Active and Passive negotiation modes.

PAgP supports Desirable and Auto negotiation modes.

Use Cases

Connecting switches using multiple physical links.

Increasing bandwidth between distribution and access switches.

Providing redundancy between network devices.

Aggregating links between switches and servers that support link
aggregation.

Building resilient network connections in enterprise environments.

Reducing the impact of a single physical link failure.

Using multiple links as one logical connection for simplified network
management.

Advantages

Higher aggregate bandwidth compared with a single physical link.

Improved redundancy and availability.

Automatic negotiation and management when using LACP or PAgP.

Traffic can continue through remaining member links when one physical
link fails.

Simplified management because multiple physical links are represented as
one logical Port-Channel.

Better utilization of available physical interfaces.

LACP provides multi-vendor interoperability because it is an IEEE
standard.

PAgP provides simple Cisco-specific EtherChannel negotiation.

LACP vs PAgP

LACP

Full Name: Link Aggregation Control Protocol

Standard: IEEE 802.3ad

Vendor: Open standard

Modes: Active and Passive

Active mode: Initiates negotiation

Passive mode: Responds to negotiation

Multi-vendor support: Yes

PAgP

Full Name: Port Aggregation Protocol

Standard: Cisco proprietary

Vendor: Cisco

Modes: Desirable and Auto

Desirable mode: Initiates negotiation

Auto mode: Responds to negotiation

Multi-vendor support: Generally no

Common EtherChannel Verification Commands

show etherchannel summary

show etherchannel detail

show interfaces port-channel

show lacp neighbor

show pagp neighbor

show interfaces etherchannel

Example LACP Configuration

Switch 1

interface range fa0/1 - 2 channel-group 1 mode active exit

interface port-channel 1 switchport mode trunk exit

Switch 2

interface range fa0/1 - 2 channel-group 1 mode active exit

interface port-channel 1 switchport mode trunk exit

Example PAgP Configuration

Switch 1

interface range fa0/1 - 2 channel-group 2 mode desirable exit

interface port-channel 2 switchport mode trunk exit

Switch 2

interface range fa0/1 - 2 channel-group 2 mode auto exit

interface port-channel 2 switchport mode trunk exit

Important Configuration Requirements

All member interfaces should have compatible speed and duplex settings.

Interfaces participating in the same EtherChannel should have matching
VLAN and trunk settings.

The interfaces should be configured consistently on both sides.

The same EtherChannel group should be used for the intended bundle.

LACP should use compatible LACP modes.

PAgP should use compatible PAgP modes.

A mismatch in configuration can prevent the EtherChannel from forming
correctly.

Verification Output

A working EtherChannel may display an output similar to:

Group Port-channel Protocol Ports 1 Po1(SU) LACP Fa0/1(P) Fa0/2(P)

SU means the Port-Channel is Layer 2 and currently in use.

SD means the Port-Channel is Layer 2 but currently down.

P means the physical interface is successfully bundled into the
Port-Channel.

Conclusion

LACP and PAgP provide mechanisms for combining multiple physical
Ethernet interfaces into a single logical EtherChannel.

LACP is generally preferred for modern networks because it is an open
IEEE standard and provides interoperability between different network
vendors.

PAgP is useful in Cisco-specific environments where Cisco proprietary
EtherChannel negotiation is appropriate.

Learning both protocols is important for understanding link aggregation,
redundancy, bandwidth optimization, and enterprise switching.
