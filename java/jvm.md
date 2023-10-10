# JVM

## JVM

- 广义上指是一种规范， 狭义上指 JDK 中的 JVM 虚拟机

## JVM架构

- 字节码文件加载进 类加载子系统
- 运行时数据区（Runtime Data Areas）包含 方法区（Method Area），堆区（Heap Area），栈区（Stack Area），PC寄存器（Register）， 本地方法栈（Native Method Area）
- 执行引擎 （Execution Engine）包含 解释器（interpreter），JIT编译器，垃圾回收器
- Java 本地接口 JNI (Native Method Interface)
- 本地方法库（Native Method Libray）

## 类加载器

- JVM 的类加载是通过 ClassLoader 及其子类来完成的
- 启动类加载器（Bootstrap ClassLoader）加载 `JAVA_HOME\lib` 目录下或通过 `-Xbootclasspath` 参数指定路径中的被虚拟机认可的类库
- 扩展类加载器 (Extension ClassLoader) 加载 `JAVA_HOME\lib\ext` 目录下或通过 `java.ext.dirs` 系统变量指定路径中的类库
- 应用程序类加载器 （Application ClassLoader）加载用户路径 `classpath` 上的类库
- 自定义加载器（User ClassLoader） 加载应用之外的类文件， 例如热部署工具

## 类加载器执行顺序

- 加载过程中会先查类是否已被加载，自底向上， 保证此类所有的 ClassLoader 只加载一次
- 自顶向下逐层尝试加载此类

## 类加载的时机

- 遇到 new， getStatic， putStatic， invokeStatic
- 使用 java.lang.reflect 包的方法时， 对类进行反射调用
- 初始化类时， 发现其父类还没有
- 当虚拟机启动时，用户需要指定一个主类 main，先将主类加载

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

## 双亲委派

- 当一个类加载器收到类加载任务， 会交给其父类加载器去完成
- 最终加载任务会传递到顶层的加载器中，只有父类加载器无法完成加载任务，才会尝试加载任务
- 双亲等同于父类

## 为什么需要双亲委派？

- 双亲委派可以避免重复加载核心的类，当父类加载器加载了该类时，子类加载器就不会再去加载

## 为什么要打破双亲委派？

- 存在 API 调用用户代码的情况需要打破双亲委派
- 打破 ClassLoader 的 loadClass 方法
- SPI， 父类委托子类加载器加载 Class， 例如 数据库驱动
- 热部署和不停机更新用的 OSGI 技术

## 打破双亲委派的方式

- 重写 ClassLoader 的 loadClass 方法
- 为了向前兼容， 保留 loadClass 方法，不推荐重写 loadClass 方法，推荐重写 findClass

## 运行时数据区

- 线程独享 程序执行区域
	- 虚拟机栈，本地方法栈，程序技术器
	- 不需要垃圾回收
	
- 线程共享 数据存储区域
	- 堆和方法区
	- 存储类的静态数据和对象数据
	- 需要垃圾回收

## 堆内存中的新生代和老年代

- 分代收集（Generational Collection）的理论进行设计
- 弱分代假说（Weak Generational Hypothesis）绝大多数对象都是出现很快消亡也很快
- 强分代假说（Strong Genrational Hypothesis）熬过越多次垃圾收集过程的对象越难以消亡
- 收集器将回收对象根据对象熬过垃圾收集过程的次数，分配到不同的区域之中存储
- 如果一个区域中的大多对象都很短命，难以熬过垃圾手机过程，把它们几种放在一起，每次回收只关注如何保留少量存活，而不是标记大量将要回收的对象
- 如果剩下的都是难以消亡的对象，把它们集中放在一起，以较低的频率回收这个区域

## JDK1.7内存模型

- Young 年轻区 保存年轻对象， 分为 1个 Eden 区 和 2个 Survivor 区
- Tenured 年老区 保存年长对象， 当对象在 Young 复制转移一定次数后，对象就会转移到 Tenured 区
- Perm 永久区 保存 class，menthod，field 对象， 一般不会溢出， 除非一次性加载很多类
- Virtual 区 最大内存和初始内存的差值

## JDK1.8内存模型

- 新生代 + 年老代
- 年轻代被划分为三个部分
	- Eden 区
	- 2 个 Survivor 区
- Perm 永久区 被替换成 Metaspace 元空间
- Metaspace 元空间所占用的内存空间不是在虚拟机内部，而是在本地内存空间中

## JDK1.9内存模型

- 取消新生代和老年代的物理划分，没有连续的区域
- 将堆划分成若干个区域， 这些区域包含了逻辑上的新生代和老年代

## 栈帧

- 栈帧（Stack Frame） 是用于支持虚拟机进行方法执行的数据结构
- 栈帧存储了方法的局部变量表，操作数栈，动态连接和方法返回地址
- 每一个方法从调用到执行完成的过程，都对应一个栈帧在虚拟机栈从入栈到出栈的过程
- 栈内存是线程私有空间，每个线程都会创建私有的栈内存空间，生命周期和线程相同
- 每一个方法在执行时，都会创建一个栈帧
- 栈内存大小决定了方法调用的深度

