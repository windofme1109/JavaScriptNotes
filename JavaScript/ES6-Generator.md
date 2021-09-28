<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Generator](#generator)
  - [1. 参考资料](#1-%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99)
  - [2. 基本说明](#2-%E5%9F%BA%E6%9C%AC%E8%AF%B4%E6%98%8E)
  - [3. `yield` 表达式](#3-yield-%E8%A1%A8%E8%BE%BE%E5%BC%8F)
  - [4. `next()` 方法接收参数的意义](#4-next-%E6%96%B9%E6%B3%95%E6%8E%A5%E6%94%B6%E5%8F%82%E6%95%B0%E7%9A%84%E6%84%8F%E4%B9%89)
  - [5. `yield*` 表达式](#5-yield-%E8%A1%A8%E8%BE%BE%E5%BC%8F)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Generator

## 1. 参考资料

1. [ES6系列之Generator](https://www.jianshu.com/p/6055bd421ca4)

2. [ES6 Generator 详解](https://zhuanlan.zhihu.com/p/49577097)

3. [ES6 生成器(Generator)](https://blog.csdn.net/qq_45007419/article/details/105099517)

4. [Generator 函数的异步应用](https://es6.ruanyifeng.com/#docs/generator-async)

5. [Generator 函数的语法](https://es6.ruanyifeng.com/#docs/generator)

## 2. 基本说明

1. `Generator` 函数是 ES6 提供的一种异步编程解决方案。

2. `Generator` 函数可以理解为一个状态机，封装多个内部状态。

3. 调用 `Generator` 函数，不会执行函数内部的代码，而是返回一个遍历器对象（`Iterator Object`）。这个遍历器对象可以依次遍历 `Generator` 函数内部的每一个状态。

4. 特点：
   1. 定义时，`function` 关键字与函数名之间有一个星号（`*`）。
   2. 函数体内部，使用 `yield` 关键字定义不同的内部状态。

5. 示例：
   ```js
      function* helloWorldGenerator() {
          yield 'hello' ;
          yield 'world' ;
          return 'ending' ;
      }
      // 生成器函数，返回的是一个遍历器对象
      // 这个遍历器对象可以依次遍历Generator函数内部的每一个状态
      var hw = helloWorldGenerator() ;

      // { value: 'hello', done: false }
      console.log(hw.next()) ;
      // { value: 'world', done: false }
      console.log(hw.next()) ;
      // { value: 'ending', done: true }
      console.log(hw.next()) ;
      // { value: undefined, done: true }
      console.log(hw.next()) ;
   ```
6. 执行说明：
   - 第一次调用 `next()` 方法，`Generator` 函数从头开始执行，当遇到第一个 `yield` 语句时，停止执行，但是会返回 `yield` 后面的值。`next()` 方法返回一个对象，这个对象包含两个属性： `value` 和 `done`。 `value` 属性就是当前 `yield` 表达式的值`hello`，`done`属性的值 `false`，表示遍历还没有结束。
   - 第二次调用 `next()` 方法，`Generator` 函数从上次 `yield` 表达式停下的地方，一直执行到下一个yield表达式。 `next()` 方法返回的对象的 `value` 属性是 `yield` 表达式的值 `world`，`done` 属性的值是 `false`，表示遍历没有结束。
   - 第三次调用 `next()` 方法， `Generator` 函数从上一次yield表达式停下的地方，执行到return语句结束（如果没有 `return` 语句，就执行到函数结束）。next方法返回的对象的 `value` 属性，就是紧跟在 `return` 语句后面的表达式的值（如果没有return语句，则 `value` 属性的值为 `undefined`），`done`属性的值 `true`，表示遍历已经结束。
   - 第四次调用 `next()` 方法，由于 `Generator` 函数已经执行完毕，所以，返回的对象的 `value` 属性的值是 `undefined`，`done` 属性的值是 `true`。以后再调用 `next()` ，都返回的是这个结果。

7. 总结一下，调用  `Generator`  函数，返回一个遍历器对象，代表  `Generator`  函数的内部指针。以后，每次调用遍历器对象的 `next()` 方法，就会返回一个有着 `value` 和 `done` 两个属性的对象。 `value` 属性表示当前的内部状态的值，是yield表达式后面那个表达式的值；`done` 属性是一个布尔值，表示是否遍历结束。

8. 每次调用 `next()` 方法，内部的指针从函数头部或上一次停下来的地方开始执行，直到遇到下一个 `yield` 语句（或 `return` 语句）为止。

## 3. `yield` 表达式

1. 由于  `Generator`  函数返回的遍历器对象，只有调用 `next` 方法才会遍历下一个内部状态，所以其实提供了一种可以暂停执行的函数。`yield` 表达式就是暂停标志。

2. 遍历器对象的`next`方法的运行逻辑如下。
   1. 遇到 `yield` 表达式，就暂停执行后面的操作，并将紧跟在 `yield` 后面的那个表达式的值，作为返回的对象的 `value` 属性值。
   2. 下一次调用 `next` 方法时，再继续往下执行，直到遇到下一个 `yield`表达式。
   3. 如果没有再遇到新的 `yield` 表达式，就一直运行到函数结束，直到 `return` 语句为止，并将 `return` 语句后面的表达式的值，作为返回的对象的 `value` 属性值。
   4. 如果该函数没有 `return` 语句，则返回的对象的 `value` 属性值为 `undefined`。

3. 需要注意的是，`yield`表达式后面的表达式，只有当调用 `next` 方法、内部指针指向该语句时才会执行，因此等于为 JavaScript 提供了手动的“惰性求值”（Lazy Evaluation）的语法功能。上述节选自《ES6标准入门》。

4. `yield` 表达式只能使用在 `Generator` 函数中，用在其他地方会报错。
   ```js
      // SyntaxError: Unexpected number
      (function demo() {
          yield 1 ;
      })()
   ```


5. `yield` 表达式用在另一个表达式中，必须放在圆括号中。
   ```js
      function* demo() {
          // SyntaxError: Unexpected identifier
          console.log('hello' + yield 123) ;
          console.log('world' + (yield 456)) ;
      }
   ```


6. `yield` 表达式作为函数的参数或者在赋值表达式的右侧，可以不必加括号。
   ```js
      function* demo() {
          // ok
          foo(yield 'a', yield 'b') ;
          // ok
          var y = yield 10 + 20 ;
      }  
   ```
7.  `next()` 方法可以接收一个参数，这个参数可以作为上一个语句的返回值。

8. `yield` 语句本身没有返回值，或者说，总是返回 `undefined`。

9. 示例：
   ```js
      function* foo(x) {
           var y = 2 * (yield (x + 1)) ;
           var z = yield (y / 3) ;

           return x + y + z ;
       }

       var a = foo(5) ;
       // { value: 6, done: false }
       console.log(a.next()) ;
       // { value: NaN, done: false }
       console.log(a.next()) ;
       // { value: NaN, done: true }
       console.log(a.next()) ;
   ```
10. 执行说明：
    - 第一次调用 `next()` ，由于 `foo()` 接收了 `5` 作为参数，返回 `yield` 后面的语句：`x+1`，所以 `value` 的值是 `6`。
    - 第二次调用 `next()` ，执行第二个 `yield` 语句，由于 `y` 等于 `2 * yield(x + 1)`，而 `yield` 本身没有返回值，所以，`y` 是 `2 * undefined`（即 `NaN`），此时返回的是 `NaN / 3`，所以还是 `NaN`。
    - 第三次调用 `next()` ，执行 `return` 语句，由于 `z` 等于 `yield(y / 3)`，而 `yield` 本身没有返回值，所以，`z` 是 `undefined`，所以 `x+y+z` 等同于 `5 + NaN + undefined`，所返回的 `value` 依然是 `NaN`。
    - 如果我们给 `next()` 传入了参数，那么情况就不一样了：
      ```js
         // 给next()传入参数
        // { value: 6, done: false }
        console.log(a.next()) ;
        // { value: 8, done: false }
        console.log(a.next(12)) ;
        // { value: 42, done: true }
        console.log(a.next(13)) ;        
      ```
    - 第一次调用 `next()` ，由于 `foo()` 接收了 `5` 作为参数，返回 `yield` 后面的语句：`x+1`，所以 `value` 的值是 `6`。
    - 第二次调用 `next()` ，并传入了 `12` 作为参数。执行第二个 `yield` 语句。由于 `12` 作为上一次的 `yield` 的返回值，所以 `y` 被赋予了 `2 * 12` 为 `24`，此时返回的是 `y / 3`,即 `24 / 3` 为 `8`。 
    - 第三次调用 `next()` ，并传入了 `13` 作为参数。执行 `return` 语句，`13` 会作为第二个 `yield` 语句的返回值。所以 `z` 等于 `13`。所以返回的结果是 `5 + 24 +13` 等于 `42`。

11. **注意**：由于 `next` 方法的参数表示上一个 `yield` 表达式的返回值，所以在第一次使用 `next` 方法时，传递参数是无效的。V8 引擎直接忽略第一次使用 `next` 方法时的参数，只有从第二次使用 `next` 方法开始，参数才是有效的。从语义上讲，第一个 `next` 方法用来启动遍历器对象，所以不用带有参数。节选自《ES6标准入门》。

## 4. `next()` 方法接收参数的意义

1.  `next()` 方法接收参数这个功能有很重要的语法意义。 `Generator` 函数从暂停状态到恢复运行，它的上下文状态（context）是不变的。通过next方法的参数，就有办法在  `Generator` 函数开始运行之后，继续向函数体内部注入值。也就是说，可以在  `Generator` 函数运行的不同阶段，从外部向内部注入不同的值，从而调整函数行为。节选自《ES6标准入门》。

2. 示例：
   ```js
      function* dataConsumer() {
          console.log('started') ;
          console.log(`1.${yield}`) ;
          console.log(`2.${yield}`) ;
          return 'ended'
      }

      var dc = dataConsumer() ;
      // started
      dc.next() ;
      // 1.5
      dc.next('5') ;
      // 1.8
      dc.next('8') ;
   ```
   传入了不同的参数，`yield` 语句会返回不同的值，当然也会执行 `yield` 所在那行的语句。

2. 我们可以使用 `for…of` 遍历 `Generator` 函数返回的 `Iterator` 对象，从而不需要使用 `next()` 方法。示例如下：
   ```js
      function* getCount() {
          yield 0 ;
          yield 1 ;
          yield 2 ;
          yield 3 ;
          yield 4 ;
          yield 5 ;
      }

      var g = getCount() ;
      for (let item of g) {
          // 0 1 2 3 4 5
          console.log(item)
      }   
   ```
   使用 `for…of` 循环，依次输出了 `5` 个 `yield` 表达式的值。

3. **注意**：一旦`next`方法的返回对象的 `done` 属性为 `true`，`for...of` 循环就会中止，且不包含该返回对象，所以上面代码的 `return` 语句返回的 `6`，不包括在 `for...of` 循环之中。

4. 使用 `Generator` 实现斐波那契数列：
   ```js
      // 使用 Generator 实现斐波那契数列
      function* fib() {
          var [pre, cur] = [0, 1] ;
          while (true) {
              yield cur ;
              [pre, cur] = [cur, pre + cur]
          }
      }

      var f = fib() ;
      for (let item of f) {
          if (item > 1000) {
              break
          }
      // 1 1 2 3 5 8 ...
          console.log(item)
      } 
   ```
5. 除了 `for..of` 循环，扩展运算符（`…`）、结构赋值和 `Array.from()` 方法都可以用在 `Generator` 函数返回的 `Iterator` 对象上。示例：
   ```js
      function* numbers() {
          yield 1 ;
          yield 2 ;
          return 3 ;
      }

     var n = numbers() ;
      var a = [...n] ;
      // 扩展运算符
      // [ 1, 2 ]
      console.log(a) ;

      // Array.from()方法
      // // [ 1, 2 ]
      console.log(Array.from(a)) ;

      // 解构赋值
      let [x, y] = a ;
      // 1
      console.log(x) ;
      // 2
      console.log(y) ;  
   ```
6. `for…of` 遍历，只是遍历 `yield` 表达式的值。扩展运算符、解构赋值和 `Array.from()` 方法也是获取的 `yield` 表达式的值。所以，`return` 的语句的值就不会获取。

## 5. `yield*` 表达式

1. `yield*` 表达式用于在一个 `Generator` 函数内部执行另一个 `Generator` 函数。示例：
   ```js
      function* bar1() {
          yield 'a' ;
          yield 'b' ;
       }

       function* foo1() {
           yield 'x' ;
           // yield*表达式运行在一个generator内部调用另一个generator函数
           yield* bar1() ;
           yield 'y'

       }

       for (let v of foo1()) {
       // x a b y
           console.log(v)
       }
   ```
   上面的代码等同于：
   ```js
       function* bar1() {
           yield 'a' ;
           yield 'b' ;
       }

        function* foo1() {
            yield 'x' ;
            // yield*表达式运行在一个generator内部调用另一个generator函数
            // yield* bar1() ;
            for (let item of bar1()) {
                yield item ;
            }
            yield 'y'

        }

        for (let v of foo1()) {
            // x a b y
            console.log(v)
        } 
   ```

2. 如果不使用 `yield*` 表达式，直接使用 `yield` 表达式，会怎么样呢？
   ```js
      function* bar1() {
          yield 'a' ;
          yield 'b' ;
      }

      function* foo1() {
          yield 'x' ;
   

          // 不使用yield*表达式
          // 直接使用yield表达式调用Generator函数
          yield bar1() ;

          yield 'y'

      }


      var gen = foo1() ;

      // x
      gen.next().value ;
      // 一个Iterator对象
      gen.next().value ;
      // y
      gen.next().value ;
   ```
   对比：
   ```js
      function* foo2() {
          yield 'x' ;

          yield* bar1() ;

          yield 'y' ;
      }


      var gen2 = foo2() ;

      // x
      console.log(gen2.next().value) ;
      // a
      console.log(gen2.next().value) ;
      // b
      console.log(gen2.next().value) ;
      // y
      console.log(gen2.next().value) ; 
   ```
   作为对比，`foo1` 返回的是一个 `Itreator` 对象，而 `foo2` 返回的是该`Itreator` 内部的值。

3. 如果 `yield` 后面跟的是一个 `Iterator` 对象，那么就需要在 `yield` 后面加一个星号，表示返回的是一个 `Iterator` 对象，可以对这个 `Iterator` 对象进行遍历。

4. `yield*` 后面的  `Generator`  函数（没有 `return` 语句时），等同于在  `Generator`  函数内部，部署一个 `for...of` 循环（如前面的例子所示）。

5. 在有 `return` 语句时，则需要用`var value = yield* iterator`的形式获取return语句的值。
   ```js
      function* getNum() {
          yield 1 ;
          yield 2 ;

          return 'done' ;
      }

      function* userNum() {
          yield 3 ;
          // 由于getNum()由返回值，所以要用一个变量接收这个返回值
          // 当只有遍历完getNum()的yield表达式的值以后，最后才会得到      return的返回值
          // 并将这个值赋给ret，然后继续向下执行
          var ret = yield* getNum() ;
          console.log('ret: ' + ret) ;
          yield 5 ;
      }

      var un = userNum() ;

      // { value: 3, done: false }
      un.next();

      // { value: 1, done: false }
      un.next() ;

      // { value: 2, done: false }
      un.next() ;

      // ret: done
      // { value: 5, done: false }
      un.next() ;   
   ```
   在第四次调用 `next()` 时，控制台会有输出，过程是这样的：第三次调用 `next()` 时，`getNum()` 返回的 `Iterator` 对象已经遍历结束。此时应该执行的是return 'done' ，所以ret的值会被设置为done。然后，第四次调用 `next()` 方法，由于yield*表达式已经执行完毕，所以继续向下执行 `console.log('ret: ' + ret)`，所以控制台会输出`ret: done`。同时执行到 `yield 5` 结束。此时 `next()` 返回的对象就是 `{ value: 5, done: false }`。

6. 任何数据结构只要实现了 `Iterator` 接口，就可以被 `yield*` 遍历。也就是说，我们可以对数组、字符串进行遍历。
