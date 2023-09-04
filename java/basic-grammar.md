# 基础语法 

## 字面量

- `long a = 1L;` L 表示 long 类型
- `float a = 1.0F;` F 表示 float 类型、
- 整数缺省是 int 类型
- `double a = 1.0D;` D 表示 double 类型 
- 浮点数默认字面量是 double , 可以不指定 D
- 前缀 `0x` 或 `0X` 十六进制
- 前缀 `0` 八进制
- 前缀 `0b` 或 `0B` 二进制

## 字面量里的下划线

- 只能使用单个下划线，不能连续使用多个
- 开头或结尾不能有下划线
- 前缀和后缀周围不能有下划线

## 数值精度顺序

- double > float > long > int > short > byte

## 基本类型的隐式转换

- byte, short, char, 自动转换成 int 类型参与运算
- char 和 short 都是 两个字节， 但是 char 是无符号数，超过 short 的范围
- 表达式中混合了 int 和 long , 则提升到 long 类型
- 宽化转型（widening conversion），不必显式进行类型转换

## 基本类型的强制转换

- boolean 类型 无法强制转换和自动转换（不允许任何类型的转换处理）
- 浮点数到整型需要强制转换 而整数到浮点数则不需要
- 窄化转型（narrowing conversation）， 需要显式类型转换

## 类型推断（type inference）

- `var` 关键字 
- `var a;` 错误 无法推断没有初始化的变量

## 数据类型

- 数据类型分为 基本数据类型 和 引用数据类型

## 基本类型和包装类

| 基本类型 | 包装类    | 字节数 |
| -------- | --------- | ------ |
| byte     | Byte      | 1      |
| short    | Short     | 2      |
| int      | Integer   | 4      |
| long     | Long      | 8      |
| float    | Float     | 4      |
| double   | Double    | 8      |
| char     | Character | 2      |
| boolean  | Boolean   | -      |
| void     | Void      |-       |

## 包装类

- 包装类是把基本类型包装成类
- 包装类的构造方法已过时 使用valueOf方法

## 自动装箱和自动拆箱

- 自动装箱（autoboxing）机制能够将基本类型自动转换为包装类对象
- 自动拆箱 包装类到基本类型的自动转换

## BigInteger 和 BigDecimal

- 支持高精度计算的类

## 字符和字符串的连接

```java
System.out.println('a' + 1 + "bin"); // 98bin
System.out.println("abi" + 'n' + 1); // abin1
```

## 字符串连接优化

```java
String a = "ab";  
String b = "abc";  
String c = a + "c";  
String d = "a" + "b" + "c"; // 编译器会优化成 “abc"  
System.out.println(b == c); // false  
System.out.println(b == d); // true
```

## Scanner

- `next()` 方法 读取输入的字符串，直到遇到空白符(空格，回车，换行)结束
- `nextLine()` 方法 读取输入的一行字符串，包括空格

## switch

- switch case 表达式中的类型支持 char, byte, short, int, Character, Byte, Short, Integer, String, or enum

## 作用域

- 隐藏变量的方式在java中是不被允许的
- 不能在外层代码块里使用内层代码块，也就是说内层代码块中创建的变量对外层代码块不可见
- 外层创建的变量对内层代码可见
- 内层命名空间中不可以重复定义外层代码块的变量

```java
// 对于c/c++合法，但是在java中语法错误
{  
    int x = 1;  
    {  
        int x = 0;  
    }  
}
```

## 基本类型的默认值

- 当基本类型作为类成员存在，才会初始化默认值，这种机制不会应用于局部变量

## printf中的格式字符串

- `%n` 换行
- `%b` boolean

## 关系运算符

- `==` 比较的是对象的引用是否相同，即变量是否指向同一个对象

## 操作符 

- 索引操作符 （indexing operator） `[]`

## 短路

- TODO

## 数组

- 数组是一种特殊的类

## 数组初始化

```java
// 在定义数组变量时确定数组元素的值
int[] a = new int[] {1, 3, 5};
int[] b = {1, 3, 5};

// 在定义数组变量时仅确定数组大小
int[] c = new int[3] {1, 3, 5}; // 编译错误
int[] c = new int[3]; // 正确
```

