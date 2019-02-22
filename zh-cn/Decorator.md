# Decorator 修饰器 `ES6`

参考：

- [修饰器 - ECMAScript 6 入门 - 阮一峰](http://es6.ruanyifeng.com/#docs/decorator)

- 只能用来修饰对象和对象的方法
  - 修饰对象的方法类似 Object.defineProperty()
  - 有多个修饰器时，会像剥洋葱一样，先从外到内进入，然后由内向外执行(栈)
- 为什么不能修饰函数，Decorator 运行在编译时，存在变量提升 **(这个地方可能有一些不对)**

```javascript
// 修饰类，target代表类本身
function addStaticNameA(target) {
  target.name = 'A'
}

function addDisable(disable) {
  return function(target) {
    target.prototype.disable = disable
  }
}

// 修饰类的属性，targetPropertype代表类的propertype
// 修饰器的本意是要“修饰”类的实例，但是这个时候实例还没生成，所以只能去修饰原型
function log(id) {
  console.log('evaluated', id)
  return function(targetPropertype, propName, descriptor) {
    var value = descriptor.value
    descriptor.value = function() {
      console.log('executed', id)
      return value.apply(this, arguments)
    }
    return descriptor
  }
}

@addStaticNameA
@addDisable(true)
class A {
  @log(1)
  @log(2)
  run() {}
}
const a = new A()

A.name // -> A
a.disable // -> true
a.run()
// evaluated 1
// evaluated 2
// executed 2
// executed 1
```
