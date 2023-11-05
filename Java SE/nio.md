~``
# java.nio

# Class ServerSocketChannel

- `static ServerSocketChannel open()` Opens a server-socket channel for an Internet protocol socket.
- `final SelectableChannel configureBlocking(boolean block) 

# abstract class SocketChannel

- `abstract int read(ByteBuffer dst)`  Reads a sequence of bytes from this channel into the given buffer.
- `abstract boolean isConnectionPending()` Tells whether or not a connection operation is in progress on this channel.
- `abstract boolean finishConnect()` 

# abstract class AbstractSelectableChannel

- `final SelectableChannel configureBlocking(boolean block)` Adjusts this channel's blocking mode. `block` - If true then this channel will be placed in blocking mode; if false then it will be placed non-blocking mode

# class Selector

- `static Selector open()` Opens a selector.
- `abstract int select()` Selects a set of keys whose corresponding channels are ready for I/O operations.

# abstract class SelectableChannel

- `final SelectionKey register(Selector sel, int ops)` Registers this channel with the given selector, returning a selection key.

# abstract class SelectionKey

- `SelectionKey interestOps(int ops)` Sets this key's interest set to the given value.

# abstract class ByteBuffer

- `static ByteBuffer allocate(int capacity)` Allocates a new byte buffer.
- `static ByteBuffer wrap(byte[] array)` Wraps a byte array into a buffer.
- `ByteBuffer flip()` Flips this buffer. The limit is set to the current position and then the position is set to zero. 
- `final boolean hasRemaining()` Tells whether there are any elements between the current position and the limit.
	
