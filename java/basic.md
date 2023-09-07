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

- `com.abincaps.Myclass`

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

# 面向对象

## 面向对象编程(OOP)

- 三大特征：封装，继承，多态

## 类和对象的关系

- 类是对象的模板， 对象是类的一个实例

## 构造器（构造方法）

- 如果没有写任何构造器，类会自动生成一个无参构造器, 术语零参构造器（zero-argument constructor）
- 如果写了构造器（无论是否有参数）， 类不会再自动生成无参构造器
- 构造器不能自己调用自己， 会形成死循环

## 静态成员和实例成员的调用

- static 成员 通过对象来调用（不推荐）或 通过类名调用（推荐）
- 非 static 成员 只能通过对象调用

## 静态方法和实例方法

- 静态方法可以放访问静态成员，但不能访问实例成员
- 实例方法可以访问静态成员和实例成员
- 静态方法里定义的变量默认是静态的，不需再额外加 static
- 在实例方法里不能定义静态变量
- 静态方法中没有 this 自引用

## static 代码块和 非 static 代码块

- 静态代码块在类加载时执行, 并且只执行一次
- 静态代码块只能访问类中的静态成员
- 实例代码块在创建对象时运行, 每创建一个对象, 就执行一次该代码块
- 实例代码块运行顺序在构造器前

## 静态代码块出现的问题

```java
class  A {  
    static {  
        a = 10;  
        System.out.println(a); // 错误  
    }  
    static int a;  
}
```

## 类的初始化顺序

```java
public class Foo {

    static {
        System.out.println("static");
    }

    {
        System.out.println("non-static");
    }

    public Student() {
        System.out.println("Foo");
    }
}

new Foo();
new Foo();

// static
// non-static
// Foo
// non-static
// Foo
```

## this关键字

- this 只能在非静态方法中使用
- this 不能同时调用两个构造器


## 前向引用（forward referencing）

- 此内容省略

## 继承

* 只支持单继承, 不支持多继承，一个类只能继承一个直接父类&#x20;
* 支持多层继承 例如c继承b，b继承a&#x20;
* 子类(派生类)可以继承父类(基类或超类)的非私有成员(成员变量和成员方法)

## 访问权限修饰符

<table><thead><tr><th width="222">关键字</th><th>访问权限范围</th></tr></thead><tbody><tr><td><code>private</code></td><td>只能在本类中可见</td></tr><tr><td>缺省</td><td>在同一包下的类可见</td></tr><tr><td><code>protected</code></td><td>在同一包下的类可见，且在任意包下的子类里可见</td></tr><tr><td><code>public</code></td><td>在任意包下的任意类里可见</td></tr></tbody></table>

## Object类

- Object 类 是所有类的基类, 默认继承 Object 类

## 方法重写(覆盖)

* 子类重写的方法必须与父类被重写的方法有相同的方法签名（方法名和参数类型列表）
* 重写方法的返回值类型必须是被重写方法返回值的类型或子类型
* 子类方法的访问权限必须大于等于父类方法
* 父类的访问权限不能是 private，因为在子类中不可见
* 静态方法不能被重写

```java
public class Base {
    protected void print() {
        System.out.println("Base");
    }
}

public class Derived extends Base {

    @Override
    public void print() {
        System.out.println("Derived");
    }
}

Derived a = new Derived();
a.print(); // Derived
```

## 方法重写(覆盖)中的异常

* 方法的重写(Override)中 子类方法不允许抛出比父类更广泛的异常
* 如果父类方法没有`throws`声明, 则子类重写方法也不能有 `throws` 声明(除非是 `RuntimeException` 及其子类)
* 父类方法 `throws` 的异常, 子类必须捕获处理（不用声明 `throws` ）或者声明 `throws`

## toString

```java
// 下面两句相同
System.out.println(a.toString());
System.out.println(a); 
// println 默认调用String.valueOf() 
// valueOf 再调用toString

public class Student {

    private String name;
    private int age;

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "[name=" + name + ", age=" + age + "]";
    }
}

Student a = new Student("abincaps", 10);
System.out.println(a); // [name=abincaps, age=10]
```

## hashCode和equals

