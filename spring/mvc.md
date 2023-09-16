
# Spring Web MVC

## DispatcherServlet

- `<context-param>` 设置上下文参数
- `<servlet>` 配置 Servlet, 负责处理所有的HTTP请求
- `<init-param>` 配置 Servlet 的初始化参数
- `<load-on-startup>` 指定 Servlet 的加载顺序
- `<servlet-mapping>` 将 URL 模式映射到 Servlet

```xml
<!-- web.xml -->
<context-param>  
    <param-name>contextConfigLocation</param-name>  
    <param-value>classpath:ApplicationContext.xml</param-value>  
</context-param>

<servlet>  
    <servlet-name>DispatcherServlet</servlet-name>  
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
	<init-param>  
	    <param-name>contextConfigLocation</param-name>  
	    <param-value>classpath:SpringWebMVC.xml</param-value>  
	</init-param>  
	<load-on-startup>10</load-on-startup>
</servlet>  
  
<servlet-mapping>  
    <servlet-name>DispatcherServlet</servlet-name>  
    <url-pattern>/</url-pattern>  
</servlet-mapping>
```

```xml
<!-- SpringWebMVC.xml 开启组件扫描 -->
<context:component-scan base-package="com.abincaps.controller"/>
```

## @RequestMapping

- `@RequestMapping` 注解将请求映射到 controller 方法
- 可以通过URL、HTTP方法、请求参数、header和媒体类型（meida type）进行匹配
- `@RequestMapping` 使用在类上表示共享映射，使用在方法上来缩小到特定的端点映射
- `value` 指定请求的URL地址(映射路径)
- `method` 指定映射到的 HTTP 请求方法 （GET、POST）
- `params` 指定参数限制请求映射
- `produces` 指定映射请求可以生成的 Media Type (媒体类型), 可以指定一个字符集

```java
@Controller  
@RequestMapping("/user")  
// 请求路径以 "/user" 开头时，将由 UserController 类处理
public class UserController {  

    @RequestMapping("/test")
    // 请求路径是 "/user/test" 时，将调用 `test` 方法来处理请求
    public String test() {  
        return "/test.jsp";  
    }  
}
```

```java
// 如果不写 produces = {"text/html;charset=utf-8"}， “中文” 会显示 “？？”
@RequestMapping(value = "/test", produces = {"text/html;charset=utf-8"})  
@ResponseBody  
public String test() {  
    return "中文";  
}
```

## 视图解析器

- 在使用 JSP 开发时，会声明一个 `InternalResourceViewResolver` bean
- `InternalResourceViewResolver` 可用于调度任何 Servlet 资源
- 建议将 JSP 文件放在 `'WEB-INF'` 目录下的 JSP 目录中，这样就不会被客户端直接访问

```xml
<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
	<!-- prefix 指定视图资源的前缀(视图文件所在的目录) -->
    <property name="prefix" value="/WEB-INF/jsp/"/> 
    <!-- suffix 指定视图资源的后缀(视图文件的扩展名) -->
    <property name="suffix" value=".jsp"/>  
</bean>
```

## 重定向

- 视图名称中 `redirect:` 前缀执行重定向
- 重定向会导致浏览器地址栏中的URL发生变化，显示为新的目标 URL

## 转发

- 视图名称中 `forward:` 前缀执行转发
- 原始请求的 URL 在浏览器的地址栏中保持不变

## ServletRequest接口

- 将客户端的请求信息传递给 Servlet
- `String getParameter(String var1)` 获取请求参数 `var1` 的值
- `RequestDispatcher getRequestDispatcher(String var1)`  `var1` 指定获取资源的路径, `RequestDispatcher` 对象是给定路径上的资源包装器
- `void setAttribute(String var1, Object var2)` 在此请求中存储属性

## HttpServletRequest接口

- `HttpServletRequest` 封装客户端发往服务器的 HTTP 请求的信息
- `HttpServletRequest` 继承（扩展） `ServletRequest` 接口以提供 HTTP servlet 的请求信息

```java
@RequestMapping("/test")  
public String test(HttpServletRequest request) {
    return "/test.jsp";  
}
```

