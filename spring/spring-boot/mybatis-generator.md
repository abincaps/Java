
# Mybatis代码生成器

## xml配置

- 避免SQL中的关键字与表名或列名冲突

```xml
<property name="autoDelimitKeywords" value="true"/>   是否自动为关键字加上限定符
<property name="beginningDelimiter" value="`"/>   开始限定符
<property name="endingDelimiter" value="`"/>  结束限定符
```

- 不生成注释

```xml
<commentGenerator>  
    <property name="suppressAllComments" value="true"/>  
</commentGenerator>
```