* `hashCode`不相等, `equals`一定是`false`
* `hashCode`相等, `equals`不一定是`true`
* `equals`返回`true`, `hashCode`必须相等
- `hashCode`相当于人名, `hashCode`一样并不代表是同一个人

## this和super

```java
public class Base {
    String name = "Base";
}

public class Derived extends Base {
    String name = "Derived";

    public void print() {
        String name = "print";
        // 优先局部变量
        System.out.println(name); // print
        // this关键字解决了局部变量与成员变量同名的情况
        System.out.println(this.name); // Derived
        // 访问父类成员
        System.out.println(super.name); // Base
    }

    // 方法的访问也和上面变量的访问类似
}
```

## 子类构造器

```java
public class Base {
    public Base() {
        System.out.println("Base");
    }
}

public class Derived extends Base {
    public Derived() {
        super(); // 写不写都会默认调用父类的无参构造器
        // 如果父类的无参构造器不存在，则会报错
        // 报错就要用super指定调用父类的有参构造器
        // 构造器不能被重写(覆盖)
        System.out.println("Derived");
    }
}

// 子类构造器会先调用父类的无参构造器，再执行自己
new Derived();
// Base
// Derived
```

## 多态

```java
public class People {
    public String name = "People";
    public void run() {
        System.out.println("people");
    }
}

public class Student extends People {
    public String name = "Student";
    @Override
    public void run() {
        System.out.println("Student");
    }

    public void study() {
        System.out.println("study");
    }
}

public class Teacher extends People {
    public String name = "Teacher";
    @Override
    public void run() {
        System.out.println("Teacher");
    }

    public void teach() {
        System.out.println("teach");
    }
}

// 多态 父类的引用变量可以指向子类的对象
// 在编译时只知道变量的父类型, 在运行时才确定具体对象
// 多态 有两个必要条件: 继承, 重写父类方法
// 向上转型 子类对象可以自动转型为父类对象
People a = new Teacher();
a.run(); // Teacher

// 变量不具备多态性,只有方法才有多态特性
System.out.println(a.name); // People

People b = new Student();
b.run(); // Student

a.teach(); // 错误 不能访问独有功能
b.study(); // 错误

// instanceof 测试一个对象是否为一个类的实例
if (a instanceof Teacher) {
    Teacher c = (Teacher)a; // 强制类型转换 向下转型
    System.out.println(c.name); // Teacher
}

Student d = (Student)a; // 错误 不报错 运行后抛出ClassCastException异常
```

## final

- final 修饰的类不能被继承
- final 修饰的方法不能被子类重写
- final 修饰的变量, 必须在声明时或构造器或代码块中初始化
- final 修饰的引用变量，无法重新赋值，但可以修改对象的内容

## static final

- static final 修饰的变量必须直接初始化


## private 和 final

- 类中的任何 private 方法都是隐式 final

## 抽象类和抽象方法

- 抽象类即使有构造器也不能实例化
- abstract不能修饰构造器
- 抽象类可以包含抽象方法和非抽象方法
- 抽象方法只包含方法签名, 没有方法体（也就是不实现）
- 有抽象方法的类必须声明为抽象类
- 子类继承抽象类时, 必须实现抽象类的所有抽象方法，否则子类必须声明为抽象类

## 内部类

```java
public class Outer {
    int age = 30;

    // 成员内部类 定义在外部类的成员位置
    public class Inner {

        public static int a; // jdk 16 开始支持定义静态成员
        int age = 20;
        public void print() {
            int age = 10;

            // 结局重名冲突
            System.out.println(age); // 10
            System.out.println(this.age); // 20
            System.out.println(Outer.this.age); // 30
        }
    }
}

// 内部类创建
Outer.Inner in = new Outer().new Inner();
```

## 静态内部类

