# java常用api

## tip1

```java
面向对象里出现过Object 回顾一下
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

    // 没有重写equals 返回的是 this == obj
    // 即比较两个对象的内存地址是否相等
    // 重写equals 判断内容是否相等
    @Override
    public boolean equals(Object o) {

        // 先判断是否是同一地址 优化
        if (this == o) {
            return true;
        }

        // 判断是否是空指针
        // 如果不是再判断是否是统一类型
        if (o == null || getClass() != o.getClass()) {
            return false;
        }

        // 强制转换为具体的类型
        Student student = (Student) o;
        return age == student.age && Objects.equals(name, student.name);
    }
}

Foo a = null;

// 判断是否是空指针
System.out.println(Objects.isNull(a));; // false
// 判断是否不是空指针
System.out.println(Objects.nonNull(a)); // true
```

## tip2

```java
// 8中基本类型 后面对应包装类
byte: 1字节 Byte
short: 2字节 Short
int: 4字节 Integer
long: 8字节 Long
float: 4字节 Float
double: 8字节 Double
char: 2字节 Character
boolean: 1字节 Boolean
// 包装类是把基本类型包装成类 

// 包装类构造方法已过时 使用valueOf
System.out.println(Integer.valueOf(10)); // 10
System.out.println(Integer.valueOf("10")); // 10
// 进制转换 需要符合进制规则
System.out.println(Integer.valueOf("10", 2)); // 2
System.out.println(Integer.valueOf("10", 8)); // 8

Integer a = 10; // 自动装箱 基本类型到包装类的自动转换
int b = a; // 自动拆箱 包装类到基本类型的自动转换

System.out.println(Integer.toString(a) + 1); // 101
System.out.println(Integer.parseInt("10") + 1); // 11
```

## tip3

```java
//可变字符串对象
// StringBuilder非线程安全
// StringBuilder()构造一个没有字符且初始容量为 16 个字符的字符串
StringBuilder a = new StringBuilder();
StringBuilder b = new StringBuilder("abc");

// append在末尾添加后会返回this引用
b.append(123).append("abc");
System.out.println(b); // abc123abc
b.reverse();
System.out.println(b); // cba321cba
// 把StringBuilder转换为String
String c = b.toString(); // 必须用toString 不可以强制类型转换
// 每次对String做拼接都会生成新的字符串,之前字符串被丢弃
// StringBuilder会存储字符序列, 并在原有序列修改, 不生成新字符串
// StringBuilder适合多次修改或大量拼接字符串

// StringBuilder非线程安全，StringBuffer线程安全
// StringBuffer的方法都是synchronize同步的, StringBuilder不是
// StringBuffer性能略低于StringBuilder
```

## tip4

```java
// 用于高效地连接字符串 指定分隔符
StringJoiner a = new StringJoiner(",");

a.add("123");
a.add("abc");
a.add("efg");
System.out.println(a); // 123,abc,efg

// 指定首尾
StringJoiner b = new StringJoiner(",", "[", "]");

b.add("123");
b.add("abc");
b.add("efg");
System.out.println(b); // [123,abc,efg]
```

## tip5

```java
//  Math类在java.lang下 无需导包
// 绝对值 对多个数值类型重载
System.out.println(Math.abs(-1)); // 1
// 向上取整 只有double
System.out.println(Math.ceil(1.2)); // 2.0
// 向下取整 只有double
System.out.println(Math.floor(1.2)); // 1.0
// 四舍五入 有float和double两个版本
System.out.println(Math.round(1.4)); // 1
System.out.println(Math.round(1.5F)); // 2

System.out.println(Math.max(1, 2)); // 2
System.out.println(Math.min(1, 2)); // 1

System.out.println(Math.pow(2, 8)); // 256.0
// 随机数 返回带正号的 double 值，大于或等于 0.0 且小于 1.0
System.out.println(Math.random());
```

## tip6

```java
import java.util.Random;

...

Random r = new Random();
System.out.println(r.nextInt(10));      // [0,10)
System.out.println(r.nextInt(10) + 1);  // [1,11)
```

## tip7

