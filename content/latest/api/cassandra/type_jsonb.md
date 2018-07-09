---
title: JSONB
summary: JSONB types
description: JSONB
menu:
  latest:
    parent: api-cassandra
    weight: 1470
aliases:
  - api/cassandra/type_jsonb
  - api/cql/type_jsonb
  - api/ycql/type_jsonb
---

## Synopsis
`JSONB` datatype is used to efficiently model json data. This datatype makes it easy to model data
json data which doesn't have a set schema and might change often.

## Syntax
```
type_specification ::= { JSONB }
```

## Semantics

- Columns of type `JSONB` cannnot be part of the `PRIMARY KEY`.
- Implicitly, values of type `JSONB` are not convertible to other datatypes. `JSONB` types can be
  compared to `TEXT/VARCHAR` datatype as long it represents valid json.
- Values of text datatypes with correct format are convertible to `JSONB`.
- `JSONB` value format supports text literals which are valid json.

## Operators and Functions

We currently support two operators which can be applied to the `JSONB` datatype. The `->` operator 
returns a result of type `JSONB` and further json operations can be applied to the result. The `->>` 
operator converts `JSONB` to its string representation and returns the same. As a result, we can't
apply further `JSONB` operators to the result of the `->>` operator. These operators can either have
a string (for keys in a json object) or integer (for array indices in a json array) as a parameter.

In some cases, we would like to process JSON attributes as numerics. For this purpose, we can use
the `CAST` function to convert text retrieved from the `->>` operator to the appropriate numeric
type.

## Examples
```{.sql .copy .separator-gt}
cqlsh> CREATE TABLE store.books ( id int PRIMARY KEY, details jsonb );
```

```{.sql .copy}
INSERT INTO store.books (id, details) VALUES
  (1, '{ "name": "Macbeth", "author": { "first_name": "William", "last_name": "Shakespeare" }, "year": 1623, "editors": ["John", "Elizabeth", "Jeff"] }');
INSERT INTO store.books (id, details) VALUES 
  (2, '{ "name": "Hamlet", "author": { "first_name": "William", "last_name": "Shakespeare" }, "year": 1603, "editors": ["Lysa", "Mark", "Robert"] }');
INSERT INTO store.books (id, details) VALUES 
  (3, '{ "name": "Oliver Twist", "author": { "first_name": "Charles", "last_name": "Dickens" }, "year": 1838, "genre": "novel", "editors": ["Mark", "Tony", "Britney"] }');
INSERT INTO store.books (id, details) VALUES 
  (4, '{ "name": "Great Expectations", "author": { "first_name": "Charles", "last_name": "Dickens" }, "year": 1950, "genre": "novel", "editors": ["Robert", "John", "Melisa"] }');
INSERT INTO store.books (id, details) VALUES 
  (5, '{ "name": "A Brief History of Time", "author": { "first_name": "Stephen", "last_name": "Hawking" }, "year": 1988, "genre": "science", "editors": ["Melisa", "Mark", "John"] }');
```

```{.sql .copy .separator-gt}
cqlsh> SELECT * FROM store.books;
```
```sql
 id | details
----+-------------------------------------------------------------------------------------------------------------------------------------------------------------
  5 | {"author":{"first_name":"Stephen","last_name":"Hawking"},"editors":["Melisa","Mark","John"],"genre":"science","name":"A Brief History of Time","year":1988}
  1 |                            {"author":{"first_name":"William","last_name":"Shakespeare"},"editors":["John","Elizabeth","Jeff"],"name":"Macbeth","year":1623}
  4 |      {"author":{"first_name":"Charles","last_name":"Dickens"},"editors":["Robert","John","Melisa"],"genre":"novel","name":"Great Expectations","year":1950}
  2 |                                {"author":{"first_name":"William","last_name":"Shakespeare"},"editors":["Lysa","Mark","Robert"],"name":"Hamlet","year":1603}
  3 |             {"author":{"first_name":"Charles","last_name":"Dickens"},"editors":["Mark","Tony","Britney"],"genre":"novel","name":"Oliver Twist","year":1838}

(5 rows)
```

```sql
cqlsh> SELECT * FROM store.books where details->'author'->>'first_name' = 'William' AND details->'author'->>'last_name' = 'Shakespeare';

 id | details
----+----------------------------------------------------------------------------------------------------------------------------------
  1 | {"author":{"first_name":"William","last_name":"Shakespeare"},"editors":["John","Elizabeth","Jeff"],"name":"Macbeth","year":1623}
  2 |     {"author":{"first_name":"William","last_name":"Shakespeare"},"editors":["Lysa","Mark","Robert"],"name":"Hamlet","year":1603}

(2 rows)
```

```sql
cqlsh> SELECT * FROM store.books WHERE details->'editors'->>0 = 'Mark';

 id | details
----+-------------------------------------------------------------------------------------------------------------------------------------------------
  3 | {"author":{"first_name":"Charles","last_name":"Dickens"},"editors":["Mark","Tony","Britney"],"genre":"novel","name":"Oliver Twist","year":1838}

(1 rows)
```

```sql
cqlsh> SELECT * FROM store.books WHERE CAST(details->>'year' AS integer) = 1950;

 id | details
----+--------------------------------------------------------------------------------------------------------------------------------------------------------
  4 | {"author":{"first_name":"Charles","last_name":"Dickens"},"editors":["Robert","John","Melisa"],"genre":"novel","name":"Great Expectations","year":1950}

(1 rows)
```

## See Also
[`Explore Json Documents`](../../../explore/transactional/json-documents)
[Data Types](..#datatypes)