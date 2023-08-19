# 基础语法

## 字面量

```java
// 浮点数默认字面量是double 需要用F指定float 或者强制转换
float a = 100.0F;
```

## 基本类型的隐式转换

在表达式中, `byte`, `short`, `char`, 直接转换成`int`类型参与运算&#x20;

如果表达式中混合了`int`和`long`, 则提升到`long`类型

## 基本类型的强制转换

<pre class="language-java"><code class="lang-java">boolean b = true;
int n = (int) b; //编译错误, 不能强制转换

// 从long类型向int类型的赋值需要强制转换, 否则会报错
int d = (int)100L;
// 类似的范围大的到范围小的赋值, 注意是赋值=，+=有表达式计算, 会触发截断

// 浮点数到整型需要强制转换 而整数到浮点数则不需要
<strong>int a = (int)100F;
</strong></code></pre>

## 八大基本类型和包装类

| 基本类型    | 包装类       | 字节数 |
| ------- | --------- | --- |
| byte    | Byte      | 1   |
| short   | Short     | 2   |
| int     | Integer   | 4   |
| long    | Long      | 8   |
| float   | Float     | 4   |
| double  | Double    | 8   |
| char    | Character | 2   |
| boolean | Boolean   | 1   |

## 包装类

```java
// 包装类构造方法已过时 使用valueOf
System.out.println(Integer.valueOf(10)); // 10
System.out.println(Integer.valueOf("10")); // 10
// 进制转换 需要符合进制规则
System.out.println(Integer.valueOf("10", 2)); // 2
System.out.println(Integer.valueOf("10", 8)); // 8
```

{% hint style="info" %}
包装类是把基本类型包装成类
{% endhint %}

## 自动装箱和自动拆箱

```java
Integer a = 10; // 自动装箱 基本类型到包装类的自动转换
int b = a; // 自动拆箱 包装类到基本类型的自动转换

System.out.println(Integer.toString(a) + 1); // 101
System.out.println(Integer.parseInt("10") + 1); // 11
```

## 字符和字符串的连接

```java
System.out.println('a' + 1 + "str"); // 98str
System.out.println("str" + 'a' + 1); // stra1
```

## 字符串连接优化

```java
String a = "ab";
String b = "abc";
String c = a + "c"; // 连接后的结果放在堆内存中
System.out.println(b == c); // false

String s1 = "abc";
String s2 = "a" + "b" + "c"; // 编译器会优化成 “abc"
System.out.println(s1 == s2); // true
```

## next和nextLin

> `next()` 读取输入的字符串，直到遇到空白符(空格，回车，换行)结束&#x20;
>
> `nextLine()` 读取输入的一行字符串，包括空格

## switch

{% hint style="info" %}
switch case 表达式中的类型支持`byte，short, int, char,` 枚举类型，`String`
{% endhint %}

## 数组初始化

```java
// 在定义数组变量时确定数组元素的值
int[] a = new int[] {1, 3, 5};
int[] b = {1, 3, 5};

// 在定义数组变量时仅确定数组大小
int[] c = new int[3] {1, 3, 5}; // 编译错误
int[] c = new int[3]; // 正确
```

## ==

\== 是关系运算符，用来判断两个对象的内存地址是否相同，即变量是否指向同一个对象

## 方法重载

{% hint style="info" %}
方法重载是根据方法签名（方法名+参数类型列表）相同，和返回值无关
{% endhint %}

## public class

{% hint style="info" %}
一个代码文件中可以写多个`class`类，但只能有一个类用`public`修饰
{% endhint %}

{% hint style="info" %}
public修饰的类的名称必须和文件名一致
{% endhint %}

{% hint style="info" %}
没有私有类，只有内部私有类
{% endhint %}

## package(包)

