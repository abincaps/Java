
# 核心特性

## Spring Boot 介绍

- 简化 spring 的开发过程
- 开箱即用
- 支持 Tomcat 和 Jetty 嵌入式 Servlet 容器

## Starter

- Starter是一系列开箱即用的依赖
- 免去需要到处大量复制粘贴依赖的烦恼，简化组件之间的依赖关系

## Starter的命名

- 所有官方的 Starter 都遵循命名模式 `spring-boot-starter-*`，其中 `*` 是一个特定类型的应用程序

## 配置pom.xml

```xml
<parent>  
    <artifactId>spring-boot-starter-parent</artifactId>  
    <groupId>org.springframework.boot</groupId>  
    <version>x.x.x</version>  
</parent>  
  
<dependencies>  
    <dependency>        
	    <groupId>org.springframework.boot</groupId>  
        <artifactId>spring-boot-starter-web</artifactId>  
    </dependency>
</dependencies>
```

## 启动类

- 程序的入口点

```java
@SpringBootApplication  
public class MySpringBoot {  
    public static void main(String[] args) {  
        SpringApplication.run(MySpringBoot.class, args);  
    }  
}
```

## @SpringBootApplication 注解

- `@SpringBootApplication` 注解在启动类上
- `@EnableAutoConfiguration` 启用 Spring Boot 的自动配置机制
- `@ComponentScan` 对应用程序所在的包中启用 `@Component` 扫描
- `@SpringBootConfiguration` 允许在 context 中注册额外的 Bean 或导入额外的配置类, 这是 Spring 中 `@Configuration` 的替代方案

## 配置文件

- `application.properties`
- `application.yml` 
- 当同时有 properties 和 YAML 格式的配置文件时，properties 优先

## 修改HTTP端口

- HTTP端口默认为 8080
- `server.port=端口号`
- `server.port=0` 使用一个随机的未分配的HTTP端口

## 常见web配置

- `server.servlet.context-path=/前缀` 修改 URL 前缀

## 外部化的配置

- `@ConfigurationProperties` 注解于类或方法上，和外部属性（在配置文件中）进行绑定，配置属性类
- 通过`@Value`注解直接注入 Bean
- 通过 Spring 的 `Environment` 访问
- 双引号会识别转义字符，单引号不会

```yaml
# application.yaml
name: abincaps  
  
person:  
  name: abincaps  
  age: 10  
  scores:  
    - 1  
    - 2  
    - 3  
  
info0: 'web\ntest'  
info1: "web\ntest"
```

```java
@SpringBootTest  
class ApplicationTests {  
  
    @Value("${name}")  
    private String name;  
  
    @Autowired  
    private Person person;  
  
    @Value("${info0}")  
    private String info0;  
  
    @Value("${info1}")  
    private String info1;
}						
```


```java
@Component  
@ConfigurationProperties(prefix = "person")  
public class Person {  
    private String name;  
    private int age;  
    private double[] scores;
	// getter setter
	// toString
}
```

## 切换Profile

- 在 application.properties 中设置
- `spring.profiles.active=dev` 使用 `application-dev.properties` 配置，设置活动配置文件，根据不同的环境激活不同的配置

## 根据环境改变配置

```yaml
---  
server:  
  port: 8081  
  
spring:  
  config:  
    activate:  
      on-profile: dev  
  
---  
  
server:  
  port: 8082  
  
spring:  
  config:  
    activate:  
      on-profile: pro  
---  
server:  
  port: 8083  
  
spring:  
  config:  
    activate:  
      on-profile: test  
  
---  
  
spring:  
  profiles:  
    active: test
```

```java
spring.profiles.active=test  
#---  
spring.config.activate.on-profile=development  
server.port=8080  
#---  
spring.config.activate.on-profile=production  
server.port=8081  
#---  
spring.config.activate.on-profile=test  
server.port=8082
```

## 自动装配

- 自动装配机制会试图根据你所添加的依赖来自动配置你的Spring应用程序

## Spring Bean

- 在启动类添加 `@ComponentScan` 注解，扫描该注解类的包下所有应用组件（`@Component`、`@Service`、`@Repository`、`@Controller`），自动注册为 Spring Bean
- 可以直接使用 `@SpringBootApplication` 注解（该注解已经包含了 `@ComponentScan` ）
- `@ComponentScan("org.abincaps.pkg")` 简洁声明 定义要扫描的特定包

##  依赖注入

- 当Bean有多个构造函数时，需要用 `@Autowired` 注解来告诉 Spring 该用哪个构造函数进行注入

## 常见配置

- `server.port=8080` 修改HTTP端口
- `server.port=0` 使用一个随机的未分配的HTTP端口

## 懒初始化

- 当启用懒初始化时，Bean在需要时被创建，而不是在应用程序启动时，可以减少应用程序的启动时间
- 懒初始化的一个缺点是会延迟发现应用程序的问题，在启动过程中不会出现故障，问题只有在Bean被初始化时才会显现出来

## 启用懒初始化

- 调用 `SpringApplicationBuilder` 的 `lazyInitialization` 方法或 `SpringApplication` 的 `setLazyInitialization` 方法
- 配置属性 `spring.main.lazy-initialization=true` 

## 自定义 Banner

- 在 classpath 中添加 `banner.txt` 文件或通过将 `spring.banner.location` 属性设置为该文件的位置

## 自定义 Application

- `SpringApplication application = new SpringApplication(MyApplication.class);`

## ApplicationRunner 和 CommandLineRunner

- 实现 `ApplicationRunner` 或 `CommandLineRunner` 接口 在 `SpringApplication` 启动后运行一些特定的代码
- 这两个接口以相同的方式工作，并提供一个单一的 `run` 方法，该方法在 `SpringApplication.run(…​)` 执行完毕之前被调用
- 适合用于执行那些需要在处理HTTP请求之前执行的任务



