# java IO流

## 文件路径分隔符

```java
import java.io.File

...

// 文件路径
// window分隔符是用的 \ 反斜杠
// 类unix 是 /
// File.separator 对应系统的文件路径分隔符
// File.pathSeparator 对应系统的环境变量的路径分隔符

// win下
System.out.println(File.separator); // \
System.out.println(File.pathSeparator); // ;
```

## 三代IO框架

```java
第一代流式阻塞IO
第二代 NIO 是非阻塞的
第三代 NIO AIO asyncIO 异步IO
```

## 文件写入

```java
import java.io.*;
import java.nio.charset.StandardCharsets;
import java.util.Scanner;


public class Test {

    private static final Scanner in = new Scanner(System.in);

    public static void main(String[] args) throws IOException {

        File target_file = createFile();

        writeToFile(target_file);

        System.out.println("finished");
    }
    
    private static void writeToFile(File target_file) throws IOException {

        // try-with-resources
        // 在try语句中声明所要使用的资源对象
        // 被声明的资源类必须实现java.lang.AutoCloseable接口
        // try代码块结束后,会自动关闭这个资源对象,即调用其close()方法
        // 无需在finally块中手动关闭资源
        try (

                // FileOutputStream 是用于向文件中写入数据的输出流
                FileOutputStream fos = new FileOutputStream(target_file);
                // OutputStreamWriter是一个字符流类 使用指定的字符集将字符写入到字节流
                OutputStreamWriter osw = new OutputStreamWriter(fos, StandardCharsets.UTF_8);
                // PrintWriter类允许方便地打印格式化字符串到文本输出流
                PrintWriter pw = new PrintWriter(osw);


        ) {

            System.out.println("输入的内容会写入文件， 输入空格结束");

            while (true) {
                // trim 返回一个字符串，其值为该字符串，删除了所有前导和尾随空格
                String line_to_write = in.nextLine().trim();
                System.out.println("输入内容：" + line_to_write);

                if (line_to_write.trim().isBlank()) {
                    System.out.println("输入结束");
                    break;
                } else {
                    pw.println(line_to_write);
                    pw.flush(); // 刷新缓存
                }
            }

        }

    }

    private static File createFile() throws IOException {
        System.out.println("输入文件名：");
        String file_name = in.nextLine();
        File f = new File("." + File.separator + file_name + ".txt");
        // isFile返回 true 当且仅当此路径名表示的文件存在时 并且是普通文件 否则返回false
        if (f.isFile()) {
            // delete删除此路径名表示的文件或目录 返回true当且仅当文件或目录被成功删除, 否则返回false
            System.out.println("目标文件存在，删除" + f.delete());
        }

        // 当且仅当具有此名称的文件尚不存在时，以原子方式创建一个以此路径名命名的新空文件
        // 返回 true 如果指定的文件不存在并且已成功创建， 返回 false 如果命名文件已经存在
        System.out.println(f.createNewFile());

        return f;
    }
}
```

## 文件读入

```java
import java.io.*;
import java.nio.charset.StandardCharsets;
import java.util.Scanner;

public class Test {
    public static void main(String[] args) {
        File source_file = new File("." + File.separator + "我的文件.txt");

        // System.in只能读取标准输入里的byte
        Scanner in = new Scanner(System.in);
        classicWay(source_file);
        lambdaWay(source_file);
    }

    private static void classicWay(File source_file) {
        System.out.println("经典处理方式");

        try (
                // 打开文件输入流
                FileInputStream fis = new FileInputStream(source_file);
                // InputStreamReader 将字节流关联到字符集, 让字节数据转换为字符数据的Reader子类
                InputStreamReader isr = new InputStreamReader(fis, StandardCharsets.UTF_8);
                // BufferedReader是一个将字符输入流包装成缓冲区的Reader子类 提高读取字符效率
                BufferedReader reader = new BufferedReader(isr);
        ) {

            String line = null;
            // readLine 读取一行文本
            // 一行被认为是由换行符 ('\n')、回车符 ('\r')、紧跟换行符的回车符或到达文件末尾中的任何一个终止的(EOF)
            // 包含行内容的 String，不包括任何行终止字符，如果在未读取任何字符的情况下已到达流末尾，则为 null
            while ((line = reader.readLine()) != null) {
                System.out.println(line.trim().toUpperCase());
            }

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private static void lambdaWay(File source_file) {
        try (
             // 打开文件输入流
             FileInputStream fis = new FileInputStream(source_file);
             // InputStreamReader 将字节流关联到字符集, 让字节数据转换为字符数据的Reader子类
             InputStreamReader isr = new InputStreamReader(fis, StandardCharsets.UTF_8);
             // BufferedReader是一个将字符输入流包装成缓冲区的Reader子类 提高读取字符效率
             BufferedReader reader = new BufferedReader(isr);
        ) {

            // lines返回一个 Stream，其元素是从此 BufferedReader 读取的行
            // Stream提供了map方法用于对 Stream 流中的每个元素应用一定的转换 该函数会将元素映射成一个新的元素
            // forEach方法用于遍历Stream流中的每个数据元素
            reader.lines().map(String::trim).map(String::toUpperCase).forEach(System.out::println);

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```