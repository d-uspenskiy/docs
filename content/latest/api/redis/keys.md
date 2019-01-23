---
title: KEYS
linkTitle: KEYS
description: KEYS
menu:
  latest:
    parent: api-redis
    weight: 2217
aliases:
  - /latest/api/redis/keys
  - /latest/api/yedis/keys
---

## Synopsis
<b>`KEYS pattern`</b>
Returns all keys matching pattern. An error is thrown if over 10,000 keys are in the db to protect
against an out of memory state, although this limit can be changed.

Supported patterns:
<li>?oo matches foo, boo, zoo, ... </li>
<li>\*oo matches oo, foo, fllllloo, ...</li>
<li>[fb]oo matches foo and boo.</li>
<li>[^f]oo matches boo, zoo, ... but not foo.</li>
<li>[f-g]oo matches foo and goo.</li>

Use \\ to escape special characters if you want to match them verbatim.

## Return Value
String array with the list of matching keys, error if there are over 10,000 keys. There is no 
ordering invariant on the returned keys.

## Examples

Add two keys into the database.
```{.sh .copy .separator-dollar}
$ ZADD z_key 1.0 v1
```
```sh
(integer) 1
```
```{.sh .copy .separator-dollar}
$ SADD key val
```
```sh
(integer) 1 
```

Get all keys in the db.
```{.sh .copy .separator-dollar}
$ KEYS *
```
```sh
1) "key"
2) "z_key"
```

Get keys matching a specific pattern.
```{.sh .copy .separator-dollar}
$ KEYS ?ey
```
```sh
1) "key"
```
