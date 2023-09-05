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
