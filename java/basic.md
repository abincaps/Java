# 基础语法 

## Java体系

- JavaSE（J2SE）Standard Edition 标准版
- JavaEE（J2EE) Enterprise Edition 企业版
- JavaME（J2ME）Micro Edition 微型版

## JDK JRE JVM 

- JDK（Java Development Kit） Java 开发工具包
- JRE（Java Runtime Environment） Java 运行环境
- JVM（Java Virtual Machine） Java 虚拟机
- JVM 是 Java 应用程序的运行时引擎，执行字节码
- JRE 提供了 Java应用程序运行所需的运行时环境，包括 JVM 和 标准类库
- JDK 包括 JRE，还提供了开发调试程序相关的工具

## java和javax

- java Java核心库 位于Java SE平台的库中
- javax Java扩展库 位于Java EE平台的库中

## 变量命名规则

- 只能由字母，数字，下划线，`$` 美元符组成
- 不能由数字开头
- 不能使用关键字
- 小驼峰命名法 getValue
- 大驼峰命名法 StudentImpl

## 数据类型

- 基本数据类型
- 引用数据类型

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

## 数值精度顺序

- double > float > long > int > short > byte

## 字面量

- 后缀 L 表示 long 类型
- 后缀 F 表示 float 类型
- 缺省表示 int 类型
- 后缀 D 表示 double 类型，浮点数默认字面量是 double , 可以不指定 D
- 前缀 `0x` 或 `0X` 表示十六进制
- 前缀 `0` 表示八进制
- 前缀 `0b` 或 `0B` 表示二进制

## 字面量里的下划线

- 只能使用单个下划线，不能连续使用多个
- 开头或结尾不能有下划线
- 前缀和后缀周围不能有下划线

## 基本类型的隐式转换

- byte short char 自动转换成 int 类型参与运算
- char 和 short 都是 2个字节, 但是 char 是无符号数，超过 short 的范围
- 表达式中混合了 int 和 long , 则提升到 long 类型
- 宽化转型（widening conversion），不必显式进行类型转换

## 基本类型的强制转换

- boolean 类型 无法强制转换和自动转换（不允许任何类型的转换处理）
- 浮点数到整型需要强制转换 而整数到浮点数则不需要
- 窄化转型（narrowing conversation）， 需要显式类型转换

## type inference 类型推断

- `var` 关键字 
- `var a;` 错误, 无法推断没有初始化的变量

## 包装类

- 包装类是把基本类型包装成类
- 包装类的构造方法已过时，推荐使用 valueOf 方法

## 自动装箱

- 自动装箱机制能够将基本类型自动转换为对应的包装类对象
## 自动拆箱

- 自动拆箱机制实现包装类到基本类型的自动转换

## 字符和字符串的连接

- `'a' + 1 + "bin"` 98bin
- `"abi" + 'n' + 1` abin1

## 字符串连接优化

- `"abc" == "a" + "b" + "c"` true

```java
String a = "a";  
a = a + "bc";  
System.out.println("abc" == a); // false
```

## switch

- switch case 表达式中的类型支持 char, byte, short, int, Character, Byte, Short, Integer, String, or enum

## 作用域

- 隐藏变量的方式在 java 中是不被允许的
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

## 关系运算符

- `==` 如果是引用数据类型比较的是对象引用是否指向相同的内存地址, 即是否指向同一个对象
- `==` 如果是基本数据类型比较的是值是否相同

## 无符号右移运算符

- `>>>`无符号右移运算符, 没有`<<<`运算符
- 在左侧用零填充，不考虑正负号

## 短路

- 在逻辑与运算中，如果第一个条件为 false，则不会继续执行第二个条件，第三个条件...
- 在逻辑或运算中，如果第一个条件为 true，则不会继续执行第二个条件，第三个条件...

## Array 数组

- 数组是一种特殊的类，是引用数据类型
- 数组长度在定义之后不能修改
- 数组占用连续的存储空间
- 数组中成员数据类型必须相同

## 数组初始化

- `int[] a = new int[] {1, 3, 5};` 在定义数组变量时确定数组元素的值
- `int[] a = {1, 3, 5};` 简化
- `int[] a = new int[3] {1, 3, 5};` 编译错误
- `int[] a = new int[3];` 在定义数组变量时仅确定数组大小，默认初始化为 0

## enhanced for 增强for循环

```java
for (x : a) {
}
```

## package 包

- 一个包包含了一组类
- 默认导入 java.lang 包

## 包访问

