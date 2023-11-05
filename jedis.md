
# class Jedis

## Constructor

- `Jedis(final String host, final int port)` 

## Method

- `String ping()` This command is often used to test if a connection is still alive, or to measure latency.
- `String set(final String key, final String value)` Set the string value as value of the key. Time complexity: O(1)
- `String get(final String key)` Get the value of the specified key. If the key does not exist the special value 'nil' is returned. Time complexity: O(1)
- `String select(final int index)` Select the DB with having the specified zero-based numeric index.

# class GenericObjectPoolConfig\<T>

- org.apache.commons.pool2.impl
- `void setMaxTotal(int maxTotal)` 设置最大连接数
- `void setMaxIdle(final int maxIdle)` 设置最大空闲数
- `void setMinIdle(final int minIdle)` 设置最少空闲数
- `void setMaxWait(final Duration maxWaitDuration)` 设置等待时间

# class JedisPool

- `Jedis getResource()` 

```java
public class JedisPoolConnectRedis {  
  
    private static JedisPool jedisPool;  
  
    static {  
        // 创建连接池配置对象  
        JedisPoolConfig jedisPoolConfig = new JedisPoolConfig();  
        // 设置最大连接数  
        jedisPoolConfig.setMaxTotal(5);  
        // 设置最大空闲数  
        jedisPoolConfig.setMaxIdle(5);  
        // 设置最少空闲数  
        jedisPoolConfig.setMinIdle(5);  
        // 设置等待时间  
        jedisPoolConfig.setMaxWait(Duration.ofMillis(100));  
        // 初始化 JedisPool 对象  
  
        jedisPool = new JedisPool(jedisPoolConfig, "192.168.80.1", 6379, 100);  
    }  
  
    public static Jedis getJedis() {  
        return jedisPool.getResource();  
    }  
}
```

