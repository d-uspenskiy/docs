---
date: 2016-03-09T00:11:02+01:00
title: Tunable Reads
weight: 24
---

With YugaByte, you can choose different consistency levels for performing reads. This includes reading from the tablet leader or the followers. Reading from the followers is similar to a reading from a cache, which can give more read IOPS with low latency. In this tutorial, we will update a single key-value over and over, and read it from the tablet leader. While that workload is running, we will start another workload to read from a follower and verify that we are able to read from a tablet follower.


## Step 1. Setup - create universe

If you have a previously running local universe, destroy it using the following:

```sh
./bin/yb-ctl destroy
```

Start a new local universe with replication factor 5. This will create 5 nodes by default.

```sh
./bin/yb-ctl --replication_factor 5 create
```

Add 2 more nodes.

```sh
./bin/yb-ctl add_node
./bin/yb-ctl add_node
```

## Step 2. Write some data

By default, the key-value sample application runs with strong read consistency where all data is read from the tablet leader. We are going to populate exactly one key with a 10KB value into the system. Since the replication factor is 5, this key will get replicated to 5 of the 7 nodes in the universe.

Let us run the CQL sample key-value app to constantly update this key-value, as well as perform reads with strong consistency against the local universe.

```sh
java -jar ./java/yb-sample-apps.jar --workload CassandraKeyValue \
                                    --nodes 127.0.0.1:9042 \
                                    --nouuid \
                                    --num_unique_keys 1 \
                                    --num_threads_write 1 \
                                    --num_threads_read 1 \
                                    --value_size 10240
```


In the above command, we have set the value of `num_unique_keys` to 1, which means we are overwriting a single key `key:0`. We can verify this using cqlsh:

```sh
./bin/cqlsh
Connected to local cluster at 127.0.0.1:9042.
[cqlsh 5.0.1 | Cassandra 3.9-SNAPSHOT | CQL spec 3.4.2 | Native protocol v4]
Use HELP for help.
cqlsh> SELECT k FROM ybdemo_keyspace.cassandrakeyvalue;

 k
-------
 key:0

(1 rows)
```

## Step 3. Strongly consistent reads from tablet leaders

When performing strongly consistent reads as a part of the above command, all reads will be served by the tablet leader of the tablet that contains the key `key:0`. If we browse to the <a href='http://127.0.0.1:7000/tablet-servers' target="_blank">tablet-servers</a> page, we will see that all the requests are indeed being served by one tserver:

![Reads from the tablet leader](/images/explore-core-features/tuanble-reads-leader.png)


## Step 4. Timeline consistent reads from tablet replicas

Let us stop the above sample app, and run the following variant of the sample app. This command will do updates to the same key `key:0` which will go through the tablet leader, but it will reads from the replicas.

```sh
java -jar ./java/yb-sample-apps.jar --workload CassandraKeyValue \
                                    --nodes 127.0.0.1:9042 \
                                    --nouuid \
                                    --num_unique_keys 1 \
                                    --num_threads_write 1 \
                                    --num_threads_read 1 \
                                    --value_size 10240 \
                                    --local_reads
```

This can be easily seen by refreshing the <a href='http://127.0.0.1:7000/tablet-servers' target="_blank">tablet-servers</a> page, where we will see that the writes are served by a single TServer that is the leader of the tablet for the key `key:0` while multiple TServers which are replicas serve the reads.

![Reads from the tablet leader](/images/explore-core-features/tuanble-reads-followers.png)


## Step 5. Clean up (optional)

Optionally, you can shutdown the local cluster created in Step 1.

```sh
$ ./bin/yb-ctl destroy
```