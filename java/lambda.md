
# lambda函数式编程

## lambda表达式

- lambda 是 Java 1.8 引入的一种类似于匿名内部类的表达式
- 只有一个参数，可以省略不写括号，前提是没有写参数类型
- 没有参数时，必须使用括号来表示空的参数列表
- 有多个参数，必须使用括号
- 如果 lambda 表达式有多行，必须放在花括号内
- 如果 lambda 表达式只有一行，可以省略不写花括号，并且如果有 return，需要删除
- 参数类型可以省略不写，只有参数名

## 函数式接口

- `@FunctionalInterface`
- 接口中有且仅有一个抽象方法，单一抽象方法

## 实现接口的方式

- new 实现类
- 匿名内部类
- lambda 表达式，需要函数式接口
- 方法引用，需要函数式接口和特定情况

## 方法引用

- 方法引用是 Lambda 表达式的进一步简写
- 类名::静态方法名 指向静态方法的方法引用
- 对象::实例方法名 指向现有对象的实例方法的方法引用
- 类::实例方法名 指向任意类的实例方法的方法引用，这个对象本身是 Lambda 的一个参数 
- 类名::new 构造方法引用
- 方法引用的签名必须和上下文类型匹配

## 函数式接口作为参数

```java
Thread thread = new Thread(() -> {  
    System.out.println("thread");  
});  
thread.start();
```

## 函数式接口作为返回值

```java
public static Comparator<String> getComparator() {  
    return (a, b) -> a.length() - b.length();  
}
```

## Supplier

- `Supplier<T>` 生产者接口 `T get()` () -> T 创建对象

## Consumer

- `Consumer<T>` 消费者接口 `void accept(T t) ` T -> void
- `default Consumer <T> andThen(Consumer <? super T > after)`  按顺序执行此操作，然后是 `after` 操作

```java
con1.accept(str);  
con2.accept(str); 
// 上面两行等同于下面一行
con1.andThen(con2).accept(str);
```

## Predicate

- `Predicate<T>` `boolean test(T t)` 布尔表达式
- `default Predicate<T> negate()` 逻辑否定, 逻辑非
- `default Predicate<T> and(Predicate<? super T> other)` 逻辑与
- `default Predicate<T> or(Predicate<? super T> other)` 逻辑或

## Function

- `Function<T, R>` `R apply(T t)`  T -> R 类型转换, 加工处理
- `default <V> Function<T, V> andThen(Function<? super R, ? extends V> after)` 先将此函数应用于其输入，然后将 `after` 函数应用于结果

```java
private static void func(String str, Function<String, Integer> fun1, Function<Integer, String> fun2) {  
    Integer integer = fun1.apply(str);  
    String string = fun2.apply(integer);  
    // 上面两行等同于下面一行  
    fun1.andThen(fun2).apply(str);  
}
```