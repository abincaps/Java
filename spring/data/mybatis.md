
# MyBatis

- 持久层框架，ORM 框架

## 官方文档

- [https://mybatis.org/mybatis-3/zh/index.html](https://mybatis.org/mybatis-3/zh/index.html)

## MyBatis 系统的核心设置

- `default="development"` 默认使用的环境 ID
- `<properties/>` 属性可以在外部进行配置

```java
driver = com.mysql.cj.jdbc.Driver  
url = jdbc:mysql://192.168.80.1:3306/demo  
username = root  
password =
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE configuration  
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"  
  "https://mybatis.org/dtd/mybatis-3-config.dtd">  
<configuration> 

  <!--  引入外部配置文件-->  
  <properties resource="jdbc.properties"/>

  <environments default="development">  
    <environment id="development">  
      <transactionManager type="JDBC"/>  
      <dataSource type="POOLED">  
        <property name="driver" value="${driver}"/>  
        <property name="url" value="${url}"/>  
        <property name="username" value="${username}"/>  
        <property name="password" value="${password}"/>  
      </dataSource>   
    </environment>  
  </environments>  
  <mappers>    
	<mapper resource="com/abincaps/mapper/UserMapper.xml"/> 
  </mappers>
</configuration>
```

## 映射器

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"  
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">  
<mapper namespace="userMapper">  
    <select id="selAll" resultType="com.abincaps.domain.User">  
        select * from user  
    </select>  
  
    <insert id="add" parameterType="com.abincaps.domain.User">  
        insert into user (username, password, status) values (#{username}, #{password}, #{status})  
    </insert>  
  
    <update id="update" parameterType="com.abincaps.domain.User">  
        update user set password=#{password}, status=#{status} where username=#{username}  
    </update>  
  
	<delete id="deleteByUsername" parameterType="String">  
	    delete from user where username=#{username}  
	</delete>
  
</mapper>
```

## SqlSession接口

- 执行命令
- 获取映射器
- 管理事务

## SqlSession接口常用方法

- `List<E> selectList(String statement)` 查询并返回多行结果
- `int insert(String statement)` 执行插入语句, 返回受影响的行数
- `int insert(String statement, Object parameter)` parameter 传递给插入语句的参数值
- `int update(String statement)` 执行更新语句，返回受影响的行数
- `int delete(String statement)` 执行删除语句。返回受影响的行数

```java
public class MyBatisTest {  
  
    private SqlSession sqlSession;  
  
    @Before  
    public void before() throws IOException {  
        InputStream resourceAsStream = Resources.getResourceAsStream("SqlMapConfig.xml");  
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);  
        sqlSession = sqlSessionFactory.openSession();  
    }  
  
    @After  
    public void after() {  
  
        sqlSession.commit();  
        sqlSession.close();  
    }  
  
    @Test  
    public void selectList() throws IOException {  
        List<User> userList = sqlSession.selectList("userMapper.selAll");  
        for (User user : userList) {  
            System.out.println(user);  
        }  
    }  
  
    @Test  
    public void insert() throws IOException {  
        User user = new User("abincaps", "abincaps","2");  
        int rows = sqlSession.insert("userMapper.add", user);  
  
        System.out.println(rows);  
    }  
  
    @Test  
    public void update() {  
        User user = new User("abincaps", "root","2");  
        int rows = sqlSession.update("userMapper.update", user);  
  
        System.out.println(rows);  
    }  
  
	@Test  
	public void delete() {  
	    int rows = sqlSession.delete("userMapper.deleteByUsername", "abincaps");  
	  
	    System.out.println(rows);  
	}  
}
```

## typeAliases 类型别名

```xml
<typeAliases>  
  <typeAlias alias="User" type="com.abincaps.domain.User"/>  
</typeAliases>
```

## mappers 映射器

- 映射文件的路径
- SQL 映射

```xml
<!-- 使用相对于类路径的资源引用 -->
<mappers>  
  <mapper resource="com/abincaps/mapper/UserMapperInterface.xml"/>  
</mappers>
```

```xml
<mappers>  
	<!-- 将包内的映射器接口实现全部注册为映射器, 面向注解CRUD -->
  <package name="com.abincaps.dao"/>  
</mappers>
```

## mapper接口

- `namespace` 和 接口的全限定名相同
- 接口方法名 和` id` 相同
- 入参类型 和 `parameterType` 相同
- 出参类型 和 `resultType` 相同

```java
package com.abincaps.dao;
  
public interface UserMapper {  
  
    List<User> selAll();  
    User selByUsername();  
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"  
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">  
<mapper namespace="com.abincaps.dao.UserMapper">  
    <select id="selAll" resultType="User">  
        select * from user  
    </select>  
  
    <select id="selByUsername" resultType="User" parameterType="String" >  
        select * from user where username=#{username}  
    </select>  
  
</mapper>
```

```java
@Test  
public void selAll() {  
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
    List<User> userList = mapper.selAll();  
  
    for (User user : userList) {  
        System.out.println(user);  
    }  
}

@Test  
public void selByUsername() {  
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
    User user = mapper.selByUsername("abincaps");  
  
    System.out.println(user);  
}
```

## 动态 SQL

- if
- foreach
- where 根据不同的条件选择性地添加 WHERE 子句的一部分

```xml
<!-- 不定条件查询-->
<select id="selBySome" resultType="User" parameterType="User">  
    select * from user  
    <where>  
        <if test="id != 0">  
            and id = #{id}  
        </if>  
  
        <if test="username != null">  
            and username = #{username}  
        </if>  
  
        <if test="password != null">  
            and password = #{password}  
        </if>  
  
        <if test="status != null">  
            and status = #{status}  
        </if>  
    </where>
</select>

<select id="selByUsernames" parameterType="list" resultType="User">  
    select * from user  
    <where>  
        <foreach collection="list" item="username" open="username in(" close=")" separator=",">  
            #{username}  
        </foreach>  
    </where>  
</select>
```

```java
@Test  
public void selBySome() {  
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
    User user = mapper.selBySome(new User(10, null, null, null));  
    System.out.println(user);  
}

@Test  
public void selByUsernames() {  
  
    ArrayList<String> nameList = new ArrayList<>();  
    nameList.add("abincaps");  
    nameList.add("abcc");  
  
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
    List<User> userList = mapper.selByUsernames(nameList);  
    for (User user : userList) {  
        System.out.println(user);  
    }  
}
```

## resultMap

- 处理多表的结果映射
- `<id/>` 标记出作为 ID 的结果可以帮助提高整体性能
- `<result/>` 注入到字段或 JavaBean 属性的普通结果
- `<association/>` 复杂类型的关联
- `<collection/>` 复杂类型的集合

## XML一对一映射

```sql
drop table if exists `student`;  
create table `student` (  
    `stu_id` int(10) unsigned not null  auto_increment,  
    `stu_name` varchar(32) not null,  
    `stu_sex` enum('0', '1', '2') default '2',  
    `stu_age` tinyint(3) unsigned default '10',  
    `lesson_id` int(10) unsigned not null,  
    primary key (`stu_id`)  
);  
  
drop table if exists `lesson`;  
create table `lesson` (  
    `lesson_id` int(10) unsigned not null auto_increment,  
    `lesson_name` varchar(32) not null,  
    `lesson_teacher` varchar(32) not null,  
    primary key (`lesson_id`)  
);
```

```java
package com.abincaps.dao;  
  
public interface StudentMapper {  
    List<Student> selStuLess();  
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"  
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">  
<mapper namespace="com.abincaps.dao.StudentMapper">  
  
    <resultMap id="stuLesson" type="com.abincaps.domain.Student"> 
        <id column="stu_id" property="stu_id"/>  
        <result column="stu_name" property="stu_name"/>  
        <result column="stu_sex" property="stu_sex"/>  
        <result column="stu_age" property="stu_age"/>  
	    <!-- 将查询结果的 lesson_id 列映射到 Student 对象的 lesson 属性的 lesson_id 子属性上 -->
        <result column="lesson_id" property="lesson.lesson_id"/>  
        <result column="lesson_name" property="lesson.lesson_name"/>  
        <result column="lesson_teacher" property="lesson.lesson_teacher"/> 
		<!-- 上三行等于下面 <association/> 块 -->
		<association property="lesson" javaType="com.abincaps.domain.Lesson">  
		    <id column="lesson_id" property="lesson_id"/>  
		    <result column="lesson_name" property="lesson_name"/>  
		    <result column="lesson_teacher" property="lesson_teacher"/>  
		</association> 
    </resultMap>  
    <select id="selStuLess" resultMap="stuLesson">  
        select * from student, lesson where student.lesson_id = lesson.lesson_id  
    </select>  
</mapper>
```

```java
@Test  
public void test() {  
    StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);  
    List<Student> studentList = mapper.selStuLess();  
  
    for (Student student : studentList) {  
        System.out.println(student);  
    }  
}
```

## XML一对多映射

```java
package com.abincaps.dao;  
  
public interface LessonMapper {  
    List<Lesson> selStuAll();  
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"  
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">  
<mapper namespace="com.abincaps.dao.LessonMapper">  

    <resultMap id="LessonMap" type="com.abincaps.domain.Lesson">  
        <id column="lesson_id" property="lesson_id"/>  
        <result column="lesson_name" property="lesson_name"/>  
        <result column="lesson_teacher" property="lesson_teacher"/>  
        <collection property="stuList" ofType="com.abincaps.domain.Student">  
            <id column="stu_id" property="stu_id"/>  
            <result column="stu_name" property="stu_name"/>  
            <result column="stu_sex" property="stu_sex"/>  
            <result column="stu_age" property="stu_age"/>  
        </collection>  
    </resultMap> 
    
    <select id="selStuAll" resultMap="LessonMap">  
        select * from lesson, student where student.lesson_id = lesson.lesson_id  
    </select>  
</mapper>
```

```java
@Test  
public void selStuAll() {  
    LessonMapper mapper = sqlSession.getMapper(LessonMapper.class);  
    List<Lesson> lessons = mapper.selStuAll();  
  
    for (Lesson lesson : lessons) {  
        System.out.println(lesson);  
    }  
}
```

## 接口

- `#{fieldname}` 占位符中的名称需要和 Bean 的属性名一致（字段名）
- 默认根据表的列名和 Bean 属性名的一致性来自动映射结果，无需额外的配置

```xml
<mappers>  
	<!-- 将包内的映射器接口实现全部注册为映射器, 面向注解CRUD -->
  <package name="com.abincaps.dao"/>  
</mappers>
```

```java
package com.abincaps.dao;  
  
public interface UserMapper {  
    @Select("select * from user")  
    List<User> selAll();  
  
    @Select("select * from user where username=#{username}")  
    User selByUsername(String username);  
  
    @Insert("insert into user (username, password, status) values (#{username}, #{password}, #{status})")  
    int add(User user                                                                                                                               );  
  
    @Update("update user set password=#{password}, status=#{status} where username=#{username}")  
    int update(User user);  
  
    @Delete("delete from user where username=#{username}")  
    int deleteByUsername(String username);  
}
```

## @Result

- `@Result` 注解，指定查询结果的映射规则
- `id` 是否为主键字段
- `column`  表的列名
- `property` JavaBean 属性名
- `javaType` JavaBean 类型，用于复杂类型的映射

## @One

- 一对一关联关系
- `select` 属性用于指定如何加载关联的对象

```java
package com.abincaps.dao;  
  
public interface LessonMapper {  
  
    @Select("select * from lesson where lesson_id = #{lesson_id}")  
    Lesson selById(int lesson_id);  
}
```

```java
package com.abincaps.dao;

public interface StudentMapper {  
    @Select("select * from student")  
    @Results(value = {  
            @Result(column = "stu_id", property = "stu_id"),  
            @Result(column = "stu_name", property = "stu_name"),  
            @Result(column = "stu_sex", property = "stu_sex"),  
            @Result(column = "stu_age", property = "stu_age"),  
            @Result(  
                    column = "lesson_id",  
                    property = "lesson",  
                    javaType = Lesson.class,  
                    one = @One(select = "com.abincaps.dao.LessonMapper.selById")  
            )  
    })  
    List<Student> selStuLess();  
}
```

##  @Many

- 一对多关联关系

```java
public class Lesson {  
    private int lesson_id;  
    private String lesson_name;  
    private String lesson_teacher;  
    private List<Student> stuList;
}
```

```java
package com.abincaps.dao;  
  
public interface LessonMapper {  
  
    @Select("select * from lesson")  
    @Results({  
            @Result(column = "lesson_id", property = "lesson_id"),  
            @Result(column = "lesson_name", property = "lesson_name"),  
            @Result(column = "lesson_teacher", property = "lesson_teacher"),  
            @Result(  
                    column = "lesson_id",  
                    property = "stuList",  
                    javaType =  List.class,  
                    many = @Many(select = "com.abincaps.dao.StudentMapper.selByStuId")  
            )  
    })  
    List<Lesson> selStuAll();  
}
```

```java
package com.abincaps.dao;  
  
public interface StudentMapper {  

    @Select("select * from student where lesson_id=#{lesson_id}")  
    List<Student> selByStuId(int lesson_id);  
}
```

## 数据库配置

- Spring Boot 可以从 URL 中推断出大多数数据库的 JDBC driver 类
- `spring.datasource.driver-class-name` 指定 JDBC driver 类

```yaml
spring:  
  datasource:  
    url: jdbc:mysql://192.168.80.1:3306/demo  
    driver-class-name: com.mysql.cj.jdbc.Driver  
    username: root  
    password:
```

```java
spring.datasource.url=jdbc:mysql://192.168.80.1:3306/demo
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.username=root
spring.datasource.password=
```

## @Mapper

- `@Mapper`注解在接口上
- 根据接口定义自动生成实现类（不需要自己编写实现类），这个接口内的方法不能重载
- 默认搜寻带有`@Mapper`注解的 mapper 接口

```java
package org.abincaps.mapper;  
  
@Mapper  
public interface StuMapper {  
  
    @Select("select * from student")  
    List<Student> findAll();  
}
```

```java
@SpringBootTest  
class ApplicationTests {  

    @Autowired  
    private StuMapper stuMapper;  
  
    @Test  
    void findAll() {  
        for (Student student : stuMapper.findAll()) {  
            System.out.println(student);  
        }  
    }  
}
```

## @MapperScan

- `@MapperScan`注解对类路径进行扫描来发现映射器

## XML映射

- `mapper-locations` 指定 Mapper 接口的 XML映射文件所在的位置
- `type-aliases-package` 指定了类型别名的基础包路径, 会自动扫描这个包路径下的类，并注册为类型别名，在 XML 映射文件中引用这些类型时不需要使用完全限定名

```java
package org.abincaps.mapper;

@Mapper  
@Repository  
public interface StuXmlMapper {  
    List<Student> findAll();  
}
```

```yaml
mybatis:  
  mapper-locations: classpath:mapper/*Mapper.xml  
  type-aliases-package: com.abincaps.domain
```

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"  
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">  
<mapper namespace="org.abincaps.mapper.StuXmlMapper">  
    <select id="findAll" resultType="Student">  
        select * from student  
    </select>  
</mapper>
```

## 常见配置

- `mybatis.configuration.map-underscore-to-camel-case=true` 将结果集中的下划线命名方式自动映射到Java对象的驼峰命名方式
- `mybatis.type-handlers-package=TypeHandler扫描包名` 搜索类型处理器的包名
- `mybatis.mapper-locations=classpath:/mapper/**/*.xml` XML 映射文件的路径

## @Options

- `useGeneratedKeys` 是否使用自增主键
- `keyProperty` 自增主键对应的属性
- `@MapperScan`  对类路径进行扫描