
# Spring Web MVC


## 注解式 Controller

-  `@Controller` 和 `@RestController` 组件使用注解来表达请求映射、请求输入、异常处理等内容
-  `@RestController` 是由 `@Controller` 和 `@ResponseBody` 组成的元注解，表示一个 controller 的每个方法都继承了类型级的 `@ResponseBody` 注解, 直接写入响应体，而不是用 HTML 模板进行视图解析和渲染


## 请求映射

- 使用 `@RequestMapping` 注解来映射请求到 controller 方法
- `@RequestMapping` 可以通过URL、HTTP方法、请求参数、header和媒体类型（meida type）进行匹配
- 默认情况下，`@RequestMapping` 匹配所有的HTTP方法
- `@RequestMapping` 可以使用在类上表示共享映射，也可以使用在方法上来缩小到一个特定的端点映射


## RequestBody

- `@RequestBody` 注解来让请求 body 反序列化为一个 `Object


## 异常（Exceptions)

- `@Controller` 和 `@ControllerAdvice` 类可以有 `@ExceptionHandler` 方法来处理来自 controller 方法的异常
- `@ExceptionHandler` 方法，每个方法通过其签名匹配一个特定的异常类型
- `@ControllerAdvice` 和 `@ResponseBody` 进行了元标注，这意味着 `@ExceptionHandler` 方法的返回值将通过响应体 message 转换呈现


