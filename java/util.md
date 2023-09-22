
# java.util

- 集合框架
- 国际化支持类
- 服务加载器
- 属性
- 随机数生成
- 字符串解析
- 扫描类
- base64 编码和解码
- 位数组
- 遗留集合类
- 遗留日期和时间类

## Scanner从键盘输入

- `Scanner sc = new Scanner(System.in);` 创建从标准输入读取数据的 `Scanner` 对象

## Scanner类常用方法

- `Scanner(InputStream source)` 生成从指定输入流扫描的值 Scanner sc = new Scanner(System.in);
- `void close()` 关闭此扫描器
- `String next()` 读取输入的字符串，直到遇到空白符(空格，回车，换行)结束
- `String nextLine()` 方法 读取输入的一行字符串，包括空格
- `int nextInt()` 返回从输入扫描的 int

## Arrays类

- 操作数组的方法

## Arrays类常用方法

- `static <T> List<T> asList(T... a)` 返回大小固定的 List 类型的集合, 内部元素仍指向原数组（可变参数在编译时转换为数组, 并没有复制数组内容, 任何对返回 List 的修改都会同步反映到原数组, 不能添加和删除元素，否则保持列表不变并抛出 `UnsupportedOperationException`
- `static void fill(int[] a, int val)` 将指定的 int 值分配给指定的 int 数组的每个元素
- `static void sort(int[] a)` 将指定数组升序排序
- `static String toString(int[] a)` 返回指定数组内容的字符串表示形式, 由数组元素列表组成，括在方括号 (`"[]"`) 中，如果指定数组是 `null`，则返回 `"null"`

```java
int[] a = {1, 2, 3};  
System.out.println(Arrays.toString(a)); // [1, 2, 3]  
System.out.println(a.toString()); // [I@4eec7777
```

## Arrays自定义排序

- Comparator 比较器用到了泛型，不能用基本数据类型的数组
	
```java
Integer[] a = {1, 5, 3, 4, 5};
Arrays.sort(a, (x, y) -> y - x);
System.out.println(Arrays.toString(a)); // [5, 5, 4, 3, 1]

Arrays.sort(a, new Comparator<Integer>() {  
    @Override  
    public int compare(Integer o1, Integer o2) {  
        return Integer.compare(o1, o2);  
    }  
});
```

# Collections

* `singletonList`  返回仅包含指定对象的不可变且可序列化的列表
* `joining` 返回一个收集器，按相遇顺序由指定分隔符连接的输入元素