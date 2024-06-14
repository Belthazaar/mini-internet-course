---
layout: default
title: Static Routes
nav_order: 3
parent: FRRouting
grand_parent: Tutorial
---

# Configuring Static Routes

OSPF and BGP are dynamic routing protocols that automatically update routing
tables based on network changes.
However, sometimes it is necessary to manually configure static routes.
This tutorial will show you how to configure static routes in FRRouting.

For example:

```bash
router# conf t
router(config)# ip route 3.0.0.0/24 2.0.0.2
router(config)# ip route 4.0.0.0/24 someinterface
```

Would force the router to direct all traffic destined to `3.0.0.0/24` to
`2.0.0.2`, and all traffic to `4.0.0.0/24` to the interface `someinterface`.
You can verify that a static route has been added by using the
command `show ip route static`:

```bash
router# show ip route static
S   3.0.0.0/24 [1/0] via 2.0.0.2 inactive
```