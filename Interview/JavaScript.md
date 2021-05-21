说一下js中的类和java中的类的区别

原生js怎么实现拖放

平时对js和css基础有过了解吗

说一下js中如何实现函数重载

实现一下es6的extends

有没有接触过移动端,小程序,h5等方向

js数据类型和存储方式,如何判断

## typeof [] 返回什么

1. 返回 object

new 操作符 做了什么

数组中一万个数据,访问第一个和最后一个效率会有什么差异,为什么

forEach for in for of 的差异

如何改变 this 指向

js中函数是如何调用的

说下原型和继承

说下浏览器事件循环

js是单线程还是多线程,为什么这么设计

类数组怎么转换成数组

new Array()接收的参数是什么

axios源码整体架构

手写Promise.all

Promise中用了什么设计模式

Promise都有哪些状态


说下jwt

说下js中数据存储方式

知道内存碎片怎么产生的吗 v8

知道jwt有哪些缺点吗

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

js中遍历数组的方式

react中遇到的坑,怎么解决的

大数据量场景前端怎么处理,让页面展示尽可能流畅

前端缩略图(截图)方案是什么

业务中的实时保存会有性能开销,又没有做什么优化？

写个继承案例

Object.create实现

Object.create传null和{} 有啥区别吗

手写promise(写完then后面试官说可以了)

实现一个数组扁平化方法flat

js为什么设计成单线程

事件循环说一下

Promise里都是微任务吗

new Promise返回的实例和实例then方法执行后返回的promise是一个吗

实现一个new操作符

git reset 和git rebase了解吗

实现Object.create

实现数组扁平化函数flat

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

为什么java没有回调地狱

promise.all返回的是什么

promise和async你觉得差异点是什么

算法1:rgb转16进制函数

算法2:

//实现一个retry函数
//如果fn返回成功,则打印一下,最终结果成功
//如果fn返回失败,则打印times下,最终结果失败
retry(fn,times)
retry(() => {
  console.log('doing')
  return Promise.reject(Error('done'))
}, 3)

retry(() => {
  console.log('doing')
  return Promise.resolve('done')
}, 3)

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

防抖节流区分,手写

实现map,reduce

统计字符串中次数最多字母

如何区分函数是new调用还是直接调用

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