<table><thead><tr><th width="379">类所在位置</th><th>是否需要导包才可以访问</th></tr></thead><tbody><tr><td>同一个包下的类</td><td>否</td></tr><tr><td>java.lang包下的类</td><td>否</td></tr><tr><td>其他包下的类</td><td>是</td></tr></tbody></table>

```java
// 访问多个包下的同名类，默认只能导入一个包下的类，另一些类必须带包名访问
Student a = new Student();
com.abincaps.Student b = new com.abincaps.Student(); 
// 直接使用完整类名(全限定类名)可以不需要import导包

import java.time.*; // 用*通配符 导入java.time包下的所有类
```

## String

```java
// String类是不可变的字符串, 不能通过[]下标索引来获取或修改其中的某个字符
// 使用charAt返回此字符串指定索引处的 char 值

String str = "abincaps";
for (int i = 0; i < str.length(); i++) {
    System.out.print(str.charAt(i) + " ");
}


// 使用 toCharArray 返回一个新分配的字符数组，其内容为该字符串

String str = "abincaps";
char[] c_str = str.toCharArray();

for (int i = 0; i < c_str.length; i++) {
    System.out.print(c_str[i] + " ");
}
```

## replace

```java
String str = "abincaps";

// replace 替换所有 并不会改变String本身
System.out.println(str.replace("a", "bc")); // bcbincbcps
System.out.println(str); // abincaps
```

## 常量池和堆内存

```java
// "..."这样的字符串对象会存储在字符串常量池中，且相同的字符串只存储一份
// 常量池被创建在堆内存中
String str1 = "abincaps";

// new 会产生新的对象放在堆内存中
String str2 = new String("abincaps");

// a 和 b 引用的是同一个字符串对象
String a = "abincaps", b = "abincaps";
System.out.println(a == b); // true

String c = new String("abincaps"), d = new String("abincaps");

// 引用不同
System.out.println(c == d); // false
// equals 比较的是对象的内容
System.out.println(c.equals(d)); // true

String e = "abincaps", f = "ABINcaps";
```

## 可变参数

可变参数(Varargs)允许方法接受个数不确定的同类型参数, 在编译时会转换为数组&#x20;

```java
public void print(String... msgs) {
  for (String msg : msgs) {
    System.out.println(msg);
  }
}

print("ab","in","caps");
```

{% hint style="info" %}
一个方法中只能指定一个可变参数, 且必须是方法的最后一个参数
{% endhint %}

## 异常

```java
int [] arr = new int[1];
arr[1] = 0;
// 抛出异常 ArrayIndexOutOfBoundsException

// 捕获异常
try {
    String str = "";
    str.substring(1, 2);

// 捕获 Exception 类型的 也可以捕获其他类型的
// 但是不能捕获没有可能抛出的异常
} catch (Exception e) {

    // 打印异常的堆栈跟踪信息
    // StringIndexOutOfBoundsException
    e.printStackTrace();
}

// 如果没有捕获异常则不会执行
System.out.println("finished");
```

## Error和Exception

```java
// 逗号分开可以抛出多个异常 也可以抛出异常的父类 Exception
public static void main(String[] args) throws ClassNotFoundException {

    // 所有异常的父类 Throwable
    // Error 不可补救 系统级错误   Exception 可以补救 程序级错误
    // Error通常是虚拟机内部错误或者不可恢复的错误
    // checked exception 必须用try catch 或 throws处理异常
    // unchecked exception 不一定用try catch 或 throws处理异常
    // Error 和 RuntimeException 是 unchecked exception

    // Class 在 java.lang 包下
    // forName 返回与具有给定字符串名称的类或接口关联的 Class 对象
    Class.forName(""); // 加上throws ClassNotFoundException 不报错

    String str = null;
    // 是unchecked exception NullPointerException extends RuntimeException
    str.toString();
}
```

## 抛出指定异常

```java
public static void causeExcetion() throws Exception {

    // 抛出指定异常 checked Exception
    throw new Exception("");
}

public static void causeRuntimeExcetion() {

    // 抛出指定异常 unchecked Exception
    throw new RuntimeException("");
}
```

