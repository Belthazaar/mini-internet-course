---
layout: default
title: FRRrouting CLI
nav_order: 1
parent: FRRouting
grand_parent: Tutorial
---

# The FRRouting CLI
We now briefly describe how to configure a FRR router

> This is a very short FRR introduction, we strongly recommend you to take a look at the
[official documentation](http://docs.frrouting.org/en/latest/) to get more information.

The following paragraphs describe the autocompletion features of the CLI,
how to test connectivity, how to switch to configuration mode,
and how to remove a command/reverse  your configuration actions.

## Autocompletion features

When you enter the FRR CLI, you see the following line:

```
router#
```

At any time in the CLI, you can type `?` to see all the
possible commands you can currently type (some of the shown commands are):

```
router# ?
    clear       Reset functions
    configure   Configuration from vty interface
    exit        Exit current mode and down to previous mode
    no          Negate a command or set its defaults
    ping        Send echo messages
    quit        Exit current mode and down to previous mode
    show        Show running system information
    traceroute  Trace route to destination
    write       Write running configuration to memory, network, or terminal
```

For example, the command `show` will print various snapshots of the
router state. To see what kind of information can be shown, just type
`show ?`. For example, `show running-config` will print the
running configuration.

>The interfaces are not visible in the output
at the beginning, but will start appearing as soon as you have configured them.

You can shorten the commands when there is no possible
ambiguity. For instance `show run` is equivalent to `show running-config`.
Similarly to the Linux terminal, you can also use
auto-completion by pressing the tabulator key.


## Testing connectivity

If you want to test your connectivity, you can use `ping` or `traceroute`
from the CLI of the routers. However, whenever it is possible, always prefer
to run the `ping` and `traceroute` from the hosts because they can use DNS whereas the routers cannot.

:bulb: Recall that you can also run `traceroute` from remote ASes using the
[measurement container](../../1.-Assignment/1.4-Tools-to-help-you#measurement-container).

## Switching to configuration mode

To configure your router, you must enter the configuration mode with `configure terminal` (`conf t`
for the short version). You can verify that you are in the configuration mode
by looking for the _config_ prefix in your CLI prompt. Use `exit` to leave the configuration mode and to go back to the previous mode.
Commands that work *in* configuration mode do not (necessarily) work *outside*
configuration mode and vice versa. Notably, commands starting with `show`
do not work in configuration mode.

## Undo a command

If you want to delete parts of the configuration,
you can prefix the command you want to remove with `no`. For example, if you add an IP address to an interface with the command

```
ip address 1.0.0.1/24
```

(explained in more detail in the next section of the tutorial), you can simply run

```
no ip address 1.0.0.1/24
```

to remove the IP address again.
