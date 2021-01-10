# 1. 异步
### 1. 什么是异步，为什么会有异步
考虑的要点是：单线程模式、同步任务、异步任务和事件循环（event loop）  
JavaScript 是单线程的，也就是说，在同一时间只能执行一个任务，其他任务必须在后面排队等待。采用单线程的好处是实现起来简单，避免 DOM 渲染时的冲突，但缺点就是只要有一个任务耗时很长，后面的任务都必须排队等着，会拖延整个程序的运行。比如等待 AJAX 请求返回结果，如果对方的服务器迟迟没有响应，或者网络不通畅，就会导致脚本的长时间停滞。      
为了解决这个问题，JavaScript 语言将任务的执行模式分为两种：同步和异步。   
同步任务是在主线程上排队执行的任务，只有前一个任务执行完毕，才会执行后一个任务。异步任务是在任务队列中的任务，只有引擎认为某个异步任务可以执行了，该任务采用回调函数的形式，进入到主线程去执行。JavaScript 通过事件轮询，也就是 event loop 来确定任务队列中的异步任务能不能进入主线程执行。
### 2. 事件轮询（Event Loop）
JavaScript中，任务分为两种，同步任务和异步任务，在执行过程中进入不同的场所，同步任务进入主线程执行，而异步任务则进入Event Table并注册回调函数。  
当指定的事件触发时，Event Table将这个函数推入Event Queue。  
主线程的任务执行完毕，会进入Event Queue取出对应的函数，放到主线程去执行。  
上述过程不断执行，就是常说的事件轮询（Event Loop）。  
维基百科的定义是：“事件循环是一个程序结构，用于等待和发送消息和事件（a programming construct that waits for and dispatches events or messages in a program）”。
### 3. 异步操作的模式
  1. 回调函数  
  回调函数的优点是简单、容易理解和实现。缺点是容易出现回调地狱现象。使得程序结构混乱，复杂，难以维护。
  2. 事件监听
  异步任务的执行不取决于代码的执行，而取决于某个事件是否发生。
  3. 发布/订阅 （不了解，暂时不说）
  4. Promise
  5. async/awiat
### 4. 宏任务和微任务
异步任务可以分为两种：宏任务和微任务。  
常见的宏任务（macro-task）
    
类型|浏览器|Node
:---:|:---:|:---:
I/O|是|否
setTimeout|是|是
setInterval|是|是
setImmediate|否|是
requestAnimationFrame|是|否

常见的微任务（micro-task）

