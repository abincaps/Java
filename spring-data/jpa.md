
# JPA

JPA(Java Persistence API)是Sun官方提出的Java持久化规范， 为关系映射提供了一种基于POJO（Plain Old Java Object 简单的Java对象）的持久化模型

## Hibernate

Hibernate是JPA的实现，是一个开源的对象关系映射(ORM)框架, 实现对关系型数据库的对象映射，屏蔽了底层数据库的细节

## 实体类(Entity Class)

TODO

## 常见注解

<table><thead><tr><th width="308">注解</th><th>作用</th></tr></thead><tbody><tr><td><code>@Entity</code></td><td>指定该类是一个实体</td></tr><tr><td><code>@Table</code></td><td>指定注释实体的主表</td></tr><tr><td><code>@Column</code></td><td>指定持久性属性或字段的映射列</td></tr><tr><td><code>@Column(updatable = false)</code></td><td>表示这个列是只读的,不允许通过SQL更新这个列的值</td></tr><tr><td><code>@Column(nullable = false)</code></td><td>列不能为空</td></tr><tr><td><code>@Id</code></td><td>指定实体的主键</td></tr><tr><td><code>@JoinTable</code></td><td>关联的映射</td></tr><tr><td><code>@ManyToMany</code></td><td></td></tr><tr><td><code>@GenerationValue</code> </td><td>规定了主键值的生成策略，可与 Id 注解一起应用于实体或映射超类的主键属性或字段</td></tr><tr><td><code>@CreationTimestamp</code></td><td>指定属性为生成的创建时间戳, 时间戳只在实体实例插入数据库时生成一次</td></tr><tr><td><code>@UpdateTimestamp</code></td><td>指定属性为生成的更新时间戳, 每次在数据库中更新实体实例时，都会重新生成时间戳</td></tr><tr><td><code>@Enumerated</code></td><td>指定属性或字段为枚举类型</td></tr><tr><td><code>@OrderBy("id")</code></td><td>指定元素集合的排序</td></tr><tr><td><code>@EnableJpaRepositories</code></td><td>用于启用JPA仓库的支持</td></tr></tbody></table>

## 常见属性

```properties
spring.jpa.hibernate.ddl-auto=create-drop #创建时生成表结构，结束时删除
spring.jpa.properties.hibernate.show_sql=true #打印SQL语句
spring.jpa.properties.hibernate.format_sql=true #格式化输出
```

## Repository\<T, ID>接口

Spring Data repository 抽象的中心接口是 `Repository<T, ID>`

* `T` 实体类类型
* `ID` 实体类的主键类型

## CrudRepository\<T, ID>

* `Iterable findAll(Sort sort)`  返回按给定选项排序的所有实体
* `Sort.by(Sort.Direction.DESC, "id")` 根据id降序