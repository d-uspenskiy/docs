---
title: HDEL
---

## SYNOPSIS
<b>`HDEL key field [field ...]`</b><br>
This command is to remove the given `fields` from the hash that is associated with the given `key`.

<li>If the given `key` does not exist, it is characterized as an empty hash, and 0 is returned for no elements are removed.</li>
<li>If the given `key` is associated with non-hash data, an error is raised.</li>

## RETURN VALUE
Returns the number of existing fields in the hash that were removed by this command.

## EXAMPLES
```
$ HSET yugahash moon "Moon"
1
$ HDEL yugahash moon
1
$ HDEL yugahash moon
0
```

## SEE ALSO
[`hexists`](../hexists/), [`hget`](../hget/), [`hgetall`](../hgetall/), [`hkeys`](../hkeys/), [`hlen`](../hlen/), [`hmget`](../hmget/), [`hmset`](../hmset/), [`hset`](../hset/), [`hstrlen`](../hstrlen/), [`hvals`](../hvals/)