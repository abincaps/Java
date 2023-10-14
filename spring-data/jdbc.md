
## Spring JDBC

## JdbcTemplate

- JdbcTemplate 处理资源的创建和释放, 执行核心 JDBC 工作流
- 避免忘记关闭连接
- `class JdbcTemplate extends JdbcAccessor implements JdbcOperations`

## JdbcTemplate构造方法

- `JdbcTemplate(DataSource dataSource)` 构造一个 JdbcTemplate，指定数据源以获取连接

## JdbcTemplate中的update方法

- 返回值 `int` 类型为受影响的行数
- `int update(String sql)` 发出单个 SQL 更新操作（插入, 更新, 删除)
- `int update(String sql, @Nullable Object... args)` 发出单个 SQL 语句更新操作（插入, 更新, 删除)，`args` 绑定给定的参数 （？问号占位符绑定）

## JdbcTemplate查询方法

- `Map<String,Object> queryForMap(String sql)` 单行查询, 结果行将映射到 Map (使用列名作为键)
- `List<Map<String,Object>> queryForList(String sql)` 结果将映射到映射列表（使用列名作为键），列表中的每个元素都是 `queryForMap` 的返回值
- `List<T> query(String sql, RowMapper<T> rowMapper)` 执行给定静态 SQL 语句查询，通过 RowMapper 将每一行映射到对象

## JdbcTemplate中的queryForObject方法

- `T queryForObject(String sql, RowMapper<T> rowMapper, @Nullable Object... args)` 查询给定的 SQL 语句，args 要绑定到查询的参数列表，通过 RowMapper 将单个结果行映射到结果对象
- `T queryForObject(String sql, Class<T> requiredType)` 给定静态 SQL 语句对结果对象执行查询，查询应为单行或单个查询，返回的结果将直接映射到对应的对象类型 

## RowMapper\<T>接口

- `T mapRow(ResultSet rs, int rowNum)` `ResultSet` 目标映射，`ResultSet` 不应调用 `next()`，`rowNum` 当前行号, 返回当前行的结果对象

## BeanPropertyRowMapper\<T>类

- 将行转换为指定映射目标类的新实例（将数据库查询结果映射到Java对象），并且必须具有默认构造方法（无参构造器）
- `BeanPropertyRowMapper(Class<T> mappedClass)` 在目标 Bean 中创建新的 `BeanPropertyRowMapper` ，接受未填充的属性，前提是字段是引用类型（可以表示null）

```java
JdbcTemplate jt = new JdbcTemplate(JdbcUtils.getDataSource());
List<Student> list = jt.query(sql, new BeanPropertyRowMapper<映射的类名可以不写>(映射的类名.class)); // 不需要对查询数据做变更
```

## JdbcAccessor

- 不推荐直接使用
- `void setDataSource(@Nullable DataSource dataSource)` 指定此访问器使用的数据源的数据库名称

## 如何使用JdbcTemplate？

```xml
<dependency>  
    <groupId>org.springframework</groupId>  
    <artifactId>spring-jdbc</artifactId>  
    <version>5.1.5.RELEASE</version>  
</dependency>  
  
<dependency>  
    <groupId>org.springframework</groupId>  
    <artifactId>spring-tx</artifactId>  
    <version>5.1.5.RELEASE</version>  
</dependency>  
  
<dependency>  
    <groupId>org.springframework</groupId>  
    <artifactId>spring-test</artifactId>  
    <version>5.1.5.RELEASE</version>  
</dependency>  
  
<dependency>  
    <groupId>junit</groupId>  
    <artifactId>junit</artifactId>  
    <version>4.12</version>  
</dependency>  

<!-- 数据库连接池 -->  
<dependency>  
    <groupId>com.mchange</groupId>  
    <artifactId>c3p0</artifactId>  
    <version>0.9.5.2</version>  
</dependency>  
  
<!-- https://mvnrepository.com/artifact/com.mysql/mysql-connector-j -->  
<dependency>  
    <groupId>com.mysql</groupId>  
    <artifactId>mysql-connector-j</artifactId>  
    <version>8.0.33</version>  
</dependency>
```

```java
public class JdbcTemplateTest {  
  
    private static final String DRIVER = "com.mysql.cj.jdbc.Driver";  
    private static final String URL = "jdbc:mysql://192.168.80.1:3306/demo";  
    private static final String USER = "root";  
    private static final String PSWD = "";  
  
    @Test  
    public void test() throws PropertyVetoException {  
     
        ComboPooledDataSource dataSource = new ComboPooledDataSource();  
        dataSource.setDriverClass(DRIVER);  
        dataSource.setJdbcUrl(URL);  
        dataSource.setUser(USER);  
        dataSource.setPassword(PSWD);  
  
        JdbcTemplate jdbcTemplate = new JdbcTemplate();  
        jdbcTemplate.setDataSource(dataSource);  
  
        int rows = jdbcTemplate.update("insert into user(username, password) value (?, ?)", "abincaps", "abc");  
        System.out.println("rows = " + rows);  
    }  
}
```

## Ioc依赖注入来使用JdbcTemplate

```xml
<context:property-placeholder location="classpath:jdbc.properties"/>  
  
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">  
    <property name="driverClass" value="${JDBC_DRIVER}"/>  
    <property name="jdbcUrl" value="${JDBC_URL}"/>  
    <property name="user" value="${JDBC_USER}"/>  
    <property name="password" value="${JDBC_PSWD}"/>  
</bean>  
  
<bean class="org.springframework.jdbc.core.JdbcTemplate">  
    <property name="dataSource" ref="dataSource"/>  
</bean>
```

```java
@Test  
public void test() {  
    ClassPathXmlApplicationContext app = new ClassPathXmlApplicationContext("ApplicationContext.xml");  
    JdbcTemplate jdbcTemplate = app.getBean(JdbcTemplate.class);  
    
    int rows = jdbcTemplate.update("insert into user(username, password) value (?, ?)", "abincaps", "abc");  
    System.out.println("rows = " + rows);  
}
```

## Junit使用JdbcTemplate

```java
// JUnit 在运行测试时使用 Spring 的上下文来加载测试配置
@RunWith(SpringJUnit4ClassRunner.class)  
// 指定 Spring 配置文件的位置
@ContextConfiguration("classpath:ApplicationContext.xml")  
public class JdbcTemplateTest {  
  
    @Autowired  
    private JdbcTemplate jdbcTemplate; 
     
    @Test  
    public void test1() {  
        int rows = jdbcTemplate.update("update user set username=? where username=?", "ab", "abincaps");  
        System.out.println("rows = " + rows);  
    }
    
	@Test  
	public void test2() {  
	    List<User> userList = jdbcTemplate.query("select * from user", new BeanPropertyRowMapper<>(User.class));  
	    System.out.println(userList);  
	}
	
	@Test  
	public void test3() { 
		// 查询返回必须是单行
	    Map<String, Object> user = jdbcTemplate.queryForMap("select * from user where id=?", 1);  
		user.forEach((k, v) -> System.out.println(k + ":" + v));
	}
	
	@Test  
	public void test4() {  
	    User user = jdbcTemplate.queryForObject("select * from user where id=?", new BeanPropertyRowMapper<>(User.class), 1);  
	    System.out.println(user);  
	}
	
	@Test  
	public void test5() {  
	    Integer integer = jdbcTemplate.queryForObject("select count(*) from user", Integer.class);  
	    System.out.println(integer);  
	}
}
```

