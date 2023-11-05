
# IoC 容器

- IoC (Inversion of Control) 控制反转
- DI（Dependency Injection）依赖注入
- 实现对象之间的依赖和解耦
- IoC容器负责对象的创建、初始化等完整生命周期
- IoC容器自动装配对象, 统一了对象管理和配置
- 对象A直接引用和操作new出来对象B，变成对象A只依赖接口B，系统启动和装配阶段，把B接口的实例对象注入到对象A中， 对象A就无需依赖B接口的具体实现
- 将对象的关系转而用配置（声明式注解和xml）来管理

# Bean

- Java 对象包装在 Bean 中，生命周期由 Spring Ioc 容器管理
- Bean 是一个由 Spring IoC 容器实例化，组装，管理的对象。Bean 之间的依赖关系都反映在容器使用的配置元数据中

# interface ApplicationContext

- `ApplicationContext` 接口代表 Spring IoC 容器，负责实例化、配置和组装bean

# class FileSystemXmlApplicationContext

- Taking the context definition files from the file system or from URLs
- Plain paths will always be interpreted as relative to the current VM working directory, even if they start with a slash. (This is consistent with the semantics in a Servlet container.) Use an explicit "file:" prefix to enforce an absolute file path.

## Class ClassPathXmlApplicationContext

- Taking the context definition files from the class path, interpreting plain paths as class path resource names that include the package path
- In case of multiple config locations, later bean definitions will override ones defined in earlier loaded files.
- `ClassPathXmlApplicationContext(String configLocation)` 从给定的 XML 文件加载定义并自动刷新上下文

# class XmlWebApplicationContext

- 


## BeanFactory接口

- `Object getBean(String var1)` 返回指定 bean 的实例

## XML的配置中的`<bean/>`标签元素

- `id` , `name` 属性指定 Bean 标识符
- 不明确提供 `id` 或 `name`，容器将为该 Bean 生成一个唯一的名称
- `class` 属性中指定类的全限定名（类的全路径名）
- `scope` 控制从特定 Bean 定义创建的对象的
- `init-method` 属性来指定具有无参数签名（void) 的方法名，执行初始化回调（初始化工作）
- `destroy-method` 属性来指定无参数签名（void) 的方法名，执行销毁回调

## 导入XML配置

- `<import/>` 标签元素从另一个文件中加载 Bean 定义

```xml
<beans> 
	<import resource="services.xml"/> 
	<import resource="resources/message.xml"/> 
</beans>
```

## Bean Scope

- `singleton` 默认情况下不写 `scope` 就是 `singleton`， 单例模式，单个对象实例
- `prototype` 多例模式，任何数量的对象实例

## getBean

- `T getBean(Class<T> requiredType)`  如果 bean 对象在核心配置文件中存在多个会报错
- `Object getBean(String name)` 使用 id 来唯一确定 bean

# lazy-init

- 懒加载

## 工厂静态方法实例化Bean

- 使用 `class` 属性来指定包含 `static` 工厂方法的类的全限定名
- `factory-method` 属性设置工厂方法名

```xml
<bean id="Foo" class="com.abincaps.MyFactory" factory-method="getFoo" />
```

```java
public class MyFactory {  
    public static Foo getFoo() {  
        return new Foo();  
    }  
}
```

## 工厂实例方法实例化Bean

- 从容器中调用现有 bean 的非静态方法来创建一个新的 bean
- 新的 bean 的 `class` 属性留空
- `factory-bean` 属性中指定当前容器中的一个 Bean 的名称, 该容器包含要被调用来创建对象的实例方法

```xml
<bean id="myFactory" class="com.abincaps.MyFactory"/>  
<bean id="Foo" factory-bean="myFactory" factory-method="getStuDaoObject"/>
```

```java
public class MyFactory {  
    public Foo getFoo() {  
        return new Foo();  
    }  
}
```

## 依赖注入

- `<property/>` 标签元素中的 `name` 元素指的是 Bean 的属性名（字段名，成员变量）
- `value` 将属性或构造方法参数指定为字符串，Spring 的转换服务将字符串转换成属性或参数的实际类型
- `ref` 一个 bean 的指定属性的值设置为被容器所管理的另一个 bean（协作者）的引用 
- `id` 和 `ref` 元素之间的联系表达了依赖关系， `ref=` 另一个 bean 的 id (属性所依赖的另一个 Bean 的名称)
- `<constructor-arg/>` 标签中 `name` 元素指定构造器参数名
- `index` 属性指定构造函数参数的索引（下标），第几个参数，从 0 开始

