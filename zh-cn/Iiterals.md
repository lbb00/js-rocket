

# 字面量(Iiterals)

由一定字词组成的语词表达式定义的常量。

- 数组字面量(Array literals)
- 布尔字面量(Boolean literals)
- 浮点数字面量(Floating-point literals)
- 整数(Integers)
- 对象字面量(Object literals)
- 正则字面量(RegExp literals)
- 字符串字面量(String literals)

!> 当使用字面量的方法时，字面量会被隐式地转化为自身数据类型的对象。

```javascript
[1, 2, 3] // 字面量
[1, 2, 3].map() // 隐式地转化为 new Array([1, 2, 3]).map()
```