
# MySQL

## 逻辑架构

- 客户端连接器 （负责和客户端建立连接）
- 系统管理和控制工具 
- 连接池 （管理用户连接，监听并接收连接的请求，转发所有连接的请求到线程管理模块）
- SQL 接口：接受用户的SQL命令，并且返回SQL执行结果
- 解析器：语法解析， 词法解析）
- 查询优化器：在查询之前会使用查询优化器对查询进行优化，explain 执行计划
- 缓存：在MySQL8中移除
- 存储引擎：存取数据、建立和更新索引、查询数据
- 文件和日志

## 设置配置

- `/etc/mysql/my.cnf` 

## 端口

- MySQL数据库默认端口 3306

## 日志文件

- MySQL 从物理结构上可以分为日志文件和数据及索引文件
- MySQL 在 Linux 中的数据索引文件和日志文件通常放在 `/var/lib/mysql` 目录下
- MySQL 通过日志记录了数据库操作信息和错误信息

## 常用日志文件

- 错误日志
- 二进制日志
- 查询日志：general_query.log 
- 慢查询日志：slow_query_log.log 
- 事务重做日志：redolog 
- 中继日志：relaylog
- Undo log
- `mysql> show variables like 'log_%';` 查看当前数据库中的日志使用信息

## 错误日志

- 默认开启，错误日志记录了运行过程中遇到的所有严重的错误信息，以及 MySQL 每次启动和关闭的详细信息
- log_error：指定错误日志存储位置
- 在8.0.3 被移除 log-warnings：配置警告信息级别 
- 0 禁用警告日志,只记录错误
- 1 记录错误和严重警告
- 2 记录错误和所有警告
- 3 记录错误、警告和提示信息
- 4 记录错误、警告、提示和调试信息
- log_error_verbosity

## 二进制日志

- 默认关闭，binlog 记录了数据库所有的 DDL 语句和 DML 语句，不包括 DQL 语句
- 语句以事件的形式保存，描述了数据的变更顺序，还包括了每个更新语句的执行时间信息
- 主要用于实现 mysql 主从复制、数据备份、数据恢复
- 不需要查看

## 通用查询日志

- 默认关闭，通用查询日志会记录用户的所有操作，包含增删查改等信息
- 在并发操作大的环境下会产生大量的信息从而导致不必要的磁盘IO，会影响MySQL的性能
- `general_log=ON|OFF`
- `general_log_file=`指定路径

## 慢查询日志

- 默认关闭，记录执行时间超过 long_query_time 秒的所有查询，收集查询时间比较长的SQL语句
- `slow_query_log=ON|OFF`
- `long_query_time=`慢查询的阈值(秒)
- `slow_query_log_file=`日志存储路径

## 数据文件

- `show variables like '%datadir%';` 查看 mysql 数据文件
- ibdata 文件：使用系统表空间存储表数据和索引信息，所有表共同使用一个或者多个ibdata文件

## InnoDB 存储引擎的数据文件

- 表名.frm文件：存储表相关的数据信息，主要包括表结构的定义信息  在MySQL 8.0中, frm 文件已被移除
- 表名.ibd文件：使用独享表空间存储表数据和索引信息，一张表对应一个ibd文件
- 在MySQL 8.0中，InnoDB 中的表定义和表数据存在 idb 文件中

## MyISAM 存储引擎的数据文件

- 表名.frm文件：存储表相关的数据信息，主要包括表结构的定义信息 在MySQL 8.0中, frm 文件已被移除
- 表名.myd文件：存储表数据信息 
- 表名.myi文件：存储索引

## 一条SQL语句的完整执行流程

- MySQL 可以分为 Server层和存储引擎层
- 第1步 连接到数据库
- 第2步 查询缓存
- 第3步 分析SQL语句
- 第4步 优化SQL语句
- 第5步 执行SQL语句

## Server层

- 连接器、查询缓存、分析器、优化器、执行器等
- 所有的内置函数（如日期、时间、数学和加密函数等）
- 存储过程、触发器、视图

## 存储引擎层

- 负责数据的存储和提取
- 可插拔式存储引擎：InnoDB、MyISAM、Memory
- 从 MySQL 5.5 版本开始，默认存储引擎是 InnoDB

## 连接到数据库

- `mysql -u username -p password` 或 `mysql -u username -p` 登录
- `mysql -h host -u username -p password`  指定 host 登录
- 连接完成后，如果你没有后续的动作，这个连接就处于空闲状态
- 客户端如果太长时间没动静，连接器就会自动断开
- 这个时间是由参数 wait_timeout 控制的默认值是 8 小时
- `mysql> show processlist;` 其中的 Command 列显示为 Sleep 表示空闲连接

## 查询缓存

- MySQL 拿到一个查询请求后，会先到查询缓存中看之前有没有执行过这条语句
- 之前执行过的语句和其结果，可能会以 key-value 对的形式直接缓存在内存中
- key 是查询的语句 hash 之后的值，value 是查询的结果
- 如果查询语句在缓存中，会直接返回给客户端
- 如果查询语句不在查询缓存中，就会继续后面的执行阶段，执行完成后，执行结果会被存入查询缓存中
- 不建议使用MySQL的内置缓存功能
- `mysql> show variables like 'query_cache_type';` 查看是否开启缓存, 在 MySQL 8.0 版本中, 查询缓存功能已被移除

