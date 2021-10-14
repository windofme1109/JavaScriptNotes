<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [`async`/`await` 的使用方法](#asyncawait-%E7%9A%84%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95)
  - [1. 基本说明](#1-%E5%9F%BA%E6%9C%AC%E8%AF%B4%E6%98%8E)
  - [2. 特点](#2-%E7%89%B9%E7%82%B9)
  - [3. `async`/`await` 用法](#3-asyncawait-%E7%94%A8%E6%B3%95)
    - [1. 基本用法](#1-%E5%9F%BA%E6%9C%AC%E7%94%A8%E6%B3%95)
    - [2. `async`](#2-async)
    - [3. `await`](#3-await)
    - [4. 错误处理](#4-%E9%94%99%E8%AF%AF%E5%A4%84%E7%90%86)
    - [5. 注意的问题](#5-%E6%B3%A8%E6%84%8F%E7%9A%84%E9%97%AE%E9%A2%98)
  - [4. `async`/`await` 的优势](#4-asyncawait-%E7%9A%84%E4%BC%98%E5%8A%BF)
  - [5. `async` / `await` 原理](#5-async--await-%E5%8E%9F%E7%90%86)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# `async`/`await` 的使用方法

## 1. 基本说明

1. `async` 函数是 `Generator` 函数的语法糖。使用关键字 `async` 来表示，在函数内部使用 `await` 来表示异步。

2. 参考资料
   - [理解 async/await](https://juejin.cn/post/6844903487805849613)
   - [7张图，20分钟就能搞定的async/await原理！为什么要拖那么久？](https://juejin.cn/post/7007031572238958629)
   - [async await 实现原理分析](https://zhuanlan.zhihu.com/p/157282332)
   - [async、await运用原理](https://blog.csdn.net/qq_44624386/article/details/107920215)
   - [async、await 实现原理](https://zhuanlan.zhihu.com/p/115112361)
   - [async/await 的基础使用及原理简介](https://www.cnblogs.com/zhengyufeng/p/11106901.html)
   - [async实现原理和执行逻辑总结](https://blog.csdn.net/qdmoment/article/details/90780583)
   - [async/await执行原理详解](https://www.jianshu.com/p/72e36168944f)
   - [Understanding JavaScript’s async await](https://ponyfoo.com/articles/understanding-javascript-async-await)

## 2. 特点

1. 内置执行器。`Generator` 函数的执行必须依靠执行器，而 `aysnc` 函数自带执行器，调用方式跟普通函数的调用一样。

2. 更好的语义。`async` 和 `await` 相较于 `*` 和 `yield` 更加语义化。

3. 更广的适用性。`co` 模块（第三方的 `Generator` 执行器）约定，`yield` 命令后面只能是 `Thunk` 函数或 `Promise` 对象。而 `async` 函数的 `await` 命令后面则可以是 `Promise` 或者原始类型的值（`number`，`string`，`boolean`，但这时等同于同步操作）。

4. 返回值是 `Promise`。`async` 函数返回值总是 `Promise` 对象，比 `Generator` 函数返回的 `Iterator` 对象方便，可以直接使用 `then()` 方法进行调用。




## 3. `async`/`await` 用法

### 1. 基本用法

1. 在函数定义时，在 `function` 关键字前面加上 `async`，表明函数内部有异步操作。`await` 放在 `async` 函数内部，在需要异步操作的语句前面加上 `await`。代码示例如下：
   ```javascript
       async function readFile(fileName) {
             // read()是一个异步读取文件的函数
             const content = await read(fileName) ;
      
             return content ;
       }
    ```
   
### 2. `async`

1. 使用 `async` 定义的函数返回值是 `Promise` 对象，可以使用 then() 方法。 
    ```javascript
       async function readFile(fileName) {
             // read()是一个异步读取文件的函数
             const content = await read(fileName) ;
      
             return content ;
       }
       readFile('a.txt').then((result) => {
           conole.log(result) ;
       })
    ```
2. `async` 函数返回的 `Promise` 对象，必须等到内部所有 `await` 命令后面的 `Promise` 对象执行完，才会发生状态改变，除非遇到 return 语句或者抛出错误。也就是说，只有 `async` 函数内部的异步操作执行完，才会执行 `then()` 方法指定的回调函数。

### 3. `await`

1. 正常情况下，`await` 命令后面是一个 `Promise` 对象，返回该 `Promise` 对象的结果。如果不是 `Promise` 对象，就直接返回对应的值。
   ```js
      async function func() {
          // 等同于 return 123
          return await 123;
      }

      func().then(res => {
         // 123
         console.log(res)
      });
   ```
   `await` 后面是数值 123，此时等同于 `return 123`。 

2. `await` 命令后面如果是一个 thenable 对象（即定义了 then 方法的对象），那么 `await` 会将其看作 `Promise` 对象。
   ```js
      class Sleep {
          constructor(timeout) {
              this.timeout = timeout;
          }

          then(resolve, reject) {
              const start = Date.now();
              setTimeout(() => {
                  resolve(Date.now() - start);
              }, this.timeout);
          }
      }

      async function func() {

          const sleepTime = await new Sleep(1000);
          console.log(sleepTime);
      }

      // 1009
      func();
   ```
   Sleep 对象的实例并不是 `Promise`，但是因为有 then() 方法，所以 `await` 会将其看作 `Promise` 对象。then() 方法接收 resolve 和 reject，类似于 `Promise` 的构造函数，then() 里面的执行过程也是类似于 `Promise` 的构造函数的执行过程，包括状态转换的过程。

3. `await` 命令后面的 `Promise` 对象如果变为 rejected 状态，则 rejected 的参数会被 `async` 函数的 `catch()` 方法的回调函数接收到。
   ```js
      async function func() {
          await Promise.reject('出错了');
      }


      func().catch(err => {
          // error:  出错了
          console.log('error: ', err);
      });
   ```
   `await` 后面的 `Promise` 对象变成 rejected 状态，即使我们没有使用 return，最后也被 `async` 函数的返回的 `Promise` 对象的` catch()` 方法捕获到。

4. 任意一个 `await` 语句后面的 `Promise` 对象，如果变成 rejected 状态，那么整个 `async` 函数都会终止执行。
   ```js
      async function func() {
           await Promise.reject('出错了');
           // 不会执行下面这句
           await Promise.resolve(123);
       }


       func().then(res => {
       console.log('res', res);
       }).catch(err => {
       // error:  出错了
       console.log('error: ', err);
       });
   ```
   第一个 `await` 后面的 `Promise` 变成 rejected 状态，因此，第二个 `await` 不会执行，整个 `async` 函数终止执行。

5. 有一些情况下，我们希望即使前一个异步操作失败，也不要中断后面的异步操作。这时可以将前面的 `await` 放在 `try...catch` 结构里面，这样不管这个异步操作是否成功，后面的 `await` 都会执行。
   ```js
      async function func() {
          try {
              await Promise.reject('出错了');
          } catch (err) {
              // err 出错了
              console.log('err', err);
          }
          
          let result = await Promise.resolve(123);

          return result;
      }


      func().then(res => {
      // 123
      console.log(res);
      });
   ```
   第一个 `await` 后面的 `Promise` 变为 rejected 状态，因为 被 `try... catch` 包裹，因此会被 `catch` 捕获这个 `Promise` 的 rejected 状态下传入的值。而第二个 `await` 不受影响，继续执行。

6. 另外一种处理方式是 `await` 后面的 `Promise` 对象再跟一个 `catch` 方法，处理前面可能出现的异常。
   ```js
      async function func() {
          await Promise.reject('出错了').catch(err => {
              // err 出错了
              console.log('err', err);
          });

          let result = await Promise.resolve(123);

          return result;
      }


      func().then(res => {
      // 123
      console.log(res);
      });
   ```
   第一个 `await` 后面的 `Promise` 对象接了一个 catch()，用来捕获 `Promise` 对象变成 rejected 状态下的值。而第二个 `await` 不受任何影响，会继续执行。


### 4. 错误处理

1. 如果 `await` 后面的异步操作出错，那么等同于 `async` 函数返回的 `Promise` 对象被 reject。即 `await` 后面的 `Promise` 对象变成 rejected 状态，那么 `async` 函数返回的 `Promise` 对象也会变成 rejected 状态，因此会被 `catch()` 函数捕获。
   ```js
      async function func() {
          await new Promise((resolve, reject) => {
              throw new Error('执行出错！');
          })
      }


      func().then(res => {
          console.log(res);
      }).catch(err => {
          // Error: 执行出错！
          console.log(err);
      });
   ```
   `async` 函数执行后，`await` 后面的 `Promise` 对象抛出一个异常，导致 `async` 函数返回值 `Promise` 对象 的 catch方法的回调函数被调用，它的参数就是抛出的异常对象。

2. 防止抛出异常到函数外面的方式是将 `await` 放到 try...catch 语句中。
   ```js
       async function func() {
           try {
               await new Promise((resolve, reject) => {
            throw new Error('执行出错！');
               });
           } catch (e) {
               // err:  Error: 执行出错！
               console.log('err: ', e);
           }

           return await 'hello world';

       }


       func().then(res => {
       // hello world
       console.log(res);
       });
   ```
   第一个 `await` 后面的 `Promise` 对象在执行过程中抛出异常，被 `async` 函数内部的 `catch` 捕获。这不影响第二个 `await` 语句的执行。

3. 如果有多个 `await` 命令，可以统一放在 try...catch 结构中。
   ```js
      async function func() {
          try {
              await new Promise((resolve, reject) => {
                  throw new Error('执行出错！');
              });

              const res1 = await Promise.resolve('hello');
              const res2 = await Promise.resolve('world');
          } catch (e) {
              console.log('err: ', e);
          }
    
      }
   ```
   将多个 `await` 语句放在 `try...catch` 中，只要一个 `await` 后面的异步过程抛出异常，都可以被 `catch` 捕获。当然，如果抛出了异常，那么后面的 `await` 语句不会继续执行。


### 5. 注意的问题

1. `async` 函数返回的是一个 `Promise` 对象（从文档中也可以得到这个信息）。`async` 函数（包含函数语句、函数表达式、Lambda 表达式）会返回一个 `Promise` 对象，如果在函数中 return 一个直接量，`async` 会把这个直接量通过 `Promise.resolve()` 封装成 `Promise` 对象。

2. `async` 函数返回一个 `Promise` 对象，所以，可以使用 `then()` 方法添加回调函数。

3. `await` 等待的是一个表达式，这个表达式计算结果可以是 `Promise` 对象也可以是其他值。
   ```javascript
       function getSomething() {
           return 'something' ;
       }
    
       async function testAsync() {
           return Promise.resolve('hello async') ;
       }
    
       async function test() {
           // await可以等待一个表达式的结果，这个表达式的结果可以是Promise对象，也可以是其他结果
           const v1 = await getSomething() ;
           const v2 = await testAsync() ;
           console.log(v1, v2) ;
       }
       // something hello async
       test() ;
   ```
4. `await` 遇到 `Promise` 对象，然后阻塞后面的代码，等着 `Promise` 对象的状态变为 resolved，然后得到 resolve 的值，作为 `await` 的表达式的结果。也就是说，`await` 等待的是 `Promise` 对象执行的结果，即执行成功的结果。`async` 函数调用不会造成阻塞,它内部所有的阻塞都被封装在一个 `Promise` 对象中异步执行。
 ```javascript
    function takeLongTime() {
        return new Promise(function (resolve, reject) {
            setTimeout(() => {
                resolve('长时间运行！')
            }, 1500) ;
        })
    }
    
    async function test() {
        var ret = await takeLongTime() ;
        console.log(ret) ;
    }
    
    test() ;
 ```
5. `await` 命令后面的 `Promise` 对象，运行结果可能是 rejected，所以最好把 `await` 命令放在 `try...catch` 代码块中。
 ```javascript
    try {
       await someWithPromise() ;
    } catch (err) {
       console.log(err) ;
    }
 ```

## 4. `async`/`await` 的优势

1. 优势在于取代 `Promise` 的 `then()` 方法的链式调用。代码示例如下：
   ```javascript
       function takeLongTime(n) {
           return new Promise(function (resolve, reject) {
               setTimeout(() => {
                   resolve(n + 200)
               }, n) ;
           })
       }
    
       function step1(n) {
           console.log(`step1 ${n}`) ;
    
           return takeLongTime(n) ;
       }
    
       function step2(n) {
           console.log(`step2 ${n}`) ;
    
           return takeLongTime(n) ;
       }
    
       function step3(n) {
           console.log(`step3 ${n}`) ;
    
           return takeLongTime(n) ;
       }
    
         // 使用Promise完成链式调用
         function doIt() {
             console.time('doIt') ;
             const time1= 300 ;
             step1(time1)
                 .then(time2 => step2(time2))
                 .then(time3 => step3(time3))   // 每一个then()均返回一个Promise对象
                 .then(ret => {
                     console.log(`执行时间是：${ret}`) ;
                     console.timeEnd('doIt') ;
                 })
         }
         // step1 300
         //    step2 500
         //    step3 700
         //    执行时间是：900
         //    doIt: 1506.600ms
         // doIt() ;
    
         // 使用async和await完成
         async function doItAsync() {
             console.time('doIt') ;
             const time1= 300 ;
             const time2 = await step1(time1) ;
             const time3 = await step2(time2) ;
             const ret = await step3(time3) ;
             console.log(`执行时间是：${ret}`) ;
             console.timeEnd('doIt') ;
    
         }
    
         //   step1 300
         //   step2 500
         //   step3 700
         //   执行时间是：900
         //   doIt: 1506.134ms 
           doItAsync() ;
   ```

2. 使用 `async`/`await` 使得写异步操作就像同步一样。

3. **注意**：`async` / `await` 的本质是使用 `Generator` 包装了异步操作，只是语法糖，并没有真正实现同步操作。所以，我们是不能直接将 `await` 等到的异步结果，返回到 `async` 函数外面。举例如下：
   ```javascript

      const readJson = (path) => {
          return new Promise((resolve, reject) => {
              fs.readFile(path, function (err, data) {
                  if (err) {
                      return reject(err);
                  }
                  resolve(data);
              })
          })
      }
  
      async function getJson(path) {
          const ret = await readJson(path);
     
          return ret;
      }
   ```
   `readJson()` 是一个异步获取文件内容的函数。我们这里使用 `async` / `await` 获取文件内容，然后将这个内容返回。接下来我们开始调用并打印结果：
   ```javascript
      let result = getJson('./a.json');
      // 输出 Promise { <pending> }
      console.log(result);
   ```  
   输出是 `Promise { <pending> }`，居然不是我们想要的文件内容。这就说明，`async` / `await` 并没有将异步操作转换为同步操作， getJson() 还是异步操作，所以我们调用 getJson() 时，会立即返回一个 `Promise` 对象，状态是 pending，表示正在等待异步操作的结果。  
  
4. **总结**：要使用 `await` 获取的异步操作结果，必须在 `async` 函数内部使用，在 `async` 函数外部，依旧无法获取异步操作的结果。
  
5. 一个知识点：从 Node 的 v10 版本里增加了 `Promise` 模块，从而我们可以通过 `Promise` 的方式来处理异步操作读取文件的API。获取的 `Promise` 模块的方式是：`require('fs').promises` 或者是：`require('fs/promises')`。使用 `Promise` 的方式进行异步操作：
    ```javascript
        const promises = require('fs').promises;
        // 或者使用解构
        // const {promises} = require('fs');
        const {readFile, writeFile} = promises;
        // 以Promise的方式进行异步操作，必须指定文件的编码方式
        readFile('./a.json', 'utf-8').then((ret) => {
            console.log(ret);
        })
    ```



## 5. `async` / `await` 原理

1. `async` 函数的实现原理，就是将 Generator 函数和自动执行器，包装在一个函数里。
   ```js
      async function func(args) {
          // ...
      }
   
      // 等同于
      function generatorToAsync(function*() {}) {
          return function() {
              return new Promise((resolve, reject) => {
                  // ...
              
              })
          }
      }
   ```
2. generatorToAsync() 的作用有两个：
   1. 自动执行 generator 函数的 next() 函数，即从 yield 中断的地方恢复执行 generator 函数。
   2. 返回 `Promise` 对象。

3. 下面给出 generatorToAsync() 函数的实现：
   ```js
      function generatorToAsync(generator) {

          return function() {
              const args = arguments;
              const ctx = this;
              return new Promise((resolve, reject) => {
                  const gen = generator.apply(ctx, args);
                  function go(key, arg) {
                      let res;
                      try {
                          res = gen[key](arg);
                      } catch (err) {
                          reject(err);
                      }

                      const {value, done} = res;

                      if (done) {
                          return resolve(value);
                      } else {
                          Promise.resolve(value).then((res) => {
                              go('next', res);
                          }, (err) => {
                              go('throw', err);
                          });
                      }

                  }

                  go('next');
              })
          }
      }
   ```