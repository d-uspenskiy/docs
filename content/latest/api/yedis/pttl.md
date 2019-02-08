---
title: PTTL
linkTitle: PTTL
description: PTTL
menu:
  latest:
    parent: api-redis
    weight: 2235
aliases:
  - /latest/api/redis/pttl
  - /latest/api/yedis/pttl
isTocNested: true
showAsideToc: true
---

## Synopsis
<b>`PTTL key`</b><br>
Similar to TTL this command returns the remaining time to live of a key that has a timeout set, with the sole difference that TTL returns the amount of remaining time in seconds while PTTL returns it in milliseconds.

## Return Value
Returns TTL in milliseconds, encoded as integer response.

## Examples

You can do this as shown below.
<div class='copy separator-dollar'>
```sh
$ SET yugakey "YugaByte"
```
</div>
```sh
"OK"
```
<div class='copy separator-dollar'>
```sh
$ EXPIRE yugakey 10
```
</div>
```sh
(integer) 1
```
<div class='copy separator-dollar'>
```sh
$ PTTL yugakey
```
</div>
```sh
(integer) 9995
```

## See Also
[`ttl`](../ttl/), [`set`](../set/), [`expire`](../expire/), [`expireat`](../expireat/)