<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [JavaScript 函数式编程](#javascript-%E5%87%BD%E6%95%B0%E5%BC%8F%E7%BC%96%E7%A8%8B)
  - [1. 函数柯里化 —— curried](#1-%E5%87%BD%E6%95%B0%E6%9F%AF%E9%87%8C%E5%8C%96--curried)
    - [1. 基本说明](#1-%E5%9F%BA%E6%9C%AC%E8%AF%B4%E6%98%8E)
    - [2. 代码实现](#2-%E4%BB%A3%E7%A0%81%E5%AE%9E%E7%8E%B0)
  - [2. 组合函数 —— compose](#2-%E7%BB%84%E5%90%88%E5%87%BD%E6%95%B0--compose)
    - [1. 基本说明](#1-%E5%9F%BA%E6%9C%AC%E8%AF%B4%E6%98%8E-1)
    - [2. 代码实现](#2-%E4%BB%A3%E7%A0%81%E5%AE%9E%E7%8E%B0-1)
    - [3. 组合的优势](#3-%E7%BB%84%E5%90%88%E7%9A%84%E4%BC%98%E5%8A%BF)
  - [3. 偏函数 —— partial](#3-%E5%81%8F%E5%87%BD%E6%95%B0--partial)
    - [1. 基本说明](#1-%E5%9F%BA%E6%9C%AC%E8%AF%B4%E6%98%8E-2)
    - [2. 代码实现](#2-%E4%BB%A3%E7%A0%81%E5%AE%9E%E7%8E%B0-2)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# JavaScript 函数式编程

## 1. 函数柯里化 —— curried

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
## 2. 组合函数 —— compose

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

### 3. 组合的优势
 
 1. 组合满足结合定律
    `compose(f, compose(g, h)) == compose(compose(f, g), h)`
2. 优势：允许我们把函数组合到各自所需的 compose() 函数中。换句话说，我们可以不一次性组合所需的函数，我们可以提前组合，实现一些特定的需求，然后将组合的结果同其他函数再度组合，可以得到相同的效果（不提前组合）。

## 3. 偏函数 —— partial

### 1. 基本说明

### 2. 代码实现

1. 闭包
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
                      args[i] = fullArguments[argIndex++];
                  }
              }

              return fn.apply(null, args);
          }
      }      
   ```