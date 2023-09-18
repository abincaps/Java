
# Spring AOP

## Spring AOP

- AOP (Aspect Oriented Programming) 面向切面的编程 
- OOP 中模块化的单位是类，而 AOP 中模块化的单位是切面
- Spring 增加中间层来实现所有对象的托管，管理对象生命周期和对象装配

## AOP底层代理方式

- 接口类型 JDK Proxy 动态代理
- 非接口类型 CGlib 生成子类

## pom.xml添加AspectJ依赖

```xml
<dependency>  
    <groupId>org.aspectj</groupId>  
    <artifactId>aspectjweaver</artifactId>  
    <version>x.x.x</version>  
</dependency>
```

## aop Schema

- `aop` 标签用于配置 Spring 中所有的 AOP
- `aop` 命名空间

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xmlns:aop="http://www.springframework.org/schema/aop"  
       xsi:schemaLocation="  
       http://www.springframework.org/schema/beans       
       https://www.springframework.org/schema/beans/spring-beans.xsd       
       http://www.springframework.org/schema/aop       
       https://www.springframework.org/schema/aop/spring-aop.xsd">  
</beans>
```

## AspectJ的Pointcut(切点)指定器

- `execution` 匹配方法执行的连接点, 确定在哪些 method（方法）上应用 Aspect（切面）的 advice (通知)

## execution

- execution(modifiers-pattern? ret-type-pattern declaring-type-pattern? name-pattern(param-pattern) throws-pattern?)
- execution(权限修饰符（可选） 返回值类型  类的全限定名（可选） 方法名  方法参数  抛出的异常类型（可选）)
- 除了返回值类型，方法名，方法参数之外的所有部分都是可选的
- `*` 通配符匹配任何 返回值类型，类名，方法名，参数类型
- `()` 匹配一个无参数的方法
- `(*)` 模式匹配一个需要任何类型的参数的方法
- `(..)` 匹配任何数量的参数
- `.` 匹配单个层级，当前包下
- `..` 匹配多个层级，当前包以及子包下

## Before Advice（前置通知）

- `<aop:before/>` 在一个匹配的方法执行之前运行

```xml
<bean id="target" class="com.abincaps.aop.Target"/>  
<bean id="myAspect" class="com.abincaps.aop.MyAspect"/>  

<aop:config>  
	<aop:aspect ref="myAspect"> 
		<!-- <aop:before> 元素用于声明 before advice, 在目标方法 `com.abincaps.aop.Target.test()` 执行前，将会调用切面中的 before 方法-->
		<aop:before method="before" pointcut="execution(public void com.abincaps.aop.Target.test())"/>  
	</aop:aspect>  
</aop:config> 
```

```java
package com.abincaps.aop;

public class Target implements TargetInterface {  
    @Override  
    public void test() {  
        System.out.println("test");  
    }  
}
```

```java
package com.abincaps.aop;  
  
public class MyAspect {  
    public void before() {  
        System.out.println("before");  
    }  
}
```

## After Returning Advice（后置通知）

- `<aop:after-returning/>` 当一个匹配的方法执行正常完成时，会运行 advice

## After Throwing Advice (异常抛出通知)

- `<aop:after-throwing/>` 当一个匹配的方法的执行通过抛出一个异常而退出时，After throwing advice 就会运行

## After (Finally) Advice（最终通知）

- `<aop:after/>` 无论匹配的方法执行如何退出都会运行

## Around Advice（环绕通知）

- `<aop:around/>` 围绕一个匹配的方法的执行而运行, 在方法运行之前和之后进行工作
- advice 方法可以声明 `Object` 作为它的返回类型, 该方法的第一个参数必须是 `ProceedingJoinPoint` 类型
- 在 advice 方法的主体中，必须在 `ProceedingJoinPoint` 上调用 `proceed()`，以使底层方法运行

```xml
<bean id="target" class="com.abincaps.aop.Target"/>  
<bean id="myAspect" class="com.abincaps.aop.MyAspect"/>  
  
<aop:config>  
    <aop:aspect ref="myAspect">  
        <aop:around method="around" pointcut="execution(public void com.abincaps.aop.Target.*(..))"/>  
    </aop:aspect>  
</aop:config>
```

```java
package com.abincaps.aop;

