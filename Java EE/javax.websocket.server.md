
# @ServerEndpoint

- `@ServerEndpoint` 通常注解在类上，指定这个类是 WebSocket 服务器端点

```java
@ServerEndpoint("/ws/{sid}")  
public class WebSocketServer {  
}
```