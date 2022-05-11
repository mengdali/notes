# 1.键值设计

> 【建议】: 可读性和可管理性

以业务名(或数据库名)为前缀(防止key冲突)，用冒号分隔，比如业务名:表名:id

~~~shell
ugc:video:1
~~~

> 【建议】：简洁性

保证语义的前提下，控制key的长度，当key较多时，内存占用也不容忽视，例如：

~~~shell
user:{uid}:friends:messages:{mid}
# 简化为
u:{uid}:fr:m:{mid}
~~~

> 【强制】：不要包含特殊字符

反例：包含空格、换行、单双引号以及其他转义字符

# 2. value设计

> 【强制】：拒绝bigkey(防止网卡流量、慢查询)

string类型控制在10KB以内，hash、list、set、zset元素个数不要超过5000。

非字符串的bigkey，不要使用del删除，使用hscan、sscan、zscan方式渐进式删除，同时要注意防止bigkey过期时间自动删除问题

例如一个200万的zset设置1小时过期，会触发del操作，造成阻塞，而且该操作不会不出现在慢查询中(latency可查)

> 【推荐】：选择适合的数据类型。

反例：

```
set user:1:name tom
set user:1:age 19
set user:1:favor football
```

正例:

```
hmset user:1 name tom age 19 favor football
```

# 3. 控制key的生命周期

建议使用expire设置过期时间(条件允许可以打散过期时间，防止集中过期)，不过期的数据重点关注idletime。

# 4.命令相关

> #### O(N)命令关注N的数量

例如hgetall、lrange、smembers、zrange、sinter等并非不能使用，但是需要明确N的值。有遍历的需求可以使用hscan、sscan、zscan代替。

> #### 禁用命令

禁止线上使用keys、flushall、flushdb等，通过redis的rename机制禁掉命令，或者使用scan的方式渐进式处理。

> #### 合理使用select

redis的多数据库较弱，使用数字进行区分，很多客户端支持较差，同时多业务用多数据库实际还是单线程处理，会有干扰。

> #### 使用批量操作提高效率

~~~shell
原生命令：例如mget、mset。
非原生命令：可以使用pipeline提高效率。
~~~

但要注意控制一次批量操作的**元素个数**(例如500以内，实际也和元素字节数有关)。

> #### Redis事务功能较弱，不建议过多使用

Redis的事务功能较弱(不支持回滚)，而且集群版本(自研和官方)要求一次事务操作的key必须在一个slot上(可以使用hashtag功能解决)

> ### 慎用将redis做为消息队列

如没有非常特殊的需求，严禁将 Redis 当作消息队列使用。redis 当作消息队列使用，会有容量、网络、效率、功能方面的多种问题。

如需要消息队列，可使用高吞吐的 Kafka 或者高可靠的 RocketMQ，nsq,(花园同步有时间前后要求，且量不大才使用的)。