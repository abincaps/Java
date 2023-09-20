
# OOP 面向对象编程 

- 三大特征：封装，继承，多态

## Method 方法

- 形参 方法定义时带有数据类型声明的参数，形参是局部变量
- 实参 方法调用时不带有数据类型声明的参数

## Method Signature 方法签名

- 方法签名是 方法名 + 参数列表（参数的数量，类型，顺序），返回值和异常不属于方法签名

## Method Signature 方法重载

- 方法名相同，参数类型列表不同就是方法重载，和返回值，异常无关

## public class

- 一个代码文件（编译单元）中可以写多个 class，但只能有一个 class 被`public`修饰
- 被`public`修饰的类的类名必须和文件名一致
- 没有私有类，只有内部私有类

## 类和对象的关系

- 类是对象的模板， 对象是类的实例化

## 成员变量

- 成员变量在类内，方法体外声明
- 成员变量有默认值，`0`，`null`

## 局部变量

- 局部变量在方法体内（包括方法的参数）声明
- 局部变量没有默认值

## 构造器（构造方法）

- 构造器没有返回值
- 构造器名称需要和类名一致
- 构造器支持重载
- 如果没有写任何构造器，类会自动生成一个无参构造器
- 无参构造器的术语是 zero-argument constructor（零参构造器）
- 如果写了构造器（无论是否有参数）， 类不会自动生成无参构造器
- 构造器不能自己调用自己，会形成死循环
- 构造器不能被重写

## 静态成员和实例成员的调用

- 静态成员被`static`修饰
- 实例成员不被`static`修饰
- 静态成员通过对象名调用（不推荐）或类名调用（推荐）
- 实例成员只能通过对象名调用

## 静态方法

- 静态方法被`static`修饰
- 静态方法只能访问静态成员
- 静态方法里定义的变量默认是静态的，不需额外加`static`修饰
- 静态方法中没有`this`自引用

## 实例方法

- 实例方法不被`static`修饰
- 实例方法可以访问静态成员和实例成员
- 在实例方法里不能定义静态变量

## 静态代码块 

- 静态代码块在类加载时执行, 并且只执行一次
- 静态代码块只能访问静态成员

## 非静态代码块

- 非静态代码块在创建对象时运行, 每创建一个对象就执行一次该代码块
- 非静态代码块运行顺序在构造器前

## 前向引用

- 代码中使用尚未声明或定义（在声明或定义之前）的变量，方法，类

## 非法的前向引用

```java
class Foo {  
    static {  
        a = 10;  // 不报错
        System.out.println(a); // 报错  
    }  
    static int a;  
}
```

## 类的初始化顺序

1. 父类static代码块（只会加载一次）
2. 子类static代码块（只会加载一次）
3. 父类non-static代码块
4. 父类构造器
5. 子类non-static代码块
6. 子类构造器

## 子类和父类

- 子类又叫派生类
- 父类又叫基类或超类

## 继承

* 只支持单继承（一个类只能继承一个直接父类）, 不支持多继承
* 支持多层继承 例如 c 继承 b，b 继承 a
* 子类可以继承父类的非私有成员(成员变量和成员方法)

## this关键字

- `this`只能在非静态方法中使用
- `this`不能同时调用两次构造器
- `this`解决了局部变量与成员变量同名的情况(命名冲突问题)

## super关键字

- `super`访问父类的成员(字段, 方法, 构造方法)

## this和super

- 如果不指定`this`或`super`会优先局部变量

## 子类构造器

- 子类构造器会默认先调用父类的无参构造器，再执行自己
- 如果父类的无参构造器不存在，需要显示指定父类构造器`super(参数)`, 否则会报错
- 构造器不能被重写

```java
public class Base {
    public Base() {
    }
}

public class Derived extends Base {
    public Derived() {
        super(); // 写不写都会默认调用父类的无参构造器
    }
}
```

## 访问权限修饰符

