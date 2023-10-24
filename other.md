
## 唯一ID

- 自增id不适合分布式数据库，分库分表场景
- UUID 通用唯一识别码（universally unique identifier）是无序的，会影响索引效率
- 雪花算法由twitter开源


## 雪花算法

- 有序（递增）id
- 总共64bit，1 bit 为 0， 41bit 时间戳， 10bit 工作机器id，12bit 序列号
- 同一毫秒内生成2^12（4096）个不重复id
- 时间回拨问题 暂时不启动服务器 

## 单点登录

- redis + token
- JWT

## JWT

- JSON Web Token 用于作为 JSON 对象在各方之间安全地传输信息
- token被解密 加盐值（密钥）
- token被第三方使用 限流

## 数据

- entity 实体， 和数据库表对应
- DTO 数据传输对象，各层之间传递数据，对前端传过来的数据进行封装
- VO 视图对象，为前端展示数据提供的对象
- POJO 普通 JAVA 对象， 只有属性字段和对应的 getter 和 setter 方法
