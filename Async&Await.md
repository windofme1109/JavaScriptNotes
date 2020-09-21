## 1. async/await

### 1.1 用法

- 在函数定义时，在function关键字前面加上async，表明函数内部有异步操作。await放在async函数内部，在需要异步操作的语句前面加上await。代码示例如下：
 ```javascript
       async function readFile(fileName) {
        // readFile()是一个异步读取文件的函数
        const content = await readFile() ;
      
        return content ;
  }
 ```
  
### 1.2 调用

- 使用async定义的函数返回值是promise对象，可以使用then()方法。 
 ```javascript
      readFile('a.txt').then((result) => {
        conole.log(result) ;
      })
 ```
### 1.3 注意的问题

- async 函数返回的是一个 Promise 对象（从文档中也可以得到这个信息）。async 函数（包含函数语句、函数表达式、Lambda表达式）会返回一个 Promise 对象，如果在函数中 return 一个直接量，async 会把这个直接量通过 Promise.resolve() 封装成 Promise 对象。

- async函数返回一个promise对象，所以，可以使用then()方法添加回调函数。

- await等待的是一个表达式，这个表达式计算结果可以是Promise对象也可以是其他值。
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
- await遇到Promise对象，然后阻塞后面的代码，等着Promise对象Resolve，然后得到resolve的值，作为await的表达式的结果。也就是说，await等待的是Promise对象执行的结果，即执行成功的结果。async 函数调用不会造成阻塞,它内部所有的阻塞都被封装在一个 Promise 对象中异步执行。
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
- await 命令后面的 Promise 对象，运行结果可能是 rejected，所以最好把 await 命令放在 try...catch 代码块中。
 ```javascript
    try {
       await someWithPromise() ;
    } catch (err) {
       console.log(err) ;
    }
 ```
#### 1.4 async/await的优势

- 优势在于Promise的then()方法的链式调用。代码示例如下：
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

- 使用 async/await 使得写异步操作就像同步一样。

- async / await 的本质是使用 generator 包装了异步操作，只是语法糖，并没有真正实现同步操作。所以，我们是不能直接将 `await` 等到的异步结果，返回到 `async` 函数外面。举例如下：
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
  `readJson()` 是一个异步获取文件内容的函数。我们这里使用 async / await 获取文件内容，然后将这个内容返回。接下来我们开始调用并打印结果：
   ```javascript
      let result = getJson('./a.json');
      // 输出 Promise { <pending> }
      console.log(result);
   ```  
  输出是 `Promise { <pending> }`，居然不是我们想要的文件内容。这就说明，async / await 并没有将异步操作转换为同步操作， getJson() 还是异步操作，所以我们调用 getJson() 时，会立即返回一个 Promise 对象，状态是 pending，表示正在等待异步操作的结果。  
  
  总结：要使用 await 获取的异步操作结果，必须在 async 函数内部使用，在 async 函数外部，依旧无法获取异步操作的结果。
  
- 从 Node 的v10版本里增加了 Promise 模块，从而我们可以通过Promise的方式来处理异步操作读取文件的API。获取的 Promise 模块的方式是：`require('fs').promises` 或者是：`require('fs/promises')`。使用 Promise 的方式进行异步操作：
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
  
