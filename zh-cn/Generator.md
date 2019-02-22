# Generator `ES6`

> ES6 提供的一种异步编程解决方案。

Generator 函数是一个状态机，封装了多个内部状态。执行 Generator 函数会返回一个遍历器对象，也就是说，Generator 函数除了状态机，还是一个遍历器对象生成函数。返回的遍历器对象，可以依次遍历 Generator 函数内部的每一个状态。

特征

- function 关键字与函数名之间有一个星号
  - ES6 没有规定，function关键字与函数名之间的星号写在哪个位置，由于 Generator 函数仍然是普通函数，建议星号紧跟在function关键字后面。`function* foo()`
- 函数体内部使用 yield 表达式，定义不同的内部状态
  - Generator 函数可以不用`yield`表达式，这时就变成了一个单纯的暂缓执行函数。

Generator 函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象，也就是上一章介绍的遍历器对象（Iterator Object）。
必须调用遍历器对象的next方法，使得指针移向下一个状态。也就是说，每次调用next方法，内部指针就从函数头部或上一次停下来的地方开始执行，直到遇到下一个yield表达式（或return语句）为止。
Generator 函数是分段执行的，yield表达式是暂停执行的标记，而next方法可以恢复执行。

```javascript
// 用 yield
function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';
}
let hw = helloWorldGenerator()
hw.next() // {value: 'hellow', done: false}
hw.next() // { value: 'world', done: false }
hw.next() // { value: 'ending', done: true }
hw.next() // { value: undefined, done: true }

// 不用 yield
funciton* foo() { // 不会立即执行，需要手动调用next方法
  console.log('foo')
}
let fooGenerator = foo()
setTimeout(function () {
  fooGenerator.next()
},2000)

```

## yield 表达式

由于 Generator 函数返回的遍历器对象，只有调用`next`方法才会遍历下一个内部状态，所以其实提供了一种可以暂停执行的函数。`yield`表达式就是暂停标志。

逻辑如下：

- 遇到`yield`表达式，就暂停执行后面的操作，并将紧跟在`yield`后面的那个表达式的值，作为返回的对象的`value`属性值。
- 下一次调用`next`方法时，再继续往下执行，直到遇到下一个`yield`表达式。
- 如果没有再遇到新的`yield`表达式，就一直运行到函数结束
  - 直到`return`语句为止，并将`return`语句后面的表达式的值，作为返回的对象的`value`属性值
  - 如果该函数没有`return`语句，则返回的对象的`value`属性值为`undefined`。

!> `yield`表达式后面的表达式，只有当调用`next`方法、内部指针指向该语句时才会执行，因此等于为 JavaScript 提供了手动的“惰性求值”（Lazy Evaluation）的语法功能。

* 只能用在 Generator 函数里面，用在其他地方都会报错

```javascript
var arr = [1, [[2, 3], 4], [5, 6]];
var flat = function* (a) {
  a.forEach(function (item) { // 不能用于非 Generator 函数里面!
    if (typeof item !== 'number') {
      yield* flat(item) // 报错!
    } else {
      yield item
    }
  })
}
for (var f of flat(arr)) console.log(f)
```

* `yield`表达式如果用在另一个表达式之中，必须放在圆括号里面

```javascript
function* foo() {
  console.log('foo' + yield); // SyntaxError
  console.log('foo' + yield 123); // SyntaxError

  console.log('foo' + (yield)); // OK
  console.log('foo' + (yield 123)); // OK
}
```

* `yield`表达式用作函数参数或放在赋值表达式的右边，可以不加括号

```javascript
function* foo() {
  bar(yield 'a', yield 'b'); // OK
  let x = yield; // OK
}
```

## 与Iterator接口的关系

Generator 函数就是遍历器生成函数，因此可以把 Generator 赋值给对象的`Symbol.iterator`属性，从而使得该对象具有 Iterator 接口。

```javascript
var myIterable = {};
myIterable[Symbol.iterator] = function* () {
  yield 1;
  yield 2;
  yield 3;
};
[...myIterable] // [1,2,3]
```

