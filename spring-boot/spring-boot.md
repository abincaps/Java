
# Spring Boot


SpringApplication application = new SpringApplication(MyApplication.class);


## 配置文件

- 在 `application.properties` 或 `application.yml` 下


## 常见配置

- `server.port=8080` 修改HTTP端口
- `server.port=0` 使用一个随机的未分配的HTTP端口
- `server.servlet.context-path=/前缀` 修改访问URL前缀 方便微 服务中路由转发