## 为什么不建议使用MySQL的查询缓存？

- 查询缓存的失效非常频繁，只要有对一个表的更新，这个表上所有的查询缓存都会被清空 
- 对于更新压力大的数据库，查询缓存的命中率会非常低
- 功能不如专业的缓存工具更好：redis

## 分析SQL语句

- 对SQL语句字符串做分析，判断请求的语法是否正确
- 从SQL语句字符串中将要查询的表、列和各种查询条件提取出来
- 本质上是对一个SQL语句编译的过程，涉及词法解析、语法分析、预处理器
- 词法分析是把一个完整的 SQL 语句分割成一个个的字符串 
- 语法分析器根据词法分析的结果做语法检查，判断输入的 SQL 语句是否满足 MySQL 语法
- 预处理器会进一步去检查解析树是否合法，比如表名是否存在，语句中表的列是否存在，检验用户是否有表的操作权限

## 优化SQL语句

- 对查询进行优化，作用是根据解析树生成不同的执行计划，然后选择最优的执行计划
- MySQL 使用的是基于成本模型的优化器，选择 执行时成本最小的执行计划 Explain 
- io_cost 和 cpu_cost 的开销总和
- `mysql> show status like 'Last_query_cost';` 查看上次查询成本开销

## 优化器

- 当有多个索引可用的时，决定使用哪个索引
- 在一个语句有多表关联（join）时，决定各个表的连接顺序，以哪个表为基准表

## 执行SQL语句

- 判断执行权限
- 调用存储引擎接口查询

## 存储引擎种类

- InnoDB 支持事务和行级锁，支持外键，支持分布式事务的解决方案
- MyISAM 不支持事务
- Memory 内存存储引擎，拥有极高的插入，更新和查询效率
- CSV CSV 存储引擎是基于 CSV 格式文件存储数据，应用于跨平台的数据交换

## 存储引擎查看和设置

- `mysql> show engines;` 查看支持的存储引擎

```sql
create table t (
a int primary key, 
b int
) engine=innodb;
```


## 存储引擎的区别

|                      | InnoDB               | MyISAM             |
| -------------------- | -------------------- | ------------------ |
| 是否支持事务         | 是                   | 否                 |
| 锁的粒度             | 表锁，行锁           | 表锁               |
| 是否支持外键         | 是                   | 否                 |
| 数据的存储方式       | 索引和数据存放在一起 | 索引和数据分开存放 |
| 索引                 | 聚簇索引             | 非聚簇索引         |
| 数据存储的结构       | B+Tree               | B+Tree             |
| 是否要求表必须有主键 | 是                   | 否                 |

## 索引的分类

- 主键索引 每个表有且只有一个主键索引， 如果没有指定主键索引，对于表会默认生成一个非空且唯一的主键索引
- 唯一索引 可以为空，但不能重复，一个表可以有多个唯一索引
- 普通索引 可以为空，可以重复

## 主键索引

- 如果没有显示指定索引，Mysql 会自动选择一个可以唯一表示数据记录的列作为主键
- 如果不存在可以唯一标识数据记录的列，Mysql 会自动生成一个隐含字段 \_rowid 作为主键

## 创建索引

- 如果索引名没有指定，默认使用列名作为索引名

- 主键索引

```sql
CREATE table t (
	id int not null PRIMARY KEY,
	name varchar(20)
)

CREATE table t (
	id int not null,
	name varchar(20),
	PRIMARY KEY (id)
)
```

- 唯一索引 

```sql
CREATE table t (
	id int not null,
	name varchar(20),
	UNIQUE KEY idx_name(name)
)
```

- 普通索引

```sql
CREATE table t (
	id int not null,
	name varchar(20),
	KEY idx_name (name)
)
```

## 添加索引

- `SHOW INDEX FROM t `查看索引
- `ALTER TABLE t add PRIMARY KEY (id)` 对表中的 id 字段添加主键索引
- `ALTER TABLE t add UNIQUE KEY idx_name (name)` 对表中的 name 字段添加唯一索引
- `ALTER TABLE t add KEY idx_name (name)` 对表中的 name 字段添加普通索引

## 删除索引

- `ALTER TABLE t DROP PRIMARY KEY` 删除 t 表中 主键索引
- `ALTER TABLE t DROP index idx_name` 删除 t 表中 唯一索引或普通索引

## 复合索引

- 复合索引是多个列同时创建一个索引
- `ALTER TABLE t add KEY idx_name_age (name, age)` 注意(name, age)和(age, name) 是不同的

## 索引为什么不选择 Hash 算法？

- hash索引不能快速处理范围查询
- hash索引没有顺序，对排序数据需要重排
- hash索引不能 like 模糊查询
- hash 冲突 影响性能 不稳定

## B+Tree 对比 B-Tree

- 叶子节点存储了所有节点的数据
- 叶子节点通过链表的方式连接

## 辅助索引

- TODO

## 聚簇索引