## 基于Setter的依赖注入

```xml
<bean id="stuDao" class="com.abincaps.dao.impl.stuDaoImpl"/>  
<bean id="stuService" class="com.abincaps.service.impl.stuServiceImpl">  
	<!-- ref="stuDao" 对应 <bean id="stuDao"/>  -->
    <property name="stuDao" ref="stuDao"/>  
</bean>
```

```java
public class StuServiceImpl implements StuService {  
  
    private StuDao stuDao; // 对应 property name

	// Setter注入
    public void setStuDao(StuDao stuDao) {  
        this.stuDao = stuDao;  
    }  
  
    @Override  
    public void serviceStuShow() {  
        System.out.println("StuServiceImpl");  
    }  
}
```

```java
ClassPathXmlApplicationContext app = new ClassPathXmlApplicationContext("ApplicationContext.xml");  
Service Service = (StuService) app.getBean("stuService");  
stuService.serviceStuShow();
```

## 基于构造器注入

```xml
<bean id="stuDao" class="com.abincaps.dao.impl.StuDaoImpl"/>  
<bean id="stuService" class="com.abincaps.service.impl.StuServiceImpl">  
    <constructor-arg name="stuDao" ref="stuDao"/>  
</bean>
```

```java
public class StuServiceImpl implements StuService {  
  
    private StuDao stuDao;  

	// StuDao stuDao 中的 stuDao 对应 <constructor-arg name="stuDao"/>
    public StuServiceImpl(StuDao stuDao) {  
        this.stuDao = stuDao;  
    }  
  
    @Override  
    public void serviceStuShow() {  
        System.out.println("StuServiceImpl");  
    }  
}
```

## 集合（Collection）类型注入

- `<list/>`，`<set/>`，`<map/>` ，`<props/>` 标签元素分别设置 `Collection` 类型 `List`、`Set`、`Map` 和 `Properties` 的属性和参数

```xml
<bean id="foo" class="com.abincaps.Foo">  
    <property name="list">  
        <list>            
	        <value>abin</value>  
            <value>caps</value>  
        </list>    
    </property> 
    
    <property name="map">  
        <map>            
	        <entry key="abincaps" value-ref="stu"/>  
        </map>    
    </property>  
  
    <property name="prop">  
        <props>            
	        <prop key="abin">caps</prop>  
            <prop key="caps">abin</prop>  
        </props>    
    </property>  
</bean>  
  
<bean id = "stu" class="com.abincaps.Student">  
    <constructor-arg index="0" value="abincaps"/>  
    <constructor-arg index="1" value="10"/>  
</bean>
```

```java
public class Student {  
  
    private String name;  
    private int age;  
  
    public Student(String name, int age) {  
        this.name = name;  
        this.age = age;  
    }  
}
```

```java
public class Foo {  
  
    private List<String> list;  
    private Map<String, Student> map;  
    private Properties prop;  
  
    // 以下需要生成 setter，getter 方法  
}
```

## 核心配置文件创建数据源（Druid）

```xml
<bean id = "dataSource" class="com.alibaba.druid.pool.DruidDataSource">  
    <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>  
    <property name="url" value="jdbc:mysql://192.168.80.1:3306/demo"/>  
    <property name="username" value="root"/>  
    <property name="password" value=""/>  
</bean>
```

```java
ClassPathXmlApplicationContext app = new ClassPathXmlApplicationContext("ApplicationContext.xml");  
DataSource dataSource = (DataSource) app.getBean("dataSource");  
Connection con = dataSource.getConnection();  
con.close();
```

## context命名空间

- 用一个专门的配置元素来配置属性占位符
- 在 `location` 属性中提供 properties
- 默认情况下，如果不能在指定的 properties 文件中找到一个属性，会检查 Spring Environment properties 和 Java System properties

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" 
	   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	   xmlns:context="http://www.springframework.org/schema/context" 增加
	   xsi:schemaLocation="http://www.springframework.org/schema/beans 
	   https://www.springframework.org/schema/beans/spring-beans.xsd 
	   http://www.springframework.org/schema/context 增加
	   https://www.springframework.org/schema/context/spring-context.xsd"> 增加
	   <context:annotation-config/> 
</beans>
```

## 占位符配置 DruidDataSource

```properties
MYSQL_DRIVER=com.mysql.cj.jdbc.Driver  
MYSQL_URL=jdbc:mysql://192.168.80.1:3306/demo  
MYSQL_USER=root  
MYSQL_PASSWORD=
```

```xml
<!--  需要增加context命名空间  -->
<context:property-placeholder location="classpath:jdbc.properties"/>  
  