```java
// System在java.lang包下 无需导包
// 以毫秒为单位返回当前时间。请注意，虽然返回值的时间单位是毫秒，但值的粒度取决于底层操作系统并且可能更大。
// 例如，许多操作系统以几十毫秒为单位测量时间
// 当前时间与 UTC 时间 1970 年 1 月 1 日午夜之间的差异，以毫秒为单位
System.out.println(System.currentTimeMillis());

// 终止当前运行的 Java 虚拟机，该参数用作状态代码，按照惯例，非零状态代码表示异常终止
// 此方法调用类 Runtime 中的 exit 方法。此方法永远不会正常返回
System.exit(-1);
// Process finished with exit code -1
```

## tip8

```java
// Runtime 是单例  且在java.lang下无需导包
// 获取系统运行时环境信息
Runtime rt = Runtime.getRuntime();

// 处理器数量 逻辑CPU核心数
// 多个逻辑CPU核心 (超线程技术) 映射到同一个物理CPU核心上。因此逻辑CPU核心数可能多于物理CPU核心数
System.out.println(rt.availableProcessors());

// 返回 Java 虚拟机中的内存总量 字节数
System.out.println(rt.totalMemory() / (1024 * 1024));

// 返回 Java 虚拟机中的可用内存量 字节数
System.out.println(rt.freeMemory() / (1024 * 1024));

// System.exit() 就是调用Runtime.getRuntime().exit(status)
rt.exit(0);
```

## tip9

```java
// double和float类型无法精确表示十进制分数 会失真
// float和double会出现舍入误差
// BigDecimal类可以表示一个任意大小且精度完全准确的浮点数

BigDecimal a = new BigDecimal("1.2"); // String指定不会出现精度错误
// valueOf调用 new BigDecimal(Double.toString(val))
BigDecimal b = BigDecimal.valueOf(2.3); // 推荐

System.out.println(a.add(b)); // 3.5
System.out.println(a.multiply(b)); // 2.76
System.out.println(a.subtract(b)); // -1.1
// 如果无法表示确切的商（因为它有一个不终止的十进制扩展），则会抛出一个 ArithmeticException
// divide方法可以指定精度和舍入模式
// RoundingMode.UP - 向上舍入 不管保留数字后面是大是小 (0 除外) 都会进 1
// RoundingMode.DOWN - 向下舍入 截断操作，后面所有数字直接去除
// RoundingMode.CEILING - 向正无穷方向舍入 整数时 CEILING不会改变值
// RoundingMode.FLOOR - 向负无穷方向舍入
// RoundingMode.HALF_UP - 四舍五入
// RoundingMode.HALF_DOWN - 五舍六入
// RoundingMode.HALF_EVEN - 银行家舍入法
// 银行家舍入法
// 舍弃部分的最高位
// 如果它前面的数字为奇数, 则舍入行为与ROUND_HALF_UP相同, 即四舍五入。
// 如果它前面的数字为偶数, 则舍入行为与ROUND_HALF_DOWN相同, 即五舍六入。
System.out.println(a.divide(b, 10, RoundingMode.HALF_UP)); // ArithmeticException
System.out.println(BigDecimal.valueOf(2.0).divide(BigDecimal.valueOf(1.0), 2, RoundingMode.UP));

double c = a.doubleValue();
int d = a.intValue();
long e = a.longValue();
String f = a.toString();
```

## tip10

```java
// jdk8之前 在java.util包下
Date d = new Date();
System.out.println(d); // 类似 Wed Aug 09 20:23:19 CST 2023

// 返回此 Date 对象表示的自 1970 年 1 月 1 日 00:00:00 GMT 以来的毫秒数。
System.out.println(d.getTime());
// 1s后
System.out.println(new Date(d.getTime() + 1000)); // 类似 Wed Aug 09 20:23:20 CST 2023
d.setTime(d.getTime() + 1000);
System.out.println(d); // 类似 Wed Aug 09 20:23:20 CST 2023

// 格式化和解析日期时间对象 a是上午/下午标记
SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH::mm::ss EEE a");
System.out.println(sdf.format(d)); // 类似 2023年08月09日 20:23:20 Wed PM
SimpleDateFormat s = new SimpleDateFormat("yyyy-MM-dd HH::mm::ss");
System.out.println(s.parse("2023-8-9 20::44::10")); //Wed Aug 09 20:44:10 CST 2023
// 添加throws ParseException 抛出ParseException

Calendar ca = Calendar.getInstance();
System.out.println(ca.get(Calendar.YEAR)); // 2023

// new Date(getTimeInMillis()); 获取毫秒
Date dt = ca.getTime(); // 获取日期对象

ca.set(Calendar.MONTH, 9); // 修改
```

