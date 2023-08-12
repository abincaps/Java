# java基础语法

## 基本类型的隐式转换

```java
在表达式中, byte, short, char, 是直接转换成int类型参与运算

java中没有无符号整数，和cpp中的unsigned不同

如果表达式混合了int和long, 则提升到long类型

// 编译错误,超出int范围 
int a = 1000_0000_0000 * 100;

// 正确 提升到long
long b = 100 * 10000000000L;

// 运行时溢出异常
long c = 10000000000L * 10000000000L;
```

## 基本类型的强制转换

```java
boolean b = true;
int n = (int) b; //错误, 不能强制转换

// 从long类型向int类型的赋值需要强制转换, 否则会报错
int d = (int)100L;
// 类似的范围大的到范围小的赋值， 注意是赋值=，+= 有表达式计算

// 浮点数到整型也需要强制转换
// 整数到浮点数则不需要
int a = (int)100F;

// 浮点数默认字面量是double 需要用F指定float
// 或者强制转换
float a = 100.0F;
```



## tip 2

```java
// char + int 和 字符串 + 字符
System.out.println('a' + 1 + "str"); // 98str
System.out.println("str" + 'a' + 1); // stra1
```

## tip 3

```java
next() 读取输入的字符串，直到遇到空白符(空格，回车，换行)结束
nextLine() 读取输入的一行字符串，包括空格
```

## switch

```java
switch case 表达式类型只能是 byte，short, int, char, jdk5开始支持枚举类型， jdk7支持String
```

## tip 6

```java
// 下面都是在堆内存中分配对象
// a，b 静态初始化，在定义数组变量时就确定数组元素的值

int[] a = new int[] {1, 3, 5};
int[] b = {1, 3, 5};

// c 动态初始化，在定义数组变量时仅确定数组大小

int[] c = new int[3] {1, 3, 5}; // 错误
int[] c = new int[3]; // 正确
System.out.println(a.length); // 数组长度
```

## tip 7

```java
一个类中方法名相同，形参列表不同，就是方法重载，和返回值类型，形参名称无关
```

## public class

```java
一个代码文件中可以写多个class类，但只能有一个用public修饰
public修饰的类的名称必须和文件名一致
```

## 自动垃圾回收机制

```java
当堆内存中的对象，没有被任何变量引用时，就会被判定为内存中的垃圾
java存在自动垃圾回收机制，会自动清除掉垃圾对象
```

## 构造器

```java
// 如果没有写构造器，类会默认生成一个无参构造器
// 如果写了有参构造器， 类不会自动生成无参构造器

public class Student {
    // 构造器重载
    // 无参数构造器
    public Student() {
        ...
    }

    // 带参数构造器
    public Student(String name) {
        ...
    }
}
```

## 面向对象

```java
面向对象的三大特征：封装，继承，多态
```

## JavaBean 实体类

```java
JavaBean 实体类 用于封装数据以及操作数据的行为
1. 有一个public的无参构造器
2. 所有的成员变量用private修饰
3. 提供public的setter和getter方法来访问成员变量(属性)
```

## package(包)

```java
1. 同一个包下的类，方法可以直接调用
2. 其他包下的类，方法, 需要导包才可以访问
3. java.lang包下的类，不需要导包就可以调用
4. 访问多个包下的同名类，默认只能导入一个包下的类，另一些类必须带包名访问

Student a = new Student();
com.abincaps.Student b = new com.abincaps.Student();

import java.time.*; // 导入java.time包下的所有类
```

## tip 14

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

## tip 15

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
// equalsIgnoreCase 忽略大小写比较
System.out.println(e.equalsIgnoreCase(f)); // true
```

## substring

```java
String str = "abincaps";

// 截取部分字符串内容 [0, 2)
System.out.println(str.substring(0, 2)); //  ab

// [4, length) 从起始位置截取到最后
System.out.println(str.substring(4)); //  caps
```

## replace

```java
String str = "abincaps";

// replace 替换所有 并不会改变String本身
System.out.println(str.replace("a", "bc")); // bcbincbcps
System.out.println(str); // abincaps
```

## tip 18

```java
String str = "abincaps";
// contains判断是否包含
System.out.println(str.contains("ab")); // true
// startsWith 判断是否开头包含， 从ab开始
System.out.println(str.startsWith("ab")); // true
```

## split

```java
String s = ",abc,efg,";

// split 根据给定分割符 拆分字符串 返回String数组
for (String x : s.split(",")) {
    System.out.println(x);
}


空
abc
efg
空
```

## tip 20

```java

String a = "ab";
String b = "abc";
String c = a + "c"; // 连接后的结果放在堆内存中
System.out.println(b == c); // false


String s1 = "abc";
String s2 = "a" + "b" + "c"; // 编译器会优化成 “abc"
System.out.println(s1 == s2); // true
```

## 匿名内部类和lambda

```java
abstract class Animal {
    public abstract void run();
}

// 匿名内部类 本质上是子类
new Animal() {

    @Override
    public void run() {
        
    }
};

// lambda表达式是Java 8开始引入的特殊匿名内部类 
// 本质上是继承父类的子类
// 参数类型可以省略不写
// 如果只有一个参数, () 可以省略
// 如果代码体只有一行, {} 和 return 可以省略
```


## lambda和函数式接口

```java
// Lambda表达式是Java 8开始引入
// 匿名内部类 本质上是子类
// 参数类型可以省略不写
// 如果只有一个参数, ()可以省略
// 如果代码体只有一行, {}可以省略

// 函数式接口
@FunctionalInterface
interface Func {
    // 只能有一个抽象方法
    void run();
 }

Func a =  () -> {
    System.out.println("run");
};

a.run(); // run

// 函数式接口
@FunctionalInterface
public interface Runnable {
    // 只能有一个抽象方法
    public abstract void run();
}

Thread thread = new Thread(() -> {
    // 方法体内就是重写的run方法体
    // 线程执行的代码
});

```

## Exception

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

## tip 23

```java
// 逗号分开可以抛出多个异常 也可以抛出异常的父类 Exception
public static void main(String[] args) throws ClassNotFoundException {

    // 所有异常的父类 Throwable
    // r两类异常：Error 和 Exception
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

## tip 24

```java
public static void causeExcetion() throws Exception {

    // 抛出自定义异常 checked Exception
    throw new Exception("");
}

public static void causeRuntimeExcetion() {

    // 抛出自定义异常 unchecked Exception
    throw new RuntimeException("");
}
```

## tip 25

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

```java
当一个方法抛出了异常, 并没有被当前方法捕获异常并处理
那么这个异常会被传递给当前方法的调用者, 一直向上抛出, 直到被捕获或者程序终止

异常传递明确调用者需要处理传播过来的异常
强制处理异常或者向上抛出, 防止异常被隐藏
```

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

```java
NullPointerException  发生空指针异常, 比如调用空对象的方法。
IndexOutOfBoundsException 索引越界, 访问了数组或集合中不存在的索引。
ClassCastException  强制类型转换错误, 转换不兼容的类型
IOException  IO操作异常, 如文件没找到
ClassNotFoundException  加载类失败异常
```

## 注解

```java
注解(annotation)是一种用于为代码提供元数据的工具
编译时和部署时的处理 运行时的检查
@Override 重写
检查是否正确重写 如果父类不存在该方法或签名不一致, 编译器会报错

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

```java
JDK 是 Java Development Kit， 是Java开发套件
JRE 是 Java Runtime Environment， 是Java运行环境
JDK中包含了JRE，并且包含开发调试程序相关的工具
```
