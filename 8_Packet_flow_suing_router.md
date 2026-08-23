# Packet Forwarding Using a Router

## Overview

A router is a **Layer 3 device** that forwards packets between different networks using the **destination IP address**.

This lab explains how a packet travels from one subnet to another using a router, routing table, and ARP.

## Network Topology

```text
PC-A                         PC-D
10.0.0.2/24                 20.0.0.2/24
Gateway: 10.0.0.1           Gateway: 20.0.0.1
    |                            |
   SW1                          SW2
    |                            |
    +------ Fa0/0      Fa0/1 ---+
             R1
          10.0.0.1
          20.0.0.1
```

- `10.0.0.0/24` → connected to **R1 Fa0/0**
- `20.0.0.0/24` → connected to **R1 Fa0/1**

## Packet Flow

Assume **PC-A wants to communicate with PC-D**:

```text
Source IP      : 10.0.0.2
Destination IP : 20.0.0.2
```

### 1. PC-A Checks the Destination

PC-A checks whether `20.0.0.2` is in its own subnet.

- PC-A network: `10.0.0.0/24`
- Destination network: `20.0.0.0/24`

Since they are different networks, PC-A sends the packet to its **default gateway: 10.0.0.1**.

### 2. PC-A Uses ARP

PC-A needs the MAC address of the gateway.

ARP resolves:

```text
10.0.0.1  →  R1 Fa0/0 MAC
```

The Ethernet frame is then sent to R1.

### 3. Router Checks the Routing Table

R1 receives the packet and checks the **destination IP: 20.0.0.2**.

Its routing table contains:

```text
C  10.0.0.0/24  → Fa0/0
C  20.0.0.0/24  → Fa0/1
```

R1 knows that `20.0.0.0/24` is directly connected through **Fa0/1**.

### 4. Router Forwards the Packet

R1 uses ARP to find PC-D's MAC address if it is not already known.

The new Ethernet frame is:

```text
Source MAC      : R1 Fa0/1 MAC
Destination MAC : PC-D MAC

Source IP       : 10.0.0.2
Destination IP  : 20.0.0.2
```

R1 then forwards the packet through **Fa0/1** to PC-D.

## Important Concept

At each router hop:

- **Routing decision is based on the destination IP address.**
- **MAC addresses change from one network segment to the next.**
- **IP addresses normally remain the same end-to-end** (unless NAT is used).
- **ARP maps an IP address to a MAC address on the local network.**

### Simple Flow

```text
PC-A
  ↓
Default Gateway
  ↓
Routing Table Lookup
  ↓
Select Outgoing Interface
  ↓
ARP for Next-Hop MAC
  ↓
Forward Packet
  ↓
PC-D
```

## Useful Cisco Commands

```bash
show ip route
show ip arp
show ip interface brief
```

Test connectivity:

```bash
ping 20.0.0.2
```

## Key Takeaway

```text
Destination IP → Routing Table → Outgoing Interface
                         ↓
                    ARP → MAC Address
                         ↓
                    Forward Packet
```

A router connects different IP networks and forwards packets by selecting the appropriate path based on the **destination IP address**.