## ServletResponse

- 向客户端发送响应
- `PrintWriter getWriter()` 返回可以将字符文本发送到客户端的对象

## PrintWriter

- TODO 准备移动至java.io
- 将格式化的对象打印到文本输出流
- `void println(String x)` 打印一行字符串

## HttpServletResponse

- `HttpServletResponse` 继承（扩展）`ServletResponse` 接口以在发送响应时提供特定的 HTTP 

```java
@RequestMapping("/test")  
public void test(HttpServletResponse response) throws IOException {  
    response.getWriter().println("abincaps");  
}
```

## @ResponseBody

- 在一个方法上使用 `@ResponseBody` 注解，通过 `HttpMessageConverter` 将返回值序列化为响应体（指定方法的返回值绑定到 Web 响应体中）
- 指定该方法的返回值应该作为 HTTP 响应的主体内容发送给客户端

```java
@RequestMapping("/test")  
@ResponseBody  
public String test() {  
    return "abincaps";  
}
```

```java
@RequestMapping("/test")  
@ResponseBody  
public String test() {  
    // 返回 json 格式的数据
    return "{\"username\":\"abincaps\",\"password\":\"abc\"}";  
}
```

## RequestDispatcher接口

- `RequestDispatcher` 指定特定路径或特定名称的服务器资源的包装器，来包装任何类型的资源
- `void forward(ServletRequest var1, ServletResponse var2)` 将请求转发给另一个资源来生成响应

```java
@RequestMapping("/test")  
public void test(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	request.setAttribute("username", request.getParameter("username"));  
	request.setAttribute("password", request.getParameter("password"));
    request.getRequestDispatcher("/test.jsp").forward(request, response);  
}
```

```jsp
<a href="/test?username=abincaps&password=abcd">test</a>
```

```jsp
<%--success.jsp--%>
<p>username = ${username}</p>  
<p>password = ${password}</p>
```

## ModelAndView

- `ModelAndView` 将模型数据和视图信息打包成一个对象, 同时持有模型（Model）和视图（View)
- `ModelAndView addObject(String attributeName, @Nullable Object attributeValue)` 向模型添加属性（键值对，键是属性的名称，值是属性的值）
- `void setView(@Nullable View view)` 指定名称设置视图

```java
@RequestMapping("/test")  
public ModelAndView test(HttpServletRequest request) { 
// 也可以写成 test(HttpServletRequest request, ModelAndView modelAndView)
// 就不用 new ModelAndView()

    ModelAndView modelAndView = new ModelAndView();  
  
    modelAndView.addObject("username", request.getParameter("username"));  
    modelAndView.addObject("password", request.getParameter("password"));  
  
    modelAndView.setViewName("/test.jsp");  

    return modelAndView;  
}
```

## Model接口

- 定义模型属性的持有者
- `Model addAttribute(String attributeName, @Nullable Object attributeValue)` 添加属性（在指定的名称下添加指定的属性）

```java
@RequestMapping("/test")  
public String test(HttpServletRequest request, Model model) {  
    model.addAttribute("username", request.getParameter("username"));  
    model.addAttribute("password", request.getParameter("password"));  
    return "/success.jsp";  
}
```

## XML配置启用注解驱动的Spring MVC功能

- `<mvc:annotation-driven/>` 注册了一些 Spring MVC 基础设施 Bean，并适应 classpath 上可用的依赖关系（例如，JSON、XML 的 payload converter）

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xmlns:context="http://www.springframework.org/schema/context"  
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="  
       http://www.springframework.org/schema/beans       
       http://www.springframework.org/schema/beans/spring-beans.xsd       
       http://www.springframework.org/schema/context       
       http://www.springframework.org/schema/context/spring-context.xsd       
       http://www.springframework.org/schema/mvc    
       https://www.springframework.org/schema/mvc/spring-mvc.xsd">  
  
    <context:component-scan base-package="com.abincaps.controller"/>  
    <mvc:annotation-driven/>  
