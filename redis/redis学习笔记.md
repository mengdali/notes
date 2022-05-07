# Redis

> 中文官方网站：https://www.redis.net.cn/tutorial/3501.html

## 1 Nosql  概述

### 1.1 为什么要用nosql 

NoSql = Not Only Sql (不仅仅是Sql)

关系型数据库：表格，行，列

很多用户的个人信息、社交网络、地理位置，这些类型的存储不需要一个固定的格式。

### 1.2 Nosql 特点

~~~shell
1.方便扩展(数据之间没有关系，容易扩展)
2.大数据量高性能(Redis一秒写8万次，读取11万次，Nosql的缓存记录级，是一种细粒度的缓存，性能比较高)
3.数据类型是多样型的！(不需要事先设计数据库，随取随用！)
~~~

### 1.3 传统RDBMS和NoSql的区别

```
传统的RDBMS
- 结构化组织
- SQL
- 数据和关系都存在单独的表中
- 基础的事务 
- 严格的一致性 
```

```
NoSql
 - 不仅仅是数据
- 没有固定的查询语言
- 键值对存储 列存储 文档存储 图形数据库（社交关系）
- 最终一致性
- CAP原理和BASE
- 高可用 高性能 高可扩
```

### 1.4 NoSql的四大分类

- KV键值对:redis

* 文档型数据库（bson格式和json一样):MongoDB

  > MongoDB:一个基于分布式文件存储的数据库，主要用于处理大量的文档。是一个介于关系型数据库和非关系型数据中中间的产品。

* 列存储数据库：HBase、分布式文件系统

*  图关系数据库：Neo4j,InfoGrid 不是存图形，而是放关系 如朋友圈社交网络、广告推荐

