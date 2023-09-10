
## Spring JDBC

## JdbcTemplate

- JdbcTemplate 处理资源的创建和释放
- 避免忘记关闭连接

## JdbcTemplate构造方法

- `JdbcTemplate(DataSource dataSource)` 构造一个 JdbcTemplate，给定一个数据源以获取连接

## JdbcTemplate更新方法

- `int update(String sql)` 发出单个 SQL 更新操作（插入、更新或删除语句, 返回受影响的行数
- `int update(String sql, @Nullable PreparedStatementSetter pss)` 使用给定的 SQL 语句设置绑定参数（？占位符绑定）

## JdbcTemplate查询方法

- `Map<String,Object> queryForMap(String sql)` 单行查询, 结果行将映射到 Map (使用列名作为键)
- `List<Map<String,Object>> queryForList(String sql)` 结果将映射到映射列表（使用列名作为键），列表中的每个元素都是 `queryForMap` 的返回值
- `List<T> query(String sql, RowMapper<T> rowMapper)` 执行给定静态 SQL 语句查询，通过 RowMapper 将每一行映射到对象
- `T queryForObject(String sql, Class<T> requiredType)` 给定静态 SQL 语句对结果对象执行查询，查询应为单行/单列查询，返回的结果将直接映射到对应的对象类型

## RowMapper\<T>接口

- `T mapRow(ResultSet rs, int rowNum)` `ResultSet` 目标映射，`ResultSet` 不应调用 `next()`，`rowNum` 当前行号, 返回当前行的结果对象

## BeanPropertyRowMapper\<T>类

- 将行转换为指定映射目标类的新实例，映射的目标类必须是顶级类或嵌套类，并且必须具有默认构造方法（无参构造器）
- `BeanPropertyRowMapper(Class<T> mappedClass)` 在目标 Bean 中创建新的 `BeanPropertyRowMapper` ，接受未填充的属性，字段是引用类型

```java
JdbcTemplate jt = new JdbcTemplate(JdbcUtils.getDataSource());
List<Student> list = jt.query(sql, new BeanPropertyRowMapper<映射的类名>(映射的类名.class)); // 不需要对查询数据做变更
```



