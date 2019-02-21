# EventLoop

> Event Loop 是一个执行模型

- 浏览器的 Event Loop 是在 html5 的规范中明确定义。
- NodeJS 的 Event Loop 是基于 libuv 实现的。可以参考 Node 的官方文档以及 libuv 的官方文档。
- libuv 已经对 Event Loop 做出了实现，而 HTML5 规范中只是定义了浏览器中 Event Loop 的模型，具体的实现留给了浏览器厂商

参考：

- [带你彻底弄懂 Event Loop](https://juejin.im/post/5b8f76675188255c7c653811)

## 宏任务和微任务

### macrotask 宏任务

异步任务的回调会依次进入 macrotask queue

- setTimeout
- setInterval
- setImmediate (Node 独有)
- requestAnimationFrame (浏览器独有)
- I/O
- UI rendering (浏览器独有)

### microtask 微任务

异步任务的回调会依次进入 microtask queue

- process.nextTick (Node 独有)
- Promise
- Object.observe
- MutationObserver

## 浏览器 EventLoop

1. 执行 Script 同步代码，这些同步代码可能包含一些异步语句（比如 setTimeout、Promise）
2. Script 代码执行完毕后，从 `microtask queue` 中依次取出 `microtask` 执行，直到的所有的任务执行完，`microtask queue` 为空

!> 如果在执行 microtask 的过程中，又产生了 microtask，那么也会加入到 `microtask queue` 的末尾，也会在这个周期被调用执行
3. 浏览器自行决定是否 `rendering`
4. 从 `macrotask queue` 中取出位于队首的一个 `macrotask` 执行
5. 重复 2 - 4

简单理解为：

1.script 执行完 -> 2.执行所有微任务队列里的所有微任务(以及微任务产生的微任务) -> 3.浏览器自行决定是否渲染 -> 4.执行一个宏任务 -> 重复 2->4

## Nodejs EventLoop

### Nodejs 宏任务队列

NodeJS 的 Event Loop 中，执行宏任务队列的回调任务有 6 个阶段:

- timers 阶段：这个阶段执行 setTimeout 和 setInterval 预定的 callback
- I/O callback 阶段：执行除了 close 事件的 callbacks、被 timers 设定的 callbacks、setImmediate()设定的 callbacks 这些之外的 callbacks
- idle, prepare 阶段：仅 node 内部使用
- poll 阶段：获取新的 I/O 事件，适当的条件下 node 将阻塞在这里
- check 阶段：执行 setImmediate()设定的 callbacks
- close callbacks 阶段：执行 socket.on('close', ....)这些 callbacks

主要位于 4 个 `macrotask queue` 中:

1. Timers Queue
2. IO Callbacks Queue
3. Check Queue
4. Close Callbacks Queue

### Nodejs 微任务队列

主要有 2 种 `microtask queue`:

1. Next Tick Queue：是放置process.nextTick(callback)的回调任务的
2. Other Micro Queue：放置其他microtask，比如Promise等

### Nodejs EventLoop 顺序

1. 执行 Script 同步代码，这些同步代码可能包含一些异步语句
2. 执行 `microtask`
  1. 执行 `Next Tick Queue` 中的所有任务
  2. 执行 `Other Microtask Queue` 中的所有任务
3. 执行 `macrotask`, 从第一个阶段开始执行相应每一个阶段macrotask中的所有任务,直到每一个阶段的`macrotask`执行完毕
4. 重复 2 - 3

## 浏览器 和 Nodejs EventLoop 对比

|                 | 浏览器                          | Nodejs                              |
| --------------- | ------------------------------- | ----------------------------------- |
| macrotask queue | 一个，每次执行queue中的一个任务 | 多个，每次执行多个queue中的所有任务 |
| microtask queue | 一个，每次执行queue中的所有任务 | 多个，每次执行多个queue中的所有任务 |

速记差别：

1. 浏览器宏任务、微任务队列只有一个，Nodejs有多个
2. 浏览器宏任务每次只执行队列里一个任务，node每次执行所有宏任务队列里的所有任务