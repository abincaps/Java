
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
- `show create table table_name;` 查询创建指定表的SQL语句
- `DROP TABLE IF EXISTS table_name`; 判断表是否存在再删除
- `RENAME TABLE test TO new_test;`  修改表名
- `ALTER TABLE test ADD tname varchar(10);` 向表添加字段
- `ALTER TABLE test modify tname varchar(10);` 修改表中字段的类型
- `ALTER TABLE test change tname new_name varchar(10);` 修改表中字段名以及类型

## DML常用命令

- `INSERT INTO student(sid, sname, age) VALUES(1, "name", 20);` 向指定表插入指定字段数据
- `INSERT INTO student VALUES(1, "name", 20);` 向指定表插入全部字段数据
- `UPDATE student SET sname = "sname" WHERE sid = 1;`  条件更新指定的字段的值
- `UPDATE student SET sname = "sname", age = 10 WHERE sid = 1;` 指定字段值的条件来更新指定的多个字段的值
- `TRUNCATE TABLE table_name;` 删除整张表再创建新的空表

## DQL查用命令

- `SELECT * FROM table_name;` 查询表中所有记录
- `SELECT sid, sname FROM table_name;` 查询指定字段的所有记录
- `SELECT sid AS 编号 FROM student;` 查询结果的列名使用别名
- `SELECT DISTINCT sname FROM student;` 去重

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

 
