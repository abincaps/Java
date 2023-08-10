# java集合框架

## tip 1

```java
// ArrayList()构造一个初始容量为 10 的空列表 
// 当元素超过容量时候，会以原来的1.5倍扩容

 // jdk 5 开始支持泛型 明确指定只能接受<>中的类型元素
ArrayList<Integer> a = new ArrayList<>();
// 指定初始容量的参数构造
ArrayList<Integer> b = new ArrayList<>(20);
```

## tip 2

```java
ArrayList<Integer> list = new ArrayList<>();

for (int i = 0; i < 3; i++) {
    // 往集合最后添加元素
    list.add(i); // 类似cpp中的push_back
}

System.out.println(list); // [0, 1, 2]

// 在指定索引位置插入元素
// 如果索引超出范围 (index < 0 || index > size())
// 抛出 IndexOutOfBoundsException
list.add(1, 3); // 类似cpp中的insert

System.out.println(list); // [0, 3, 1, 2]
```

## tip 3

```java
ArrayList<Integer> list = new ArrayList<>();

for (int i = 0; i < 3; i++) {
    list.add(i);
}

System.out.println(list); // [0, 1, 2]
// 返回此list中指定索引位置的元素
System.out.println(list.get(1)); // 1
// 返回此list中的元素个数 返回类型是int
System.out.println(list.size()); // 3
// 移除此list中指定位置的元素, 将任何后续元素向左移动 size减1
// 并返回移除的元素
System.out.println(list.remove(1)); // 1
System.out.println(list); // [0, 2]
```

## tip 4

```java
ArrayList<Integer> list = new ArrayList<>();

for (int i = 0; i < 3; i++) {
    list.add(i);
}

System.out.println(list); // [0, 1, 2]
// 返回此list中指定索引位置的元素
System.out.println(list.get(1)); // 1
// 返回此list中的元素个数 返回类型是int
System.out.println(list.size()); // 3
// 移除此list中指定位置的元素, 将任何后续元素向左移动 size减1
// 并返回移除的元素
System.out.println(list.remove(1)); // 1
System.out.println(list); // [0, 2]
// 从此list中移除第一次出现的指定元素（如果存在）, 返回boolean表示是否包含该元素
System.out.println(list.remove(Integer.valueOf(2))); // true
System.out.println(list); // [0]
// 用指定元素替换(修改）此list中指定位置(索引)的元素。
list.set(0, 1);
System.out.println(list); // [1]
```

## tip 5

```java
java 常用 Collection 接口及其底层数据结构和继承关系如下:

List接口:
    ArrayList - 动态数组
    LinkedList - 双向链表
    Vector - 动态数组(线程安全)
Set接口:
    HashSet - 哈希表 通过HashMap来实现
    LinkedHashSet - 哈希表 + 链表
    TreeSet - 红黑树
Queue接口:
    LinkedList - 双向链表
    PriorityQueue - 基于堆的优先级队列
Deque接口:
    ArrayDeque - 动态数组
    LinkedList - 双向链表
Map接口:
    HashMap - 哈希表
    LinkedHashMap - 哈希表+链表
    TreeMap - 红黑树
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

// 使用自己实现的类作为key，必须保证hashCode和 equals 方法都重写
// 作为key一定是不可变的
```

## tip 101

```java

TODO
泛型为什么选择包装类而不是基本类型
泛型不支持基本类型，只支持引用类型
基本类型不能为null
泛型实现时使用了擦除机制, 需要对象类型信息来保持类型安全
泛型的主要目的是实现通用算法和集合, 与对象相关性更强

擦除机制(Type Erasure)
将泛型参数的类型信息在编译时替换为对象类型
也就是将泛型类型“擦除”, 转换为普通的非泛型类型代码
```

## tip 102

```java
TODO
hashCode和equals的约定
equals返回true hashCode必须相等
hashCode不相等 equals一定是false
hashCode相等 equals不一定是true
 
hashCode相当于人名
hashCode一样并不代表是同一个人

```

## tip 103

```java
TODO
Iterator 和 Iterable接口
```
