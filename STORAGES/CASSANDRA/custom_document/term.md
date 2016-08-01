
# Cassandra requirement
* prerequisites document : https://cassandra.apache.org/doc/latest/getting_started/installing.html#prerequisites
* java 1.8 require
* python 2.7 require
	* Pytho v2.7.12 설치
	* http://zetawiki.com/wiki/%EB%A6%AC%EB%88%85%EC%8A%A4_Python_2.7_%EC%BB%B4%ED%8C%8C%EC%9D%BC_%EC%84%A4%EC%B9%98
	* python bug
		* https://issues.apache.org/jira/browse/CASSANDRA-11850
    * bug
        * error 
            ```
            Connection error: ('Unable to connect to any servers', {'10.77.33.96': TypeError('ref() does not take keyword arguments',)})
            ```
        * cause : https://issues.apache.org/jira/browse/CASSANDRA-11850
        * action
            * reinstall python 2.7.9 or 2.7.11
            * waiting patch version

# information
cassandra도 netty를 사용하고 있다.

```
INFO  03:23:15 Netty using Java NIO event loop
INFO  03:23:15 Using Netty Version: [netty-buffer=netty-buffer-4.0.36.Final.e8fa848, netty-codec=netty-codec-4.0.36.Final.e8fa848, netty-codec-haproxy=netty-codec-haproxy-4.0.36.Final.e8fa848, netty-codec-http=netty-codec-http-4.0.36.Final.e8fa848, netty-codec-socks=netty-codec-socks-4.0.36.Final.e8fa848, netty-common=netty-common-4.0.36.Final.e8fa848, netty-handler=netty-handler-4.0.36.Final.e8fa848, netty-tcnative=netty-tcnative-1.1.33.Fork15.906a8ca, netty-transport=netty-transport-4.0.36.Final.e8fa848, netty-transport-native-epoll=netty-transport-native-epoll-4.0.36.Final.e8fa848, netty-transport-rxtx=netty-transport-rxtx-4.0.36.Final.e8fa848, netty-transport-sctp=netty-transport-sctp-4.0.36.Final.e8fa848, netty-transport-udt=netty-transport-udt-4.0.36.Final.e8fa848]
```

# architecture
* http://docs.datastax.com/en/cassandra/3.x/cassandra/architecture/archIntro.html

# term
* superuser : cassandra/cassandra 
	* create another superuser, not named cassandra. this step is optional but highly recommended.
   		* https://docs.datastax.com/en/cassandra/1.2/cassandra/security/security_config_native_authenticate_t.html
* cqlversion
* gossip : A peer-to-peer communication protocol to discover and share location and state information about the other nodes in a Cassandra cluster. Gossip information is also persisted locally by each node to use immediately when a node restarts.
    * 3 handshake message exchange between 2 hosts;
    * It is based on Accrual Failure Detection algorithm.
```
comment of org.apache.cassandra.gms.Gossiper class

/**
 * This module is responsible for Gossiping information for the local endpoint. This abstraction
 * maintains the list of live and dead endpoints. Periodically i.e. every 1 second this module
 * chooses a random node and initiates a round of Gossip with it. A round of Gossip involves 3
 * rounds of messaging. For instance if node A wants to initiate a round of Gossip with node B
 * it starts off by sending node B a GossipDigestSynMessage. Node B on receipt of this message
 * sends node A a GossipDigestAckMessage. On receipt of this message node A sends node B a
 * GossipDigestAck2Message which completes a round of Gossip. This module as and when it hears one
 * of the three above mentioned messages updates the Failure Detector with the liveness information.
 * Upon hearing a GossipShutdownMessage, this module will instantly mark the remote node as down in
 * the Failure Detector.
 */
```    
* seed 
* Key Space
* commit log : All data is written first to the commit log for durability. After all its data has been flushed to SSTables, it can be archived, deleted, or recycled.
* memtable : A Cassandra table-specific, in-memory data structure that resembles a write-back cache.
    * http://docs.datastax.com/en/glossary/doc/glossary/gloss_memtable.html
* SSTables : A sorted string table (SSTable) is an immutable data file to which Cassandra writes memtables periodically. SSTables are stored on disk sequentially and maintained for each Cassandra table. SSTables are append only and stored on disk sequentially and maintained for each Cassandra table.
* durable_writes : Whether to use the commit log for updates on this keyspace (disable this option at your own risk!).
* Table
* initial token
* num tokens
* virtual node
* LocalStrategy
* SimpleStrategy : 인접 노드에 데이터를 복제함.
* NetworkTopologyStrategy : 네트워크 상에 배치된 구조를 고려하여 복사본 저장
* replication_factor
* bloomfilter
* SSTable Compaction : merge sort 
* snitch : 
    * simple snitch :
    * rack inferring snitch : 
    * property file snitch :
