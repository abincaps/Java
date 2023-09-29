
# MySQL

## SQL

- SQL（Structured Query Language）结构化查询语言
- 所有关系型数据库的统一查询规范
- 不区分大小写

## SQL分类

- 数据定义语言 DDL （Data Definition Language）
- 数据操作语言 DML （Data Manipulation Language）
- 数据查询语言 DQL （Data Query Language）
- 数据控制语言 DCL （Data Control Language）

## DDL常用命令

- `create database database_name;` 创建指定名称的数据库
- `use database_name;` 指定使用的数据库
- `create database database_name character set utf8mb4;`  创建数据库时指定字符集
- `ALTER DATABASE demo CHARACTER SET utf8mb4;` 修改数据库字符集
- `drop database database_name;` 删除数据库
- `DESC database_name;` 查看表结构
- `create table database_name like database_name；` 创建和指定的表的表结构相同的表
- `show databases;` 查看所有数据库列表
- `SELECT DATABASE();` 查询当前正在使用的数据库 `DATABASE()` 函数
- `SHOW TABLES;` 列出数据库包含的所有表
- `show create database database_name;` 查询创建指定数据库的SQL语句
- `DROP TABLE IF EXISTS table_name`; 判断表是否存在再删除
- `RENAME TABLE test TO new_test;`  修改表名
- `ALTER TABLE test ADD tname varchar(10);` 向表添加字段
- `ALTER TABLE test modify tname varchar(10);` 修改表中字段的类型
- `ALTER TABLE test change tname new_name varchar(10);` 修改表中字段名以及类型
- `CREATE TABLE B AS SELECT * FROM A` 创建 B 表 并拷贝 A 表中的所有数据

## DML常用命令

- `INSERT INTO student(sid, sname, age) VALUES(1, "name", 20);` 向指定表插入指定字段数据
- `INSERT INTO student VALUES(1, "name", 20);` 向指定表插入全部字段数据
- `UPDATE student SET sname = "sname" WHERE sid = 1;`  条件更新指定的字段的值
- `UPDATE student SET sname = "sname", age = 10 WHERE sid = 1;` 指定字段值的条件来更新指定的多个字段的值
- `TRUNCATE TABLE table_name;` 删除整张表再创建新的空表，执行效率高

## DQL查用命令

- `SELECT * FROM table_name;` 查询表中所有记录
- `SELECT sid, sname FROM table_name;` 查询指定字段的所有记录
- `SELECT sid AS 编号 FROM student;` 查询结果的列名使用别名
- `SELECT DISTINCT sname FROM student;` 去重

## 聚簇索引

- 是一种数据存储方式
- 聚簇索引是指数据库表中数据的物理顺序和键值的索引顺序相同，数据和索引的数据结构存储在一起
- InnoDB 的主键索引中所有叶子节点都存储了对应行的数据

## 非聚簇索引

- 将数据存储和索引分开，索引结构的叶子节点指向了对应的数据

## 二级索引（辅助索引）

- 除了聚簇索引之外的所有索引都是二级索引

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

- `/etc/mysql/my.cnf` 配置文件路径

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

## 存储引擎查看

- `mysql> show engines;` 查看支持的存储引擎
- `mysql> show engine innodb status;` 查看innodb存储引擎状态

## 存储引擎设置

```sql
create table t (
a int primary key, 
b int
) engine=innodb;
```

## InnoDB内存结构

- Buffer Pool 缓冲池
- Change Buffer 修改缓冲
- Adaptive Hash Index 自适应索引
- Log Buffer 日志缓冲

## Buffer Pool 缓冲池

- Buffer Pool 用于加速热点数据读写，将热点数据缓存在内存，最大限度地减少磁盘IO
- 默认大小为128MB
- Buffer Pool 中的数据以页为存储单位
- 实现的数据结构是以页为单位的单链表
- Buffer Pool 使用LRU算法（Least Recently Used 最近最少使用）淘汰非热点数据页
- LRU：根据页数据的历史访问来淘汰数据，优先淘汰最近没有被访问到的数据
- `mysql> show variables like 'innodb_buffer_pool_size';` 查看 Buffer Pool 大小

## Change Buffer 修改缓冲

- Change Buffer 用于加速非热点数据中二级索引的写入操作
- 由于二级索引数据的不连续性，导致 修改二级索引时需要进行频繁的磁盘 IO 消耗大量性能
- Change Buffer 对二级索引的修改操作会录入 redo log 中
- 在缓冲到一定量或系统较空闲时进行 merge 操作将修改写入磁盘中
- Change Buffer 在系统表空间中有相应的持久化区域
- Change Buffer 大小默认占 Buffer Pool 的25%，最大50%
- 物理结构为一棵名为 ibuf 的 B Tree

## AHI 自适应哈希索引