## 当前栈帧

- 位于 JVM 虚拟机栈顶的元素称为当前栈帧， 和这个栈帧相关连的方法称为当前方法，定义这个方法的类叫做当前鳄梨
- 执行引擎运行的所有字节码指令都只针对当前栈帧进行操作

## 创建栈帧

- 调用新的方法，新的栈帧也会创建，随着程序控制权转移到新方法，新的栈帧成为了当前栈帧
- 方法返回时，原栈帧会返回方法的执行结果给之前的栈帧（方法的调用者），虚拟机会丢弃此栈帧

## 栈异常

- 线程请求的栈深度大于虚拟机允许的深度（-Xss 默认 1m）， 会抛出 StackOverflowError 异常
- 在创建新的线程时，没有足够的空间区创建对应的虚拟机栈，有可能会抛出 OutOfMemoryError 异常

## 本地方法栈

- 本地方法栈和虚拟机栈类似， 虚拟机栈为虚拟机执行字节码服务， 本地方法栈为虚拟机使用到的 Native 方法服务

## 方法区

- 方法区（Method Area） 是可供各个线程共享的运行时内存区， 方法区本质上是编译后代码存储区域， 存储每个类的结构信息
- 运行时常量区，成员变量，方法数据，构造方法，普通方法的字节码指令
- 方法区的实现：永久代（PermGen）或 元空间 （Metaspace）

## 方法区存储的数据

- Class
	- 类型信息
	- 方法信息 （方法名称，方法参数列表，方法返回值）
	- 字段信息 （字段类型，字段名称需要特殊设置才能保存）
	- 类变量（静态变量） JDK1.7后转移到堆中存储
	- 方法表（方法调用的时候）查找合适的方法
	
- 运行时常量池 （字符串常量池）JDK1.7后转移到堆中存储
	 - 字面量类型
	 - 引用类型指向的内存地址
	 
- JIT编译器后的代码缓存

## 永久代和元空间

- JDK1.8 前方法区的实现是永久代，JDK1.8 及以后使用的方法区实现是元空间

- 存储位置不同
	- 永久使用的内存区域是JVM进程所使用的内存区域，大小受JVM大小限制
	- 元空间使用的内存区域是物理内存区域，元空间代使用的大小受物理内存大小限制
	
- 存储内容不同
	- 永久代存储的是方法区存储的数据
	- 元空间存储类的元信息，静态变量和运行时常量池移到堆中

## 为什么元空间替换永久代？

- 字符串存在永久代容易出现内存溢出和性能问题
- 类和方法的信息难以确定大小，不容易指定永久代的大小， 太小容易溢出，太大老年代容易溢出
- 永久代会给 GC 带来不必要的复杂度， 回收率低

## 常量池

- Class 文件常量池 一个 Class 文件 一个
- 运行时常量池 一个 Class 对象一个
- 字符串常量池 全局只有一个

## 符号引用

- Class，Method， Field

## 字符串常量池存储数据的方式

- 字符串常量池使用 StringTable 数据结构存储数据， 类似于 HashTable 哈希表

## 字符串常量池

- 使用双引号创建的字符串是常量，在编译器就确定存储在字符串常量池中
- 使用 `new String("")` 创建的对象是运行期创建的, 会存储到堆中
- 只包含字符串连接符的 `"a" + "b"` 是常量，在编译器就确定存储在字符串常量池中
- 包含变量的字符串连接 `"a" + str` 是运行期创建的, 会存储到堆中
- 运行期调用 String 的 intern() 方法向字符串常量池中动态添加对象

## PC 程序计数器

- 程序计数器（PC寄存器）当前线程执行的字节码指令的行号指示器
- 线程上下文切换后准确恢复执行位置
- 记录虚拟机字节码指令地址
- 不记录 Native 方法
- 异常 唯一没有 OOM 异常的区域

## 直接内存

- 直接内存不是虚拟机运行时数据区的一部分， 也不是 《Java虚拟机规范》中定义的内存区域
- NIO 引入了 Channel 和 Buffer 的 IO 方式，可以使用 native 方法直接分片对外内存，通过 DirectByteBuffer 对象可以操作直接内存
- 直接内存的分配不受到 Java 堆大小的限制， 受到总内存大小的限制

## 直接内存和堆内存

|          | 直接内存 | 堆内存 |
| -------- | -------- | ------ |
| 读写操作性能 | 好   | 差 |
| 分配空间性能 | 差   | 好 |

## 数据流的角度

- 非直接内存的作用链  本地IO -> 直接内存 -> 非直接内存 -> 直接内存 -> 本地IO
- 直接内存作用链 本地IO -> 直接内存 —> 本地IO

## 直接内存的使用场景

- 很大数据的存储， 生命周期长
- 频繁的IO操作， 网络并发场景

## 对象的创建流程

- new 指令
- 常量池检查类是否已加载
- 分配内存空间
- 内存空间初始化默认值
- 信息，保存在对象头
	- 对象类的元数据
	- 对象的哈希值
	- GC 分代年龄

