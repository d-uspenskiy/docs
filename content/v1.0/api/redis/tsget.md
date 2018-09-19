---
title: TSGET
linkTitle: TSGET
description: TSGET
menu:
  v1.0:
    parent: api-redis
    weight: 2420
aliases:
  - api/redis/tsget
  - api/yedis/tsget
---

## Synopsis
<b>`TSGET key timestamp`</b><br>
This command fetches the value for the given `timestamp` in the time series that is specified by the
given `key`.

<li>If the given `key` or `timestamp` does not exist, nil is returned.</li>
<li>If the given `key` is associated with non-timeseries data, an error is raised.</li>
<li>If the given `timestamp` is not a valid signed 64 bit integer, an error is raised.</li>

## Return Value
Returns the value for the given `timestamp`

## Examples

The timestamp can be arbitrary integers used just for sorting values in a certain order.
```{.sh .copy .separator-dollar}
$ TSADD cpu_usage 10 "70"
```
```sh
OK
```
```{.sh .copy .separator-dollar}
$ TSADD cpu_usage 20 "80" 30 "60" 40 "90"
```
```sh
OK
```

We could also encode the timestamp as “yyyymmddhhmm”, since this would still produce integers that are sortable by the actual timestamp.
```{.sh .copy .separator-dollar}
$ TSADD cpu_usage 201710311100 "50"
```
```sh
OK
```

A more common option would be to specify the timestamp as the unix timestamp.
```{.sh .copy .separator-dollar}
$ TSADD cpu_usage 1509474505 "75"
```
```sh
OK
```
```{.sh .copy .separator-dollar}
$ TSGET cpu_usage 10
```
```sh
"70"
```
```{.sh .copy .separator-dollar}
$ TSGET cpu_usage 100
```
```sh
(nil)
```
```{.sh .copy .separator-dollar}
$ TSGET cpu_usage 201710311100
```
```sh
"50"
```
```{.sh .copy .separator-dollar}
$ TSGET cpu_usage 1509474505
```
```sh
"75"
```
An error is returned when the timestamp is not an int64.
```{.sh .copy .separator-dollar}
$ TSGET cpu_usage xyz
```
```sh
(error) Request was unable to be processed from server.
```

## See Also
[`tsadd`](../tsadd/), [`tsrem`](../tsrem/), [`tsrangebytime`](../tsrangebytime/),
[`tsrevrangebytime`](../tsrevrangebytime/), [`tslastn`](../tslastn/), [`tscard`](../tscard/)