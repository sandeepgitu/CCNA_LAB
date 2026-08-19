# Cisco Switch Port Security

## Overview

Port Security is a Cisco switch security feature used to control which source MAC addresses are allowed to communicate through a switch interface.

It can restrict access to a switch port by allowing only specific MAC addresses or by limiting the maximum number of MAC addresses learned on the port.

In this lab, Port Security is configured on Switch0 FastEthernet0/1 with the violation mode set to shutdown.

## Why Port Security Is Used

Port Security is commonly used to prevent unauthorized devices from connecting to a switch access port.

Common use cases include:

- Enterprise access networks
- Office user access ports
- Server access ports
- Classrooms and computer labs
- Small office networks
- Sensitive network segments
- Environments where only approved devices should connect

## Features

- Restricts MAC addresses allowed on an interface
- Limits the maximum number of secure MAC addresses
- Supports manually configured secure MAC addresses
- Supports dynamically learned secure MAC addresses
- Supports sticky MAC addresses
- Supports multiple violation modes
- Can automatically disable a port after a security violation
- Helps prevent unauthorized devices from accessing an access port

## Port Security Requirements

Port Security is normally configured on Layer 2 access ports.

Example:

```text
interface fa0/1
switchport mode access
switchport port-security
```

## Port Security Violation Modes

Cisco Port Security provides three common violation modes.

### Shutdown

This is the default violation mode on many Cisco platforms.

When an unauthorized MAC address is detected:

- Unauthorized traffic is dropped
- The interface enters an error-disabled state
- The port stops forwarding traffic
- The security violation counter is incremented
- The interface must be recovered before normal communication resumes

Configuration:

```text
interface fa0/1
switchport port-security violation shutdown
```

### Restrict

When an unauthorized MAC address is detected:

- Unauthorized traffic is dropped
- The interface remains operational
- The security violation counter is incremented
- Notification messages may be generated depending on the platform

Configuration:

```text
interface fa0/1
switchport port-security violation restrict
```

### Protect

When an unauthorized MAC address is detected:

- Unauthorized traffic is dropped
- The interface remains operational
- The port is not placed into an error-disabled state
- Violation counters and notifications are more limited than Restrict

Configuration:

```text
interface fa0/1
switchport port-security violation protect
```

## Violation Mode Comparison

| Violation Mode | Unauthorized Traffic | Port State | Violation Counter | Error-Disabled |
|---|---|---|---|---|
| Shutdown | Dropped | Down | Incremented | Yes |
| Restrict | Dropped | Up | Incremented | No |
| Protect | Dropped | Up | Limited | No |

## Basic Port Security Configuration

The following configuration enables Port Security on FastEthernet0/1 and allows one secure MAC address.

```text
enable
configure terminal

interface fa0/1
switchport mode access
switchport port-security
switchport port-security maximum 1
switchport port-security violation shutdown
switchport port-security mac-address 000D.BC41.AB44
exit

end
write memory
```

## Using Sticky MAC Addresses

Sticky MAC allows the switch to dynamically learn MAC addresses and add them as secure MAC addresses.

Example:

```text
enable
configure terminal

interface fa0/1
switchport mode access
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation shutdown
exit

end
write memory
```

The switch learns MAC addresses from traffic received on the interface until the configured maximum number of secure MAC addresses is reached.

## How to Add Multiple MAC Addresses on One Interface

A single switch interface can allow multiple secure MAC addresses.

First configure the maximum number of MAC addresses:

```text
interface fa0/1
switchport mode access
switchport port-security
switchport port-security maximum 2
switchport port-security violation shutdown
```

Then configure the allowed MAC addresses:

```text
switchport port-security mac-address 000D.BC41.AB44
switchport port-security mac-address 0000.C49C.C141
```

The complete configuration can look like:

```text
interface FastEthernet0/1
switchport mode access
switchport port-security
switchport port-security maximum 2
switchport port-security violation shutdown
switchport port-security mac-address 000D.BC41.AB44
switchport port-security mac-address 0000.C49C.C141
```

This configuration allows both MAC addresses on FastEthernet0/1.

The maximum value must be equal to or greater than the number of configured secure MAC addresses.

For example:

```text
2 MAC addresses = maximum 2
3 MAC addresses = maximum 3
```

## How to Check Whether Port Security Is Enabled

Use:

```text
show port-security interface fa0/1
```

Look for:

```text
Port Security              : Enabled
```

You can also check the running configuration:

```text
show running-config interface fa0/1
```

Look for:

```text
switchport port-security
```

To see Port Security information for all interfaces:

```text
show port-security
```

## How to Check the Violation Mode

Run:

```text
show port-security interface fa0/1
```

Look for:

```text
Security Violation Mode
```

The output will normally show:

