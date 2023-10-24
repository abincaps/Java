
## 启用缓存注解

- `@EnableCaching` 注解添加到 `@Configuration` 类上
- XML配置使用 `cache:annotation-driven` 元素

## @Cacheable

- 将方法的返回值缓存，以便在后续相同参数调用时，直接返回缓存的结果，而无需重新执行方法
- `String key()` 默认所有方法参数都被视为键， 指定 key 的生成方式

## @CacheEvict

- 从缓存中删除数据
- `boolean allEntries()` Whether all the entries inside the cache(s) are removed. Note that setting this parameter to true and specifying a key() is not allowed.





