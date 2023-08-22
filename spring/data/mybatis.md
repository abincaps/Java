---
description: 一款持久层框架
---

## 官方文档
[https://mybatis.org/mybatis-3/zh/index.html](https://mybatis.org/mybatis-3/zh/index.html)

## 常见属性

- `mybatis.configuration.map-underscore-to-camel-case=true` 将结果集中的下划线命名方式自动映射到Java对象的驼峰命名方式
- `mybatis.type-handlers-package=TypeHandler扫描包名` 

## @Mapper

定义接口, MyBatis会根据接口定义自动生成实现类（不需要自己编写实现类）， 这个接口内的方法不能重载

## @Result

- `id` 是否为主键字段
- `column`  数据库列名
- `property`  Bean属性名

@Options
- `useGeneratedKeys` 是否使用自增主键
- `keyProperty` 自增主键对应的属性