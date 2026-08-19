PVSTP (Per-VLAN Spanning Tree Protocol)

Overview

PVSTP (Per-VLAN Spanning Tree Protocol) is a Cisco implementation of Spanning Tree Protocol (STP) that maintains a separate STP instance for each VLAN.

This allows each VLAN to have its own Root Bridge and spanning-tree topology.

Key Features

* Separate STP instance for each VLAN
* Different Root Bridge can be configured for each VLAN
* Prevents Layer-2 switching loops
* Provides redundancy between switches
* Automatically recalculates the topology when a link fails
* Allows better utilization of redundant links
* Provides per-VLAN traffic path control
* Works with VLANs and 802.1Q trunk links

What Can We Achieve With PVSTP?

PVSTP can be used to:

1. Prevent Layer-2 loops
2. Provide network redundancy
3. Provide automatic failover
4. Control the preferred traffic path for each VLAN
5. Distribute traffic across redundant links
6. Improve utilization of available network links

Example

Consider three switches:

 
              SW1
             /   \
            /     \
           /       \
         SW2-------SW3
   

Different Root Bridges can be configured for different VLANs:

 
VLAN 10 → SW1 = Root Bridge
VLAN 20 → SW2 = Root Bridge
VLAN 30 → SW3 = Root Bridge
   

Therefore, different VLANs can use different forwarding paths.

Where Can We Use PVSTP?

PVSTP is useful in Cisco Layer-2 switched networks where multiple VLANs and redundant links exist.

Common Use Cases

* Enterprise networks
* Campus networks
* Office networks
* Data center Layer-2 networks
* Server networks
* Networks with multiple VLANs
* Networks with redundant switch-to-switch links

Example VLAN Structure


VLAN 10 → Users
VLAN 20 → Servers
VLAN 30 → Management
VLAN 40 → Voice
   

Each VLAN can have a different preferred STP path.

Redundancy and Failover

PVSTP blocks redundant paths when necessary to prevent Layer-2 loops.

Example:

        SW1
       /   \
      /     \
    SW2-----SW3
   

If one path is blocked by STP:

        SW1
       /   \
      /     \
    SW2--X--SW3
   

If the active path fails, STP recalculates the topology and can allow the alternative path to forward traffic.

Key Concept

PVSTP = One Spanning Tree instance per VLAN

Example:

   text
VLAN 10 → Root Bridge: SW1
VLAN 20 → Root Bridge: SW2
VLAN 30 → Root Bridge: SW3
   

This provides:

* Loop prevention
* Network redundancy
* Automatic failover
* Per-VLAN traffic path control
* Better utilization of redundant links

Useful Cisco Commands

Enable PVST:

   cisco
spanning-tree mode pvst
   

Make a switch Root Bridge:

   cisco
spanning-tree vlan 10 root primary
   

Configure a Secondary Root:

   cisco
spanning-tree vlan 10 root secondary
   

Check STP Status:

   cisco
show spanning-tree
   

Check a Specific VLAN:

   cisco
show spanning-tree vlan 10
   

Check Root Bridges:

   cisco
show spanning-tree root
   

Summary

| Feature                 | PVSTP        |
| ----------------------- | ------------ |
| STP Instance            | One per VLAN |
| Loop Prevention         | Yes          |
| Redundancy              | Yes          |
| Automatic Failover      | Yes          |
| Different Root per VLAN | Yes          |
| Traffic Path Control    | Yes          |
| Cisco Environment       | Yes          |

One-Line Summary

PVSTP allows Cisco switches to run a separate Spanning Tree for each VLAN, preventing Layer-2 loops while providing VLAN-level redundancy and traffic-path control.
