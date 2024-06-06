---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults
title: PacNOG 33 - Mini-Internet Demo

layout: default
---

# PacNOG 33 - Mini-Internet Demo

## Instructions

TODO: Add links to tutorials and instructions

## Monitoring

TODO: Add links to monitoring web page

## Network Diagram

TODO: Add network diagram

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

- AS 4
- AS 13
- AS 14
- AS 15
- As 16
- AS 17
- AS 18