public class MyAspect {  
    public void around(ProceedingJoinPoint pjp) throws Throwable {  
        System.out.println("around before");  
        pjp.proceed(); 
        // Object res = pjp.proceed();  可以得到返回值 
        System.out.println("around after");  
    }  
}
```

## 声明Aspect(切面)

- `<aop:aspect>` 元素来声明切面，`ref` 属性来引用支持 Bean

```xml
<aop:config> 
	<aop:aspect id="myAspect" ref="myAspect">
	</aop:aspect> 
</aop:config> 
```

## 声明Pointcut（切点）

- `<aop:pointcut/>` 元素定义切入点

```xml
<bean id="target" class="com.abincaps.aop.Target"/>  
<bean id="myAspect" class="com.abincaps.aop.MyAspect"/>  
  
<aop:config>  
    <aop:pointcut id="myPoint" expression="execution(public void com.abincaps.aop.Target.*(..))"/>  
    <aop:aspect ref="myAspect">  
        <aop:before method="before" pointcut-ref="myPoint"/>  
        <aop:after method="after" pointcut-ref="myPoint"/>  
    </aop:aspect>  
</aop:config>
```

## 用XML配置启用 @AspectJ 的支持

```xml  
<aop:aspectj-autoproxy/>  
```

## @Aspect

- `@Aspect` 注解的类声明切面，被 Spring 自动检测到，并用于配置 Spring AOP
- 在 Spring AOP 中，切面本身不能成为其他切面的 advice 的目标

```java
@Component("myAspect")  
@Aspect  
public class MyAspect {  
}
```

## @Before

- `@Before` 注解在一个Aspect(切面)中声明 before advice(前置通知)，在切点匹配的方法执行之前运行

```java
@Component("myAspect")  
@Aspect  
public class MyAspect {  
  
    @Before("execution(public void com.abincaps.aop.Target.*(..))")  
    public void before() {  
        System.out.println("before");  
    }  
}
```

## @After

- `@After` 注解 当一个匹配的方法执行退出时，After (finally) advice（后置通知）会运行
- AspectJ 中的 `@After` 类似于 try-catch 语句中的 finally 块，它将对任何结果，正常返回或抛出异常进行调用

```java
@Component("myAspect")  
@Aspect  
public class MyAspect {  

    @After("execution(public void com.abincaps.aop.Target.*(..))")  
    public void after() {  
        System.out.println("after");  
    }  
}
```

## @Around

- `@Around`注解声明一个方法, 可以声明 `Object` 为返回值类型，该方法的第一个参数必须是 `ProceedingJoinPoint` 类型
- 在 advice 方法体中，必须在`ProceedingJoinPoint`上调用`proceed()`来执行切点方法

## Advice执行顺序

1. `@Around` 调用 `proceed()` 之前
2. `@Before`
3. 调用 `proceed()` 中
4. `@Around` 调用 `proceed()` 之后
5. `@After`
6. `@AfterReturning` 或 `@AfterThrowing`

## @Pointcut

- `@Pointcut` 注解来表示的 Pointcut（切点），pointcut 看作是对 Spring Bean 上的方法执行的匹配
- `@Pointcut` 注解的方法的返回类型必须是 `void` 

```java
@Component("myAspect")  
@Aspect  
public class MyAspect {  
  
    @Pointcut("execution(public void com.abincaps.aop.Target.*(..))")  
    public void pointcut() {}  
  
  
    @Before("pointcut()")  
    public void before() {  
        System.out.println("before");  
    }  
  
    @Before("pointcut()")  
    public void after() {  
        System.out.println("after");  
    }  
}
```

## JoinPoint接口

- `JoinPoint` 被拦截的方法，提供对连接点可用状态及其静态信息的反射访问
- 任何 advice method 都可以声明一个 `org.aspectj.lang.JoinPoint` 类型的参数作为其第一个参数
- `interface ProceedingJoinPoint extends JoinPoint`
- `Object[] getArgs()` 返回方法的参数

## Exceptions(异常)

- `@Controller` 和 `@ControllerAdvice` 类可以有 `@ExceptionHandler` 方法来处理来自 controller 方法的异常
- `@ExceptionHandler` 方法，每个方法通过其签名匹配一个特定的异常类型
- `@ControllerAdvice` 和 `@ResponseBody` 进行了元标注，这意味着 `@ExceptionHandler` 方法的返回值将通过响应体 message 转换呈现

