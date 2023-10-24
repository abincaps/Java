
# Junit单元测试

## 测试

- 黑盒，不需要写代码， 输入值对应输出的期望值
- 白盒，需要写代码，关注程序的执行流程

## @Test

- `@Test` 用于指示带注释的方法是一种测试方法
- `@Test` 方法不能是 `private` 或 `static` 或 有返回值 （也就是说返回值类型必须是void）
- 不写 main 方法 就可以有运行入口点

## 常见注解

- `@BeforeAll` 注解的方法应在当前测试类中的所有测试之前执行
- `@AfterAll` 注解的方法应在当前测试类中的所有测试之后执行

## Assertions

- `void assertEquals(int expected, int actual)` 断言 `expected` 和 `actual` 相等的

## @RunWith

- `@RunWith(SpringJUnit4ClassRunner.class)` 或 `@RunWith(SpringRunner.class)` 来注解测试类
- `@ContextConfiguration` 配置 ApplicationContext

```java
@RunWith(SpringJUnit4ClassRunner.class)  
@ContextConfiguration(classes = SpringConfiguration.class) // 指定用于配置 Spring应用程序上下文的配置类
public class SpringJunitTest {  
  
    @Resource(name = "dataSource")  
    private DataSource dataSource;  
  
    @Test  
    public void test() throws SQLException {  
        Connection con = dataSource.getConnection();  
    }  
}
```




