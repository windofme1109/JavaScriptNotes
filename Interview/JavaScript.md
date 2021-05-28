
## 说一下 js 中如何实现函数重载

1. 函数重载指的是函数名相同，但是参数不同（个数、类型）的几个函数。调用时根据参数来决定调用哪个函数。

2. js 中没有真正的函数重载，后面定义的同名函数会覆盖前面定义的函数。

3. 可以使用 arguments 根据参数的个数来模拟函数重载。

## 实现一下 es6 的 extends

1. 通过组合继承的方式实现继承。

2. 组合继承地缺点是调用了两次构造函数，同时子类原型还有父类的实例属性。

3. 示例代码：
   ```js
      function SuperType(name) {
        this.name = name;
      }
      superType.prototype.getName = function() {
        return this.name
      }
      function SubType(name, age) {
          SuperType.call(this, name);
          this.age = age;
      }

      SubType.prototype = new SuperType();

      SubType.prototype.getAge = function() {}
   ```

4. 改进版
   ```js
      function inherit(SuperType, SubType) {
        const prototype = Object(SuperType.prototype);
        prototype.constructor = SubType;
        SubType.prototype = prototype;
      }
   ```
   使用 inherit() 实现继承，避免再次调用父类的构造函数。

有没有接触过移动端,小程序,h5等方向

## 写个继承案例

## js数据类型和存储方式,如何判断

1. js 的数据类型可以分为一下几种：
   - 基本数据类型：Number、String 和 Boolean，基本数据类型存储在栈内存中。
   - 引用数据类型，就是对象，包括数组、函数、对象等，在栈内存中存储的是一个指针，指向存储在堆内存中的具体的值。
   - 特殊类型：undefined 和 null，也是存储在栈内存中。
   - ES6 新增的 symbol，表示独一无二的值。

2. 判断方式
   - 基本数据类型使用 typeof，但是注意：对 null 使用 typeof，返回的是 object。
   - 对引用数据类型使用 typeof 返回 object。如果是函数类型，返回的是 function。
   - 对象可以使用 instanceof 来判断
   - Object.prototype.toString.call()

## 说下js中数据存储方式

## typeof [] 返回什么

1. 返回 object

## new 操作符 做了什么

1. 文字描述：
   1. 新建一个空对象
   2. 将空对象的 `__proto__` 指向构造函数的 `prototype`
   3. 将构造函数的 this 指向 当前的对象，并执行构造函数内部的代码（给空对象添加属性）
   4. 将这个空对象赋给一个变量

5. 代码实现
   ```js
      function newClass(className) {
          let obj = Object.create(className.prototype);
          let params = Array.prototype.slice.call(arguments).slice(1);
          let ret = className.apply(obj, params);
          
          return typeof ret === 'objcet' ? ret : obj;
      }
   ```

## 实现一个new操作符

## 数组中一万个数据,访问第一个和最后一个效率会有什么差异,为什么

1. 效率是一样的，因为数组是线性表，读取操作都是 O(1)。


## forEach for in for of 的差异

