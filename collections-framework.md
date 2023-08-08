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

## tip 6

```java

```

## tip 98

```java

TODO
泛型为什么选择包装类而不是基本类型
```

## tip 99

```java

TODO
自动装箱

```

