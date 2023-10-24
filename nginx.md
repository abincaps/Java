
- 反向代理
- 负载均衡
- 避免暴露接口

## 配置文件

- `conf/nginx.conf`

## 反向代理

```nginx
server {
	listen 80;
	server_name localhost;
	
	location / {
		root html/sky;
		index index.html index.htm;
	}
	
	# 反向代理, 处理管理端发送的请求
	location /api/ {
		proxy_pass http://localhost:8080/admin/;
	}
	
	# 反向代理,处理用户端发送的请求
	location /user/ {
		proxy_pass   http://webservers/user/;
	}
}
```

## 负载均衡策略

- 轮询，默认
- weight，权重， 默认为 1
- ip_hash, 根据 ip 分配
- least_conn, 依据最少连接
- url_hash, 根据 url 分配
- fair， 响应时间短的服务优先分配

```nginx
upstream webservers{
	server 127.0.0.1:8080 weight=90;
}
```