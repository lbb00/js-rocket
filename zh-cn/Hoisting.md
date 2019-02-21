# Hoisting

> 提升并不是一个技术名词

参考：[我用了两个月的时间才理解 let - 方应杭](https://zhuanlan.zhihu.com/p/28140450)

目前理解为四个阶段：

1. 创建 create - 可以理解给变量分配了一个标识符，例如 `x`
2. 锁死区 - `let` 和 `const` 有锁死区的概念，在分配空间发生错误，就再也不能赋值了。
3. 初始化 initialize - 给标识符分配空间`x`现在有了一个空间 `&x`
4. 赋值 assign - 往空间里放东西 `*x = 1`

可以理解为，在分配空间以前，变量都是无法使用的。

```javascript
// created x
// initialize x -> undefined
// ... script code start
// ... can use x
var x = 1 // assigned x
```

```javascript
// created fn
// initialize & assigned fn -> function fn () { console.log('fn') }
// ... script code start
// ... can use fn
function fn() {
  console.log('fn')
}
```

```javascript
// created x
// ... script code start
// ... x -> temporal dead zone
let x = 1 // initialize & assigned x -> 1
// ... can use x
x = 2 // assigned x
```

```javascript
// created x
// ... script code start
// ... x -> temporal dead zone
const x = 1 // initialize & assigned x -> 1
// ... can use x
x = 2 // error
```