1. [总结一下JavaScript 中的 for 循环](https://www.cnblogs.com/lynn-z/p/13068866.html)

1. forEach 
   - 单纯的对数组进行遍历，作用类似于 for 循环。
   - 接收一个函数，函数的第一个参数是元素，第二个参数是元素的索引。
   - forEach 没有返回值。 
   - 不能使用 break。

2. for in 
  - 用于对象的遍历。获得的是对象的属性，根据这个属性就可以获得属性值。
  - 因为数组也是对象，因此 for in 也可以用于数组的遍历。
  - for in 遍历数组存在的问题：
    - for in 只遍历可枚举的属性，因此对于数组的 length 属性等不可枚举的属性无法被遍历到。
    - for in 可以遍历 array 原型链上的所有可枚举的属性。即不仅遍历数组上的元素，还可能遍历我们添加的自定义元素。
    - for in 遍历的顺序可能不确定。
  - for in 适合遍历普通对象，不适用于数组。

3. for of
   - ES6 最新引入的遍历方式。
   - for of 直接拿到的是数组的元素，不是索引。
   - 可以相应 break、continue
   - 不仅支持数组，还支持绝大多数类似数组对象，如 DOM nodelist 对象。
   - 支持字符串遍历。
   - 支持 Map 和 Set 对象的遍历。
   - 不支持普通对象的遍历。

## js中遍历数组的方式

## 如何改变 this 指向

1. 使用 call、apply 或者 bind。

2. call 与 apply 的相同与不同
   1. call 与 apply 的第一个参数都是要调用 call 或 apply 的函数的所要指向的 this 对象。
   2. 调用 call 或者 apply 相当于执行函数。
   3. call 接收函数的参数的形式是逗号分隔的参数列表，从第二个参数开始。
   4. apply 的第二个参数是数组，接收函数的参数是放在这个数组中。

3. bind 返回一个新的函数实例。

4. bind 的第一个参数是要绑定的 this 对象。第二个以及后面的参数可以是调用 bind 的函数的参数，被称为占位参数，即将调用 bind 的函数的参数固定下来，然后调用新的函数实例时，直接传入后面的参数即可。

5. 手写 call、apply 和 bind。

## js 中函数是如何调用的

## 说下原型和继承

## 说下浏览器事件循环

## js是单线程还是多线程，为什么这么设计

## 类数组怎么转换成数组

1. 扩展运算符：`...`

2. Array.prototype.slice.call()

3. for 循环

## new Array() 接收的参数是什么

1. 如果只有一个参数且为数字，这个参数表示的是数组的长度。数组元素是 empty。 

2. 多余一个参数或者这一个参数不是数字，会根据这些参数创建一个 JavaScript 数组。



## Object.create 实现

1. 代码：
   ```js
      function create(obj) {
        function F() {}
        F.prototype = obj;

        return new F();
      }
   ```

## 实现 Object.create

## Object.create 传 null 和 {} 有啥区别吗

1. 传入 null。返回的是空对象，这个对象不会继承 Object 原型链上的属性，如 tostring() 方法这些。

2. 传入 {}。返回的也是是空对象，这个对象会继承 Object 原型链上的属性，如 tostring() 方法这些。

3. 为什么用Object.create(null)
   - 使用 create() 创建的对象，没有任何属性，显示 No properties，我们可以把它当作一个非常纯净的map来使用，我们可以自己定义 hasOwnProperty、toString 方法，不管是有意还是不小心，我们完全不必担心会将原型链上的同名方法覆盖掉。
   - 在我们使用 for…in 循环的时候会遍历对象原型链上的属性，使用 create(null) 就不必再对属性进行检查了，也可以使用Object.keys()。

4. 什么时候用 Object.create(null)
   - 你需要一个非常干净且高度可定制的对象当做数据字典的时候。
   - 减少 hasOwnProperty 造成的性能损失并且可以偷懒少些一点代码的时候。
   - 其他的时候，请用 {}。

## 如何区分函数是 new 调用还是直接调用

1. 通过 instanceof 判断


## 防抖节流区分,手写

1. 防抖
   - 事件触发以后，开始计时，如果事件继续被触发，则清空计时器，重新开始计时，直到计时间隔内没有事件被触发，则执行事件处理函数。
   - 代码：
     ```js
        function debounce(fn, delay = 300) {
            let timer;

            return function () {
                const context = this;
                const args = arguments;

                if (timer) {
                    clearTimeout(timer);
                }

                timer = setTimeout(function () {
                    fn.apply(context, args);
                }, delay);
            }
        }
     ```
2. 节流
   - 事件触发后，开始计时，在计时这段事件内，即使事件被触发，也不再响应，计时时间到，执行事件处理函数，同时清空定时器。
   - 代码：
     ```js
        function throttle(fn, delay = 300) {
            let timer;
            return function () {
                const context = this;
                const args = arguments;
                if (!timer) {
                    timer = setTimeout(function () {
                        timer = null;
                        fn.apply(context, args);
                    }, delay);
                }
            }
        }
     ```

## 实现 map,reduce

1. map
   ```javascript
      /**
       *
       * @param callback
       * @returns {[]}
       */
      Array.prototype.myMap = function (callback) {
          let ret = [];
      
          for (let i = 0; i < this.length; i++) {
              let temp = this[i];
              ret.push(callback(temp, i));
          }
      
          return ret;
      }
      
      let arr = [1, 2, 3];
      
      let ret = arr.myMap((item, index) => item * 2);
      
      console.log(ret);
   ```

2. reduce 
   ```javascript
      /**
       *
       * @param callback
       * @param initialVal
       * @returns {*}
       */
      Array.prototype.myReduce = function (callback, initialVal) {
      
          // 存放累计结果，即 执行完 callback 的返回值
          let accumulator = undefined;
      
          if (initialVal !== undefined) {
              accumulator = initialVal;
          } else {
              accumulator = this[0];
          }
      
          if (initialVal !== undefined) {
              for (let i = 0; i < this.length; i++) {
                  accumulator = callback(accumulator, this[i], i);
              }
          } else {
              for (let i = 1; i < this.length; i++) {
                  accumulator = callback(accumulator, this[i], i);
              }
          }
      
          return accumulator;
      
      }
      
      let arr = [1, 2, 3, 4];
      
      console.log(arr.myReduce((prev, cur) => prev + cur));
   ```

## 统计字符串中次数最多字母

1. 思路
   - 切分字符串
   - 使用 reduce 分组，同级每个字母出现的次数
   - for 循环找到出现次数最多的字母

2. 代码：
   ```js
      function maxCount(str) {

          // 统计每个字符出现的次数
          let ret = str.split('').reduce((acc, ele) => {
              if(acc[ele]) {
                  acc[ele]++;
              } else {
                  acc[ele] = 1;
              }
              return acc;
          }, {});

          let maxCount = 0;
          let maxCountLetter = '';

          for (let key of Object.keys(ret)) {
             let count = ret[key];
             if (count > maxCount) {
              maxCount = count;
              maxCountLetter = key;
             }
          }

       return maxCountLetter;

      }
   ``` 



## 实现一个数组扁平化方法 flat

1. 递归版
   ```javascript
      function flatten(arr) {
          let ret = [];
      
          for (let key of arr) {
      
              if (Array.isArray(key)) {
                  ret = ret.concat(flatten(key));
              } else {
                  ret.push(key)
              }
          }
      
          return ret;
      }
      const ret1 = flatten([['a'], ['b', 'd', ['e'], 'f'], ['c', [[['l']]]],'g', 'r', 'w']);
      
      console.log(ret1)
      
   ```
2. reduce 版本
   ```javascript
      /**
        * 2. reduce 版本
        * @param arr
        * @returns {*}
        */
        function flatten(arr) {
            return arr.reduce((prev, cur) => {
                return prev.concat(Array.isArray(cur) ? flatten(cur) : cur);
            }, []);
        }
   ```
## 实现数组扁平化函数 flat

## Promise 都有哪些状态

1. pending

2. fulfill

3. reject

## 手写 promise(写完then后面试官说可以了)

## Promise 中用了什么设计模式

1. 观察者模式

## 手写 Promise.all

## promise.all 返回的是什么

1. 返回值是一个 Promise。

2. all 方法接收一个由多个 Promise 组成的数组，只有当所有的 Promise 都返回成功的状态，all 方法才返回成功状态的 Promise，只要其中有一个失败，就返回这个失败的 Promise。

## promise.race 

1. 返回值是一个 Promise。

2. race 方法接收一个由多个 Promise 组成的数组，只要有一个 Promise 返回结果，无论是成功还是失败，race 就返回这个 Promise 的结果。取决于最快的哪个 Promise 结果。

## Promise 里都是微任务吗

1. then / catch / finally 都是微任务

## new Promise 返回的实例和实例 then 方法执行后返回的 promise 是一个吗

1. 不是一个，then 方法返回的是新的 Promise

## promise 和 async 你觉得差异点是什么

## js 为什么设计成单线程

1. 这主要和 js 的用途有关，js 主要用于浏览器，实现用户和浏览器的交互，比如操作DOM，发送 ajax 请求等，这样的需求决定浏览器只能是单线程，如果是多线程，会带来复杂的同步问题。如果 js 是多线程，一个线程要修改 DOM，另外一个线程需要删除 DOM，那么浏览器完全不知所措。所以，现实的需求以及实现的简单性决定 js 是单线程的。

## 事件循环说一下

1. js 的任务分为两种，同步任务和异步任务，其中同步任务进入主线程执行，异步任务进入任务队列。

2. 对于异步任务，首先其进入 event table 注册回调函数，然后等待异步任务被触发。当异步任务被触发以后，将回调函数放入 event queue 中，等待进入 js 主线程执行。当主线程的任务全部执行完成后，去 event queue 中取出任务放到主线程执行。上述过程不断循环执行，就是 event loop。

## js 为什么会有回调地狱呢

1. 因为 js 是单线程的，所以将任务分为同步任务和异步任务，异步任务依赖回调函数。

2. 当一个事件被触发或任务完成时，回调函数此时会执行，同时接收这次事件或者任务的结果。此时如果有别的的异步任务依赖此次异步任务的结果，那么只能将别的异步任务放入回调中，这样就形成了回调函数的嵌套。

3. 如果很多异步任务都依赖前一个异步任务的结果，那么就会形成多个回调函数的嵌套，因此就形成了回调地狱。

4. 回调地狱的问题：代码可读性低、编写费劲、容易出错

5. 解决方法：
   - Promise
   - async / await

## 为什么 java 没有回调地狱

1. java 一般用于服务端，服务端主要是 I/O 操作非常多，比如读取数据库、读取文件等，这些任务适合使用同步模式，也不会造成阻塞，因此，java 不会出现回调地狱。

## 说一下 js 中的类和 java 中的类的区别

1. js 没有真正的类，所谓的类，使用构造函数模拟出来。

2. ES6 引入了 class 关键字，可以实现类似于 java 中类的功能，这个 class 只是语法糖，并不是真正的 class。


## 说一下进程线程，如何通信?

1. 进程
   - 资源分配的最小的单位
   - 它代表CPU 所能处理的单个任务。任一时刻，CPU 总是运行一个进程，其他进程处于非运行状态。
   - 如果把 CPU 比作一个工厂，那么进程就是其中的一个车间，只不过这个工厂电力有限，一次只能一个车间开动。
   - 不同的线程之间数据很难共享。
   - 进程之间不会相互影响。
2. 线程
   - CPU 调度的最小单位。
   - 还是以车间为例，车间里面有很多工人，他们一起协作，完成一个任务。线程就类似于车间里面的工人，一个进程下包括多个线程，多个线程一起完成一个任务。
   - 进程之间可以共享数据，共享内存空间。
   - 一个线程挂掉会导致整个进程崩溃。

3. 

## 两个线程可以直接通信吗

1. 可以直接通信

原生 js 怎么实现拖放

平时对js和css基础有过了解吗

axios源码整体架构

说下jwt
知道jwt有哪些缺点吗

知道内存碎片怎么产生的吗 v8

说一下锁(懵逼),举例是读锁和写锁

为什么要去看axios的源码,大体实现

业务中的安全问题有没有遇到过?怎么解决的?(说了base64,cors,xss,csrf,cookie的httponly和samesite属性)

react中遇到的坑，怎么解决的

大数据量场景前端怎么处理,让页面展示尽可能流畅

前端缩略图(截图)方案是什么

业务中的实时保存会有性能开销,又没有做什么优化？

如何设计这套缓存

实现一个Map

对技术栈或新技术上是怎样的一个态度
聊一下代码检查(eslint,ts)

git reset 和git rebase了解吗

requestAnimationFrame 和 requestIdleCallback了解吗

## this指向
```js
   var fullname = '1';
   var obj = {
       fullname: '2',
       prop: {
           fullname: '3',
           //如果改成普通函数呢？
           getFullname:()=> {
              return this.fullname;
           }
        }
   };
  console.log(obj.prop.getFullname());
  var test = obj.prop.getFullname;
  console.log(test());
```


## 算法题1:节点渲染
```js
   const el = require('./element.js')；
   const ul = el('ul', {id: 'list'}, [
     el('li', {class: 'item'}, ['Item 1']),
     el('li', {class: 'item'}, ['Item 2']),
     el('li', {class: 'item'}, ['Item 3'])
   ])
   const ulRoot = ul.render();
   document.body.appendChild(ulRoot);

   <ul id='list'>
     <li class='item'>Item 1</li>
     <li class='item'>Item 2</li>
     <li class='item'>Item 3</li>
   </ul>

```

算法题2:属性获取
```js
   {
    a:{
      b:{
        c:{f:"aa"}
      },
      d:{
        e:{g:"bb"},
        h:{i:"cc"}
      },
      j:{
        k:"dd"
      }
    }
}
// [f,g,i,c,e,h,k,b,d,j,a]
```



手写一个Scheduler类,实现并发控制
//JS实现一个带并发限制的异步调度器Scheduler,
//保证同时运行的任务最多有两个。
//完善代码中Scheduler类,使得以下程序能正确输出：
//Scheduler内部可以写其他的方法
class Scheduler {
  add(promiseCreator) { ... }

  // ...
}

const timeout = (time) => new Promise(resolve => {
  setTimeout(resolve, time)
})

const scheduler = new Scheduler()
const addTask = (time, order) => {
  scheduler.add(() => timeout(time))
    .then(() => console.log(order))
}

addTask(1000, '1')
addTask(500, '2')
addTask(300, '3')
addTask(400, '4')
// output: 2 3 1 4

// 一开始,1、2两个任务进入队列
// 500ms时,2完成,输出2,任务3进队
// 800ms时,3完成,输出3,任务4进队
// 1000ms时,1完成,输出1
// 1200ms时,4完成,输出4


打印题

const o1 = {
 text: 'o1',
  fn: function() { 
   return this.text
  }
}

const o2 = {
 text: 'o2',
  fn: function() {
   return o1.fn()
  }
}

const o3 = {
 text: 'o3',
  fn: function() {
   var fn = o1.fn
    return fn()
  }
}
console.log(o1.fn())
console.log(o2.fn())
console.log(o3.fn())



打印题

var a = 20;
var test = {
  a: 40,
  init: () => {
    console.log(this.a);
    function go() {
      console.log(this.a);
    }
    go.prototype.a = 50;
    return go;
  }
};

var p = test.init();
p();
new p()





解释下下面两段代码执行结果的差异

function foo() {
    foo();
}
function foo() {
    setTimeout(() => {
        foo();
    }, 0)
}




//a.js 
function foo() {
    console.log('foo'); // 
 }
foo();
//b.js
require('./a.js');
require('./a.js');
//node b.js



事件循环的打印题(比较基础)

setTimeout(() => {
    console.log(1)
}, 0)
new Promise((resolve) => {
    console.log(2)
    for (let i = 0; i < 10000; i++) {
        if (i === 9999) { resolve() }
    }
    console.log(3)
}).then(() => {
    console.log(4)
})
console.log(5)



