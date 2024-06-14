---
layout: default
title: Data Center Connectivity
nav_order: 1
parent: Instructions
---

# Data Center Connectivity

The first step to setting up your AS is to enable direct layer-2
connectivity in the North Data Center.
The goal is to allow direct connectivity between the FIFA hosts, between
the UEFA hosts, but not directly between the FIFA and UEFA hosts.
Yet the hosts should still be able to communicate between themselves, but
via a layer-3 router.

## Configure IP addresses and default gateways
Configure the hosts within the North Data Center using the subnet X.200.0.0/23,
where X is the AS number. You will also need to configure the interfaces of
the routers ZURI and BASE connected to the Data Center.

For the default gateway, configure it so that all the hosts use `ZURI` as the default gateway.

{: .highlight }
Tutorials on how to [configure a host](../tutorial/configure-host) and how
to [configure a router](../tutorial/routing/index).


## Configure the VLANs

In order to separate the hosts, you will need to configure the switches to use VLANs.
Configure the switches in the North Data Center to use VLAN 20 for the
FIFA hosts and VLAN 30 for the UEFA hosts.
To ensure that the hosts can communicate with each other, you will need to
configure both access and trunk ports.

{: .highlight }
Tutorial to [configure the switches](configure-switches.md) including
how to configure the VLANs.

👉 The interface of `ZURI` connected to the North Data Center connected to VLAN 10
is named `ZURI-L2.10` and the interface connected to VLAN 20 is named `ZURI-L2.20`.
You can see them using the command `show interface brief` in the FRRouting CLI.
The same name convention applies to the interfaces of `BASE`.

# Related Tutorials

- [Configuring a host](../tutorial/configure-host)
- [Configuring switches](../tutorial/configure-switches)
- [FRRrouting CLI](../tutorial/routing/FRRrouting_cLI)
- [FRRouting configure interfaces](../tutorial/routing/routerinterfaces)
- [FRRouting configure static routes](../tutorial/routing/static_routes)