| 类所在位置        | 是否需要导包才可以访问 |
| -| -|
|同一个包下的类    | 否|
|java.lang包下的类| 否|
|其他包下的类      | 是|

## 包的命名

- 包的命名惯例使用域名反转作为包名 `com.abincaps.project`

##  访问多个包下的同名类

- 访问多个包下的同名类，默认只能导入一个包下的类，另一些类必须带包名访问
- 直接使用完整类名可以不需要 `import` 导包

```java
Student a = new Student();
com.abincaps.Student b = new com.abincaps.Student(); 
```

## 通配符

- `import java.util.*;` 用`*`通配符导入 java.util 包下的所有类

## 可变参数

- 可变参数允许方法接受个数不定的同类型参数
- 可变参数在编译时会转换为数组
- 一个方法中只能指定一个可变参数, 且必须是方法的最后一个参数
- `void print(String... args)`

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

## try-with-resources

- 在 try 语句中声明所要使用的资源对象
- 被声明的资源类必须实现 java.lang.AutoCloseable 接口
- try代码块结束后,会自动关闭这个资源对象,即调用其close()方法
- 无需在 finally 块中手动关闭资源

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

## 全限定名

- `com.abincaps.Myclass`

## Diamond语法

- Diamond（钻石）语法是 Java 7 中新增的语言特性, 用于简化泛型代码的书写
- `Map<String> map = new HashMap<这里不需要填写类型>();`

## 泛型为什么选择包装类而不是基本类？

- 泛型不支持基本数据类型，只支持引用数据类型
* 基本类型不能为 `null`
* 泛型实现时使用了[类型擦除](object-oriented.md#lei-xing-ca-chu)机制, 需要对象类型信息来保持类型安全
* 泛型的主要目的是实现通用算法和集合, 与对象相关性更强

## 字符集

- Java 中用的是 UTF-16 编码的 Unicode

## toString

- `println()` 默认调用 `String.valueOf()` 
- `valueOf()` 再调用 `toString()`

## hashCode和equals

* `hashCode`不相等, `equals`一定是`false`
* `hashCode`相等, `equals`不一定是`true`
* `equals`返回`true`, `hashCode`必须相等
- `hashCode`相当于人名, `hashCode`一样并不代表是同一个人

## 泛型

```java
public class Vector<E> {

    Object[] array = new Object[10];
    private int size = 0;
    public boolean add(E e) {
        ...
        return ...;
    }

    public E get(int index) {
        ...
        return ...;
    }
}

// E 只能是Base类或者其子类
public class Vector<E extends Base> {
}

// 泛型方法
public static <T> T test(T t) {
    return t;
}

// 调用时就可以获得正确的类型
int a = test(0);
// 泛型会在返回的地方插入强制转换，在需要的时候
test(0); // 这里不需要强制转换

// 使用泛型中 ？表示通配
public static void test(ArrayList<?> e) {
}
// 上限继承 上界通配符
// 表示参数化的类型可能是T 或 T 的子类型
public static void test(ArrayList<? extends Base> e) {
}

// 下限继承 下界通配符
// 表示参数化的类型可能是 T 或 T 的父类型
public static void test(ArrayList<? super Derived> e) {
}
```

## 协变和逆变

```java
// 协变和逆变是针对引用类型而言
// 在泛型中的协变(covariance)和逆变(contravariance)表示泛型类型参数在继承层次中的变化关系
class Base {
}

class Derived extends  Base {
}

// 泛型类型不管继承关系
ArrayList<Base> a = new ArrayList<>();
a.add(Derived); // 报错

// 让没有泛型的引用指向有泛型的引用, 可以放任意数据
// 但是会造成污染，再用泛型引用指向没有泛型的引用，去使用数据的时候会报错

// 协变
// Derived 是 Base的子类 但是 ArrayList<Derived> 不是 ArrayList<Base> 的子类
// ArrayList<Base> a = new ArrayList<Derived>(); 错误
// ? extends Base 可以接受Base或者其子类
ArrayList<? extends Base> a = new ArrayList<Derived>();

// a.add(new Base()); 错误 会引发更多的错误
// a.add(new Derived()); 错误 会引发更多的错误

// ? super Derived 可以接受Derived或者其父类 直到Object类
ArrayList<? super Derived> b = new ArrayList<Base>();


// new ArrayList<? super Derived>(); 错误 只有引用的时候可以宽泛一点、
// 读取使用协变， 写入使用逆变
```

## 类型擦除

- 将泛型参数的类型信息在编译时替换为对象类型
- 也就是将泛型类型“擦除”, 转换为普通的非泛型类型的代码