- 自适应哈希索引（Adaptive Hash Index）用于实现对于热数据页的一次查询（哈希索引O1查询）
- AHI 是建立在索引之上的索引, 使用聚簇索引进行数据页定位的时候需要根据索引树的高度从根节点走到叶子节点
- AHI 的大小为 Buffer Pool 的 1/64
- AHI 所作用的目标是频繁查询的数据页和索引页
- 由于数据页是聚簇索引的一部分，因此 AHI 是建立 在索引之上的索引
- 对于二级索引，若命中 AHI，则将直接从 AHI 获取二级索引页的记录指针, 再根据主键沿着聚簇索引查找数据
- 若聚簇索引查询同样命中 AHI，则直接返回目标数据页的记录指针，此时就可以根据记录指针直接定位数据页
- `mysql> show variables like 'innodb_adaptive_hash_index';` 查看是否开启自适应哈希索引配置，默认是开启的

## Log Buffer 日志缓冲

- 使用 Log Buffer 来缓冲日志文件的写入操作
- 内存写入和日志文件顺序写

## InnoDB磁盘结构

- InnoDB 将所有数据都逻辑地存放在一个空间中，称为 Tablespace 表空间
- 表空间由 Segment 段 、extent 区、Page 页 组成
- 关闭独占表空间 innodb_file_per_table=0，所有基于InnoDB存储引擎的表数据都会记录到系统 表空间，即 ibdata1 文件

## 表空间

- System Tablespace 系统表空间
- File-per-table Tablespace 独立表空间
- General Tablespace 通用表空间
- Undo Tablespace 回滚表空间 
- The Temporary Tablespace 临时表空间

## 系统表空间

- 系统表空间是 InnoDB 数据字典、双写缓冲、修改缓冲和回滚日志的存储位置
- 如果关闭独立表空间，系统表空间将存储所有表数据和索引
- 默认下是一个初始大小 12MB 的 ibdata1 文件
- `innodb_data_file_path=`系统表空间的文件路径和大小
- Data Dictionary 数据字典： 数据字典是由各种表对象的元数据信息（表结构，索引，列信息）组成的内部表
- Doublewrite Buffer 双写缓冲：双写缓冲用于保证写入磁盘时页数据的完整性，防止发生部分写失效问题
- Change Buffer 修改缓冲： 内存中 Change Buffer 对应的持久化区域
- 系统表空间不会缩容

## 独立表空间

- 独立表空间用于存放每个表的数据和索引
- `innodb_file_per_table=ON`
- 默认开启独立表空间 
- 数据库文件夹内，每个数据表单独建立一个表空间文件 表名.ibd 存储数据和索引, 同时创建一个 表名.frm 文件用于保存表结构信息
- 每个独立表空间的初始大小是 96KB

## 通用表空间

- 通用表空间存在的目的是为了在系统表空间与独立表空间之间作出平衡
- 系统表空间中的表无法直接与独立表空间中的表相互转化， 需要向通用表空间移动
- 
## 回滚表空间

- Undo TableSpace 用于存放 Undo log 文件
- 默认 Undo log 存储在系统表空间中
- 支持自定义 Undo log 表空间
- 一旦用户定义了 Undo Tablespace，则系统 表空间中的 Undo log 区域将失效
- Undo Tablespace 默认大小为 10MB

## 临时表空间

- MySQL 5.7 之前临时表存储在系统表空间中，会导致 ibdata1 在使用临时表的场景下疯狂增长
- MySQL 5.7 之后 InnoDB 引擎从系统表空间中抽离出临时表空间，用于独立保存临时表数据及其回滚信息

## Segment 段

- 表空间由各个段组成
- 段类型分为数据段、索引段、回滚段
- InnoDB 采用 聚簇索引和 B+Tree 的结构存储数据，数据页和二级索引页是 B+Tree 的叶子节点，Leaf node segment 数据段
- Non-Leaf node 索引段指的是 B+Tree 的非叶子节点
- 一个段会包含多个区，至少会有一个区，段扩展的最小单位是区

## Extent 区

- 区是由连续的页组成的空间
- 大小固定为 1MB
- 一个区默认存储64个连续的页，默认页大小为16K
- 保证页的连续性，InnoDB 存储引擎会一次从磁盘申请 4 ~ 5 个区

## Page 页

- 页是 InnoDB 的基本存储单位
- 每个页默认大小为 16KB 
- `innodb_page_size=`页大小
- InnoDB 首次加载后无法更改页大小
- 操作系统读写磁盘的最小单位是页， 4KB
- `mysql> show variables like 'innodb_page_size';` 查看MySQL页大小

## Row 行

- InnoDB 的数据是以行为单位存储
- 一个页中包含多个行
- InnoDB 提供了4种行格式：Compact，Redundant，Dynamic，Compressed
- Dynamic 为 MySQL 5.7 默认的行格式

```sql
CREATE TABLE t (
	id INT
) ROW_FORMAT=DYNAMIC;
```

- `SET GLOBAL innodb_default_row_format=DYNAMIC;` 修改全局行的默认格式
- `SHOW TABLE STATUS LIKE '表名'` 查看指定表的行格式

## 脏页

