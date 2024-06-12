---
layout: default
title: Configuring a host
parent: Intra-Domain Connectivity
grand_parent: Tutorial
---

# Configuring a host

This section guides you through the process of configuring the various hosts in your mini-Internet.

## Nameing conventions

The interface to the router is called `router-name`router, e.g. GENErouter for GENE.

## Show the current configuration

To show the current configuration for the host, use the following command:

```bash
ip address show
```

## Changing the configuration

To change the IP address to an interface on the host, use the following command:

```bash
ip address add IP/SUBNET_SIZE dev INTERFACENAME
```

For example:

```bash
ip adddress add 111.0.222.3/24 dev BASErouter
```