## 实现类中的异常

```java
public interface WithEx {
    void danger() throws Exception;
    void safe();
}

public class Foo implements WithEx {

    @Override
    // 抛出的异常必须和父类一样或者是父类异常的子类
    public void danger() throws Exception {
        // 接口中声明抛出异常
        // 实现类中可抛可不抛  throws Exception
        throw new Exception();

    }

    @Override
    public void safe() {

        // 接口中没有声明抛出异常
        // 实现类中可以抛出RunTimeException 或者不抛
        // throw new Exception(); 错误
        throw new RuntimeException();
    }
}
```

## 异常传递

当一个方法抛出了异常, 并没有被当前方法捕获异常并处理&#x20;

那么这个异常会被传递给当前方法的调用者, 一直向上抛出, 直到被捕获或者程序终止

{% hint style="info" %}
异常传递明确调用者需要处理传播过来的异常 强制处理异常或者向上抛出, 防止异常被隐藏
{% endhint %}

## 自定义异常类

```java
public class MyException extends Exception {
    public MyException() {
    }

    public MyException(String message) {
        super(message);
    }
    // cause 传入谁引起的异常
    public MyException(String message, Throwable cause) {
        super(message, cause);
    }
    public MyException(Throwable cause) {
        super(cause);
    }
}
```

## try-catch-finally

```java
public static int func() {
    int len = 0;
    try {
        String s = null;
        return s.length();

    } catch (Exception e) {
        len = -1;
        System.out.println("return");
        return len;
    } finally {
        // return结束还是异常结束都会执行 finally
        // finally 最好不要有return语句
        // finally 里尝试改变 return 值 没有作用
        System.out.println("finally");
        len = 0;
    }
}

System.out.println(func());
// return
// finally
// -1
```

## 多层catch

```java
try {
// 依次匹配，匹配到就不往下匹配了 catch的异常不能是上一层catch的子类
} catch (NullPointerException e) {

} catch (IOException e) {

}

try {
// 其中之一
} catch (NullPointerException e | IOException e) {

}
```

## 常见异常

|                             |                       |
| --------------------------- | --------------------- |
| `NullPointerException`      | 空指针异常, 比如调用空对象的方法     |
| `IndexOutOfBoundsException` | 索引越界, 访问了数组或集合中不存在的索引 |
| `ClassCastException`        | 强制类型转换错误, 转换不兼容的类型    |
| `IOException`               | IO操作异常, 如文件没找到        |
| `ClassNotFoundException`    | 加载类失败异常               |

## 注解

注解(annotation)是一种用于为代码提供元数据（Metadata）的工具&#x20;

`@Override`注解 检查是否正确重写 如果父类不存在该方法或签名不一致, 编译器会报错

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override {
}

@Deprecated
表示被标注的元素已过时 但并不会阻止过时元素的使用

// 不定义Target 缺省就是METHOD
@Target(ElementType.METHOD) // 可使用的位置
@Retention(RetentionPolicy.RUNTIME) // 保留阶段 (源码、编译、运行)
// SOURCE - 注解只保留在源代码中, 编译时就被丢弃
// CLASS - 注解在class文件中可用, 但会被虚拟机丢弃
// RUNTIME - 虚拟机将在运行期也保留注解, 因此可以通过反射机制读取

public @interface Myannotation {
    // 注解里支持的类型 基本数据类型 Class，String，枚举类，其他注解
    // 增加元数据 metadata
}
```

## JDK和JRE

JDK 是 Java Development Kit (Java开发套件 )

JRE 是 Java Runtime Environment (Java运行环境)

JDK中包含了JRE，并且包含开发调试程序相关的工具

## java 和 javax

java    Java核心库  位于Java SE平台的库中

javax  Java扩展库  位于Java EE平台的库中

## 序列化id

```java
TODO
```
