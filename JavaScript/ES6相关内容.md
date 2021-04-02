<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [ES6 基础](#es6-%E5%9F%BA%E7%A1%80)
  - [1. const 和 let](#1-const-%E5%92%8C-let)
    - [1. var声明变量的特点](#1-var%E5%A3%B0%E6%98%8E%E5%8F%98%E9%87%8F%E7%9A%84%E7%89%B9%E7%82%B9)
    - [2. let声明变量的特点](#2-let%E5%A3%B0%E6%98%8E%E5%8F%98%E9%87%8F%E7%9A%84%E7%89%B9%E7%82%B9)
    - [3. const声明变量的特点](#3-const%E5%A3%B0%E6%98%8E%E5%8F%98%E9%87%8F%E7%9A%84%E7%89%B9%E7%82%B9)
  - [2. 解构赋值](#2-%E8%A7%A3%E6%9E%84%E8%B5%8B%E5%80%BC)
  - [3. 模板字符串](#3-%E6%A8%A1%E6%9D%BF%E5%AD%97%E7%AC%A6%E4%B8%B2)
  - [4. 箭头函数](#4-%E7%AE%AD%E5%A4%B4%E5%87%BD%E6%95%B0)
  - [5. 扩展运算符](#5-%E6%89%A9%E5%B1%95%E8%BF%90%E7%AE%97%E7%AC%A6)
  - [6. Promise](#6-promise)
  - [7. Set和Map](#7-set%E5%92%8Cmap)
  - [8. class](#8-class)
  - [9. 模块](#9-%E6%A8%A1%E5%9D%97)
  - [10. Symbol](#10-symbol)
  - [11. 函数的扩展](#11-%E5%87%BD%E6%95%B0%E7%9A%84%E6%89%A9%E5%B1%95)
    - [1. 函数的默认参数](#1-%E5%87%BD%E6%95%B0%E7%9A%84%E9%BB%98%E8%AE%A4%E5%8F%82%E6%95%B0)
    - [2. rest 参数](#2-rest-%E5%8F%82%E6%95%B0)
  - [12. 新增数组方法](#12-%E6%96%B0%E5%A2%9E%E6%95%B0%E7%BB%84%E6%96%B9%E6%B3%95)
    - [11. flat()](#11-flat)
  - [13. 新增字符串方法](#13-%E6%96%B0%E5%A2%9E%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%96%B9%E6%B3%95)
  - [14. 新增对象方法](#14-%E6%96%B0%E5%A2%9E%E5%AF%B9%E8%B1%A1%E6%96%B9%E6%B3%95)
  - [15.async 和 await](#15async-%E5%92%8C-await)
    - [1. 用法](#1-%E7%94%A8%E6%B3%95)
    - [2. 调用](#2-%E8%B0%83%E7%94%A8)
    - [3. 注意的问题](#3-%E6%B3%A8%E6%84%8F%E7%9A%84%E9%97%AE%E9%A2%98)
    - [4. async/await 的优势](#4-asyncawait-%E7%9A%84%E4%BC%98%E5%8A%BF)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# ES6 基础

## 1. const 和 let

1. 由于 ES5 没有块级作用域，所以 var 声明的变量，要么是函数作用域，要么是全局作用域。
  
2. ES6 引入了 let 和 const 声明变量，作用范围是局部作用域。

### 1. var声明变量的特点

1. 变量提升

2. 重复声明

3. 全局作用域或函数作用域

### 2. let声明变量的特点


1. 没有变量提升，存在暂时性死区
2. 在未声明前引用，会报错
3. 不允许重复声明
4. 局部作用域

### 3. const声明变量的特点

1. 没有变量提升

2. 声明时就必须赋值

3. 局部作用域

4. 对于引用类型，只是指针不变，数据不保证不变

## 2. 解构赋值

## 3. 模板字符串

1. 形式：
   ```javascript
      var a = good ;
      // 模板字符串
      var str = `you are a ${a} boy` ;

   ```
2. 要点：
   1. 使用反引号（``）
   2. 替换的部分使用 `${}` 将内容包裹
   

## 4. 箭头函数

1. 形式：`=>`

2. 用法：不用多说

3. 要点：
   1. 箭头函数体内 this 对象指向定义是所在的对象，而不是使用时所在的对象。
   2. 不能当做构造函数，也就是不能使用 new。
   3. 不能使用 arguments 对象，该对象在函数体内不存在。如果要使用，可以用 rest 参数替代。
   4. 不可以使用 yield 命令。
   
## 5. 扩展运算符

1. 形式：`...`
2. 作用：将一个数组展开为用逗号分隔的序列。对于字符串，对象、Map、Set、Generator、实现了 Iterator 接口的对象（例如 NodeList 对象）都能使用。  
3. 举例：
   ```javascript
      // 对象
      var obj = {a: 1, b: 2} ;
      // {a: 1, b: 2, c: 3}
      var obj2 = {...obj, c: 3} ;
      // 数组
      var a = [1, 2, 3] ;
      // [1, 2, 3, 4]
      var b = [...a, 4] ;
      // Map
      var map = new Map([
          ['1', 'one'],
          ['2', 'two'],
          ['3', 'three']
      ]) ;
      // [1, 2, 3]
      var arr = [...map.keys()] ;
   ```
4. 用途：
   1. 替代 apply()
   2. 字符串转换为数组
   3. 与结构赋值结合，生成新的数组
   4. Map、Set 和 Generator
   
## 6. Promise

1. 异步编程的一种解决方案。使用 Promise 完成异步任务，可以避免回调地狱的出现。写法更简单，语义更明确。

2. 理解：
   1. Promise 既是一个对象，又是一个构造函数。可以将其看做为一个容器，里面存放着未来才会完成的异步任务。 
   2. 一共有三种状态：pending（等待）、fulfilled（成功）、rejected（失败）。转态转换只能是：pending --> fulfilled 或者 pending --> rejected。
   3. 状态一旦发生变化，就不可逆转。Promise 对象就一直保持这个状态。在任何时候都可以获取这个状态的数据。
   
3. 常用方法：
   1. then()
   2. catch()
   3. all()
      - 参考资料
        - [javascript异步之Promise.all()、Promise.race()、Promise.finally()](https://segmentfault.com/a/1190000017974025?utm_source=tag-newest)
        - [MDN-Promise.all()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)
      - all 这个方法，以数组形式接收多个 Promise 对象，只有所有的 Promise 对象都返回成功状态，all 方法才会返回成功状态，只要有一个失败了，all 就会返回失败的状态。
      - `Promise.all(iterable)`
        - 参数  iterable
          - 一个可迭代对象，如 Array 或 String。
        - 返回值
          - 如果传入的参数是一个空的可迭代对象，则返回一个已完成（already resolved）状态的 Promise。
          - 如果传入的参数不包含任何 promise，则返回一个异步完成（asynchronously resolved） Promise。注意：Google Chrome 58 在这种情况下返回一个已完成（already resolved）状态的 Promise。
          - 其它情况下返回一个处理中（pending）的 Promise。这个返回的 promise 之后会在所有的 promise 都完成或有一个 promise 失败时异步地变为完成或失败。
      - 此方法在集合多个 promise 的返回结果时很有用。
      - 说明：
        - 完成（Fulfillment）：
          - 如果传入的可迭代对象为空，Promise.all 会同步地返回一个已完成（resolved）状态的promise。
          - 如果所有传入的 promise 都变为完成状态，或者传入的可迭代对象内没有 promise，Promise.all 返回的 promise 异步地变为完成。
          - 在任何情况下，Promise.all 返回的 promise 的完成状态的结果都是一个数组，它包含所有的传入迭代参数对象的值（也包括非 promise 值）。

        - 失败/拒绝（Rejection）：
          - 如果传入的 promise 中有一个失败（rejected），Promise.all 异步地将失败的那个结果给失败状态的回调函数，而不管其它 promise 是否完成。
      - 应用场景
        - 几个 ajax 全部执行完了，才能渲染页面，
        - 几个 ajax 全部执行完了，才能做一些数据的计算操作，
        - 不关心执行顺序，只关心集体的执行结果
      - 示例：
        - 全部成功：
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
        - 有一个失败：
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
   4. race()
      - 参考资料
        - [javascript异步之Promise.all()、Promise.race()、Promise.finally()](https://segmentfault.com/a/1190000017974025?utm_source=tag-newest)
        - [MDN-Promise.race()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/race) 
      - race 方法返回一个 Promise，一旦迭代器中的某个 Promise 成功或失败，返回的 Promise 就会成功或失败。
      - 从 race 这个方法的名称我们就可以看出，这个方法的结果取决于最快地那个 Promise 的结果。
      - `Promise.race(iterable)`
        - 参数  iterable
          - 可迭代对象，类似 Array。详见 iterable。
        - 返回值
          - 一个待定的 Promise 只要给定的迭代中的一个 Promise 解决或拒绝，就采用第一个 Promise 的值作为它的值，从而异步地解析或拒绝（一旦堆栈为空）。
      - 示例：
        - 成功示例
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
        - 失败示例：
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

如果传的迭代是空的，则返回的 promise 将永远等待。

如果迭代包含一个或多个非承诺值和/或已解决/拒绝的承诺，则 Promise.race 将解析为迭代中找到的第一个值。
   
## 7. Set和Map

## 8. class

## 9. 模块

## 10. Symbol

## 11. 函数的扩展

### 1. 函数的默认参数

### 2. rest 参数

## 12. 新增数组方法

1. Array.from()

- Array.from方法用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象（包括 ES6 新增的数据结构 Set 和 Map）。

2. Array.of()

- 将参数合并成一个数组返回。

3. copyWithin()

- 数组中的元素的替换。

4. find()

5. findIndex()

6. fill()

- 把数组中的每一个元素替换成指定值。

7. entries()

8. keys()

9. values()

10. includes()

### 11. flat()

12. flatMap()

## 13. 新增字符串方法

1. includes()  
   - 同样能接收 2 个参数，填写一个参数在全局找，填写第二个参数，则从填写的位置往后找。如果找到返回 true，没找到返回 false。
   
2. startsWith()  
   - 查询是否以什么什么开头，同样能接收 2 个参数，1个参数的话在全局找，2 个参数的话则从填写的位置往后找，找到返回 true，没找到返回 false。
   
3. endsWith()  
   - 用法与第3个一样，如果填写第二个参数的话，则是从填写的位置往前找。
   
4. repeat()  
   - 能将原字符串重复几次，并返回一个新的字符串，注意：如果输入的是小数则会被向下取整，NaN 会被当做 0，输入其他的则会报错。
   
5. padStart()  
   - 用于头部补全，接收2个参数，第一个参数是补全后的字符串的最大长度，第二个是要补的字符串，返回的是补全后的字符串。如果原字符串长度大于第一个参数，则会返回原字符串。如果不写第二个参数，则会用空格替补。
   
6.padEnd()
  - 用法与 padStart() 类似，只不过是从末尾开补全。
## 14. 新增对象方法

## 15.async 和 await

### 1. 用法

1. 在函数定义时，在 function 关键字前面加上 async，表明函数内部有异步操作。await 放在 async 函数内部，在需要异步操作的语句前面加上 await。代码示例如下：
   ```javascript
       async function readFile(fileName) {
            // readFile()是一个异步读取文件的函数
            const content = await readFile() ;
      
            return content ;
       }
   ```
  
### 2. 调用

1. 使用async定义的函数返回值是promise对象，可以使用then()方法。 
   ```javascript
      readFile('a.txt').then((result) => {
        conole.log(result) ;
      })
   ```
### 3. 注意的问题

1. async 函数返回的是一个 Promise 对象（从文档中也可以得到这个信息）。async 函数（包含函数语句、函数表达式、Lambda表达式）会返回一个 Promise 对象，如果在函数中 return 一个直接量，async 会把这个直接量通过 Promise.resolve() 封装成 Promise 对象。

2. async函数返回一个promise对象，所以，可以使用 then() 方法添加回调函数。

3. await等待的是一个表达式，这个表达式计算结果可以是 Promise 对象也可以是其他值。
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
4. await 遇到 Promise 对象，然后阻塞后面的代码，等着 Promise 对象 Resolve，然后得到 resolve 的值，作为 await 的表达式的结果。也就是说，await 等待的是 Promise 对象执行的结果，即执行成功的结果。async 函数调用不会造成阻塞,它内部所有的阻塞都被封装在一个 Promise 对象中异步执行。
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
5. await 命令后面的 Promise 对象，运行结果可能是 rejected，所以最好把 await 命令放在 try...catch 代码块中。
   ```javascript
      try {
        await someWithPromise() ;
      } catch (err) {
        console.log(err) ;
      }
   ```
### 4. async/await 的优势

1. 优势在于 Promise 的 then() 方法的链式调用。代码示例如下：
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
2. 使用async/await使得写异步操作就像同步一样。

