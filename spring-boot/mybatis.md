
## 官方文档

[https://mybatis.org/mybatis-3/zh/index.html](https://mybatis.org/mybatis-3/zh/index.html)

## 常见配置

- `mybatis.configuration.map-underscore-to-camel-case=true` 将结果集中的下划线命名方式自动映射到Java对象的驼峰命名方式
- `mybatis.type-handlers-package=TypeHandler扫描包名` 搜索类型处理器的包名
- `mybatis.mapper-locations=classpath:/mapper/**/*.xml` XML 映射文件的路径


- `@Mapper` 定义接口, MyBatis会根据接口定义自动生成实现类（不需要自己编写实现类），这个接口内的方法不能重载
- MyBatis-Spring-Boot-Starter 将默认搜寻带有 `@Mapper` 注解的 mapper 接口
- `@MapperScan`  对类路径进行扫描来发现映射器

## @Result

- `id` 是否为主键字段
- `column`  数据库列名
- `property`  Bean属性名

@Options
- `useGeneratedKeys` 是否使用自增主键
- `keyProperty` 自增主键对应的属性


- `@MapperScan`  对类路径进行扫描


## xml

- 固定开头

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
```
