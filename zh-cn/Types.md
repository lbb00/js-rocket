# 数据类型

## 基本类型(值类型)和引用类型(Object)

- 基本类型
  - undefined
  - null
  - Boolean
  - Number
    - Int
    - Float
    - NaN
  - String
  - Symbol `ES6`
- 引用类型(Object)
  - Array
  - Function
  - Date
  - RegExp
  - Map `ES6`
  - Set `ES6`
  - ...

## 基本类型和引用类型的区别

存储方式的不同。

- 栈内存

  - 对于`基本类型` 保存`变量标识符`和`变量值`
  - 对于`引用类型` 保存`变量标识符`和`指向堆内存中该对象的指针`

- 堆内存
  - 保存`引用类型`具体的内容

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
