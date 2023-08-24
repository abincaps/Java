# 核心特性

## IoC 容器 

- IoC(Inversion of Control),即控制反转
- 通过依赖注入 DI(Dependency Injection)实现对象之间的依赖和解耦
- IoC容器负责对象的创建、初始化等完整生命周期
- IoC容器自动装配对象, 统一了对象管理和配置

## 组件

- `@Repository` （持久层） `@Service` （服务层） 和 `@Controller` （表现层） 是 `@Component` 的特殊化

### Bean

> Bean是一个由Spring IoC容器实例化、组装和管理的对象
>
> Bean以及它们之间的依赖关系都反映在容器使用的配置元数据中

## DAO 层

> DAO(Data Access Object) 层是访问数据持久层的接口
>
> 包含各种数据库操作,如CRUD

## Service层

- Service层包含应用核心的业务逻辑处理，对业务逻辑进行封装
- 可以通过接口调用DAO层进行数据库操作

## Spring Framework版本

- Spring Framework 5.x 需要  java SE 8+ / java EE 7+


## AOP

- AOP(Aspect Oriented Programming) 面向切面的编程 
- OOP中模块化的单位是类，而AOP中模块化的单位是切面


## 切点指定器

- `execution`: 用于匹配方法执行的连接点
- `within`: 将匹配限制在某些类型内的连接点（使用Spring AOP时，执行在匹配类型内声明的方法）。
- `this`: 将匹配限制在连接点（使用Spring AOP时方法的执行），其中bean引用（Spring AOP代理）是给定类型的实例。 
- `target`: 将匹配限制在连接点（使用Spring AOP时方法的执行），其中目标对象（被代理的应用程序对象）是给定类型的实例。
- `args`: 将匹配限制在连接点（使用Spring AOP时方法的执行），其中参数是给定类型的实例。 
- `@target`: 限制匹配到连接点（使用Spring AOP时方法的执行），其中执行对象的类有一个给定类型的注解。  
- `@args`: 将匹配限制在连接点（使用Spring AOP时方法的执行），其中实际传递的参数的运行时类型有给定类型的注解。
- `@within`: 将匹配限制在具有给定注解的类型中的连接点（使用Spring AOP时，执行在具有给定注解的类型中声明的方法）。
- `@annotation`: 将匹配限制在连接点的主体（Spring AOP中正在运行的方法）具有给定注解的连接点上。

## 切点（Pointcut）

- `@Pointcut` 注解
- 
- `*` 通配符
- ..
- `()` 匹配一个不需要参数的方法
- `(..)` 匹配任何数量（零或更多）的参数
- `(*)` 模式匹配一个需要任何类型的参数的方法
- `(*,String)` 匹配一个需要两个参数的方法，第一个参数可以是任何类型，而第二个参数必须是一个 `String`

## @Before

- 在切点匹配的方法执行之前

## @Around

- Around advice "围绕" 一个匹配的方法的执行而运行，它有机会在方法运行之前和之后进行工作，并决定何时、如何、甚至是否真正运行该方法
- 该方法应该声明 `Object` 为其返回类型，并且该方法的第一个参数必须是 `ProceedingJoinPoint` 类型

## 执行顺序

1. @Around 调用proceed之前
2. @Before
3. 方法执行
4. @After
5. @Around 调用proceed之后

## ProceedingJoinPoint


## JoinPoint

- 公共接口连接点，提供对连接点可用状态及其静态信息的反射访问
- 任何 advice method 都可以声明一个 `JoinPoint` 类型的参数作为其第一个参数

### 常见方法

- `getArgs()` 返回方法的参数
- `getThis()` 返回代理对象
- `getTarget()` 返回目标对象
- `getSignature()` 返回正在被 advice 的方法的签名
- `toString()` 打印对所 advice 的方法的有用描述