```text
Shutdown
```

or:

```text
Restrict
```

or:

```text
Protect
```

Example:

```text
Port Security              : Enabled
Port Status                : Secure-up
Violation Mode             : Shutdown
Maximum MAC Addresses      : 2
Total MAC Addresses        : 2
Configured MAC Addresses   : 2
Security Violation Count   : 0
```

The important fields are:

```text
Port Security              : Enabled
Violation Mode             : Shutdown
Maximum MAC Addresses      : 2
Security Violation Count   : 0
```

## How to Check Secure MAC Addresses

Use:

```text
show port-security address
```

This displays secure MAC addresses configured or learned by the switch.

For interface-specific information:

```text
show port-security interface fa0/1
```

## How to Check the Current Interface Status

Use:

```text
show interfaces fa0/1 status
```

or:

```text
show interfaces fa0/1
```

If Port Security with Shutdown mode detects an unauthorized MAC address, the interface may enter an error-disabled state.

## How to Test a Port Security Violation

In this lab:

PC0 MAC address:

```text
000D.BC41.AB44
```

PC0 IP address:

```text
10.10.10.1
```

Laptop0 MAC address:

```text
0000.C49C.C141
```

Laptop0 IP address:

```text
10.10.10.2
```

Assume Fa0/1 is configured to allow only PC0:

```text
interface fa0/1
switchport mode access
switchport port-security
switchport port-security maximum 1
switchport port-security mac-address 000D.BC41.AB44
switchport port-security violation shutdown
```

PC0 should communicate normally because its MAC address is authorized.

If Laptop0 replaces PC0 on Fa0/1, the switch detects the unauthorized MAC address.

Because the violation mode is Shutdown, Fa0/1 enters the error-disabled state.

## Recovering a Port After a Shutdown Violation

If the interface is placed into the error-disabled state because of Port Security, first remove the unauthorized device or correct the Port Security configuration.

Then recover the interface:

```text
enable
configure terminal

interface fa0/1
shutdown
no shutdown
exit

end
```

## Automatic Error-Disabled Recovery

Depending on the Cisco IOS/platform, automatic recovery can be configured:

```text
enable
configure terminal
errdisable recovery cause psecure-violation
errdisable recovery interval 30
end
```

The switch will attempt to recover the interface after the configured interval.

## Useful Verification Commands

```text
show port-security
```

Displays Port Security information for interfaces.

```text
show port-security interface fa0/1
```

Displays detailed Port Security information for Fa0/1.

```text
show port-security address
```

Displays secure MAC addresses.

```text
show running-config interface fa0/1
```

Displays the current configuration of Fa0/1.

```text
show interfaces fa0/1
```

Displays detailed interface information.

```text
show interfaces status
```

Displays the status of switch interfaces.

## Important Port Security Concepts

### Maximum MAC Address

Defines how many secure MAC addresses can be allowed on an interface.

Example:

```text
switchport port-security maximum 2
```

This allows up to two secure MAC addresses.

### Static Secure MAC

A specific MAC address is manually configured and permitted on the interface.

Example:

```text
switchport port-security mac-address 000D.BC41.AB44
```

### Sticky Secure MAC

The switch dynamically learns MAC addresses and treats them as secure addresses.

Example:

```text
switchport port-security mac-address sticky
```

### Violation Mode

Defines what happens when an unauthorized MAC address is detected.

The available modes are:

```text
shutdown
restrict
protect
```

## Advantages

- Prevents unauthorized devices from using protected switch ports
- Reduces the risk of unauthorized network access
- Controls the number of devices connected to an access port
- Provides automatic response to MAC address violations
- Supports static and dynamically learned secure MAC addresses
- Helps protect access-layer switch ports
- Provides security violation counters
- Supports different security responses based on network requirements

## Lab Topology

The lab contains:

```text
PC0
MAC: 000D.BC41.AB44
IP: 10.10.10.1
        |
        | Fa0
        |
      Fa0/1
     Switch0
      Fa0/2
        |
        |
       PC1
IP: 10.10.10.3
```

Laptop0 has:

```text
MAC: 0000.C49C.C141
IP: 10.10.10.2
```

If PC0 is the authorized device on Fa0/1 and Laptop0 is connected instead, Laptop0 will cause a Port Security violation.

With Shutdown mode, Fa0/1 will enter the error-disabled state.

## Conclusion

Cisco Port Security is an important access-layer security feature used to control which devices can communicate through a switch interface.

Shutdown mode provides a strong response because the interface is placed into an error-disabled state when an unauthorized MAC address is detected.

Multiple approved devices can be allowed on a single interface by increasing the maximum secure MAC address value and configuring multiple secure MAC addresses.

Sticky MAC learning can also be used when manually configuring every MAC address is not practical.
