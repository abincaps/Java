# 集合框架

## Diamond语法

```java
Map<String, List<Integer>> map = new HashMap<这里不需要填写类型>();
```

{% hint style="info" %}
Diamond语法是Java 7中新增的语言特性, 用于简化泛型代码的书写
{% endhint %}

## ArrayList的构造器

```java
// ArrayList()构造一个初始容量为 10 的空列表 
// 当元素超过容量时候，会以原来的1.5倍扩容
ArrayList<Integer> a = new ArrayList<>();

// 指定初始容量的参数构造
ArrayList<Integer> b = new ArrayList<>(20); 
```

## Collection 接口

```java
常用 Collection 接口及其底层数据结构和继承关系如下:

List接口:
    ArrayList - 动态数组
    LinkedList - 双向链表
    Vector - 动态数组(线程安全)
Set接口:
    HashSet - 哈希表 通过HashMap来实现 无序
    LinkedHashSet - 哈希表 + 链表
    TreeSet - 红黑树 有序
Queue接口:
    LinkedList - 双向链表
    PriorityQueue - 基于堆的优先级队列
Deque接口:
    ArrayDeque - 动态数组
    LinkedList - 双向链表
Map接口:
    HashMap - 哈希表 无序
    LinkedHashMap - 哈希表+链表
    TreeMap - 红黑树 有序
```

## HashMap

```java
// HashMap 和 Map 在 java.util 包下
// HashMap<Key,Value> 键值映射
// Key不能重复，Value可以重复
// 底层数据结构是哈希表 集合里面的元素是无序的
HashMap<String, Integer> h = new HashMap<>();
// HashMap是实现了Map接口的类
// 接口仅定义行为规范, 不关注具体实现
// 实现类提供接口具体的实现方式
Map<String, Integer> m = new HashMap<>();

for (int i  = 0; i < 10; i++) {
    h.put("Key" + i, i);
}

h.put(null, null); // 不会抛出异常

// 不存在key返回null
// get接受的是Object类型
System.out.println(h.get("Key")); // null
System.out.println(h.get("Key1")); // 1
// 根据键删除键值对 返回删除键的值
System.out.println(h.remove("Key1")); // 1

// 返回此map中包含的映射的 Set 视图。
// 如果在对集合进行迭代时映射被修改
// （除了通过迭代器自己的 remove 操作，或通过迭代器返回的映射条目上的 setValue 操作）
//  迭代的结果是未定义的 (未定义行为 undefined behavior UB）
for (Map.Entry<String, Integer> e : h.entrySet()) {
    System.out.println(e.getKey() + " " + e.getValue());

}

// 如果是int 会空指针异常 因为之前插入了null
for (Integer value : h.values()) {
    System.out.println(value);
}

for (String key : h.keySet()) {
    System.out.println(key);
}

// 使用自己实现的类作为key，一定是不可变的
// 必须保证hashCode 和 equals 方法都重写(覆盖)
```

## 泛型为什么选择包装类而不是基本类？

* 基本类型不能为`null`
* 泛型实现时使用了[类型擦除](object-oriented.md#lei-xing-ca-chu)机制, 需要对象类型信息来保持类型安全
* 泛型的主要目的是实现通用算法和集合, 与对象相关性更强

{% hint style="info" %}
泛型不支持基本类型，只支持引用类型
{% endhint %}

## Iterator 和 Iterable接口

```java
TODO
```

## Collection\<E>接口

* `stream` 方法返回以此集合为源的顺序 `Stream`

## Stream\<T>接口

* `Stream map(Function<? super T, ? extends R> mapper)` 返回一个流，由给定函数应用于此流的元素的结果所组成，说人话就是，将每个元素进行对应规则的映射(转换）

```java
list.stream().map(o -> o.getId().toString())
```

* `collect`  使用收集器对该数据流的元素执行可变还原操作

## Function\<T,R>接口