## enhanced for

```java
for (x : a) {
	...
}
```

## 方法签名

- 方法签名是 方法名 + 参数类型列表， 返回值不属于方法签名

## 方法重载

- 方法名相同，参数类型列表不同就是方法重载，和返回值无关

## public class

- 一个代码文件（编译单元）中可以写多个 `class` 类，但只能有一个类用 `public` 修饰
- `public` 修饰的类的名称必须和文件名一致
- 没有私有类，只有内部私有类

## package(包)

- 一个包 包含了一组类
- 默认导入java.lang

| 类所在位置        | 是否需要导包才可以访问 |
| -| -|
|同一个包下的类    | 否|
|java.lang包下的类| 否|
|其他包下的类      | 是|

## 包的命名

- 包的命名惯例，使用域名反转作为包名 例如 `com.example.project`

##  访问多个包下的同名类

- 访问多个包下的同名类，默认只能导入一个包下的类，另一些类必须带包名访问
- 直接使用完整类名可以不需要 `import` 导包

```java
Student a = new Student();
com.abincaps.Student b = new com.abincaps.Student(); 
```

## 通配符

- `import java.util.*;` 用\*通配符 导入java.util包下的所有类

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

- 可变参数(Varargs)允许方法接受个数不确定的同类型参数, 在编译时会转换为数组
- 一个方法中只能指定一个可变参数, 且必须是方法的最后一个参数

```java
public void print(String... msgs) {
  for (String msg : msgs) {
    System.out.print(msg);
  }
}

print("ab","in","caps");
```

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

- 当一个方法抛出了异常, 并没有被当前方法捕获异常并处理，那么这个异常会被传递给当前方法的调用者, 一直向上抛出, 直到被捕获或者程序终止
- 异常传递明确调用者需要处理传播过来的异常 强制处理异常或者向上抛出, 防止异常被隐藏

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

- `return` 结束还是异常结束都会执行 finally
- `finally` 里最好不要有 `return` 语句, `finally` 里尝试改变 `return` 的值没有任何作用

```java
// try-catch-finally 的执行顺序
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
// 其中之一满足即可
} catch (NullPointerException e | IOException e) {

}
```

## 常见异常

| 关键字                           | 解释                      |
| --------------------------- | --------------------- |
| `NullPointerException`      | 空指针异常, 比如调用空对象的方法     |
| `IndexOutOfBoundsException` | 索引越界, 访问了数组或集合中不存在的索引 |
| `ClassCastException`        | 强制类型转换错误, 转换不兼容的类型    |
| `IOException`               | IO操作异常, 如文件没找到        |
| `ClassNotFoundException`    | 加载类失败异常               |

## 注解

- 注解(annotation)是一种用于为代码提供元数据（Metadata）的工具

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override {
}


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

## 常见注解

- `@Override` 注解 检查是否正确重写，如果父类不存在该方法或签名不一致, 编译器会报错
- `@Deprecated` 表示被标注的元素已过时弃用，但不会阻止过时元素的使用

## JDK和JRE

- JDK 是 Java Development Kit (Java开发套件 )
- JRE 是 Java Runtime Environment (Java运行环境)
- JDK中包含了JRE，并且包含开发调试程序相关的工具

## java 和 javax

- java Java核心库 位于Java SE平台的库中
- javax Java扩展库 位于Java EE平台的库中

## 序列化id

- TODO
## 全限定名

- TODO

## Diamond语法

- Diamond（钻石）语法是Java 7中新增的语言特性, 用于简化泛型代码的书写
- `Map<String> map = new HashMap<这里不需要填写类型>();`

## 泛型为什么选择包装类而不是基本类？

- 泛型不支持基本类型，只支持引用类型
* 基本类型不能为 `null`
* 泛型实现时使用了[类型擦除](object-oriented.md#lei-xing-ca-chu)机制, 需要对象类型信息来保持类型安全
* 泛型的主要目的是实现通用算法和集合, 与对象相关性更强

## 字符集

- Java 中用的是 UTF-16 编码的 Unicode

## 转义符

- `\n` 换行符
- `\"` 双引号
- `\t` 制表符
- `\uXXXX` unicode 对应的字符