## tip11

```java
// jdk8之前 可变对象 线程不安全 能精确到纳秒
// jdk8 新增时间 都是不可变对象 线程安全， 能精确到纳秒
// 在 java.time 包下

LocalDate ld = LocalDate.now();
System.out.println(ld.getYear()); // 2023
System.out.println(ld.getMonth()); // AUGUST
System.out.println(ld.getMonth().getValue()); // 8
System.out.println(ld.getDayOfMonth()); // 9
// 一年中的第几天
System.out.println(ld.getDayOfYear()); // 221
System.out.println(ld.getDayOfWeek()); // WEDNESDAY
System.out.println(ld.getDayOfWeek().getValue()); // 3

// 不可变对象 获取返回值
LocalDate a = ld.withYear(2077);
System.out.println(a.getYear()); // 2077

System.out.println(ld.plusDays(31)); // 2023-09-09
System.out.println(ld.minusDays(31)); // 2023-07-09

LocalDate l = LocalDate.of(2077, 1, 1);
System.out.println(l); // 2077-01-01
System.out.println(ld.equals(l)); // false
System.out.println(ld.isBefore(l)); // true
System.out.println(ld.isAfter(l)); // false

LocalTime lt = LocalTime.now();
System.out.println(lt); // 21:41:51.239104500
System.out.println(lt.getHour()); // 21
System.out.println(lt.getMinute()); // 41
System.out.println(lt.getSecond()); // 51
System.out.println(lt.getNano()); // 返回纳秒

// plus minus 类似 LocalDate
System.out.println(lt.plusHours(24));

lt = LocalTime.of(11, 11, 11);
System.out.println(lt); // 11:11:11

/// LocalDateTime 是 LocalDate 和 LocalTime 的结合体

LocalDateTime ldt = LocalDateTime.now();
// 转换
ld = ldt.toLocalDate();
lt = ldt.toLocalTime();
```

## tip12

```java
ZoneId zid = ZoneId.systemDefault();
System.out.println(zid); // Asia/Shanghai
System.out.println(ZoneId.getAvailableZoneIds()); // 所用的时区
zid = ZoneId.of("America/New_York");
System.out.println(zid); // America/New_York

// 对应时区的时间
System.out.println(ZonedDateTime.now(zid));
System.out.println(ZonedDateTime.now(Clock.systemUTC()));

// TimeZone和Calendar 来自于早期Java API 在 java.util 包下
// TimeZone默认时区为GMT
// ZoneId默认时区为系统时区
Calendar.getInstance(TimeZone.getTimeZone(zid));
```

## tip13

```java
// Instant表示一个时间戳 表示 UTC时间, 绝对时间点的时间戳
// 可以精确到纳秒

Instant i = Instant.now();
// 从 1970-01-01 00:00:00 获取秒数
System.out.println(i.getEpochSecond());

i = Instant.ofEpochSecond(3600);
System.out.println(instant.getEpochSecond()); // 3600
```

## tip14

```java
// SimpleDateFormat 线程不安全
// 在 java.time.format 包下
DateTimeFormatter f = DateTimeFormatter.ofPattern("yyyy年MM月dd日 HH::mm::ss");
LocalDateTime now = LocalDateTime.now();
System.out.println(now); // 2023-08-09T23:34:12.554003400
// 两种格式化方法
System.out.println(f.format(now)); // 2023年08月09日 23::34::33
System.out.println(now.format(f)); // 2023年08月09日 23::35::45

String d = "2023年08月09日 20::44::10";
// 解析时间
LocalDateTime l = LocalDateTime.parse(d, f);
System.out.println(l); // 2023-08-09T20:44:10
```

## tip15

