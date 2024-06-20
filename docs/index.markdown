---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults
title: PacNOG 33 - <nobr>Mini-Internet Demo</nobr>

layout: default
nav_order: 1
---

# PacNOG 33 — <nobr>Mini-Internet Demo</nobr>

## Instructions

Instructions for the mini-Internet demo can be found in the [instructions](instructions/index) section.

The related tutorials can be found in the [tutorial](tutorial/index) section.

## Monitoring

<!-- TODO: Add links to monitoring web page -->

Monitoring site as part of the mini-Internet demo can be found here.

## Network Topology

### AS Topology

The mini-Internet demo consists of 23 ASes in total, that consists of 2 regions
connected to each other via 3 IXPs.
The topology consists of the following types of ASes:

| AS Numbers | Type | Description |
|-----------|------|-------------|
| 1, 2, 11, 12 and 9, 10, 19, 20 | Tier 1 and Stub ASes | ASes used by TAs to help debugging |
| 7 | Layer 2 configured AS | AS that have Layer 2 DCs configured |
| 5 | Intranet configured AS | AS that are configured up to and including iBGP |
| 6, 8 | Fully configured AS | ASes that are fully configured |
| 3 | Demo AS | AS used for demo purposes |
| 4, 13, 14, 15, 16, 17, 18 | Configurable AS | ASes that can be configured by participants |

#### AS diagram
<br>
![example_topology.png](images/example_topology.png)
<br>
### Layer 3 Topology

Every AS (except the TA ASes) have 2 data centers (DCs) and 8 routers.
Each AS has four connections to other ASes, and one to an IXP.
Every AS has been allocated one /8 IPv4 prefix that it can use internally.
If you are using AS X, you can use the X.0.0.0/8 prefix for your internal network.
For example AS 13 has the prefix 13.0.0.0/8.
Finally, each router has one host connected to it, however not shown in
the figure for simplicity.

![l3-network.png](images/l3-network.png)

### Layer 2 Topology

Finally, the Layer 2 topology consists of 2 data centers in each AS.
The North DC comprises three switches, and the South DC comprises one switch.
The hosts in Data Centers belong to two customers, FIFA and UEFA.
Each switch is connected to one FIFA hose and one UEFA host.

![l2-network.png](images/l2-network.png)

## Access:

Click on the AS that you want to connect to.
It will take you to a terminal window where you can connect to the proxy container.
Please refer to the presentation slides for the credential details.

in the proxy container you can use the `goto.sh` helper script to connect to the nodes within the AS.
Example:

```bash
# Connect to the router
./goto.sh LUGA router
# Connect to a switch in the North DC
./goto.sh DCN S2
# Connect to a host in the North DC
./goto.sh DCN FIFA_1
# COnnect to the host connected to a router
./goto.sh LUGA host
```


The following AS numbers are available for use in the lab:

| Region 1     | Region 2       | Region 3       | Region 4       | Region 5       |
| ------------ | -------------- | -------------- | -------------- | -------------- |
| [AS 3](/as3) | [AS 13](/as13) | [AS 23](/as23) | [AS 33](/as33) | [AS 43](/as43) |
| [AS 4](/as4) | [AS 14](/as14) | [AS 24](/as24) | [AS 34](/as34) | [AS 44](/as44) |
| [AS 5](/as5) | [AS 15](/as15) | [AS 25](/as25) | [AS 35](/as35) | [AS 45](/as45) |
| [AS 6](/as6) | [AS 16](/as16) | [AS 26](/as26) | [AS 36](/as36) | [AS 46](/as46) |
| [AS 7](/as7) | [AS 17](/as17) | [AS 27](/as27) | [AS 37](/as37) | [AS 47](/as47) |
| [AS 8](/as8) | [AS 18](/as18) | [AS 28](/as28) | [AS 38](/as38) | [AS 48](/as48) |
