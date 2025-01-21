---
layout: default
title: Router Interfaces
nav_order: 2
parent: FRRouting
grand_parent: Tutorial
---

# Configuring Router Interfaces

This tutorial will guide you through the process of configuring router interfaces in FRRouting.

## Naming conventions

Each interface to its neighboring router follows the naming convention `port_NEIGBOR`,
where `NEIGHBOR` is the name of the neighboring router.
For example, the interface connecting `SING` to `SYDN` is named `port_SYDN`.
Each router has an interface connected to it's host named `host` and a loopback
interface named `lo`.
An interface connected to another AS is called `ext_ASNUMBER_ROUTERNAME`.
For example, the interface on `BANK` in AS 88 connected to `MUMB` in AS 86 has the name `ext_86_MUMB`.

## Show the current configuration

To get an overview of the interfaces, use the command

```bash
router# show interface
```

or for a brief overview

```bash
router# show interface brief
```

You can get information for one specific interface with

```bash
router# show interface INTERFACENAME
```

> Please ignore the interface `sit0`, it comes by default when enabling IPv6 on linux, but we don't need it.

### Changing interface configurations

You must first enter the configuration mode, and then specify the name of the
interface you want to configure:

```bash
router# conf t
router(config)# interface INTERFACENAME
router(config-if)# ip address 1.0.0.1/24
```

You can verify that the running configuration has been updated correctly with
the command `show run`.

⚠️ Do **not** configure two different IP addresses
on one interface at the same time. If you have configured a wrong IP address,
first remove the address with the `no` command and then configure a new IP address:

```bash
router# conf t
router(config)# interface INTERFACENAME
router(config-if)# no ip address 1.0.0.1/24
router(config-if)# ip address 2.0.0.1/24
```

Once you have configured an IP address and a subnet on an interface, the router
knows that packets with a destination IP in this subnet must be forwarded to
this interface. To show the subnets that are directly connected to your router,
you can use the following command.

```bash
router# show ip route connected
C>* 2.0.0.0/24 is directly connected, INTERFACENAME
```

We see that `2.0.0.0/24` is directly connected and reachable with the interface
`INTERFACENAME`. At this stage, a packet with a destination IP that is not in
a directly connected subnet will be dropped. If you want your router to know
where to forward packets with an IP destination in a remote subnet, you must
use routing protocols, such as OSPF or BGP (explained in the following tutorial sections).
