---
layout: default
title: OSPF
nav_order: 4
parent: FRRouting
grand_parent: Tutorial
---

# Configuring OSPF

OSPF routers flood IP routes over OSPF adjacencies. FRR routers continuously
(and automatically) probe any OSPF-enabled interface to discover new neighbors
to establish adjacencies with. By default, FRR router will activate OSPF on
any interface whose prefix is covered by a `network` command under the
`router ospf` part of the configuration. For instance, the following
configuration would activate OSPF on any interface whose IP address falls under
`1.0.0.0/24` or `2.0.0.0/24`:

```
router# conf t
router(config)# router ospf
router(config)# network 1.0.0.0/24 area 0
router(config)# network 2.0.0.0/24 area 0
```

OSPF has scalability issues when there is a large number of routers. To make it
more scalable, the router topology can be hierarchically divided in what is
called "areas". In this assignment, your network is small and you do not need
more than one area: you will only use area 0.

To check the OSPF neighbors of a router, you can use the following command:

```
router# show ip ospf neighbor
    Neighbor    ID  Pri State Dead Time   Address Interface   RXmtL   RqstL   DBsmL
    1.0.0.2 1   Full/Backup     1.0.0.2 port_BASE:1.0.0.1   0   0   0
    2.0.0.2 1   Full/Backup     2.0.0.2 port_LUGA:2.0.0.1   0   0   0
```

> The `Neighbor` may be a different IP than you expect since it is an ID that may come from
any interface, e.g. also the `ssh` interface. Do not worry about this.


Here, we see that the router has established two OSPF sessions with two
neighbors. The first one is connected via the interface `1.0.0.1` and its IP is
`1.0.0.2`. The second one is connected via the interface `2.0.0.1` and its IP is
`2.0.0.2`. Now that you are connected to two routers with OSPF, they can send you
information about the topology of the network. Let's take at look at the routes
received by OSPF with the following command.

```
router# show ip route ospf
O   1.0.0.0/24 [110/10] is directly connected, port_BASE, 07:09:33
O   2.0.0.0/24 [110/10] is directly connected, port_LUGA, 06:14:24
O>* 10.104.0.0/24 [110/20] via 2.0.0.2, port_LUGA, 00:00:10
```

You can see that our router has learned how to reach the subnet `10.104.0.0/24`.
The `O` at the beginning of each line indicates that the router has
learned this subnet from OSPF. To reach it, it must send the packets to its
neighbor router `2.0.0.2`. If you want to have more information about the routers
of this OSPF area, you can use `show ip ospf database`.

With OSPF, each link between two routers has a specific weight, and only the
shortest paths are used to forward packets. Below is an example of how you set
the weight of a link connected to interface `INTERFACENAME` to 900:

```
router# conf t
router(config)# interface INTERFACENAME
router(config-if)# ip ospf cost 900
```

Use these commands to configure all the OSPF weights in your own network.
