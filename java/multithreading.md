# Java 多线程

## 线程的状态

```java
NEW - 线程被创建, 但是还未启动 初始状态
RUNNABLE - 线程正在运行中, 或者准备就绪（Ready）, 等待CPU时间片
BLOCKED - 线程被阻塞
WAITING - 线程进入无限等待状态, 等待其它线程通知
TIMED_WAITING - 线程进入计时等待状态, 在指定时间后会被自动唤醒 （超时等待）
TERMINATED - 线程已终止运行完成

在 java.lang 包下   
Thread.State是个描述线程状态的枚举类
```

## 守护线程

```java
如果一个进程里没有线程，或者都是守护线程，那么进程就结束了
线程默认情况下都是用户线程(User Thread), 非守护线程

守护线程(Daemon Thread)是一种特殊的线程
该线程会在用户线程全部结束后自动退出

所以其存在依赖于非守护线程的运行
通过setDaemon(true)可以设置为守护线程


守护线程的常见用途:
后台服务线程, 如日志、监控等
JVM的垃圾回收线程
Tomcat中的Acceptor和Poller线程。
```

## Thread.sleep

```java
public static void main(String[] args) throws InterruptedException {
    print("abincaps", 500);
}

public static void print(String text, long interval) throws InterruptedException {

    for (char ch : text.toCharArray()) {
        // Thread 在 java.lang 包下
        // 使当前正在执行的线程休眠（暂时停止执行）
        // 暂停时长为指定的毫秒数
        Thread.sleep(interval);
        System.out.print(ch);
    }
    System.out.println();
}
```

## Runnable接口

```java
public static void main(String[] args) {

        for (int i = 1; i <= 10; i++) {
            Thread thread = new Thread(new Print(i), "thread name" + i);
            thread.start();
        }
    }


// 实现 Runnable接口, 覆盖(重写）run方法
static class Print implements Runnable {

    private long interval;
    public Print(long interval) {
        this.interval = interval;
    }

    @Override
    public void run() {

        // 如果不指定线程名 是 Thread-0 的格式
        System.out.println("当前正在运行：" + Thread.currentThread().getName());
        try {
            Thread.sleep(100 * interval);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

## interrupt和isInterrupted

```java
List<Thread> threads = new ArrayList<>();

for (int i = 1; i <= 10; i++) {
    Thread thread = new Thread(new Print(i), "thread name" + i);
    // 将普通线程设置为守护线程
    thread.setDaemon(true);
    thread.start();
    threads.add(thread);
}


//  currentThread静态方法 返回对当前正在执行的线程对象的引用
//  isInterrupted实例方法 测试此线程是否存活
Thread.currentThread().isInterrupted();

// interrupt 中断此线程 将线程的中断标志设置为true
// 如果此线程在调用 wait, join, sleep方法时 (被阻塞这个类的方法)
// 然后它的中断状态将被清除，会抛出InterruptedException 
threads.forEach(Thread::interrupt);

interrupt()来中断线程需要配合isInterrupted()方法
interrupt()仅仅是设置了线程的中断标志, 并不会直接终止线程
被中断的线程需要自行检查中断标志并处理

stop方法会强制线程停止执行 
本质上是不安全的，比如锁没有释放 已弃用
```

## synchronized

```java
synchronized同步控制
在默认情况下，synchronized关键字是非公平锁

synchronized用于对方法或代码块实现同步互斥
同步互斥 同一时刻只能有一个线程进入方法或代码块执行

如果修饰的是实例方法，则锁对象是该实例对象；
如果修饰的是静态方法，则锁对象是该类的Class对象

public class Data {
    private long value = 0;
    private static long static_value = 0;

    private long block_value = 0;

    // synchronized 同步实例方法
    public synchronized void change(long delta) {
        value += delta;
    }

    // synchronized 同步静态方法
    public synchronized static void changeStatic(long delta) {
        static_value += delta;
    }

