# 设计模式

## 单例设计模式

- 单例设计模式 要求一个类只能有一个实例

```java
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

