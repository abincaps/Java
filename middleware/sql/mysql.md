
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

## 端口

- MySQL数据库默认端口 `3306` 

## 常用命令

- `mysql -u username -p password` 或 `mysql -u username -p` 登录
- `mysql -h host -u username -p password`  指定 host 登录
- `systemctl start mysql` 启动mysql服务
- `systemctl enable mysql` 设置mysql自启动
- `systemctl status mysqld` 查看mysql状态
- `systemctl restart mysql` 重启mysql服务
- `systemctl restart mysql` 停止mysql服务

## 设置相关配置

- `/etc/mysql/my.cnf` 

## MySQL存储架构

- server 层
- 存储引擎层
- 客户端 -> 连接模块 -> 词法解析 -> 语法解析 -> 预处理器 -> 查询优化器 -> 执行引擎
- 连接模块 -> 缓存模块

## 存储引擎

- 在磁盘或者内存中存储的组织方式
- `SHOW TABLE STATUS FROM demo`  查看 demo 数据库中的表的状态
- `SHOW CREATE TABLE demo` 查看创建 demo 表的 SQL 语句 
- `SHOW VARIABLES LIKE 'default_storage_engine'` 查看默认存储引擎
- `SHOW VARIABLES LIKE 'datadir'` 查看数据存储的目录
- InnoDB 中的表定义和表数据存在 idb 文件中
- MyISAM 中的表定义存在 sdi 文件中
- `ibd2sdi demo.ibd` 查看 idb 文件，以 json 格式显示、

## 存储引擎的区别

|                | InnoDB               | MyISAM             |
| -------------- | -------------------- | ------------------ |
| 是否支持事务   | 是                   | 否                 |
| 锁的粒度       | 表锁，行锁           | 表锁               |
| 是否支持外键   | 是                   | 否                 |
| 数据的存储方式 | 索引和数据存放在一起 | 索引和数据分开存放 |
| 数据存储的结构 | B+Tree               | B+Tree             |

## 索引的分类

- 主键索引 每个表有且只有一个主键索引， 如果没有指定主键索引，对于表会默认生成一个非空且唯一的主键索引
- 唯一索引 可以为空，但不能重复，一个表可以有多个唯一索引
- 普通索引 可以为空，可以重复

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

## 为什么不选择 Hash 算法？

- hash索引不能快速处理范围查询
- hash索引没有顺序，对排序数据需要重排
- hash索引不能 like 模糊查询
- hash 冲突 影响性能 不稳定

## B+Tree 对比 B-Tree

- 叶子节点存储了所有节点的数据
- 叶子节点通过链表的方式连接

## DDL常用命令

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

## 其他命令

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

## 纵向分表







 
