---
description: 提升开发效率
---

# Lombok

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

## builder和build

```java
Coffee coffee = Coffee.builder().name("abincaps").price(20).build();
// 最后使用build来返回Coffee对象    
```

## @Slf4j

提供日志