| 关键字    | 访问权限范围                                 |
| --------- | -------------------------------------------- |
| private   | 在本类中可见                                 |
| default      | 在同一包下的类可见                           |
| protected | 在同一包下的类可见，且在任意包下的子类里可见 |
|public           | 在任意包下的任意类可见                                             |

## Object类

- `Object`类是所有类的基类(默认继承`Object`类)

## 方法重写(覆盖)

* 子类重写的方法必须与父类被重写的方法有相同的方法签名
* 重写方法的返回值类型必须是被重写方法返回值的类型或子类型
* 子类方法的访问权限必须大于等于父类方法（不能是`private`修饰，因为在子类中不可见），子类的访问权限不能比父类更加限制
* 静态方法不能被重写

## 方法重写中的异常

* 如果父类方法没有声明`throws`, 子类重写方法也不能有`throws`声明(`RuntimeException`及其子类除外)
* 子类重写父类方法不允许抛出比父类方法更宽泛的异常

## 多态

- 父类的引用变量可以指向子类的对象
- 多态调用成员方法，编译看左，运行看右（在编译时只知道变量的父类型, 在运行时才确定具体对象）
- 多态调用成员属性编译运行都看左（变量没有多态特性, 只有方法才有多态特性）

## 向上转型 

- 子类对象可以自动转型为父类对象
- 向上转型一定是安全的
- 无法调用子类特有的方法（子类有的方法在父类中没有）

## 向下转型

- 需要强制类型转换来还原子类对象
- 使用`instanceof`避免错误

## instanceof

- `obj instanceof Class` 测试一个对象是否为一个类的实例, obj 是对象名，Class 是类名

## 抽象类

- 被`abstract`修饰的类是抽象类
- 抽象类不能实例化，即使声明了构造器
- 构造器不能被`abstract`修饰
- 抽象类可以包含抽象方法和非抽象方法

## 抽象方法 

- 被`abstract`修饰的方法是抽象方法
- 抽象方法没有方法体（也就是没有具体实现）

## 抽象类和抽象方法 

- 有抽象方法的类必须声明为抽象类
- 子类继承抽象类时, 必须实现抽象类的所有抽象方法，否则子类必须声明为抽象类

```java
abstract class Foo {
	abstract void method();
}
```

## 接口

- 接口不能实例化
- 接口可以单继承或多继承
- 接口中的变量都是常量 (默认被`public static final`修饰)，必须直接赋值初始化
- 方法都是抽象方法（默认被`public abstract`修饰），不带方法体

```java
public interface Foo {
}
```

## 实现类

- 一个类可以`implements`（实现）多个接口
- 实现类必须重写接口中的所有抽象方法，否则必须声明为抽象类

```java
public class Foo implements A, B {
}
```

## 接口冲突

- 如果多个接口存在重复的方法，只需要提供一个方法的具体实现即可，不需要多次实现
- 接口多继承时，如果多个接口存在重复的默认方法，需要对重复的默认方法进行重写

```java
interface A {  
    default void method() {  
    }  
}  
  
interface B {  
    default void method() {  
    }  
}  
  
interface C extends A, B {  

    @Override  
    default void method() {  
    }  
}

class D implements A, B {  
    @Override  
    public void method() {  
    }  
}
```

## 继承和实现接口

- 在继承和实现接口中，父类中的方法和接口中的方法重名，优先执行父类中的方法，除非方法被重写

```java
interface A {  
    void method();  
}  
  
class B {  
    public void method() {  
    }  
}  
  
class C extends B implements A {  
}
```

## 接口的默认方法

- 在接口中的方法可以用`default`关键字指定默认实现
- 默认方法必须要有方法体
- 实现类可以直接调用默认方法，不用实现
- 实现类可以重写默认方法
- 需要 JDK 1.8 版本及以上

```java
public interface Foo {  
    default void Method() {  
    }  
}
```

## 接口的私有方法

