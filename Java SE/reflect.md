
# 反射

## 反射

- 反射是在运行时动态访问类和对象的技术
- 在`java.lang.reflect`包下
- 基于反射实现参数配置，动态注入
- 反射需要消耗一定的资源

## 反射的核心类

- Class 类和接口
- Constructor 构造方法类
- Method 方法类
- Field 成员变量类 字段

## Class类

- Class 对象包含了某个特定类的结构信息
- 每个类都有一个 Class 对象
- 通过 Class 对象可以获取对应类的构造方法，方法，成员变量

## 获取Class对象

- `Class.forName(“类的全限定名”)`
- `类名.class`
- `对象名.getClass()`
- `包装类.TYPE`

## Class对象获取构造方法

- `getConstructors()` 获取所有公共构造方法
- `getDeclaredConstructors()` 获取所有构造方法
- `getConstructor(Class <?>... parameterTypes)` 获取指定参数的公共构造方法 例如 `getConstructor(String.class)`
- `getDeclaredConstructor(Class <?>... parameterTypes)` 获取指定参数的任意权限的构造方法

## Constructor对象常用方法

- `newInstance(Object ... initargs)` 使用该构造方法并指定参数创建和初始化新的实例 
- `void setAccessible(boolean flag)` `true` 表示反射对象在使用时禁止检查访问控制权限， `false` 表示反射对象在使用时应强制检查访问控制权限

## 获取公共构造方法实例化

```java
Class<Student> stu = Student.class;
Constructor<Student> con = stu.getConstructor(String.class);  
Student student = con.newInstance("abincaps");
```

## 获取私有构造方法实例化

```java
Class<Student> stu = Student.class;
Constructor<Student> con = stu.getDeclaredConstructor(String.class);  
con.setAccessible(true);  
Student student = con.newInstance("abincaps");  
```

## Class对象获取字段

- `getFields()` 获取所有可访问公共字段
- `Field [] getDeclaredFields()` 获取所有字段
- `Field  getField(String  name)` 获取指定公共成员字段
- `Field  getDeclaredField(String  name)` 获取指定任意权限成员字段

## Field对象常用方法

- `Object  get(Object  obj)` 返回指定对象上字段的值，基本类型会自动包装成包装类 等同于 `obj.getFieldName` FieldName 是字段名
`
## 给私有字段赋值

```java
Class<Student> stu = Student.class;  
Field age = stu.getDeclaredField("age");  
Student student = stu.getConstructor().newInstance();  
age.setAccessible(true);  
age.set(student, 10);  
```

## Class对象获取成员方法

- `Method [] getMethods()` 获取所有公共方法，包括父类继承的方法
- `Method [] getDeclaredMethods()` 获取所有已声明方法，不包括父类继承的方法

## Method类常用方法

- `Object invoke(Object  obj, Object ... args)` 在指定对象上调用指定参数的方法
- `void setAccessible(boolean flag)` `true` 表示反射对象在使用时禁止检查访问控制权限， `false` 表示反射对象在使用时应强制检查访问控制权限
- `<T extends Annotation> T getAnnotation(Class<T> annotationClass)` 如果找到指定的注解，则返回该注解的实例，否则返回 `null`

## 调用私有方法

```java
Class<Student> stu = Student.class;  
Method method = stu.getDeclaredMethod("method");  
method.setAccessible(true);  
Student student = stu.getConstructor().newInstance();  
method.invoke(student);
```

## 反射绕过泛型检测

```java
ArrayList<Integer> list = new ArrayList<>();  
list.add(10);  
Class<? extends ArrayList> listClass = list.getClass();  
Method add = listClass.getMethod("add", Object.class);  
add.invoke(list, "abincaps");  
System.out.println(list);
```

