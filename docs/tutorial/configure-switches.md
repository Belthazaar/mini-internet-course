---
layout: default
title: Configuring Open vSwitch
parent: Tutorial
nav_order: 2
---

# Configuring Open vSwitch

Open vSwitch is one of the most popular software switches.
It is typically used in virtual environments for instance, connecting to
virtual machines and gives more flexibility compared to the normal Linux bridge.

The following section explains how you can use and configure the switches in your mini-Internet.

## Naming conventions

Each switch has one port named `br0` and the corresponding interface has type internal.
This is a local port used by the host to communicate with the switch.
You do not need to use this port.
The ports to FIFA and UEFA hosts follow the following name pattern, `X-FIFA_i` and `X-UEFA_i`
respectively, with X being the AS number and i being the host number.
For example, the port to the first FIFA host in AS 88 is `88-FIFA_1`.
Ports to other switches have the name `X-SWITCHNAME`, e.g., `88-S3` for the port to `S3` in AS 88.
If there is a port to a router, it is called `ROUTERNAMErouter`,
e.g., `BASErouter` for the port to the `BASE` router.

## Show the current configuration

### Brief switch overview

To print a brief overview of the switch, use the following command:

```bash
ovs-vsctl show
```

This command will also show you which VLANs each port is a member of.

### Port Status

To get more detailed information about the ports, use the following command:

```bash
ovs-ofctl show br0
```

### Current configuration and statistics

To get some statistics about the switch, as well as the configuration
of the ports, use the following command:

```bash
ovsdb-client dump
```

## Changing the VLAN configuration

To add a port to a VLAN, use the following command:

```bash
ovs-vsctl set port PORT tag=10
```
This will add the port `PORT` to VLAN 10.
Since the `PORT` is in only one VLAN, the port is an access port.
Should you add a port to several VLANs, the port is a trunk port.
To do that use the following command:

```bash
ovs-vsctl set port PORT trunks=10,20
```

The port `PORT` is now a trunk port and is a member of VLAN 10 and 20.\
To clear the VLAN configuration on a port (e.g., the trunk port), use the following command:

```bash
ovs-vsctl clear port PORT trunks
```