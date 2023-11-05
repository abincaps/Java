
# java.net


# Class ServerSocket

- `ServerSocket()` 创建一个未绑定的服务套接字
- `ServerSocket(int port)` 创建绑定到指定端口的服务套接字 
- `Socket accept()` Listens for a connection to be made to this socket and accepts it. The method blocks until a connection is made.


# Class Socket 

- A socket is an endpoint for communication between two machines.
## Constructor

- `Socket(String host, int port)` Creates a stream socket and connects it to the specified port number on the named host.

## Method

- `InputStream getInputStream()` Returns an input stream for this socket.


# Class ServerSocket

- `void bind(SocketAddress endpoint)` Binds the ServerSocket to a specific address (IP address and port number).

# Class InetSocketAddress

- Creates a socket address from a hostname and a port number.

## Constructor

- `InetSocketAddress(int port)` 
- `InetSocketAddress(String hostname, int port)`


# final class URL

- `String getProtocol()` Gets the protocol name of this URL.
- `String getPath()` Gets the path part of this URL.