---
layout: default
title: BGP Policies
nav_order: 6
parent: FRRouting
grand_parent: Tutorial
---

# BGP Policies

Now that you have BGP running, you want to configure some BGP policies. For
example, you may want to prefer to send traffic to your cheapest provider.
You may also want to avoid sending traffic to one AS for a particular
prefix for security reasons. BGP offers several ways to configure such
policies. As we have seen in the lecture, the `LOCAL-PREF` attribute can help you to influence the
outbound routing decisions. To influence the inbound routing decisions, you can use `AS-Path` prepending,
a more specific prefix advertisement or the `MED` attribute (in case of multiple different connections to the same AS neighbor).
In FRR, BGP policies are defined using `route-maps`.
In the following section we describe how a FRR route-map is configured and applied to certain BGP sessions.
Then we look at BGP community values. Community values are a good way to mark certain BGP routes.
At the end of the section we present an example of a route-map using BGP community values.

## Route-maps

Route-maps enable you to express input and output filters
that will be applied immediately after a BGP route
has been received from a neighbor, or just before it is sent to a neighbor. Note that the route-maps are applied to BGP routes and _not_ to the actual traffic that passes your routers. A
route-map is composed of two parts: a `match` and a `set` part.
This can somehow be compared to the `If...Then...` statements in many
programming languages. The `match` part is a boolean predicate that
decides on which BGP routes the route-map applies the actions (usually, attribute
manipulation) defined in the `set` part.

### Match part

Let's take a look on what you can *match* on (most important options are shown):

```
router# conf t
router(config)# route-map MY_ROUTE_MAP permit 10
router(config-route-map)# match ?
    as-path
        Match BGP AS path list
    community
        Match BGP community list
    interface
        Match first hop interface of route
    ip
        IP information
    metric
        Match metric of route
    origin
        BGP origin code
    peer
        Match peer address
```

As you can see, you can match pretty much any attribute contained in a BGP
UPDATE including: the AS-PATH, community value, IP addresses or subnets, the peer IP address, etc.

Whenever you create a `route-map`, you must indicate if you want to `permit` or `deny` the input data. Intuitively, only permitted routes will go through the set of actions, while denied routes won't be considered further. If you have no match part, the set part is applied to all BGP routes.

### Set part

Now let's take a look on what you can do with the matched routes using the *set* part of the route-map (most important options are shown):

```
router# conf t
router(config)# route-map MY_ROUTE_MAP permit 10
router(config-route-map)set ?
    as-path
        Transform BGP AS-path attribute
    community
        BGP community attribute
    ip
        IP information
    metric
        Metric value for destination routing protocol
    local-pref
        BGP local preference path attribute
    origin
        BGP origin code
```

As described in the course, the set part enables you to modify any route
attribute. Among others, you can: set a local-preference, modify the AS-path
(to perform AS-path prepending), or set a community attribute.
Observe that these operations will only be done on the routes you have matched
with the `match` command if you used the `permit` clause.
A set clause can be empty, which is useful if you only want to filter certain routes (these which pass the match part).
Finally, you can also modify multiple route attributes in the same set clause. Just use multiple set commands.

#### Apply the route-map

Once you have defined a route-map, you need to apply it to a BGP session. As
mentioned before, a route-map can either be applied on incoming routes (routes received from a neighbor) or on outgoing routes (routes sent to a neighbor).
To apply the route-map you have to extend your [BGP configuration](./BGP). In the following example, the route-map `MY_ROUTE_MAP`
is applied to all incoming routes from neighbor `2.0.0.2`:


```
router# conf t
router(config)# router bgp 15
router(config-router)# neighbor 2.0.0.2 route-map MY_ROUTE_MAP in
```

If you use `out` instead of `in`, the route-map is applied to all outgoing routes instead.

### Multiple route-maps

You can only apply two route-maps to a single BGP neighbor, one in the `in` direction and one in the `out` direction. However, sometimes you might want to apply multiple actions at once. For example, for all routes matching a first subnet you want to change the local-preference and you want to drop all routes matching a second subnet. To achieve that, you can chain multiple route-maps together using their sequence numbers. The following pseudocode shows an example:

```
route-map MY_CHAIN permit 10
   match ...
   set ...

route-map MY_CHAIN permit 20
   match ...
   set ...
```

Note that both route-maps have the same name (`MY_CHAIN`) but different sequence numbers (10 and 20). The route-map with the lowest sequence number is executed first (10 in our example). As before you can now apply the route-map in the `in` or `out` direction for one of your BGP neighbors.


## BGP community values

A BGP community can be seen as a label or a tag
that can be attached to any route in the set part of a route-map and matched
against later on a different router. In practice, a community value is often written as two integers separated by a colon.
The first integer identifies the AS and the second one describes the value/purpose of the community value.
For example, if you are AS number 15 and you want to use the community value to tag certain BGP routes with the value `100`,
you could use the following BGP community value: `15:100`. You can also add more than one BGP community value to a route.

