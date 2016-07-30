
# Cassandra requirement
* https://cassandra.apache.org/doc/latest/getting_started/installing.html#prerequisites
    * java 1.8 require
	* python 2.7 require
			* Pytho v2.7.12 설치
			* http://zetawiki.com/wiki/%EB%A6%AC%EB%88%85%EC%8A%A4_Python_2.7_%EC%BB%B4%ED%8C%8C%EC%9D%BC_%EC%84%A4%EC%B9%98
			* python bug
				* https://issues.apache.org/jira/browse/CASSANDRA-11850

# cqlsh

* bug
    * error 
```
Connection error: ('Unable to connect to any servers', {'10.77.33.96': TypeError('ref() does not take keyword arguments',)})
```
    * cause : https://issues.apache.org/jira/browse/CASSANDRA-11850
    * action
```
reinstall python 2.7.9 or 2.7.11
```

# information
* cassandra도 netty를 사용하고 있다.
```
INFO  03:23:15 Netty using Java NIO event loop
INFO  03:23:15 Using Netty Version: [netty-buffer=netty-buffer-4.0.36.Final.e8fa848, netty-codec=netty-codec-4.0.36.Final.e8fa848, netty-codec-haproxy=netty-codec-haproxy-4.0.36.Final.e8fa848, netty-codec-http=netty-codec-http-4.0.36.Final.e8fa848, netty-codec-socks=netty-codec-socks-4.0.36.Final.e8fa848, netty-common=netty-common-4.0.36.Final.e8fa848, netty-handler=netty-handler-4.0.36.Final.e8fa848, netty-tcnative=netty-tcnative-1.1.33.Fork15.906a8ca, netty-transport=netty-transport-4.0.36.Final.e8fa848, netty-transport-native-epoll=netty-transport-native-epoll-4.0.36.Final.e8fa848, netty-transport-rxtx=netty-transport-rxtx-4.0.36.Final.e8fa848, netty-transport-sctp=netty-transport-sctp-4.0.36.Final.e8fa848, netty-transport-udt=netty-transport-udt-4.0.36.Final.e8fa848]
```

# term
	* superuser : cassandra/cassandra 
		* create another superuser, not named cassandra. this step is optional but highly recommended.
    		* https://docs.datastax.com/en/cassandra/1.2/cassandra/security/security_config_native_authenticate_t.html
		* cqlversion
		* seed
		* Key Space
		* durable_writes
		* Table
		* initial token
		* num tokens
		* virtual node
		* LocalStrategy
		* SimpleStrategy
		* NetworkTopologyStrategy
		* replication_factor

# trouble shooting

* error
```
Connection error: ('Unable to connect to any servers', {'10.77.33.96': ProtocolError("cql_version '3.4.0' is not supported by remote (w/ native protocol). Supported versions: [u'3.4.2']",)})
```
command
```
[hite95@gplinux64 bin]$ ./cqlsh 10.77.33.96 9042 --cqlversion=3.4.2
```

cqlsh command
https://docs.datastax.com/en/cql/3.1/cql/cql_reference/create_keyspace_r.html

mysql
show databases;
describe [DATABASE_NAME];

cqlsh> DESCRIBE keyspaces;

system_traces  system_schema  system_auth  system  system_distributed


cqlsh> DESCRIBE system;

CREATE KEYSPACE system WITH replication = {'class': 'LocalStrategy'}  AND durable_writes = true;

CREATE TABLE system.available_ranges (
    keyspace_name text PRIMARY KEY,
    ranges set<blob>......



java.lang.RuntimeException: Unable to gossip with any seeds
    at org.apache.cassandra.gms.Gossiper.doShadowRound(Gossiper.java:1386) ~[apache-cassandra-3.7.jar:3.7]
    at org.apache.cassandra.service.StorageService.checkForEndpointCollision(StorageService.java:561) ~[apache-cassandra-3.7.jar:3.7]
    at org.apache.cassandra.service.StorageService.prepareToJoin(StorageService.java:855) ~[apache-cassandra-3.7.jar:3.7]
    at org.apache.cassandra.service.StorageService.initServer(StorageService.java:725) ~[apache-cassandra-3.7.jar:3.7]
    at org.apache.cassandra.service.StorageService.initServer(StorageService.java:625) ~[apache-cassandra-3.7.jar:3.7]
    at org.apache.cassandra.service.CassandraDaemon.setup(CassandraDaemon.java:370) [apache-cassandra-3.7.jar:3.7]
    at org.apache.cassandra.service.CassandraDaemon.activate(CassandraDaemon.java:585) [apache-cassandra-3.7.jar:3.7]
    at org.apache.cassandra.service.CassandraDaemon.main(CassandraDaemon.java:714) [apache-cassandra-3.7.jar:3.7]




