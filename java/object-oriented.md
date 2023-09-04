
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

{% hint style="info" %}
values方法返回一个包含此枚举类型常量的数组
{% endhint %}

## 接口

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

{% hint style="info" %}
接口可以继承
{% endhint %}

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

将泛型参数的类型信息在编译时替换为对象类型

也就是将泛型类型“擦除”, 转换为普通的非泛型类型的代码
