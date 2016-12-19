# Data Types
CQL is a typed language and supports a rich set of data types, including native types, collection types, user-defined types, tuple types and custom types:

```
cql_type ::=  native_type | collection_type | user_defined_type | tuple_type | custom_type
```

# Native Types
The native types supported by CQL are:

```
native_type ::=  ASCII
                 | BIGINT
                 | BLOB
                 | BOOLEAN
                 | COUNTER
                 | DATE
                 | DECIMAL
                 | DOUBLE
                 | FLOAT
                 | INET
                 | INT
                 | SMALLINT
                 | TEXT
                 | TIME
                 | TIMESTAMP
                 | TIMEUUID
                 | TINYINT
                 | UUID
                 | VARCHAR
                 | VARINT
```
The following table gives additional informations on the native data types, and on which kind of constants each type supports:

| type	| constants supported	| description | 
| ------------- | ------------- | ------------- | 
| ascii	| string | 	ASCII character string | 
| bigint | 	integer | 	64-bit signed long | 
| blob | 	blob | 	Arbitrary bytes (no validation) | 
| boolean | 	boolean | 	Either true or false | 
| counter | 	integer | 	Counter column (64-bit signed value). See Counters for details | 
| date | 	integer, string | 	A date (with no corresponding time value). See Working with dates below for details | 
| decimal | 	integer, float | 	Variable-precision decimal | 
| double | 	integer float | 	64-bit IEEE-754 floating point | 
| float | 	integer, float | 	32-bit IEEE-754 floating point | 
| inet | 	string | 	An IP address, either IPv4 (4 bytes long) or IPv6 (16 bytes long). Note that there is no inet constant, IP address should be input as strings | 
| int | 	integer | 	32-bit signed int | 
| smallint | 	integer | 	16-bit signed int | 
| text | 	string | 	UTF8 encoded string | 
| time | 	integer, string | 	A time (with no corresponding date value) with nanosecond precision. See Working with times below for details | 
| timestamp | 	integer, string | 	A timestamp (date and time) with millisecond precision. See Working with timestamps below for details | 
| timeuuid | 	uuid | 	Version 1 UUID, generally used as a “conflict-free” timestamp. Also see Timeuuid functions | 
| tinyint | 	integer | 	8-bit signed int | 
| uuid | 	uuid | 	A UUID (of any version) | 
| varchar | 	string | 	UTF8 encoded string | 
| varint | 	integer | 	Arbitrary-precision integer | 

## Counters
The counter type is used to define counter columns. A counter column is a column whose value is a 64-bit signed integer and on which 2 operations are supported: incrementing and decrementing (see the UPDATE statement for syntax). Note that the value of a counter cannot be set: a counter does not exist until first incremented/decremented, and that first increment/decrement is made as if the prior value was 0.

Counters have a number of important limitations:

* They cannot be used for columns part of the PRIMARY KEY of a table.
* A table that contains a counter can only contain counters. In other words, either all the columns of a table outside the PRIMARY KEY have the counter type, or none of them have it.
* Counters do not support expiration.
* The deletion of counters is supported, but is only guaranteed to work the first time you delete a counter. In other words, you should not re-update a counter that you have deleted (if you do, proper behavior is not guaranteed).
* Counter updates are, by nature, not idemptotent. An important consequence is that if a counter update fails unexpectedly (timeout or loss of connection to the coordinator node), the client has no way to know if the update has been applied or not. In particular, replaying the update may or may not lead to an over count.

# Working with timestamps
Values of the timestamp type are encoded as 64-bit signed integers representing a number of milliseconds since the standard base time known as the epoch: January 1 1970 at 00:00:00 GMT.

Timestamps can be input in CQL either using their value as an integer, or using a string that represents an ISO 8601 date. For instance, all of the values below are valid timestamp values for Mar 2, 2011, at 04:05:00 AM, GMT:

* 1299038700000
* '2011-02-03 04:05+0000'
* '2011-02-03 04:05:00+0000'
* '2011-02-03 04:05:00.000+0000'
* '2011-02-03T04:05+0000'
* '2011-02-03T04:05:00+0000'
* '2011-02-03T04:05:00.000+0000'

The +0000 above is an RFC 822 4-digit time zone specification; +0000 refers to GMT. US Pacific Standard Time is -0800. The time zone may be omitted if desired ('2011-02-03 04:05:00'), and if so, the date will be interpreted as being in the time zone under which the coordinating Cassandra node is configured. There are however difficulties inherent in relying on the time zone configuration being as expected, so it is recommended that the time zone always be specified for timestamps when feasible.

The time of day may also be omitted ('2011-02-03' or '2011-02-03+0000'), in which case the time of day will default to 00:00:00 in the specified or default time zone. However, if only the date part is relevant, consider using the date type.

# Working with dates
Values of the date type are encoded as 32-bit unsigned integers representing a number of days with “the epoch” at the center of the range (2^31). Epoch is January 1st, 1970

As for timestamp, a date can be input either as an integer or using a date string. In the later case, the format should be yyyy-mm-dd (so '2011-02-03' for instance).

# Working with times
Values of the time type are encoded as 64-bit signed integers representing the number of nanoseconds since midnight.

As for timestamp, a time can be input either as an integer or using a string representing the time. In the later case, the format should be hh:mm:ss[.fffffffff] (where the sub-second precision is optional and if provided, can be less than the nanosecond). So for instance, the following are valid inputs for a time:

* '08:12:54'
* '08:12:54.123'
* '08:12:54.123456'
* '08:12:54.123456789'

# Collections
CQL supports 3 kind of collections: Maps, Sets and Lists. The types of those collections is defined by:

```
collection_type ::=  MAP '<' cql_type ',' cql_type '>'
                     | SET '<' cql_type '>'
                     | LIST '<' cql_type '>'
```

and their values can be inputd using collection literals:

```
collection_literal ::=  map_literal | set_literal | list_literal
map_literal        ::=  '{' [ term ':' term (',' term : term)* ] '}'
set_literal        ::=  '{' [ term (',' term)* ] '}'
list_literal       ::=  '[' [ term (',' term)* ] ']'
```

Note however that neither bind_marker nor NULL are supported inside collection literals.

## Noteworthy characteristics

Collections are meant for storing/denormalizing relatively small amount of data. They work well for things like “the phone numbers of a given user”, “labels applied to an email”, etc. But when items are expected to grow unbounded (“all messages sent by a user”, “events registered by a sensor”...), then collections are not appropriate and a specific table (with clustering columns) should be used. Concretely, (non-frozen) collections have the following noteworthy characteristics and limitations:

* Individual collections are not indexed internally. Which means that even to access a single element of a collection, the while collection has to be read (and reading one is not paged internally).
* While insertion operations on sets and maps never incur a read-before-write internally, some operations on lists do. Further, some lists operations are not idempotent by nature (see the section on lists below for details), making their retry in case of timeout problematic. It is thus advised to prefer sets over lists when possible.

Please note that while some of those limitations may or may not be removed/improved upon in the future, it is a anti-pattern to use a (single) collection to store large amounts of data.

# Maps

# Sets

# Lists

# User-Defined Types

# Tuples

# Custom Types