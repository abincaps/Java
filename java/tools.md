# 常见工具类

# Arrays

## asList方法

* `public static List asList(T... a`) 接受[可变参数](basic.md#ke-bian-can-shu), 返回大小固定的List类型的集合
* 内部元素仍指向原数组（可变参数在编译时转换为数组）, 并没有复制数组内容
* 任何对返回List的修改都会同步反映到原数组
* 不能添加和删除元素，否则保持列表不变并抛出 `UnsupportedOperationException`

## toString方法

- 返回指定数组内容的字符串表示形式
- 如果指定数组是 `null`，则返回 `"null"`

```java
int[] a = {1, 2, 3};  
System.out.println(Arrays.toString(a)); // [1, 2, 3]  
System.out.println(a.toString()); // [I@4eec7777

int[] b = null;  
System.out.println(Arrays.toString(b)); // null
```

# Collections

* `singletonList`  返回仅包含指定对象的不可变且可序列化的列表
* `joining` 返回一个收集器，按相遇顺序由指定分隔符连接的输入元素