    public void changeBlock(long delta) {
        // synchronized代码块 只能同步一个对象
        // synchronized (锁对象)

        // Object lock = new Object();
        // synchronized (lock)
        // synchronized (Data.class)
        synchronized (this) {
            block_value += delta;
        }
    }

    public void print() {
        System.out.println("value = " + value);
        System.out.println("static value = " + static_value);
    }

}


public class ChangeData implements Runnable {

    private Data data_holder;
    private long delta;
    private long loop_count;

    public ChangeData(long delta, long loop_count, Data data_holder) {
        this.delta = delta;
        this.loop_count = loop_count;
        this.data_holder = data_holder;
    }

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName());
        for (int i = 0; i < loop_count; i++) {
            data_holder.change(delta);
            Data.changeStatic(delta);
            data_holder.changeBlock(delta);
        }

        data_holder.print();
    }
}

Data data = new Data();
Thread increase = new Thread(new ChangeData(1, Integer.MAX_VALUE / 100, data));
Thread decrease = new Thread(new ChangeData(-1, Integer.MAX_VALUE / 100, data));
System.out.println("run");
increase.start();
decrease.start();
```

## 锁和监视器

```java
锁是一种同步机制，用于控制对共享资源的访问

监视器是一种对象，用于实现线程间的协调和通信
每个对象都有一个内置的监视器，可以通过synchronized关键字来使用

当一个线程执行带有synchronized关键字的方法或代码块时，它会获得该对象的监视器，
并且其他线程必须等待该线程释放监视器之后才能继续执行
```

## notify和notifyAll

```java
Object locker = new Object();

