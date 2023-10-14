
# Lombok

## 官方api

- https://projectlombok.org/api/

## Lombok简介

- 提升开发效率

## @Data

- 为所有字段生成 getter 方法
- 为所有非 final 字段生成 setter 方法
- 生成 toString 实现
- 生成 equals 实现
- 生成 hashCode 实现
- 生成一个构造方法
- 相当于 @Getter、@Setter、@ToString、@EqualsAndHashCode 和 @RequiredArgsConstructor 注解


## 常见注解

|                               |                        |
| ----------------------------- | ---------------------- |
| `@ToString`                   |                        |
| `@ToString(callSuper = true)` | 在输出中包含超类实现的`toString`  |
| `@EqualsAndHashCode`          |                        |
| `@Getter`                     |                        |
| `@Setter`                     |                        |
|                               |                        |
| `@Data`                       |                        |
| `@Builder`                    | 生成一个builder方法来构造       |


## @Builder

- `build()` 返回原始类型的完整实例
- `Foo foo = Foo.builder().name("abincaps").age(10).build();`

## @Slf4j

提供日志
