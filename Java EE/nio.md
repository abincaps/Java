
# BIO

- Blocking IO 同步阻塞IO
- Blocking IO 指的是用户空间主动发起，需要等待内核IO操作彻底完成后才返回到用户空间的IO操作, 在IO操作过程中，发起IO请求的用户进程（或者线程）处于阻塞状态
- java.io 包中

# Non Blocking IO

- NIO, 同步非阻塞IO
- java.nio 包中

# Asynchronous IO

- AIO, 异步非阻塞IO

# BIO

- 一个服务端可以开启多个线程来处理客户端的连接，但是一个处理线程只能对应一个客户端连接

```java
public class SocketClient {  
  
    public static void main(String[] args) throws IOException {  
  
        // 连接服务端  
        Socket socket = new Socket("localhost", 9090);  
  
        // 发送数据  
        OutputStream outputStream = socket.getOutputStream();  
        outputStream.write("client A".getBytes());  
        outputStream.flush();  
  
        // 接受服务端返回的数据  
        InputStream inputStream = socket.getInputStream();  
  
        byte[] bytes = new byte[1024];  
        int len = inputStream.read(bytes);  
        System.out.println("收到服务端的数据 : " + new String(bytes, 0, len));  
  
  
        socket.close();  
    }  
}
```


```java
public class SocketServerMultipleThread {  
  
    public static void main(String[] args) throws IOException {  
  
        // 创建服务端的 Socket        
        ServerSocket serverSocket = new ServerSocket(9090);  
  
        while (true) {  
            System.out.println("Waiting for client to connect...");  
  
            // 阻塞等待连接  
            Socket socket = serverSocket.accept();  
  
            System.out.println("已经有客户端连接");  
  
            new Thread(new Runnable() {  
                @Override  
                public void run() {  
  
                    try {  
                        handle(socket);  
                    } catch (IOException e) {  
                        e.printStackTrace();  
                    }  
                }  
  
            }).start();  
        }  
    }  
  
    private static void handle(Socket socket) throws IOException {  
  
        // 开始处理客户端的连接请求  
        InputStream inputStream = socket.getInputStream();  
        byte[] bytes = new byte[1024];  
  
        // 阻塞的等待客户端向IO流通道写入数据  
        int len = inputStream.read(bytes);  
        System.out.println("收到客户端的数据 : " + new String(bytes, 0, len));  
  
        // 服务端返回信息给客户端  
        OutputStream outputStream = socket.getOutputStream();  
        outputStream.write("success".getBytes());  
        outputStream.flush();  
    }  
}
```

# NIO

- 一个线程可以处理多个客户端的请求，客户端发送的请求会注册到多路复用器 Selector 上，多路复用器 Selector 轮询各个客户端的请求并处理

# NIO线程模型

- Channel 通道，每个通道对应一个 buffer 缓冲池
- buffer 缓冲池，底层是数组
- Selector 选择器， Selector 对应一个或多个线程
- channel 会注册到 selector 上， 由 selector 根据 channel 读写事件的发生交给某个空闲线程来执行
- Buffer 和 Channel 可读可写

# Channel

- 通道，表示打开IO设备的连接
- 获取用于连接IO设备的通道和容纳数据的缓冲区

# Channel主要实现类

- FileChannel
- SocketChannel
- ServerSocketChannel
- DatagramChannel