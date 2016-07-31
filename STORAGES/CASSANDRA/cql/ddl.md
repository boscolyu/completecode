# Data Definition
CQL stores data in tables, whose schema defines the layout of said data in the table, and those tables are grouped in keyspaces. A keyspace defines a number of options that applies to all the tables it contains, most prominently of which is the replication strategy used by the keyspace. It is generally encouraged to use one keyspace by application, and thus many cluster may define only one keyspace.

This section describes the statements used to create, modify, and remove those keyspace and tables.

# Common definitions
The names of the keyspaces and tables are defined by the following grammar:

```
keyspace_name ::=  name
table_name    ::=  [ keyspace_name '.' ] name
name          ::=  unquoted_name | quoted_name
unquoted_name ::=  re('[a-zA-Z_0-9]{1, 48}')
quoted_name   ::=  '"' unquoted_name '"'
```

Both keyspace and table name should be comprised of only alphanumeric characters, cannot be empty and are limited in size to 48 characters (that limit exists mostly to avoid filenames (which may include the keyspace and table name) to go over the limits of certain file systems). By default, keyspace and table names are case insensitive (myTable is equivalent to mytable) but case sensitivity can be forced by using double-quotes ("myTable" is different from mytable).

Further, a table is always part of a keyspace and a table name can be provided fully-qualified by the keyspace it is part of. If is is not fully-qualified, the table is assumed to be in the current keyspace (see USE statement).

Further, the valid names for columns is simply defined as:

```
column_name ::=  identifier
```

We also define the notion of statement options for use in the following section:

```
options ::=  option ( AND option )*
option  ::=  identifier '=' ( identifier | constant | map_literal )
```

# CREATE KEYSPACE
A keyspace is created using a CREATE KEYSPACE statement:

```
create_keyspace_statement ::=  CREATE KEYSPACE [ IF NOT EXISTS ] keyspace_name WITH options
```

For instance:

```
CREATE KEYSPACE Excelsior
           WITH replication = {'class': 'SimpleStrategy', 'replication_factor' : 3};

CREATE KEYSPACE Excalibur
           WITH replication = {'class': 'NetworkTopologyStrategy', 'DC1' : 1, 'DC2' : 3}
            AND durable_writes = false;
```

The supported options are:

| name	| kind	| mandatory |	default	| description |
|----|----|----|----|---|
| replication    | 	map    | 	yes | 	     |	The replication strategy and options to use for the keyspace (see details below). | 
| durable_writes | 	simple | 	no | 	true | 	Whether to use the commit log for updates on this keyspace (disable this option at your own risk!). | 


The replication property[^footnote] is mandatory and must at least contains the 'class' sub-option which defines the replication strategy class to use. The rest of the sub-options depends on what replication strategy is used. By default, Cassandra support the following 'class':

* 'SimpleStrategy': A simple strategy that defines a replication factor for the whole cluster. The only sub-options supported is 'replication_factor' to define that replication factor and is mandatory.
* 'NetworkTopologyStrategy': A replication strategy that allows to set the replication factor independently for each data-center. The rest of the sub-options are key-value pairs where a key is a data-center name and its value is the associated replication factor.

Attempting to create a keyspace that already exists will return an error unless the IF NOT EXISTS option is used. If it is used, the statement will be a no-op if the keyspace already exists.

[^footnote]: Test, [Link](https://google.com).

# USE

# ALTER KEYSPACE

# DROP KEYSPACE

# CREATE TABLE

Creating a new table uses the CREATE TABLE statement:

```
create_table_statement ::=  CREATE TABLE [ IF NOT EXISTS ] table_name
                            '('
                                column_definition
                                ( ',' column_definition )*
                                [ ',' PRIMARY KEY '(' primary_key ')' ]
                            ')' [ WITH table_options ]
column_definition      ::=  column_name cql_type [ STATIC ] [ PRIMARY KEY]
primary_key            ::=  partition_key [ ',' clustering_columns ]
partition_key          ::=  column_name
                            | '(' column_name ( ',' column_name )* ')'
clustering_columns     ::=  column_name ( ',' column_name )*
table_options          ::=  COMPACT STORAGE [ AND table_options ]
                            | CLUSTERING ORDER BY '(' clustering_order ')' [ AND table_options ]
                            | options
clustering_order       ::=  column_name (ASC | DESC) ( ',' column_name (ASC | DESC) )*
```

For instance:

```
CREATE TABLE monkeySpecies (
    species text PRIMARY KEY,
    common_name text,
    population varint,
    average_size int
) WITH comment='Important biological records'
   AND read_repair_chance = 1.0;

CREATE TABLE timeline (
    userid uuid,
    posted_month int,
    posted_time uuid,
    body text,
    posted_by text,
    PRIMARY KEY (userid, posted_month, posted_time)
) WITH compaction = { 'class' : 'LeveledCompactionStrategy' };

CREATE TABLE loads (
    machine inet,
    cpu int,
    mtime timeuuid,
    load float,
    PRIMARY KEY ((machine, cpu), mtime)
) WITH CLUSTERING ORDER BY (mtime DESC);
```

A CQL table has a name and is composed of a set of rows. Creating a table amounts to defining which columns the rows will be composed, which of those columns compose the primary key, as well as optional options for the table.

Attempting to create an already existing table will return an error unless the IF NOT EXISTS directive is used. If it is used, the statement will be a no-op if the table already exists.

## Column definitions

Every rows in a CQL table has a set of predefined columns defined at the time of the table creation (or added later using an alter statement).

A column_definition is primarily comprised of the name of the column defined and it’s type, which restrict which values are accepted for that column. Additionally, a column definition can have the following modifiers:

* STATIC : it declares the column as being a static column.
* PRIMARY KEY : it declares the column as being the sole component of the primary key of the table.

### Static columns

Some columns can be declared as STATIC in a table definition. A column that is static will be “shared” by all the rows belonging to the same partition (having the same partition key). For instance:

```
CREATE TABLE t (
    pk int,
    t int,
    v text,
    s text static,
    PRIMARY KEY (pk, t)
);

INSERT INTO t (pk, t, v, s) VALUES (0, 0, 'val0', 'static0');
INSERT INTO t (pk, t, v, s) VALUES (0, 1, 'val1', 'static1');

SELECT * FROM t;
   pk | t | v      | s
  ----+---+--------+-----------
   0  | 0 | 'val0' | 'static1'
   0  | 1 | 'val1' | 'static1'
```

As can be seen, the s value is the same (static1) for both of the row in the partition (the partition key in that example being pk, both rows are in that same partition): the 2nd insertion has overridden the value for s.

The use of static columns as the following restrictions:

* tables with the COMPACT STORAGE option (see below) cannot use them.
* a table without clustering columns cannot have static columns (in a table without clustering columns, every partition has only one row, and so every column is inherently static).
* only non PRIMARY KEY columns can be static.

# ALTER TABLE

# DROP TABLE

# TRUNCATE