If you want to use community values in the match part of a route-map, you need to use a so called `community list`.
To define a community-list matching on the community value from before (`15:100`), you can use the following command:

```
router# conf t
router(config)# bgp community-list 1 permit 15:100
```

The community list has the name/number `1`.

### Example

Let's assume you have a router which has two eBGP connections to two different
neighbors with the IPs `11.11.11.11` and `22.22.22.22`.
You want to set the local preference of all routes coming from `11.11.11.11` to `1000`.
Furthermore, you want to use BGP community values to make sure that only the routes
coming from neighbor `11.11.11.11` are advertised to neighbor `22.22.22.22`:

```
router# conf t
router(config)# route-map MAP_IN permit 10
router(config-route-map)# set community 15:100
router(config-route-map)# set local-preference 1000
router(config-route-map)# exit

router(config-router)# bgp community-list 1 permit 15:100

router(config-router)# route-map MAP_OUT permit 10
router(config-route-map)# match community 1
router(config-route-map)# exit

router(config)# router bgp 15
router(config-router)# neighbor 11.11.11.11 route-map MAP_IN in
router(config-router)# neighbor 22.22.22.22 route-map MAP_OUT out
```

## BGP AS path

Each route comes with the ASes it has traversed on the way to your router. Routers allow you to match on the AS path to, for example, block all routes that go through a certain AS. At the same time, you can also modify the AS path by prepending your AS number multiple times to influence the path selection of other routers.

If you want to match on AS paths, you need to use a so called `as-path access-list` and AS path expressions.

### AS path expressions

Restricted regular expressions allow you to define a pattern to match in an AS path. Most of the regular expression symbols are supported:

| Symbol | Description                                |
| ------ | ------------------------------------------ |
| `.`    | match any                                  |
| `^`    | match beginning of the AS path             |
| `$`    | match end of the AS path                   |
| `_`    | match space, and beginning or end of the line |
| `*`    | match 0 or more instances of previous atom |
| `?`    | match 0 or 1 instance of previous atom     |
| `+`    | match 1 or more instances of previous atom |
| `\|`    | logical OR |

If you, for example, want to:

* match all routes passing through AS 500, use `_500_`;
* match all routes originating at AS 999, use `_999$`;
* match all routes received from AS 1800, use `^1800_`;

If you want to define more complex matching policies yet with simple regexp, you can also use multiple regexp with the same access-list, for instance:

```
bgp as-path access-list TEST permit ^32
bgp as-path access-list TEST permit ^30
```

This will match on all the routes with an AS path starting with AS 32 or AS 30.

You can use the command `show ip bgp regexp YOUR_REGEXP` to display routes matching the AS path regular expression `YOUR_REGEXP`.

### AS path access list

To use an AS path match in a route-map, you need to define an AS path access list.

```
router# conf t
router(config)# bgp as-path access-list 55 permit _100_
```

The access list has the name/number `55`.

#### Example

Let's assume you have a router which has an eBGP connection to a
neighbor with the IP `11.11.11.11`.
You want to only accept routes that have originated at AS 999 from your neighbor:

```
router# conf t

router(config-router)# bgp as-path access-list 55 permit _999$

router(config)# route-map MAP_IN permit 10
router(config-route-map)# match as-path 55
router(config-route-map)# exit

router(config)# router bgp 15
router(config-router)# neighbor 11.11.11.11 route-map MAP_IN in
```

### AS path prepending

Normally, a router just prepends its own AS number to the AS path of a route before sending it to its neighbors. The AS path is mostly used to avoid routing loops and is also incorporated in the decision process.

In some situations, it is desirable to make the AS path longer than it actually is by prepending your own AS number multiple times.

To achieve that, you can use the following command: `set as-path prepend ASN ASN ASN ...`.

#### Example

Hence, if you want to append your own AS number (for example 500) three times, you can do it as follows:

```
router# conf t

router(config)# route-map MAP_OUT permit 10
router(config-route-map)# set as-path prepend 500 500 500
router(config-route-map)# exit

router(config)# router bgp 15
router(config-router)# neighbor 11.11.11.11 route-map MAP_OUT out
```

## Final remarks

The `show ip bgp` command is very useful to display all prefixes/routes,
for example to check that your route-maps are working correctly. You can also filter the output.
If you want to display all the prefixes/routes with a certain BGP community value (e.g. `15:100`),
you can use the following command:

```
router# sh ip bgp community 15:100
```

Remember that you can always use `?` to show all available commands (e.g. `show ip ?`). Sometimes BGP and
FRR take time to converge. If so, you can use `clear ip bgp *` to force a reset of all BGP sessions on that router.

And finally, whenever you use a *route-map*, the following *route-map* is implicitly added:

```
router(config)# route-map implicit-default-route-map deny 65536
router(config-route-map)# match everything
```

This *route-map* denies everything and makes sure that only the routes which pass the match
clauses of your route-maps are accepted.

This section only sketched what *route-maps* have to offer. In this
assignment, one of your goals will be to get more familiar with them so as to
implement the right routing decisions.
