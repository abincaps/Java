
# java.lang

- Class 类的实例代表运行时的类
- Void 类是一个不可实例化的类
- Math 类提供常用的数学函数
- String 类，StringBuffer 类，StringBuilder 提供对字符串的操作
- ClassLoader 类管理类的动态加载
- Process
- ProcessBuilder
- Runtime
- SecurityManager
- System
- Throwable 类包含可能由 `throw` 语句抛出的对象， Throwable 类的子类表示错误和异常

## Object类

- `Object`类是类层次结构的根
- `Object`类是所有类的基类，默认继承`Object`类
- 所有对象，包括数组，都继承了`Object`类的方法

## Object类常用方法

- `String toString()` 返回对象的字符串表示形式， 建议所有子类重写此方法， 默认返回`getClass().getName() + "@" + Integer.toHexString(hashCode())`
- `boolean equals(Object obj)` obj 对象是否“等于”这个对象， 默认返回`this == obj`

## System类

- `final class System` System 类不能被实例化 

## System类常用字段

- `static final InputStream in` 标准输入流，用户通过键盘输入的数据流
- `static final PrintStream out` 标准输出流
- `static final PrintStream err` 标准错误输出流

## System类常用方法

- `static void exit(int status)` 终止当前运行的 Java 虚拟机， status 作为退出状态码，非零状态码表示异常终止，相当于调用 `Runtime.getRuntime().exit(status);` 
- `static long currentTimeMillis()` 返回当前时间(以毫秒为单位), 当前时间与 UTC 时间 1970 年 1 月 1 日 0:00 之差

## Integer类

- `final class Integer extends Number implements Comparable<Integer>, Constable, ConstantDesc`

## Integer类常用字段

- `static final int MAX_VALUE = 0x7fffffff` 2^31-1
- `static final int   MIN_VALUE = 0x80000000` -2^31
- `static final Class<Integer> TYPE` 代表原始类型`int`的`Class`实例

## Integer类常用方法

- Integer 的构造方法已弃用
- `static Integer valueOf(int i)` 返回指定`int`值的`Integer`实例
- `static Integer valueOf(String s)` 返回指定`String`值的`Integer`实例
- `static Integer valueOf(String s, int radix)` radix 是进制转换你

## String类

- 字符串常量也可以调用 String 方法
- `final class String`
- String 对象是不可变的

## String类常用方法

- `String(byte[] bytes, int offset, int length)` `bytes` The bytes to be decoded into characters, `offset` The index of the first byte to decode, `length` The number of bytes to decode
- `int length()` 返回此字符串的长度
- `boolean equals(Object anObject)` 与指定对象进行比较
- `char charAt(int index) ` 返回指定索引 index 处的 char 值
- `int compareTo(String anotherString)` 按字典顺序比较两个字符串, 相等返回 0， 小于 anotherString 返回小于0的整数， 大于 anotherString 返回大于0的整数
- `String concat(String str)` 将指定的字符串连接到末尾
- `byte[] getBytes()` 返回字节序列，使用平台的默认字符集
- `byte[] getBytes(String  charsetName)` 返回指定字符集的字节数组
- `int indexOf(String str)` 返回指定字符串第一次出现的索引位置
- `String[] split(String regex)` 指定正则表达式的匹配项进行拆分字符串
- `String substring(int beginIndex, int endIndex)` 返回子字符串 从指定的 `beginIndex` 开始到 `endIndex - 1` 处的字符串，半闭半开区间\[beginIndex, endIndex)
- `String substring(int beginIndex)` Returns a string that is a substring of this string. The substring begins with the character at the specified index and extends to the end of this string.
- `char[] toCharArray()` 将此字符串转换为新的 char 数组
- `String trim()` 去除所有的前导空格和后缀空格
- `boolean isEmpty()` length 等于 0 时返回`true`， 否则返回`false`
- `boolean isBlank()` 为空或仅包含空白返回`true`， 否则返回`false`
- `boolean contains(CharSequence s)` 是否包含指定 char 值序列
- `boolean startsWith(String prefix)` 是否以指定的前缀开头
- `boolean endsWith(String suffix)` 是否以指定的后缀结尾
- `String replace(CharSequence target, CharSequence replacement)
`
## StringBuilder类

- 可变的字符序列
- StringBuilder 比 StringBuffer 快

## StringBuilder类构造方法

- `StringBuilder()` 没有字符且初始容量为 16 个字符 
- `StringBuilder(int capacity)` 没有字符且初始容量由`capacity`参数指定
- `StringBuilder(String str)` 指定字符串初始化

## StringBuilder类常用方法

- `StringBuilder append(String str)`  将指定的字符串附加到末尾
- `StringBuilder reverse()` 反转字符序列
- `int capacity()` 返回当前容量
- `String toString()` 返回表示此字符序列的字符串

# abstract class ClassLoader

- A class loader is an object that is responsible for loading classes.
- Every Class object contains a reference to the ClassLoader that defined it.
- `URL getResource(String name)` Finds the resource with the given name. The name of a resource is a '/'-separated path name that identifies the resource.

# class Thread

- class Thread implements Runnable
- `ClassLoader getContextClassLoader()` Returns the context ClassLoader for this thread.

# class Class\<T>

- `boolean isAnnotationPresent(Class<? extends Annotation> annotationClass)` Returns true if an annotation for the specified type is present on this element, else false. 
- `native boolean isAssignableFrom(Class<?> cls)` Determines if the class or interface represented by this Class object is either the same as, or is a superclass or superinterface of, the class or interface represented by the specified Class parameter. It returns true if so; otherwise it returns false.
- `Field[] getDeclaredFields()` Returns an array of Field objects reflecting all the fields declared by the class or interface represented by this Class object. 
- `String getSimpleName()` Returns the simple name of the underlying class as given in the source code. An empty string is returned if the underlying class is anonymous.

# class Field

- `Class<?> getType()` Returns a Class object that identifies the declared type for the field represented by this Field object.
- `void set(Object obj, Object value)` Sets the field represented by this Field object on the specified object argument to the specified new value. 
- `<T extends Annotation> T getAnnotation(Class<T> annotationClass)` Returns this element's annotation for the specified type if such an annotation is present, else null.