for (int i = 0; i < 5; i++) {
    // lambda
    new Thread(() -> {
        System.out.println(Thread.currentThread().getName() + ": start");

        try {
            synchronized (locker) {

                // wait 进入阻塞状态 会自动释放该对象的锁
                // 当前线程进入等待直到被唤醒，通常是 notified 或 interrupted
                // wait方法必须进入synchronized中使用
                Thread.sleep(TimeUnit.SECONDS.toMillis(1));

                System.out.println(Thread.currentThread().getName() + ": wait");
                locker.wait();

                System.out.println(Thread.currentThread().getName() + ": continue");
                Thread.sleep(TimeUnit.SECONDS.toMillis(1));
                System.out.println(Thread.currentThread().getName() + ": finished");

            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

    }, "thread" + i).start();
}

System.out.println("线程准备开始sleep");

Thread.sleep(TimeUnit.SECONDS.toMillis(2));

System.out.println("线程准备结束sleep");

// 等待所有线程进入等待才能获取锁
synchronized (locker) {
    // notify / notifyAll 方法必须进入相应对象的synchronized中
    // 也就是锁对象和线程中的锁对象要一样

    // notify 唤醒在此对象的监视器上等待的单个线程
    // 如果有任何线程正在等待该对象，则选择唤醒其中一个线程，选择是任意的

    // notifyAll 唤醒在此对象的监视器上等待的所有线程

    System.out.println("开始唤醒所有线程");
    locker.notifyAll();
}

lost notification问题是指一个线程在等待状态下错过了通知
等待线程进入等待状态之前就调用了notify()或notifyAll()方法
```

## 生产者和消费者模型

```java
public class Producer<T> {

    private Queue<T> tasks;

    private int maxTaskCount = 0;

    public Producer(Queue<T> tasks, int maxTaskCount) {
        this.tasks = tasks;
        this.maxTaskCount = maxTaskCount;
    }

    public void produce(T task) throws InterruptedException {

        synchronized (tasks) {

            // 限制队列里的任务不能超过maxTaskCount
            while (tasks.size() >= maxTaskCount) {
                System.out.println("生产者线程进入等待" + Thread.currentThread().getName());
                // wait 需要添加 throws InterruptedException 或者 try-catch处理
                tasks.wait();
            }

            tasks.add(task);

            // 通知消费者进行消费
            tasks.notifyAll();
        }
    }
}

public class Consumer<T> {
    private Queue<T> tasks;

    public Consumer(Queue<T> tasks) {
        this.tasks = tasks;
    }

    public T consume() throws InterruptedException {


        synchronized (tasks) {
            // 不能用if 替代 while
            // 所有线程被唤醒后 如果任务被先到的线程消耗完，需要再次检查是否为空
            // 没有任务可以做
            while (tasks.isEmpty()) {
                System.out.println("消费者线程进入等待：" + Thread.currentThread().getName());
                tasks.wait();
            }

            // poll返回删除此list的头部（第一个元素）
            // 如果为空返回null
            // removeFirst会抛出NoSuchElementException异常
            T ret = tasks.poll();

            // 通知生产者放入任务
            tasks.notifyAll();
            return ret;
        }
    }
}

public static void main(String[] args) {

    // LinkedList implements List<E>, Deque<E>
    // interface Deque<E> extends Queue<E>
    Queue<String> q = new LinkedList<>();

    Consumer<String> cosumer = new Consumer<>(q);
    Producer<String> producer = new Producer<>(q, 100);

    for (int i = 0; i < 10; i++) {

        Thread thread = new Thread(() -> {
            while (true) {
                try {
                    String url = cosumer.consume();
                    processURL(url);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }

            }
        }, "消费者-" + i);

        thread.start();
    }


    for (int i = 0; i < 5; i++) {

        Thread thread = new Thread(() -> {

            while (true) {
                try {
                    String url = produceURL();
                    producer.produce(url);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }

        }, "生产者-" + i);

        thread.start();
    }
}

private static String produceURL() {

    StringBuilder ret = new StringBuilder();
    ret.append("www.");

    for (int i = 0; i < 5; i++) {

        int rand = ((int) (Math.random() * 100)) % 26;
        char ch = (char) ('a' + rand);
        ret.append(ch);
    }

    ret.append(".com");

    return ret.toString();
}

private static void processURL(String url) throws InterruptedException {
    System.out.println("开始处理：" + url);
    Thread.sleep(TimeUnit.SECONDS.toMillis(1));
    System.out.println("处理完成：" + url);
}
```

## join

```java
join方法 进入阻塞 等待这个线程结束
在已经结束的线程中调用join会立刻返回
调用join方法要保证线程已经start，否则会立刻返回
也可以加入参数进入超时等待
参数全部为0 意味着永远等待

void join() 
void join(long millis)
void join(long millis, int nanos)
```

## 死锁

```java
A线程先申请input锁
B线程先申请output锁

B线程执行完output，想获取input锁, 
但是A线程并没有释放input锁，B线程进入等待

A线程执行完input，想获取output锁，
但是B线程因为没有拿到input锁，而没有释放output锁，A线程进入等待

解决办法 按统一顺序获取锁

先获取input锁，在input锁没有释放的情况下，获取output锁
```

## ThreadLocal

```java
每个线程都可以通过ThreadLocal存取数据, 该数据仅对当前线程可见
每个线程都会保留一个该变量的本地副本, 不同线程互不干扰
通过设置或获取当前线程的本地ThreadLocal变量, 来实现多线程的数据隔离

T  get() 返回此线程局部变量的当前线程副本中的值
void set(T  value) 将此线程局部变量的当前线程副本设置为指定值
```

## 指令重排

```java
指令重排(Instruction Reordering)指的是编译器和处理器为了优化程序性能
可以对指令序列进行重排序的一种技术

单线程环境下, 指令重排不会影响程序执行结果
多线程环境下, 存在可见性问题和有序性问题

可见性问题 
多个线程之间无法及时看到对方的操作
主要原因是线程间的缓存一致性问题, 导致数据没有及时刷新到主内存

有序性问题
代码在执行时, 顺序被编译器或CPU改变

解决方法
使用volatile关键字保证变量的可见性。
使用synchronized和Lock保证同步可见性。
```

## volaite

```java
volatile 会强制将变量的值刷新到主内存, 确保所有线程读取的是同一份最新数据，也就是强制从主存获取变量数据
```

## CAS

```java
CAS(Compare-And-Swap) 是一种无锁编程中的轻量级同步机制
不依赖锁来实现原子性

1.获取当前内存值
2.计算新的值
3.比较当前内存值和旧的预期值, 如果相同则执行交换, 内存里的值才会被设置为新的值
如果V和A不相等, 说明有并发修改
```

## 自旋

```java
自旋(spinning)是一种线程等待同步的方法
让等待的线程不断地检查状态, 而不是睡眠线程

不会使线程上下文切换, 避免了切换的开销
当等待时间很短时, 可以避免切换带来的性能损失
```

## 上下文切换

```java
TODO
```

## atomic类

```java
基于CAS实现线程安全, 不需要加锁
支持原子方式更新基本类型
可以避免同步带来的性能影响

TODO
```

## ReentrantLock

```java
Lock 可以比使用 synchronized 方法获得的更广泛的锁定操作

// ReentrantLock 可重入锁
// 如果当前线程已获取锁, 可以再次获取锁而不会阻塞
// 避免线程在获得锁之后,再次请求这个锁时导致的自锁死锁
// ReentrantLock(false) 缺省就是false 非公平锁
// true 是公平锁
Lock lock = new ReentrantLock();


try {
    // 仅当调用时 锁空闲 才获取锁
    // 如果在给定的等待时间内是空闲的并且当前线程还没有interrupted，则获取锁
    if (lock.tryLock(1, TimeUnit.SECONDS)) {

        try {
            process1();
        } finally {
            // finally可以保证释放锁
            // 在 finally中的第一行unlock
            lock.unlock();
        }
    } 

} catch (InterruptedException e) {
    e.printStackTrace();
}

try {
    lock.lock();
    Thread.sleep(TimeUnit.SECONDS.toMillis(2));
} catch (InterruptedException e) {
    e.printStackTrace();
} finally {
    lock.unlock();
}
```

## await和signAll

```java
Lock locker = new ReentrantLock();
Condition condition = locker.newCondition();

// 导致当前线程等待，直到收到信号或 interrupted
// 与此 Condition 关联的锁被自动释放
// wait() 释放锁后不能中断线程,而 await() 可以在等待过程中中断线程
condition.await(); // 对标Object的wait
// 唤醒所有等待的线程
condition.signalAll(); // 对标Object的notifyAll
```

## CountDownLatch

```java
CountDownLatch countDownLatch = new CountDownLatch(10);

// 每个线程工作完就countDown一下
// 减少锁的计数，如果计数达到零，则释放所有等待的线程
countDownLatch.countDown();

// 导致当前线程等待，直到锁存器倒计时到零
// 比join好的地方是， join必须保证线程已经start
countDownLatch.await();
```

## LinkedBlockingQueue

```java
// 默认容量
//  public LinkedBlockingQueue() {
//      this(Integer.MAX_VALUE);
//  }
LinkedBlockingQueue<String> linkedBlockingQueue = new LinkedBlockingQueue<>(100);

// 实现生产者和消费者模式
// 将指定元素插入此队列的尾部，必要时等待可用空间 (等于capacity时 不能往里面放)
linkedBlockingQueue.put("");

// 检索并删除此队列的头部，必要时等待直到元素可用（没有元素可用）
String s = linkedBlockingQueue.take();
```

## ConcurrentHashMap

```java
key和value不能为null
TODO
```

## 线程池

```java
// 生成指定数量的线程池
ExecutorService exe = Executors.newFixedThreadPool(1);

exe.submit(()-> {
    System.out.println("run");
});

// Future表示一个异步任务的执行结果
// Callable接口代表一种可调用的异步任务, 它可以返回执行的结果
Future<String> future = exe.submit(()->{
    return "abincaps";
});

// 函数式区分Runnable和Callable是通过重写有无返回值

// 如有必要，等待计算完成，然后检索其结果
future.get();

// 启动有序关闭，执行先前提交的任务，但不会接受新任务
// 等待已经提交的任务执行结束才会关闭
// 如果已经关闭，则调用没有额外效果
exe.shutdown();
```