</beans>
```

## jackson中的Json转换

```xml
<!-- 添加jackson依赖 -->
<dependency>  
    <groupId>com.fasterxml.jackson.core</groupId>  
    <artifactId>jackson-databind</artifactId>  
    <version>x.x.x</version>  
</dependency>
```

```java
@RequestMapping(value = "/test")  
@ResponseBody  
public User test() {
	// 需要启用注解驱动的 Spring MVC 功能
    return new User("abincaps", "abc");  
}
```

```java
// 不需要启用注解驱动的 Spring MVC 功能
@RequestMapping(value = "/test")  
@ResponseBody  
public String test() throws JsonProcessingException {  
    User user = new User("abincaps", "abc");  
    ObjectMapper om = new ObjectMapper();  // ObjectMapper 用于将 Java 对象转换为 JSON 字符串
    String userJson = om.writeValueAsString(user); // 将 User 对象序列化为 JSON 字符串
    return userJson;  
}
```

## 获取基本类型引用类型的请求参数

```java
@RequestMapping("/test")  
@ResponseBody  
public void test(String username, String password) {  
    System.out.println(username + ":" + password);  
}
```

## 获取POJO的请求参数

```java
// user类需要 getter setter 方法
@RequestMapping("/test")  
@ResponseBody  
public void test(User user) {  
    System.out.println(user.getUsername() + ":" + user.getPassword());  
}
```

## 获取数组类型请求参数

```java
// myweb/test?array=0&array=1  
@RequestMapping("/test")  
@ResponseBody  
public void test(String[] array) {  
    for (String e : array) {  
        System.out.println(e);  
    }  
}
```

## 获取集合类型请求参数

```java
// User类需要有无参构造方法
public class ValueObject {  
    private List<User> userList;  
  
    public List<User> getUserList() {  
        return userList;  
    }  
  
    public void setUserList(List<User> userList) {  
        this.userList = userList;  
    }  
  
    @Override  
    public String toString() {  
        return "ValueObject{" +  
                "userList=" + userList +  
                '}';  
    }  
}
```


```java
@RequestMapping("/test")  
@ResponseBody  
public void test(ValueObject vo) {  
    System.out.println(vo);  
}
```

## @RequestBody

- `@RequestBody` 注解来让请求 body 通过 HttpMessageConverter 读取并反序列化为一个 Object
- 指定方法参数应该绑定到 HTTP 请求的主体

```java
// User类需要写无参构造方法
// 需要开启 <mvc:annotation-driven/>
// 添加对应的 json 依赖
@RequestMapping(value = "/test", method = RequestMethod.POST)  
@ResponseBody  
public void test(@RequestBody List<User> userList) {  
    System.out.println(userList);  
}
```

## @RequestParam

- `@RequestParam` 注解将 Servlet 请求参数绑定到 controller 中的方法参数
- 如果不用 `@RequestParam`，请求参数名需要和方法参数名对应一致
- `value` 请求参数名
- `required` 指定请求参数是否为必需，`false` 请求参数里没有该参数也不会发生错误请求(HTTP状态 400), 默认为 `true`， 绑定的方法参数需要是引用类型，否则不能表示 null, 除非指定 `defaultValue`
- `defaultValue` 指定当请求参数不存在或参数值为空时的默认值

```java
// test?uname=abincaps&passwd=root
// 也可以是 test?passwd=root 
@RequestMapping(value = "/test")  
@ResponseBody  
public void test(@RequestParam(value = "uname", required = false, defaultValue = "abincaps")String username,@RequestParam("passwd") String password) {  
    System.out.println(username + ":" + password);  
}
```

## @PathVariable

- `value` 将请求的 URI 模板变量绑定到控制器方法的参数上, URI 模板变量是 URI中 的占位符，用花括号 `{}` 括起来

```java
// /myweb/test/abincaps  
@RequestMapping("/test/{username}")  
@ResponseBody  
public void test(@PathVariable("username") String username) {  
    System.out.println("username = " + username);  
}
```

## @RequestHeader

- `@RequestHeader` 注解将请求头和 controller 中的方法参数绑定
- Host
- Accept
- Accept-Language
- Accept-Encoding

```java
@RequestMapping("/test")  
@ResponseBody  
public void test(@RequestHeader("Host") String host, @RequestHeader("Referer")String referer) {  
    System.out.println(referer);  
    System.out.println("Host : " + host);  
}
```

## @CookieValue

- `@CookieValue` 注解将 HTTP cookie 的值绑定到 controller 的方法参数上
- HTTP Cookie 是一种用于在客户端和服务器之间存储状态信息的机制，通常用于跟踪用户会话或存储用户首选项（服务器识别和跟踪用户）

```java
@RequestMapping("/test")  
@ResponseBody  
public void test(@CookieValue("JSESSIONID") String jsession_id) {  
    System.out.println(jsession_id);  
}
```

## 类型转换

- 如果参数被声明为 `String` 以外，可能需要类型转换, 对于这种情况，类型转换会根据配置的 converter 自动应用
- 默认情况下，支持简单类型（`int`、`long`、`Date`)

## 自定义类型转换

- 自定义类型转换会覆盖原有的转换格式

```java
// /test?date=2023-9-16
@RequestMapping("/test")  
@ResponseBody  
public void test(Date date) {  
    System.out.println(date);  
}
```

```java
package com.abincaps.convert;

