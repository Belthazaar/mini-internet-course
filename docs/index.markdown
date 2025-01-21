---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults
title: IIJ Research Lab- <nobr>Mini-Internet Demo</nobr>

layout: default
nav_order: 1
---

# IIJ Research Lab — <nobr>Mini-Internet Demo</nobr>

## Instructions

Instructions for the mini-Internet demo can be found in the [instructions](instructions/index) section.

The related tutorials can be found in the [tutorial](tutorial/index) section.

## Monitoring


Monitoring site as part of the mini-Internet demo can be found [here](http://mini-internet.web.iijlab.net/matrix).
(Note this is only accessible from the internal NAT network using the internal DNS)

## Network Topology

### AS Topology

The mini-Internet demo consists of 23 ASes in total, that consists of 2 regions
connected to each other via 3 IXPs.
The topology consists of the following types of ASes:

| AS Numbers | Type | Description |
|-----------|------|-------------|
| x1, x2, and x9, x0 | Tier 1 and Stub ASes | ASes used by TAs to help debugging |
| x3, x4, x5, x6, x7, x8 | Configurable AS | ASes that can be configured by participants |

**Note:** Replace X with region numbers

#### AS diagram

<br>
![example-topology.svg](images/example-topology.svg)
<br>
### Layer 3 Topology

Every AS (except the TA ASes) have 2 data centers (DCs) and 8 routers.
Each AS has four connections to other ASes, and one to an IXP.
Every AS has been allocated one /8 IPv4 prefix that it can use internally.
If you are using AS X, you can use the X.0.0.0/8 prefix for your internal network.
For example AS 13 has the prefix 13.0.0.0/8.
Finally, each router has one host connected to it, however not shown in
the figure for simplicity.

![l3-network.svg](images/l3-network.svg)

### Layer 2 Topology

Finally, the Layer 2 topology consists of 2 data centers in each AS.
The North DC comprises three switches, and the South DC comprises one switch.
The hosts in Data Centers belong to two customers, WIN and MAC.
Each switch is connected to one WIN host and one MAC host.

![l2-network.svg](images/l2-network.svg)

## Access

You can ssh into the proxy container, from within the NAT segment, for a group using the following command:

```bash
ssh 172.16.30.30 -p $GROUP_NUMBER
```

Check with Chris for a group number and the password.
Once you have access, feel free to copy over your ssh keys to the proxy container.

in the proxy container you can use the `goto.sh` helper script to connect to the nodes within the AS.
Example:

```bash
# Connect to the router
./goto.sh TOKY router
# Connect to a switch in the North DC
./goto.sh DCN S2
# Connect to a host in the North DC
./goto.sh DCN WIN_1
# Connect to the host connected to a router
./goto.sh TOKY host
```
