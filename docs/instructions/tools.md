---
layout: default
title: Tools to help
nav_order: 4
parent: Instructions
---

# Tools to help you

As any network operator, you must verify that your configuration does what you
want, and debug it in case something goes wrong. We offer you several tools that
you can use to verify your configuration. These tools are either incorporated in the mini-Internet
or are made available on the mini-Internet website.
These tools are similar to the ones network operators use in practice.

## Built-in tools

### Measurement container
We have setup a measurement container which will enable you
to launch `traceroutes` from any transit AS (and not
necessarily only your own AS), towards any destination in the mini-Internet. This will
help you to know the paths used _towards_ your network.
The measurement container is connected to each AS via the interface `measurement_X` of
the router `MILA`. The IP address of this interface is pre-configured and follows
the convention `X.0.199.1/24` (see the [l3-network](1.2-Your-mini-Internet#l3-topology) network description),
with X your group number. For example if you are group 63, your pre-configured IP address on the interface
`measurement_63` at `MILA` will be `63.0.199.1/24`.
The `X.0.199.1/24` subnet must be reachable from anywhere in your network.

❗️**You must therefore add it in your OSPF configuration**.

To access the measurement container, use the webtty

<!-- TODO: Add webtty for measurement container -->
- Measurement Container access

You can find the password in your GitLab repository.
To launch a traceroute, you can use the script
`launch_traceroute.sh`, which takes as argument the group number from
which the traceroute starts, and the destination IP address (possibly in another AS).
For instance, if you want to perform a traceroute from group 11 to `22.107.0.1`
(_i.e.,_ the host connected to `VIEN` in group 22), just use the following command
in the measurement container:

```
./launch_traceroute.sh 11 22.107.0.1
```

Note that the traceroute will start from the router `MILA` of group 11, since
the measurement container is connected to that router.
In practice, network operators can use large-scale Internet measurement platforms
such as [RIPE Atlas](https://atlas.ripe.net) to assess the connectivity
of their network from outside.

⚠️ Recall that you can only launch traceroutes from transit ASes and not Tier1 and or Stub ASes.

### DNS service

To help you decoding your traceroute output, we have setup a
DNS server and have pre-configured your hosts to use it.
If the DNS server is used, the IP addresses in the traceroute output will be replaced
by the corresponding router names.
For example, `12.0.2.2` will
be translated into `GENE-ZURI.group12`, because this is the IP address configured
on the interface `port_ZURI` of the router `GENE` in AS12.
The DNS server is located in a container connected to the interface `dns` of the
router `ZURI`. The IP address on that interface is pre-configured, you do not need
to modify it. Also, each host is pre-configured to use the DNS server.
Only hosts use the DNS server, routers do not. Of course, hosts can only
use the DNS server if they can reach the network `198.0.0.0/24` (where the DNS server
is located).

❗️**As such, do not forget to include this network in your OSPF configuration**.

## The mini-Internet website

<!-- TODO: Add new pacnog site -->
<!-- The mini-Internet [website](http://duvel.ethz.ch), available at `http://duvel.ethz.ch`, offers the following monitoring tools. -->

### Connectivity matrix
We also provide a connectivity matrix that shows you whether two ASes can ping between each other.
Before you setup the eBGP sessions, everything will be red.
As soon as you will setup the eBGP sessions with your neighbors,
the matrix will turn orange (if the inter-AS path is invalid)
or green (if the inter-AS path is valid) for some pairs of ASes. At the end of this assignment,
we hope to see this matrix completely green!
The matrix is updated every few minutes.

### BGP looking glass
In practice, looking glasses are servers remotely accessible which display the routing information
of an IP router. For example, SWITCH, the Swiss educational network, gives public
access to its [looking glass](https://www.switch.ch/network/tools/lookingglass/).
This is useful to see how your BGP advertisements look like from a remote point of view.
On our mini-Internet website (under the `looking glass` tab), we provide such a looking glass service for the mini-Internet.
Simply indicate a group number and a router name and the website will print the result of a `show ip bgp` executed on that router.
The looking glass output is updated every minute.

## BGP policy analyzer
To help you debug your BGP policy configurations, the looking glass page also shows the output of our BGP policy analyzer.
This analyzer automatically parses the looking glass files and infers the currently configured BGP policies by looking at which prefixes each AS advertises to its neighboring ASes.
The tool detects [Route Leaks](https://tools.ietf.org/html/rfc7908#section-2), which are, in a nutshell,
violations of the expected policies following the business agreements.
This information will help you to see if your configured policies are correct, and if not, will help you to debug.