public class DateConverter implements Converter<String, Date> {  
    @Override  
    public Date convert(String source) {  
  
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd");  
  
        Date date = null;  
  
        try {  
            date = simpleDateFormat.parse(source);  
        } catch (ParseException e) {  
            e.printStackTrace();  
        }  
  
        return date;  
    }  
}
```

- `ConversionServiceFactoryBean` 创建和配置 Spring 的类型转换服务（Conversion Service）

```xml
<bean id="dateConverter" class="org.springframework.context.support.ConversionServiceFactoryBean">  
    <property name="converters">  
        <set>            
	        <bean class="com.abincaps.convert.DateConverter"/>  
        </set>    
    </property>  
</bean>  
  
<mvc:annotation-driven conversion-service="dateConverter"/>
```

## Multipart接口

- 处理文件上传
- `String getOriginalFilename()` 返回客户端文件系统中的原始文件名
- `void transferTo(File var1)` 将接收到的文件传输到指定的目标文件

```xml
<!-- https://mvnrepository.com/artifact/commons-io/commons-io -->  
<dependency>  
    <groupId>commons-io</groupId>  
    <artifactId>commons-io</artifactId>  
    <version>2.4</version>  
</dependency>  
  
<!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->  
<dependency>  
    <groupId>commons-fileupload</groupId>  
    <artifactId>commons-fileupload</artifactId>  
    <version>1.3.1</version>  
</dependency>
```

```jsp
<form action="/myweb/test" method="post" enctype="multipart/form-data">  
    <input type="text" name="username" placeholder="用户名">  
    <input type="file" name="uploadFile">  
    <input type="submit" value="上传">  
</form>
```

```xml
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">  
    <property name="defaultEncoding" value="UTF-8"/>  
    <property name="maxUploadSize" value="102400"/>  
</bean>
```

```java
@RequestMapping(value = "/test", method = RequestMethod.POST)  
@ResponseBody  
public void test(String username, MultipartFile uploadFile) throws IOException {  
    System.out.println("username = " + username);  
    String filename = username + uploadFile.getOriginalFilename();  
    uploadFile.transferTo(new File("C:\\workspace\\upload\\" + filename));  
}
```

## 多文件上传

```jsp
<%-- 增加 multiple --%>
<form action="/myweb/test" method="post" enctype="multipart/form-data">  
    <input type="text" name="username" placeholder="用户名">  
    <input type="file" name="uploadFiles" multiple>  
    <input type="submit" value="上传">  
</form>
```

```java
@RequestMapping(value = "/test", method = RequestMethod.POST)  
@ResponseBody  
public void test(String username, MultipartFile[] uploadFiles) throws IOException {  
    System.out.println("username = " + username);  
    
    for (MultipartFile e : uploadFiles) {  
        String filename = username + e.getOriginalFilename();  
        e.transferTo(new File("C:\\workspace\\upload\\" + filename));  
    }  
}
```




