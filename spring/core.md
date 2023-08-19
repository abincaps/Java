# 核心特性

## IoC 容器 <a href="#beans" id="beans"></a>

> IoC(Inversion of Control),即控制反转
>
> 通过依赖注入 DI(Dependency Injection)实现对象之间的依赖和解耦
>
> IoC容器负责对象的创建、初始化等完整生命周期
>
> IoC容器自动装配对象, 统一了对象管理和配置

### Bean

> Bean是一个由Spring IoC容器实例化、组装和管理的对象
>
> Bean以及它们之间的依赖关系都反映在容器使用的配置元数据中

## DAO 层

> DAO(Data Access Object) 层是访问数据持久层的接口
>
> 包含各种数据库操作,如CRUD

## Service层

> Service层包含应用核心的业务逻辑处理，对业务逻辑进行封装
>
> 可以通过接口调用DAO层进行数据库操作

## Spring Framework版本

{% hint style="info" %}
Spring Framework 5.x 需要  java SE 8+ / java EE 7+
{% endhint %}