类型|浏览器|Node
:---:|:---:|:---:
process.nextTick|否|是
MutationObserver|是|否
Promise.then/catch/finally|是|是
宏任务、微任务、事件循环的关系如下图所示：
![宏任务与微任务](https://user-gold-cdn.xitu.io/2017/11/21/15fdcea13361a1ec?imageView2/0/w/1280/h/960/ignore-error/1)
（图片来自于：[这一次，彻底弄懂 JavaScript 执行机制](https://juejin.im/post/59e85eebf265da430d571f89#heading-2)）  
总结：常见的宏任务有整体代码script（代码第一次执行，会被当做宏任务）、setTimeout、setInterval。常见的微任务有Promise和Process.nextTick。在整体代码执行完毕以后，**优先**执行微任务，再去执行宏任务。在执行过程中，宏任务和微任务进入不同的事件队列（Evevt Queue）。
### 5. setTimeout()和setInterval()
  1. setTimeout()  
     - 预定的时间到了以后，把任务放入Event Queue中。但是真正执行还是要看主线程的程序是否执行完成，如果没有执行完成，那么就继续等待。有可能导致真正的延迟远大于预定的时间。  
     - setTimeout(fn, 0)的含义是：指定某个任务在主线程最早可得的空闲时间执行，意思就是不用再等多少秒了，只要主线程执行栈内的同步任务全部执行完成，栈为空就马上执行。虽然时间为0，但是这也是异步任务。
    
       ```javascript
           console.log('开始啦') ;
           setTimeout(() => {
              console.log('异步任务开始啦') ;
           })
    
           // 执行结果
           // 开始啦
           // 异步任务开始啦
       ```
    
  2. setInterval()  
     - setInterval()是循环执行某个任务。同setTimeout()类似，setInterval()每隔一段时间将注册的函数放入事件队列（Event Queue）中，如果主线程的任务耗时较长，那么同样需要等待。  
     - **注意**：`setInterval(fn,ms)`并不是每隔ms就会执行fn，而是每隔ms将注册好的fn放入事件队列（Event Queue）。如果fn执行的时间较长，有可能超过ms，那么看起来setInterval()设置的间隔根本就不是ms了。所以，使用setInterval()并不保证时间间隔一定是设定的那个时间。
  3. 使用setTimeout()模拟setInterval()  
  使用递归
  ```javascript
    var i = 0 ;
    var timer = setTimeout(function f() {
        console.log(i++) ;
        timer = setTimeout(f, 2000) ;
    }, 2000) ;
    // 封装一个函数
    function mySetInterval(func, delay) {
        var inter = function() {
            func.call(null) ;
            setTimeout(inter, delay)
        }
        // 第一次调用
        setTimeout(inter, delay) ;
    }
    
    mySetInterval(() => {
        console.log(2) ;
    }, 1000)
```
# 2. 事件
### 1. 对事件的理解  
应该问的是什么是事件吧。JavaScript与HTML之间的交互是通过事件实现的。**事件，就是文档或浏览器窗口中发生的一些特定的交互瞬间**。可以使用侦听器（或处理程序）来预订事件，以便事件发生时执行相应的代码。这种在传统软件工程中被称为观察员模式的模型，支持页面的行为（JavaScript 代码）与页面的外观（HTML 和 CSS 代码）之间的松散耦合。  
要点：
   1. JavaScript与HTML交互
   2. 文档或浏览器窗口中发生特定的交互瞬间
   3. 事件发生，执行响应的代码
### 2. 事件流
事件流分为三个阶段：  
  1. 捕获阶段  
  事件有不具体的元素向具体发生的元素逐级传播。用意在于在事件到达预定目标之前捕获它。从document开始，经过html，一直到具体发生事件的元素。
  2. 处于目标阶段  
  实际的目标元素接收到事件。
  3. 冒泡阶段  
  从事件发生的具体元素开始，向不具体的元素逐级传播，直到document。
### 3. addEventListener()  
给元素指定事件处理函数。第一个参数是事件名，第二个参数是事件处理函数，第三个是布尔值，参数如果是 true ，表示在捕获阶段调用事件处理程序；如果是 false ，表示在冒泡阶段调用事件处理程序。默认是false。
### 4. addEventListener()与onclick的区别  
addEventListener()可以向一个事件绑定多个处理函数，事件发生后，依次调用。onclick只能绑定一个处理函数，多次绑定，后面的会将前面的覆盖。
### 5. 移除绑定的事件
使用removeEventListener()。移除不能使用匿名函数。
# 3. ajax
### 1. XMLHttpRequest
ajax的核心是XLMHttpRequest对象。这个对象具有下面几个属性：
  1. responseText  
  响应主体返回的报文
  2. status
  响应的http状态
  3. statusText
  http状态说明
  4. readyState
  请求/响应过程的当前活动阶段，值为：0，1，2，3，4
### 2. 状态
readyState一共五个状态，说明如下：
  1. 0  
  未初始化，尚未调用open()
  2. 1
  启动，已经调用open()，但是未调用send()
  3. 2  
  发送，已经调用send，但是没有接受到响应
  4. 3  
  接收，已经接收到部分响应
  5. 4  
  完成，已经接收到全部响应
### 3. 代码
  ```javascript
     var url = 'sdfa.com' ;
     var xhr = new XMLHttpRequest() ;
     xhr.onreadystatechange = function() {
        if (xhr.readyState === 4) {
            if (xhr.status === 200) {
                console.log(xhr.responseText) ;
            } else {
                console.log('访问出错') ;
            }
        }
     }
     xhr.open('GET', url) ;
     // 如果发送的是post请求，可以在这里传入数据
     xhr.send() ;
  ```
### 4. ajax缺点
1. Ajax干掉了Back与History功能，即对浏览器机制的破坏  
　　在动态更新页面的情况下，用户无法回到前一页的页面状态，因为浏览器仅能记忆历史纪录中的静态页面
2. 安全问题  
　　AJAX技术给用户带来很好的用户体验的同时也对IT企业带来了新的安全威胁，Ajax技术就如同对企业数据建立了一个直接通道。这使得开发者在不经意间会暴露比以前更多的数据和服务器逻辑。
3. 对搜索引擎支持较弱  
4. 破坏程序的异常处理机制 
5. 违背URL与资源定位的初衷
6. 不能很好地支持移动设备
7. 客户端肥大，太多客户段代码造成开发上的成本
### 5. onreadystatechange与onload的区别
1. 在一些较旧的浏览器不支持onload事件，只支持onreadystatechange事件。onreadystatechange事件的兼容性更好。
2. onreadystatechange事件的定义是只要返回的状态码只要变化时就调用一次函数，可以输出的状态码是2、3、4。而onload只有状态码为4时才能调用一次函数。
3. 新版浏览器可以直接使用onload事件，保证兼容性，使用onreadystatechange。
# 4. JSON 
序列化（serialization），指的是将内存中的对象的信息转换为可以传输或者存储的形式。在JavaScript中，使用JSON作为序列化后的存储格式。  
### 1. 序列化
将对象变为二进制形式  
使用的方法：`JSON.stringify()`  
### 2. 反序列化
将二进制变为对象  
使用的方法：`JSON.parse()` 
# 5. cookie和session
### 1. cookie
- 保存在浏览器端，记录了一些用户操作信息（包括用户登录信息）。
- 大小限制  
单个cookie保存的数据不能超过4kb。每个域可保存的cookie数量和浏览器相关。
- 应用场景  
  1. 判断用户是否登陆过网站，以便下次登录时能够实现自动登录（或者记住密码）。如果我们删除cookie，则每次登录必须从新填写登录的相关信息。
   2. 保存上次登录的时间等信息。
   3. 保存上次查看的页面
   4. 浏览计数
- 缺点
  1. 大小受限。
  2. 用户可以操作（禁用）cookie，使功能受限。
  3. 安全性较低。
  4. 有些状态不可能保存在客户端。
  5. 每次访问都要传送cookie给服务器，浪费带宽。
  6. cookie数据有路径（path）的概念，可以限制cookie只属于某个路径下。
### 2.  session
- 当服务器收到请求需要创建session对象时，首先会检查客户端请求中是否包含sessionid。如果有sessionid，服务器将根据该id返回对应session对象。如果客户端请求中没有sessionid，服务器会创建新的session对象，并把sessionid在本次响应中返回给客户端。通常使用cookie方式存储sessionid到客户端，在交互中浏览器按照规则将sessionid发送给服务器。如果用户禁用cookie，则要使用URL重写，可以通过response.encodeURL(url) 进行实现；API对encodeURL的约束为，当浏览器支持Cookie时，url不做任何处理；当浏览器不支持Cookie的时候，将会重写URL将SessionID拼接到访问地址后。
- session通过类似与Hashtable的数据结构来保存，能支持任何类型的对象。
- session没有任何大小限制。
- 应用场景
  Session用于保存每个用户的专用信息，变量的值保存在服务器端，通过SessionID来区分不同的客户。
  1. 网上商城中的购物车。
  2. 保存用户登录信息。
  3. 将某些数据放入session中，供同一用户的不同页面使用。
  4. 防止用户非法登录。
- 缺点
  1. Session保存的东西越多，就越占用服务器内存，对于用户在线人数较多的网站，服务器的内存压力会比较大。
  2. 依赖于cookie（sessionID保存在cookie），如果禁用cookie，则要使用URL重写，不安全。
  3. 创建Session变量有很大的随意性，可随时调用，不需要开发者做精确地处理，所以，过度使用session变量将会导致代码不可读而且不好维护。
# 6. localStorage
- 是html5新增的API。目的是解决cookies存储容量小（只有4k）的问题。localStorage主要是用来作为本地存储的；localStorage 中一般浏览器支持的容量大小是5M，针对不同的浏览器，localStorage容量大小会有所不同。  
- 同cookies相比，localStorage并且不会随着HTTP传输，这样可以节约带宽。
- 提供一种在cookie之外存储会话数据的路径。
- 提供一种存储大量可以跨会话存在的数据的机制。
- 应用场景：  
常用于长期登录（+判断用户是否已登录），适合长期保存在本地的数据。    
注意：localStorage存储的只能是字符串。
常用API：
  1. setItem(name, value)  
  为指定的 name 设置一个对应的值
  2. getItem(name)  
  根据指定的名字 name 获取对应的值
  3. key(index)  
  获得 index 位置处的值的名字
  4. removeItem(name)  
  删除由 name 指定的名值对儿
  5. clear()  
  清除所有的键值对
# 7. sessionStorage
- sessionStorage也是html5新增加的web storage。也是本地存储，容量一般是5M。但是与localStorage不同的是，sessionStorage并不会持久化保存数据，而是仅仅维持一个会话时间。也就是该数据只保持到浏览器关闭、或页面关闭。存储在sessionStorage中的数据可以跨越页面刷新而存在。
- sessionStorage的清除时机是在会话结束，会话结束是在用户关闭标签页或者关闭窗口的时候；手动新开一个标签或窗口时，会新开会话，即使链接一样，也不会共享sessionStorage。
- API与localStorage类似。
- 应用场景  
  敏感数据的一次登录。如网上银行的登录。
# 8. localStorage和sessionStorage共同的特点
- localStorage和sessionStorage都受到浏览器同源策略的限制。不同源的页面无法访问localStorage和sessionStorage。
- 优点
  1. 存储空间更大：cookie为4KB，而WebStorage是5MB。
  2. 节省网络流量：WebStorage不会传送到服务器，存储在本地的数据可以直接获取，也不会像cookie一样美词请求都会传送到服务器，所以减少了客户端和服务器端的交互，节省了网络流量。
  3. 对于那种只需要在用户浏览一组页面期间保存而关闭浏览器后就可以丢弃的数据，sessionStorage会非常方便。
  4. 快速显示：有的数据存储在WebStorage上，再加上浏览器本身的缓存。获取数据时可以从本地获取会比从服务器端获取快得多，所以速度更快。
  5. 安全性：WebStorage不会随着HTTP header发送到服务器端，所以安全性相对于cookie来说比较高一些，不会担心截获，但是仍然存在伪造问题；
  6. WebStorage提供了一些方法，数据操作比cookie方便。
# 9. cookie、localStorage以及sessionStorage跨域问题的解决
### 1. cookie
- 在服务器端的响应头中的Set-Cookie字段，设置值domain为一级域名，则相应的二级域名可以访问次cookie。例如：`Set-Cookie: 'id=123;domain=baidu.com'`。
- 两个网页一级域名相同，只是二级域名不同，浏览器允许通过设置document.domain共享 Cookie。
### 2. localStorage和sessionStorage
- 通过html5新增api——postMessage实现跨域。
- **设置document.domain属性不能实现跨域。**
#10. localSorage存储满了，怎么办？
- 跨域请求其他域下的存储空间。
# 11. 闭包
# 12. 跨域

