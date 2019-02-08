---
title: 3. Test YCQL API
linkTitle: 3. Test YCQL API
description: Test YugaByte Cloud QL (YCQL) API
aliases:
  - /quick-start/test-cassandra/
  - /latest/quick-start/test-cassandra/
  - /latest/quick-start/test-ycql/
menu:
  latest:
    parent: quick-start
    weight: 130
isTocNested: true
showAsideToc: true
---

After [creating a local cluster](../create-local-cluster/), follow the instructions below to test YugaByte DB's Cassandra-compatible [YCQL](../../api/ycql/) API.

[**cqlsh**](http://cassandra.apache.org/doc/latest/tools/cqlsh.html) is a command line shell for interacting with Apache Cassandra through [CQL (the Cassandra Query Language)](http://cassandra.apache.org/doc/latest/cql/index.html). It uses the Python CQL driver, and connects to the single node specified on the command line. For ease of use, YugaByte DB ships with the 3.10 version of cqlsh in its bin directory.


<ul class="nav nav-tabs nav-tabs-yb">
  <li >
    <a href="#macos" class="nav-link active" id="macos-tab" data-toggle="tab" role="tab" aria-controls="macos" aria-selected="true">
      <i class="fab fa-apple" aria-hidden="true"></i>
      macOS
    </a>
  </li>
  <li>
    <a href="#linux" class="nav-link" id="linux-tab" data-toggle="tab" role="tab" aria-controls="linux" aria-selected="false">
      <i class="fab fa-linux" aria-hidden="true"></i>
      Linux
    </a>
  </li>
  <li>
    <a href="#docker" class="nav-link" id="docker-tab" data-toggle="tab" role="tab" aria-controls="docker" aria-selected="false">
      <i class="fab fa-docker" aria-hidden="true"></i>
      Docker
    </a>
  </li>
  <li >
    <a href="#kubernetes" class="nav-link" id="kubernetes-tab" data-toggle="tab" role="tab" aria-controls="kubernetes" aria-selected="false">
      <i class="fas fa-cubes" aria-hidden="true"></i>
      Kubernetes
    </a>
  </li>
</ul>

<div class="tab-content">
  <div id="macos" class="tab-pane fade show active" role="tabpanel" aria-labelledby="macos-tab">
    {{% includeMarkdown "binary/test-cassandra.md" /%}}
  </div>
  <div id="linux" class="tab-pane fade" role="tabpanel" aria-labelledby="linux-tab">
    {{% includeMarkdown "binary/test-cassandra.md" /%}}
  </div> 
  <div id="docker" class="tab-pane fade" role="tabpanel" aria-labelledby="docker-tab">
    {{% includeMarkdown "docker/test-cassandra.md" /%}}
  </div>
  <div id="kubernetes" class="tab-pane fade" role="tabpanel" aria-labelledby="kubernetes-tab">
    {{% includeMarkdown "kubernetes/test-cassandra.md" /%}}
  </div>
</div>


## 2. Create a table

Create a keyspace called 'myapp'.
<div class='copy separator-gt'>
```sql
cqlsh> CREATE KEYSPACE myapp;
```
</div>


Create a table named 'stock_market' which can store stock prices at various timestamps for different stock ticker symbols.
<div class='copy separator-gt'>
```sql
cqlsh> CREATE TABLE myapp.stock_market (
  stock_symbol text,
  ts text,
  current_price float,
  PRIMARY KEY (stock_symbol, ts)
);
```
</div>



## 3. Insert data

Let us insert some data for a few stock symbols into our newly created 'stock_market' table. You can copy-paste these values directly into your cqlsh shell.
<div class='copy separator-gt'>
```sql
cqlsh> INSERT INTO myapp.stock_market (stock_symbol,ts,current_price) VALUES ('AAPL','2017-10-26 09:00:00',157.41);
INSERT INTO myapp.stock_market (stock_symbol,ts,current_price) VALUES ('AAPL','2017-10-26 10:00:00',157);
```
</div>
<div class='copy separator-gt'>
```sql
cqlsh> INSERT INTO myapp.stock_market (stock_symbol,ts,current_price) VALUES ('FB','2017-10-26 09:00:00',170.63);
INSERT INTO myapp.stock_market (stock_symbol,ts,current_price) VALUES ('FB','2017-10-26 10:00:00',170.1);
```
</div>
<div class='copy separator-gt'>
```sql
cqlsh> INSERT INTO myapp.stock_market (stock_symbol,ts,current_price) VALUES ('GOOG','2017-10-26 09:00:00',972.56);
INSERT INTO myapp.stock_market (stock_symbol,ts,current_price) VALUES ('GOOG','2017-10-26 10:00:00',971.91);
```
</div>

## 4. Query the table

Query all the values we have inserted into the database for the stock symbol 'AAPL' as follows.
<div class='copy separator-gt'>
```sql
cqlsh> SELECT * FROM myapp.stock_market WHERE stock_symbol = 'AAPL';
```
</div>
```sql
 stock_symbol | ts                  | current_price
--------------+---------------------+---------------
         AAPL | 2017-10-26 09:00:00 |        157.41
         AAPL | 2017-10-26 10:00:00 |           157

(2 rows)
```


Query all the values for 'FB' and 'GOOG' as follows.
<div class='copy separator-gt'>
```sql
cqlsh> SELECT * FROM myapp.stock_market WHERE stock_symbol in ('FB', 'GOOG');
```
</div>
```sql
 stock_symbol | ts                  | current_price
--------------+---------------------+---------------
           FB | 2017-10-26 09:00:00 |        170.63
           FB | 2017-10-26 10:00:00 |     170.10001
         GOOG | 2017-10-26 09:00:00 |        972.56
         GOOG | 2017-10-26 10:00:00 |     971.90997

(4 rows)
```