- 私有方法允许在接口中定义只供接口本身调用的方法
- 需要 JDK 9 版本及以上

## 接口的静态方法

- 需要 JDK 1.8 版本及以上
- 通过接口名直接调用

```java
interface Foo {  
    static void method() {  
    }  
}

Foo.method();
```

## final

- 被`final`修饰的类不能被继承
- 被`final`修饰的方法不能被子类重写
- 接口中的抽象方法不能被`final`修饰
- 被`final`修饰的变量必须在声明时或构造器或代码块中赋值初始化
- 被`final`修饰的引用变量无法重新赋值，但可以修改对象的内容

## static final

- 被`static final`修饰的变量都是常量必须直接初始化

## private和final

- 类中的任何 private 方法都是隐式 final

## 内部类

- 内部类可以访问外部类成员（包括私有成员）
- 外部类要访问内部类的非静态成员必须要创建对象
- JDK 16 开始支持内部类定义静态成员

```java
public class Outer {
    int a = 3;
    public class Inner {
	    public static int a; // jdk 16 开始支持定义静态成员
        int a = 2;
        public void print() {
            int a = 1;
            // 解决重名冲突
            System.out.println(a); // 1
            System.out.println(this.a); // 2
            System.out.println(Outer.this.a); // 3
        }
    }
}
```

## 内部类的创建

- `Outer.Inner inner = new Outer().new Inner();`

```java
class Outer {  
    class Inner {  
    }  
    void method() {  
        Inner inner = new Inner();    
    }  
}
```

## 成员内部类

- 成员内部类在类的成员位置

```java
```java
class Outer {  
    class Inner {  
    }  
}
```

## 局部内部类

- 局部内部类在外部类的方法中
- 局部内部类不能写访问权限修饰符

```java
class Outer {  
    int a = 0; 
    
    void method() {  
        int a = 1;  
         class Inner {  
             void print() {  
		        // 优先访问方法中的局部变量
                 System.out.println(a);  
             }  
        }  
  
        Inner inner = new Inner();  
        inner.print(); // 1  
    }  
}
```

## 静态内部类

- `static`不可以修饰外部类, 但可以修饰内部类
- 静态内部类是一个与外部类无关的独立类，不持有外部类的 this 引用(不依赖于外部类的实例)
- 静态内部类可以直接访问外部类的静态成员
- 静态内部类不能直接访问外部类的非静态成员，需要先实例化外部类, 再通过外部类实例才能访问
- 静态内部类可以在外部类没有实例化的情况下直接使用 new 进行实例化


```java
class Outer {  
    static class Inner {  
        static void staticMethod() {  
        }  
  
        void method() {  
        }  
    }  
}

Outer.Inner.staticMethod(); // 访问静态内部类的静态方法

Outer.Inner inner = new Outer.Inner();  
inner.method(); // 访问静态内部类的实例方法
```

## 匿名内部类

- 匿名内部类本质上是继承父类或实现接口
- 不能使用访问修饰符和`static`修饰

```java
class A {  
    public void method() {  
    }  
}  
  
interface B {  
    void method();  
}

new A().method();
new B() {  
    @Override  
    public void method() {  
    }  
}.method();
```

## 枚举类

- 枚举类是常量的集合
- 枚举类是被`final`隐式修饰的类，不可以被继承
- 枚举类会隐式继承 Enum 抽象类
- 枚举类可以在类的内部声明, 也可以作为独立的类声明
- 枚举类的构造器默认私有（单例模式）
- 枚举类可以包含方法
- `values()` 方法返回一个包含此枚举类型常量的数组

```java
enum Color {  
  
    // 第一行只能定义常量  
    // 默认从0开始数  
    RED, GREEN, BLUE;  
  
    void method() {  
    }  
}

Color[] values = Color.values();  

System.out.println(Color.RED); // RED  
  
Color red = Color.valueOf("RED");  
  
System.out.println(red.ordinal()); // 0  
  
Color.RED.method();
```
