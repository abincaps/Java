---
description: 包含各种操作数组方法的类
---

# Arrays

## asList方法

* `public static List asList(T... a`) 接受[可变参数](../basic-grammar.md#ke-bian-can-shu), 返回大小固定的List类型的集合
* 内部元素仍指向原数组（可变参数在编译时转换为数组）, 并没有复制数组内容
* 任何对返回List的修改都会同步反映到原数组
* 不能添加和删除元素，否则保持列表不变并抛出`UnsupportedOperationException`

