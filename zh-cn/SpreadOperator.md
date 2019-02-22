# Spread Operator `ES6`

用法: `...ObjectImplementsSymbolIterator`

调用的是遍历接口（Symbol.iterator)，如果对象没有这个接口，将无法展开。

```javascript
// array
const newArray = [...[1, 2], ...[3, 4]] // [1, 2, 3, 4]
const strArray = [...'123'] // ['1', '2', '3']
const [first, ...otherArray] = [1, 2, 3, 4] // otherArray -> [2, 3, 4]

// object
const newObj = { ...{ a: 1, b: 2 }, ...{ b: 3, c: 3 } } // { a: 1, b: 3, c: 3 }
const { a, b, ...otherObj } = { a: 1, b: 2, c: 3 } // otherObj -> { c: 3 }

// args
function funcA(...args) {}
function funcB(a, b, c, ...otherArgs) {}
```
