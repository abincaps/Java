
## Spring Data Redis


## 连接到 Redis

- TODO

## Interface RedisConnectionFactory

- Thread-safe factory of Redis connections

## Class RedisTemplate<K,V>

- `ValueOperations<K, V> opsForValue()` Returns the operations performed on simple values
- `<HK, HV> HashOperations<K, HK, HV> opsForHash()` Returns the operations performed on hash values
- `Set<K> keys(K pattern)` Find all keys matching the given pattern.
- `Long delete(Collection<K> keys)` Delete given keys.

## Interface RedisSerializer\<T>

- Basic interface serialization and deserialization of Objects to byte arrays

## Interface ValueOperations<K,V>

- `void set(K key, V value)`  Set `value` for `key`
- `Boolean setIfAbsent(K key, V value)` Set `key` to hold the string `value` if `key` is absent

## Interface HashOperations<H,HK,HV>

- `void put(H key, HK hashKey, HV value)` Set the `value` of a hash `hashKey`
- `HV get(H key, Object hashKey)` Get value for given `hashKey` from hash at `key`
- `Long delete(H key, Object... hashKeys)` Delete given hash `hashKeys`