![](http://cdn.lsbysl.com//202204121507045.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto)

### 1.5 安装

#### 1.5.1 Docker安装redis

- 步骤一

~~~shell
mkdir redis_conf
cd redis
~~~

- 下载redis官方配置文件

​		[redis配置文件](http://download.redis.io/redis-stable/redis.conf)

- 网上找的可能有坑用自己的

~~~
# Redis configuration file example.
#
# Note that in order to read the configuration file, Redis must be
# started with the file path as first argument:
#
# ./redis-server /path/to/redis.conf

# Note on units: when memory size is needed, it is possible to specify
# it in the usual form of 1k 5GB 4M and so forth:
#
# 1k => 1000 bytes
# 1kb => 1024 bytes
# 1m => 1000000 bytes
# 1mb => 1024*1024 bytes
# 1g => 1000000000 bytes
# 1gb => 1024*1024*1024 bytes
#
# units are case insensitive so 1GB 1Gb 1gB are all the same.

################################## INCLUDES ###################################

# Include one or more other config files here.  This is useful if you
# have a standard template that goes to all Redis servers but also need
# to customize a few per-server settings.  Include files can include
# other files, so use this wisely.
#
# Note that option "include" won't be rewritten by command "CONFIG REWRITE"
# from admin or Redis Sentinel. Since Redis always uses the last processed
# line as value of a configuration directive, you'd better put includes
# at the beginning of this file to avoid overwriting config change at runtime.
#
# If instead you are interested in using includes to override configuration
# options, it is better to use include as the last line.
#
# include /path/to/local.conf
# include /path/to/other.conf

################################## MODULES #####################################

# Load modules at startup. If the server is not able to load modules
# it will abort. It is possible to use multiple loadmodule directives.
#
# loadmodule /path/to/my_module.so
# loadmodule /path/to/other_module.so

################################## NETWORK #####################################

# By default, if no "bind" configuration directive is specified, Redis listens
# for connections from all available network interfaces on the host machine.
# It is possible to listen to just one or multiple selected interfaces using
# the "bind" configuration directive, followed by one or more IP addresses.
# Each address can be prefixed by "-", which means that redis will not fail to
# start if the address is not available. Being not available only refers to
# addresses that does not correspond to any network interfece. Addresses that
# are already in use will always fail, and unsupported protocols will always BE
# silently skipped.
#
# Examples:
#
# bind 192.168.1.100 10.0.0.1     # listens on two specific IPv4 addresses
# bind 127.0.0.1 ::1              # listens on loopback IPv4 and IPv6
# bind * -::*                     # like the default, all available interfaces
#
# ~~~ WARNING ~~~ If the computer running Redis is directly exposed to the
# internet, binding to all the interfaces is dangerous and will expose the
# instance to everybody on the internet. So by default we uncomment the
# following bind directive, that will force Redis to listen only on the
# IPv4 and IPv6 (if available) loopback interface addresses (this means Redis
# will only be able to accept client connections from the same host that it is
# running on).
#
# IF YOU ARE SURE YOU WANT YOUR INSTANCE TO LISTEN TO ALL THE INTERFACES
# JUST COMMENT OUT THE FOLLOWING LINE.
# ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
# bind 127.0.0.1 -::1

# Protected mode is a layer of security protection, in order to avoid that
# Redis instances left open on the internet are accessed and exploited.
#
# When protected mode is on and if:
#
# 1) The server is not binding explicitly to a set of addresses using the
#    "bind" directive.
# 2) No password is configured.
#
# The server only accepts connections from clients connecting from the
# IPv4 and IPv6 loopback addresses 127.0.0.1 and ::1, and from Unix domain
# sockets.
#
# By default protected mode is enabled. You should disable it only if
# you are sure you want clients from other hosts to connect to Redis
# even if no authentication is configured, nor a specific set of interfaces
# are explicitly listed using the "bind" directive.
protected-mode no

# Accept connections on the specified port, default is 6379 (IANA #815344).
# If port 0 is specified Redis will not listen on a TCP socket.
port 6379

# TCP listen() backlog.
#
# In high requests-per-second environments you need a high backlog in order
# to avoid slow clients connection issues. Note that the Linux kernel
# will silently truncate it to the value of /proc/sys/net/core/somaxconn so
# make sure to raise both the value of somaxconn and tcp_max_syn_backlog
# in order to get the desired effect.
tcp-backlog 511

# Unix socket.
#
# Specify the path for the Unix socket that will be used to listen for
# incoming connections. There is no default, so Redis will not listen
# on a unix socket when not specified.
#
# unixsocket /run/redis.sock
# unixsocketperm 700

# Close the connection after a client is idle for N seconds (0 to disable)
timeout 0

# TCP keepalive.
#
# If non-zero, use SO_KEEPALIVE to send TCP ACKs to clients in absence
# of communication. This is useful for two reasons:
#
# 1) Detect dead peers.
# 2) Force network equipment in the middle to consider the connection to be
#    alive.
#
# On Linux, the specified value (in seconds) is the period used to send ACKs.
# Note that to close the connection the double of the time is needed.
# On other kernels the period depends on the kernel configuration.
#
# A reasonable value for this option is 300 seconds, which is the new
# Redis default starting with Redis 3.2.1.
tcp-keepalive 300

################################# TLS/SSL #####################################

# By default, TLS/SSL is disabled. To enable it, the "tls-port" configuration
# directive can be used to define TLS-listening ports. To enable TLS on the
# default port, use:
#
# port 0
# tls-port 6379

# Configure a X.509 certificate and private key to use for authenticating the
# server to connected clients, masters or cluster peers.  These files should be
# PEM formatted.
#
# tls-cert-file redis.crt 
# tls-key-file redis.key
#
# If the key file is encrypted using a passphrase, it can be included here
# as well.
#
# tls-key-file-pass secret

# Normally Redis uses the same certificate for both server functions (accepting
# connections) and client functions (replicating from a master, establishing
# cluster bus connections, etc.).
#
# Sometimes certificates are issued with attributes that designate them as
# client-only or server-only certificates. In that case it may be desired to use
# different certificates for incoming (server) and outgoing (client)
# connections. To do that, use the following directives:
#
# tls-client-cert-file client.crt
# tls-client-key-file client.key
#
# If the key file is encrypted using a passphrase, it can be included here
# as well.
#
# tls-client-key-file-pass secret

# Configure a DH parameters file to enable Diffie-Hellman (DH) key exchange:
#
# tls-dh-params-file redis.dh

# Configure a CA certificate(s) bundle or directory to authenticate TLS/SSL
# clients and peers.  Redis requires an explicit configuration of at least one
# of these, and will not implicitly use the system wide configuration.
#
# tls-ca-cert-file ca.crt
# tls-ca-cert-dir /etc/ssl/certs

# By default, clients (including replica servers) on a TLS port are required
# to authenticate using valid client side certificates.
#
# If "no" is specified, client certificates are not required and not accepted.
# If "optional" is specified, client certificates are accepted and must be
# valid if provided, but are not required.
#
# tls-auth-clients no
# tls-auth-clients optional

# By default, a Redis replica does not attempt to establish a TLS connection
# with its master.
#
# Use the following directive to enable TLS on replication links.
#
# tls-replication yes

# By default, the Redis Cluster bus uses a plain TCP connection. To enable
# TLS for the bus protocol, use the following directive:
#
# tls-cluster yes

# By default, only TLSv1.2 and TLSv1.3 are enabled and it is highly recommended
# that older formally deprecated versions are kept disabled to reduce the attack surface.
# You can explicitly specify TLS versions to support.
# Allowed values are case insensitive and include "TLSv1", "TLSv1.1", "TLSv1.2",
# "TLSv1.3" (OpenSSL >= 1.1.1) or any combination.
# To enable only TLSv1.2 and TLSv1.3, use:
#
# tls-protocols "TLSv1.2 TLSv1.3"

# Configure allowed ciphers.  See the ciphers(1ssl) manpage for more information
# about the syntax of this string.
#
# Note: this configuration applies only to <= TLSv1.2.
#
# tls-ciphers DEFAULT:!MEDIUM

# Configure allowed TLSv1.3 ciphersuites.  See the ciphers(1ssl) manpage for more
# information about the syntax of this string, and specifically for TLSv1.3
# ciphersuites.
#
# tls-ciphersuites TLS_CHACHA20_POLY1305_SHA256

# When choosing a cipher, use the server's preference instead of the client
# preference. By default, the server follows the client's preference.
#
# tls-prefer-server-ciphers yes

# By default, TLS session caching is enabled to allow faster and less expensive
# reconnections by clients that support it. Use the following directive to disable
# caching.
#
# tls-session-caching no

# Change the default number of TLS sessions cached. A zero value sets the cache
# to unlimited size. The default size is 20480.
#
# tls-session-cache-size 5000

# Change the default timeout of cached TLS sessions. The default timeout is 300
# seconds.
#
# tls-session-cache-timeout 60

################################# GENERAL #####################################

# By default Redis does not run as a daemon. Use 'yes' if you need it.
# Note that Redis will write a pid file in /var/run/redis.pid when daemonized.
# When Redis is supervised by upstart or systemd, this parameter has no impact.
daemonize no

# If you run Redis from upstart or systemd, Redis can interact with your
# supervision tree. Options:
#   supervised no      - no supervision interaction
#   supervised upstart - signal upstart by putting Redis into SIGSTOP mode
#                        requires "expect stop" in your upstart job config
#   supervised systemd - signal systemd by writing READY=1 to $NOTIFY_SOCKET
#                        on startup, and updating Redis status on a regular
#                        basis.
#   supervised auto    - detect upstart or systemd method based on
#                        UPSTART_JOB or NOTIFY_SOCKET environment variables
# Note: these supervision methods only signal "process is ready."
#       They do not enable continuous pings back to your supervisor.
#
# The default is "no". To run under upstart/systemd, you can simply uncomment
# the line below:
#
# supervised auto

# If a pid file is specified, Redis writes it where specified at startup
# and removes it at exit.
#
# When the server runs non daemonized, no pid file is created if none is
# specified in the configuration. When the server is daemonized, the pid file
# is used even if not specified, defaulting to "/var/run/redis.pid".
#
# Creating a pid file is best effort: if Redis is not able to create it
# nothing bad happens, the server will start and run normally.
#
# Note that on modern Linux systems "/run/redis.pid" is more conforming
# and should be used instead.
pidfile /var/run/redis_6379.pid

# Specify the server verbosity level.
# This can be one of:
# debug (a lot of information, useful for development/testing)
# verbose (many rarely useful info, but not a mess like the debug level)
# notice (moderately verbose, what you want in production probably)
# warning (only very important / critical messages are logged)
loglevel notice

# Specify the log file name. Also the empty string can be used to force
# Redis to log on the standard output. Note that if you use standard
# output for logging but daemonize, logs will be sent to /dev/null
logfile ""

# To enable logging to the system logger, just set 'syslog-enabled' to yes,
# and optionally update the other syslog parameters to suit your needs.
# syslog-enabled no

# Specify the syslog identity.
# syslog-ident redis

# Specify the syslog facility. Must be USER or between LOCAL0-LOCAL7.
# syslog-facility local0

# To disable the built in crash log, which will possibly produce cleaner core
# dumps when they are needed, uncomment the following:
#
# crash-log-enabled no

# To disable the fast memory check that's run as part of the crash log, which
# will possibly let redis terminate sooner, uncomment the following:
#
# crash-memcheck-enabled no

# Set the number of databases. The default database is DB 0, you can select
# a different one on a per-connection basis using SELECT <dbid> where
# dbid is a number between 0 and 'databases'-1
databases 16

# By default Redis shows an ASCII art logo only when started to log to the
# standard output and if the standard output is a TTY and syslog logging is
# disabled. Basically this means that normally a logo is displayed only in
# interactive sessions.
#
# However it is possible to force the pre-4.0 behavior and always show a
# ASCII art logo in startup logs by setting the following option to yes.
always-show-logo no

# By default, Redis modifies the process title (as seen in 'top' and 'ps') to
# provide some runtime information. It is possible to disable this and leave
# the process name as executed by setting the following to no.
set-proc-title yes

# When changing the process title, Redis uses the following template to construct
# the modified title.
#
# Template variables are specified in curly brackets. The following variables are
# supported:
#
# {title}           Name of process as executed if parent, or type of child process.
# {listen-addr}     Bind address or '*' followed by TCP or TLS port listening on, or
#                   Unix socket if only that's available.
# {server-mode}     Special mode, i.e. "[sentinel]" or "[cluster]".
# {port}            TCP port listening on, or 0.
# {tls-port}        TLS port listening on, or 0.
# {unixsocket}      Unix domain socket listening on, or "".
# {config-file}     Name of configuration file used.
#
proc-title-template "{title} {listen-addr} {server-mode}"

################################ SNAPSHOTTING  ################################

# Save the DB to disk.
#
# save <seconds> <changes>
#
# Redis will save the DB if both the given number of seconds and the given
# number of write operations against the DB occurred.
#
# Snapshotting can be completely disabled with a single empty string argument
# as in following example:
#
# save ""
#
# Unless specified otherwise, by default Redis will save the DB:
#   * After 3600 seconds (an hour) if at least 1 key changed
#   * After 300 seconds (5 minutes) if at least 100 keys changed
#   * After 60 seconds if at least 10000 keys changed
#
# You can set these explicitly by uncommenting the three following lines.
#
# save 3600 1
# save 300 100
# save 60 10000

# By default Redis will stop accepting writes if RDB snapshots are enabled
# (at least one save point) and the latest background save failed.
# This will make the user aware (in a hard way) that data is not persisting
# on disk properly, otherwise chances are that no one will notice and some
# disaster will happen.
#
# If the background saving process will start working again Redis will
# automatically allow writes again.
#
# However if you have setup your proper monitoring of the Redis server
# and persistence, you may want to disable this feature so that Redis will
# continue to work as usual even if there are problems with disk,
# permissions, and so forth.
stop-writes-on-bgsave-error yes

# Compress string objects using LZF when dump .rdb databases?
# By default compression is enabled as it's almost always a win.
# If you want to save some CPU in the saving child set it to 'no' but
# the dataset will likely be bigger if you have compressible values or keys.
rdbcompression yes

# Since version 5 of RDB a CRC64 checksum is placed at the end of the file.
# This makes the format more resistant to corruption but there is a performance
# hit to pay (around 10%) when saving and loading RDB files, so you can disable it
# for maximum performances.
#
# RDB files created with checksum disabled have a checksum of zero that will
# tell the loading code to skip the check.
rdbchecksum yes

# Enables or disables full sanitation checks for ziplist and listpack etc when
# loading an RDB or RESTORE payload. This reduces the chances of a assertion or
# crash later on while processing commands.
# Options:
#   no         - Never perform full sanitation
#   yes        - Always perform full sanitation
#   clients    - Perform full sanitation only for user connections.
#                Excludes: RDB files, RESTORE commands received from the master
#                connection, and client connections which have the
#                skip-sanitize-payload ACL flag.
# The default should be 'clients' but since it currently affects cluster
# resharding via MIGRATE, it is temporarily set to 'no' by default.
#
# sanitize-dump-payload no

# The filename where to dump the DB
dbfilename dump.rdb

# Remove RDB files used by replication in instances without persistence
# enabled. By default this option is disabled, however there are environments
# where for regulations or other security concerns, RDB files persisted on
# disk by masters in order to feed replicas, or stored on disk by replicas
# in order to load them for the initial synchronization, should be deleted
# ASAP. Note that this option ONLY WORKS in instances that have both AOF
# and RDB persistence disabled, otherwise is completely ignored.
#
# An alternative (and sometimes better) way to obtain the same effect is
# to use diskless replication on both master and replicas instances. However
# in the case of replicas, diskless is not always an option.
rdb-del-sync-files no

# The working directory.
#
# The DB will be written inside this directory, with the filename specified
# above using the 'dbfilename' configuration directive.
#
# The Append Only File will also be created inside this directory.
#
# Note that you must specify a directory here, not a file name.
dir ./

################################# REPLICATION #################################

# Master-Replica replication. Use replicaof to make a Redis instance a copy of
# another Redis server. A few things to understand ASAP about Redis replication.
#
#   +------------------+      +---------------+
#   |      Master      | ---> |    Replica    |
#   | (receive writes) |      |  (exact copy) |
#   +------------------+      +---------------+
#
# 1) Redis replication is asynchronous, but you can configure a master to
#    stop accepting writes if it appears to be not connected with at least
#    a given number of replicas.
# 2) Redis replicas are able to perform a partial resynchronization with the
#    master if the replication link is lost for a relatively small amount of
#    time. You may want to configure the replication backlog size (see the next
#    sections of this file) with a sensible value depending on your needs.
# 3) Replication is automatic and does not need user intervention. After a
#    network partition replicas automatically try to reconnect to masters
#    and resynchronize with them.
#
# replicaof <masterip> <masterport>

# If the master is password protected (using the "requirepass" configuration
# directive below) it is possible to tell the replica to authenticate before
# starting the replication synchronization process, otherwise the master will
# refuse the replica request.
#
# masterauth <master-password>
#
# However this is not enough if you are using Redis ACLs (for Redis version
# 6 or greater), and the default user is not capable of running the PSYNC
# command and/or other commands needed for replication. In this case it's
# better to configure a special user to use with replication, and specify the
# masteruser configuration as such:
#
# masteruser <username>
#
# When masteruser is specified, the replica will authenticate against its
# master using the new AUTH form: AUTH <username> <password>.

# When a replica loses its connection with the master, or when the replication
# is still in progress, the replica can act in two different ways:
#
# 1) if replica-serve-stale-data is set to 'yes' (the default) the replica will
#    still reply to client requests, possibly with out of date data, or the
#    data set may just be empty if this is the first synchronization.
#
# 2) If replica-serve-stale-data is set to 'no' the replica will reply with
#    an error "SYNC with master in progress" to all commands except:
#    INFO, REPLICAOF, AUTH, PING, SHUTDOWN, REPLCONF, ROLE, CONFIG, SUBSCRIBE,
#    UNSUBSCRIBE, PSUBSCRIBE, PUNSUBSCRIBE, PUBLISH, PUBSUB, COMMAND, POST,
#    HOST and LATENCY.
#
replica-serve-stale-data yes

# You can configure a replica instance to accept writes or not. Writing against
# a replica instance may be useful to store some ephemeral data (because data
# written on a replica will be easily deleted after resync with the master) but
# may also cause problems if clients are writing to it because of a
# misconfiguration.
#
# Since Redis 2.6 by default replicas are read-only.
#
# Note: read only replicas are not designed to be exposed to untrusted clients
# on the internet. It's just a protection layer against misuse of the instance.
# Still a read only replica exports by default all the administrative commands
# such as CONFIG, DEBUG, and so forth. To a limited extent you can improve
# security of read only replicas using 'rename-command' to shadow all the
# administrative / dangerous commands.
replica-read-only yes

# Replication SYNC strategy: disk or socket.
#
# New replicas and reconnecting replicas that are not able to continue the
# replication process just receiving differences, need to do what is called a
# "full synchronization". An RDB file is transmitted from the master to the
# replicas.
#
# The transmission can happen in two different ways:
#
# 1) Disk-backed: The Redis master creates a new process that writes the RDB
#                 file on disk. Later the file is transferred by the parent
#                 process to the replicas incrementally.
# 2) Diskless: The Redis master creates a new process that directly writes the
#              RDB file to replica sockets, without touching the disk at all.
#
# With disk-backed replication, while the RDB file is generated, more replicas
# can be queued and served with the RDB file as soon as the current child
# producing the RDB file finishes its work. With diskless replication instead
# once the transfer starts, new replicas arriving will be queued and a new
# transfer will start when the current one terminates.
#
# When diskless replication is used, the master waits a configurable amount of
# time (in seconds) before starting the transfer in the hope that multiple
# replicas will arrive and the transfer can be parallelized.
#
# With slow disks and fast (large bandwidth) networks, diskless replication
# works better.
repl-diskless-sync no

# When diskless replication is enabled, it is possible to configure the delay
# the server waits in order to spawn the child that transfers the RDB via socket
# to the replicas.
#
# This is important since once the transfer starts, it is not possible to serve
# new replicas arriving, that will be queued for the next RDB transfer, so the
# server waits a delay in order to let more replicas arrive.
#
# The delay is specified in seconds, and by default is 5 seconds. To disable
# it entirely just set it to 0 seconds and the transfer will start ASAP.
repl-diskless-sync-delay 5

# -----------------------------------------------------------------------------
# WARNING: RDB diskless load is experimental. Since in this setup the replica
# does not immediately store an RDB on disk, it may cause data loss during
# failovers. RDB diskless load + Redis modules not handling I/O reads may also
# cause Redis to abort in case of I/O errors during the initial synchronization
# stage with the master. Use only if you know what you are doing.
# -----------------------------------------------------------------------------
#
# Replica can load the RDB it reads from the replication link directly from the
# socket, or store the RDB to a file and read that file after it was completely
# received from the master.
#
# In many cases the disk is slower than the network, and storing and loading
# the RDB file may increase replication time (and even increase the master's
# Copy on Write memory and salve buffers).
# However, parsing the RDB file directly from the socket may mean that we have
# to flush the contents of the current database before the full rdb was
# received. For this reason we have the following options:
#
# "disabled"    - Don't use diskless load (store the rdb file to the disk first)
# "on-empty-db" - Use diskless load only when it is completely safe.
# "swapdb"      - Keep a copy of the current db contents in RAM while parsing
#                 the data directly from the socket. note that this requires
#                 sufficient memory, if you don't have it, you risk an OOM kill.
repl-diskless-load disabled

# Replicas send PINGs to server in a predefined interval. It's possible to
# change this interval with the repl_ping_replica_period option. The default
# value is 10 seconds.
#
# repl-ping-replica-period 10

# The following option sets the replication timeout for:
#
# 1) Bulk transfer I/O during SYNC, from the point of view of replica.
# 2) Master timeout from the point of view of replicas (data, pings).
# 3) Replica timeout from the point of view of masters (REPLCONF ACK pings).
#
# It is important to make sure that this value is greater than the value
# specified for repl-ping-replica-period otherwise a timeout will be detected
# every time there is low traffic between the master and the replica. The default
# value is 60 seconds.
#
# repl-timeout 60

# Disable TCP_NODELAY on the replica socket after SYNC?
#
# If you select "yes" Redis will use a smaller number of TCP packets and
# less bandwidth to send data to replicas. But this can add a delay for
# the data to appear on the replica side, up to 40 milliseconds with
# Linux kernels using a default configuration.
#
# If you select "no" the delay for data to appear on the replica side will
# be reduced but more bandwidth will be used for replication.
#
# By default we optimize for low latency, but in very high traffic conditions
# or when the master and replicas are many hops away, turning this to "yes" may
# be a good idea.
repl-disable-tcp-nodelay no

# Set the replication backlog size. The backlog is a buffer that accumulates
# replica data when replicas are disconnected for some time, so that when a
# replica wants to reconnect again, often a full resync is not needed, but a
# partial resync is enough, just passing the portion of data the replica
# missed while disconnected.
#
# The bigger the replication backlog, the longer the replica can endure the
# disconnect and later be able to perform a partial resynchronization.
#
# The backlog is only allocated if there is at least one replica connected.
#
# repl-backlog-size 1mb

# After a master has no connected replicas for some time, the backlog will be
# freed. The following option configures the amount of seconds that need to
# elapse, starting from the time the last replica disconnected, for the backlog
# buffer to be freed.
#
# Note that replicas never free the backlog for timeout, since they may be
# promoted to masters later, and should be able to correctly "partially
# resynchronize" with other replicas: hence they should always accumulate backlog.
#
# A value of 0 means to never release the backlog.
#
# repl-backlog-ttl 3600

# The replica priority is an integer number published by Redis in the INFO
# output. It is used by Redis Sentinel in order to select a replica to promote
# into a master if the master is no longer working correctly.
#
# A replica with a low priority number is considered better for promotion, so
# for instance if there are three replicas with priority 10, 100, 25 Sentinel
# will pick the one with priority 10, that is the lowest.
#
# However a special priority of 0 marks the replica as not able to perform the
# role of master, so a replica with priority of 0 will never be selected by
# Redis Sentinel for promotion.
#
# By default the priority is 100.
replica-priority 100

# -----------------------------------------------------------------------------
# By default, Redis Sentinel includes all replicas in its reports. A replica
# can be excluded from Redis Sentinel's announcements. An unannounced replica
# will be ignored by the 'sentinel replicas <master>' command and won't be
# exposed to Redis Sentinel's clients.
#
# This option does not change the behavior of replica-priority. Even with
# replica-announced set to 'no', the replica can be promoted to master. To
# prevent this behavior, set replica-priority to 0.
#
# replica-announced yes

# It is possible for a master to stop accepting writes if there are less than
# N replicas connected, having a lag less or equal than M seconds.
#
# The N replicas need to be in "online" state.
#
# The lag in seconds, that must be <= the specified value, is calculated from
# the last ping received from the replica, that is usually sent every second.
#
# This option does not GUARANTEE that N replicas will accept the write, but
# will limit the window of exposure for lost writes in case not enough replicas
# are available, to the specified number of seconds.
#
# For example to require at least 3 replicas with a lag <= 10 seconds use:
#
# min-replicas-to-write 3
# min-replicas-max-lag 10
#
# Setting one or the other to 0 disables the feature.
#
# By default min-replicas-to-write is set to 0 (feature disabled) and
# min-replicas-max-lag is set to 10.

# A Redis master is able to list the address and port of the attached
# replicas in different ways. For example the "INFO replication" section
# offers this information, which is used, among other tools, by
# Redis Sentinel in order to discover replica instances.
# Another place where this info is available is in the output of the
# "ROLE" command of a master.
#
# The listed IP address and port normally reported by a replica is
# obtained in the following way:
#
#   IP: The address is auto detected by checking the peer address
#   of the socket used by the replica to connect with the master.
#
#   Port: The port is communicated by the replica during the replication
#   handshake, and is normally the port that the replica is using to
#   listen for connections.
#
# However when port forwarding or Network Address Translation (NAT) is
# used, the replica may actually be reachable via different IP and port
# pairs. The following two options can be used by a replica in order to
# report to its master a specific set of IP and port, so that both INFO
# and ROLE will report those values.
#
# There is no need to use both the options if you need to override just
# the port or the IP address.
#
# replica-announce-ip 5.5.5.5
# replica-announce-port 1234

############################### KEYS TRACKING #################################

# Redis implements server assisted support for client side caching of values.
# This is implemented using an invalidation table that remembers, using
# a radix key indexed by key name, what clients have which keys. In turn
# this is used in order to send invalidation messages to clients. Please
# check this page to understand more about the feature:
#
#   https://redis.io/topics/client-side-caching
#
# When tracking is enabled for a client, all the read only queries are assumed
# to be cached: this will force Redis to store information in the invalidation
# table. When keys are modified, such information is flushed away, and
# invalidation messages are sent to the clients. However if the workload is
# heavily dominated by reads, Redis could use more and more memory in order
# to track the keys fetched by many clients.
#
# For this reason it is possible to configure a maximum fill value for the
# invalidation table. By default it is set to 1M of keys, and once this limit
# is reached, Redis will start to evict keys in the invalidation table
# even if they were not modified, just to reclaim memory: this will in turn
# force the clients to invalidate the cached values. Basically the table
# maximum size is a trade off between the memory you want to spend server
# side to track information about who cached what, and the ability of clients
# to retain cached objects in memory.
#
# If you set the value to 0, it means there are no limits, and Redis will
# retain as many keys as needed in the invalidation table.
# In the "stats" INFO section, you can find information about the number of
# keys in the invalidation table at every given moment.
#
# Note: when key tracking is used in broadcasting mode, no memory is used
# in the server side so this setting is useless.
#
# tracking-table-max-keys 1000000

################################## SECURITY ###################################

# Warning: since Redis is pretty fast, an outside user can try up to
# 1 million passwords per second against a modern box. This means that you
# should use very strong passwords, otherwise they will be very easy to break.
# Note that because the password is really a shared secret between the client
# and the server, and should not be memorized by any human, the password
# can be easily a long string from /dev/urandom or whatever, so by using a
# long and unguessable password no brute force attack will be possible.

# Redis ACL users are defined in the following format:
#
#   user <username> ... acl rules ...
#
# For example:
#
#   user worker +@list +@connection ~jobs:* on >ffa9203c493aa99
#
# The special username "default" is used for new connections. If this user
# has the "nopass" rule, then new connections will be immediately authenticated
# as the "default" user without the need of any password provided via the
# AUTH command. Otherwise if the "default" user is not flagged with "nopass"
# the connections will start in not authenticated state, and will require
# AUTH (or the HELLO command AUTH option) in order to be authenticated and
# start to work.
#
# The ACL rules that describe what a user can do are the following:
#
#  on           Enable the user: it is possible to authenticate as this user.
#  off          Disable the user: it's no longer possible to authenticate
#               with this user, however the already authenticated connections
#               will still work.
#  skip-sanitize-payload    RESTORE dump-payload sanitation is skipped.
#  sanitize-payload         RESTORE dump-payload is sanitized (default).
#  +<command>   Allow the execution of that command
#  -<command>   Disallow the execution of that command
#  +@<category> Allow the execution of all the commands in such category
#               with valid categories are like @admin, @set, @sortedset, ...
#               and so forth, see the full list in the server.c file where
#               the Redis command table is described and defined.
#               The special category @all means all the commands, but currently
#               present in the server, and that will be loaded in the future
#               via modules.
#  +<command>|subcommand    Allow a specific subcommand of an otherwise
#                           disabled command. Note that this form is not
#                           allowed as negative like -DEBUG|SEGFAULT, but
#                           only additive starting with "+".
#  allcommands  Alias for +@all. Note that it implies the ability to execute
#               all the future commands loaded via the modules system.
#  nocommands   Alias for -@all.
#  ~<pattern>   Add a pattern of keys that can be mentioned as part of
#               commands. For instance ~* allows all the keys. The pattern
#               is a glob-style pattern like the one of KEYS.
#               It is possible to specify multiple patterns.
#  allkeys      Alias for ~*
#  resetkeys    Flush the list of allowed keys patterns.
#  &<pattern>   Add a glob-style pattern of Pub/Sub channels that can be
#               accessed by the user. It is possible to specify multiple channel
#               patterns.
#  allchannels  Alias for &*
#  resetchannels            Flush the list of allowed channel patterns.
#  ><password>  Add this password to the list of valid password for the user.
#               For example >mypass will add "mypass" to the list.
#               This directive clears the "nopass" flag (see later).
#  <<password>  Remove this password from the list of valid passwords.
#  nopass       All the set passwords of the user are removed, and the user
#               is flagged as requiring no password: it means that every
#               password will work against this user. If this directive is
#               used for the default user, every new connection will be
#               immediately authenticated with the default user without
#               any explicit AUTH command required. Note that the "resetpass"
#               directive will clear this condition.
#  resetpass    Flush the list of allowed passwords. Moreover removes the
#               "nopass" status. After "resetpass" the user has no associated
#               passwords and there is no way to authenticate without adding
#               some password (or setting it as "nopass" later).
#  reset        Performs the following actions: resetpass, resetkeys, off,
#               -@all. The user returns to the same state it has immediately
#               after its creation.
#
# ACL rules can be specified in any order: for instance you can start with
# passwords, then flags, or key patterns. However note that the additive
# and subtractive rules will CHANGE MEANING depending on the ordering.
# For instance see the following example:
#
#   user alice on +@all -DEBUG ~* >somepassword
#
# This will allow "alice" to use all the commands with the exception of the
# DEBUG command, since +@all added all the commands to the set of the commands
# alice can use, and later DEBUG was removed. However if we invert the order
# of two ACL rules the result will be different:
#
#   user alice on -DEBUG +@all ~* >somepassword
#
# Now DEBUG was removed when alice had yet no commands in the set of allowed
# commands, later all the commands are added, so the user will be able to
# execute everything.
#
# Basically ACL rules are processed left-to-right.
#
# For more information about ACL configuration please refer to
# the Redis web site at https://redis.io/topics/acl

# ACL LOG
#
# The ACL Log tracks failed commands and authentication events associated
# with ACLs. The ACL Log is useful to troubleshoot failed commands blocked 
# by ACLs. The ACL Log is stored in memory. You can reclaim memory with 
# ACL LOG RESET. Define the maximum entry length of the ACL Log below.
acllog-max-len 128

# Using an external ACL file
#
# Instead of configuring users here in this file, it is possible to use
# a stand-alone file just listing users. The two methods cannot be mixed:
# if you configure users here and at the same time you activate the external
# ACL file, the server will refuse to start.
#
# The format of the external ACL user file is exactly the same as the
# format that is used inside redis.conf to describe users.
#
# aclfile /etc/redis/users.acl

# IMPORTANT NOTE: starting with Redis 6 "requirepass" is just a compatibility
# layer on top of the new ACL system. The option effect will be just setting
# the password for the default user. Clients will still authenticate using
# AUTH <password> as usually, or more explicitly with AUTH default <password>
# if they follow the new protocol: both will work.
#
# The requirepass is not compatable with aclfile option and the ACL LOAD
# command, these will cause requirepass to be ignored.
#
# requirepass foobared

# New users are initialized with restrictive permissions by default, via the
# equivalent of this ACL rule 'off resetkeys -@all'. Starting with Redis 6.2, it
# is possible to manage access to Pub/Sub channels with ACL rules as well. The
# default Pub/Sub channels permission if new users is controlled by the 
# acl-pubsub-default configuration directive, which accepts one of these values:
#
# allchannels: grants access to all Pub/Sub channels
# resetchannels: revokes access to all Pub/Sub channels
#
# To ensure backward compatibility while upgrading Redis 6.0, acl-pubsub-default
# defaults to the 'allchannels' permission.
#
# Future compatibility note: it is very likely that in a future version of Redis
# the directive's default of 'allchannels' will be changed to 'resetchannels' in
# order to provide better out-of-the-box Pub/Sub security. Therefore, it is
# recommended that you explicitly define Pub/Sub permissions for all users
# rather then rely on implicit default values. Once you've set explicit
# Pub/Sub for all existing users, you should uncomment the following line.
#
# acl-pubsub-default resetchannels

# Command renaming (DEPRECATED).
#
# ------------------------------------------------------------------------
# WARNING: avoid using this option if possible. Instead use ACLs to remove
# commands from the default user, and put them only in some admin user you
# create for administrative purposes.
# ------------------------------------------------------------------------
#
# It is possible to change the name of dangerous commands in a shared
# environment. For instance the CONFIG command may be renamed into something
# hard to guess so that it will still be available for internal-use tools
# but not available for general clients.
#
# Example:
#
# rename-command CONFIG b840fc02d524045429941cc15f59e41cb7be6c52
#
# It is also possible to completely kill a command by renaming it into
# an empty string:
#
# rename-command CONFIG ""
#
# Please note that changing the name of commands that are logged into the
# AOF file or transmitted to replicas may cause problems.

################################### CLIENTS ####################################

# Set the max number of connected clients at the same time. By default
# this limit is set to 10000 clients, however if the Redis server is not
# able to configure the process file limit to allow for the specified limit
# the max number of allowed clients is set to the current file limit
# minus 32 (as Redis reserves a few file descriptors for internal uses).
#
# Once the limit is reached Redis will close all the new connections sending
# an error 'max number of clients reached'.
#
# IMPORTANT: When Redis Cluster is used, the max number of connections is also
# shared with the cluster bus: every node in the cluster will use two
# connections, one incoming and another outgoing. It is important to size the
# limit accordingly in case of very large clusters.
#
# maxclients 10000

############################## MEMORY MANAGEMENT ################################

# Set a memory usage limit to the specified amount of bytes.
# When the memory limit is reached Redis will try to remove keys
# according to the eviction policy selected (see maxmemory-policy).
#
# If Redis can't remove keys according to the policy, or if the policy is
# set to 'noeviction', Redis will start to reply with errors to commands
# that would use more memory, like SET, LPUSH, and so on, and will continue
# to reply to read-only commands like GET.
#
# This option is usually useful when using Redis as an LRU or LFU cache, or to
# set a hard memory limit for an instance (using the 'noeviction' policy).
#
# WARNING: If you have replicas attached to an instance with maxmemory on,
# the size of the output buffers needed to feed the replicas are subtracted
# from the used memory count, so that network problems / resyncs will
# not trigger a loop where keys are evicted, and in turn the output
# buffer of replicas is full with DELs of keys evicted triggering the deletion
# of more keys, and so forth until the database is completely emptied.
#
# In short... if you have replicas attached it is suggested that you set a lower
# limit for maxmemory so that there is some free RAM on the system for replica
# output buffers (but this is not needed if the policy is 'noeviction').
#
# maxmemory <bytes>

# MAXMEMORY POLICY: how Redis will select what to remove when maxmemory
# is reached. You can select one from the following behaviors:
#
# volatile-lru -> Evict using approximated LRU, only keys with an expire set.
# allkeys-lru -> Evict any key using approximated LRU.
# volatile-lfu -> Evict using approximated LFU, only keys with an expire set.
# allkeys-lfu -> Evict any key using approximated LFU.
# volatile-random -> Remove a random key having an expire set.
# allkeys-random -> Remove a random key, any key.
# volatile-ttl -> Remove the key with the nearest expire time (minor TTL)
# noeviction -> Don't evict anything, just return an error on write operations.
#
# LRU means Least Recently Used
# LFU means Least Frequently Used
#
# Both LRU, LFU and volatile-ttl are implemented using approximated
# randomized algorithms.
#
# Note: with any of the above policies, when there are no suitable keys for
# eviction, Redis will return an error on write operations that require
# more memory. These are usually commands that create new keys, add data or
# modify existing keys. A few examples are: SET, INCR, HSET, LPUSH, SUNIONSTORE,
# SORT (due to the STORE argument), and EXEC (if the transaction includes any
# command that requires memory).
#
# The default is:
#
# maxmemory-policy noeviction

# LRU, LFU and minimal TTL algorithms are not precise algorithms but approximated
# algorithms (in order to save memory), so you can tune it for speed or
# accuracy. By default Redis will check five keys and pick the one that was
# used least recently, you can change the sample size using the following
# configuration directive.
#
# The default of 5 produces good enough results. 10 Approximates very closely
# true LRU but costs more CPU. 3 is faster but not very accurate.
#
# maxmemory-samples 5

# Eviction processing is designed to function well with the default setting.
# If there is an unusually large amount of write traffic, this value may need to
# be increased.  Decreasing this value may reduce latency at the risk of 
# eviction processing effectiveness
#   0 = minimum latency, 10 = default, 100 = process without regard to latency
#
# maxmemory-eviction-tenacity 10

# Starting from Redis 5, by default a replica will ignore its maxmemory setting
# (unless it is promoted to master after a failover or manually). It means
# that the eviction of keys will be just handled by the master, sending the
# DEL commands to the replica as keys evict in the master side.
#
# This behavior ensures that masters and replicas stay consistent, and is usually
# what you want, however if your replica is writable, or you want the replica
# to have a different memory setting, and you are sure all the writes performed
# to the replica are idempotent, then you may change this default (but be sure
# to understand what you are doing).
#
# Note that since the replica by default does not evict, it may end using more
# memory than the one set via maxmemory (there are certain buffers that may
# be larger on the replica, or data structures may sometimes take more memory
# and so forth). So make sure you monitor your replicas and make sure they
# have enough memory to never hit a real out-of-memory condition before the
# master hits the configured maxmemory setting.
#
# replica-ignore-maxmemory yes

# Redis reclaims expired keys in two ways: upon access when those keys are
# found to be expired, and also in background, in what is called the
# "active expire key". The key space is slowly and interactively scanned
# looking for expired keys to reclaim, so that it is possible to free memory
# of keys that are expired and will never be accessed again in a short time.
#
# The default effort of the expire cycle will try to avoid having more than
# ten percent of expired keys still in memory, and will try to avoid consuming
# more than 25% of total memory and to add latency to the system. However
# it is possible to increase the expire "effort" that is normally set to
# "1", to a greater value, up to the value "10". At its maximum value the
# system will use more CPU, longer cycles (and technically may introduce
# more latency), and will tolerate less already expired keys still present
# in the system. It's a tradeoff between memory, CPU and latency.
#
# active-expire-effort 1

############################# LAZY FREEING ####################################

# Redis has two primitives to delete keys. One is called DEL and is a blocking
# deletion of the object. It means that the server stops processing new commands
# in order to reclaim all the memory associated with an object in a synchronous
# way. If the key deleted is associated with a small object, the time needed
# in order to execute the DEL command is very small and comparable to most other
# O(1) or O(log_N) commands in Redis. However if the key is associated with an
# aggregated value containing millions of elements, the server can block for
# a long time (even seconds) in order to complete the operation.
#
# For the above reasons Redis also offers non blocking deletion primitives
# such as UNLINK (non blocking DEL) and the ASYNC option of FLUSHALL and
# FLUSHDB commands, in order to reclaim memory in background. Those commands
# are executed in constant time. Another thread will incrementally free the
# object in the background as fast as possible.
#
# DEL, UNLINK and ASYNC option of FLUSHALL and FLUSHDB are user-controlled.
# It's up to the design of the application to understand when it is a good
# idea to use one or the other. However the Redis server sometimes has to
# delete keys or flush the whole database as a side effect of other operations.
# Specifically Redis deletes objects independently of a user call in the
# following scenarios:
#
# 1) On eviction, because of the maxmemory and maxmemory policy configurations,
#    in order to make room for new data, without going over the specified
#    memory limit.
# 2) Because of expire: when a key with an associated time to live (see the
#    EXPIRE command) must be deleted from memory.
# 3) Because of a side effect of a command that stores data on a key that may
#    already exist. For example the RENAME command may delete the old key
#    content when it is replaced with another one. Similarly SUNIONSTORE
#    or SORT with STORE option may delete existing keys. The SET command
#    itself removes any old content of the specified key in order to replace
#    it with the specified string.
# 4) During replication, when a replica performs a full resynchronization with
#    its master, the content of the whole database is removed in order to
#    load the RDB file just transferred.
#
# In all the above cases the default is to delete objects in a blocking way,
# like if DEL was called. However you can configure each case specifically
# in order to instead release memory in a non-blocking way like if UNLINK
# was called, using the following configuration directives.

lazyfree-lazy-eviction no
lazyfree-lazy-expire no
lazyfree-lazy-server-del no
replica-lazy-flush no

# It is also possible, for the case when to replace the user code DEL calls
# with UNLINK calls is not easy, to modify the default behavior of the DEL
# command to act exactly like UNLINK, using the following configuration
# directive:

lazyfree-lazy-user-del no

# FLUSHDB, FLUSHALL, and SCRIPT FLUSH support both asynchronous and synchronous
# deletion, which can be controlled by passing the [SYNC|ASYNC] flags into the
# commands. When neither flag is passed, this directive will be used to determine
# if the data should be deleted asynchronously.

lazyfree-lazy-user-flush no

################################ THREADED I/O #################################

# Redis is mostly single threaded, however there are certain threaded
# operations such as UNLINK, slow I/O accesses and other things that are
# performed on side threads.
#
# Now it is also possible to handle Redis clients socket reads and writes
# in different I/O threads. Since especially writing is so slow, normally
# Redis users use pipelining in order to speed up the Redis performances per
# core, and spawn multiple instances in order to scale more. Using I/O
# threads it is possible to easily speedup two times Redis without resorting
# to pipelining nor sharding of the instance.
#
# By default threading is disabled, we suggest enabling it only in machines
# that have at least 4 or more cores, leaving at least one spare core.
# Using more than 8 threads is unlikely to help much. We also recommend using
# threaded I/O only if you actually have performance problems, with Redis
# instances being able to use a quite big percentage of CPU time, otherwise
# there is no point in using this feature.
#
# So for instance if you have a four cores boxes, try to use 2 or 3 I/O
# threads, if you have a 8 cores, try to use 6 threads. In order to
# enable I/O threads use the following configuration directive:
#
# io-threads 4
#
# Setting io-threads to 1 will just use the main thread as usual.
# When I/O threads are enabled, we only use threads for writes, that is
# to thread the write(2) syscall and transfer the client buffers to the
# socket. However it is also possible to enable threading of reads and
# protocol parsing using the following configuration directive, by setting
# it to yes:
#
# io-threads-do-reads no
#
# Usually threading reads doesn't help much.
#
# NOTE 1: This configuration directive cannot be changed at runtime via
# CONFIG SET. Aso this feature currently does not work when SSL is
# enabled.
#
# NOTE 2: If you want to test the Redis speedup using redis-benchmark, make
# sure you also run the benchmark itself in threaded mode, using the
# --threads option to match the number of Redis threads, otherwise you'll not
# be able to notice the improvements.

############################ KERNEL OOM CONTROL ##############################

# On Linux, it is possible to hint the kernel OOM killer on what processes
# should be killed first when out of memory.
#
# Enabling this feature makes Redis actively control the oom_score_adj value
# for all its processes, depending on their role. The default scores will
# attempt to have background child processes killed before all others, and
# replicas killed before masters.
#
# Redis supports three options:
#
# no:       Don't make changes to oom-score-adj (default).
# yes:      Alias to "relative" see below.
# absolute: Values in oom-score-adj-values are written as is to the kernel.
# relative: Values are used relative to the initial value of oom_score_adj when
#           the server starts and are then clamped to a range of -1000 to 1000.
#           Because typically the initial value is 0, they will often match the
#           absolute values.
oom-score-adj no

# When oom-score-adj is used, this directive controls the specific values used
# for master, replica and background child processes. Values range -2000 to
# 2000 (higher means more likely to be killed).
#
# Unprivileged processes (not root, and without CAP_SYS_RESOURCE capabilities)
# can freely increase their value, but not decrease it below its initial
# settings. This means that setting oom-score-adj to "relative" and setting the
# oom-score-adj-values to positive values will always succeed.
oom-score-adj-values 0 200 800


#################### KERNEL transparent hugepage CONTROL ######################

# Usually the kernel Transparent Huge Pages control is set to "madvise" or
# or "never" by default (/sys/kernel/mm/transparent_hugepage/enabled), in which
# case this config has no effect. On systems in which it is set to "always",
# redis will attempt to disable it specifically for the redis process in order
# to avoid latency problems specifically with fork(2) and CoW.
# If for some reason you prefer to keep it enabled, you can set this config to
# "no" and the kernel global to "always".

disable-thp yes

############################## APPEND ONLY MODE ###############################

# By default Redis asynchronously dumps the dataset on disk. This mode is
# good enough in many applications, but an issue with the Redis process or
# a power outage may result into a few minutes of writes lost (depending on
# the configured save points).
#
# The Append Only File is an alternative persistence mode that provides
# much better durability. For instance using the default data fsync policy
# (see later in the config file) Redis can lose just one second of writes in a
# dramatic event like a server power outage, or a single write if something
# wrong with the Redis process itself happens, but the operating system is
# still running correctly.
#
# AOF and RDB persistence can be enabled at the same time without problems.
# If the AOF is enabled on startup Redis will load the AOF, that is the file
# with the better durability guarantees.
#
# Please check https://redis.io/topics/persistence for more information.

appendonly yes

# The name of the append only file (default: "appendonly.aof")

appendfilename "appendonly.aof"

# The fsync() call tells the Operating System to actually write data on disk
# instead of waiting for more data in the output buffer. Some OS will really flush
# data on disk, some other OS will just try to do it ASAP.
#
# Redis supports three different modes:
#
# no: don't fsync, just let the OS flush the data when it wants. Faster.
# always: fsync after every write to the append only log. Slow, Safest.
# everysec: fsync only one time every second. Compromise.
#
# The default is "everysec", as that's usually the right compromise between
# speed and data safety. It's up to you to understand if you can relax this to
# "no" that will let the operating system flush the output buffer when
# it wants, for better performances (but if you can live with the idea of
# some data loss consider the default persistence mode that's snapshotting),
# or on the contrary, use "always" that's very slow but a bit safer than
# everysec.
#
# More details please check the following article:
# http://antirez.com/post/redis-persistence-demystified.html
#
# If unsure, use "everysec".

# appendfsync always
appendfsync everysec
# appendfsync no

# When the AOF fsync policy is set to always or everysec, and a background
# saving process (a background save or AOF log background rewriting) is
# performing a lot of I/O against the disk, in some Linux configurations
# Redis may block too long on the fsync() call. Note that there is no fix for
# this currently, as even performing fsync in a different thread will block
# our synchronous write(2) call.
#
# In order to mitigate this problem it's possible to use the following option
# that will prevent fsync() from being called in the main process while a
# BGSAVE or BGREWRITEAOF is in progress.
#
# This means that while another child is saving, the durability of Redis is
# the same as "appendfsync none". In practical terms, this means that it is
# possible to lose up to 30 seconds of log in the worst scenario (with the
# default Linux settings).
#
# If you have latency problems turn this to "yes". Otherwise leave it as
# "no" that is the safest pick from the point of view of durability.

no-appendfsync-on-rewrite no

# Automatic rewrite of the append only file.
# Redis is able to automatically rewrite the log file implicitly calling
# BGREWRITEAOF when the AOF log size grows by the specified percentage.
#
# This is how it works: Redis remembers the size of the AOF file after the
# latest rewrite (if no rewrite has happened since the restart, the size of
# the AOF at startup is used).
#
# This base size is compared to the current size. If the current size is
# bigger than the specified percentage, the rewrite is triggered. Also
# you need to specify a minimal size for the AOF file to be rewritten, this
# is useful to avoid rewriting the AOF file even if the percentage increase
# is reached but it is still pretty small.
#
# Specify a percentage of zero in order to disable the automatic AOF
# rewrite feature.

auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb

# An AOF file may be found to be truncated at the end during the Redis
# startup process, when the AOF data gets loaded back into memory.
# This may happen when the system where Redis is running
# crashes, especially when an ext4 filesystem is mounted without the
# data=ordered option (however this can't happen when Redis itself
# crashes or aborts but the operating system still works correctly).
#
# Redis can either exit with an error when this happens, or load as much
# data as possible (the default now) and start if the AOF file is found
# to be truncated at the end. The following option controls this behavior.
#
# If aof-load-truncated is set to yes, a truncated AOF file is loaded and
# the Redis server starts emitting a log to inform the user of the event.
# Otherwise if the option is set to no, the server aborts with an error
# and refuses to start. When the option is set to no, the user requires
# to fix the AOF file using the "redis-check-aof" utility before to restart
# the server.
#
# Note that if the AOF file will be found to be corrupted in the middle
# the server will still exit with an error. This option only applies when
# Redis will try to read more data from the AOF file but not enough bytes
# will be found.
aof-load-truncated yes

# When rewriting the AOF file, Redis is able to use an RDB preamble in the
# AOF file for faster rewrites and recoveries. When this option is turned
# on the rewritten AOF file is composed of two different stanzas:
#
#   [RDB file][AOF tail]
#
# When loading, Redis recognizes that the AOF file starts with the "REDIS"
# string and loads the prefixed RDB file, then continues loading the AOF
# tail.
aof-use-rdb-preamble yes

################################ LUA SCRIPTING  ###############################

# Max execution time of a Lua script in milliseconds.
#
# If the maximum execution time is reached Redis will log that a script is
# still in execution after the maximum allowed time and will start to
# reply to queries with an error.
#
# When a long running script exceeds the maximum execution time only the
# SCRIPT KILL and SHUTDOWN NOSAVE commands are available. The first can be
# used to stop a script that did not yet call any write commands. The second
# is the only way to shut down the server in the case a write command was
# already issued by the script but the user doesn't want to wait for the natural
# termination of the script.
#
# Set it to 0 or a negative value for unlimited execution without warnings.
lua-time-limit 5000

################################ REDIS CLUSTER  ###############################

# Normal Redis instances can't be part of a Redis Cluster; only nodes that are
# started as cluster nodes can. In order to start a Redis instance as a
# cluster node enable the cluster support uncommenting the following:
#
# cluster-enabled yes

# Every cluster node has a cluster configuration file. This file is not
# intended to be edited by hand. It is created and updated by Redis nodes.
# Every Redis Cluster node requires a different cluster configuration file.
# Make sure that instances running in the same system do not have
# overlapping cluster configuration file names.
#
# cluster-config-file nodes-6379.conf

# Cluster node timeout is the amount of milliseconds a node must be unreachable
# for it to be considered in failure state.
# Most other internal time limits are a multiple of the node timeout.
#
# cluster-node-timeout 15000

# A replica of a failing master will avoid to start a failover if its data
# looks too old.
#
# There is no simple way for a replica to actually have an exact measure of
# its "data age", so the following two checks are performed:
#
# 1) If there are multiple replicas able to failover, they exchange messages
#    in order to try to give an advantage to the replica with the best
#    replication offset (more data from the master processed).
#    Replicas will try to get their rank by offset, and apply to the start
#    of the failover a delay proportional to their rank.
#
# 2) Every single replica computes the time of the last interaction with
#    its master. This can be the last ping or command received (if the master
#    is still in the "connected" state), or the time that elapsed since the
#    disconnection with the master (if the replication link is currently down).
#    If the last interaction is too old, the replica will not try to failover
#    at all.
#
# The point "2" can be tuned by user. Specifically a replica will not perform
# the failover if, since the last interaction with the master, the time
# elapsed is greater than:
#
#   (node-timeout * cluster-replica-validity-factor) + repl-ping-replica-period
#
# So for example if node-timeout is 30 seconds, and the cluster-replica-validity-factor
# is 10, and assuming a default repl-ping-replica-period of 10 seconds, the
# replica will not try to failover if it was not able to talk with the master
# for longer than 310 seconds.
#
# A large cluster-replica-validity-factor may allow replicas with too old data to failover
# a master, while a too small value may prevent the cluster from being able to
# elect a replica at all.
#
# For maximum availability, it is possible to set the cluster-replica-validity-factor
# to a value of 0, which means, that replicas will always try to failover the
# master regardless of the last time they interacted with the master.
# (However they'll always try to apply a delay proportional to their
# offset rank).
#
# Zero is the only value able to guarantee that when all the partitions heal
# the cluster will always be able to continue.
#
# cluster-replica-validity-factor 10

# Cluster replicas are able to migrate to orphaned masters, that are masters
# that are left without working replicas. This improves the cluster ability
# to resist to failures as otherwise an orphaned master can't be failed over
# in case of failure if it has no working replicas.
#
# Replicas migrate to orphaned masters only if there are still at least a
# given number of other working replicas for their old master. This number
# is the "migration barrier". A migration barrier of 1 means that a replica
# will migrate only if there is at least 1 other working replica for its master
# and so forth. It usually reflects the number of replicas you want for every
# master in your cluster.
#
# Default is 1 (replicas migrate only if their masters remain with at least
# one replica). To disable migration just set it to a very large value or
# set cluster-allow-replica-migration to 'no'.
# A value of 0 can be set but is useful only for debugging and dangerous
# in production.
#
# cluster-migration-barrier 1

# Turning off this option allows to use less automatic cluster configuration.
# It both disables migration to orphaned masters and migration from masters
# that became empty.
#
# Default is 'yes' (allow automatic migrations).
#
# cluster-allow-replica-migration yes

# By default Redis Cluster nodes stop accepting queries if they detect there
# is at least a hash slot uncovered (no available node is serving it).
# This way if the cluster is partially down (for example a range of hash slots
# are no longer covered) all the cluster becomes, eventually, unavailable.
# It automatically returns available as soon as all the slots are covered again.
#
# However sometimes you want the subset of the cluster which is working,
# to continue to accept queries for the part of the key space that is still
# covered. In order to do so, just set the cluster-require-full-coverage
# option to no.
#
# cluster-require-full-coverage yes

# This option, when set to yes, prevents replicas from trying to failover its
# master during master failures. However the replica can still perform a
# manual failover, if forced to do so.
#
# This is useful in different scenarios, especially in the case of multiple
# data center operations, where we want one side to never be promoted if not
# in the case of a total DC failure.
#
# cluster-replica-no-failover no

# This option, when set to yes, allows nodes to serve read traffic while the
# the cluster is in a down state, as long as it believes it owns the slots. 
#
# This is useful for two cases.  The first case is for when an application 
# doesn't require consistency of data during node failures or network partitions.
# One example of this is a cache, where as long as the node has the data it
# should be able to serve it. 
#
# The second use case is for configurations that don't meet the recommended  
# three shards but want to enable cluster mode and scale later. A 
# master outage in a 1 or 2 shard configuration causes a read/write outage to the
# entire cluster without this option set, with it set there is only a write outage.
# Without a quorum of masters, slot ownership will not change automatically. 
#
# cluster-allow-reads-when-down no

# In order to setup your cluster make sure to read the documentation
# available at https://redis.io web site.

########################## CLUSTER DOCKER/NAT support  ########################

# In certain deployments, Redis Cluster nodes address discovery fails, because
# addresses are NAT-ted or because ports are forwarded (the typical case is
# Docker and other containers).
#
# In order to make Redis Cluster working in such environments, a static
# configuration where each node knows its public address is needed. The
# following four options are used for this scope, and are:
#
# * cluster-announce-ip
# * cluster-announce-port
# * cluster-announce-tls-port
# * cluster-announce-bus-port
#
# Each instructs the node about its address, client ports (for connections
# without and with TLS) and cluster message bus port. The information is then
# published in the header of the bus packets so that other nodes will be able to
# correctly map the address of the node publishing the information.
#
# If cluster-tls is set to yes and cluster-announce-tls-port is omitted or set
# to zero, then cluster-announce-port refers to the TLS port. Note also that
# cluster-announce-tls-port has no effect if cluster-tls is set to no.
#
# If the above options are not used, the normal Redis Cluster auto-detection
# will be used instead.
#
# Note that when remapped, the bus port may not be at the fixed offset of
# clients port + 10000, so you can specify any port and bus-port depending
# on how they get remapped. If the bus-port is not set, a fixed offset of
# 10000 will be used as usual.
#
# Example:
#
# cluster-announce-ip 10.1.1.5
# cluster-announce-tls-port 6379
# cluster-announce-port 0
# cluster-announce-bus-port 6380

################################## SLOW LOG ###################################

# The Redis Slow Log is a system to log queries that exceeded a specified
# execution time. The execution time does not include the I/O operations
# like talking with the client, sending the reply and so forth,
# but just the time needed to actually execute the command (this is the only
# stage of command execution where the thread is blocked and can not serve
# other requests in the meantime).
#
# You can configure the slow log with two parameters: one tells Redis
# what is the execution time, in microseconds, to exceed in order for the
# command to get logged, and the other parameter is the length of the
# slow log. When a new command is logged the oldest one is removed from the
# queue of logged commands.

# The following time is expressed in microseconds, so 1000000 is equivalent
# to one second. Note that a negative number disables the slow log, while
# a value of zero forces the logging of every command.
slowlog-log-slower-than 10000

# There is no limit to this length. Just be aware that it will consume memory.
# You can reclaim memory used by the slow log with SLOWLOG RESET.
slowlog-max-len 128

################################ LATENCY MONITOR ##############################

# The Redis latency monitoring subsystem samples different operations
# at runtime in order to collect data related to possible sources of
# latency of a Redis instance.
#
# Via the LATENCY command this information is available to the user that can
# print graphs and obtain reports.
#
# The system only logs operations that were performed in a time equal or
# greater than the amount of milliseconds specified via the
# latency-monitor-threshold configuration directive. When its value is set
# to zero, the latency monitor is turned off.
#
# By default latency monitoring is disabled since it is mostly not needed
# if you don't have latency issues, and collecting data has a performance
# impact, that while very small, can be measured under big load. Latency
# monitoring can easily be enabled at runtime using the command
# "CONFIG SET latency-monitor-threshold <milliseconds>" if needed.
latency-monitor-threshold 0

############################# EVENT NOTIFICATION ##############################

# Redis can notify Pub/Sub clients about events happening in the key space.
# This feature is documented at https://redis.io/topics/notifications
#
# For instance if keyspace events notification is enabled, and a client
# performs a DEL operation on key "foo" stored in the Database 0, two
# messages will be published via Pub/Sub:
#
# PUBLISH __keyspace@0__:foo del
# PUBLISH __keyevent@0__:del foo
#
# It is possible to select the events that Redis will notify among a set
# of classes. Every class is identified by a single character:
#
#  K     Keyspace events, published with __keyspace@<db>__ prefix.
#  E     Keyevent events, published with __keyevent@<db>__ prefix.
#  g     Generic commands (non-type specific) like DEL, EXPIRE, RENAME, ...
#  $     String commands
#  l     List commands
#  s     Set commands
#  h     Hash commands
#  z     Sorted set commands
#  x     Expired events (events generated every time a key expires)
#  e     Evicted events (events generated when a key is evicted for maxmemory)
#  t     Stream commands
#  d     Module key type events
#  m     Key-miss events (Note: It is not included in the 'A' class)
#  A     Alias for g$lshzxetd, so that the "AKE" string means all the events
#        (Except key-miss events which are excluded from 'A' due to their
#         unique nature).
#
#  The "notify-keyspace-events" takes as argument a string that is composed
#  of zero or multiple characters. The empty string means that notifications
#  are disabled.
#
#  Example: to enable list and generic events, from the point of view of the
#           event name, use:
#
#  notify-keyspace-events Elg
#
#  Example 2: to get the stream of the expired keys subscribing to channel
#             name __keyevent@0__:expired use:
#
#  notify-keyspace-events Ex
#
#  By default all notifications are disabled because most users don't need
#  this feature and the feature has some overhead. Note that if you don't
#  specify at least one of K or E, no events will be delivered.
notify-keyspace-events ""

############################### GOPHER SERVER #################################

# Redis contains an implementation of the Gopher protocol, as specified in
# the RFC 1436 (https://www.ietf.org/rfc/rfc1436.txt).
#
# The Gopher protocol was very popular in the late '90s. It is an alternative
# to the web, and the implementation both server and client side is so simple
# that the Redis server has just 100 lines of code in order to implement this
# support.
#
# What do you do with Gopher nowadays? Well Gopher never *really* died, and
# lately there is a movement in order for the Gopher more hierarchical content
# composed of just plain text documents to be resurrected. Some want a simpler
# internet, others believe that the mainstream internet became too much
# controlled, and it's cool to create an alternative space for people that
# want a bit of fresh air.
#
# Anyway for the 10nth birthday of the Redis, we gave it the Gopher protocol
# as a gift.
#
# --- HOW IT WORKS? ---
#
# The Redis Gopher support uses the inline protocol of Redis, and specifically
# two kind of inline requests that were anyway illegal: an empty request
# or any request that starts with "/" (there are no Redis commands starting
# with such a slash). Normal RESP2/RESP3 requests are completely out of the
# path of the Gopher protocol implementation and are served as usual as well.
#
# If you open a connection to Redis when Gopher is enabled and send it
# a string like "/foo", if there is a key named "/foo" it is served via the
# Gopher protocol.
#
# In order to create a real Gopher "hole" (the name of a Gopher site in Gopher
# talking), you likely need a script like the following:
#
#   https://github.com/antirez/gopher2redis
#
# --- SECURITY WARNING ---
#
# If you plan to put Redis on the internet in a publicly accessible address
# to server Gopher pages MAKE SURE TO SET A PASSWORD to the instance.
# Once a password is set:
#
#   1. The Gopher server (when enabled, not by default) will still serve
#      content via Gopher.
#   2. However other commands cannot be called before the client will
#      authenticate.
#
# So use the 'requirepass' option to protect your instance.
#
# Note that Gopher is not currently supported when 'io-threads-do-reads'
# is enabled.
#
# To enable Gopher support, uncomment the following line and set the option
# from no (the default) to yes.
#
# gopher-enabled no

############################### ADVANCED CONFIG ###############################

# Hashes are encoded using a memory efficient data structure when they have a
# small number of entries, and the biggest entry does not exceed a given
# threshold. These thresholds can be configured using the following directives.
hash-max-ziplist-entries 512
hash-max-ziplist-value 64

# Lists are also encoded in a special way to save a lot of space.
# The number of entries allowed per internal list node can be specified
# as a fixed maximum size or a maximum number of elements.
# For a fixed maximum size, use -5 through -1, meaning:
# -5: max size: 64 Kb  <-- not recommended for normal workloads
# -4: max size: 32 Kb  <-- not recommended
# -3: max size: 16 Kb  <-- probably not recommended
# -2: max size: 8 Kb   <-- good
# -1: max size: 4 Kb   <-- good
# Positive numbers mean store up to _exactly_ that number of elements
# per list node.
# The highest performing option is usually -2 (8 Kb size) or -1 (4 Kb size),
# but if your use case is unique, adjust the settings as necessary.
list-max-ziplist-size -2

# Lists may also be compressed.
# Compress depth is the number of quicklist ziplist nodes from *each* side of
# the list to *exclude* from compression.  The head and tail of the list
# are always uncompressed for fast push/pop operations.  Settings are:
# 0: disable all list compression
# 1: depth 1 means "don't start compressing until after 1 node into the list,
#    going from either the head or tail"
#    So: [head]->node->node->...->node->[tail]
#    [head], [tail] will always be uncompressed; inner nodes will compress.
# 2: [head]->[next]->node->node->...->node->[prev]->[tail]
#    2 here means: don't compress head or head->next or tail->prev or tail,
#    but compress all nodes between them.
# 3: [head]->[next]->[next]->node->node->...->node->[prev]->[prev]->[tail]
# etc.
list-compress-depth 0

# Sets have a special encoding in just one case: when a set is composed
# of just strings that happen to be integers in radix 10 in the range
# of 64 bit signed integers.
# The following configuration setting sets the limit in the size of the
# set in order to use this special memory saving encoding.
set-max-intset-entries 512

# Similarly to hashes and lists, sorted sets are also specially encoded in
# order to save a lot of space. This encoding is only used when the length and
# elements of a sorted set are below the following limits:
zset-max-ziplist-entries 128
zset-max-ziplist-value 64

# HyperLogLog sparse representation bytes limit. The limit includes the
# 16 bytes header. When an HyperLogLog using the sparse representation crosses
# this limit, it is converted into the dense representation.
#
# A value greater than 16000 is totally useless, since at that point the
# dense representation is more memory efficient.
#
# The suggested value is ~ 3000 in order to have the benefits of
# the space efficient encoding without slowing down too much PFADD,
# which is O(N) with the sparse encoding. The value can be raised to
# ~ 10000 when CPU is not a concern, but space is, and the data set is
# composed of many HyperLogLogs with cardinality in the 0 - 15000 range.
hll-sparse-max-bytes 3000

# Streams macro node max size / items. The stream data structure is a radix
# tree of big nodes that encode multiple items inside. Using this configuration
# it is possible to configure how big a single node can be in bytes, and the
# maximum number of items it may contain before switching to a new node when
# appending new stream entries. If any of the following settings are set to
# zero, the limit is ignored, so for instance it is possible to set just a
# max entries limit by setting max-bytes to 0 and max-entries to the desired
# value.
stream-node-max-bytes 4096
stream-node-max-entries 100

# Active rehashing uses 1 millisecond every 100 milliseconds of CPU time in
# order to help rehashing the main Redis hash table (the one mapping top-level
# keys to values). The hash table implementation Redis uses (see dict.c)
# performs a lazy rehashing: the more operation you run into a hash table
# that is rehashing, the more rehashing "steps" are performed, so if the
# server is idle the rehashing is never complete and some more memory is used
# by the hash table.
#
# The default is to use this millisecond 10 times every second in order to
# actively rehash the main dictionaries, freeing memory when possible.
#
# If unsure:
# use "activerehashing no" if you have hard latency requirements and it is
# not a good thing in your environment that Redis can reply from time to time
# to queries with 2 milliseconds delay.
#
# use "activerehashing yes" if you don't have such hard requirements but
# want to free memory asap when possible.
activerehashing yes

# The client output buffer limits can be used to force disconnection of clients
# that are not reading data from the server fast enough for some reason (a
# common reason is that a Pub/Sub client can't consume messages as fast as the
# publisher can produce them).
#
# The limit can be set differently for the three different classes of clients:
#
# normal -> normal clients including MONITOR clients
# replica  -> replica clients
# pubsub -> clients subscribed to at least one pubsub channel or pattern
#
# The syntax of every client-output-buffer-limit directive is the following:
#
# client-output-buffer-limit <class> <hard limit> <soft limit> <soft seconds>
#
# A client is immediately disconnected once the hard limit is reached, or if
# the soft limit is reached and remains reached for the specified number of
# seconds (continuously).
# So for instance if the hard limit is 32 megabytes and the soft limit is
# 16 megabytes / 10 seconds, the client will get disconnected immediately
# if the size of the output buffers reach 32 megabytes, but will also get
# disconnected if the client reaches 16 megabytes and continuously overcomes
# the limit for 10 seconds.
#
# By default normal clients are not limited because they don't receive data
# without asking (in a push way), but just after a request, so only
# asynchronous clients may create a scenario where data is requested faster
# than it can read.
#
# Instead there is a default limit for pubsub and replica clients, since
# subscribers and replicas receive data in a push fashion.
#
# Both the hard or the soft limit can be disabled by setting them to zero.
client-output-buffer-limit normal 0 0 0
client-output-buffer-limit replica 256mb 64mb 60
client-output-buffer-limit pubsub 32mb 8mb 60

# Client query buffers accumulate new commands. They are limited to a fixed
# amount by default in order to avoid that a protocol desynchronization (for
# instance due to a bug in the client) will lead to unbound memory usage in
# the query buffer. However you can configure it here if you have very special
# needs, such us huge multi/exec requests or alike.
#
# client-query-buffer-limit 1gb

# In the Redis protocol, bulk requests, that are, elements representing single
# strings, are normally limited to 512 mb. However you can change this limit
# here, but must be 1mb or greater
#
# proto-max-bulk-len 512mb

# Redis calls an internal function to perform many background tasks, like
# closing connections of clients in timeout, purging expired keys that are
# never requested, and so forth.
#
# Not all tasks are performed with the same frequency, but Redis checks for
# tasks to perform according to the specified "hz" value.
#
# By default "hz" is set to 10. Raising the value will use more CPU when
# Redis is idle, but at the same time will make Redis more responsive when
# there are many keys expiring at the same time, and timeouts may be
# handled with more precision.
#
# The range is between 1 and 500, however a value over 100 is usually not
# a good idea. Most users should use the default of 10 and raise this up to
# 100 only in environments where very low latency is required.
hz 10

# Normally it is useful to have an HZ value which is proportional to the
# number of clients connected. This is useful in order, for instance, to
# avoid too many clients are processed for each background task invocation
# in order to avoid latency spikes.
#
# Since the default HZ value by default is conservatively set to 10, Redis
# offers, and enables by default, the ability to use an adaptive HZ value
# which will temporarily raise when there are many connected clients.
#
# When dynamic HZ is enabled, the actual configured HZ will be used
# as a baseline, but multiples of the configured HZ value will be actually
# used as needed once more clients are connected. In this way an idle
# instance will use very little CPU time while a busy instance will be
# more responsive.
dynamic-hz yes

# When a child rewrites the AOF file, if the following option is enabled
# the file will be fsync-ed every 32 MB of data generated. This is useful
# in order to commit the file to the disk more incrementally and avoid
# big latency spikes.
aof-rewrite-incremental-fsync yes

# When redis saves RDB file, if the following option is enabled
# the file will be fsync-ed every 32 MB of data generated. This is useful
# in order to commit the file to the disk more incrementally and avoid
# big latency spikes.
rdb-save-incremental-fsync yes

# Redis LFU eviction (see maxmemory setting) can be tuned. However it is a good
# idea to start with the default settings and only change them after investigating
# how to improve the performances and how the keys LFU change over time, which
# is possible to inspect via the OBJECT FREQ command.
#
# There are two tunable parameters in the Redis LFU implementation: the
# counter logarithm factor and the counter decay time. It is important to
# understand what the two parameters mean before changing them.
#
# The LFU counter is just 8 bits per key, it's maximum value is 255, so Redis
# uses a probabilistic increment with logarithmic behavior. Given the value
# of the old counter, when a key is accessed, the counter is incremented in
# this way:
#
# 1. A random number R between 0 and 1 is extracted.
# 2. A probability P is calculated as 1/(old_value*lfu_log_factor+1).
# 3. The counter is incremented only if R < P.
#
# The default lfu-log-factor is 10. This is a table of how the frequency
# counter changes with a different number of accesses with different
# logarithmic factors:
#
# +--------+------------+------------+------------+------------+------------+
# | factor | 100 hits   | 1000 hits  | 100K hits  | 1M hits    | 10M hits   |
# +--------+------------+------------+------------+------------+------------+
# | 0      | 104        | 255        | 255        | 255        | 255        |
# +--------+------------+------------+------------+------------+------------+
# | 1      | 18         | 49         | 255        | 255        | 255        |
# +--------+------------+------------+------------+------------+------------+
# | 10     | 10         | 18         | 142        | 255        | 255        |
# +--------+------------+------------+------------+------------+------------+
# | 100    | 8          | 11         | 49         | 143        | 255        |
# +--------+------------+------------+------------+------------+------------+
#
# NOTE: The above table was obtained by running the following commands:
#
#   redis-benchmark -n 1000000 incr foo
#   redis-cli object freq foo
#
# NOTE 2: The counter initial value is 5 in order to give new objects a chance
# to accumulate hits.
#
# The counter decay time is the time, in minutes, that must elapse in order
# for the key counter to be divided by two (or decremented if it has a value
# less <= 10).
#
# The default value for the lfu-decay-time is 1. A special value of 0 means to
# decay the counter every time it happens to be scanned.
#
# lfu-log-factor 10
# lfu-decay-time 1

########################### ACTIVE DEFRAGMENTATION #######################
#
# What is active defragmentation?
# -------------------------------
#
# Active (online) defragmentation allows a Redis server to compact the
# spaces left between small allocations and deallocations of data in memory,
# thus allowing to reclaim back memory.
#
# Fragmentation is a natural process that happens with every allocator (but
# less so with Jemalloc, fortunately) and certain workloads. Normally a server
# restart is needed in order to lower the fragmentation, or at least to flush
# away all the data and create it again. However thanks to this feature
# implemented by Oran Agra for Redis 4.0 this process can happen at runtime
# in a "hot" way, while the server is running.
#
# Basically when the fragmentation is over a certain level (see the
# configuration options below) Redis will start to create new copies of the
# values in contiguous memory regions by exploiting certain specific Jemalloc
# features (in order to understand if an allocation is causing fragmentation
# and to allocate it in a better place), and at the same time, will release the
# old copies of the data. This process, repeated incrementally for all the keys
# will cause the fragmentation to drop back to normal values.
#
# Important things to understand:
#
# 1. This feature is disabled by default, and only works if you compiled Redis
#    to use the copy of Jemalloc we ship with the source code of Redis.
#    This is the default with Linux builds.
#
# 2. You never need to enable this feature if you don't have fragmentation
#    issues.
#
# 3. Once you experience fragmentation, you can enable this feature when
#    needed with the command "CONFIG SET activedefrag yes".
#
# The configuration parameters are able to fine tune the behavior of the
# defragmentation process. If you are not sure about what they mean it is
# a good idea to leave the defaults untouched.

# Enabled active defragmentation
# activedefrag no

# Minimum amount of fragmentation waste to start active defrag
# active-defrag-ignore-bytes 100mb

# Minimum percentage of fragmentation to start active defrag
# active-defrag-threshold-lower 10

# Maximum percentage of fragmentation at which we use maximum effort
# active-defrag-threshold-upper 100

# Minimal effort for defrag in CPU percentage, to be used when the lower
# threshold is reached
# active-defrag-cycle-min 1

# Maximal effort for defrag in CPU percentage, to be used when the upper
# threshold is reached
# active-defrag-cycle-max 25

# Maximum number of set/hash/zset/list fields that will be processed from
# the main dictionary scan
# active-defrag-max-scan-fields 1000

# Jemalloc background thread for purging will be enabled by default
jemalloc-bg-thread yes

# It is possible to pin different threads and processes of Redis to specific
# CPUs in your system, in order to maximize the performances of the server.
# This is useful both in order to pin different Redis threads in different
# CPUs, but also in order to make sure that multiple Redis instances running
# in the same host will be pinned to different CPUs.
#
# Normally you can do this using the "taskset" command, however it is also
# possible to this via Redis configuration directly, both in Linux and FreeBSD.
#
# You can pin the server/IO threads, bio threads, aof rewrite child process, and
# the bgsave child process. The syntax to specify the cpu list is the same as
# the taskset command:
#
# Set redis server/io threads to cpu affinity 0,2,4,6:
# server_cpulist 0-7:2
#
# Set bio threads to cpu affinity 1,3:
# bio_cpulist 1,3
#
# Set aof rewrite child process to cpu affinity 8,9,10,11:
# aof_rewrite_cpulist 8-11
#
# Set bgsave child process to cpu affinity 1,10,11
# bgsave_cpulist 1,10-11

# In some cases redis will emit warnings and even refuse to start if it detects
# that the system is in bad state, it is possible to suppress these warnings
# by setting the following config which takes a space delimited list of warnings
# to suppress
#
# ignore-warnings ARM64-COW-BUG

~~~



- 修改配置项

~~~shell
bind 127.0.0.1 #注释掉这部分，这是限制redis只能本地访问

protected-mode no #默认yes，开启保护模式，限制为本地访问

daemonize no#默认no，改为yes意为以守护进程方式启动，可后台运行，除非kill进程，改为yes会使配置文件方式启动redis失败

dir  ./ #输入本地redis数据库存放文件夹（可选）

appendonly yes #redis持久化（可选）

logfile "access.log"

# requirepass 123456(设置成你自己的密码)
~~~

- 创建容器

~~~shell
docker run -p 6379:6379 --name myredis \
-v /root/redis/redis.conf:/etc/redis/redis.conf \
-v /root/redis/data:/data:rw \
--privileged=true -d redis redis-server /etc/redis/redis.conf --appendonly yes
~~~

## 2 Redis入门

### 2.1 redis简介

> Redis是什么？

Redis(Remote Dictionary Server),即远程字典服务。

redis会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件，并实现了master-slave（主从）同步。

Redis 是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作 数据库、缓存和消息中间件 。 

它支持多种类型的数据结构，如 字符串（strings）， 散列（hashes）， 列表（lists）， 集合（sets）， 有序集合（sorted sets） 与范围查询， bitmaps， hyperloglogs 和 地理空间（geospatial） 索引半径查询。 

Redis 内置了 复制（replication），LUA脚本（Lua scripting）， LRU驱动事件（LRU eviction），事务（transactions） 和不同级别的 磁盘持久化（persistence）， 并通过 Redis哨兵（Sentinel）和自动 分区（Cluster）提供高可用性（high availability）。

> Redis能干嘛？

1.  内容存储、持久化，内存中的数据是断电即失，所以持久化很重要（rdb、aof）

2.  效率高，可用于高速缓存

3.  发布订阅系统（消息队列）

4.  地图信息分析

5.  计时器、计数器（浏览量）

6.  ......

> 特性

1.  多样的数据类型
2.  持久化
3.  集群
4.  事务
5.  …

### 2.2 测试性能

redis-benchmark 是一个压力测试工具！官方自带的性能测试工具！

> 命令

| 序号 |        选项        |                 描述                  |  默认值   |
| :--: | :----------------: | :-----------------------------------: | :-------: |
|  1   |       **-H**       |           指定服务器主机名            | 127.0.0.1 |
|  2   |       **-p**       |            指定服务器端口             |   6379    |
|  3   |       **-s**       |           指定服务器套接字            |           |
|  4   |       **-C**       |            指定相对连接数             |    50     |
|  5   |       **-n**       |              指定请求数               |   10000   |
|  6   |       **-d**       | 以字节的形式指定 SET/GET 值的数据大小 |     3     |
|  7   |       **-k**       |       1=保持活动状态 0=重新连接       |     1     |
|  8   |       **-r**       | SET/GET/INCR 使用随时键，SADD 使用值  |           |
|  9   |       **-P**       |      通过管道传输 <numreq> 请求       |     1     |
|  10  |       **-q**       |   强制退出redis。只显示query/sec值    |           |
|  11  |     --**csv**      |             以CSV格式输出             |           |
|  12  | **-l（L 的小写）** |        生成循环，永久执行测试         |           |
|  13  |       **-t**       |     仅以图片分隔的测试命令列表。      |           |
|  14  | **-I（i的大写）**  |  空闲模式。只打开N个空闲连接并等待。  |           |

#### 2.2.1 测试

~~~shell
# 测试:100个并发连接 100000请求
redis-benchmark -h localhost -p 6379 -c 100 -n 100000

====== SET ======
  100000 requests completed in 4.92 seconds #对10万个请求进行写入测试
  100 parallel clients #100个并发客户端
  3 bytes payload # 每次写入三个字节
  keep alive: 1 # 只有一台服务器 单机性能
  host configuration "save": 3600 1 300 100 60 10000
  host configuration "appendonly": no
  multi-thread: no

Latency by percentile distribution:
0.000% <= 0.591 milliseconds (cumulative count 1)
50.000% <= 2.279 milliseconds (cumulative count 51116)
100.000% <= 16.767 milliseconds (cumulative count 100000)

Cumulative distribution of latencies:
0.000% <= 0.103 milliseconds (cumulative count 0)
15.261% <= 2.103 milliseconds (cumulative count 15261)
100.000% <= 17.103 milliseconds (cumulative count 100000)

Summary:
  #每秒处理 20312.82个请求
  throughput summary: 20312.82 requests per second
  latency summary (msec):
          avg       min       p50       p95       p99       max
        2.635     0.584     2.279     4.127     9.079    16.767
~~~

### 2.3 基本知识

#### 2.3.1 Redis单线程理解

> Redis是单线程的

Redis速度很快。Redis基于内存操作，CPU不是Redis的性能瓶颈，而是机器的内存和网络带宽。 

**Redis为什么单线程还这么快？**

1：高性能的服务器不一定是多线程的

2：多线程（CPU会上下文切换）的效率不一定比单线程高

核心：redis是将所有的数据全部放在内存中的，所以使用单线程操作效率就是最高的，多线程（CPU会上下文切换，耗时的操作），对弈内存系统来说，如果没有上下文切换那么效率就是最高的。多次读写在一个CPU上，这个就是最佳的方案！ 

#### 2.3.2 基础语法

|        命令        |                释意                 |          备注           |
| :----------------: | :---------------------------------: | :---------------------: |
|    select index    |             切换数据库              |        默认16个         |
|       dbsize       |             数据库大小              | 查看数据库当前存在的key |
|   set key value    |             存储键值对              |                         |
|      get key       |        获得key所对应的value         |                         |
|       keys *       |            查看所有的key            |                         |
|      flushdb       |           清空当前数据库            |                         |
|      flushall      |           清空所有数据库            |                         |
|     EXISTS key     | 判断某个key是否存在 返回1则代表存在 |                         |
|      move key      |             移出某个值              |                         |
| EXPIRE key seconds |        设置某个key的过期时间        |                         |
|      ttl key       |          查看还有多久过期           |                         |
|      type key      |            查看key的类型            |                         |
|      shutdown      |              停止运行               |                         |



### 2.4 五大数据类型

#### 2.4.1 String 

> 适用场景：更适合字符串存储

|           命令            |                          作用                          |                             使用                             |          备注           |
| :-----------------------: | :----------------------------------------------------: | :----------------------------------------------------------: | :---------------------: |
|     APPEND key value      | 在key对应的值上附加值 若append的值不存在，则新建一个值 | ![image-20220412151816972](http://cdn.lsbysl.com//202204121518074.png) |                         |
|        STRLEN key         |                     查看key的长度                      | ![image-20220412151837134](http://cdn.lsbysl.com//202204121518228.png) |                         |
|         incr key          |                        key值加1                        | ![image-20220412152242255](http://cdn.lsbysl.com//202204121522390.png) |                         |
|         decr key          |                        key值减1                        | ![image-20220412152310076](http://cdn.lsbysl.com//202204121523179.png) |                         |
|     INCRBY key count      |                     key值增长count                     | ![image-20220412152547548](http://cdn.lsbysl.com//202204121525644.png) |                         |
|     DECRBY key count      |                     key值减少count                     | ![image-20220412152612718](http://cdn.lsbysl.com//202204121526808.png) |                         |
|  GETRANGE key start end   |         获得某个范围的字符串（类似于subString)         | ![image-20220412152741994](http://cdn.lsbysl.com//202204121527068.png) |                         |
|     GETRANGE key 0 -1     |                    查看全部的字符串                    | ![image-20220412152841868](http://cdn.lsbysl.com//202204121528962.png) |                         |
| SETRANGE key offset value |                替换指定位置开始的字符串                | ![image-20220412152954366](http://cdn.lsbysl.com//202204121529460.png) |                         |
|  setex key seconds value  |                   设置key的过期时间                    | ![image-20220412153339126](http://cdn.lsbysl.com//202204121533231.png) |                         |
|           setnx           |            不存在则设置 在分布式锁中常使用             | ![image-20220412153418007](http://cdn.lsbysl.com//202204121534085.png) | 存在的key再次设置会失败 |
|     mset k1 v1 k2 v2      |                       批量设置值                       | ![image-20220412150830420](http://cdn.lsbysl.com//202204121508499.png) |                         |
|        mget k1 k2         |                       批量获取值                       | ![image-20220412150844687](http://cdn.lsbysl.com//202204121508753.png) |                         |
|    msetnx k1 v1 k2 v2     |         原子操作 只要有一个存在 就不会创建成功         |                                                              |                         |
|     getset key value      |                      先get 再set                       | ![image-20220412150754506](http://cdn.lsbysl.com//202204121507580.png) |                         |

> 扩展

```shell
# 对象操作
set user:1 {name:lsbysl,age:24}	--设置一个user:1对象 值为json字符串来保存一个对象
# 另一种存储对象的方式：user:{id}:{filed}
mset user:1:name lsbysl user:1:age 2  4
```

> String类似的使用场景：value除了字符串，还可以是数字(可以应用于计数器)

#### 2.4.2 List

> 在redis里面，我们可以把List设计成栈、队列、阻塞队列！

> 所有的List命令都是以l开头的

|                   命令                   |                          作用                          |                             使用                             | 备注 |
| :--------------------------------------: | :----------------------------------------------------: | :----------------------------------------------------------: | :--: |
|             LPUSH list value             |        向list中添加一个值 添加到列表头部（左）         |       ![](http://cdn.lsbysl.com//202204121554163.png)        |      |
|             RPUSH list value             |        向list中添加一个值 添加到列表尾部（右）         | ![image-20220412155440948](http://cdn.lsbysl.com//202204121554034.png) |      |
|             LRANGE list 0 -1             |                   查看list中的所有值                   | ![image-20220412155450324](http://cdn.lsbysl.com//202204121554385.png) |      |
|          LRANGE list start end           |                  查看指定位置的元素值                  | ![image-20220412155523568](http://cdn.lsbysl.com//202204121555628.png) |      |
|                LPOP list                 |                  移出list最左边的元素                  | ![image-20220412155602013](http://cdn.lsbysl.com//202204121556092.png) |      |
|                RPOP list                 |                  移出list最右边的元素                  | ![image-20220412155617176](http://cdn.lsbysl.com//202204121556244.png) |      |
|            LINDEX list index             |                   获取指定位置的元素                   | ![image-20220412155734300](http://cdn.lsbysl.com//202204121557367.png) |      |
|                Llen list                 |                     返回list的长度                     | ![image-20220412155749987](http://cdn.lsbysl.com//202204121557051.png) |      |
|          Lrem list count value           |                  移出count个value元素                  | ![image-20220412160924531](http://cdn.lsbysl.com//202204121609613.png) |      |
|           Ltrim list start end           |             截取操作 只会保留范围内的数据              | ![image-20220412161832291](http://cdn.lsbysl.com//202204121618372.png) |      |
|          rpoplpush list1 list2           | 移出列表最后一个元素 并将该元 素添加到另一个列表的头部 | ![image-20220412163529289](http://cdn.lsbysl.com//202204121635368.png) |      |
|          lset list index value           | 指定位置更新值（注意：此位置必须存在值才可以进行设置） | ![image-20220412163610943](http://cdn.lsbysl.com//202204121636019.png) |      |
| LINSERT list after sourcevalue aimvalue  |                   在原位置前插入数据                   | ![image-20220412174002914](http://cdn.lsbysl.com//202204121740030.png) |      |
| LINSERT list before sourcevalue aimvalue |                   在原位置后插入数据                   | ![image-20220412174035322](http://cdn.lsbysl.com//202204121740389.png) |      |

*   实际是一个链表 (left Node right)
*   如果key不存在，创建新的链表。如果key存在，新增内容
*   如果移除了所有值，空链表，也代表不存在  
*   在两边插入或改动值，效率最高。中间元素，效率较低
*   消息排队，消息队列（Lpush Rpop) 栈（Lpush Lpop)

#### 2.4.3 Set

> 无序 不重复 集合

> `所有的命令以s开头`

|              命令               |                  作用                  |                             使用                             |              备注              |
| :-----------------------------: | :------------------------------------: | :----------------------------------------------------------: | :----------------------------: |
|        sadd set mermber         |         set集合中添加一个元素          | ![image-20220412224846651](http://cdn.lsbysl.com//202204122248720.png) |                                |
|          smembers set           |        查看set集合中的所有元素         | ![image-20220412225009404](http://cdn.lsbysl.com//202204122250443.png) |                                |
|      sismember set member       |     查看set集合中是否存在某个元素      | ![image-20220412231320220](http://cdn.lsbysl.com//202204122313281.png) |                                |
|            scard set            |      查看set集合中所有元素的个数       | ![image-20220412231357103](http://cdn.lsbysl.com//202204122313142.png) |                                |
|         srem set mermer         |              移除某个元素              | ![image-20220412231843055](http://cdn.lsbysl.com//202204122318094.png) |                                |
|       srandmember	set        |           随机抽选出一个元素           | ![image-20220412232025754](http://cdn.lsbysl.com//202204122320829.png) |                                |
|      srandmember	set 2       |           随机抽选出两个元素           | ![image-20220412232044159](http://cdn.lsbysl.com//202204122320201.png) |                                |
|            spop set             |            随机删除一个元素            | ![image-20220412232101660](http://cdn.lsbysl.com//202204122321700.png) |                                |
| smove source destination member | 将某个集合的指定元素移动到另一个集合中 | ![image-20220412232901469](http://cdn.lsbysl.com//202204122329543.png) |                                |
|         sdiff set1 set2         |                  差集                  | ![image-20220412233013755](http://cdn.lsbysl.com//202204122330793.png) | 找出set1中有而set2中没有的元素 |
|        sinter set1 set2         |                  交集                  | ![image-20220412233045326](http://cdn.lsbysl.com//202204122330397.png) |         共同好友的实现         |
|        sunion set1 set2         |                  并集                  | ![image-20220412233137111](http://cdn.lsbysl.com//202204122331182.png) |                                |

#### 2.4.4 Hash(哈希)

> 存储结构：key->Map

> 适用场景：存储变更数据，尤其是用户信息之类的 更适合对象的存储

|              命令              |            作用            |                             使用                             |      备注      |
| :----------------------------: | :------------------------: | :----------------------------------------------------------: | :------------: |
|     hset hash field value      |       添加一个entry        | ![image-20220413000014843](http://cdn.lsbysl.com//202204130000884.png) | (field->value) |
|        hget hash field         |    获取指定hash的字段值    | ![image-20220413000033634](http://cdn.lsbysl.com//202204130000682.png) |                |
| hmset hash field1 v1 field2 v2 |         批量添加值         | ![image-20220413000453574](http://cdn.lsbysl.com//202204130004614.png) |                |
|    hmget hash field1 field2    |         批量获取值         | ![image-20220413000543052](http://cdn.lsbysl.com//202204130005091.png) |                |
|        hdel hash field         |    删除hash指定的entry     | ![image-20220413000903498](http://cdn.lsbysl.com//202204130009539.png) |                |
|           hlen hash            |   当前hash中entry的个数    | ![image-20220413000939922](http://cdn.lsbysl.com//202204130009961.png) |                |
|       hexists hash field       |  判断hash中指定字段是否存  | ![image-20220413001010350](http://cdn.lsbysl.com//202204130010391.png) |                |
|           hkeys hash           |  获取hash中所有entry的key  | ![image-20220413001034799](http://cdn.lsbysl.com//202204130010851.png) |                |
|           hvals hash           | 获取hash中所有entry的value | ![image-20220413001056096](http://cdn.lsbysl.com//202204130010139.png) |                |
|   hincrby	hash field	1   |      指定字段值自增1       | ![image-20220413001203550](http://cdn.lsbysl.com//202204130012595.png) |                |
|     hincrby hash field -1      |      指定字段值自减1       | ![image-20220413001825508](http://cdn.lsbysl.com//202204130018549.png) |                |
|    hsetnx hash field value     |     field不存在时创建      | ![image-20220413001400360](http://cdn.lsbysl.com//202204130014404.png) |                |
|    hset user:1 name lsbysl     |        存储一个对象        | ![image-20220413002020928](http://cdn.lsbysl.com//202204130020970.png) |                |
|            hgetall             |     获取hash中所有k v      | ![image-20220413000801420](http://cdn.lsbysl.com//202204130008505.png) |                |

#### 2.4.5 Zset

> 在set的基础上增加一个score字段 用于排序

|                  命令                  |              作用               |                             使用                             |     备注      |
| :------------------------------------: | :-----------------------------: | :----------------------------------------------------------: | :-----------: |
|         zadd set score member          |           增加一个值            | ![image-20220413003630769](http://cdn.lsbysl.com//202204130036816.png) |               |
| zadd set score1 member1 score2 member2 |           增加多个值            | ![image-20220413003741842](http://cdn.lsbysl.com//202204130037881.png) |               |
|            zrange set 0 -1             |        查看set中的所有值        | ![image-20220413004243700](http://cdn.lsbysl.com//202204130042736.png) |               |
|     ZRANGEBYSCORE set set min max      |     按照score进行排序 升序      | ![image-20220413004201869](http://cdn.lsbysl.com//202204130042905.png) |               |
| ZRANGEBYSCORE set -inf +inf withscores | 按照score进行排序 并带出score值 | ![image-20220413005004452](http://cdn.lsbysl.com//202204130050489.png) |               |
|           ZREVRANGE set 0 -1           |            降序排序             | ![image-20220413003906616](http://cdn.lsbysl.com//202204130039652.png) |               |
|            ZREM set member             |          移出某个元素           | ![image-20220413005226742](http://cdn.lsbysl.com//202204130052783.png) |               |
|               ZCARD set                |        集合中的元素个数         | ![image-20220413005248702](http://cdn.lsbysl.com//202204130052744.png) |               |
|          zcount set start end          |     获取指定区间的元素个数      | ![image-20220413005357383](http://cdn.lsbysl.com//202204130053422.png) | 按照score筛选 |

### 2.5 三种特殊数据类型

#### 2.5.1 geospatial（地理位置）

> 适用场景：定位、附近的人、打车距离、方圆几里的人

> 命令

|       命令        |                    作用                    |                             使用                             |                             备注                             |
| :---------------: | :----------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|      GEOADD       |            添加一个地理空间位置            | ![image-20220413212632471](http://cdn.lsbysl.com//202204132126506.png) |                     两级是无法直接添加的                     |
|      GEOPOS       |          获取指定位置的经度和纬度          | ![image-20220413212746450](http://cdn.lsbysl.com//202204132127492.png) |                                                              |
|      GEODIST      |           返回两个位置之间的距离           | ![image-20220413213840874](http://cdn.lsbysl.com//202204132138911.png) | `m` 表示单位为米。 `km` 表示单位为千米。 `mi` 表示单位为英里。 `ft` 表示单位为英尺。 |
|     GEORADIUS     | 以给定的经纬度为中心，找出某一半径内的元素 | ![image-20220413214158912](http://cdn.lsbysl.com//202204132141959.png) |                                                              |
| GEORADIUSBYMEMBER |       找出位于指定元素周围的其他元素       | ![image-20220413215749063](http://cdn.lsbysl.com//202204132157105.png) |                                                              |
|      GEOHASH      |    返回一个或多个位置元素的geohash表示     | ![image-20220413220149639](http://cdn.lsbysl.com//202204132201686.png) |                   11个字符的Geohash字符串                    |
|                   |                                            |                                                              |                                                              |

#### 2.5.2 hyperloglog（基数统计的算法）

> 基数：不重复的元素

> 优点：占用内存小

> 0.81%的错误率

> 适用场景：网页的浏览量（一个人浏览网页多次，还是算作一个人）

> 传统方式，set来保存用户id，然后就可以统计set中的元素数量作为标准判断

|  命令   |     作用      |                             使用                             | 备注 |
| :-----: | :-----------: | :----------------------------------------------------------: | :--: |
|  PFadd  |   添加元素    | ![image-20220413222524045](http://cdn.lsbysl.com//202204132225085.png) |      |
| PFcount | 统计key的元素 | ![image-20220413222548915](http://cdn.lsbysl.com//202204132225949.png) |      |
| PFmerge |    合并key    | ![image-20220413222652601](http://cdn.lsbysl.com//202204132226634.png) |      |

#### 2.3.3 bitmap

> 操作二进制位来进行记录的 ，就只有0和1两个状态

> 适用场景：统计用户信息（如活跃、不活跃；365天打卡；两个状态的都适用）

|   命令   |       作用       |                             使用                             | 备注 |
| :------: | :--------------: | :----------------------------------------------------------: | :--: |
|  setbit  |      设置值      | ![image-20220413223507688](http://cdn.lsbysl.com//202204132235792.png) |      |
|  Getbit  | 获取指定位置的值 | ![image-20220413223618973](http://cdn.lsbysl.com//202204132236090.png) |      |
| Bitcount | 获取状态为1的个  | ![image-20220413223827099](http://cdn.lsbysl.com//202204132238143.png) |      |

## 3 事务

### 3.1 基础语法

> Redis单条命令是保证原子性的，但是事务不保证原子性！

> Redis事务本质：一组命令的集合！一个事务中的所有命令都会被序列化，在事务执行的过程中，会按照顺序执行。

> 特性：一次性、顺序性、排他性（事务运行中不允许被打断）

```
------队列 set set set 执行 -----
```

> Redis事务没有隔离级别的概念

> 所有的命令在事务中，并没有直接执行！只有发起执行命令的时候才会执行！Exec

> redis的事务命令：

|  命令   |   作用   |                             使用                             |            备注            |
| :-----: | :------: | :----------------------------------------------------------: | :------------------------: |
|  multi  | 开启事务 | ![image-20220414002250864](http://cdn.lsbysl.com//202204140022905.png) |           步骤一           |
|         | 命令入队 | ![image-20220414002312779](http://cdn.lsbysl.com//202204140023819.png) |           步骤二           |
|  exec   | 执行事务 | ![image-20220414002302590](http://cdn.lsbysl.com//202204140023625.png) |           步骤三           |
| discard | 放弃事务 | ![image-20220414002458568](http://cdn.lsbysl.com//202204140024605.png) | 事务队列中命令都不会被执行 |

### 3.2 异常情况

|  异常说明  |                           触发场景                           |                      备注                      |
| :--------: | :----------------------------------------------------------: | :--------------------------------------------: |
| 编译型异常 | ![image-20220414002911141](http://cdn.lsbysl.com//202204140029175.png) |           事务中的所有命令都不会执行           |
| 运行时异常 | ![image-20220414003059658](http://cdn.lsbysl.com//202204140030760.png) | 事务中的其他命令可以正常执行，错误命令抛出异常 |

### 3.3 监控 （Watch）

> 可用于做乐观锁

> 悲观锁：很悲观，做什么都会加锁（Synchronize关键字就是这样）

> 乐观锁：很乐观，认为什么时候都不会出问题，所以不会上锁。更新的时候去判断一下，在此期间是否有人操作过这个数据。

```shell
#监视一个key 如果数据在此期间没有发生变动，就会正常执行成功
# 事务执行之后 监控也会自动取消 但如果事务执行失败 就需要使用unwatch先解锁
watch key		 # 加锁
unwatch # 解锁
```

 ![image-20220414004102131](http://cdn.lsbysl.com//202204140041165.png)

## 4 Redis配置文件

> 单位 配置文件unit单位对大小写不敏感

![image-20220414205833048](http://cdn.lsbysl.com//202204142058153.png)

> 包含 多个配置文件

![image-20220414210129901](http://cdn.lsbysl.com//202204142101935.png)

> 网络

- 绑定的ip

![image-20220414210251919](http://cdn.lsbysl.com//202204142102962.png)

- 保护模式

![image-20220414210329279](http://cdn.lsbysl.com//202204142103315.png)

- 端口

![image-20220414210350205](http://cdn.lsbysl.com//202204142103288.png)

> 通用配置 GENERAL

- 守护进程运行程序

![](http://cdn.lsbysl.com//202204142105395.png)

- pid文件 (如果守护进程方式运行需要有一个pid文件)

![image-20220414210647441](http://cdn.lsbysl.com//202204142106526.png)

- 日志级别

![image-20220414210732312](http://cdn.lsbysl.com//202204142107398.png)

- 日志文件

![image-20220414211652283](http://cdn.lsbysl.com//202204142116320.png)

- 默认数据库数量

![image-20220414211812474](http://cdn.lsbysl.com//202204142118511.png)

- 进入redis的时候是否显示logo

![image-20220414211940738](http://cdn.lsbysl.com//202204142119775.png)

> 快照配置  SNAPSHOTTING

- 持久化操作时间

~~~tex
持久化，再贵的时间内执行了多少次操作，则会持久化到文件.rbd .aof中
redis是内存数据库，如果没有持久化，那木数据断电及失！
~~~

![image-20220414212400121](http://cdn.lsbysl.com//202204142124162.png)

如果3600内至少有一个key修改了我们进行持久化操作

- 持久化错误是否继续工作

![image-20220414212819398](http://cdn.lsbysl.com//202204142128489.png)

- 是否压缩rdb文件

![image-20220414212903942](http://cdn.lsbysl.com//202204142129980.png)

- 保存rdb文件的时候，是否校验rdb文件

![](http://cdn.lsbysl.com//202204142129980.png)

- rdb文件保存的目录

![image-20220414213157768](http://cdn.lsbysl.com//202204142131809.png)

> 主从复制相关 REPLICATION



> 安全 SECURITY

- 密码设置 默认没有密码

![image-20220414213429834](http://cdn.lsbysl.com//202204142134871.png)

- 设置密码且登陆

~~~shell
# 设置密码
config set requirepass "passwd"
# 登陆
auth passwd
~~~

> 限制 CLIENTS

- 最大客户端数量

![image-20220414214007687](http://cdn.lsbysl.com//202204142140778.png)

- redis配置最大的内存

![image-20220414214106681](http://cdn.lsbysl.com//202204142141740.png)

- 内存上限后的处理策略 移除一些过期的key

![image-20220414214201889](http://cdn.lsbysl.com//202204142142983.png)

> APPEND ONLY 模式 aof配置

![image-20220414214618249](http://cdn.lsbysl.com//202204142146288.png)

默认不开启，因为使用的rdb持久化

- aof文件名

![image-20220414214754561](http://cdn.lsbysl.com//202204142147648.png)

- 同步规则

![image-20220414214915594](http://cdn.lsbysl.com//202204142149684.png)



## 5 持久化

redis是内存数据库，如果不将内存中的数据库状态保存到磁盘，那么一旦服务器进程退出，服务器中的数据库状态也会消失，所以redis提供了持久化的功能

`如果只做缓存，是不用持久化的`

### 5.1 RDB（Redis DataBase）

指定的时间间隔内将内存中的数据集快照写入磁盘，也就是行话Snapshot。它恢复时是将快照文件直接读到内存中。

Redis会单独创建一个fork子进程来进行持久化，先将数据写入一个临时文件，持久化结束后，用这个临时文件替代上次持久化好的文件。整个过程中，主进程是不进行任何IO操作的，这就确保了极高的性能。

如果需要进行大规模的数据恢复，且对于数据恢复的完整性不是特别敏感，那么RDB方式比AOF更加高效能。

RDB的缺点就是最后一次持久化的数据可能会丢失。

在主从复制中，rdb就是备用的，放在从机上面

![在这里插入图片描述](http://cdn.lsbysl.com//202204142256867.png)

有时候在生产环境 我们会将dump.rdb进行备份

在rdb保存的文件时dump.rdb 都是在我们的配置文件中的快照模块进行配置的

![在这里插入图片描述](https://www.icode9.com/i/ll/?i=20210114173932175.png#pic_center)



#### 5.1.1 rdb触发机制

1：save（save 900 1)的规则满足的情况下

2：执行flushall命令

3：退出redis

会生成一个dump.rdb文件

> 如何恢复rdb文件

1：只需要将rdb文件放在我们的redis启动目录即可，redis启动的时候就会自动检查dump.rdb，并恢复其中的数据

2：查看rdb存放的位置

![在这里插入图片描述](https://www.icode9.com/i/ll/?i=20210114174000351.png#pic_center)

#### 5.1.2 rdb优缺点

`优点`

1.  使用大场景的数据恢复
2.  对数据的完整性不高

`缺点`

1：需要一定的时间间隔进行操作，如果redis意外宕机，那么最后一次修改数据就没有了

2：fork进程的时候，会占用一定的内存空间

### 5.2 AOF(Append Only File)

将我们所有的命令都记录下来，恢复的时候 就把这个文件全部再执行一遍

#### 5.2.1 AOF是什么

![image-20220414231849316](http://cdn.lsbysl.com//202204142318354.png)



以日志的形式来记录每个写操作，将Redis执行过的所有命令记录下来（读操作不记录），只许追加文件但不可以改写文件，redis启动之初就会读取该文件重新构造数据。换言之，redis重启的话就根据日志文件的内容将写指令从前到后执行有一次完成数据的恢复。

AOF保存的是appendonly.aof文件

> 配置文件 append

默认是不开启的 需要手动开启（appendonly yes)，重启，即可生效

#### 5.2.2 修复aof文件

如果aof文件有错位的话，这时候redis是启动不起来的。

redis提供了一个工具 redis-check-aof --fix (使用这个命令即可修复aof文件)

如果文件正常，重启即可恢复

#### 5.2.3 优缺点

` 优点:`

1.每一次修改都同步，文件的完整性会更好

2.每秒同步一次，可能会丢失一秒的数据

3.从不同步效率最高

`缺点：`

1.相对于数据文件来说，aof远远大于rdb，修复的速度也比rdb慢

2.aof运行效率也要比rdb慢。所以redis默认配置爱就是rdb持久化

#### 5.2.4 重写规则说明

aof默认就是文件的无线追加，文件会越来越大

![在这里插入图片描述](http://cdn.lsbysl.com//202204142316404.png)

如果文件大于64m，就会fork一个新的进程来将我们的文件进行重写

#### 5.2.5 扩展

- RDB 持久化方式能够在指定的时间间隔内对你的数据进行快照存储
- AOF持久化方式记录每次对服务器写的操作，当服务器重启的时候会重新执行这些命令来恢复原始的数据，AOF命令以Redis 协
  议追加保存每次写的操作到文件末尾，Redis还能对AOF文件进行后台重写，使得AOF文件的体积不至于过大
- 只做缓存，如果你只希望你的数据在服务器运行的时候存在，你也可以不使用任何持久化
- 同时开启两种持久化方式
  - 在这种情况下 ，当redis重启的时候会优先载入AOF文件来恢复原始的数据，因为在通常情况下AOF文件保存的数据集要比RDB
    文件保存的数据集要完整。
  - RDB 的数据不实时，同时使用两者时服务器重启也只会找AOF文件，那要不要只使用AOF呢？作者建议不要，因为RDB更适合
    用于备份数据库（AOF在不断变化不好备份），快速重启，而且不会有AOF可能潜在的Bug，留着作为一个万一的手段。
- 性能建议
  - 因为RDB文件只用作后备用途，建议只在Slave上持久化RDB文件 ，而且只要15分钟备份一次就够了 ，只保留 save 900 1 这条
    规则。
  - 如果Enable AOF，好处是在最恶劣情况下也只会丢失不超过两秒数据，启动脚本较简单只load自己的AOF文件就可以了，代价
    一是带来了持续的IO，二是AOF rewrite 的最后将 rewrite 过程中产生的新数据写到新文件造成的阻塞几乎是不可避免的。只要
    硬盘许可，应该尽量减少AOF rewrite 的频率，AOF重写的基础大小默认值64M太小了，可以设到5G以上，默认超过原大小
    100%大小重写可以改到适当的数值。
  - 如果不Enable AOF ，仅靠 Master-Slave Repllcation 实现高可用性也可以，能省掉一大笔IO，也减少了rewrite时带来的系统
    波动。代价是如果Master/Slave 同时倒掉，会丢失十几分钟的数据，启动脚本也要比较两个 Master/Slave 中的 RDB文件，载
    入较新的那个，微博就是这种架构。

### 6 Redis发布订阅

Redis发布订阅（pub/sub)是一种消息通信模式：发布者发送消息，订阅者接收消息。（如微博、微信、关注系统等）

Redis客户端可以订阅任意数量的频道

订阅/发布消息图

消息发送者、消息订阅者、频道

![在这里插入图片描述](http://cdn.lsbysl.com//202204150015837.png)

|              命令              |               作用               | 备注 |
| :----------------------------: | :------------------------------: | :--: |
|  SUBSCRIBE channel1 channel2   |             订阅频道             |      |
|    PUBLISH channel message     |             发布信息             |      |
| UNSUBSCRIBE channel1 channel2  |             退订频道             |      |
|  PSUBSCRIBE pattern1 pattern2  | 订阅一个或多个符合给定模式的频道 |      |
|       PUBSUB suncommand        |     查看订阅与发布系统的状态     |      |
| PUNSUBSCRIBE pattern1 pattern2 |        退订所给模式的频道        |      |

这些命令被广泛用于构建即时通信应用，比如网络聊天室、实时广播和实时通信等

`一定要订阅之后才会接收，退订之后是无法接收的`

#### 6.1 原理

Redis是使用C实现的，通过分析Redis源码里面的pubsub.c文件，即可了解发布和订阅机制的底层实现。

Redis是通过PUBLISH、SUBSCRIBE、PSUBCRIBE等命令实现发布和订阅功能

通过SUBSCRTIBE命令订阅某个频道后，redis-server里维护了一个字典，字典的键就是频道，而字典的值就是一个链表，这个链表保存了所有订阅这个频道的客户端。SUBCRIBE命令的关键，就是将客户端添加到给定频道的订阅链表中。

用过PUBLISH命令向订阅者发布消息，redis-server会使用给定的频道作为键，在它所维护的频道子弟班中查找订阅了这个频道的所有客户端链表，遍历这个链表，将消息发布给所有订阅者。

> 使用场景：

*   实时消息系统
*   实时聊天（频道当做聊天室，将信息回显给所有人即可）
*   订阅、关注系统

> 稍微复杂的场景，就会使用消息中间件MQ



### 7 Redis主从复制

#### 7.1 概念理解

`主从复制，读写分离！80%的情况都是在进行读操作。减缓服务器的压力，架构中经常使用，最少都需要一主二从！`

> 概念

主从复制，就是指一台redis服务器上的数据复制到其他的redis服务器上。前者称为主节点（master/leader)，后者称为从节点（slave/follower)。`数据的复制是单向的`，只能由主节点到从节点。主节点写，从节点读。

默认情况下，每台redis服务器都是主节点，且一个主节点可以有多个从节点或没有从节点，但一个从节点只能有一个主节点。

主从复制的作用：

*   数据冗余：主从复制实现了数据的热备份，是持久化之外的一种数据冗余方式
*   故障恢复：主节点出现问题后，可以由从节点提供服务，实现快速的故障恢复。实际上是一种服务冗余
*   负载均衡：主从复制中，配合读写分离，可以由主节点提供写服务，从节点提供读服务，分担服务器负载，可以大大提高redis服务器的并发量
*   高可用（集群）基石：主从复制还是哨兵和集群能够实施的基础

一般来说，将Redis运用于工程项目，只使用一台redis是万万不能的，原因如下：

*   从结构上，单个Redis服务器会发生单点故障，并且一台服务器需要处理所有的请求负载，压力较大
*   从容量上，单个redis服务器容量有限，一般来说，单台redis的最大使用内存不应该超过20G



<img src="http://cdn.lsbysl.com//202204152247879.png" alt="image-20220415224719831" style="zoom:50%;" />





#### 7.2 环境配置-命令配置主从

- 查看当前库的主从复制信息

~~~shell
127.0.0.1:6379> info replication
# Replication
role:master # 角色master
connected_slaves:0 # 从机个数
master_failover_state:no-failover
master_replid:e6a51ad11c1e88ccd456d08103c08a2070a1e9e9
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
~~~

- Docker创建两个从机

~~~shell
# docker 创建网络让容器在同一个网络内
docker network create redis --subnet 172.38.0.0/16

# 复制配置文件
[root@lsbysl redis]# ll
总用量 280
-rw-r--r-- 1 root    root 93737 4月  15 11:05 redis-6380.conf # 从
-rw-r--r-- 1 root    root 93737 4月  15 11:05 redis-6381.conf # 从
-rw-r--r-- 1 root    root 93737 4月  15 10:35 redis.conf # 主
# 主机
docker run -p 6379:6379 --name myredis \
-v /root/redis/redis.conf:/etc/redis/redis.conf \
-v /root/redis/data:/data \
--privileged=true --net redis --ip 172.38.0.79 -d redis redis-server /etc/redis/redis.conf --appendonly yes
# 从机1
docker run -p 6380:6379 --name myredis-6380 \
-v /root/redis/redis-6380.conf:/etc/redis/redis.conf \
-v /root/redis/data_6380:/data \
--privileged=true --net redis --ip 172.38.0.80 -d redis redis-server /etc/redis/redis.conf --appendonly yes
# 从机2
docker run -p 6381:6379 --name myredis-6381 \
-v /root/redis/redis-6381.conf:/etc/redis/redis.conf \
-v /root/redis/data_6381:/data \
--privileged=true --net redis --ip 172.38.0.81 -d redis redis-server /etc/redis/redis.conf --appendonly yes
# 主机目录现在的样子
[root@lsbysl redis]# ll
总用量 288
drwxr-xr-x 2 polkitd root  4096 4月  15 10:40 data
drwxr-xr-x 2 polkitd root  4096 4月  15 11:13 data_6380
drwxr-xr-x 2 polkitd root  4096 4月  15 11:14 data_6381
-rw-r--r-- 1 root    root 93737 4月  15 11:05 redis-6380.conf
-rw-r--r-- 1 root    root 93737 4月  15 11:05 redis-6381.conf
-rw-r--r-- 1 root    root 93737 4月  15 10:35 redis.conf
# docker ps的样子
[root@lsbysl redis]# docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED              STATUS              PORTS                               NAMES
fab2d429c095   redis                 "docker-entrypoint.s…"   58 seconds ago       Up 57 seconds       0.0.0.0:6381->6379/tcp              myredis-6381
415d2c0f79de   redis                 "docker-entrypoint.s…"   About a minute ago   Up About a minute   0.0.0.0:6380->6379/tcp              myredis-6380
fecbb580f253   redis                 "docker-entrypoint.s…"   39 minutes ago       Up 39 minutes       0.0.0.0:6379->6379/tcp              myredis
~~~

- 默认每台redis都是master

![image-20220415232122363](http://cdn.lsbysl.com//202204152321400.png)

- 正常情况下我们配置从机就好了

  > 命令:SLAVEOF host port (从机上使用)
  >
  >  主:6379 从:6380 6381 

![image-20220416000406823](http://cdn.lsbysl.com//202204160004929.png)



#### 7.3 环境配置-配置文件配置主从

- 配置文件中配置好 启动即可

![image-20220415234244910](http://cdn.lsbysl.com//202204152342943.png)

#### 7.4 细节

> 主机可写，从机不可写只能读
>
> 主机中的所有数据都会被从机保存
>
> master断开再重新连接一样可以同步数据到从机
>
> 从机如果不从不是配置到配置文件中服务断开及脱离主节点
>
> 连接主机数据会直接同步

- 都是没数据的

![image-20220415234808777](http://cdn.lsbysl.com//202204152348846.png)

- 主机设置数据

![image-20220416011159095](http://cdn.lsbysl.com//202204160111143.png)

没有配置哨兵的话，即使主机宕机，从机所属的主机依然不会改变。这个时候如果主机恢复了，从机依然可以获取主机写入的信息

如果是使用命令行配置的主从，这个时候如果重启了，就会变回主机。只要变回从机，就会立马从主机中获取值

> 复制原理

Slave启动成功连接到master后会发送一个sync同步命令

Master接收到命令后，启动后台的存盘进程，同时手机所有接收到的用于修改数据集命令，后台进程执行完毕之后，master将传送整个数据文件到slave，并完成一次完全同步

全量复制：slave获取到master的所有数据

增量复制：master新增的修改命令传给slave

只要是重新连接master，一次完全同步（全量复制）将被自动执行。

![在这里插入图片描述](http://cdn.lsbysl.com//202204160132349.png)

### 7 哨兵模式

> 没有哨兵模式的时候

当主机宕机后，这时候需要从从机中选出一个来当主节点。即使主机回来后，也无法当之前的主节点了。

```shell
SLAVEOF no one		#从节点变为主节点
```

![在这里插入图片描述](http://cdn.lsbysl.com//202204160319126.png)

> 哨兵模式（自动选举老大的模式）

`概述`：

主从切换技术的方法是：当主服务器宕机后，需要手动把一台服务器切换为主服务器，这就需要人工干预，费时费力，还会造成一段时间内服务不可用。这不是一种推荐的方式，更多时候，我们优先考虑哨兵模式。Redis从2.8开始正式提供Sentinel（哨兵）架构来解决这个问题。

能够监控主机是否故障，如果故障了根据投票数自动将从库转化为主库

哨兵模式是一种特殊的模式，首先redis提供了哨兵的命令，哨兵是一个独立的进程，作为进程，它会独立运行。原理是：哨兵通过发送命令，等待redis服务器响应，从而监控多个redis示例。

![在这里插入图片描述](http://cdn.lsbysl.com//202204160319688.png)

（单机哨兵）

一个哨兵进行监控，可能会出现问题，为此，我们可以使用多个哨兵进行监控。各个哨兵之间还会进行监控，这样就形成了哨兵模式

![在这里插入图片描述](http://cdn.lsbysl.com//202204160320091.png)

（多哨兵模式）

假设主服务器宕机，哨兵1先检测到这个结果，系统并不会马上进行failover(重新选举)，仅仅是哨兵1主观认为主服务器不可用，称为`主观下线`。当后面的哨兵也检测到主服务器不可用，并且打到一定数量是，那么哨兵就会进行一次投票，投票的结果由一个哨兵发起，进行failover【故障转移】操作。切换成功后，通过发布订阅模式，让各个哨兵把自己监控的从服务器切换成主服务器，这个过程称为`客观下线`

> 开启哨兵模式

1.  配置sentinel.conf文件

    *   创建一个哨兵配置文件（sentinel.conf）

        ![在这里插入图片描述](http://cdn.lsbysl.com//202204160320440.png)

    *   配置文件

        ```
        sentinel monitor 被监控的主机名称 host port 1  #1代表主机挂了，要进行重新选举
        ```

        ![在这里插入图片描述](https://www.icode9.com/i/ll/?i=20210114175157626.png#pic_center)

2.  启动哨兵

    ```
    redis-sentinel kconfig/sentinel.conf		#启动哨兵模式
    ```

    ![在这里插入图片描述](http://cdn.lsbysl.com//202204160321144.png)

    主机宕机后，会重新选择主机。这时候，即使主机回来，也只能当新主机的从机

> 优缺点

优点：

*   主从可以切换、故障可以转移，可用性高
*   哨兵模式是主从模式的升级，手动到自动，更加健壮

缺点：

*   redis不好在线扩容，集群数量一旦打到上限，扩容就十分麻烦

*   实现哨兵模式的配置其实很麻烦，里面有很多选择

> 哨兵模式的全部配置（百度查看）

~~~shell
# Example sentinel.conf
 
# 哨兵sentinel实例运行的端口 默认26379
port 26379
 
# 哨兵sentinel的工作目录
dir /tmp
 
# 哨兵sentinel监控的redis主节点的 ip port 
# master-name  可以自己命名的主节点名字 只能由字母A-z、数字0-9 、这三个字符".-_"组成。
# quorum 当这些quorum个数sentinel哨兵认为master主节点失联 那么这时 客观上认为主节点失联了
# sentinel monitor <master-name> <ip> <redis-port> <quorum>
sentinel monitor mymaster 127.0.0.1 6379 1
 
# 当在Redis实例中开启了requirepass foobared 授权密码 这样所有连接Redis实例的客户端都要提供密码
# 设置哨兵sentinel 连接主从的密码 注意必须为主从设置一样的验证密码
# sentinel auth-pass <master-name> <password>
sentinel auth-pass mymaster MySUPER--secret-0123passw0rd
 
 
# 指定多少毫秒之后 主节点没有应答哨兵sentinel 此时 哨兵主观上认为主节点下线 默认30秒
# sentinel down-after-milliseconds <master-name> <milliseconds>
sentinel down-after-milliseconds mymaster 30000
 
# 这个配置项指定了在发生failover主备切换时最多可以有多少个slave同时对新的master进行 同步，
这个数字越小，完成failover所需的时间就越长，
但是如果这个数字越大，就意味着越 多的slave因为replication而不可用。
可以通过将这个值设为 1 来保证每次只有一个slave 处于不能处理命令请求的状态。
# sentinel parallel-syncs <master-name> <numslaves>
sentinel parallel-syncs mymaster 1
 
 
 
# 故障转移的超时时间 failover-timeout 可以用在以下这些方面： 
#1. 同一个sentinel对同一个master两次failover之间的间隔时间。
#2. 当一个slave从一个错误的master那里同步数据开始计算时间。直到slave被纠正为向正确的master那里同步数据时。
#3.当想要取消一个正在进行的failover所需要的时间。  
#4.当进行failover时，配置所有slaves指向新的master所需的最大时间。不过，即使过了这个超时，slaves依然会被正确配置为指向master，但是就不按parallel-syncs所配置的规则来了
# 默认三分钟
# sentinel failover-timeout <master-name> <milliseconds>
sentinel failover-timeout mymaster 180000
 
# SCRIPTS EXECUTION
 
#配置当某一事件发生时所需要执行的脚本，可以通过脚本来通知管理员，例如当系统运行不正常时发邮件通知相关人员。
#对于脚本的运行结果有以下规则：
#若脚本执行后返回1，那么该脚本稍后将会被再次执行，重复次数目前默认为10
#若脚本执行后返回2，或者比2更高的一个返回值，脚本将不会重复执行。
#如果脚本在执行过程中由于收到系统中断信号被终止了，则同返回值为1时的行为相同。
#一个脚本的最大执行时间为60s，如果超过这个时间，脚本将会被一个SIGKILL信号终止，之后重新执行。
 
#通知型脚本:当sentinel有任何警告级别的事件发生时（比如说redis实例的主观失效和客观失效等等），将会去调用这个脚本，
#这时这个脚本应该通过邮件，SMS等方式去通知系统管理员关于系统不正常运行的信息。调用该脚本时，将传给脚本两个参数，
#一个是事件的类型，
#一个是事件的描述。
#如果sentinel.conf配置文件中配置了这个脚本路径，那么必须保证这个脚本存在于这个路径，并且是可执行的，否则sentinel无法正常启动成功。
#通知脚本
# sentinel notification-script <master-name> <script-path>
  sentinel notification-script mymaster /var/redis/notify.sh
 
# 客户端重新配置主节点参数脚本
# 当一个master由于failover而发生改变时，这个脚本将会被调用，通知相关的客户端关于master地址已经发生改变的信息。
# 以下参数将会在调用脚本时传给脚本:
# <master-name> <role> <state> <from-ip> <from-port> <to-ip> <to-port>
# 目前<state>总是“failover”,
# <role>是“leader”或者“observer”中的一个。 
# 参数 from-ip, from-port, to-ip, to-port是用来和旧的master和新的master(即旧的slave)通信的
# 这个脚本应该是通用的，能被多次调用，不是针对性的。
# sentinel client-reconfig-script <master-name> <script-path>
sentinel client-reconfig-script mymaster /var/redis/reconfig.sh
~~~



### 8 Redis缓存穿透和雪崩

#### 8.1 缓存穿透（查不到导致的）

> 概念

用户查询一个数据，发现redis数据库没有，也就是缓存没有命中，于是向持久层数据库查询，发现也没有，于是此次查询失败。当用户很多的时候，缓存都没有命中，于是都去请求数据库，这会给持久层数据库造成很大压力，也就相当于出现了缓存穿透。

> 解决方案

> `布隆过滤器`

布隆过滤器是一种数据结构，对所有可能查询的参数以hash形式存储，在控制层先校验，不符合则丢弃，从而避免了对底层存储系统的查询压力。

![在这里插入图片描述](http://cdn.lsbysl.com//202204162120606.png)

`缓存空对象`

当存储层不命中后，即便返回的空对象也将其缓存起来，同时设置一个过期时间，之后再访问这个数据将会从缓存中获取，保护了后端数据源

![在这里插入图片描述](http://cdn.lsbysl.com//202204162120435.png)

> 这种方法会存在两个问题：

1.  存储空值会消耗大量的空间
2.  即使对控制设置了过期时间，还是会存在缓存层和存储层的数据会有一段时间的窗口不一致，这对需要保存一直性的业务会有影响



#### 8.2 缓存击穿（量太大、缓存过期导致的）

> 概念

缓存击穿是值一个key非常热点（如微博热搜），在不停的扛着大并发，大并发集中对这一个点进行访问，当这个key在失效的瞬间，持续的大并发就穿透缓存，直接请求数据库。这就像在一个品章上凿开了一个洞。

> 解决方案

`设置热点数据永不过期`

从缓存层面来说，没有设置过期时间，就不会出现热点key过期后产生的问题

`加互斥锁`

分布式锁：使用分布式锁，保证每个key同时只有一个线程去查询后端服务（后端数据库），其他key没有获得分布式锁的权限，因此只需等待即可。这种方式将高并发的压力转移到了分布式锁，因此对分布式锁的考验很大

### 9 缓存雪崩

> 概念

在某个时间段，缓存集中过期失效（redis宕机也会导致）。

其实缓存集中过期不是非常致命，而是缓存服务器某个节点宕机或者断网才是致命的，很有可能瞬间就把数据库压垮

> 解决方案

`redis高可用`

多设几台redis，即使一台挂掉之后其他的也可以继续工作。其实就是搭建集群（异地多活）

`限流降级`

缓存失效后，通过加锁或者队列来控制读数据库写缓存的线程数量。比如某个key只允许一个线程查询数据和写缓存，其他线程等待。

`数据预热`

正式部署前，将可能的数据预先访问一遍，这样部分可能大量访问的数据就会加在到缓存中。在即将发生大并发前，手动触发加载缓存不同的key，设置不同的过期时间，让缓存失效的时间尽量均匀。



### 10 扩展

#### 10.1 Redis为什么变慢了

Redis 作为优秀的内存数据库，其拥有非常高的性能，单个实例的 OPS 能够达到 10W 左右。但也正因此如此，当我们在使用 Redis 时，如果发现操作延迟变大的情况，就会与我们的预期不符。

如果你发现你的业务服务 API 响应延迟变长，首先你需要先排查服务内部，究竟是哪个环节拖慢了整个服务，比较高效的做法是，在服务内部集成链路追踪，也就是在服务访问外部依赖的出入口，记录下每次请求外部依赖的响应延时。

如果你发现确实是操作 Redis 的这条链路耗时变长了，那么此刻你需要把焦点关注在业务服务到 Redis 这条链路上。
从你的业务服务到 Redis 这条链路变慢的原因可能也有 2 个：

1. 业务服务器到 Redis 服务器之间的网络存在问题，例如网络线路质量不佳，网络数据包在传输时存在延迟、丢包等情况
2. Redis 本身存在问题，需要进一步排查是什么原因导致 Redis 变慢

#### 10.2 基准性能

##### 10.2.1 什么是基准性能？

基准性能就是指 Redis 在一台负载正常的机器上，其最大的响应延迟和平均响应延迟分别是怎样的

##### 10.2.2 测试

- 测试redis实例 60 秒内的最大响应延迟：

~~~sh
root@872ae87fa554:/data# redis-cli -h 127.0.0.1 -p 6379 --intrinsic-latency 60
Max latency so far: 1 microseconds.
Max latency so far: 2 microseconds.
Max latency so far: 21 microseconds.
Max latency so far: 221 microseconds.
Max latency so far: 319 microseconds.
Max latency so far: 579 microseconds.
Max latency so far: 936 microseconds.
Max latency so far: 1334 microseconds.
Max latency so far: 9160 microseconds.
Max latency so far: 24605 microseconds.

1537757350 total runs (avg latency: 0.0390 microseconds / 39.02 nanoseconds per run).
Worst run took 630609x longer than the average latency.
# 从输出结果可以看到，这 60 秒内的最大响应延迟为 39微秒（0.039毫秒）。
~~~

- 查看一段时间内 Redis 的最小、最大、平均访问延迟：

~~~sh
root@872ae87fa554:/data# redis-cli -h 127.0.0.1 -p 6379 --latency-history -i 1
min: 0, max: 1, avg: 0.41 (91 samples) -- 1.01 seconds range
min: 0, max: 2, avg: 0.29 (91 samples) -- 1.00 seconds range
min: 0, max: 2, avg: 0.33 (91 samples) -- 1.01 seconds range
min: 0, max: 2, avg: 0.31 (91 samples) -- 1.01 seconds range
min: 0, max: 11, avg: 0.59 (86 samples) -- 1.01 seconds range
min: 0, max: 12, avg: 0.55 (89 samples) -- 1.00 seconds range
min: 0, max: 2, avg: 0.53 (91 samples) -- 1.01 seconds range
min: 0, max: 11, avg: 0.47 (89 samples) -- 1.00 seconds range
min: 0, max: 2, avg: 0.49 (91 samples) -- 1.00 seconds range
min: 0, max: 1, avg: 0.29 (92 samples) -- 1.01 seconds range
min: 0, max: 2, avg: 0.44 (91 samples) -- 1.01 seconds range
min: 0, max: 3, avg: 0.45 (91 samples) -- 1.01 seconds range
# 以上输出结果是，每间隔 1 秒，采样 Redis 的平均操作耗时，其结果分布在 0.29 ~ 0.59 毫秒之间。
~~~

##### 10.2.3 Redis 的慢日志（slowlog）

~~~sh
# Redis 提供了慢日志命令的统计功能，它记录了有哪些命令在执行时耗时比较久。
# 查看 Redis 慢日志之前，你需要设置慢日志的阈值。例如，设置慢日志的阈值为 5 毫秒，并且保留最近 500 条慢日志记录：
# 命令执行耗时超过 5 毫秒，记录慢日志
redis-cli CONFIG SET slowlog-log-slower-than 5000
~~~

- 只保留最近 500 条慢日志

~~~sh
CONFIG SET slowlog-max-len 500
# 设置完成之后，所有执行的命令如果操作耗时超过了 5 毫秒，都会被 Redis 记录下来。
~~~

- 查看慢日志

~~~sh
127.0.0.1:6379> slowlog get 5
(empty array)
# 通过查看慢日志，我们就可以知道在什么时间点，执行了哪些命令比较耗时。
~~~

~~~
如果你的应用程序执行的 Redis 命令有以下特点，那么有可能会导致操作延迟变大：
经常使用 O(N) 以上复杂度的命令，例如 SORT、SUNION、ZUNIONSTORE 聚合类命令
使用 O(N) 复杂度的命令，但 N 的值非常大
第一种情况导致变慢的原因在于，Redis 在操作内存数据时，时间复杂度过高，要花费更多的 CPU 资源。
第二种情况导致变慢的原因在于，Redis 一次需要返回给客户端的数据过多，更多时间花费在数据协议的组装和网络传输过程中。
另外，我们还可以从资源使用率层面来分析，如果你的应用程序操作 Redis 的 OPS 不是很大，但 Redis 实例的 CPU 使用率却很高，那么很有可能是使用了复杂度过高的命令导致的。
除此之外，我们都知道，Redis 是单线程处理客户端请求的，如果你经常使用以上命令，那么当 Redis 处理客户端请求时，一旦前面某个命令发生耗时，就会导致后面的请求发生排队，对于客户端来说，响应延迟也会变长。
~~~

#### 10.3 redis慢优化方法

~~~
针对这种情况如何解决呢？
答案很简单，你可以使用以下方法优化你的业务：
尽量不使用 O(N) 以上复杂度过高的命令，对于数据的聚合操作，放在客户端做
执行 O(N) 命令，保证 N 尽量的小（推荐 N <= 300），每次获取尽量少的数据，让 Redis 可以及时处理返回
操作bigkey
如果你查询慢日志发现，并不是复杂度过高的命令导致的，而都是 SET / DEL 这种简单命令出现在慢日志中，那么你就要怀疑你的实例否写入了 bigkey。
Redis 在写入数据时，需要为新的数据分配内存，相对应的，当从 Redis 中删除数据时，它会释放对应的内存空间。
如果一个 key 写入的 value 非常大，那么 Redis 在分配内存时就会比较耗时。同样的，当删除这个 key 时，释放内存也会比较耗时，这种类型的 key 我们一般称之为 bigkey。
此时，你需要检查你的业务代码，是否存在写入 bigkey 的情况。你需要评估写入一个 key 的数据大小，尽量避免一个 key 存入过大的数据。
如果已经写入了 bigkey，那有没有什么办法可以扫描出实例中 bigkey 的分布情况呢？
答案是可以的。
~~~

#### 10.4 扫描 bigkey 的命令

Redis 提供了扫描 bigkey 的命令，执行以下命令就可以扫描出，一个实例中 bigkey 的分布情况，输出结果是以类型维度展示的：

~~~sh
root@872ae87fa554:/data# redis-cli -h 127.0.0.1 -p 6379 -a 'passwd' --bigkeys -i 0.01
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.

# Scanning the entire keyspace to find biggest keys as well as
# average sizes per key type.  You can use -i 0.1 to sleep 0.1 sec
# per 100 SCAN commands (not usually needed).


-------- summary -------

Sampled 0 keys in the keyspace!
Total key length in bytes is 0 (avg len 0.00)


0 hashs with 0 fields (00.00% of keys, avg size 0.00)
0 lists with 0 items (00.00% of keys, avg size 0.00)
0 strings with 0 bytes (00.00% of keys, avg size 0.00)
0 streams with 0 entries (00.00% of keys, avg size 0.00)
0 sets with 0 members (00.00% of keys, avg size 0.00)
0 zsets with 0 members (00.00% of keys, avg size 0.00)
# 从输出结果我们可以很清晰地看到，每种数据类型所占用的最大内存 / 拥有最多元素的 key 是哪一个，以及每种数据类型在整个实例中的占比和平均大小 / 元素数量。
~~~

#### 10.5 Redis 的过期策略

| 过期机制 |                             解释                             |
| :------: | :----------------------------------------------------------: |
| 被动过期 | 当访问某个 key 时，才判断这个 key 是否已过期，如果已过期，则从实例中删除 |
| 主动过期 | Redis 内部主线程中维护了一个定时任务，默认每隔 100 毫秒（1秒10次）就会从<br />全局的过期哈希表中随机取出 20 个 key，<br />然后删除其中过期的 key，如果过期 key 的比例超过了 25%，则继续重复此过程<br />直到过期 key 的比例下降到 25% 以下，或者这次任务的执行耗时超过了 25 毫秒，才会退出循环 |

也就是说如果在执行主动过期的过程中，出现了需要大量删除过期 key 的情况，那么此时应用程序在访问 Redis 时，必须要等待这个过期任务执行结束，Redis 才可以服务这个客户端请求。

此时就会出现，应用访问 Redis 延时变大。

如果此时需要过期删除的是一个 bigkey，那么这个耗时会更久。而且，这个操作延迟的命令并不会记录在慢日志中。

因为慢日志中只记录一个命令真正操作内存数据的耗时，而 Redis 主动删除过期 key 的逻辑，是在命令真正执行之前执行的



#### 10.6 规避redis集中过期

集中过期 key 增加一个随机过期时间，把集中过期的时间打散，降低 Redis 清理过期 key 的压力

Redis 是 4.0 以上版本，可以开启 lazy-free 机制，当删除过期 key 时，把释放内存的操作放到后台线程中执行，避免阻塞主线程

~~~shell
lazyfree-lazy-expire yes # 当删除过期 key 时，把释放内存的操作放到后台线程中执行
~~~



#### 10.7 实例内存上限导致redis慢

如果你的 Redis 实例设置了内存上限 maxmemory，那么也有可能导致 Redis 变慢。

原因在于，当 Redis 内存达到 maxmemory 后，每次写入新的数据之前，Redis 必须先从实例中踢出一部分数据，让整个实例的内存维持在 maxmemory 之下，然后才能把新数据写进来。



#### 10.8 淘汰策略(回收机制)

|     淘汰策略     |                             解释                             |
| :--------------: | :----------------------------------------------------------: |
|   allkeys-lru    |       不管 key 是否设置了过期，淘汰最近最少访问的 key        |
|   volatile-lru   |          只淘汰最近最少访问、并设置了过期时间的 key          |
|  allkeys-random  |            不管 key 是否设置了过期，随机淘汰 key             |
| volatile-random  |                只随机淘汰设置了过期时间的 key                |
| noeviction(默认) | 不淘汰任何 key，实例内存达到 maxmeory 后，再写入新数据直接返回错误 |
|   allkeys-lfu    | 不管 key 是否设置了过期，淘汰访问频率最低的 key（4.0+版本支持） |
|   volatile-lfu   |   只淘汰访问频率最低、并设置了过期时间 key（4.0+版本支持）   |

一般最常使用的是 allkeys-lru / volatile-lru 淘汰策略，它们的处理逻辑是，每次从实例中随机取出一批 key（这个数量可配置），然后淘汰一个最少访问的 key，之后把剩下的 key 暂存到一个池子中，继续随机取一批 key，并与之前池子中的 key 比较，再淘汰一个最少访问的 key。以此往复，直到实例内存降到 maxmemory 之下。

### 11 常遇问题

- Redis的数据结构及使用场景?

~~~shell
# String字符串
字符串类型是 Redis 最基础的数据结构，首先键都是字符串类型，而且 其他几种数据结构都是在字符串类型基础上构建的，我们常使用的 set key value 命令就是字符串。常用在缓存、计数、共享Session、限速等。

# Hash哈希
在Redis中，哈希类型是指键值本身又是一个键值对结构，哈希可以用来存放用户信息，比如实现购物车。

# List列表（双向链表）
列表（list）类型是用来存储多个有序的字符串。可以做简单的消息队列的功能。

# Set集合
集合（set）类型也是用来保存多个的字符串元素，但和列表类型不一 样的是，集合中不允许有重复元素，并且集合中的元素是无序的，不能通过索引下标获取元素。 利用 Set 的交集、并集、差集等操作，可以计算共同喜好，全部的喜好，自己独有的喜好等功能。

# Sorted Set有序集合（跳表实现）
Sorted Set 多了一个权重参数 Score，集合中的元素能够按 Score 进行排列。可以做排行榜应用，取 TOP N 操作。
~~~

- Redis持久化的几种方式?

Redis为了保证效率，数据缓存在了内存中，但是会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件中，以保证数据的持久化。

> RDB

RDB：快照形式是直接把内存中的数据保存到一个dump的文件中，定时保存，保存策略。
当Redis需要做持久化时，Redis会fork一个子进程，子进程将数据写到磁盘上一个临时RDB文件中。当子进程完成写临时文件后，将原来的RDB替换掉。

> AOF

AOF：把所有的对Redis的服务器进行修改的命令都存到一个文件里，命令的集合。
使用AOF做持久化，每一个写命令都通过write函数追加到appendonly.aof中。 
aof的默认策略是每秒钟fsync一次，在这种配置下，就算发生故障停机，也最多丢失一秒钟的数据。

缺点是对于相同的数据集来说，AOF的文件体积通常要大于RDB文件的体积。根据所使用的fsync策略，AOF的速度可能会慢于RDB。

- 单线程的Redis为什么快?

~~~shell
纯内存操作
单线程操作，避免了频繁的上下文切换
合理高效的数据结构
采用了非阻塞I/O多路复用机制
~~~

- 如何解决Redis缓存雪崩问题?

~~~shell
使用 Redis 高可用架构：使用 Redis 集群来保证 Redis 服务不会挂掉
缓存时间不一致，给缓存的失效时间，加上一个随机值，避免集体失效
限流降级策略：有一定的备案，比如个性推荐服务不可用了，换成热点数据推荐服务
~~~

- 如何解决Redis缓存穿透问题?

~~~shell
在接口层做校验
存null值（缓存击穿加锁）
布隆过滤器拦截：将所有可能的查询key 先映射到布隆过滤器中，查询时先判断key是否存在布隆过滤器中，存在才继续向下执行，如果不存在，则直接返回。 布隆过滤器将值进行多次哈希bit存储，布隆过滤器说某个元素在，可能会被误判。布隆过滤器说某个元素不在，那么一定不在
~~~

- Redis并发竞争key如何解决？

~~~she
可以利用分布式锁和时间戳来解决.
利用消息队列解决.
~~~

- Redis有序集合zset底层怎么实现的？

~~~shell
Redis中的set数据结构底层用的是跳表实现的.
跳表是一个随机化的数据结构，实质就是一种可以进行二分查找的有序链表。
跳表在原有的有序链表上面增加了多级索引，通过索引来实现快速查找。
跳表不仅能提高搜索性能，同时也可以提高插入和删除操作的性能。
(1)跳表是可以实现二分查找的有序链表；
(2)每个元素插入时随机生成它的level；
(3)最低层包含所有的元素；
(4)如果一个元素出现在level(x)，那么它肯定出现在x以下的level中；
(5)每个索引节点包含两个指针，一个向下，一个向右；
(6)跳表查询、插入、删除的时间复杂度为O(log n)，与平衡二叉树接近；
~~~

- 为什么Redis选择使用跳表而不是红黑树来实现有序集合？(O(logN))

~~~shell
Redis的有序集合支持的操作：
插入元素
删除元素
查找元素
有序输出所有元素
查找区间内所有元素
其中，前4项红黑树都可以完成，且时间复杂度与跳表一致。
但是，最后一项，红黑树的效率就没有跳表高了。
在跳表中，要查找区间的元素，我们只要定位到两个区间端点在最低层级的位置，然后按顺序遍历元素就可以了，非常高效。
而红黑树只能定位到端点后，再从首位置开始每次都要查找后继节点，相对来说是比较耗时的。 
此外，跳表实现起来很容易且易读，红黑树实现起来相对困难，所以Redis选择使用跳表来实现有序集合。
~~~

- 跳表的查询过程是怎么样的，查询和插入的时间复杂度

~~~shell
先从第一层查找，不满足就下沉到第二层找，因为每一层都是有序的，写入和插入的时间复杂度都是O(logN)
~~~

