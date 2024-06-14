---
layout: default
title: Configuring a host
parent: Tutorial
nav_order: 1
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
ip adddress add 111.0.222.3/24 dev BASErouterOn
```

To remove an IP address, you can use the following command:

```bash

ip address del IP/SUBNET_SIZE dev INTERFACENAME
```

At this point, the host knows how to reach all the IP addresses with its own subnet.
However, it does not know how to reach IP addresses outside their subnet.
Hence, you normally add a route towards a default gateway at the house uses
to reach all unknown IP addresses.
To do that use the following command:

```bash
ip route add default via IP_ADDRESS
```

Continuing with our previous example:

```bash
ip route add default via 111.0.222.1
```

To see the current default gateway, use the following command:

```bash
ip route show
# or
netstat -rn
```

To delete past entries, use:

```bash
ip route del default via IP_ADDRESS
```