# Static Routing -- Cisco Packet Tracer

## Overview

This lab demonstrates **static routing** between three Cisco routers
(R1, R2, R3) and three LANs.

The objective is to configure static routes so that all PCs can
communicate with remote networks through the appropriate next-hop
router.

## Topology

``` text
PC0 ── R1 ── R2 ── R3 ── PC2
       │      │      │
    10.0.0.0 20.0.0.0 30.0.0.0
```

## IP Addressing

  Device   Interface   IP Address     Network
  -------- ----------- -------------- -------------
  PC0      Fa0         10.0.0.2/24    10.0.0.0/24
  R1       Eth0/0      10.0.0.1/24    10.0.0.0/24
  R1       Eth1/0      100.0.0.1/30   R1--R2
  R2       Eth1/0      100.0.0.2/30   R1--R2
  R2       Eth0/0      20.0.0.1/24    20.0.0.0/24
  PC1      Fa0         20.0.0.2/24    20.0.0.0/24
  R2       Eth2/0      200.0.0.1/30   R2--R3
  R3       Eth1/0      200.0.0.2/30   R2--R3
  R3       Eth0/0      30.0.0.1/24    30.0.0.0/24
  PC2      Fa0         30.0.0.2/24    30.0.0.0/24

## Static Route Configuration

### R1

``` text
ip route 20.0.0.0 255.255.255.0 100.0.0.2
ip route 30.0.0.0 255.255.255.0 100.0.0.2
ip route 200.0.0.0 255.255.255.252 100.0.0.2
```

### R2

``` text
ip route 10.0.0.0 255.255.255.0 100.0.0.1
ip route 30.0.0.0 255.255.255.0 200.0.0.2
```

### R3

``` text
ip route 10.0.0.0 255.255.255.0 200.0.0.1
ip route 20.0.0.0 255.255.255.0 200.0.0.1
ip route 100.0.0.0 255.255.255.252 200.0.0.1
```

## Verification

Use the following commands on each router:

``` text
show ip interface brief
show ip route
show ip route static
```

Test end-to-end connectivity from the PCs:

``` text
PC0> ping 20.0.0.2
PC0> ping 30.0.0.2
PC1> ping 10.0.0.2
PC1> ping 30.0.0.2
PC2> ping 10.0.0.2
PC2> ping 20.0.0.2
```

## Key Learning

-   Configured **static routes** using next-hop IP addresses.
-   Understood how routers forward traffic to remote networks.
-   Used `/30` subnets for point-to-point router links.
-   Used `/24` subnets for LAN networks.
-   Verified routing tables and end-to-end connectivity.

**Tool:** Cisco Packet Tracer
