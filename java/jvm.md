# JVM

## JVM

- 广义上指是一种规范， 狭义上指JDK中的JVM虚拟机
- “编写一次到处运行”的理念在JavaScript上得到了实现

## JVM架构

- 字节码文件加载进 类加载子系统
- 运行时数据区（Runtime Data Areas）包含 方法区（Method Area），堆区（Heap Area），栈区（Stack Area），PC寄存器（Register）， 本地方法栈（Native Method Area）
- 执行引擎 （Execution Engine）包含 解释器（interpreter）， JIT编译器，垃圾回收器
- Java 本地接口 JNI (Native Method Interface)
- 本地方法库 （Native Method Libray）

## 类加载器

- JVM的类加载是通过 ClassLoader 及其子类来完成的
- 启动类加载器（Bootstrap ClassLoader）加载 `JAVA_HOME\lib` 目录下或通过 `-Xbootclasspath` 参数指定路径中的被虚拟机认可的类库
- 扩展类加载器 (Extension ClassLoader) 加载 `JAVA_HOME\lib\ext` 目录下或通过 `java.ext.dirs` 系统变量指定路径中的类库
- 应用程序类加载器 （Application ClassLoader）加载用户路径 `classpath` 上的类库
- 自定义加载器（User ClassLoader） 加载应用之外的类文件， 例如热部署工具

## 类加载器执行顺序

- 加载过程中会先查类是否已被加载，自底向上， 保证此类所有的 ClassLoader 只加载一次
- 自顶向下逐层尝试加载此类

## 类加载的时机

- 遇到 new， getStatic， putStatic， invokeStatic
- 使用java.lang.reflect包的方法时， 对类进行反射调用
- 初始化类时， 发现其父类还没有
- 当虚拟机启动时，用户需要指定一个主类main，先将主类加载

## 类的生命周期

- 验证 -> 准备 -> 解析 -> 初始化 -> 使用 -> 卸载

## 类加载的过程

- 类的全限定名 定位到 class 文件 -> 二进制字节流加载 class
- 字节流静态数据 -> 方法区（永久代，元空间）
- 创建字节码 Class 对象

## 类加载途径

- jar / war
- jsp 生成的 class
- 数据库中的二进制字节流
- 网络中的二进制字节流
- 动态代理生成的二进制字节流

## 自定义类加载器

```java
// 生成class文件后放在指定路径
package com.jvm.classloader;  
public class Test {  
    public void say() {  
        System.out.println("hello");  
    }  
}
```

```java
package com.jvm.classloader;  
  
import java.io.*;  
  
public class MyClassLoader extends ClassLoader {  
    private String classpath;  
  
    public MyClassLoader(String classpath) {  
        this.classpath = classpath;  
    }  
  
	// 需要重写findClass
    // 入参 String name 类的全限定名 
    @Override  
    protected Class<?> findClass(String name) {  
  
        try { 
            byte[] classDate = getDate(name);  
  
            if (classDate != null) {  
                // 将字节数组数据 转换为 字节码对象  
                return defineClass(name, classDate, 0, classDate.length);  
            }  
  
        } catch (IOException e) {  
            e.printStackTrace();  
        }  
  
        return null;  
    }  
  
    private byte[] getDate(String className) throws IOException {  
	    // 生成全路径path 
        String path = classpath + File.separator + className.replace(".", File.separator) + ".class";  
  
        try (InputStream in = new FileInputStream(path);  
             ByteArrayOutputStream out = new ByteArrayOutputStream()  
        ) {  
			
            byte[] buffer = new byte[2048];  
  
            int len = 0;  
  
            while ((len = in.read(buffer)) != -1) {  
                out.write(buffer, 0, len);  
            }  
  
            return out.toByteArray();  
  
        } catch (FileNotFoundException e) {  
            e.printStackTrace();  
        }  
        return null;  
    }  
}
```

```java
package com.jvm.classloader;  
  
import java.lang.reflect.Method;  
  
public class TestMyClassLoader {  
  
    public static void main(String[] args) throws Exception {  
		// windows C:\\lib
        MyClassLoader myClassLoader = new MyClassLoader("C:\\lib");  
  
        Class<?> c = myClassLoader.loadClass("com.jvm.classloader.Test");  
  
        if (c != null) {  
            Object obj = c.getDeclaredConstructor().newInstance();  
            Method method = c.getMethod("say", null);  
            method.invoke(obj, null);  
            System.out.println(c.getClassLoader().toString());  
        }  
    }  
}
```

