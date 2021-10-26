<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [JavaScript 函数式编程](#javascript-%E5%87%BD%E6%95%B0%E5%BC%8F%E7%BC%96%E7%A8%8B)
  - [1. 参考资料](#1-%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99)
  - [2. 函数柯里化 —— curried](#2-%E5%87%BD%E6%95%B0%E6%9F%AF%E9%87%8C%E5%8C%96--curried)
    - [1. 基本说明](#1-%E5%9F%BA%E6%9C%AC%E8%AF%B4%E6%98%8E)
    - [2. 代码实现](#2-%E4%BB%A3%E7%A0%81%E5%AE%9E%E7%8E%B0)
  - [3. 组合函数 —— compose](#3-%E7%BB%84%E5%90%88%E5%87%BD%E6%95%B0--compose)
    - [1. 基本说明](#1-%E5%9F%BA%E6%9C%AC%E8%AF%B4%E6%98%8E-1)
    - [2. 代码实现](#2-%E4%BB%A3%E7%A0%81%E5%AE%9E%E7%8E%B0-1)
    - [3. 组合的优势](#3-%E7%BB%84%E5%90%88%E7%9A%84%E4%BC%98%E5%8A%BF)
  - [4. 管道函数 —— pipe](#4-%E7%AE%A1%E9%81%93%E5%87%BD%E6%95%B0--pipe)
    - [1. 基本说明](#1-%E5%9F%BA%E6%9C%AC%E8%AF%B4%E6%98%8E-2)
    - [2. 代码实现](#2-%E4%BB%A3%E7%A0%81%E5%AE%9E%E7%8E%B0-2)
  - [5. 偏函数 —— partial](#5-%E5%81%8F%E5%87%BD%E6%95%B0--partial)
    - [1. 基本说明](#1-%E5%9F%BA%E6%9C%AC%E8%AF%B4%E6%98%8E-3)
    - [2. 代码实现](#2-%E4%BB%A3%E7%A0%81%E5%AE%9E%E7%8E%B0-3)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# JavaScript 函数式编程


## 1. 参考资料

1. [前端同学如何函数式编程？](https://juejin.cn/post/7012523441609768997)

2. [函数式编程入门教程](http://www.ruanyifeng.com/blog/2017/02/fp-tutorial.html)

3. [函数式编程进阶：杰克船长的黑珍珠号](https://juejin.cn/post/6844904034260910094)

4. [JS函数式编程究竟是什么?](https://juejin.cn/post/6844903907647291400)

5. [浅析JavaScript函数式编程](https://juejin.cn/post/6940442700889980965)

6. [函数式编程（五）—— 函子](https://zhuanlan.zhihu.com/p/339722986)

7. [函数式编程进阶：应用函子](https://zhuanlan.zhihu.com/p/275686659)

8. [函数式编程 -- 函子（Functor）](https://blog.csdn.net/zimeng303/article/details/109280319)

9. [JavaScript ES6函数式编程（三）：函子](https://www.cnblogs.com/chenwenhao/p/11742517.html)

## 2. 函数柯里化 —— curried

### 1. 基本说明

1. 作用 —— 将多参数函数转换为单一参数函数

2. 参考资料
   - [「前端面试题系列6」理解函数的柯里化](https://segmentfault.com/a/1190000018180159)
   - [详解JS函数柯里化](https://www.jianshu.com/p/2975c25e4d71)

3. 基本思想
  - 闭包
  - 递归
  - 判断当前函数接收的参数数量是否等于预定的函数的参数个数
  - apply() 执行预定的函数

### 2. 代码实现

1. 基本版
   ```js
      function curriedFunc(fn) {
          if (typeof fn !== 'function') {
              throw Error('fn must be function');
          }
          return function curried() {
              const args = Array.prototype.slice.call(arguments);
              if (arguments.length !== fn.length) {
                  return function () {
                      return curried.apply(this, args.concat(...arguments));
                  }

              }

              return fn.apply(this, args);
          }
      }
   ```
2. 简约版
   ```js
      function curriedFunc(fn, params) {
          return function() {
              let args = Array.prototype.slice.call(arguments);

              if (params !== undefined) {
                  args = args.concat(params);
              }

              if (args.length !== fn.length) {
                  return curriedFunc(fn, args);
              }

              return fn.apply(null, args);
          }
      }
   ```

## 3. 组合函数 —— compose

### 1. 基本说明

1. 将单一功能的函数组合为功能复杂的函数

2. 核心思想：每一个函数的输出是另一个函数的输入

3. 组合的数据流是从右向左的

### 2. 代码实现

1. 闭包 + reduce
   ```js
      /**
       * 组合多个函数
       * 使用reduce()，一个一个执行fns数组中的函数
       * composeN()的返回值是一个函数，这个函数接收一个参数，是初始值
       * 我们将这个初始值作为reduce()的初始值，传入reduce()函数中
       * 还需要对fns数组进行反转，因为我们传入的函数执行的顺序是从右向左
       * 但是reduce()迭代数组是从左向右，为了保证函数执行顺序的正确，我们需要将数组反转
       *
       *
       * @param fns 用来接收多个函数
       * @returns {function(*=): *[]}
       */
       const compose = (...fns) => {
           // 对fns数组进行反转，保证函数的执行顺序是从右向左
           // 因为 reduce 的执行顺序是从左向右
           fns.reverse() ;
           return (value) => reduce(fns, (acc, fn) => fn(acc), value) ;
       }
   ```
2. 使用原生的 reduce() 方法：
   ```js
      function compose() {
          const fnList = Array.prototype.slice.call(arguments);
          fnList.reverse();
          return function(value) {
              return fnList.reduce((acc, fn) => {
                  return fn(acc);
              }, value);
          }
      }
   ```
3. 使用原生的 reduceRight() 方法：
   ```js
      function compose() {
          const fnList = Array.prototype.slice.call(arguments);
          return function(value) {
              // reduceRight() 是从右向左遍历数组，实现对数组数据的规约
              return fnList.reduceRight((acc, fn) => {
                  return fn(acc);
              }, value);
          }
      }
   ```

### 3. 组合的优势
 
 1. 组合满足结合定律
    `compose(f, compose(g, h)) == compose(compose(f, g), h)`
2. 优势：允许我们把函数组合到各自所需的 compose() 函数中。换句话说，我们可以不一次性组合所需的函数，我们可以提前组合，实现一些特定的需求，然后将组合的结果同其他函数再度组合，可以得到相同的效果（不提前组合）。

## 4. 管道函数 —— pipe

### 1. 基本说明

1. 组合函数的数据流向是从右向左的，实际上我们也可以让数据从左向右流动，即先执行左侧的函数，再执行右侧的函数。就像 Unix 中的管道操作符一样：`|`，器数据流向就是从左向右。

2. 从左向右处理数据流的过程被称为管道（pipeline）或者序列（sequence）。

### 2. 代码实现

1. pipe() 方法与组合函数的实现方法类似，就是闭包 + reduce()，不同的是不需要对函数列表进行反转。

2. 代码实现：
   ```js
      function pipe() {
          const args = Array.prototype.slice.call(arguments);
   
          return (value) => {
             return args.reduce((acc, fn) => {
                 return fn(acc);
             }, value);
          }
      }
   ```

## 5. 偏函数 —— partial

### 1. 基本说明

1. 偏函数，固定一个函数的一个或多个参数，然后返回一个新函数，在新函数中传入剩下的参数。

2. 对任意位置进行占位。

3. 柯里化可以理解为是一种特殊的偏函数。

### 2. 代码实现

1. 闭包 —— 基础版
   ```js
      /**
       *
       * 偏函数
       * 偏函数用来将一个函数中的几个参数固定下来，并返回一个新的函数
       * 后面使用这个参数更少的函数
       *
       */

      const partial = (fn, ...fixedArgs) => {
          let args = fixedArgs;

          return (...fullArguments) => {
              let argIndex = 0;
              for (let i = 0; i < fixedArgs.length && argIndex < fullArguments.length; i++) {
                  if (args[i] === undefined) {
                      args[i] = fullArguments[argIndex];
                      argIndex = argIndex + 1;
                  }
              }

              return fn.apply(null, args);
          }
      }      
   ```
  
2. 进阶版 —— 带占位符（仿照 underscore 中的 partial）
   - 实现方式 1
     ```javascript
   
      // 全局变量
      const _ = 'PLACEHOLDER';
      
      /**
       * 偏函数 - 占位符
       * 定义全局变量：_
       * 使用 _ 表示占位
       * @param fn
       * @returns {function(): *}
       */
       const partial2 = (fn, ...fixedArgs) => {
           return (...restArgs) => {
               // 建立 fixedArgs 的一个副本，使用这个副本接收完整的参数
               // 避免多次调用固定参数后的函数，对 fixedArgs 造成污染
               let args = fixedArgs.slice(0);

               let position = 0;

               for (let i = 0; i < args.length; i++) {
                   if (args[i] === _) {
                       args[i] = restArgs[position];
                       position += 1;
                   }
               }

               while (position < restArgs.length) {
                   args.push(restArgs[position]);
                   position += 1;
               }
               console.log(args);
               return fn.apply(this, args);
           }

       }
     
      function sub(x, y) {
         return y - x;
      }
      const subFive = partial(sub, 5);
      const subFrom20 = partial(sub, _, 20);
      // 15
      console.log(subFive(20));
      // 18
      console.log(subFrom20(2));
     ```
   - 实现方式 2：
     ```js

        // 全局变量
        const _ = 'PLACEHOLDER';
        
     ```
