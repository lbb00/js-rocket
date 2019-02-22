# Symbol

```
ES5 的对象属性名都是字符串，这容易造成属性名的冲突。比如，你使用了一个他人提供的对象，但又想为这个对象添加新的方法（mixin 模式），新方法的名字就有可能与现有方法产生冲突。如果有一种机制，保证每个属性的名字都是独一无二的就好了，这样就从根本上防止属性名的冲突。这就是 ES6 引入Symbol的原因。
```

ES6引入的一种新的原始数据类型Symbol，表示独一无二的值。

Symbol通过`Symbol()`函数生成

对象的属性名现在可以有两种类型：1、字符串 2、Symbol

`Symbol`函数前不能使用`new`命令，否则会报错。这是因为生成的 Symbol 是一个原始类型的值，不是对象。也就是说，由于 Symbol 值不是对象，所以不能添加属性。基本上，它是一种类似于字符串的数据类型。

`Symbol`函数可以接受一个字符串作为参数，表示对 Symbol 实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分。

```javascript
let s1 = Symbol('foo')
let s2 = Symbol('bar')

s1 // Symbol(foo)
s2 // Symbol(bar)
// 如果不加参数输出的都是Symbol()，不方便区分

s1.toString() // "Symbol(foo)"
s2.toString() // "Symbol(bar)"
```

`Symbol`函数的参数只是表示对当前 Symbol 值的描述，因此相同参数的`Symbol`函数的返回值是不相等的。

```javascript
Symbol('foo') === Symbol('foo') // false
```

Symbol不能与其他类型的值进行运算，会报错。

Symbol可以显示转换为字符串,也可以转为布尔值但不能为数值

```javascript
let foo = Symbol('foo')

String(foo) // 'Symbol(foo)'
foo.toString() // 'Symbol(foo)'

Boolean(foo) // true
!foo // false

foo+2 // TypeError
Number(foo) // TypeError
```

## 作为属性名Symbol

由于每一个Symbol都是唯一的，所以可以保证不出现同名的属性。

```javascript
let foo = Symbol('foo')

// 1
let bar = {}
bar[foo] = 'hello'

// 2
let bar = {
  [foo]: 'hello' // 如果foo不放在[]中，该属性就为foo，而非Symbol('foo')
}

// 3
let bar = {}
Object.defineProperty(bar,foo,{value: 'hello'})

// 以上写法都能得到如下结果
bar[foo] = 'hello'
```

注意：

使用Symbol作为属性名，该属性不会出现在for...in、for...of循环中，也不会被Object.keys()\Object.getOwnPropertyNames()\JSON.stringfly()返回。

Object.getOwnPropertySymbols方法，可以获取制定对象的所有Symbol属性名。

Object.getOwnPropertySymbol方法返回一个数组，成员是当前对象的所有用作属性名的Symbol值。

```javascript
const obj = {}
let foo = Symbol('foo')
let bar = Symbol('bar')
obj[foo] = 'foo'
obj [bar] = 'bar'

let symbols = Object.getOwnPropertySymbols(obj)

symbols // [Symbol(foo),Symbol(bar)]
```

另一个新的api，Reflect.ownKeys方法可以返回所有类型的键名，包括常规键名和Symbol键名。

```javascript
let obj = {
    [Symbol('foo')]: 'foo',
  bar: 'bar'
}
Reflect.ownKeys(obj)
// [Symbol(foo),bar]
```

由于Symbol值作为名称的属性，不会被常规的方法遍历得到，可以利用这个特性，为对象定义一些非私有的、但又希望只用于内部的方法。

## Symbol.for()、Symbol.keyFor()

`Symbol.for()`与`Symbol()`这两种写法，都会生成新的Symbol。它们的区别是，前者会被登记在全局环境中供搜索，后者不会。`Symbol.for()`不会每次调用就返回一个新的 Symbol 类型的值，而是会先检查给定的`key`是否已经存在，如果不存在才会新建一个值。

```javascript
Symbol.for('foo') === Symbol.for('foo') // true
Symbol('foo') === Symbol('foo') // false
```

`Symbol.for`为Symbol值登记的名字，是全局环境的，可以在不同的 iframe 或 service worker 中取到同一个值。

`Symbol()`写法没有登记机制，所以每次调用都会返回一个不同的值。

`Symbol.keyFor`方法返回一个已登记的 Symbol 类型值的`key`。

```javascript
let foo = Symbol.for('foo')
Symbol.keyFor(foo) // "foo"

let bar = Symbol('bar')
Symbol.keyFor(bar) // undefined
```