* partitioner :
* consistency level :
    * quorum : 
* request delegation
* Staged Event-Driven Architecture
* AntiEntropy : https://wiki.apache.org/cassandra/AntiEntropy

# Directory 

## Bin
nodetool
cassandra
cqlsh
## conf
cassandra.yaml
## data
commitlog
data
hints
saved_cashes
## lib
## logs
## pylib
## tools


# tuning memory allocation
you can use the jemalloc library to speed up memory allocations
It is very useful for memory table.

log not to add the configuration 
```
WARN  02:36:42 jemalloc shared library could not be preloaded to speed up memory allocations
```


Cassandra v2.2.4 updates
```
Depreacation

    - Configuration parameter memory_allocator in cassandra.yaml has been deprecated
      and will be removed in 3.0.0. As mentioned below for 2.2.0, jemalloc is
      automatically preloaded on Unix platforms.
```

modification
```
# Cassandra uses an installed jemalloc via LD_PRELOAD / DYLD_INSERT_LIBRARIES by default to improve off-heap
# memory allocation performance. The following code searches for an installed libjemalloc.dylib/.so/.1.so using
# Linux and OS-X specific approaches.
# To specify your own libjemalloc in a different path, configure the fully qualified path in CASSANDRA_LIBJEMALLOC.
# To disable jemalloc preload at all, set CASSANDRA_LIBJEMALLOC=-
#
CASSANDRA_LIBJEMALLOC=/home/hite95/DEV/jemalloc/jemalloc-4.2.1/lib/libjemalloc.so.2
#
```

result
```
INFO  02:37:54 jemalloc seems to be preloaded from /home/hite95/DEV/jemalloc/jemalloc-4.2.1/lib/libjemalloc.so.2
```

# cqlsh
```
cqlsh:leader_board> CREATE TABLE monkeySpecies (
                ...     species int PRIMARY KEY,
                ...     common_name text,
                ...     population varint,
                ...     average_size int
                ... ) WITH comment='Important biological records'
                ...    AND read_repair_chance = 1.0;
cqlsh:leader_board> 
cqlsh:leader_board> 
cqlsh:leader_board> 
cqlsh:leader_board> describe leader_board;

CREATE KEYSPACE leader_board WITH replication = {'class': 'SimpleStrategy', 'replication_factor': '3'}  AND durable_writes = true;

CREATE TABLE leader_board.monkeyspecies (
    species int PRIMARY KEY,
    average_size int,
    common_name text,
    population varint
) WITH bloom_filter_fp_chance = 0.01
    AND caching = {'keys': 'ALL', 'rows_per_partition': 'NONE'}
    AND comment = 'Important biological records'
    AND compaction = {'class': 'org.apache.cassandra.db.compaction.SizeTieredCompactionStrategy', 'max_threshold': '32', 'min_threshold': '4'}
    AND compression = {'chunk_length_in_kb': '64', 'class': 'org.apache.cassandra.io.compress.LZ4Compressor'}
    AND crc_check_chance = 1.0
    AND dclocal_read_repair_chance = 0.1
    AND default_time_to_live = 0
    AND gc_grace_seconds = 864000
    AND max_index_interval = 2048
    AND memtable_flush_period_in_ms = 0
    AND min_index_interval = 128
    AND read_repair_chance = 1.0
    AND speculative_retry = '99PERCENTILE';
```

# trouble shooting
## cql version mismatch case
* error
    ```
    Connection error: ('Unable to connect to any servers', {'10.77.33.96': ProtocolError("cql_version '3.4.0' is not supported by remote (w/ native protocol). Supported versions: [u'3.4.2']",)})
    ```
* command
    ```
    [hite95@gplinux64 bin]$ ./cqlsh 10.77.33.96 9042 --cqlversion=3.4.2
    ```
* cqlsh command
    * https://docs.datastax.com/en/cql/3.1/cql/cql_reference/create_keyspace_r.html

