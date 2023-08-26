
# Spring Web MVC


 - `@RequestBody` 注解来让请求 body 反序列化为一个 `Object

## 异常（Exceptions)

- `@Controller` 和 `@ControllerAdvice` 类可以有 `@ExceptionHandler` 方法来处理来自 controller 方法的异常
- `@ExceptionHandler` 方法，每个方法通过其签名匹配一个特定的异常类型
- `@ControllerAdvice` 和 `@ResponseBody` 进行了元标注，这意味着 `@ExceptionHandler` 方法的返回值将通过响应体 message 转换呈现


