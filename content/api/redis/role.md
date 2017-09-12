---
title: ROLE
---
Early Releases: YugaByte only have master nodes for Redis services.

## SYNOPSIS
<b>`ROLE`</b><br>
This command is to provide information of a Redis instance, such as its role, its state of replication, its slaves, or its master. Roles are either "master", "slave", or "sentinel".
<li>Information of a master instance may include the following.
  <ol>
  <li>"master"</li>
  <li>An integer that represents state of replication</li>
  <li>An array of connected slaves { IP address, IP port, State of replication }</li>
  </ol>
</li>

<li>Information of a slave instance may include the following.
  <ol>
  <li>"slave"</li>
  <li>Master IP address</li>
  <li>Master IP port</li>
  <li>Connection state that is either "disconnected", "connecting", "sync", or "connected"</li>
  <li>An integer that represents state of replication</li>
  </ol>
</li>

<li>Information of a sentinel instance may include the following.
  <ol>
  <li>"sentinel"</li>
  <li>An array of master names.</li>
  </ol>
</li>

## RETURN VALUE
Returns an array of values.

## EXAMPLES
```
$ ROLE
1) "master"
2) 0
3) 1) 1) "127.0.0.1"
      2) "9200"
      3) "0"
   2) 1) "127.0.0.1"
      2) "9201"
      3) "0"
```

## SEE ALSO
[`auth`](../auth/), [`config`](../config/)