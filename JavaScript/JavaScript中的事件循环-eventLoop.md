# JavaScript 中的 event loop

## 1. 参考资料

1. [这一次，彻底弄懂 JavaScript 执行机制](https://juejin.im/post/59e85eebf265da430d571f89#heading-2)

2. [浏览器与Node的事件循环(Event Loop)有何区别?](https://zhuanlan.zhihu.com/p/54882306)

3. [又被node的eventloop坑了，这次是node的锅](https://zhuanlan.zhihu.com/p/54951550)

4. [js事件循环机制(浏览器端Event Loop) 以及async/await的理解](https://www.cnblogs.com/smile-fanyin/p/14622432.html)

5. [js事件循环机制(浏览器端Event Loop) 以及async/await的理解](https://segmentfault.com/a/1190000017554062)

6. [Async、Await 从源码层面解析其工作原理](https://zhuanlan.zhihu.com/p/143826516)

7. [async/await 在chrome 环境和 node 环境的 执行结果不一致，求解？](https://www.zhihu.com/question/268007969)

8. [微任务、宏任务与Event-Loop](https://juejin.cn/post/6844903657264136200)

## 2. 事件循环的概念

### 1. 异步

1. JavaScript 是单线程的，也就是说，在同一时间只能执行一个任务，其他任务必须在后面排队等待。采用单线程的好处是实现起来简单，避免 DOM 渲染时的冲突，但缺点就是只要有一个任务耗时很长，后面的任务都必须排队等着，会拖延整个程序的运行。比如等待 AJAX 请求返回结果，如果对方的服务器迟迟没有响应，或者网络不通畅，就会导致脚本的长时间停滞。

2. 为了解决这个问题，JavaScript 语言将任务的执行模式分为两种：同步和异步。

3. 同步任务是在主线程上排队执行的任务，只有前一个任务执行完毕，才会执行后一个任务。异步任务是在任务队列中的任务，只有引擎认为某个异步任务可以执行了，该任务采用回调函数的形式，进入到主线程去执行。JavaScript 通过事件轮询，也就是 event loop 来确定任务队列中的异步任务能不能进入主线程执行。

### 2. 事件循环（event loop）

1. JavaScript中，任务分为两种，同步任务和异步任务，在执行过程中进入不同的场所，同步任务进入主线程执行，而异步任务则进入Event Table并注册回调函数。

2. 当指定的事件触发时，Event Table 将这个函数推入 Event Queue。

3. 主线程的任务执行完毕，会进入 Event Queue 取出对应的函数，放到主线程去执行。

4. 上述过程不断执行，就是常说的事件轮询（Event Loop）。

5. 维基百科的定义是：“事件循环是一个程序结构，用于等待和发送消息和事件（
   > a programming construct that waits for and dispatches events or messages in a program.


### 3. 异步操作的模式

1. 回调函数
    - 回调函数的优点是简单、容易理解和实现。缺点是容易出现回调地狱现象。使得程序结构混乱，复杂，难以维护。

2. 事件监听
    - 异步任务的执行不取决于代码的执行，而取决于某个事件是否发生。

3. 发布/订阅

4. Promise

5. async/await

## 3. 宏任务

## 4. 微任务

## 5. async / await

## 6. 事件循环 - 同步任务、宏任务、微任务、async/await 执行顺序