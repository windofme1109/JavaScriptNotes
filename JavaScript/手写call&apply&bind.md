<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [手写 call、apply 和 bind](#手写-callapply-和-bind)
  - [1. 参考资料](#1-参考资料)
  - [2. call](#2-call)
  - [3. apply](#3-apply)
  - [4. bind](#4-bind)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 手写 call、apply 和 bind

## 1. 参考资料

1. [前端面试之手写一个bind方法](https://juejin.cn/post/6844903688897576974)
2. [JS手写bind函数](https://juejin.cn/post/6844904056431837197)
3. [都2020年了，你应该知道如何手写Call、Apply、Bind了吧](https://juejin.cn/post/6844904042452221960)
4. [js基础-面试官想知道你有多理解call,apply,bind？[不看后悔系列]](https://juejin.cn/post/6844903906279964686)


## 2. call

1. call 的主要作用是改变函数的 this 指向。

2. 实现的要点：
   1. 第一个参数是绑定的 this 对象，第一个参数不传入时，this 默认为 window。
   2. 第二个及后面的参数，是调用 call 的函数的参数。
   3. 调用了 call()，相当于执行了调用 call 的函数。

3. 实现过程：
   ```javascript
      Function.prototype.myCall = function (context) {
          // 判断是否传入绑定的 this 对象
          context = context ? context : window;
          
          // this 指向调用 call 的函数
          // context.fn = this;

          // 优化
          // 使用 ES6 新增的数据类型 Symbol，保证 fn 是独一无二的，使得其不影响 context 中原有的属性
          let fn = Symbol('fn');
          context[fn] = this;

          // 取出 call 第二个及后面的参数，因为 call 的第二个以及后面的参数是传入 fn 的
          const args = [].slice.call(arguments).slice(1);
          
          // 执行 fn，并传入 call 接收的参数
          // const ret = context.fn(...args);
          const ret = context[fn](...args);

          // 执行完毕以后，删除 fn 属性
          // delete context.fn;
          delete context[fn];

          return ret;
       }
   
   ```
4. 实现说明
   - 判断是否传入绑定的 this 对象，如果没有传入 this 对象，将context 设置为 window。
   - 使用 ES6 新增的数据类型 Symbol，保证 fn 是独一无二的，使得其不影响 context 中原有的属性。
   - 将原函数添加到 context 中。
   - 取出 call 第二个及后面的参数，因为 call 的第二个以及后面的参数是传入 fn 的。
   - 执行 fn
   - 执行完毕以后，删除 fn 属性。
   - 返回执行结果。

## 3. apply

1. apply 函数与 call 的功能类似，都是改变函数的 this 指向，区别就是 apply 只接收两个参数，其中第二个参数为数组，而 call 可以接收多个参数。

2. apply 的实现过程与 call 类似：
   ```javascript
      Function.prototype.myApply = function (context) {
          context = context || window;

          // 绑定函数
          // context.fn = this;
          // 使用 ES6 新增的数据类型 Symbol，保证 fn 是独一无二的，使得其不影响 context 中原有的属性
          let fn = Symbol('fn');
          context[fn] = this;


          if (!Array.isArray(arguments[1])) {
              // 第二个参数必须是数组，如果不是数组，抛出异常
             throw new TypeError('second parameter must be array');
          }
          let ret;

          if (arguments[1]) {
              // apply 只接收两个参数，第二个参数是数组
              // 所以之类要判断一下第二个参数是否存在
              // ret = context.fn(...arguments[1]);
              ret = context[fn](...arguments[1]);
          } else {
              // ret = context.fn();
              ret = context[fn]();
          }

          // delete context.fn;
          delete context[fn];

          return ret;
      }
   ```
## 4. bind

1. bind 函数也是改变函数的 this 指向，但是 bind 会返回一个新的函数实例，因此，我们可以在其他地方调用这个新的函数实例。

2. 实现要点：
   - 第一个参数是对象，即绑定上下文。
   - 返回值应该是原函数的拷贝。
   - 第二个以及之后的参数，是作为调用 bind 的方法的占位参数，当我们调用绑定后的函数时，我们最新传入的参数，实际上是要在bind 的第二个及后面的参数的后面传入。
   - **注意：** 一个绑定函数也能使用 new 操作符创建对象，这种行为就像把原函数当成构造器，thisArg 参数无效。也就是 new 操作符修改 this 指向的优先级更高。

3. 实现
   ```javascript
      Function.prototype.myBind = function (context) {
          const _this = this;
          // 拿到 bind 的第二个及以后的参数，作为调用 bind 的占位参数
          const args = [...arguments].slice(1);

          // 因为 bind 是返回一个函数，这个函数既可以当作普通函数，也可以当作构造函数
          // 所以，要判断一些边界条件
          function bindFn() {
             // console.log(this);

            /**
             * 当 bind 返回的函数 bindFn 作为普通函数使用的时候，bindFn 内部的 this 指向的是 window
            * 当 bind 返回的函数 bindFn 作为构造函数的时候，bindFn 内部的 this 指向的是实例
            * 如果是下面这种写法
            * return _this.apply(context, args.concat(...arguments));
            *
            * 我们在 info 函数中添加一个 message 属性，使用 myBind 绑定以后，实例化，并将 message 属性打印出来，一定是 undefined
            * 这样我们的 info 的 this 指向就丢失了，并没有指向我们的实例对象
            *
            * 也就是说，我们使用 bind 的目的是给函数的 this 指定一个新的对象（上下文），这样函数就可以借用新的对象的一些属性，实现新的功能
            * 但是同时，还要保证函数自身的功能不变，比如说，作为构造函数，实例化新的对象，这种行为就像把原函数当成构造器，context 参数无效。也就是 new 操作符修改 this 指向的优先级更高
            * 如果我们只绑定 context，那么实例化过程中，函数本身的 this 指向就会丢失，从而无法完成实例化的要求
            *
            *
            * 因此我们需要添加验证，判断当前的 this
            * 如果 this instanceof bindFn 说明这是 new 出来的实例，函数的 this 指向这个实例，这个实例可以获得调用 bind 的函数的内部的值
            *
            * 这里使用的是 apply 完成 this 的指向
            *
            */
               return _this.apply(this instanceof bindFn ? this : context, args.concat(...arguments));
           }
 
          // 新建一个函数 Fn
          function Fn() {}
          // Fn 的原型对象指向调用 bind 函数的原型
          Fn.prototype = _this.prototype;
          // 将 bindFn 的原型指向 Fn 的实例
          // 就是实现 bindFn 的原型继承调用 bind 函数的原型
          bindFn.prototype = new Fn();

          bindFn.prototype.constructor = bindFn;
          // 不能像下面这样写，因为这样如果 bindFn 的原型对象发生变化，会影响到原来的函数的原型对象
          // 所以需要使用 Fn 的实例对象进行隔离
          // bindFn.prototype = _this.prototype
          return bindFn;

      }
   ```
  
4. 总结
   - 参数拼接
   - 能作为构造函数使用，此时会忽略 context 参数，将函数的 this 指向实例，判断是不是 new 的过程，关键是：`this instanceof bindFn`，如果 `this instanceof bindFn` 说明这是 new 出来的实例，函数的 this 指向这个实例。
   - 保证绑定函数的 prototype 不丢失，这样保证 new 出来的对象的原型不是 bindFn，而是调用 bind 的函数。