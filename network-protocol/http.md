
# HTTP

## HTTP协议

- 超文本传输协议（Hyper text Transfer Protocol)

## http请求

- 浏览器从 URL 中解析出域名
- 浏览器根据域名查询 DNS ， 获取到域名对应的 IP 地址
- 服务器监听 80 或 443 等 Web 端口
- 浏览器和服务器三次握手建立 TCP 连接 （完成 TLS/SSL 握手）
- 浏览器构造 HTTP 请求，填充上下文至 HTTP 头部
- 浏览器发起 HTTP 请求
- 服务器进行 HTTP 响应
- 浏览器引擎解析响应，渲染包体至用户界面
- 浏览器根据超链接构造其他 HTTP 请求

## HTTP协议格式

```http
GET / HTTP/1.1  请求行
Host: abincaps.com
```

```http
HTTP/1.1 200 响应行
Content-Type: application/json
Content-Length: 6
Date: Mon, 28 Aug 2023 12:39:40 GMT
Keep-Alive: timeout=60
Connection: keep-alive
```

## OSI模型

- 开放式系统互联模型（Open System Interconnection Model)
- 第7层 应用层 (Application Layer)
- 第6层 表现层(表示层)（Presentation Layer）
- 第5层 会议层(会话层)（Session Layer）
- 第4层 传输层（Transport Layer）
- 第3层 网络层（Network Layer）
- 第2层 数据链路层（Data Link Layer）
- 第1层 物理层（Physical Layer）

## TCP/IP 协议族

- 第4层 应用层（Application layer）
- 第3层 传输层（Transport layer）
- 第2层 网际层（Internet layer）
- 第1层 网络接口层 （Network Access layer）


## 报文

- Bit 比特流
- Frame 帧 
- Packet 包
- 








