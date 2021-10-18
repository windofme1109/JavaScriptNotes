# JavaScript 中的 Thunk 函数

## 1. 参考资料

1. [Thunk 函数的含义和用法](http://www.ruanyifeng.com/blog/2015/05/thunk.html)

## 2. 参数求值策略

1. 对于一个函数，如果其接收的参数，是一个表达式，如下所示：
   ```js
      const x = 3;
      function f(n) {
          return n * 2;
      }
   
      f(x + 5);
   ```
2. f() 接收的参数是一个表达式：`x + 5`，那么我们应该什么时候计算 `x + 5` 呢？

3. 第一种意见是“传值调用”（call by value），即在传入函数体之前就计算 x + 5 的值，然后将这个值即 8，传入 f() 中。C语言就采用这种策略。
   ```js
      f(x + 5);
      // 等同于
      f(8);
   ```

4. 另外一种意见是“传名调用”（call by name），即直接将表达式 `x + 5` 整体传入函数体内，只有在用到它的时候求值。Haskell语言采用这种策略。
   ```js
      f(x + 5);
      // 等同于
      (x + 5) * 2
   ```
5. 传值调用和传名调用，哪一种策略比较好？回答是各有利弊。传值调用比较简单，但是对参数求值的时候，实际上还没用到这个参数，有可能造成性能损失。
   ```js
      function f(a, b) {
          return b;
      }
      const x = 5;
      f(x*x + 2 * x - x / 2, 5);
   ```
   上面的例子中，f() 的第一个参数是一个复杂的表达式，如果是传值调用，就会提前将这个表达式的值算出来，但实际上，f() 根本没有用到这个值。这样就会造成性能的损失。

## 3. Thunk 函数

1. 编译器的"传名调用"实现，往往是将参数放到一个临时函数之中，再将这个临时函数传入函数体。这个临时函数就叫做 Thunk 函数。举例如下：
   ```js
      function f(x) {
          return x * 2;
      }

      const n = 5;

      f(n + 5);

      // 等同于

      const thunk = () => {
          return n + 5;
      }

      function f(thunk) {
          return thunk() * 2;
      }
   ```
2. 上面示例中，函数 f() 的参数 x + 5 被一个函数替换了。凡是用到原参数的地方，对 thunk() 函数求值即可。

3. 这就是 Thunk 函数的定义，它是"传名调用"的一种实现策略，用函数来替换某个表达式。

4. 也就是说，Thunk 函数是函数式编程的一种思想。用函数来替代具体的求值。

## 4. JavaScript 中的 Thunk 函数

1. JavaScript 采取的是传值调用.因此它的 Thunk 函数含义有所不同。在 JavaScript 中，Thunk 函数替换的不是表达式，而是多参数函数，将其替换成单参数的版本，且只接受回调函数作为参数。
   ```js
      // 多参数函数，最后一个参数是回调函数
       fs.readFile(path, callback);

       // 经过 Thunk 函数转换，变成单参数的函数
       const readFileThunk = Thunk(path);
       readFileThunk(callback);


       function Thunk(path) {
           return function (callback) {
              fs.readFile(path, callback);
           }
       }
   ```

2. 上面的例子中，readFile 是一个多参数函数，最后一个参数是回调函数，经过 Thunk() 函数的转换，变成 readFileThunk()，这个函数只接收回调函数作为参数，这个单参数，且只接收回调函数的版本，就叫做 Thunk() 函数。

3. 任何函数，只要参数有回调函数，就能写成 Thunk 函数的形式，下面是一个简易的 Thunk 函数转换器：
   ```js
      
      
      
      /**
        * 接收一个带有回调函数的参数的函数，这个回调函数参数必须是最后一个参数
        *
        * @param fn 最后一个参数是回调函数的函数
        * @returns {function(): function(*): *}
        * @constructor
        */
      function Thunk(fn) {
          return function () {
              const args = [...arguments];
              return function (callback) {
                  // args.push(callback);
                  return fn.apply(this, [...args, callback]);
              }
          }
      }
   ```

4. 下面使用简易的 Thunk 函数转换器实现 readFile() 的转换：
   ```js
      
      const fs = require('fs');
      
   
      /**
        * readFileThunk() 是一个函数，是 fs.readFile() 经过 Thunk 函数的包装后的结果
        * readFileThunk() 接收的参数是 fs.readFile() 的回调函数前面的参数
        * readFileThunk() 的返回值也是一个函数，这个函数接收的是 fs.readFile() 的最后一个回调函数
        *
        * @type {function(): function(*): *}
        */
      const readFileThunk = Thunk(fs.readFile);

      const path = './aa.txt';

      function callback(err, res) {
          if (err) {
             console.log('err', err);
          }
          
          // I like this night!
          console.log(res.toString());
      }

      readFileThunk(path)(callback);
   ```

## 5. Thunkify 模块

1. 生产环境对于带有回调函数的函数的转换建议使用 Thunkify 模块。

2. npm 链接：[Thunkify](https://www.npmjs.com/package/thunkify)

3. Thunkify 的主要作用是将传统的 node 函数（回调风格的函数）转换成 Thunk 函数。转换后的 Thunk 函数主要用在基于 Generator 的自动流程管理上，比如说 Generator 的自动执行器 co 模块。

4. Thunkify 的源码与上面的简易的 Thunk 非常类似：
   ```js 
      function thunkify(fn){
          return function(){
      var args = new Array(arguments.length);
      var ctx = this;

          for(var i = 0; i < args.length; ++i) {
            args[i] = arguments[i];
          }

          return function(done){
            var called;

            args.push(function(){
              if (called) return;
              called = true;
              done.apply(null, arguments);
            });

            try {
              fn.apply(ctx, args);
            } catch (err) {
              done(err);
            }
          }
      }
      };
   ```
5. 它的源码主要多了一个检查机制，变量 called 确保回调函数只运行一次。


## 7. Thunk 函数与 Curried 函数的区别

1. Thunk 函数用于延迟执行。只有真正用到的地方才去执行 Thunk 函数。

2. Curried 函数用于将多参数函数转换成单参数函数。 主要目的是对函数进行部分配置，以实现重用函数，提供更好地接口。
