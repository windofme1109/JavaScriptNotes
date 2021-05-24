
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

js中函数是如何调用的

说下原型和继承

说下浏览器事件循环

js是单线程还是多线程，为什么这么设计

## 类数组怎么转换成数组

1. 扩展运算符：`...`

2. Array.prototype.slice.call()

3. for 循环

## new Array()接收的参数是什么

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
   - 使用create创建的对象，没有任何属性，显示No properties，我们可以把它当作一个非常纯净的map来使用，我们可以自己定义hasOwnProperty、toString方法，不管是有意还是不小心，我们完全不必担心会将原型链上的同名方法覆盖掉。
   - 在我们使用for…in循环的时候会遍历对象原型链上的属性，使用create(null)就不必再对属性进行检查了，也可以使用Object.keys[]

4. 什么时候用Object.create(null)
   - 你需要一个非常干净且高度可定制的对象当做数据字典的时候
   - 减少hasOwnProperty造成的性能损失并且可以偷懒少些一点代码的时候
   - 其他的时候，请用{} 

## 如何区分函数是 new 调用还是直接调用

1. 通过 instanceof 判断


防抖节流区分,手写

实现 map,reduce

统计字符串中次数最多字母




实现一个数组扁平化方法 flat

实现数组扁平化函数 flat


手写promise(写完then后面试官说可以了)

说一下js中的类和 java 中的类的区别

原生js怎么实现拖放

平时对js和css基础有过了解吗

axios源码整体架构

手写Promise.all

Promise中用了什么设计模式

Promise 都有哪些状态


说下jwt
知道jwt有哪些缺点吗



知道内存碎片怎么产生的吗 v8



说一下锁(懵逼),举例是读锁和写锁

为什么要去看axios的源码,大体实现

this指向
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


算法题1:节点渲染

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

算法题2:属性获取

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

说一下进程线程,如何通信?

两个线程可以直接通信吗

业务中的安全问题有没有遇到过?怎么解决的?(说了base64,cors,xss,csrf,cookie的httponly和samesite属性)



react中遇到的坑，怎么解决的

大数据量场景前端怎么处理,让页面展示尽可能流畅

前端缩略图(截图)方案是什么

业务中的实时保存会有性能开销,又没有做什么优化？



js为什么设计成单线程

事件循环说一下

Promise里都是微任务吗

new Promise返回的实例和实例then方法执行后返回的promise是一个吗


git reset 和git rebase了解吗





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


js为什么会有回调地狱呢

为什么 java 没有回调地狱

promise.all返回的是什么

promise和async你觉得差异点是什么



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

聊一下代码检查(eslint,ts)



解释下下面两段代码执行结果的差异

function foo() {
    foo();
}
function foo() {
    setTimeout(() => {
        foo();
    }, 0)
}
requestAnimationFrame 和 requestIdleCallback了解吗

node中加载相同模块,会重复打印吗

//a.js 
function foo() {
    console.log('foo'); // 
 }
foo();
//b.js
require('./a.js');
require('./a.js');
//node b.js

如何设计这套缓存

实现一个Map

对技术栈或新技术上是怎样的一个态度

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



