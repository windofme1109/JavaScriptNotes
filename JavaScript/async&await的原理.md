<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [`async` / `await` 的原理](#async--await-%E7%9A%84%E5%8E%9F%E7%90%86)
  - [1. 参考资料](#1-%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99)
  - [2. 基本说明](#2-%E5%9F%BA%E6%9C%AC%E8%AF%B4%E6%98%8E)
  - [3. Generator](#3-generator)
  - [4. Generator 的基本使用](#4-generator-%E7%9A%84%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8)
    - [1. 没有返回值的 `Generator` 函数](#1-%E6%B2%A1%E6%9C%89%E8%BF%94%E5%9B%9E%E5%80%BC%E7%9A%84-generator-%E5%87%BD%E6%95%B0)
    - [2. 有返回值的 generator 函数](#2-%E6%9C%89%E8%BF%94%E5%9B%9E%E5%80%BC%E7%9A%84-generator-%E5%87%BD%E6%95%B0)
    - [3. `yield` 后面接函数](#3-yield-%E5%90%8E%E9%9D%A2%E6%8E%A5%E5%87%BD%E6%95%B0)
    - [4. `yield` 后面接 Promise](#4-yield-%E5%90%8E%E9%9D%A2%E6%8E%A5-promise)
    - [5. `next()` 函数传参](#5-next-%E5%87%BD%E6%95%B0%E4%BC%A0%E5%8F%82)
  - [5. 实现 `async` 和 `await`](#5-%E5%AE%9E%E7%8E%B0-async-%E5%92%8C-await)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# `async` / `await` 的原理

## 1. 参考资料

1. [ECMAScript 6 入门 - async 函数的实现原理](https://es6.ruanyifeng.com/#docs/async#async-%E5%87%BD%E6%95%B0%E7%9A%84%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86)

2. [co 函数库的含义和用法](https://www.ruanyifeng.com/blog/2015/05/co.html)

3. [Thunk 函数的含义和用法](ruanyifeng.com/blog/2015/05/thunk.html)

4. [async await 实现原理分析](https://zhuanlan.zhihu.com/p/157282332)

5. [7张图，20分钟就能搞定的async/await原理！为什么要拖那么久？](https://juejin.cn/post/7007031572238958629)

## 2. 基本说明

1. `async` / `await` 是 `Generator` 的语法糖，使用 `Generator` 函数和自定执行器同样可以实现 `async` / `await` 的效果。但是 `async` / `await` 的优势如下：
    1. 内置执行器。`Generator` 函数的执行必须依靠执行器，而 `aysnc` 函数自带执行器，调用方式跟普通函数的调用一样。
    2. 更好的语义。`async` 和 `await` 相较于 `*` 和 `yield` 更加语义化。
    3. 更广的适用性。`co` 模块（第三方的 `Generator` 执行器）约定，`yield` 命令后面只能是 `Thunk` 函数或 `Promise` 对象。而 `async` 函数的 `await` 命令后面则可以是 `Promise` 或者原始类型的值（`number`，`string`，`boolean`，但这时等同于同步操作）。
    4. 返回值是 `Promise`。`async` 函数返回值总是 `Promise` 对象，比 `Generator` 函数返回的 `Iterator` 对象方便，可以直接使用 `then()` 方法进行调用。

## 3. Generator

1. `Generator` 是 ES6 引入的一种异步编程的解决方案。主要作用是可以中断和恢复函数的执行。对于异步任务而言，我们先中断 `Generator` 函数的执行，接着去执行异步任务，并等待异步任务的结果，当异步任务返回结果以后，再恢复 `Generator` 函数的执行，这种中断-等待-恢复的过程，使得我们可以不用使用回调函数，而以同步的方式执行异步任务。

2. `Generator` 函数跟普通函数在定义上的区别就是，在 function 关键字后面多了一个星号 `*`，并且只有在 `Generator` 函数中才能使用 `yield` 关键字。 

3. 什么是 `yield` 呢，它相当于`Generator` 函数执行的中途暂停点，也就是遇到 `yield` 关键字，`Generator` 函数函数就中断执行。

4. 怎么才能暂停后继续走呢？我们执行 `Generator` 函数后，其返回值是一个 `Iterator` 对象，`Iterator` 对象有一个 `next()` 方法，这个`next()` 用来执行 `Generator` 函数内部的代码。即调用一次 `next()`，`Generator` 函数从上次中断的地方开始向下执行，遇到 `yield` 停止执行。`next()` 方法执行后会返回一个对象，对象中有 `value` 和 `done` 两个属性：
    - `value`：暂停点后面接的值，也就是 `yield` 后面接的值
    - `done`： `Generator` 函数是否已走完，没走完为 `false`，走完为 `true`，走完的标准是执行完整个 `Generator` 函数或者遇到 `return` 或者调用了 `Iterator` 对象的 `return()` 方法。

## 4. Generator 的基本使用

### 1. 没有返回值的 `Generator` 函数
   
1. 示例代码
   ```js
      function* gen() {
          yield 1;
          yield 2;
          yield 3;
      }

      const g = gen();

      // { value: 1, done: false }
      console.log(g.next());
      // { value: 2, done: false }
      console.log(g.next());
      // { value: 3, done: false }
      console.log(g.next());

      
      // { value: undefined, done: true }
      console.log(g.next());

   ```
2. 最后一次调用 `next()`，`value` 实际上是 undefined，done 为 true，表示 `Generator` 函数已经走完。当 `Generator` 函数走完时，`value` 接收的是 `Generator` 的返回值，因为我们这个 `Generator` 没有返回值，所以 `value` 是 undefined。

### 2. 有返回值的 generator 函数

1. 示例代码：
   ```js
      function* gen2() {
           yield 'a';
           yield 'b';
           yield 'c';

           return 'd'
       }

       const g2 = gen2();
       // { value: 'a', done: false }
       console.log(g2.next());
       // { value: 'b', done: false }
       console.log(g2.next());
       // { value: 'c', done: false }
       console.log(g2.next());
       // { value: 'd', done: true }
       console.log(g2.next());
   ```
2. `Generator` 函数有返回值以后，那么最后一次调用的 `next()`，`next()` 函数的返回值对象的 `value` 属性就是 `Generator` 函数的返回值。

### 3. `yield` 后面接函数

1. `yield` 后面接函数的话，到了对应暂停点：`yield` 语句，会马上执行此函数，并且该函数的执行返回值，会被当做此暂停点对象的 `value`，即 `next()` 函数的返回值的 `value` 属性。

2. 示例代码：
   ```js
      function fn(num) {
          return num;

      }
       
      function* gen() {
          yield fn(1);
          yield fn(2);

          return 3;
      }

      const g = gen();

      // { value: 1, done: false }
      console.log(g.next());
      // { value: 2, done: false }
      console.log(g.next());
      // { value: 3, done: true }
      console.log(g.next());
   ```
3. `fn()` 的返回值作为 `next()` 返回值的 `value` 属性。

### 4. `yield` 后面接 Promise

1. 因为函数执行返回值会当做暂停点对象的 `value` 值，当`yield` 后面是 `Promise` 对象时，`next()` 函数的返回值的 `value` 就是是 pending 状态的 `Promise` 对象。

2. 示例代码：
   ```js
      function func(num) {
          return new Promise((resolve, reject) => {
              setTimeout(() => {
                  resolve(num);
              }, num * 1000)
          });
      }

      function* gen() {
      yield func(1);
      yield func(2);

          return 3;
      }

      const g = gen();

      // { value: Promise { <pending> }, done: false }
      console.log(g.next());
      // { value: Promise { <pending> }, done: false }
      console.log(g.next());
      // { value: 3, done: true }
      console.log(g.next());
   ```
3. 调用 `next()`，拿到的是 pending 状态的 `Promise` 的结果 。如果想拿到 `Promise` 最终的状态，那么需要调用 `then()` 方法，如下所示：
   ```js
      const next1 = g.next();
      next1.value.then((res) => {
          // 1s 后输出 1
          console.log(res);
      });

      const next2 = g.next();
      next2.value.then((res) => {
          // 2s 后输出 2
          console.log(res);
      });
   ```
### 5. `next()` 函数传参

1. `next()` 方法可以接收一个参数，并且可以通过 `yield` 来接收这个参数，注意两点：
   1. 第一次 `next()` 传参是没用的，只有从第二次开始 `next()` 传参才有用。
   2. `next()` 传值时，要记住顺序是，先右边 `yield`，后左边接收参数。

2. `next()` 接收的参数，会作为 `Generator` 函数内部整体的 `yield` 表达式的值。

3. 示例代码 - 不给 `next()` 传参：
   ```js
      function* gen() {
          const num1 = yield 1;
          console.log(num1);

          const num2 = yield 2;
          console.log(num2);

          return 3;
      }

       // 不给 next() 传参
       const g = gen();
      
       // { value: 1, done: false }
       console.log(g.next());

       // undefined
       // { value: 2, done: false }
       console.log(g.next());

       // undefined
       // { value: 3, done: true }
       console.log(g.next());
   ```
   第一次调用 `next()`，返回值是：`{ value: 1, done: false }`。第二次调用 `next()`，从刚刚暂停的地方开始执行，也就是先执行：`console.log(num1);`， 因为 `yield` 本身没有返回值，所以 `num1` 是 `undefined`，因此输出 `undefined`。 
   接着执行 `const num2 = yield 2`，所以 `next()` 的返回值是：`{ value: 1, done: false }`。第三次调用 `next()`，从刚刚暂停的地方开始执行，也就是先执行：`console.log(num2)`。因为 `yield` 本身没有返回值，所以 num2 是 `undefined`，因此输出 `undefined`，接着执行 `return 3`，所以 `next()` 的返回值是：`{ value: 3, done: true }`。

4. 示例代码 - `next()` 传参：
   ```js
      // 第一次调用 next()，不需要传值
      // { value: 1, done: false }
      console.log(g.next());

      // 第二次调用 next()，传值
      // 1111
      // { value: 2, done: false }
      console.log(g.next(1111));


      // 2222
      // { value: 3, done: true }
      console.log(g.next(2222));
   ```
   第一次调用 `next()`，没有传值，返回值是：`{ value: 1, done: false }`。 
   第二次调用 `next()`，从刚刚暂停的地方开始执行，也就是先执行：`console.log(num1)`。我们给 `next()` 传了 1111，因此这个参数会作为 `yield` 的返回值，因此 `nums1` 是 1111。所以这个 `console` 语句输出 1111，接着执行 `const num2 = yield 2`，所以 `next()` 的返回值是：`{ value: 1, done: false }`。
   第三次调用 `next()`，从刚刚暂停的地方开始执行，也就是先执行：`console.log(num2)`， 我们给 `next()` 传了 2222，因此这个参数会作为 `yield` 的返回值，因此 `nums2` 是 2222，接着执行 `return 3`， 所以 `next()` 的返回值是：`{ value: 3, done: true }`。

### 6. `yield` 后面接 `Promise` + `next()` 传参

1. `yield` 后面接一个 `Promise`，就类似于 `await` 后面跟一个异步任务。

2. `next()` 传参，可以作为 `yield` 表达式的返回值，那么我们将 `Promise` 对象变为 resolved 状态时的值，传给 `next()`，那么相当于 `Generator` 函数内部的 `yield` 表达式有了结果，这样我们就在 `Generator` 函数内部拿到了异步操作的结果，就要 `await` 那种感觉了。

3. 示例代码：
   ```js
        function func(num) {
            return new Promise((resolve, reject) => {
                setTimeout(() => {
                   resolve(num * 2);
                }, num * 1000)
            });
        }

        function* gen() {
            // 调用 Promise 函数
            const num1 = yield func(1);
            const num2 = yield func(num1);
            const num3 = yield  func(num2);
            return num3;
        }

        const g = gen();
        // 传参
        const next1 = g.next();
        next1.value.then(res1 => {
           // { value: Promise { 2 }, done: false }
           console.log('next1: ', next1);
            // res1 为 2
            console.log('res - 1 : ', res1);
   
            const next2 = g.next(res1);
   
            next2.value.then(res2 => {
   
            // { value: Promise { 4 }, done: false }
            console.log('next2: ', next2);
            
            // res2 为 4
            console.log('res - 2 : ', res2);
            const next3 = g.next(res2);
            next3.value.then(res3 => {
                  
                  // { value: Promise { 8 }, done: false }
                  console.log('next3: ', next3);
                  // res3 为 8
                  console.log('res - 3 : ', res3);
                  const next4 = g.next(res3);
                  // { value: 8, done: true }
                  console.log(next4);
              })
          })
      })
   ```
   next1 的 `value` 是 pending 状态的 `Promise`，调用其 `then()` 方法，等到 `Promise` 变为 resolved 状态，就会调用 `then()` 的 resolved 回调函数。初始传入 `func()` 的值为 1，等到 1s 后，`Promise` 状态变为 resolved，此时值会变为 2（`resolve(num * 2)`），所以 next1 为 `{ value: Promise { 2 }, done: false }`。
   将 res1 传入 `next()` 中，那么 res1 就会作为第一个 `yield` 表达式的返回值，所以 `gen()` 中，num1 就是 2。所以传入第二个 `yield` 表达式中的 `func()` 的就是 2。
   next2 是第二个 `yield` 表达式中 `func()` 的返回值。等待 2s 后，第二个 `Promise` 变为 resolved 状态，所以 res2 为 4。
   将 res2 作为 `next()` 的参数传入，那么第二个 `yield` 表达式的结果就是 4，所以 `gen()` 中，num2 就是 4。所以传入第三个 `yield` 表达式中的 `func()` 的就是 4。
   next3 是第三个 `yield` 表达式中 `func()` 的返回值。等待 4s 后，第三个 `Promise` 变为 resolved 状态，所以 res3 是 8。 
   将 res3 作为 `next()` 的参数传入，那么第三个 `yield` 表达式的结果就是 8，所以 `gen()` 中，num3 就是 8。
   next4 就是 `gen()` 函数 return 的结果，所以返回值的 `value` 属性为 8，`done` 属性为 `true`。

4. 借用一张图来描述上面的过程：
   ![](./img/promise-next.awebp)
    图片来源：[7张图，20分钟就能搞定的async/await原理！为什么要拖那么久？](https://juejin.cn/post/7007031572238958629)

## 5. 实现 `async` 和 `await`

1. `yield` 后面跟 `Promise` 和 `next()` 传参结合，就已经实现了 `async` / `await` 基本功能，但是与官方的 `async` / `await` 相比，区别如下：
   1. `async` 函数返回的是 `Promise` 对象，而 `Generator` 函数返回的不是 `Promise` 对象。
   2. 需要我们手动执行 `next()`。
   3. 执行 `next()` 的次数与 `yield` 的数量相关，有多少个 `yield` 语句，就需要执行多少次 `next()`。而 `async` 函数内部无论有多少个 `await`，都能自动执行。

2. 我们可以封装一个高阶函数，用来处理 `Generator` 函数，使其能返回一个具有 `async` 功能的函数。
   ```js
      function generatorToAsyncFunc(generatorFun) {


          // 具有 async 功能的函数
          return (...args) => {
              // ...
          }
      }
   ```
3. `async` 函数的返回值是 `Promise` 对象，因此，generatorToAsyncFunc() 返回值函数的返回值就必须是 `Promise`，如下所示：
   ```js
      function generatorToAsyncFunc(generatorFun) {


          // 具有 async 功能的函数
          return (...args) => {
              return new Promise((resolve, reject) => {
                  // ...
              })
          }
      }
      
      function* gen() {
        
      }
   
      const asyncFn = generatorToAsync(gen);
      // Promise
      console.log(asyncFn()) 
   ```

4. 加入一系列的操作：
   ```js
      function generatorToAsyncFunc(generatorFun) {


          // 具有 async 功能的函数
          return (...args) => {

              // generatorFun 函数有可能接收参数
              const gen = generatorFun.apply(this, args);

              // 因为 async 函数返回的 `Promise` 对象，所以，这里返回一个 `Promise` 对象
              return new Promise((resolve, reject) => {

                  /**
                   * 自动执行 generator 函数返回的 Iterator 对象中的 next() 函数
                   * @param key next 或者 throw
                   * @param arg 传给 next() 或者 throw() 函数的参数
                   */
                  function go(key, arg) {
                      let res;
                      // 因为某个 yield 语句中，执行的 Promise 可能 rejected 状态的 Promise，所以这里使用 try catch 包裹
                      // Generator 函数返回的 Iterator 对象，都有一个 throw 方法，可以在函数体外抛出错误，然后在 Generator 函数体内捕获
                      try {
                          res = gen[key](arg);
                      } catch (err) {
                          // 如果是 rejected 状态的 Promise，我们调用 Iterator 对象的 throw 方法，然后将结果传入 reject 函数中
                          return reject(err);
                      }

                      // res 为 next() 执行的结果，所以从 next 对象中，取出 value 和 执行状态 done
                      const {value, done} = res;

                      if (done) {
                          // done 为 true，表示当前的 generator 已经走完，所以 将 Promise 状态变为 resolved 状态，并传入 value
                          return resolve(value);
                      } else {
                          // done 为 false，我们需要继续向下遍历，因为 value 有可能是 常量，Promise 对象
                          // 而 Promise 对象又有 resolved 状态或者 rejected 状态，所以这里统一使用 Pormise.resolve() 进行包装
                          // 如果是常量，直接调用 then() 的resolved 状态的回调函数
                          // 如果是 Promise 对象，那么 Promise.resolve(value) 就直接返回这个 Promise 对象
                          // 后面是调用 then() 的 resolved 状态的函数还是 rejected 状态的函数，就根据 这个 Promise 对象状态而定
                          return Promise.resolve(value).then(val => go('next', val), err => go('throw', err));
                      }
                  }

                  // 执行第一个 next
                  go('next');

              });
          }

      }
   ```

5. 核心操作是 `go()` 函数，`go()` 函数是一个自动执行器，将 `Iterator` 对象中的 `next()` 依次执行。`go()` 接收两个参数，第一个是执行 `Iterator` 对象的哪个方法：`next` 还是 `throw`，而第二个参数就是执行 `next` 或者 `throw` 方法时传入的参数。`go()` 的执行过程如下：
   - 根据 `go()` 的 `key` 参数，确定执行的是 `next` 还是 `throw`。执行 `next()`，可以理解为是 `Promise` 的 `resolved` 状态，而 throw() 可以理解为 `Promise` 状态的`rejected` 状态。如果执行的是 `throw()`，如果 `Generator` 函数内部有 `try...catch` 代码块包裹，那么 `throw()` 函数抛出的异常会在 `Generator` 函数内部处理，否则就在外部处理。因此我们使用 `try...catch` 包裹了 `gen[key](arg)`，在异常发生时，我们直接将 `async` 函数的返回的 `Promise` 状态变为 `rejected`。
   - res 为 `next()` 执行的结果，所以从 `res` 对象中，取出 `value` 和 执行状态 `done`。
   - `done` 为 `true`，表示当前的 `Generator` 已经走完，所以 将 `async` 函数的返回的 `Promise` 状态变为 resolved 状态，并传入 `value`。
   - `done` 为 false，我们需要继续向下遍历，因为 `value` 有可能是常量 或者是 `Promise` 对象。所以这里统一使用 `Pormise.resolve()` 进行包装。
     - 如果 `value` 是常量，直接调用 `then()` 的 resolved 状态的回调函数。
     - 如果 `value` 是 `Promise` 对象，那么 `Promise.resolve(value)` 就直接返回这个 `Promise` 对象，后面是调用 `then()` 的 resolved 状态的函数还是 rejected 状态的函数，就根据这个 `Promise` 对象状态而定。
   - 在 `then()` 方法接收的回调函数参数中，我们递归调用 `go()` 函数，这块是实现自动执行的关键，也就是，只要 done 不为 true，我们就递归执行 `go()`。每执行一次 `go()`，`Generator` 函数就向下走一段（从上一次中断的地方到 `yield` 表达式）。这样递归的过程，有点类似与 Koa 的中间件执行的的过程，只不过 Koa 中我们得手动触发 `next()` 才能实现对下一个中间件的调用。而 `go()` 函数是自动调用的，通过 done 来判断是否要终止调用。
   - 根据 `value` 的状态，来决定下一次是调用 `next()` 还是 `throw()`。这样能处理异步任务执行过程中出现的异常。
     - 如果 `value` 是常量，直接调用 `then()` 的 resolved 状态的回调函数。在 resolved 状态的回调函数中，调用 `go('next', val)`，将 `value` 直接注入 `Generator` 函数中。
     - 如果 `value` 是 `Promise` 对象，那么 `Promise.resolve(value)` 就直接返回这个 `Promise` 对象。此时 `value` 表示一个异步任务，异步任务成功，`value` 代表的 `Promise` 对象变成 resolved 状态，此时是调用 `then()` 方法的 resolved 状态的回调函数，在这个回调函数内，调用 `go('next', val)`，将 `Promise` 对象 resolved 状态的值传入 `go()` 函数内，即相当于调用 `next(val)`，因此 `Generator` 函数内部可以拿到异步任务成功的结果。而 `value` 表示的异步任务如果抛出异常，则 `Promise` 状态变为 rejected 状态，此时会调用 `then()` 方法的 rejected 状态的回调函数，在这个函数内部调用 `go('throw', err)`，相当于调用 `throw(err)`，主动抛出异常，这样无论在 `Generator` 函数内部还是外部，只要有 `try...catch` 包裹，就能捕获这个异常。
   - 最后我们需要手动执行一下 `go('next')`，即启动 `Generator` 函数。
