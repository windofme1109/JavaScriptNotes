<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Thunk 函数与 Generator](#thunk-%E5%87%BD%E6%95%B0%E4%B8%8E-generator)
  - [1. 参考资料](#1-%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99)
  - [2. `Thunk` 函数的作用](#2-thunk-%E5%87%BD%E6%95%B0%E7%9A%84%E4%BD%9C%E7%94%A8)
    - [1. 手动执行 `Generator` 函数](#1-%E6%89%8B%E5%8A%A8%E6%89%A7%E8%A1%8C-generator-%E5%87%BD%E6%95%B0)
    - [2. 自动执行 `Generator` 函数](#2-%E8%87%AA%E5%8A%A8%E6%89%A7%E8%A1%8C-generator-%E5%87%BD%E6%95%B0)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Thunk 函数与 Generator

## 1. 参考资料

1. [Thunk 函数的含义和用法](http://www.ruanyifeng.com/blog/2015/05/thunk.html)

## 2. `Thunk` 函数的作用

1. `Thunk` 函数主要是用于 `Generator` 自动流程管理上。

### 1. 手动执行 `Generator` 函数

1. 举个例子，我们使用 `Thunk()` 函数将 `fs.readFile` 封装为 具有 `Thunk` 风格的函数，如下所示：
   ```js
      function Thunk(fn) {
          return function() {
              const args = Array.prototype.slice.call(arguments);
              return function(callback) {
                  args.push(callback);
                  return fn.apply(this, args);
              }
          }
      }
   
      const fs = require('fs');
      const readFileThunk = Thunk(fs.readFile);
   ```

2. 使用 `Generator` 结合 `readFileThunk()`，封装两个读取文件的异步操作：
   ```js
      function* gen() {
          const content1 = yield readFileThunk('./static/aa.txt');
          console.log('content1', content1.toString());
          const content2 = yield readFileThunk('./static/bb.txt');
          console.log('content2', content2.toString());

      }
      // 输出
      // content1 I like this night!
      // content2 I like this night too!
   ```
3. yield 中断 `Generator` 函数的执行，我们需要在 `Generator` 函数外部调用 `next()` 方法恢复 `Generator` 函数的执行。首先手动的方式执行 `next()` 函数：
   ```js
      const g = gen();

       const next1 = g.next();

       // { value: [Function (anonymous)], done: false }
       console.log(next1);

       next1.value(function(err, data1) {
           if(err) {
               return g.throw(err);
           }

           const next2 = g.next(data1);
           // { value: [Function (anonymous)], done: false }
           console.log(next2);
           next2.value(function (err, data2) {
               if(err) {
                   return g.throw(err);
               }

               const next3 = g.next(data2);
               // { value: undefined, done: true }
               console.log(next3);
           });
       });
   ```
4. 手动执行的过程如下：
    1. `readFileThunk()` 返回的是一个函数，这个函数接收的是 `fs.readFile()` 的回调函数。当第一次调用 `next()` 的时候，执行第一个 yield 语句，所有 `next()` 函数的返回值就是：`{ value: [Function (anonymous)], done: false }`，value 的属性值就是前面说的 `readFileThunk()` 返回的函数。然后我们执行这个函数，并传入一个回调函数。在这个回调函数的内部，我们能拿到读取 `aa.txt` 的结果 —— `data1`。
    2. 第二次调用 `next()`，并将 `data1` 传入 `next()` 中， `Generator` 函数既可以向下执行，同时又能拿到异步读取文件的结果。这样 `data1` 会作为 第一个 `yield` 语句的返回值赋值给 `content1`。所以 `console.log('content1', content1.toString());` 会输出 `content1 I like this night!`。接着 `Generator` 继续向下执行，到第二个 `yield` 会暂停下来，第二个 `yield` 后面还是跟着 `readFileThunk()` 函数，所以 第二个 `next()` 的返回值是 `{ value: [Function (anonymous)], done: false }`，value 属性值还是一个函数。我们继续执行这个函数，并传入一个回调函数，同样，在这个回调函数内部，我们能拿到 `bb.txt` 的结果 —— `data2`。
    3. 第三次调用 `next()`，并将 `data2` 传入 `next()` 中， `Generator` 函数既可以继续向下执行，同时又能拿到异步读取文件的结果。这样 `data2` 作为第二个 `yield` 语句的返回值赋值给 `content2`。所以 `console.log('content2', content2.toString());` 会输出 `content2 I like this night too!`。接着 `Generator` 继续向下执行，因为后面没有 `yield` 语句，所以 `Generator` 函数会一路执行到底。而 `Generator` 函数没有返回值，所以第三个 `next()` 的返回值是：`{ value: undefined, done: true }`。

5. 从上面的例子中，我们发现，传入第一个 `next()` 和 第二个 `next()` 返回值的 `value` 属性的回调函数的结构类似，可以简化为下面的模型：
   ```js
      function callback(err, data) {
          if (err) {
              return g.throw(err);
          }
          const next = g.next(data);
      }
   ```
6. 我们可以看出 `Generator` 函数的执行过程，就是将同一个回调函数反复传入 `next()` 函数的 `value` 属性值（`value` 的属性值必须是接收回调函数作为参数的函数）中。这里的问题是，回调函数内部又嵌套调用 `value` 属性值表示的函数，这个函数又接收回调函数，...，也就是 `Generator` 函数内有多少个 `yield` 表达式，我们就需要嵌套多少层，这显然是不可能的，因为 `Generator` 函数内部的 `yield` 数量不确定，所以，我们不能使用这种方式执行 `Generator` 函数。

### 2. 自动执行 `Generator` 函数

1. 即然是一层一层的嵌套，我们可以使用递归来实现这个嵌套过程。在回调函数内部调用 `value` 属性代表的方法，然后将回调函数传入禁区，如下所示：
   ```js
      /**
        * Generator 自动执行器
        * 接收一个 Generator 函数作为参数，返回值是一个函数，调用这个返回值函数，就能自动执行 Generator 函数
        * 要求 Generator 函数内部 yield 后面必须跟着 Thunk 函数
        * @param gen Generator 函数
        * @returns {(function(): void)|*}
        */
          function generatorToAsync(gen) {
              return function () {
              const g = gen.apply(this, arguments);

               /**
                * node 风格的回调函数，即第一个参数是 err，第二个参数是拿到的数据，内部有一个递归的过程，因此能实现 Generator 函数的自动执行
                * 
                * @param err
                * @param data
                * @returns {*}
                */
               function go(err, data) {

                   const next = g.next(data);

                   if (err) {
                       return g.throw(err);
                   }
                   if (next.done) {
                       // done 属性为 true，表示 Generator 函数执行完毕，因此终止回调函数执行
                       return;
                   }
                   // 递归地调用 value() 属性代表的方法，这个方法接收一个回调函数作为参数，而这个参数就是 go() 本身
                   next.value(go);

               }

               // 第一次执行
               go();
           }
      }
   ```
2. 上面的 `generatorToAsync()` 函数就是一个 `Generator` 函数的自动执行器。内部的 `go()` 函数就是 `Thunk` 的回调函数。 `go()` 函数先将指针移到 `Generator` 函数的下一步（g.`next()` 方法），然后判断 `Generator` 函数是否结束（next.done 属性），如果没结束，就将 `go()` 函数再传入 `Thunk` 函数（`result.value` 属性），否则就直接退出。然后我们需要手动执行 `go()` 函数一次，这样才能开始执行 `Generator` 函数。

3. 这样，我们只需要将 `Generator` 函数传入 `generatorToAsync()` 这个自动执行器函数中，无论 `Generator` 函数中有多少个异步操作（`yield` 语句），都能自动执行完成。**需要注意的是**：每一个异步操作，都要是 `Thunk` 函数，也就是说，跟在 `yield` 命令后面的必须是 `Thunk` 函数。
