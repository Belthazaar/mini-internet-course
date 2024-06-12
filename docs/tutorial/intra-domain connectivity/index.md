---
layout: default
title: Intra-Domain Connectivity
nav_order: 1
has_children: true
parent: Tutorial
---

# Intra-Domain Connectivity

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

Tutorial to [configure a host.](configure-hosts.md)

[configure-hosts]: configure-hosts.md

## Configure the VLANs

In order to separate the hosts, you will need to configure the switches to
use VLANs. Configure the switches in the North Data Center to use VLAN 20
for the FIFA hosts and VLAN 30 for the UEFA hosts.