## seed error log
* error
    ```
    java.lang.RuntimeException: Unable to gossip with any seeds
    at org.apache.cassandra.gms.Gossiper.doShadowRound(Gossiper.java:1386) ~[apache-cassandra-3.7.jar:3.7]
    at org.apache.cassandra.service.StorageService.checkForEndpointCollision(StorageService.java:561) ~[apache-cassandra-3.7.jar:3.7]
    at org.apache.cassandra.service.StorageService.prepareToJoin(StorageService.java:855) ~[apache-cassandra-3.7.jar:3.7]
    at org.apache.cassandra.service.StorageService.initServer(StorageService.java:725) ~[apache-cassandra-3.7.jar:3.7]
    at org.apache.cassandra.service.StorageService.initServer(StorageService.java:625) ~[apache-cassandra-3.7.jar:3.7]
    at org.apache.cassandra.service.CassandraDaemon.setup(CassandraDaemon.java:370) [apache-cassandra-3.7.jar:3.7]
    at org.apache.cassandra.service.CassandraDaemon.activate(CassandraDaemon.java:585) [apache-cassandra-3.7.jar:3.7]
    at org.apache.cassandra.service.CassandraDaemon.main(CassandraDaemon.java:714) [apache-cassandra-3.7.jar:3.7]
    ```


# Compare with MySQL
* list key space, databases between MySQL and Cassandra
    * MySQL
        ```
        show databases;
        describe [DATABASE_NAME];
        ```
    * Cassandra
        ```
        cqlsh> DESCRIBE keyspaces;

        system_traces  system_schema  system_auth  system  system_distributed
        ``` 
 * view the key space, databases between MySQL and Cassandra
    * MySQL 
        ```
        DESCRIBE [Database Name]
        ```
    * Cassandra
        ```
        cqlsh> DESCRIBE system;

        CREATE KEYSPACE system WITH replication = {'class': 'LocalStrategy'}  AND durable_writes = true;

        CREATE TABLE system.available_ranges (
            keyspace_name text PRIMARY KEY,
            ranges set<blob>......
        ```


# Operation


cassandra shutdown

```
[hite95@gplinux64 bin]$ nodetool stopdaemon
Cassandra has shutdown.
error: Connection refused
-- StackTrace --
java.net.ConnectException: Connection refused
        at java.net.PlainSocketImpl.socketConnect(Native Method)
        at java.net.AbstractPlainSocketImpl.doConnect(AbstractPlainSocketImpl.java:350)
        at java.net.AbstractPlainSocketImpl.connectToAddress(AbstractPlainSocketImpl.java:206)
        at java.net.AbstractPlainSocketImpl.connect(AbstractPlainSocketImpl.java:188)
        at java.net.SocksSocketImpl.connect(SocksSocketImpl.java:392)
        at java.net.Socket.connect(Socket.java:589)
        at java.net.Socket.connect(Socket.java:538)
        at java.net.Socket.<init>(Socket.java:434)
        at java.net.Socket.<init>(Socket.java:211)
        at sun.rmi.transport.proxy.RMIDirectSocketFactory.createSocket(RMIDirectSocketFactory.java:40)
        at sun.rmi.transport.proxy.RMIMasterSocketFactory.createSocket(RMIMasterSocketFactory.java:148)
        at sun.rmi.transport.tcp.TCPEndpoint.newSocket(TCPEndpoint.java:613)
        at sun.rmi.transport.tcp.TCPChannel.createConnection(TCPChannel.java:216)
        at sun.rmi.transport.tcp.TCPChannel.newConnection(TCPChannel.java:202)
        at sun.rmi.server.UnicastRef.invoke(UnicastRef.java:130)
        at com.sun.jmx.remote.internal.PRef.invoke(Unknown Source)
        at javax.management.remote.rmi.RMIConnectionImpl_Stub.close(Unknown Source)
        at javax.management.remote.rmi.RMIConnector.close(RMIConnector.java:505)
        at javax.management.remote.rmi.RMIConnector.close(RMIConnector.java:445)
        at org.apache.cassandra.tools.NodeProbe.close(NodeProbe.java:236)
        at org.apache.cassandra.tools.NodeTool$NodeToolCmd.run(NodeTool.java:251)
        at org.apache.cassandra.tools.NodeTool.main(NodeTool.java:162)


```


# tuning guide

https://tobert.github.io/pages/als-cassandra-21-tuning-guide.html

```
memtable_allocation_type: offheap_objects

Offheap memtables can improve write-heavy workloads by reducing the amount of data stored on the Java heap. 1-2GB of offheap memory should be sufficient for most workloads. The memtable size whould be left at 25% of the heap as well, since it is still in use when offheap is in play.
An additional performance boost can be realized by installing and enabling jemalloc. On common distros this is usually a yum/apt-get install away. Worst case you can simply install the .so file in /usr/local/lib or similar. Instructions for configuring it are in cassandra-env.sh.
```