## 对象的内存分配

 - Bump the Pointer 指针碰撞
	 - 内存地址连续
	 - Serial 和 ParNew 收集器
	 
 - Free List 空闲列表
	 - 内存地址不连续
	 - CMS 和 Mark-Sweep 收集器

## 内存分配的线程安全问题的解决办法

- CAS 是乐观锁的一种实现方式，JVM 采用 CAS 失败重试的方式保证更新操作的原子性
- TLAB Thread Local Allocation Buffer 本地线程分配缓冲，为每一个线程预先分配一块内存

## 对象进入老年代

- 新对象大多数默认进入新生代的 Eden 区
- Eden 区存储不下新分配的对象时，会触发 minor GC
- minor GC 后再放不下就进入老年代
- 老年代空间不够， 进行 Full GC
- Full GC 后再放不下 OOM
- 年龄太大，年龄是MinorGC次数 `-XX:MaxTenuringThreshold`指定，默认超过15次
- 动态年龄判断，MinorGC 后，发现 Survivor 区中的一批对象的总大小大于了这块 Survivor 区的 50%，将大于等于这批对象年龄最大值的所有对象直接进入老年代，希望可能是长期存活的对象，尽早进入老年代 `-XX:TargetSurvivorRatio`指定
- 大对象直接进入老年代, `-XX:PretenureSizeThreshold`指定，默认值是 0，不限制，一般设置为 1M

## JVM参数

- `-Xmx100m` 设置堆内存的最大值为 100MB
- `-Xms100m` 设置堆内存的初始值为 100MB
- `-XX:NewRatio=2` 年轻代的大小和老年代的大小 1:2 的比值
- `-XX:SurvivorRatio=2` 1 个 Survivor 区的大小和 Eden 区的大小 1:2 的比值
- `-XX:+PrintGCDetails` 打印 GC 详细信息

## 内存担保机制

- minor GC 后还存活的对象，存入到 Survivor 区
- 当无法存入到 Survivor 区时，此时会触发内存担保机制
- 触发内存担保机制时，将 Eden 区 minor GC 之后还存活的对象放入老年代
- 把新对象放入腾空的 Eden 区

## 对象内存布局

- 对象头
- 数据区
	- Instance Data 实例数据，对象内部的成员变量
- 对齐填充
	- 对象采用 8 bytes 对齐，不够 8 bytes 会自动补齐

## 对象头

- MarkWord 标记字段
	- 占 8 bytes
	- 默认存储：对象 HashCode、GC 分代年龄、锁状态
	- 存储对象自身的运行时数据，是 synchronized 实现轻量级锁和偏向锁的关键
	
- 类型指针
	- 占 8 bytes，默认开启指针压缩存储空间, 占 4 bytes
	- 对象指向类元数据的指针，JVM 通过这个指针来确定这个对象是哪个类的实例
	
- 数组长度
	- 占 4 bytes 对应 int 类型
	
- 对齐填充
	- 保证数组的大小是 8 bytes 的整数倍

## Object对象大小

- 对象头 12 bytes + 实例数据 0 bytes + 对齐填充 4 bytes = 16 bytes

## 访问对象

- 句柄
	- 对象被移动只要修改句柄中的地址
- 直接指针
	- 访问速度快，节省了一次指针定位的开销

## 垃圾

- 垃圾是没有被引用的对象

## 找到垃圾

- Reference Counting 引用计数法
- 根可达算法

## 引用计数法

- 消失一个引用计数减一，当引用都消失了，计数就会变为0，此时这个对象就会变成垃圾
- 引用计数法不能解决循环引用问题

## 引用关系

- 单一引用
- 循环引用
- 无引用

## GCRoots Tracing 根可达算法

- 又叫根搜索算法
- 通过一系列的名为“GCRoot”的对象作为起始点
- 从这些起始节点开始向下搜索，搜索所走过的路径称为 Reference Chain 引用链
- 当一个对象到 GCRoot 没有任何引用链相连时，说明此对象是不可用的，也就是不可达的

## 可作GCRoots的对象

- 栈中, 栈帧的本地变量表引用的对象
- 方法区中，类静态属性引用的对象
- 方法区中，常量引用的对象
- 本地方法栈中，JNI 引用的对象

## 回收过程

- 被判定不可达的对象至少要经历两次标记才会死亡
- 可达性分析后，发现没有与 GC Roots 相连接的引用链，会被第一次标记
- 第一次标记后，接着会进行一次筛选，筛选条件是此对象是否有必要执行 finalize() 方法，在 finalize() 方法中没有重新和引用链建立关联关系的，将被进行第二次标记

## 强引用

```java
Object obj = new Object();
```

## 软引用

- 非必须引用，内存溢出之前进行回收，内存还不够，才会抛异常
- 实现内存敏感的高速缓存

```java
SoftReference<Object> sf = new SoftReference<Object>(obj);
```

## 弱引用

- 弱引用比软引用更弱，在垃圾回收时更容易被回收
- 弱引用通常用于避免内存泄漏或实现某些缓存策略

```java
WeakReference<Object> wf = new WeakReference<>(obj);
```

## 虚引用

- 非必须引用，有 GC 就会被回收

## 无引用