```java
static不可以修饰外部类, 但可以修饰内部类

因为静态内部类不持有外部类的this引用, 不依赖于外部类的实例
也就是静态内部类不能直接引用外部类的this

public class Outer {
    static int outerStaticNum;
    int outerNum;

    // 静态内部类
    static class Inner {
        
        void accessMembers() {
            System.out.println(outerStaticNum); // 直接访问外部类的静态成员

            // 不能直接访问外部类的非静态成员
            // 需要先实例化外部类, 再通过外部类实例才能访问
            Outer outer = new Outer();
            System.out.println(outer.outerNum);

        }

        static void staticInnerMethod(){
            // 静态内部类可以包含静态成员
            System.out.println("static inner method");
        }
    }
}

// 访问静态内部类
Outer.Inner.staticInnerMethod();

// 静态内部类可以在外部类没有实例化的情况下直接使用new进行实例化
Outer.Inner inner = new Outer.Inner();
inner.accessMembers();
```

## 匿名内部类

```java
public class Foo {

    public void print() {
        System.out.println("Foo");
    }
}
// 匿名内部类 是一种特殊的内部类
// 经常作为参数
// 不能使用访问修饰符和static修饰
// 必须继承父类或实现接口
new Foo() {
    @Override
    public void print() {
        System.out.println("anonymous");
    }
}.print(); // anonymous
```

## lambda

```java
lambda表达式是Java 8开始引入的特殊匿名内部类 
本质上是继承父类的子类
参数类型可以省略不写
如果只有一个参数, () 可以省略
如果代码体只有一行, {} 和 return 可以省略
```

## 枚举类

```java
枚举类 常量集合
枚举类是final修饰的类 不可以被继承
枚举类会隐式继承Enum
枚举类既可以在类的内部声明, 也可以作为独立的类声明

public enum Color {

    // 默认从0开始数

    // 成员变量默认声明为final
    // 第一行只能定义常量
    RED, GREEN, BLUE;
    // 枚举类的构造器默认私有
}

Color a = Color.RED;
System.out.println(a); // RED

// 返回一个数组包含所有常量
Color[] b = Color.values();

for (int i = 0; i < b.length; i++) {
    System.out.print(b[i] + " ");
}
// RED GREEN BLUE
Color c = Color.valueOf("RED");

System.out.println(c); // RED
// 序号
System.out.println(c.ordinal()); // 0


public enum Color {
    RED, GREEN, BLUE;

    public void print() {
        System.out.println(this);
    }
}

Color.BLUE.print(); // BLUE

// 枚举类实现单例

// TODO
// 抽象枚举类
```

## 枚举类中的values方法

- values方法返回一个包含此枚举类型常量的数组

## 接口

- 接口可以继承

```java
// 不能实例化
public interface A {
    // 接口中的变量都是常量(自动隐式为由public static final修饰)，必须赋值
    String name = "Foo";

    // 方法默认是public abstract类型, 抽象方法不带方法体
    void print();
}

public interface B {
    void print();
}

// 一个类可以实现(implements)多个接口
// 必须重写接口中的所有抽象类或者声明是抽象类
public class C implements A, B {

    // 接口之间声明冲突 实现类通过方法签名区分 (方法重载)
    @Override
    public void print() {

    }
}
```


## 接口的默认方法和私有方法

```java
public interface A {

    // 实现类可以不实现默认方法, 可以覆盖默认方法
    default void method1() {
        System.out.println("method1");
        method2();
    }

    // jdk 9 开始支持私有方法
    // 私有方法允许在接口中定义只供接口本身调用的方法
    private void method2() {
        System.out.println("method2");
    }

    static void method3() {
        System.out.println("method3");
    }
}

public class B implements A {
}

A a = new B();

a.method1();
// method1
// method2

a.method3(); // 错误
A.method3(); // 只能接口名调用静态方法
```

## 接口的默认值

```java
在接口中的方法可以用default关键字指定默认实现

interface Example {
  String value() default ""; 
}

当实现类没有实现value方法时, 会使用默认实现并返回""。

提供默认的通用实现,简化客户端使用。
扩展接口时保持向后兼容。
```

## 函数式接口

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

## 子类和父类存在同名变量

```java
子类与父类存在同名变量

实例变量会隐藏父类变量, 两者不是同一个变量
静态变量不会被父类所隐藏, 两者不是同一个变量
TODO
```

## 类型擦除

- 将泛型参数的类型信息在编译时替换为对象类型
- 也就是将泛型类型“擦除”, 转换为普通的非泛型类型的代码






