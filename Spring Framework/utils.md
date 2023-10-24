
# Utils

## DigestUtils类常用方法

- `static String md5DigestAsHex(byte[] bytes)` 返回给定字节数组的 MD5 的十六进制字符串

## ## BeanUtils类常用方法

- `static void copyProperties(Object source, Object target)` 将指定源 bean 的属性值复制到目标 bean 中，复制的是属性交集，属性需要匹配，如果源对象中的属性在目标对象中不存在，那么这些属性将被忽略，不会复制到目标对象中

