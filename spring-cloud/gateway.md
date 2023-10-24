
# Gateway

## 启用日志

设置 vm options `-Dreactor.netty.http.server.accessLogEnabled=true`

## 常见配置

- `spring.cloud.gateway.globalcors.cors-configurations.[/**].allowedOriginPatterns=*`  允许的访问源 
- `spring.cloud.gateway.globalcors.cors-configurations.[/**].allowedHeaders=*`  允许的请求头 
- `spring.cloud.gateway.globalcors.cors-configurations.[/**].allowedMethods=*`  允许的请求方法  
- `spring.cloud.gateway.globalcors.cors-configurations.[/**].allowCredentials=true`  是否允许携带cookie 
- `spring.cloud.gateway.globalcors.cors-configurations.[/**].maxAge=3600` 跨域检测有效期 

## 全局 Filter

- `GlobalFilter` 接口是特殊的过滤器，有条件地应用于所有路由
- `GlobalFilter` 的顺序由 `Ordered` 接口进行排序（优先级）， 通过实现 `getOrder()` 方法来设置这个接口, 按照返回值int升序
