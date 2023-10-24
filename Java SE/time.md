
# Time

## LocalDateTime类

- Java 8版本及以后的类
- 无时区信息
- 不可变
- 包含 LocalDate 和 LocalTime

```java
private final LocalDate date;  
private final LocalTime time;
```

## LocalDateTime类常用方法

- `static LocalDateTime now()` 从默认时区的系统时钟获取当前日期时间