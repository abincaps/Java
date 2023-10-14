
# Redis

## Redis

- 基于内存的 key-value 结构的数据库
- 内存读写性能高
- 存储热点数据
- 键的类型只能是字符串

## 官网

- https://redis.io

## CLI 命令行界面

- `redis-cli` 进入命令行界面, 默认情况下，连接到地址为 127.0.0.1, 端口为 6379 的服务器
- `redis-cli -h 127.0.0.1 -p 6379` 指定连接的 host 和 port

## Data types 数据类型

- string
- hash
- list
- set

## string

- `keys *` 返回所有的 key
- `set key value` 设置指定的 key-value
- `mset key1 value1 key2 value2 ...` 设置多个 key-value
- `get key` 返回指定的 key 的 value
- `mget key1 key2 ...` 返回多个 key 的 value
- `setex key seconds value` 设置指定的 key-value并指定`seconds`过期时间，在生命周期中被`set`重新设置，会清除时间
- `psetex key milliseconds value` 设置指定的 key-value并指定`seconds`过期时间
- `strlen key`
- `append key`
- `incr key` 指定 key 中的值增加 1
- `decr key` 指定 key 中的值减少 1
- `incrby key increment`  指定 key 中的 value 增加指定的 increment
- `decrby key increment`  指定 key 中的 value 减少指定的 increment
- `incrbyfloat key increment`

## hash

- value 只能存储 string
- `hset key field value [field value ...]`
- `hgetall key` 返回指定哈希表的所有字段和对应的值
- `hexists key field` 判断 key 中的 field 是否存在
- `hkeys key` 返回指定 key 中的所有的 field
- `hvals key` 返回指定 key 中的所有的 value
- `hincrby key field increment` 指定 key 中的 field 对应的 value 增加指定的 increment

## list

- `lpush key element [element ... ]`
- `rpush key element [element ... ]`
- `lrange key start stop`  -1 表示到结尾 \[start, stop) 区间内的 element
- `lindex key index`  返回指定 index 的 element
- `llen key` 获取列表长度
- `lpop key` 
- `rpop key` 
- `blpop key [key ...] timeout` 如果列表中没有任何元素，将会阻塞（等待）直到有元素，或者超过指定的 timeout
- `brpop key [key ...] timeout`
- `lrem key count element`  count 等于 0 时，删除所有指定的 element， count 大于 0 时，从列表头部开始删除，count 小于 0 时，从列表尾部开始

## set

- `sadd key member [member ...]`
- `smembers key` 返回指定 key 中的所有成员
- `srem key member [member ...]` 删除指定 key 中的指定的 member
- `scard key` 返回指定 key 中的成员数量
- `sismember key member` 判断是否存在该成员
- `sinter key [key ...]` 交集
- `sunion key [key ...]` 并集
- `diff key [key ...]` 差集

## sorted_set

- `zadd key [NX|XX] [CH] [INCR] score member [score member ...]` score 是排序字段
- `zrange key start stop [withscores]` 可选的`withscores`参数，返回的列表包含 score
- `zrem key member [member ...]`

## 格式规范

- `set 表名:主键:主键值:key value`
- `get 表名:主键:主键值:key`

## 通用命令

- `del key` 删除指定的 key
- `exists key [key ...]` 返回存在的 key 的计数
- `type key` 返回指定`key`中存储的值的类型
- `expire key seconds` 设置超时，超时后，key 将被自动删除
- `pexpire key milliseconds` 
- `expireat key timestamp` 
- `ttl key` 返回 key 的剩余生存时间，返回 `-2`， key 不存在， 返回 `-1`，key存在但没有关联的过期时间
- `pttl key`
- `persist key` 删除`key`上的超时, 从易失性转变为持久
- `rename key newkey` 如果 `newkey` 已经存在，则会被覆盖
- `renamex key newkey` 如果 `newkey` 已经存在，则会被覆盖

## 数据库命令

- `flushdb` 清空当前数据库
- `flushall` 清空所有数据库
- `dbsize` 返回当前选定数据库中的键数

## 常用命令

- `shutdown`
- `shutdown save`
- `redis-server --port 6379` 

## 配置文件

- `/etc/redis/redis.conf` 配置文件的路径

## 常见配置参数

- `bind 127.0.0.1 ::1`, 绑定 host, `::1` 绑定到 IPv6 的本地回环地址
- `port 6379`
- `timeout 0` Close the connection after a client is idle for N seconds (0 to disable)
- `databases 16` Set the number of databases. db id is a number between 0 and 'databases'-1
- `loglevel notice` Specify the server verbosity level
	- `debug` (useful for development/testing)
	- `verbose` (not a mess like the debug level)
	- `notice` (in production probably)
	- `warning` (only very important / critical messages are logged)
- `daemonize yes` Redis run as a daemon. Redis will write a pid file in /var/run/redis.pid when daemonized
- `pidfile /var/run/redis/redis-server.pid` 
- `save <seconds> <changes>` Save the DB on disk. After \<seconds> seconds if at least \<changes> key changed
- `protected-mode yes` The server only accepts connections from clients connecting from the IPv4 and IPv6 loopback addresses 127.0.0.1 and ::1, and from Unix domain

## 持久化

- RDB（Redis Database Backup） snapshot 快照
- AOF（Append-Only File） 日志

## RDB

- `dbfilename dump.rdb` The filename where to dump the DB
- `dir /var/lib/redis` 指定存储 rdb 文件的路径
- `rdbcompression yes` Compress string objects using LZF when dump .rdb databases? If you want to save some CPU in the saving child set it to 'no'
- `rdbchecksum yes` RDB 文件校验

## AOF

- `appendonly yes` 开启
- `appendfilename "appendonly.aof"` The name of the append only file 
- `appendfsync everysec` everysec: fsync only one time every second. Compromise.
- `appendfsync always` always: fsync after every write to the append only log. Slow, Safest.         
- `appendfsync no` no: don't fsync, just let the OS flush the data when it wants. Faster.

## 事务处理

- `multi` 开启事务
- `exec` 执行事务
- `discard` 取消执行事务

## 锁

- 监控数据的变化
- `watch key [key ...]` 在 exec 之前， 如果 key 发生了变化，终止事务的执行

## 分布式锁

- `setnx key value` Set key to hold string value if key does not exist. SETNX is short for "set if not exists".

## 主从复制

- 主服务器 Maste 接收来自客户端的写操作，并发送给从服务器
- 从服务器 Slave 接收来自主服务器的写操作，和主服务器保持数据一致，进行读操作
- 读写分离
- 负载均衡
- 数据备份

## Jedis

- TODO

## Spring Data Redis


