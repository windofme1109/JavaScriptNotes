<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Promise](#promise)
  - [1. 参考资料](#1-%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99)
  - [2. 基本说明](#2-%E5%9F%BA%E6%9C%AC%E8%AF%B4%E6%98%8E)
  - [3. 基本用法](#3-%E5%9F%BA%E6%9C%AC%E7%94%A8%E6%B3%95)
  - [4. `Promise` 的链式调用](#4-promise-%E7%9A%84%E9%93%BE%E5%BC%8F%E8%B0%83%E7%94%A8)
  - [5. 常用方法](#5-%E5%B8%B8%E7%94%A8%E6%96%B9%E6%B3%95)
    - [1. Promise.prototype.then()](#1-promiseprototypethen)
    - [2. Promise.prototype.catch()](#2-promiseprototypecatch)
    - [3. Promise.prototype.finally()](#3-promiseprototypefinally)
    - [5. Promise.reject()](#5-promisereject)
    - [6. Promise.all()](#6-promiseall)
    - [7. Promise.race()](#7-promiserace)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Promise

## 1. 参考资料

1. [Promise 对象](https://es6.ruanyifeng.com/#docs/promise)

2. [当面试官问Promise的时候他想知道什么](https://juejin.cn/post/6953127598457094174)

3. [ES6基础知识】promise和await/async](https://juejin.cn/post/6868138778306412552)

4. [promise和await async进阶](https://juejin.cn/post/6844904180096712711)

5. [7张图，20分钟就能搞定的async/await原理！为什么要拖那么久？](https://juejin.cn/post/7007031572238958629)

6. [看了就会，手写Promise原理，最通俗易懂的版本！！！](https://juejin.cn/post/6994594642280857630)

7. [你真的完全掌握了promise么？](https://juejin.cn/post/6844903604009041928)

8. [Promise 必知必会（十道题）](https://juejin.cn/post/6844903509934997511)

9. [面试精选之Promise](https://juejin.cn/post/6844903625609707534)

10. [整体流程的介绍](https://juejin.cn/post/6856213486633304078)

11. [Promise实现原理（附源码）](https://juejin.cn/post/6844903665686282253)

12. [这一次，彻底弄懂 Promise 原理](https://juejin.cn/post/6844904063570542599)

13. [Promise不会？？看这里！！！史上最通俗易懂的Promise！！！](https://juejin.cn/post/6844903607968481287)

14. [从一道让我失眠的 Promise 面试题开始，深入分析 Promise 实现细节](https://juejin.cn/post/6945319439772434469)

15. [使用 Promise - MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Using_promises)

16. [Promise - MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)

## 2. 基本说明

1. 异步编程的一种解决方案。使用 `Promise` 完成异步任务，可以避免回调地狱的出现。写法更简单，语义更明确。

2. `Promise` 既是一个对象，又是一个构造函数。可以将其看做为一个容器，里面存放着未来才会完成的异步任务。

3. `Promise` 作为构造函数，接收一个执行器函数。这个执行器函数实际上就是一个异步任务。执行器函数接收两个参数：`resolve` 和 `reject`。当异步任务成功时， 调用 resolve 函数，将数据传入 `resolve` 函数，当异步任务失败时，调用 `reject` 函数，将错误信息传入 reject 函数中。

4. 一共有三种状态：`pending`（等待）、`fulfilled`（成功）、`rejected`（失败）。转态转换只能是：`pending` --> `fulfilled` 或者`pending` --> `rejected`。

5. 状态一旦发生变化，就不可逆转。`Promise` 对象就一直保持这个状态。在任何时候都可以获取这个状态的数据。

7. 以下内容引用自 《ECMAScript 6 入门》
   > 1. 对象的状态不受外界影响。`Promise` 对象代表一个异步操作，有三种状态：pending（进行中）、`fulfilled`（已成功）和 `rejected`（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是 `Promise` 这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。
   > 2. 一旦状态改变，就不会再变，任何时候都可以得到这个结果。`Promise`对象的状态改变，只有两种可能：从 pending 变为 `fulfilled` 和从 pending 变为 `rejected`。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 `resolved`（已定型）。如果改变已经发生了，你再对 `Promise` 对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

8. 有了 `Promise` 对象，就可以将异步操作以同步操作的流程表达出来，避免了回调函数的层层嵌套。此外，`Promise` 对象提供了统一的接口，使得控制异步操作，更加容易。

9. `Promise` 也有一些缺点：
   1. 无法取消 `Promise`，一旦新建它就会立即执行，无法中途取消。2. 如果不设置回调函数，`Promise` 内部抛出的错误，不会反应到外部。
   3. 当处于 pending 状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

10. 下面用一张图来表示 `Promise` 对象状态的改变：
   ![](./img/promise.png)

## 3. 基本用法

1. `Promise` 是一个构造函数，下面的代码创建一个 `Promise` 示例：
   ```js
      var fs = require('fs') ;

      // 新建 `Promise` 对象
      var p1 = new Promise(function(resolve, reject) {
	     // 异步操作
         fs.readFile('./data/a.txt', 'utf-8', function(err, data) {
              if (err) {
                 // 任务失败
                 // 容器的pending状态转变为rejected
                 reject(err) ; 
              } else {
                 // 任务成功
                // 容器的pending状态转变为resolved
                resolve(data) ;
              }
          })
      })

   ```
2. `Promise` 构造函数接收一个函数作为参数，这个函数接收两个参数：`resolve`，`reject`。通常这个作为参数的函数内部会封装一个异步任务。

3. `resolve()` 函数的作用是将 `Promise` 对象的状态由“未完成”变为“成功”（pending状态转变为 `resolved`）。在异步操作成功时调用，并将异步操作的结果，作为参数传递出去。

4. `reject()` 函数的作用是将`Promise`对象的状态由“未完成”变为“失败”（pending状态转变为reject）。在异步操作失败时调用，并将错误对象，作为参数传递出去。

5. 在 `Promise` 对象生成以后，我们可以通过 `then()` 方法指定 `resolved` 状态和 `rejected` 状态的回调函数。

6. 这个回调函数指的是在 `resolved` 状态和 `rejected` 状态下如何处理数据和错误。

7. 使用 `then()` 
   ```js
      p1.then(function(result) {
          console.log('成功') ;
          console.log(result) ;
      }, function(err) {
          console.log('失败') ;
          console.log(err) ;
      })

   ```
9. `then()` 接收两个函数作为参数，其中第二个参数可选。第一个函数是 `Promise` 对象状态变为 `resolved` 时调用，第二个函数是 `Promise` 对象状态变为 `rejected` 时调用。
10. 两个函数都接收 `Promise` 传出的值作为参数。

11. 使用 `Promise` 包装 ajax 请求的例子：
    ```js
       function  myRequest(url, options) {
          return new Promise((resolve, reject) => {
              const xhr = new XMLHttpRequest();
               xhr.onreadystatechange = function () {
                   if (xhr.readyState === 4) {
                       if (xhr.status === 200) {
                           resolve(xhr.response);
                       } else {
                           reject(xhr.response);
                       }
                   }
               }

               xhr.open('GET', url);
               xhr.send();
          })
      }

      myRequest('/getInfo').then((res) => {
          console.log('响应是：', res);
      }, (err) => {
          console.log('出错了：', err);
      })
    ```

## 4. `Promise` 的链式调用

1. `Promise` 对象的一大特色就是可以实现 `then()` 方法的链式调用。链式调用也是 `Promise` 用来解决异步任务中回调函数嵌套的问题的。

2. 实现链式调用的前提是 `then()` 方法可以返回一个新的 `Promise` 示例，因此我们可以继续调用 `then()` 方法。

3. 一个简单的链式调用：
   ```js
      const p1 = new Promise((resolve, reject) => {

          setTimeout(() => {
              resolve('时间到了，触发 resolve');
          }, 1000);
      });

      // 简单的链式调用

      p1
         .then((res) => {
             console.log('then1: ', res);
             return res;
         })
         .then((res) => {
            console.log('then2: ', res);
            return res;
         })
         .then((res) => {
             console.log('then3: ', res);
         });
      // 输出
      // then1:  时间到了，触发 resolve
      // then2:  时间到了，触发 resolve
      // then3:  时间到了，触发 resolve
   ```

4. 上面的例子中，第一个 `then()` 和第二个 `then()` 方法返回的是普通的值，同样会被包装成 `Promise` 对象，且会被传入 resolve 函数中，即当作成功的状态。

5. 返回 `Promise` 的链式调用：
   ```js
      const p1 = new Promise((resolve, reject) => {

          setTimeout(() => {
              resolve('1s 时间到了，触发 resolve 1');
          }, 1000);
      });

      const p2 = new Promise((resolve, reject) => {

          setTimeout(() => {
              resolve('2s 时间到了，触发 resolve 2');
          }, 2000);
      });

      const p3 = new Promise((resolve, reject) => {

          setTimeout(() => {
              resolve('3s 时间到了，触发 resolve 3');
          }, 3000);
      });

      // 简单的链式调用

      p1
          .then((res) => {
              console.log('then1: ', res);
              return p2;
          })
          .then((res) => {
             console.log('then2: ', res);
             return p3;
          })
          .then((res) => {
              console.log('then3: ', res);
          })
   
   // 输出
   // then1:  1s 时间到了，触发 resolve 1
   // then2:  2s 时间到了，触发 resolve 2
   // then3:  3s 时间到了，触发 resolve 3
   ```
6. 上例中，第一个 `then()` 返回的是一个 `Promise` 对象，这时第二个 `then()` 方法，就会等待该 `Promise` 对象的状态发生变化，才会被调用。如果这个 `Promise` 变为成功状态，即 `resolved`，那么会调用第二个 `then()` 方法的第一个回调函数，如果变成失败状态，即 `rejected`，那么会调用第二个 `then()` 方法的第一个回调函数。同理可见第二个  `then()` 方法返回的 `Promise` 对象和第三个 `then()` 方法的关系。

7. 含有 `rejected` 状态的 `Promise` 的链式调用：
   ```js
      const p1 = new Promise((resolve, reject) => {

          setTimeout(() => {
              // resolve('1s 时间到了，触发 resolve 1');

              reject('1s 时间到了，触发 rejected 1');
          }, 1000);
      });

      const p2 = new Promise((resolve, reject) => {

          setTimeout(() => {
              resolve('2s 时间到了，触发 resolve 2');
          }, 2000);
      });

      const p3 = new Promise((resolve, reject) => {

          setTimeout(() => {
              // resolve('3s 时间到了，触发 resolve 3');
              reject('3s 时间到了，触发 rejected 3');
          }, 3000);
      });

      // 简单的链式调用

      p1
          .then((res) => {
              console.log('then1 success: ', res);
              return p2;
          }, (err) => {
              console.log('then1 failure: ', err);
              return p2;
          })
          .then((res) => {
             console.log('then1 success: ', res);
             return p3;
          }, (err) => {
              console.log('then2 failure: ', err);
              return p3;
          })
          .then((res) => {
              console.log('then3 success: ', res);
          }, (err) => {
              console.log('then3 failure: ', err);
          })
   
         // 输出
         // then1 failure:  1s 时间到了，触发 rejected 1
         // then1 success:  2s 时间到了，触发 resolve 2
         // then3 failure:  3s 时间到了，触发 rejected 3

   ```
8. 上例中，p1 的状态最后是 `rejected`，所以调用的是第一个 `then()` 第二个回调函数。而 p2 的状态是 `resolved`，因此调用第二个 `then()` 的第二个回调函数，而 p3 的状态是 `rejected`，因此调用第三个 `then()` 的第二个回调函数。

9. 综上，解决回调函数嵌套的方法是链式调用 `then()`，并且在每个 `then()` 的处理 `resolved` 状态或者 `rejected` 状态的回调函数中返回 `Promise` 对象。这样后续的 then() 会根据新的 `Promise` 对象状态决定调用哪个回调函数。以此类推。

## 5. 常用方法

### 1. Promise.prototype.then()

1. `then()` 方法接收两个参数：`onFulfilled` 和 `onRejected`，`onFulfilled` 和 `onRejected` 是回调函数，其中 `onFulfilled` 是 `Promise` 状态变为 `resolved` 时调用，而 `onRejected` 是 `Promise` 状态变为 `rejected` 时调用。

2. 如果忽略针对某个状态的回调函数参数，或者提供非函数 (nonfunction) 参数，那么 then 方法将会丢失关于该状态的回调函数信息，但是并不会产生错误。如果调用 then 的 `Promise` 的状态（fulfillment 或 rejection）发生改变，但是 then 中并没有关于这种状态的回调函数，那么 then 将创建一个没有经过回调函数处理的新 `Promise` 对象，这个新 `Promise` 只是简单地接受调用这个 then 的原 `Promise` 的终态作为它的终态。-- MDN

3. `then()` 函数形式如下：
   ```js
      p.then(onFulfilled, [onRejected]);
      p.then(value => {
      // fulfillment
      }, reason => {
      // rejection
      });
   ```
4. 函数说明：
   - 参数
     - `onFulfilled` 可选  
       当 `Promise` 变成接受状态（`fulfilled`）时调用的函数。该函数有一个参数，即接受的最终结果（the fulfillment  value）。如果该参数不是函数，则会在内部被替换为 `(x) => x`，即原样返回 `Promise` 最终结果的函数
     - `onRejected` 可选  
       当 `Promise` 变成拒绝状态（`rejected`）时调用的函数。该函数有一个参数，即拒绝的原因（rejection reason）。 如果该参数不是函数，则会在内部被替换为一个 "Thrower" 函数 (it throws an error it received as argument)。
   - 返回值：当一个 `Promise` 完成（`fulfilled`）或者失败（`rejected`）时，返回函数将被异步调用（由当前的线程循环来调度完成）。具体的返回值依据以下规则返回。如果 then 中的回调函数：
     1. 返回了一个值，那么 then 返回的 `Promise` 将会成为 `resolved` 状态，并且将返回的值作为 `resolved` 状态的回调函数的参数值。
     2. 没有返回任何值，那么 then 返回的 `Promise` 将会成为 `resolved` 状态，并且该 `resolved` 状态的回调函数的参数值为 undefined。
     3. 抛出一个错误，那么 then 返回的 `Promise` 将会成为 `rejected` 状态，并且将抛出的错误作为 `rejected` 状态的回调函数的参数值。
     4. 返回一个已经是 `resolved` 状态的 `Promise`，那么 then 返回的 `Promise` 也会成为 `resolved` 状态，并且将那个 `Promise` 的 `resolved` 状态的回调函数的参数值作为该被返回的 `Promise` 的 `resolved` 状态回调函数的参数值。
     5. 返回一个已经是 `rejected` 状态的 `Promise`，那么 then 返回的 `Promise` 也会成为 `rejected` 状态，并且将那个 `Promise` 的 `rejected` 状态的回调函数的参数值作为该被返回的 `Promise` 的 `rejected` 状态回调函数的参数值。
     6. 返回一个未定状态（pending）的 `Promise`，那么 then 返回 `Promise` 的状态也是未定的，并且它的终态与那个 `Promise` 的终态相同；同时，它变为终态时调用的回调函数参数与那个 `Promise` 变为终态时的回调函数的参数是相同的。

5. `then()` 函数返回一个 `Promise` 示例。这样就可以实现 `then()` 的链式调用。

6. 如果前一个 `then()` 函数返回一个值，那么这个值会被 `Promise.resolve()` 包装，作为下一个 `then()` 的第一个回调函数的参数，即前一个 `then()` 返回的 `Promise` 变成 `resolved` 状态。

7. 如果前一个 `then()` 函数抛出一个异常，那么这个异常会被 `Promise.reject()` 包装，作为下一个 `then()` 的第二个回调函数的参数，即前一个 `then()` 返回的 `Promise` 变成 `rejected` 状态。

8. 示例 - 基本用法：
      ```js
      var fs = require('fs') ;

      // 新建Promise对象
      var p1 = new Promise(function(resolve, reject) {
         // 异步操作
         fs.readFile('./data/a.txt', 'utf-8', function(err, data) {
              if (err) {
                 // 任务失败
                 // 容器的pending状态转变为rejected
                 reject(err) ; 
              } else {
                 // 任务成功
                // 容器的pending状态转变为resolved
                resolve(data) ;
              }
          })
      });
   
      p1.then(function(result) {
          console.log('成功') ;
          console.log(result) ;
      }, function(err) {
          console.log('失败') ;
          console.log(err) ;
      })

   ```

9. 示例 - 链式调用：
   ```js
      const p1 = new Promise((resolve, reject) => {

          setTimeout(() => {
              // resolve('1s 时间到了，触发 resolve 1');

              reject('1s 时间到了，触发 rejected 1');
          }, 1000);
      });

      const p2 = new Promise((resolve, reject) => {

          setTimeout(() => {
              resolve('2s 时间到了，触发 resolve 2');
          }, 2000);
      });

      const p3 = new Promise((resolve, reject) => {

          setTimeout(() => {
              // resolve('3s 时间到了，触发 resolve 3');
              reject('3s 时间到了，触发 rejected 3');
          }, 3000);
      });

      // 简单的链式调用

      p1
          .then((res) => {
              console.log('then1 success: ', res);
              return p2;
          }, (err) => {
              console.log('then1 failure: ', err);
              return p2;
          })
          .then((res) => {
             console.log('then1 success: ', res);
             return p3;
          }, (err) => {
              console.log('then2 failure: ', err);
              return p3;
          })
          .then((res) => {
              console.log('then3 success: ', res);
          }, (err) => {
              console.log('then3 failure: ', err);
          })
   
         // 输出
         // then1 failure:  1s 时间到了，触发 rejected 1
         // then1 success:  2s 时间到了，触发 resolve 2
         // then3 failure:  3s 时间到了，触发 rejected 3

   ```

### 2. Promise.prototype.catch()

1. 参考资料
   - [Promise.prototype.catch() ](https://es6.ruanyifeng.com/#docs/promise#Promise-prototype-catch)
   - [深刻理解Promise系列(四):catch](https://www.jianshu.com/p/1c829edec185)
   - [关于promise中reject和catch的问题](https://www.jianshu.com/p/78711885955b)
   - [关于Promise.catch()错误捕获机制的理解](https://blog.csdn.net/weixin_44776206/article/details/109402410)

2. `catch()` 方法用来捕获异常，即处理 `Promise` 为 `rejected` 的状态。实际上，`catch()` 内部调用的 Promise.prototype.then() 方法中的 `rejected` 状态下的方法，也就是调用 `obj.catch(onRejected)`，即调用其内部的 `obj.then(undefined, onRejected)`。

3. 函数调用形式：
   ```js
      p.catch(onRejected);

      p.catch(function(reason) {
          // 拒绝
      });

   ```
4. 函数参数说明：
   - 参数：
     - `onRejected`  
       当 `Promise` 被 `rejected` 时,被调用的一个回调函数。 该函数拥有一个参数：reason，表示 rejection 的原因。  
       如果 `then()` 方法的第二个回调函数 onRejected 抛出一个错误或返回一个本身失败的 `Promise`， 通过 `catch()` 返回的 `Promise` 被 `rejected`；否则，它将显示为成功（`resolved`）。 
   - 返回值：一个 `Promise`。

5. `catch()` 方法返回的还是一个 `Promise` 对象，因此后面还可以接着调用  `then()` 方法。

6. `catch()` 方法之中，还能再抛出错误。

7. 示例：
   ```js
      const p1 = new Promise((resolve, reject) => {
          // 异步操作
    
      });

      p1.then((res) => {
          console.log('resolved', res);
      }).catch((err) => {
          console.log('rejected', err);
      });
   ```
8. 上例中，p1 是一个 `Promise` 对象，里面封装了一个异步操作。如果该对象状态变为 `resolved`，则会调用 `then()` 方法指定的回调函数；如果异步操作抛出错误，状态就会变为 `rejected`，就会调用 `catch()` 方法指定的回调函数，处理这个错误。另外，`then()` 方法指定的回调函数，如果运行中抛出错误，也会被 `catch()` 方法捕获。

9. 抛出一个错误，大多数情况下，会调用 `catch()` 方法：
    ```js
       const p1 = new Promise((resolve, reject) => {
           // 异步操作
           throw 'error occurred';
       });

       p1.then((res) => {
           console.log('resolved', res);
       }).catch((err) => {
           console.log('rejected: ', err);
       });

    ```
10. 如果是异步任务抛出的错，则不会调用 `catch()` 方法：
    ```js
       const p1 = new Promise((resolve, reject) => {
           setTimeout(() => {
               throw 'error occurred';
           }, 1000)

       });

       p1.then((res) => {
           console.log('resolved', res);
       }).catch((err) => {
           // 不会执行
           console.log('rejected: ', err);
       });
    ```
    **注意**，这里是抛出异常，并不是调用 `reject` 函数，抛出异常不会将 `Promise` 状态变为 `rejected`，只有调用 `reject` 函数，才会将 `Promise` 状态变为 `rejected`。

11. 在 `resolve()` 后面抛出的错误会被忽略。因为 一旦调用 `resolve()` 函数，`Promise` 的状态就变成 `resolved` 状态，不能变化了，因此即使抛出错误，也不会调用 `catch()` 方法。
    ```js
       const p1 = new Promise((resolve, reject) => {
           resolve('success');
           throw 'error occurred';


       });

       p1.catch((err) => {
           // 不会执行
           console.log('rejected: ', err);
       });
    ```

12. `Promise` 对象的错误具有“冒泡”性质，会一直向后传递，直到被捕获为止。也就是说，错误总是会被下一个 `catch()` 捕获。
    ```js
       const p1 = new Promise((resolve, reject) => {
           setTimeout(() => {
               reject('1s 时间到了，触发 rejected 1');
           }, 1000);


       });
       const p2 = new Promise((resolve, reject) => {

           setTimeout(() => {
               // reject('2s 时间到了，触发 rejected 2');
               resolve('2s 时间到了，触发 resolve 2');
           }, 2000);
       });
       const p3 = new Promise((resolve, reject) => {

           setTimeout(() => {
               resolve('3s 时间到了，触发 resolve 3');
               // reject('3s 时间到了，触发 rejected 3');
           }, 3000);
       });

       // 简单的链式调用
       //
       p1
           .then((res) => {
               console.log('then1 success: ', res);
               return p2;
           })
           .then((res) => {
              console.log('then1 success: ', res);
              return p3;
           })
           .then((res) => {
               console.log('then3 success: ', res);
           })
           .catch((err) => {
               // err:  1s 时间到了，触发 rejected 1
               console.log('err: ', err);
           });   
    ```
    上面的代码中，一共有三个 `Promise` 对象：p1、p2 和 p3。这三个 `Promise` 对象中，任意一个抛出异常（调用了 `reject()` 函数），都会被最后一个 `catch()` 捕获。上面是 p1 抛出异常，下面是 p2 抛出异常：
    ```js
       const p1 = new Promise((resolve, reject) => {
           setTimeout(() => {
               resolve('1s 时间到了，触发 resolve 1');
               // reject('1s 时间到了，触发 rejected 1');
           }, 1000);


        });

       const p2 = new Promise((resolve, reject) => {

           setTimeout(() => {
               reject('2s 时间到了，触发 rejected 2');
               // resolve('2s 时间到了，触发 resolve 2');
           }, 2000);
       });

       const p3 = new Promise((resolve, reject) => {

           setTimeout(() => {
               resolve('3s 时间到了，触发 resolve 3');
               // reject('3s 时间到了，触发 rejected 3');
           }, 3000);
       });

       // 简单的链式调用
       //
       p1
           .then((res) => {
               console.log('then1 success: ', res);
               return p2;
           })
           .then((res) => {
              console.log('then1 success: ', res);
              return p3;
           })
           .then((res) => {
               console.log('then3 success: ', res);
           })
           .catch((err) => {
               // then1 success:  1s 时间到了，触发 resolve 1
               // err:  2s 时间到了，触发 rejected 2
               console.log('err: ', err);
           });
    ```

13. 注意，一个 `catch()` 只能处理一个 `Promise` 对象抛出的异常。如果有多个 `Promise` 对象抛出异常，而只有一个 `catch()` 函数，那么 `catch()` 只能处理第一个 `Promise` 对象抛出的异常，后面的 `Promise` 对象抛出的异常，就无法处理了。如下面的例子所示：
    ```js
       const p1 = new Promise((resolve, reject) => {
           setTimeout(() => {
               // resolve('1s 时间到了，触发 resolve 1');
               reject('1s 时间到了，触发 rejected 1');
           }, 1000);
       });

       const p2 = new Promise((resolve, reject) => {

           setTimeout(() => {
               reject('2s 时间到了，触发 rejected 2');
               // resolve('2s 时间到了，触发 resolve 2');
           }, 2000);
       });

       const p3 = new Promise((resolve, reject) => {

           setTimeout(() => {
               resolve('3s 时间到了，触发 resolve 3');
               // reject('3s 时间到了，触发 rejected 3');
           }, 3000);
       });

       // 简单的链式调用
       //
       p1
           .then((res) => {
               console.log('then1 success: ', res);
               return p2;
           })
           .then((res) => {
              console.log('then1 success: ', res);
              return p3;
           })
           .then((res) => {
               console.log('then3 success: ', res);
           })
           .catch((err) => {
               // err:  1s 时间到了，触发 rejected 1
               // (node:23532) UnhandledPromiseRejectionWarning: 2s 时间到了，触发 rejected 2
               // (Use `node --trace-warnings ...` to show where the warning was created)
               // (node:23532) UnhandledPromiseRejectionWarning: Unhandled promise rejection. This error originated either by throwing inside of an async function without a catch block, or by reject
               // ing a promise which was not handled with .catch(). To terminate the node process on unhandled promise rejection, use the CLI flag `--unhandled-rejections=strict` (see https://nodej
               //     s.org/api/cli.html#cli_unhandled_rejections_mode). (rejection id: 1)
               // (node:23532) [DEP0018] DeprecationWarning: Unhandled promise rejections are deprecated. In the future, promise rejections that are not handled will terminate the Node.js process wi
               // th a non-zero exit code.
               console.log('err: ', err);
           });
        
    ```
    p1 和 p2 都是抛出了异常，只有一个 `catch()`，因此只处理了 p1 抛出的异常，而 p2 也会抛出异常，因为没有 `catch()` 处理了，所以会报 `UnhandledPromiseRejectionWarning`。如果想处理多个 `Promise` 对象抛出的异常，就要定义多个 `catch()` 函数。如下面的例子所示：
    ```js
       const p1 = new Promise((resolve, reject) => {
           setTimeout(() => {
               // resolve('1s 时间到了，触发 resolve 1');
               reject('1s 时间到了，触发 rejected 1');
           }, 1000);


       });

       const p2 = new Promise((resolve, reject) => {

           setTimeout(() => {
               reject('2s 时间到了，触发 rejected 2');
               // resolve('2s 时间到了，触发 resolve 2');
           }, 2000);
       });

       const p3 = new Promise((resolve, reject) => {

           setTimeout(() => {
               resolve('3s 时间到了，触发 resolve 3');
               // reject('3s 时间到了，触发 rejected 3');
           }, 3000);
       });

       // 简单的链式调用
       //
       p1
           .then((res) => {
               console.log('then1 success: ', res);
               return p2;
           })
           .then((res) => {
              console.log('then1 success: ', res);
              return p3;
           })
           .then((res) => {
               console.log('then3 success: ', res);
           })
           .catch((err) => {
               // err:  1s 时间到了，触发 rejected 1
               console.log('err: ', err);
               return p2;
           })
           .catch((err) => {
               // err:  2s 时间到了，触发 rejected 1
               console.log('err: ', err);
           });
    ```
    在第一个 `catch()` 中，将 p2 作为其返回值，只有这样，p2 抛出的异常才会被第二个 `catch()` 捕获，即抛出异常也是链式捕获的。如果第一个 `catch()` 不返回 p2，第二个 `catch()` 仍然不会捕获到 p2 的异常。

14. 一般来说，不要在 `then()` 方法里面定义 `rejected` 状态的回调函数（即then的第二个参数），总是使用catch方法。
    ```js
            // bad
        p1
          .then(function(data) {
            // success
          }, function(err) {
            // error
          });

        // good
        p1
          .then(function(data) { //cb
            // success
          })
          .catch(function(err) {
            // error
          });
    ```
15. 上面的例子中，第二种写法要好于第一种写法，理由是第二种写法可以捕获前面 `then()` 方法执行中的错误，也更接近同步的写法（try/catch）。因此，建议总是使用 `catch()` 方法，而不使用 `then()` 方法的第二个参数。如下面的例子所示：
    ```js
       const p1 = new Promise((resolve, reject) => {
           setTimeout(() => {
               resolve('1s 时间到了，触发 resolve 1');
               // reject('1s 时间到了，触发 rejected 1');
           }, 1000);
       });

       const p2 = new Promise((resolve, reject) => {

           setTimeout(() => {
               reject('2s 时间到了，触发 rejected 2');
               // resolve('2s 时间到了，触发 resolve 2');
           }, 2000);
       });

       const p3 = new Promise((resolve, reject) => {

           setTimeout(() => {
               resolve('3s 时间到了，触发 resolve 3');
               // reject('3s 时间到了，触发 rejected 3');
           }, 3000);
       });

       // 简单的链式调用
       //
       p1
           .then((res) => {
               // then1 success:  1s 时间到了，触发 resolve 1
               console.log('then1 success: ', res);
               return p2;
           })
           .then((res) => {
              console.log('then1 success: ', res);
              return p3;
           })
           .then((res) => {
               console.log('then3 success: ', res);
           })
           .catch((err) => {
               // err:  2s 时间到了，触发 rejected 2
               console.log('err: ', err);
           });
    ```
    p1 状态是 `resolved`，因此调用第一个 `then()` 的 resolve 回调函数，然后再这个回调函数中返回 p2，而 p2 的状态是 `rejected`，因此会被最后一个 `catch()` 捕获，处理其抛出的异常。

16. 一般总是建议，`Promise` 对象后面要跟 `catch()` 方法，这样可以处理 `Promise` 内部发生的错误。`catch()` 方法返回的还是一个 `Promise` 对象，因此后面还可以接着调用 `then()` 方法。如下面的例子所示：
    ```js
       const p1 = new Promise((resolve, reject) => {
           resolve(x + 2);

       });


       p1.catch((err) => {
          console.log('err: ', err);
       }).then((res) => {
           console.log('then 方法执行了');
       });

       // 输出
       // err:  ReferenceError: x is not defined
       // then 方法执行了
    ```
    p1 抛出了异常，因此执行了 `catch()` 指定的回调函数，会接着运行后面那个 `then()` 方法指定的回调函数。如果 `catch()` 指定的回调函数返回一个 `Promise` 对象，那么会根据 `Promise` 的状态决定执行 `then()` 中的哪个回调函数。如果没有报错，则会跳过 `catch()` 方法。如下面的例子所示：
    ```js
      const p1 = new Promise((resolve, reject) => {
           resolve('resolved ');

       });


       p1.catch((err) => {
           console.log('err: ', err);
       }).then((res) => {
           // then 方法执行了 resolved
           console.log('then 方法执行了', res);
       })
    ```
    p1 没有抛出异常，跳过了 `catch()` 方法，直接执行后面的 `then()` 方法。此时，要是 `then()` 方法里面报错，就与前面的 `catch()` 无关了。

17. `catch()` 方法之中，还能再抛出错误。如下的例子所示：
    ```js
       const p1 = new Promise((resolve, reject) => {
           resolve(x + 7);

        });


        p1.catch((err) => {
            // err 1 :  ReferenceError: x is not defined
            console.log('err 1 : ', err);
            y + 6;
        }).then((res) => {
            console.log('then 方法执行了', res);
        }).catch((err) => {
            // err 2 :  ReferenceError: y is not defined
            console.log('err 2 : ', err);
        });
    ```
    p1 抛出异常，被第一个 `catch()` 捕获，在第一个 `catch()` 中，依然发生一个异常：`y + 6`。这个异常会被第二个 `catch()` 捕获。如果第一个 `catch()` 后面没有别的 `catch()` 方法了，那么这个错误不会被捕获，也不会传递到外层。


18. 总结：
    1. `catch()` 用来捕获异常，即处理 `Promise` 为 `rejected` 的状态。
    2. `Promise` 由 pending 变为 `rejected` 状态，就会调用 `catch()` 方法。
    3. `catch()` 指定的回调函数可以抛出异常，抛出的异常会被后面的 `catch()` 捕获，如果后面没有 `catch()`，那么这个异常不会被捕获。
    4. `catch()` 指定的回调函数可以返回一个 `Promise` 对象，因此`catch()` 后面可继续调用 `then()`，执行 `then()` 的哪个回调函数取决于返回的 `Promise` 的状态。
    5. 实际使用中，推荐使用 `catch()` 而不是 `then()` 的第二个回调函数参数来捕获异常，因为 `catch()` 可以捕获 `catch()` 前面的东西抛出的异常，包括 `then()` 中抛出的异常，以及 `catch()` 抛出的异常。
    6. 一个 `catch()` 只能处理一个 `Promise` 对象的异常，如果有多个异常需要处理，需要多个 `catch()` 链式调用。
    7. `Promise` 对象的状态变成 `resolved` 状态后再抛出异常，无法调用 `catch()`。异步任务中抛出异常，也无法调用 `catch()`。

### 3. Promise.prototype.finally()

1. 参考资料
   - [finally - ES6标准入门](https://es6.ruanyifeng.com/#docs/promise#Promise-prototype-finally)

2. `finally()` 方法用于指定不管 `Promise` 对象最后状态如何，都会执行的操作。该方法是 ES2018 引入标准的。

3. `finally()` 方法返回一个 `Promise` 对象。在 `Promise` 结束时，无论结果是 `fulfilled` 或者是 `rejected`，都会执行指定的回调函数。这为在 `Promise` 是否成功完成后都需要执行的代码提供了一种方式。避免了同样的语句需要在 `then()` 和 `catch()` 中各写一次的情况。

4. `finally()` 回调函数不接受任何参数，因此我们无法得知 `Promise` 是 `resolved` 状态还是 `rejected` 状态，因此我们执行的应该是与 `Promise` 状态无关的操作。

5. 语法
   ```js
      p.finally(onFinally);
      p.finally(function() {
      // 返回状态为(resolved 或 rejected)
      });   
   ```
6. 参数
   - `onFinally`
     `Promise` 结束后调用的函数。
   - 返回值
     返回一个设置了 `finally` 回调函数的 `Promise` 对象。

7. 描述
   - 如果你想在 `Promise` 执行完毕后无论其结果怎样都做一些处理或清理时，`finally()` 方法可能是有用的。

8. `finally()` 虽然与 `then(onFinally, onFinally)` 类似，它们不同的是：
   - 调用内联函数时，不需要多次声明该函数或为该函数创建一个变量保存它。
   - 由于无法知道 `Promise` 的最终状态，所以 `finally()` 的回调函数中不接收任何参数，它仅用于无论最终结果如何都要执行的情况。

9. `finally()` 的基本用法：
   ```js
      const p1 = new Promise((resolve, reject) => {
          setTimeout(() => {
              resolve('1s 时间到了，触发 resolve 1');
              // reject('1s 时间到了，触发 rejected 1');
          }, 1000);

      });


      p1.then((res) => {
          // then 方法执行了 1s 时间到了，触发 resolve 1
          console.log('then 方法执行了', res);
      }).catch((err) => {
          console.log('err 2 : ', err);
      }).finally(() => {
          // 执行过程结束!!!!!!!!!!
          console.log('执行过程结束!!!!!!!!!!');
      });
   ```
10. `finally()` 方法本质上是 `then()` 方法的特例：
    ```js
       p1
           .finally(() => {
             // 语句
           });

           // 等同于
        p1
           .then(
             result => {
               // 语句
               return result;
             },
             error => {
               // 语句
               throw error;
             }
           );
    ```
    等同于一个 `then()` 方法，同时定义成功状态和失败状态的回调函数，这样无论 `Promise` 对象无论是哪种状态，都能调用相应的回调函数。

11. `finally()` 方法的实现如下：
    ```js
       /**
        * finally 方法的实现
        * @param callback
        * @returns {Promise<unknown>}
        */
       Promise.prototype.finally = function (callback) {
           const P = this.constructor;

           return this.then(
               result => P.resolve(callback()).then(() => result),
               reason => P.resolve(callback()).then(() => throw reason)
           )
       }
    ```
12. `finally()` 总是返回原来的值：
    1. `Promise.resolve(2).then(() => {}, () => {})`
       最终的 `resolved` 的结果为 undefined，因为 `then()` 的第一个回调没有返回任何值，因此最终的 `resolved` 的结果为 undefined。
    2. `Promise.resolve(2).finally(() => {})` 
       `resolved` 状态的结果为 2。
    3. `Promise.reject(3).then(() => {}, () => {})`
       `rejected` 状态的结果为 undefined。因为 `then()` 的第二个回调没有返回任何值，因此最终的 `rejected` 的结果为 undefined。
    4. `Promise.reject(3).finally(() => {})` 
       `rejected` 状态的结果为 3。

13. 分隔行

### 4. Promise.resolve()

1. 参考资料
   1. [Promise.resolve()详解](https://www.cnblogs.com/qianxiaox/p/14124551.html)
   2. [Promise详解（resolve,reject,catch）](https://blog.csdn.net/qq_37939251/article/details/93650542)
   3. [Promise.resolve()](https://www.jianshu.com/p/3bd1c6865bba)
   4. [resolve() - ES6 标准入门](https://es6.ruanyifeng.com/#docs/promise#Promise-resolve)

2. 有时需要将现有对象转为 `Promise` 对象，`Promise.resolve()` 方法就起到这个作用。将一个值直接转换为 `Promise` 对象，且这个 `Promise` 对象的状态是 `resolved` 状态。
   ```js
      const p1 = Promise.resolve(fetch('./a.json'));
   ```
   将 fetch 请求包装为 `Promise` 对象。

3. `resolve()` 等同于下面这种写法：
   ```js
      const p1 = Promise.resolve(1);
      //  等同于
      const p1 = new Promise(resolve => resolve(1));
   ```
4. 语法：`Promise.resolve(value)`
   - 参数 `value`
     将被 `Promise` 对象解析的参数，也可以是一个 `Promise` 对象，或者是一个 `thenable` 对象（具有 `then()` 方法的对象）。
   - 返回值
     返回一个带着给定值解析过的 `Promise` 对象，如果参数本身就是一个 `Promise` 对象，则直接返回这个 `Promise` 对象。

5. `resolve()` 方法接收的参数 `value` 可以是一个 `Promise` 对象。如果是 `Promise` 对象，则 `resolve()` 方法返回值就是这个 `Promise` 对象。

6. `resolve()` 方法接收的参数 `value` 可以是一个 `thenable`对象。thenable 对象指的是具有 then 方法的对象，如下所示：
   ```js
      const p = {
          then: (resolve, reject) => {resolve('theable')}
      }
   ```
   `Promise.resolve()` 方法会将这个对象转为 `Promise` 对象，然后就立即执行 thenable 对象的 `then()` 方法。如下所示：
   ```js
      const thenable = {
          
          then: (resolve, reject) => {
              resolve('thenable');
          }
      }

      const p1 = Promise.resolve(thenable);

      p1.then((res) => {
          // res is : thenable
          console.log('res is :', res);
      });
   ```
   上面代码中，`thenable` 对象的 `then()` 方法执行后，对象 p1 的状态就变为 `resolved`，从而立即执行最后那个 `then()` 方法指定的回调函数，输出 `thenable`。

7. `resolve()` 方法接收的参数 `value` 还可以是一个原始值，或者是一个不具有 `then()` 方法的对象，则 `resolve()` 方法返回一个新的 `Promise` 对象，状态为 `resolved`。
   ```js
      const p1 = Promise.resolve('hello');

      p1.then((res) => {
          // res is : hello
          console.log('res is :', res);
      });
   ```
   上面代码生成一个新的 `Promise` 对象的实例 p1。由于字符串 Hello 不属于异步操作（判断方法是字符串对象不具有 then 方法），返回 `Promise` 实例的状态从一生成就是 `resolved`，所以回调函数会立即执行。`resolve()` 方法的参数会同时传给回调函数。

8. `resolve()` 方法允许调用时不带参数，直接返回一个 `resolved` 状态的 `Promise` 对象。所以，如果希望得到一个 `Promise` 对象，比较方便的方法就是直接调用 `Promise.resolve()` 方法。 

9. 需要注意的是，调用 `resolve()` 函数，是在本轮事件循环（event loop）的结束时执行，而不是在下一轮事件循环的开始时。
   ```js
      setTimeout(function () {
          console.log('three');
      }, 0);

      Promise.resolve().then(function () {
          console.log('two');
      });

      console.log('one');

      // one
      // two
      // three
   ```
   上面代码中，`setTimeout(fn, 0)` 在下一轮事件循环开始时执行，`Promise.resolve()` 在本轮事件循环结束时执行，`console.log('one')` 则是立即执行，因此最先输出。

10. 分隔行

### 5. Promise.reject()

1. `Promise.reject(reason)` 方法会返回一个新的 `Promise` 实例，该实例的状态为 `rejected`。

2. 语法：`Promise.reject(reason)`
   - 参数 reason
     表示 `Promise` 被拒绝的原因。
   - 返回值
     一个给定原因了的 `rejected` 状态的 `Promise`。

3. 静态函数 `Promise.reject()` 返回一个 `rejected` 状态的 `Promise` 对象。

4. `reject()` 的基本用法：
   ```js
      const p = Promise.reject('失败了');
      // 等同于
      const p = new Promise((resolve, reject) => {
          reject('失败了');
      });
   ```
5. 调用了 `reject()`，返回的 `Promise` 对象的状态是 `rejected`，因此会调用 `then()` 的第二个回调函数或者 `catch()` 方法。
   ```js
      const p1 = Promise.reject('出错了');

      p1.then(() => {}, (err) => {
          // 出错了
          console.log(err);
      })
   ```
   使用 `catch()` 捕获：
   ```js
      const p1 = Promise.reject('出错了');

      p1.then(() => {}).catch((err) => {
          // 出错了
          console.log(err);
      })
   ```
6. `Promise.reject()` 方法的参数，会原封不动地作为失败（`rejected`）的理由，变成后续方法的参数。

### 6. Promise.all()

1. 参考资料
    - [javascript异步之Promise.all()、Promise.race()、Promise.finally()](https://segmentfault.com/a/1190000017974025?utm_source=tag-newest)
    - [MDN-Promise.all()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)

2. `all()` 这个方法，以数组形式接收多个 `Promise` 对象，只有所有的 `Promise` 对象都返回成功状态，`all()` 方法才会返回成功状态，只要有一个失败了，all 就会返回失败的状态。

3. `Promise.all(iterable)`
    - 参数  `iterable`
        - 一个可迭代对象，如 Array 或 String。
    - 返回值
        - 如果传入的参数是一个空的可迭代对象，则返回一个已完成（already `resolved`）状态的 `Promise`。
        - 如果传入的参数不包含任何 `Promise`，则返回一个异步完成（asynchronously resolved） `Promise`。注意：Google Chrome 58 在这种情况下返回一个已完成（already resolved）状态的 `Promise`。
        - 其它情况下返回一个处理中（pending）的 `Promise`。这个返回的 `Promise` 之后会在所有的 `Promise` 都完成或有一个 `Promise` 失败时异步地变为完成或失败。

4. 此方法在集合多个 `Promise` 的返回结果时很有用。

5. 方法说明：
    - 完成（Fulfillment）：
        - 如果传入的可迭代对象为空，`Promise.all()` 会同步地返回一个已完成（`resolved`）状态的`Promise`。
        - 如果所有传入的 `Promise` 都变为完成状态，或者传入的可迭代对象内没有 `Promise`，`Promise.all` 返回的 `Promise` 异步地变为完成。
        - 在任何情况下，`Promise.all` 返回的 `Promise` 的完成状态的结果都是一个数组，它包含所有的传入迭代参数对象的值（也包括非 `Promise` 值）。
    - 失败/拒绝（`Rejection`）：
        - 如果传入的 `Promise` 中有一个失败（`rejected`），Promise.all 异步地将失败的那个结果给失败状态的回调函数，而不管其它 `Promise` 是否完成。

6. 应用场景
    - 几个 ajax 全部执行完了，才能渲染页面，
    - 几个 ajax 全部执行完了，才能做一些数据的计算操作，
    - 不关心执行顺序，只关心集体的执行结果

7. 示例 - 全部成功：
   ```javascript
             const p1 = Promise.resolve(50);
             const p2 = 12345;
             const p3 = new Promise((resolve) => {
                 setTimeout(() => {
                     resolve(100);
                 }, 2000);
             });
             // 等到最后一个 Promise 成功即 p3 成功后，，all 返回一个成功的 Promise

             Promise.all([p1, p2, p3]).then(data => {
                 // [ 50, 12345, 100 ]
                 console.log(data);
             });
    ```
8. 示例 - 有一个失败：
   ```javascript
      const p1 = new Promise((resolve, reject) => {
            setTimeout(resolve, 1000, 'one');
      });
             const p2 = new Promise((resolve, reject) => {
                 setTimeout(resolve, 2000, 'two');
             });
             const p3 = new Promise((resolve, reject) => {
                 setTimeout(resolve, 3000, 'three');
             });
             const p4 = new Promise((resolve, reject) => {
                 setTimeout(resolve, 4000, 'four');
             });


             const p5 = new Promise((resolve, reject) => {
                 reject('reject');
             });

             Promise.all([p1, p2, p3, p4, p5]).then(res => {
                 console.log('成功', res);
             }, err => {
                 // 只要有一个 Promise 失败，all 就会返回一个失败的 Promise
                 // 失败 reject
                 console.log('失败', err);
             });

             Promise.all([p1, p2, p3, p4, p5]).then(res => {
                 console.log('成功', res);
             }).catch(reason => {
                 // 失败 reject
                 console.log('失败', reason);
             })
    ```

9.

### 7. Promise.race()

1. 参考资料
    - [javascript异步之Promise.all()、Promise.race()、Promise.finally()](https://segmentfault.com/a/1190000017974025?utm_source=tag-newest)
    - [MDN-Promise.race()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/race)

2. `race` 方法返回一个 `Promise`，一旦迭代器中的某个 `Promise` 成功或失败，返回的 `Promise` 就会成功或失败。

3. 从 `race` 这个方法的名称我们就可以看出，这个方法的结果取决于最快地那个 `Promise` 的结果。

4. `Promise.race(iterable)`
    - 参数  `iterable`
        - 可迭代对象，类似 Array。详见 iterable。
    - 返回值
        - 一个待定的 `Promise` 只要给定的迭代中的一个 `Promise` 解决或拒绝，就采用第一个 `Promise` 的值作为它的值，从而异步地解析或拒绝（一旦堆栈为空）。

5. 示例 - 成功示例
   ```JavaScript
             const p1 = new Promise((resolve, reject) => {
                setTimeout(() => {
                    resolve('p1');
                }, 2000);
             });

            const p2 = new Promise((resolve, reject) => {
                setTimeout(() => {
                    resolve('p2');
                }, 1000);
            });

            Promise.race([p1, p2]).then((res) => {
               // p2 最先完成，因此输出 p2
               console.log(res);
            });  
   ```
6. 示例 - 失败示例：
   ```javascript
             const p5 = new Promise(function(resolve, reject) {
                 setTimeout(resolve, 500, "five");
             });
             const p6 = new Promise(function(resolve, reject) {
                 setTimeout(reject, 100, "six");
             });

             Promise.race([p5, p6]).then(function(value) {
                 // 未被调用
             }, function(reason) {
                  console.log(reason); // "six"
                  // p6 更快，所以它失败了
             });
   ```

7. 如果传的迭代是空的，则返回的 `Promise` 将永远等待。

8. 如果迭代包含一个或多个非承诺值和/或已解决/拒绝的承诺，则 `Promise.race` 将解析为迭代中找到的第一个值。