```java
LocalDate start = LocalDate.of(2023, 11, 11);
LocalDate end = LocalDate.of(2077, 1, 1);
// Period类代表一个时间段, 可以用于计算两个日期之间的差值或执行日期运算
// 在 java.time 包下
Period p = Period.between(start, end);

System.out.println(p.getYears()); // 53
System.out.println(p.getMonths()); // 1
System.out.println(p.getDays()); // 21

LocalTime a = LocalTime.of(2, 11, 11);
LocalTime b = LocalTime.of(23, 33, 33);
Instant c = Instant.now();

// 两者类型一致
Duration du = Duration.between(a, b);
System.out.println(du.toDays()); // 0
System.out.println(du.toHours()); // 21
System.out.println(du.toMinutes()); // 1282
System.out.println(du.toSeconds()); // 76942
```

## tip16

```java
public class Student implements Comparable<Student> {

    public String name;
    public int age;

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public int compareTo(Student o) {

        // 按age升序
        return age < o.age ? -1 : 1;

        // 相同返回 0
        // return age - o.age;
        // 降序
        // return o.age - age;
    }
}

int[] a = {34, 12, 56, 78};
System.out.println(Arrays.toString(a)); // [34, 12, 56, 78]
System.out.println(a.toString()); // [I@4eec7777

int[] b = Arrays.copyOf(a, 1);
System.out.println(Arrays.toString(b)); // [34]
b = Arrays.copyOfRange(a, 0, 2); // [0, 2) 区间
System.out.println(Arrays.toString(b)); // [34, 12]

double[] c = {34, 12, 56, 78};

Arrays.setAll(c, new IntToDoubleFunction() {

    @Override
    public double applyAsDouble(int value) {
        return c[value] * 0.5;
    }
});

System.out.println(Arrays.toString(c)); // [17.0, 6.0, 28.0, 39.0]

Arrays.sort(a);
System.out.println(Arrays.toString(a)); // [12, 34, 56, 78]


Student[] st = new Student[4];

st[0] = new Student("cd", 22);
st[1] = new Student("ef", 11);
st[2] = new Student("ab", 33);
st[3] = new Student("gh", 22);

Arrays.sort(st);

for (int i = 0; i < st.length; i++) {
    System.out.println(st[i].name + " " + st[i].age);
}

//  ef 11
//  cd 22
//  gh 22
//  ab 33

import java.util.Comparator

// 匿名对象创建比较规则
Arrays.sort(st, new Comparator<Student>() {
    // 指定比较规则
    @Override
    public int compare(Student o1, Student o2) {

        // return (x < y) ? -1 : ((x == y) ? 0 : 1);
        return  Integer.compare(o1.age, o2.age);
        // 降序
        // return  -Integer.compare(o1.age, o2.age);
        // return  Integer.compare(o2.age, o1.age);
    }
});

// lambda简化
Arrays.sort(st, (Student a, Student b) -> {
    // 指定比较规则
    return  Integer.compare(a.age, b.age);
});

// 继续简化 省略参数类型
Arrays.sort(st, (a, b) -> Integer.compare(a.age, b.age));

// lambda表达式 a -> a.age 表示获取每个元素的age属性
Arrays.sort(st, Comparator.comparingInt(a -> a.age));


class Compare {
    public static int compareByAge(Student a, Student b) {
        return a.age - b.age;
    }

}
Arrays.sort(st, (o1, o2) -> Compare.compareByAge(o1, o2));
// 参数对应一致 可以替换成静态方法引用
Arrays.sort(st, Compare::compareByAge);


class Compare {
    public int compareByAge(Student a, Student b) {
        return a.age - b.age;
    }
}

// 实例方法引用
Compare cmp = new Compare();
Arrays.sort(st, cmp::compareByAge);

Arrays.sort(st, (a, b) -> a.name.compareTo(b.name));

String[] str = {"Ab", "bA", "caps", "abin"};
// 特定类型的方法引用
Arrays.sort(str, String::compareToIgnoreCase);
System.out.println(Arrays.toString(str)); // [Ab, abin, bA, caps]


TODO
构造器引用
```

## tip17

```java
Time.unit
```

## asList方法

```java
asList方法定义在java.util.Arrays类中
可以把一个数组转换为一个List集合

public static <T> List <T> asList(T... a)
接受可变参数, 返回一个List类型的集合

返回的List仅仅是一个转换接口, 内部元素仍指向原数组, 并没有复制数组内容
返回List的大小是固定的, 不能添加和删除元素
否则保持列表不变并抛出UnsupportedOperationException
任何对返回List的修改都会同步反映到原数组
```