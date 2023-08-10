# java基础语法

## tip 1

```java
在表达式中, byte, short, char, 是直接转换成int类型参与运算

boolean b = true;
int n = (int) b; //错误, 不能直接转换

java中没有无符号整数，和cpp unsigned不同
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

## tip 4

```java
switch case 表达式类型只能是 byte，short, int, char, jdk5开始支持枚举类型， jdk7支持String
```

## tip 5

```java
已移除
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

## tip 8

```java
一个代码文件中可以写多个class类，但只能有一个用public修饰
public修饰的类的名称必须和文件名一致
```

## tip 9

```java
当堆内存中的对象，没有被任何变量引用时，就会被判定为内存中的垃圾
java存在自动垃圾回收机制，会自动清除掉垃圾对象
```

## tip 10

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

## tip 11

```java
面向对象的三大特征：封装，继承，多态
```

## tip 12

```java
JavaBean 实体类 用于封装数据以及操作数据的行为
1. 有一个public的无参构造器
2. 所有的成员变量用private修饰
3. 提供public的setter和getter方法来访问成员变量(属性)
```

## tip 13

```java
1. 同一个包(package)下的类，方法可以直接调用
2. 其他包(package)下的类，方法, 需要导包才可以访问
3. java.lang包下的类，不需要导包就可以调用
4. 访问多个包下的同名类，默认只能导入一个包下的类，另一些类必须带包名访问

public static void main(String[] args) {
    Student a = new Student();
    com.abincaps.Student b = new com.abincaps.Student();
}

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

## tip 16

```java
String str = "abincaps";

// 截取部分字符串内容 [0, 2)
System.out.println(str.substring(0, 2)); //  ab

// [4, length) 从起始位置截取到最后
System.out.println(str.substring(4)); //  caps
```

## tip 17

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

## tip 19

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

## tip 21

```java
abstract class Animal {
    public abstract void run();
}

@FunctionalInterface
interface Func {
    // 只能有一个抽象方法
    void run();
 }
// 匿名内部类 本质上是子类
new Animal() {

    @Override
    public void run() {
        
    }
};

// Lambda表达式是Java 8开始引入
// 匿名内部类 本质上是子类
// 参数类型可以省略不写
// 如果只有一个参数, ()可以省略
// 如果代码体只有一行, {}可以省略
Func a =  () -> {
    System.out.println("run");
};

a.run(); // run
```

## tip 22

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
    // orName 返回与具有给定字符串名称的类或接口关联的 Class 对象
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

## tip 26

```java
异常传递
当一个方法抛出了异常, 并没有被当前方法捕获异常并处理
那么这个异常会被传递给当前方法的调用者, 一直向上抛出, 直到被捕获或者程序终止
```

## tip 27

```java
// 自定义异常类
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

## tip 28

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

## tip 29

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


```
