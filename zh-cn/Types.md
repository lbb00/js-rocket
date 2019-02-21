# 数据类型

## 基本类型(值类型)和引用类型(Object)

- 基本类型
  - Boolean
  - null
  - undefined
  - Number
    - Int
    - Float
    - NaN
  - String
  - Symbol `ES6`
- 引用类型
  - Object
    - Array
    - Function
    - Date
    - RegExp
    - Map `ES6`
    - Set `ES6`
    - ...

## 基本类型和引用类型的区别

> 主要是存储方式的不同

- 栈内存

  - `基本类型` 保存 `变量标识符` 和 `变量值`
  - `引用类型` 保存 `变量标识符` 和 `指向堆内存中该对象的指针`

- 堆内存
  - 保存 `引用类型` 具体的内容

```javascript
const obj = {
  a: 1,
  b: {
    c: 2
  }
}
const obj2 = obj.b

obj2.c = 3 // 此时 obj.b.c 的值也为3，obj.c 和 obj2 指向的是同一个堆内存地址
```

## 字面量(literals)

> 由一定字词组成的语词表达式定义的常量

- 数组字面量(Array literals)
- 布尔字面量(Boolean literals)
- 浮点数字面量(Floating-point literals)
- 整数(Integers)
- 对象字面量(Object literals)
- RegExp literals
- 字符串字面量(String literals)

当使用字面量的方法时，字面量会被隐式地转化为自身数据类型的对象。

```javascript
[1, 2, 3] // 字面量
[1, 2, 3].map() // 隐式地转化为 new Array([1, 2, 3]).map()
```