- 对于数据的修改操作，首先修改在缓冲区中的页，缓冲区中的页与磁盘中的页数据不一致，则缓冲区中的页是脏页

## 脏页进入磁盘持久化数据

- 脏页从缓冲区刷新到磁盘，不是每次页更新后触发，是通过 CheckPoint 机制进行脏页落盘
- 日志先行，所有操作前先写 Redo 日志

## 写入性能

- 在内存中写，操作过程记录日志, 通过定期批量写入磁盘的方式提高写入效率减少磁盘 IO
- 分散写变成顺序写

## 数据安全性

- 记录操作日志：Force Log at Commit 机制与 Write Ahead Log（WAL）策略
- CheckPoint 机制 
- Double Write 机制

## Force Log at Commit 机制

- 当事务提交时，所有事务产生的日志都必须刷到磁盘
- 如果日志刷新成功后，缓冲池中的数据刷新到磁盘前数据库发生了宕机，那么重启时，数据库可以从日志中恢复数据

## Write Ahead Log（WAL）策略

- 要求数据的变更写入到磁盘前，必须将内存中的日志写入到磁盘
- InnoDB 的 WAL（Write Ahead Log）技术的产物就是 redo log
- 对于写操作，永远都是日志先行，先写入 redo log 确保一致性之后，再对修改数据进行落盘

## 确保日志安全

- 为了确保日志写入到磁盘，将 redo log 写入 Log Buffer 后调用 fsync 函数，将缓冲文件从文件系统缓存中写入磁盘

## Redo日志落盘

- Redo日志默认落盘策略，事务提交立即落盘
- Log Buffer 写入磁盘的时机由参数 innodb_flush_log_at_trx_commit 控制， 此参数控制每次事务提交时 InnoDB 的行为

## innodb_flush_log_at_trx_commit 配置

- TODO

## CheckPoint 检查点机制

- CheckPoint 决定了脏页落盘（将缓冲池中的脏页数据刷到磁盘上）的时机，条件，脏页的选择，避免数据更改直接操作磁盘
- 缩短数据库的恢复时间，Checkpoint 之前的页都已经刷新回磁盘，当数据库发生宕机时，数据库不需要重做所有的日志
- 数据库只需对 Checkpoint 后的 redo log 进行恢复
- 缓冲池不够用时，根据LRU算法会溢出最近最少使用的页，如果此页为脏页，需要执行 Checkpoint，将脏页刷新回磁盘
- redo日志不可用时，刷新脏页，日志文件可以循环使用，不会无限增长
- InnoDB通过LSN（Log Sequence Number）来标记日志刷新的版本
- LSN 是 8 字节的数字

## Checkpoint 分类

- sharp checkpoint：在关闭数据库的时候，将 buffer pool 中的脏页全部刷新到磁盘中
- fuzzy checkpoint：数据库正常运行时，在不同的时机，将部分脏页写入磁盘，仅刷新部分脏页到磁盘，避免一次刷新全部的脏页造成的性能问题

## fuzzy checkpoint

- Master Thread Checkpoint ：在主线程中，会以每秒或者每10秒一次的固定频率，将部分脏页从内存中刷新到磁盘，异步不会阻塞用户线程
- FLUSH_LRU_LIST Checkpoint：缓冲池不够用时，根据LRU算法会淘汰最近最少使用的页（淘汰非热点数据），如果该页是脏页，执行 CheckPoint, 将该脏页刷新回磁盘
- Async/Sync Flush Checkpoint：一般不会发生，redo log 不可用时，强制脏页落盘
- Dirty Page too much：脏页数量太多，强制进行 Checkpoint，由参数 innodb_max_dirty_pages_pt 来控制，默认75（%）

## 写失效

- 页大小默认为 16KB，文件系统页大小为 4KB
- 数据库准备刷新脏页，将16KB的刷入磁盘，但当写入了8KB时，宕机了

## Double Write 双写

- 写两次，解决写失效问题
- redo log 不能解决写失效问题，redo log 记录的是对页的修改记录，而不是数据本身
- 在 redo log 前，对需要写入的页的做个副本，当写失效发生时，通过页的副本来还原该页再重做

## Double write 脏页刷新流程

- 复制，脏页刷新时不直接写磁盘，而是先将脏页复制到内存的 Doublewrite  buffer
- 顺序写, 内存的 Doublewrite buffer 分两次，每次 1MB 顺序写入共享表空间的物理磁盘上，立即调用 fsync 函数同步 OS缓存 到磁盘中
- 离散写, 内存的 Doublewrite buffer 最后将页写入各自表空间文件中

## Double write崩溃恢复

- 从系统表空间中的 Double write 中找到该页的一个副本
- 将其复制到独立表空间
- 应用并清空 redo log

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

- hash 索引不能快速处理范围查询
- hash 索引没有顺序，对排序数据需要重排
- hash 索引不能 like 模糊查询
- hash 冲突影响性能, 不稳定

## B+Tree 对比 B-Tree

- 叶子节点存储了所有节点的数据
- 叶子节点通过链表的方式连接

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








 
