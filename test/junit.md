
# Junit单元测试

## 测试

- 黑盒，不需要写代码， 输入值对应输出的期望值
- 白盒，需要写代码，关注程序的执行流程

## @Test

- `@Test` 用于指示带注释的方法是一种测试方法
- `@Test` 方法不能是 `private` 或 `static` 或 有返回值
- 不写 main 方法 就可以有运行入口点


## 常见注解

- `@BeforeAll` 注解的方法应在当前测试类中的所有测试之前执行
- `@AfterAll` 注解的方法应在当前测试类中的所有测试之后执行

## Assertions

- `void assertEquals(int expected, int actual)` 断言 `expected` 和 `actual` 相等的