<bean id = "dataSource" class="com.alibaba.druid.pool.DruidDataSource">  
    <property name="driverClassName" value="${MYSQL_DRIVER}"/>  
    <property name="url" value="${MYSQL_URL}"/>  
    <property name="username" value="${MYSQL_USER}"/>  
    <property name="password" value="${MYSQL_PASSWORD}"/>  
</bean>
```

## 基于注解的容器配置

- `@Component` 是一个通用的，适用于任何 Spring 管理的组件
- `@Repository`  DAO层（持久层），`@Service` Service层（服务层），`@Controller`（表现层） 是 `@Component` 的特殊化
- `@Resource` 在字段或 setter 方法且方法只有一个参数进行注入，需要一个 `name` 属性，默认情况下，该值解释为要注入的 Bean 名称
- `@Value` 注入外部化 properties
- `@Scope` 作用范围

## XML方案自动检测类

- `<context:annotation-config/>` 标签元素隐含注册
- `<context:component-scan base-package="com.abincaps">` 隐含地实现了 `<context:annotation-config>` 的功能
- 在使用 `<context:component-scan>` 时，通常不需要包括 `<context:annotation-config>` 元素

## @Component

- `value` 指定组件的名称

```java
package com.abincaps.dao;

@Component("dao") 
// 或 @Repository("dao")
public class Dao {
}

// 上面注解等同于下面xml配置文件
<bean id="dao" class="com.abincaps.dao.Dao">
```

## @Autowired和@Qualifier

- `@Autowired` 注解自动装配依赖注入
- `@Autowired` 注解应用于
	- 构造方法
	- setter 方法
	- 任意名称和多个参数的方法
	- 字段
	
- `@Qualifier` 将限定符的值与特定的参数联系起来，缩小类型匹配的范围，提供细粒度的控制，和`@Autowired`一起使用，用于存在多个相同类型的 Bean 时，指定要注入的具体 Bean

```java
package com.abincaps.service;

@Component("service") 
// 或 @Service("service")
public class Service {  
  
    @Autowired  
    @Qualifier("dao") 
    private Dao dao;
}

// 上面注解等同于下面xml配置文件
<bean id="service" class="com.abincaps.service.Service">  
	<property name="dao" ref="dao"/> 
</bean>
```

## @Resource

- `@Resource` 在 `jakarta.annotation.Resource` 包下, 低版本在 `javax.annotation.Resource` 包下
- @Resource = @Autowired + @Qualifier

```java
package com.abincaps.service.impl;

@Component("service") 
// 或 @Service("service")
public class ServiceImpl implements ServiceInter {  
  
    @Resource(name = "stuDao") // 依赖注入，在容器中查找名为 "stuDao" 的 Bean，将它注入到 daoInter 字段中.
    private DaoInter daoInter;
}
```

## @Value

```java
@Repository("dao")  
public class Dao {  
    @Value("abincaps")  
    private String name;  
    @Value("10")  
    private int age;
}
```

## @Value注入外部化properties

```xml
<!-- 指定 properties 路径 -->  
<context:property-placeholder location="classpath:jdbc.properties"/>  
<!-- 自动扫描和注册组件，指定了需要扫描的包名 -->  
<context:component-scan base-package="com.abincaps"/>
```

```java
@Component("myDruidDataSource")  
public class MyDruidDataSource {  
  
    @Value("${MYSQL_DRIVER}")  
    private String driver;  
  
    @Value("${MYSQL_URL}")  
    private String url;  
  
    @Value("${MYSQL_USER}")  
    private String username;  
  
    @Value("${MYSQL_PASSWORD}")  
    private String password;  
}
```

## @Scope

- `@Scope` 注解指定 Spring 容器如何管理和创建 Bean 实例
- 自动检测的组件的默认的scope是 `singleton`
- `@Scope("prototype")` 每次从容器中请求该 Bean 时，容器都会创建一个新的实例并返回

## @PropertySource

- `@PropertySource` 注解向 Spring 的 Environment 中添加 PropertySource 

```java
@PropertySource("classpath:jdbc.properties")

// 上面注解替代下面 XML配置文件
<context:property-placeholder location="classpath:jdbc.properties"/>
```

## @Bean

- `@Bean` 注解表示一个方法实例化、配置和初始化了一个新的对象，由 Spring IoC 容器管理

## @ComponentScan

- `@ComponentScan` 注解自动检测组件并注册相应的 Bean (配置注解的扫描路径)

```java
@ComponentScan("com.abincaps")  
public class SpringConfiguration {  
}

