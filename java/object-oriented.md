# java面向对象

## static

```java
在静态方法里定义的变量就是静态的，不需再加static
在静态方法里不能访问实例变量

在实例方法里不能定义静态变量，但可以访问静态变量

类变量(class variable)是定义在类中, 被static修饰，也称为静态变量
静态变量在内存中只存在一份副本, 存在方法区的静态域中

实例变量(Instance Variable)是定义在类中的非静态变量
实例变量存储在堆内存的对象中

类似的
类方法 也叫静态方法
实例方法 
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

## 静态变量和实例变量的访问

```java
public class Student {
    public static String name = "abincaps";
    int age = 10;
}

// 在没new之前也可以访问类变量
System.out.println(Student.name); // 类名访问类变量 推荐
Student a = new Student();
System.out.println(a.name); // 对象名访问类变量 不推荐
System.out.println(a.age); // 实例变量只能用对象名访问
```

## tip 3

```java
记录创建了多少个类

public class Student {
    public static int count = 0;
    public Student() {
        count++;
    }
}

new Student();
new Student();
System.out.println(Student.count); // 2
```

## tip 4

```java
public class Student {
    // 类方法
    public static void print() {
        System.out.println("Student");
    }

    // 实例方法
    public void method() {
        System.out.println("method");
    }
}

// 类名调用类方法 
Student.print(); // Student
Student a = new Student();
// 类方法调用
a.method(); // method

类方法最常见的应用场景是做工具类 
建议将工具类的构造器进行私有来阻止创建对象
java.lang包下的Math类是典型的工具类
```

## tip 5

```java

类成员指类变量和类方法

public class Student {

    // 静态方法
    public static void print() {
        System.out.println(age); // 正确 类方法可以放访问类成员
        System.out.println(name); // 错误 类方法不能访问实例成员
        System.out.println(get()); // 错误 静态方法不能调用实例方法
        System.out.println(this.name); // 错误 只有实例方法中可以出现this
    }

    static int age = 10; // 静态成员
    String name = "abincaps"; // 实例成员

    public String get() {
        return name;
    }

    private Student() {

    }
}

// 报错 non-static method get() cannot be referenced from a static context

实例方法可以访问类成员，也可以访问实例成员
```

## tip 6

```java
public class Student {

    static {
        // 静态代码块在类加载时执行,并且只执行一次
        // 主要用于在类加载时进行静态资源的初始化
        // 静态代码块只能访问类中的静态成员
        System.out.println("static");

    }

    {
        // 实例代码块在创建对象时运行, 每创建一个对象, 就执行一次该代码块
        // 实例代码块运行顺序在构造器前
        System.out.println("non-static");
    }

    public Student() {
        System.out.println("Student");
    }
}

new Student();
new Student();

// 可以看到初始化顺序
// static
// non-static
// Student
// non-static
// Student

```

## tip 7

```java
单例设计模式 要求一个类只能有一个实例

public class Foo {
    // 饿汉式单例模式 在类装载时就完成实例化
    private static Foo a = new Foo(); 

    // 必须私有构造器 外部无法直接实例化
    private Foo() {

    }
    // 调用静态方法(类方法）来返回已创建的实例
    public static Foo getObject() {
        return a;
    }
}


public class Foo {

    private static Foo a;
    
    private Foo() {

    }

    // 懒汉式单例模式
    public static Foo getObject() {
        // 静态成员变量在第一次被使用时进行初始化
        if (a == null) {
            a = new Foo();
        }
        return a;
    }
}
```

## 继承

```java
java只支持单继承, 一个类只能继承一个直接父类，支持多层继承 c继承b， b继承a
子类(派生类)可以继承父类(基类或超类) 的非私有成员(成员变量，成员方法)

public class Base {
    public int a;

    public void print1() {
        System.out.println("public");
    }

    private int b;

    private void print2() {
        System.out.println("private");
    }
}
```

## 访问权限修饰符

```java
private 在本类中（在同一类内才可见）
缺省 满足private， 且同一个包下的其他类 （对同一包内的类才可见）
protected 满足缺省， 且在任意包下的子类里 （同一包内的类才可见，所有子类可见）
public 满足protect，且在任意包下的任意类里
```

## tip 10

```java
Object类是所有类的基类, 默认继承Object类

// 下面相同两句
System.out.println(a.toString());
System.out.println(a); //  默认调用toString

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

## 方法重写(覆盖)

```java
public class Base {
    protected void print() {
        System.out.println("Base");
    }
}

public class Derived extends Base {

    // 方法重写 子类的方法必须与父类被重写的方法有相同的方法名和参数列表
    // 子类方法的访问权限必须大于等于父类方法
    // 当然父类的访问权限不能是private 因为在子类中不可见
    // 静态方法不能被重写
    // @Override注解 检查重写方法是否正确
    @Override
    public void print() {
        System.out.println("Derived");
    }
}

Derived a = new Derived();
a.print(); // Derived
```

## 方法重写中的异常

```java
方法的重写(Override)中
子类方法不允许抛出比父类更广泛的异常
如果父类方法没有throws声明, 则子类重写方法也不能有throws声明(除非是RuntimeException及其子类)
父类方法throws的异常, 子类必须处理或者再次声明throws
如果可以捕获处理异常，子类方法可以不再声明throws
```

## tip 12

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

    // 方法的访问也和上面类似
}
```

## tip 13

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
        // 构造器不能被重写(Override)
        System.out.println("Derived");
    }
}

// 子类构造器会先调用父类的无参构造器，再执行自己
new Derived();
// Base
// Derived
```

## tip 14

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

```java
// 声明为final的类不能被继承,该类不能作为父类
public final class Base {
}

// 错误 不能继承
public class Derived extends Base {
}


public class Base {
    public final void method() {
    }
}

public class Derived extends Base {
    
    // 声明为final的方法不能被子类重写
    @Override
    public final void method() {
    }
}

final int b = 10;
b = 10; // 不能进行二次赋值

// final static 修饰的变量必须直接赋值 称为常量
final static int a = 0;

final修饰的引用变量，其地址不可以改变，但是可以通过地址修改对象内容

在类中使用final修饰的变量, 必须在声明时或者构造函数中初始化
```

## 抽象类

```java
// 抽象类不能实例化， 必须继承后才能实例化
// 可以包含抽象方法和非抽象方法
// 子类继承抽象类时,必须实现抽象类的所有抽象方法
// 如果子类没有实现父类所有的抽象方法, 那么子类也必须声明为抽象类
public abstract class Foo {

    // 有构造器也不能实例化
    // 构造器不能被声明为 abstract
    // 构造器的目的是创建对象, 假如构造器是抽象的, 就无法实例化该类的对象
    public Foo() {

    }

    // 有抽象方法的类必须声明为抽象类
    // 抽象方法只包含方法签名, 没有方法体, 也就是不实现
    public abstract void run();
}
```

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

接口可以继承
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

## tip 18

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

## tip 19

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

```java
values方法返回一个包含此枚举类型常量的数组
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

## tip 101

```java
TODO
子类与父类存在同名变量,这种情况下:

实例变量会隐藏父类变量,但两者不是同一个变量。
静态变量也不会被父类所隐藏,两者也不是同一个变量。
```

## tip 102

```java
TODO
类型擦除
```