- 聚簇索引是指数据库表中数据的物理顺序和键值的索引顺序相同，数据存储和索引放在一起

## 非聚簇索引

- 将数据存储和索引分开， 索引结构的叶子节点指向了对应的数据

## 为什么选择/不选择索引？

- 提高表数据的检索效率
- 降低排序成本
- 增加数据的维护成本，需要空间存储， 新增，删除，修改需要对索引结构进行修改维护

## 如何选取索引？

- 频繁作为查询条件的字段适合作为索引，前提是唯一性不能太差
- 更新频繁的字段不适合作为索引
- 不出现在 where 子句中的字段不适合创建索引

## 执行计划

- `explain` 添加在SQL语句之前

### id

- 每一条 SQL 语句都会有一个编号 id
- id 值越大，对应的 SQL 语句越先执行
- id 值相同， 从上往下依次执行
- id 值为 null，在最后执行
 
### select_type

- select_type 表示对应行是简单还是复杂查询
- simple 简单查询 查询语句里不包括自查询和 union
- primary 复杂查询中 最外层的 select， 也就是最后执行的
- subquery 包含在 select 中的子查询 （不在 from 子句中）
- derived 包含在 from 子句中的子查询， MySQL 会将结果放在一个临时表内 （派生表）
- union 在 union 中的第二个和随后的 select

### table

- 显示从哪个表中查询数据
- <union1,2>  id = 1 和 id = 2的查询结构集的联合
- <derived,1> 基于 id = 1 的一个派生表

### type

- 链接类型
- system > const > eq_ref > range > index > ALL
- system 表仅有一行 
- const 表最多有一个匹配行，在查询开始时被读取
- eq_ref 多表连接中使用 primary key 或 unique key 作为关联条件
- ref 多表连接中使用 普通索引 作为关联条件
- range 只检索给定范围的行
- index 扫描索引树
- ALL 扫描完整表
- NULL MySQL 在优化阶段分解查询语句，在执行阶段不用访问表或索引， 比如 max（索引）

### key_len

|                    | 索引列的总长度    |
| ------------------ | ------- |
| char(n) utf-8下    | 3n      |
| varchar(n) utf-8下 | 3n + 2  |
| int                | 4个字节 |
| bigint             | 8个字节 |
| date               | 3个字节 |

### 其余

- possible_keys 可能使用的索引列
- key 实际使用的索引列
- ref 使用的哪一个列来关联
- rows 估计要扫描的行数

### Extra

- Extra 显示的是执行计划的扩展说明信息
- Not exists 
- Using index 需要查询的字段是索引中的字段， 不需要回表操作，只需要在索引树上找
- Using where 
- Using index condition 判断条件在索引树的列中包含
- Using temporary 创建临时表来处理查询， 可以进行优化
- Using filesort 需要对潮汛的结果集进行排序， 数据小在内存中排序， 数据大借助磁盘外部排序，可以进行优化

## 其他命令

- `SHOW TABLE STATUS FROM demo`  查看 demo 数据库中的表的状态
- `SHOW CREATE TABLE demo` 查看创建 demo 表的 SQL 语句 
- `SHOW VARIABLES LIKE 'default_storage_engine'` 查看默认存储引擎
- `SHOW VARIABLES LIKE 'datadir'` 查看数据存储的目录
- `ibd2sdi demo.ibd` 查看 idb 文件，以 json 格式显示
- `SHOW PROFILES` 查看性能

## LIKE

- % 匹配任意多个字符
- _ 匹配一个字符

## 自带数据库

- information_schema 保存其他数据库的信息
- mysql 用户和权限相关的信息
- performance_schema 保存监控的性能相关的数据
- sys 运行情况

## Unicode 字符集

- `utf8mb4` Unicode 字符集的 UTF-8 编码，每个字符使用一到四个字节。
- `utf8mb3` Unicode 字符集的 UTF-8 编码，每个字符使用一到三个字节，此字符集在 MySQL 8.0 中已弃用
- `utf8` 的别名 `utf8mb3`。在 MySQL 8.0 中，这个别名已被弃用

## 数据类型

### 整数类型（精确值）

| 类型      | 字节数 |
| --------- | ------ |
| TINYINT   | 1      |
| SMALLINT  | 2      |
| MEDIUMINT | 3      |
| INT       | 4      |
| BIGINT    | 8      |

### 定点类型 (精确值)

- `DECIMAL(精度,小数位数)` 精度表示值存储的有效位数，小数位数表示小数点后可以存储的位数
- `NUMERIC` 同 `DECIMAL`

### 浮点类型（近似值）

- FLOAT 单精度 4字节
- DOUBLE 双精度 8字节

### 字符串类型

- char
- varchar 可变长度 

### 日期类型

- DATE `YYYY-MM-DD`
- DATETIME `YYYY-MM-DD hh:mm:ss`
- TIMESTAMP 

## 脏读(Dirty Read)

- 脏读是数据库事务中的一个概念, 指一个事务读取了另一个事务还未提交的修改的数据
- 例如：事务A更新一个字段,事务B随后读取同一字段值,然后事务A回滚，这样事务B读到的就是无效数据








 