// 上面注解替代下面 XML配置文件
<context:component-scan base-package="com.abincaps"/>
```

## @Import

- `@Import`注解允许从另一个配置类中加载 `@Bean` 定义

## @Configuration

- `@Configuration` 注解一个类是核心配置文件类，表明该类是作为 Bean 定义的来源

```java
@PropertySource("classpath:jdbc.properties")  
public class DataSourceConfiguration {  
  
    @Value("${MYSQL_DRIVER}")  
    private String driver;  
  
    @Value("${MYSQL_URL}")  
    private String url;  
  
    @Value("${MYSQL_USER}")  
    private String username;  
  
    @Value("${MYSQL_PASSWORD}")  
    private String password;  
  
    // getDataSource 方法创建，配置并返回了 DataSource 这个 Bean    
    @Bean("dataSource")  
    public DataSource getDataSource() {  
  
        DruidDataSource dataSource = new DruidDataSource();  
        dataSource.setDriverClassName(driver);  
        dataSource.setUrl(url);  
        dataSource.setUsername(username);  
        dataSource.setPassword(password);  
  
        return dataSource;  
    }  
}
```


```java
@Configuration  // 配置类
@ComponentScan("com.abincaps")  // 启用组件扫描, 查找指定包及其子包中的组件类，将它们自动注册为 Spring Bean
@Import(DataSourceConfiguration.class)  // 导入其他配置类
public class SpringConfiguration {  
}
```

```java
AnnotationConfigApplicationContext app = new AnnotationConfigApplicationContext(SpringConfiguration.class);  
DataSource dataSource = (DataSource) app.getBean("dataSource");  
Connection con = dataSource.getConnection();  
con.close();
```

## PropertyResolverJ接口

- 属性解析器

## PropertyResolver接口常用方法

- `String getProperty(String key)` 返回和指定键关联的属性值，如果无法解析键返回 `null` 

```java
@SpringBootTest  
class ApplicationTests {  
  
    @Autowired  
    private Environment env;  
  
    @Test  
    void contextLoads() {  
  
        System.out.println(env.getProperty("name"));  
        System.out.println(env.getProperty("person.name"));  
    }  
}
```


## IOC 流程

## getBean流程

- XML 配置文件一次性加载， 通过配置文件的地址以流的方式读取到内存
- XML 配置文件解析， 进行语义处理， 按照bean标签为单位解析，通过集合统一存储bean标签数据
- bean标签对应一个存储类 BeanDefinition 进行单独存储
- 使用 PropertyValue 表示 bean 标签中的一个 property 标签
- TypedStringValue 基本类型的值
- RuntimeBeanReference 引用类型的值
- 通过反射且使用单例模式创建对象，使用 map 集合来实现单例管理
- `Class clazz = Class.forName(类的全限定名);`
- `Object bean = clazz.newInstance;`
- 遍历设置属性
- `Field field = clazz.getField(属性名);`
- `field.set(bean, 属性值);`

# interface BeanFactory

- `BeanFactory` 为 Spring IoC 功能提供了底层基础
- `String FACTORY_BEAN_PREFIX = "&";` Used to dereference a FactoryBean instance and distinguish it from beans created by the FactoryBean.
- `Object getBean(String name)` 
- `boolean isSingleton(String name)` Is this bean a shared singleton? That is, will getBean(java.lang.String) always return the same instance?

```Java
public class UserFactoryBean implements FactoryBean {  
  
    @Override  
    public Object getObject() throws Exception {  
        return new User();  
    }  
  
    @Override  
    public Class<?> getObjectType() {  
        return User.class;  
    }  
}
```

```Java
public class FactoryBeanTest {  

    @Test  
    public void test() {  
  
        String xmlPath = "src/main/resources/ApplicationContext.xml";  
        ApplicationContext applicationContext = new FileSystemXmlApplicationContext(xmlPath);  
  
        User user = (User)applicationContext.getBean("userFactoryBean");  
        UserFactoryBean userFactoryBean = (UserFactoryBean)applicationContext.getBean("&userFactoryBean");  
  
        System.out.println("user = " + user);  
        System.out.println("userFactoryBean = " + userFactoryBean);  
    }  
}
```

# 容器初始化

- 配置文件 读取 ——>  Resource 解析 ——> BeanDefinition 注册 ——> 容器

# interface Environment

- Environment 接口是一个集成在容器中的抽象，对配置文件（profiles）和属性（properties）进行建模
- 用于配置属性源并解析属性
- `interface Environment extends PropertyResolver`
