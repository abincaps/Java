# java.io

## 换行符

- windows `\r\n`
- linux `\n`
- mac `\r`

## printf中的格式字符串

- `%n` 换行
- `%b` boolean

## 文件路径分隔符

- Window `\\` 
- 类 Unix `/` 

## 三代IO框架

- 第一代 流式阻塞 IO
- 第二代 NIO 是非阻塞的
- 第三代 NIO AIO asyncIO 异步 IO

## File类

- 文件和目录路径名的抽象表示

## File类构造方法

- `File(String pathname)` 指定路径名来创建 
- `File(String parent, String child)` 指定父路径名和子路径名创建

## File类常用字段

- `static final String separator` 系统相关的文件路径分隔符
- `static final char separatorChar` 系统相关的文件路径分隔符

## File类常用方法

- `boolean mkdir()` 创建单级目录
- `boolean mkdirs()` 创建多级目录
- `boolean isDirectory()` 是否是目录
- `boolean isFile()` 是否是文件
- `boolean exists()` 文件或目录是否存在
- `File getAbsoluteFile()` 返回绝对路径
- `String getPath()` 返回路径名
- `String getName()` 返回文件或目录的最后一个名称

## InputStream抽象类

- InputStream 抽象类是所有表示字节输入流的类的超类

## InputStream类的常用方法

- `int read()` 返回从输入流中读取下一个字节的数据, 如果到达流的末尾而没有可用字节，则返回 -1
- `int read(byte b[])` 从输入流中读取一定数量的字节存储到缓冲区字节数组 `b` 中， 返回实际读取的字节数

## FileOutputStream类

- 文件输出流用于写入原始字节流

## FileOutputStream类的构造方法

- `FileOutputStream(String name)` 用来写入指定文件名的文件
- `FileOutputStream(File file)` 用来写入由指定的`File`对象的文件
- `FileOutputStream(File file, boolean append)` append 参数如果是 `true` ，则将写入文件末尾而不是开头

## FileInputStream类

- 从文件中读取原始字节流

## BufferedOutputStream类

- 实现缓冲输出流

## BufferedOutputStream类构造方法
 `
- `BufferedOutputStream(OutputStream out)` 默认缓冲区大小 8192（8KB）
- `BufferedOutputStream(OutputStream out, int size)` size 参数指定缓冲区大小

## Writer类

- 写入字符流的抽象类

## Writer类的常用方法

- `void write(String str)` 写一个字符串

## InputStreamReader类

- 字符输入流

## InputStreamReader类的构造方法

- `InputStreamReader(InputStream in, String charsetName)` charsetName 参数指定字符集

## FileReader类

- 简化实例化过程
- 只有构造方法
- `class FileReader extends InputStreamReader`

## FileReader类构造方法

- `FileReader(String fileName)` 指定要读取的文件名(路径名)

## BufferedReader类

- 带有缓冲区的字符输入流

## BufferedReader类构造方法

- `BufferedReader(Reader in)` 使用默认输入缓冲区大小 8192（8KB）
- `BufferedReader(Reader in, int sz)` 使用 sz 参数指定输入缓冲区的大小

## BufferedReader类常用方法

- `String readLine()` 读取一行文本

## BufferedWriter类常用方法

- `void newLine()` 写入与系统相关的行分隔符

## ObjectOutputStream类

- 将对象序列化为字节流
- 只有实现 java.io.Serializable 接口的对象才能写入流
- `serialVersionUID` 是序列化机制中的一个标识符，用于确定一个类的版本是否与序列化的实例的版本匹配, 防止类改变导致的问题
- 成员变量被`transient`修饰不会被序列化

```java
class Foo implements Serializable {
	private static final long serialVersionUID = 1L;
}
```

## ObjectOutputStream类常用方法

-  `void writeObject(Object obj)` 序列化 obj 对象