# Stream

## Stream

- Stream 流，内部迭代
- 流会使用一个提供数据的源，如集合、数组或输入/输出资源
- 流的数据处理功能支持类似于数据库的操作，如 filter、map、reduce、find、match、sort等
- 很多流操作本身会返回一个流，形成流水线，链式编程

## 流的过程

- 生成流：通过数据源生成流
- 中间流：一个流后面跟着任意个中间操作，然后返回一个新的流进行操作
- 结束流：一个流只能有一个终止操作

## 创建流（生成流）的方式

- 使用静态方法 Stream.of 由值创建流
- 使用静态方法 Arrays.stream 由数组创建流
- 使用静态方法 Files.lines 构成的字符串流
- 集合 Collection 的 stream 方法

## Map获取流

```java
Map<String, String> map = new HashMap<>();  
map.keySet().stream();  
map.values().stream();
map.entrySet().stream();
```

## 数组获取流

- `Stream.of(T... values)` 就是调用的 `Arrays.stream（T[] array）`

## 创建无限流

- TODO

## 中间流（中间）操作链

- `Stream <T> filter(Predicate <? super T > predicate)` 和给定谓词匹配，过滤，筛选条件
* `Stream map(Function<? super T, ? extends R> mapper)` 由给定函数应用于此流的元素的结果所组成，将每个元素进行对应规则的映射(转换）
* `Stream <T> limit(long maxSize)` 截断后的长度不超过 `maxSize` 
* `Stream <T> skip(long n)` 丢弃(跳过）流的前 n 个元素, 如果此流包含的元素少于 `n` 个，则将返回一个空流
* `static <T> Stream <T> concat(Stream <? extends T> a, Stream <? extends T> b)` 串联流，其元素是第一个流的所有元素，后面跟上第二个流的所有元素
* `Stream <T> distinct()` 返回由该流的不同元素, 不包含重复值，去重
* `Stream <T> sorted(Comparator <? super T > comparator)` 根据提供的 `Comparator` 排序
* `Stream <T> sorted()` 按自然顺序排序, 如果此流的元素不是实现 Comparable\<T> 接口，则在执行终端操作时可能会抛出 `java.lang.ClassCastException`

## 结束流(终端)操作

- `void forEach(Consumer <? super T > action)` 对此流的每个元素执行一个操作，遍历
- `long count()` 元素个数计数


## 原始类型流特化接口

- IntStream, DoubleStream, LongStream