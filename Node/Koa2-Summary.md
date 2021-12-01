<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Koa2 基本用法总结](#koa2-%E5%9F%BA%E6%9C%AC%E7%94%A8%E6%B3%95%E6%80%BB%E7%BB%93)
  - [1.  相关资料](#1--%E7%9B%B8%E5%85%B3%E8%B5%84%E6%96%99)
  - [2. Koa2 的基本使用](#2-koa2-%E7%9A%84%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8)
  - [3. Koa2 的路由](#3-koa2-%E7%9A%84%E8%B7%AF%E7%94%B1)
    - [1. `@koa/router` 的基本使用](#1-koarouter-%E7%9A%84%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8)
    - [2. 获取查询字符串](#2-%E8%8E%B7%E5%8F%96%E6%9F%A5%E8%AF%A2%E5%AD%97%E7%AC%A6%E4%B8%B2)
    - [3. 动态路由](#3-%E5%8A%A8%E6%80%81%E8%B7%AF%E7%94%B1)
  - [4. Koa2 的中间件](#4-koa2-%E7%9A%84%E4%B8%AD%E9%97%B4%E4%BB%B6)
    - [1. 中间件的基本说明](#1-%E4%B8%AD%E9%97%B4%E4%BB%B6%E7%9A%84%E5%9F%BA%E6%9C%AC%E8%AF%B4%E6%98%8E)
    - [2. 应用级中间件](#2-%E5%BA%94%E7%94%A8%E7%BA%A7%E4%B8%AD%E9%97%B4%E4%BB%B6)
    - [4. 错误处理中间件](#4-%E9%94%99%E8%AF%AF%E5%A4%84%E7%90%86%E4%B8%AD%E9%97%B4%E4%BB%B6)
    - [5. 第三方中间件](#5-%E7%AC%AC%E4%B8%89%E6%96%B9%E4%B8%AD%E9%97%B4%E4%BB%B6)
  - [5. Koa2 处理 post 请求](#5-koa2-%E5%A4%84%E7%90%86-post-%E8%AF%B7%E6%B1%82)
    - [1. 原生方式处理 post 请求](#1-%E5%8E%9F%E7%94%9F%E6%96%B9%E5%BC%8F%E5%A4%84%E7%90%86-post-%E8%AF%B7%E6%B1%82)
    - [2. 使用 `koa-bodyparser` 处理](#2-%E4%BD%BF%E7%94%A8-koa-bodyparser-%E5%A4%84%E7%90%86)
  - [6. Koa2 处理静态资源](#6-koa2-%E5%A4%84%E7%90%86%E9%9D%99%E6%80%81%E8%B5%84%E6%BA%90)
  - [7. Koa2 处理 Cookies](#7-koa2-%E5%A4%84%E7%90%86-cookies)
  - [8. Koa2 中间件的开发](#8-koa2-%E4%B8%AD%E9%97%B4%E4%BB%B6%E7%9A%84%E5%BC%80%E5%8F%91)
    - [1. 基本中间件开发](#1-%E5%9F%BA%E6%9C%AC%E4%B8%AD%E9%97%B4%E4%BB%B6%E5%BC%80%E5%8F%91)
    - [2. 中间件最佳实践 - 中间件配置项](#2-%E4%B8%AD%E9%97%B4%E4%BB%B6%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5---%E4%B8%AD%E9%97%B4%E4%BB%B6%E9%85%8D%E7%BD%AE%E9%A1%B9)
    - [3. 中间件最佳实践 - 命名的中间件](#3-%E4%B8%AD%E9%97%B4%E4%BB%B6%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5---%E5%91%BD%E5%90%8D%E7%9A%84%E4%B8%AD%E9%97%B4%E4%BB%B6)
    - [4. 中间件最佳实践 - 组合多个中间件](#4-%E4%B8%AD%E9%97%B4%E4%BB%B6%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5---%E7%BB%84%E5%90%88%E5%A4%9A%E4%B8%AA%E4%B8%AD%E9%97%B4%E4%BB%B6)
    - [5. 中间件最佳实践 - 响应中间件](#5-%E4%B8%AD%E9%97%B4%E4%BB%B6%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5---%E5%93%8D%E5%BA%94%E4%B8%AD%E9%97%B4%E4%BB%B6)
  - [9. Koa2 处理 Session](#9-koa2-%E5%A4%84%E7%90%86-session)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Koa2 基本用法总结
 
## 1.  相关资料

1. [官方教程 - guide](https://github.com/koajs/koa/blob/master/docs/guide.md)
   
2. [官方 github 地址](https://github.com/koajs)

3. [koa2 中间件列表](https://github.com/koajs/koa/wiki#middleware)

4. [koa2 官方示例](https://github.com/koajs/examples)

5. [koa 官网 - 英文](https://koajs.com)

6. [Koa2教程（入门篇）](https://www.jianshu.com/p/5ba990127978)

7. [KOA2框架原理解析和实现](https://juejin.cn/post/6844903709592256525)

8. [《koa2 进阶学习笔记》附教程 demo](https://juejin.cn/post/6844903464586182663)

9. [深入浅出 Koa](https://juejin.cn/post/6844903439843983373)

10. [带你走进 koa2 的世界（koa2 源码浅谈）](https://juejin.cn/post/6844903477798240264)

11. [Koa源码剖析笔记(一) - 主体流程、koa-compose洋葱模型](https://juejin.cn/post/6998677014722904101)

12. [koa实践总结，总有你用的到抄的走的](https://juejin.cn/post/6952665400890884127)

13. [《Koa2进阶学习笔记》已完结](https://chenshenhai.github.io/koa2-note/)

## 2. Koa2 的基本使用

1. Koa2 需要 v7.6.0 及其更高版本的 Node.js，在这个版本及以上的 node 支持 ES 2015 以及 async/await 函数。

2. 安装 Koa2：`npm install koa --save`

3. 使用 koa 编写 web 应用，可以免除重复繁琐的回调函数嵌套，并极大地提升错误处理的效率。koa 不在内核方法中绑定任何中间件，它仅仅提供了一个轻量优雅的函数库，使得编写 Web 应用变得得心应手。最大的特点就是可以避免异步嵌套。

4. Koa2 使用中间件机制，本身只提供基础的 http 服务，路由处理、日志打印、异常处理等都需要中间件来实现。

5. 基本使用 -- 启动一个 web 服务：
   ```js
      const Koa = require('koa');
      const app = new Koa();
      
      // 引入中间件
      app.use(async (ctx) => {
          ctx.body = '<h1>Hello Koa2</h1>';
      });
      // 监听端口
      app.listen(7001);
   ```

6. `use()` 方法接收一个中间件函数，用来实现对 ctx（上下文对象）进行处理。中间件使用 async 函数，这样使得内部可以以同步的方式处理异步任务。


## 3. Koa2 的路由

1. 路由（Routing）是由一个 URI（或者叫路径）和一个特定的 HTTP 方法（GET、POST 等）组成的，涉及到应用如何响应客户端对某个网站节点的访问。

2. 路由就是根据不同的 URL 地址，加载不同的页面实现不同的功能。

3. Koa2 中的路由和 Express 有所不同，在 Express 中直接引入Express 就可以配置路由，但是在 Koa 中我们需要安装对应的  `koa-router` 路由模块来实现。

4. `koa-router` 这个模块的最新版本已经更名为：`@koa/router` 

4. github 地址：[@koa/router](https://github.com/koajs/router)

### 1. `@koa/router` 的基本使用

1. 安装 `@koa/router`：
   - `npm install --save koa-router`
   - **注意**：最新版的 koa-router 安装方式已经发生变化，新版的安装方式如下（主要是包名的变化）：`npm install @koa/router`

2. 使用：
   ```js
      const Koa = require('koa');
      const Router = require('@koa/router');

      const app = new Koa();
      const router = new Router({
          // path的前缀，设置了 prefix 参数，则所有的path必须以/users      开头，才能被处理
          // / ————> /users/
          // /news ————> /users/news
          // prefix: '/users'
      });

      /**
       * ctx 表示上下文，包含了 request、response 等
       */
      router.get('/', async (ctx, next) => {
          // body 属性是返回的数据
          // 相当于 express 中的 res.send()
          ctx.body = '<h1>首页</h1>'
      });

      
      // 应用路由
      app
          .use(router.routes())    // 启动路由
          .use(router.allowedMethods());

      /**
       * router.allowedMethods()的作用：
       * 这是官方推荐的用法，router.allowedMethods()应用在路由匹配router.routes()之后
       * 所以在所有路由中间件调用之后调用，此时根据ctx.status设置response响应头
       *
       * 也就是说，如果出错，或者是我们忘记设置响应头，router.allowedMethods()就会帮助我们进行设置
       */

      app.listen(7001);
   ```
3. 应用了 router 这个中间件以后，应用就拥有了处理 get、post 的等请求的能力。

4. get() 方法用于处理 get 请求，第一个参数是一个路径，第二个参数是一个中间件函数，用来对当前请求的路径做出响应。

### 2. 获取查询字符串

1. 查询字符串常用于 get 请求。查询字符串的是多个 `key=value`形式的键值对使用 `&` 符号相连。然后将查询字符串与请求路径拼接，中间使用 `?` 连接。示例如下：
   `https://www.abc.com/index.html?key=123&name=jack&age=25`

2. 在 koa 中，获得查询字符串的方式有两种：
   1. `query`：格式化的参数对象，即 {key: value} 的形式。
   2. ` querystring`：查询字符串，也就是 `key=value` 的这种形式。

3. ctx 是上下文对象，包装了请求对象 - request 和响应对象 - response。同请求相关的内容，如查询字符串、路径、请求头等都封装在 requset 对象中。因此我们可以从 request 对象中拿到两种形式的查询字符串：
   ```js
      // 获取查询参数
      router.get('/newscontent', async (ctx, next) =>{

          // 1. 通过 ctx 的 request 对象获取
          const req = ctx.request;
          const {query, querystring} = req;
          console.log('query', query);
          console.log('querystring', querystring);
          // 通过request对象获取当前请求的url
          console.log(req.url);
          
          // 2. 直接通过ctx获取
          const {query, querystring} = ctx;
          console.log('query', query);
          console.log('querystring', querystring);
          // 通过ctx（上下文）获取当前请求的url
          console.log(ctx.url);
          ctx.body = '<h1>news content</h1>'
      })
   ```
4. ctx 本身也会对外暴露一些同 request 对象和 response 对象相关的属性，方便我们的使用。比如说查询字符串，可以直接从 ctx 对象中获取，如下所示：
   ```js
      // 获取查询参数
      router.get('/newscontent', async (ctx, next) =>{

          // 2. 直接通过ctx获取
          const {query, querystring} = ctx;
          console.log('query', query);
          console.log('querystring', querystring);
          // 通过ctx（上下文）获取当前请求的url
          console.log(ctx.url);
          ctx.body = '<h1>news content</h1>'
      })
   ```
### 3. 动态路由
1. 使用 url 参数（url parameters）动态捕获参数值，并解析为 `key: value` 的形式，放入 `ctx.params` 中。

2. url 参数形式：`/:xxx`，url参数只有一层的形式，匹配的 url 是：`/news/xxx`。如 `/news/12345` 匹配的路由是：`/news/:aid`，将 `{aid: 12345}`放入 `ctx.body` 中。

3. 示例如下：
   ```js
       router.get('/news/:aid', async (ctx, next) => {
          // 获取 url 参数
          const params = ctx.params;
          console.log(params);

          // body属性是返回的数据
          // 相当于express中的res.send()
          ctx.body = '<h1>新闻</h1>';
      });
   ```
     
## 4. Koa2 的中间件

### 1. 中间件的基本说明

1. 中间件就是匹配路由之前或者匹配路由完成做的一系列的操作，我们就可以把它叫做中间件。

2. 在 express 中，中间件（Middleware）是一个函数，它可以访问请求对象 - requst 和 响应对象 - response 以及 web 应用中处理请求-响应循环流程中的中间件，一般被命名为 next 的变量。express 中的中间件有以下作用：
   1. 执行任何代码
   2. 改变 request 对象和 response 对象
   3. 结束 request-response 循环
   4. 调用堆栈中的下一个中间件，调用方式是 执行 next() 方法。

3. Koa 中的中间件和 express 有点类似，因此 koa 中的中间件的作用也是上面说的几点。

4. 如果应用的 get、post 回调函数中，没有 next 参数，那么就匹配上第一个路由，就不会往下匹配了。如果想往下匹配的话，那么需要写next()。

5. `koa` 把很多 `async` 函数（路由处理函数）组成一个处理链，每个async 函数都可以做一些自己的事情，然后用 `await next()` 来调用下一个 `async` 函数。这里的每个 `async` 函数称为 middleware，这些 middleware 可以组合起来，完成很多有用的功能。

6. koa 的中间件是按照洋葱圈模式调用。不是按照先后顺序进行调用。如下图所示：  
   ![](./img/koa2-middleware.png)
   而 express 的中间件是按照顺序执行的，执行顺序的差异是 express 与 koa 中间件的最大的差异。

7. 在中间件执行的过程中，遇到 `next()`，就暂停执行当前的这个中间件，转而去执行下一个中间件。

8. 最后一个中间件或者是路由处理函数执行完成，就会返回上一个中间件，继续执行 `next()` 后面的代码。以此类推，只要当前中间件全部执行完成，就返回上一个中间件，执行 `next()` 后面的代码。如果当前中间件没有 `next()`，则不再继续调用下一个中间件。这个过程就类似于函数的嵌套调用，内层函数执行完成，才会继续执行外层函数。

9. 具体过程如下图所示：
   ![](./img/koa2-call_middleware.png)

10. 来自官网的一张图：
    ![](./img/koa2-middleware-official.gif)
11. 中间件分类
    1. 应用级中间件
    2. 路由级中间件
    3. 错误处理中间件
    4. 第三方中间件

### 2. 应用级中间件
1. 应用级中间件通过 `app.use()` 绑定到 app 对象的实例上。来扩展 app 的功能。

2. 示例：
   ```js

       const Koa = require('koa');
       const Router = require('@koa/router');

       const app = new Koa();
       const router = new Router({
          // path的前缀，设置了 prefix 参数，则所有的path必须以/users      开头，才能被处理
          // / ————> /users/
          // /news ————> /users/news
          // prefix: '/users'
       });

       /**
        * 应用级别中间件
        * 匹配任意路由
        */
        app.use(async (ctx, next) => {
      
            // 匹配路由之前，打印日期
            console.log(new Date());
      
            // 如果不调用 next()，则路由匹配在这里就会结束
            // 调用 next()，继续匹配下一个路由
           await next();
        });
        

        router.get('/', function (ctx, next) {
            ctx.body="Hello koa";
        });

        router.get('/news',(ctx,next)=>{
            ctx.body="新闻页面"
        });
        
        // 应用路由
        app
           .use(router.routes())    // 启动路由
           .use(router.allowedMethods());
   ```

### 3. 路由级中间件

1. 路由级中间件指的是在路由处理中使用中间件。对不同的路径进行不同的处理，同一个路径可以使用多个中间件对同一 request 对象和response 对象进行多次处理。

2. 示例：
   ```js
      router.get('/', async(ctx, next)=>{
          console.log(1);
          next();
      });
      router.get('/', function (ctx) {
          ctx.body = "Hello koa";
      });
   ```
   请求根路径的时候，先输出 1，然后调用 next()，进入下一个中间件，设置响应内容为 `Hello koa`。第二个中间件没有调用 next()，对根路径的响应处理结束。

### 4. 错误处理中间件

1. 示例代码：
   ```js
      app.use(async (ctx, next)=> {
         // 执行顺序：由于这个中间件匹配任何路由，所以首先会进入这个中间件，并开始执行
         console.log('中间件01');
         // 遇到 next()，调用下一个路由处理函数，此时会进行路由匹配，也就是会进入匹配的路由处理函数
         await next();
         // 执行完成路由处理函数，回到这里，接着执行 next()下面的代码
         if (ctx.status === 404) {
             ctx.body = '<h1>404 NOT FOUND</h1>'
         } else {
             console.log(ctx.url);
         }
     })
   ```

### 5. 第三方中间件

1. 第三方中间件用来给 koa 构建的应用增加额外的功能。可以在应用级或者路由级的应用上使用。如解析 post 请求体等功能依赖第三方中间件。

2. 示例：
   ```js
       const static = require('koa-static');
       const staticPath = './static';
       app.use(static(
           path.join( __dirname, staticPath)
       ));
       const bodyParser = require('koa-bodyparser');
       app.use(bodyParser());  
   ```

## 5. Koa2 处理 post 请求

### 1. 原生方式处理 post 请求

1. koa 和 node 本身并没有内置对 post 请求体解析的能力，原生解析 post 请求体是通过流的方式，如下所示：
   ```js
     const parsePostData = (ctx) => {
         // 函数的返回值是Promise，这样可以使用async/await获取异步操作结果
         return new Promise((resolve, reject) => {
             try {
                 let str = '';
                 // 使用的是 stream 的方式，从流中拼接出我们需要的内容
                 // 监听 data 事件
                 ctx.req.on('data', (chunk) => {
                     str += chunk;
                 })
                // 监听end事件
                // 表示流的传输已经结束，调用状态成功的的函数    resolve()，并传入str
                ctx.req.on('end', (chunk) => {
                    resolve(str);
                })
             } catch(err) {
                reject(err);
            }
         })

     }

     // 使用
     router.post('/login', async (ctx, next) => {
         // 原生方法（stream）获取post数据
         const ret = await getPostData(ctx);
         console.log(ret);
         ctx.body = ret;
      });
   ```
2. post 请求本身就是一个流，因此我们监听其 data 事件，将获得的数据片段不断拼接，当流结束的时候，会触发 end 事件，此时我们就获得了完整的流，这个流的形式是字符串。因为触发流的事件的过程是异步，因此使用 Promise 进行包裹，当流结束的时候，调用 resolve 函数，即将 Promise 由 pending 转换为 fulfilled 状态。

### 2. 使用 `koa-bodyparser` 处理

1. 使用第三方中间件 `koa-bodyparser` 解析 post 请求体。解析后的数据存放在 `ctx.request.body` 中。

2. 默认解析的是 json 格式的 post 请求数据。

3. 安装 `koa-bodyparser`：`npm install --save koa-bodyparser`

4. github 地址：[koa-bodyparser](https://github.com/koajs/bodyparser)

4. 使用：
   ```js
      const Koa = require('koa');
      const app = new Koa();

      const Router = require('@koa/router');
      const router = new Router();

      const bodyParser = require('koa-bodyparser');
      // 在解析post请求体之前，必须先使用bodyParser中间件
      app.use(bodyParser());
      app.use(async ctx => {
      ctx.body = ctx.request.body;
          console.log(ctx.request.body);
          ctx.body = '<h1>login success!!!!!</h1>';
      });
   ```
6. 另外一个处理 post 请求体的中间件：`koa-body`，这个中间件支持下面几种请求的 `content-type`：
   - `multipart/form-data`
   - `application/x-www-urlencoded`
   - `application/json`
   - `application/json-patch+json`
   - `application/vnd.api+json`
   - `application/csp-report`
   - `text/xml`

7. `koa-body` 还支持对上传文件的解析。而 `koa-bodyparser` 只支持 `json`, `form` and `text` 这几种类型的 post 请求体，不支持 `multipart/form-data`。也就是不支持对上传文件的解析。

8. `koa-body` 的安装：`npm install koa-body`

9. github 地址：[koa-body](https://github.com/koajs/koa-body)

9. `koa-body` 的使用：
   ```js
      const Koa = require('koa');
      const koaBody = require('koa-body');
 
      const app = new Koa();
 
      app.use(koaBody());
      app.use(async (ctx, next) => {
          ctx.body = `Request Body: ${JSON.stringify(ctx.request.body)}`;
      });
 
      app.listen(3000);
   ```

10. `koa-body` 与 `@koa/router` 一起使用：
    ```js
       const Koa = require('koa');
       const Router = require('@koa/router');
       const app = new Koa();
       const router = new Router({
          // path的前缀，设置了 prefix 参数，则所有的path必须以/users      开头，才能被处理
          // / ————> /users/
          // /news ————> /users/news
          // prefix: '/users'
       });
       const koaBody = require('koa-body');
       app.use(koaBody());
       router.post('/users', (ctx) => {
           console.log(ctx.request.body);
         // => POST body
         ctx.body = JSON.stringify(ctx.request.body);
       });
 
       app.use(router.routes());
 
       app.listen(3000);
    ```

## 6. Koa2 处理静态资源

1. 一个 http 请求访问 web 服务静态资源，一般响应结果有三种情况
   - 访问文本，例如js，css，png，jpg，gif
   - 访问静态目录
   - 找不到资源，抛出404错误

2. 在 Koa 中，我们使用中间件来实现对静态资源的访问。这里主要介绍一下中间件 `koa-static` 的基本用法。

3. github 地址：[koa-static](https://github.com/koajs/static)

4. `koa-static` 底层基于 `koa-send`，`koa-send` 是一个用于静态文件存储的中间件。`koa-send` 的官方文档地址：[koa-send](https://github.com/koajs/send)

5. 安装 `koa-static`：`npm install --save koa-static`
6. 示例代码：
   ```js
      const Koa = require('koa');
      const app = new Koa();

      const Router = require('@koa/router');
      const router = new Router();

      // 解析静态资源
      // 通俗的说，就是将指定的文件夹暴露给浏览器，使得其能够直接访问
      const serve = require('koa-static');

      // serve() 方法接收一个根路径作为参数
      app.use(serve('./static'));

      // 可以指定多个根路径
      // 服务器会从上到下（调用next()方法），依次进行路径的拼接，并根据路径查询相关的资源
      // 找到了，就直接返回给浏览器
      app.use(serve('./public'));
   ```
   server() 可以接收一个相对路径，也可以接收一个绝对路径。比如说引入 css 的标签是：`<link rel="stylesheet" href="css/style.css">`，相对路径是：`css/style.css`浏览器请求的 url 是：`http://localhost:7001/static/css/style.css`，直接请求肯定会报 404。使用 koa-static 这个中间件，服务器收到我们请求静态资源（image、js、css）的请求，将相对路径与 `serve()` 中传入的根路径进行拼接，即 `./static/css/style.css`,将这个路径所指向的文件，作为响应，返回给浏览器。

## 7. Koa2 处理 Cookies

1、在 Koa 中，我们可以给某个响应设置  Cookie 的信息，用来在客户端保持同用户相关的信息，实现免登录等功能。

2. Koa 中，可以直接设置 Cookie，如下所示：`ctx.cookies.set(name, value, [options])`

3. Cookie 本身就是一对键值对，name 用来设置键，value 用来设置值，options 是一个对象，用来对这条 Cookie 设置一些其他信息，比如 过期时间（expires）、在哪个域名下生效等。

4. options 可以设置的属性
   属性名称|属性说明
   :---:|:---:  
   maxAge | 一个数字表示从 Date.now() 得到的毫秒数，表示 Cookie 在 maxAge 指定的时间内生效，超过了这个 maxAge 指定的毫秒数，这个 Cookie 就失效了。
   expires | 过期的日期。expires 会指定一个日期时间。，在这个时间内，Cookie 有效，超过这个时间，Cookie 就失效了。
   path | 设置当前的Cookie 属于哪个路径下, 默认是 `/`。即在哪个路径下及其子路径下，可以访问到这个 Cookie
   domain| 域，表示当前 Cookie 所属于哪个域或子域下面。
   secure | 只在 https 的时候发送。如果当前是 http，则不会发送这个 Cookie。默认false。
   httpOnly | 表示无法通过 JavaScript 中的 document.cookie 访问。保证了安全性。默认是 true。
   overwrite | 一个布尔值，表示是否覆盖以前设置的同名的 Cookie (默认是 false). 如果是 true, 在同一个请求中设置相同名称的所有 Cookie（不管路径或域）是否在设置此 Cookie 时从 Set-Cookie 标头中过滤掉。

5. Koa中获取 Cookie 的值：`ctx.cookies.get('name')`

6. Koa 中设置中文 Cookie
   - Cookie 不支持设置中文，因此我们需要将中文转换成 base64 编码的形式写入 Cookie 中，再获取的时候，将 base64 编码转换为原来的中文形式。在 Node 中，Base64 编码的使用方式如下：
     ```js
        // 转换成base64字符串：aGVsbG8sIHdvcmxkIQ==
        console.log(new Buffer('hello, world!').toString ('base64'));
        // 还原base64字符串：hello, world!
        console.log(new Buffer('aGVsbG8sIHdvcmxkIQ==', 'base64').toString());
     ```

7. 示例代码：
   ```js
      const Koa = require('koa');
      const app = new Koa();

      const Router = require('@koa/router');
      const router = new Router();

      router.get('/news', async (ctx, next) => {

          // 通过 ctx.cookies.get('name')来获取cookie，接收的参数是cookie的name
          const userinfo = ctx.cookies.get('userinfo');
          // 获取中文形式的cookie，由于中文是经过base64编码的，所以获取cookie值以后，还得进行base64解码
          const encodedUserSchool = ctx.cookies.get('school');
          // base64解码
          const userSchool = Buffer.from(encodedUserSchool,    'base64').toString();
          console.log(userinfo);
          console.log(userSchool);
   
          ctx.body = `
              <h1>新闻</h1>
              <p>userInfo ${userinfo}</p>
          `

      });

      router.get('/shop', async (ctx, next) => {

         // 通过 ctx.cookies.get('name')来获取cookie，接收的参数是cookie的name
         const userinfo = ctx.cookies.get('userinfo');

   
          console.log(userinfo);
   
          ctx.body = `
              <h1>购物</h1>
              <p>userInfo ${userinfo}</p>
         `

      });

      router.get('/', async (ctx, next) => {

          // 通过 ctx.cookies.set(name, value, [options])用来设置cookies
          // options是可选参数，用来配置cookies的一些选项，值是一个对象
          ctx.cookies.set('userinfo', 'jack', {
           // maxAge表示这个cookies的最大存活时间
           maxAge: 60*1000*60*24,
              // expires表示过期的具体时间
              // expires: '2020-12-23',
              // 在哪个路径下，可以访问到这个 cookie，设置了/news，则只有在这个路径下，或者是以这个路径开头的其他路径，如 /news/001，能访问cookie
              // path: '/news',
              // 设置 cookie 的域名，在这个域名下的二级域名界面（a.baidu.com，b.baidu.com），都可以访问这个cookie
              // domain: '*.baidu.com',
              // true表示这个 cookie 只有服务器可以访问，设置为false，服务器和客户端（js）都可以访问
              // httpOnly: true,
          }); 
          // koa中的 cookies 无法直接设置中文
          // 直接设置了中文，就提示 TypeError: argument value is invalid
          // ctx.cookies.set('school', '北邮', {
          //     maxAge: 60*1000*60*24,
          // })

          // 想要在 cookie 中设置中文，必须先将中文转换为base64编码，获取这个 cookie 后，再进行 base64 解码
          const userSchool = Buffer.from('北邮').toString('base64');
          ctx.cookies.set('school', userSchool, {
           maxAge: 60*1000*60*24,
          });
          ctx.body = `
             <h1>首页</h1>
          `
      });
   ```


## 8. Koa2 中间件的开发

### 1. 基本中间件开发

1. 中间件就是一个简单的函数，函数的参数必须是 ctx 和 next，即上下文对象和 next 函数。中间件函数在运行过程中，必须手动调用 next 函数，这样才能运行下游的中间件。

2. 举个例子，我们想用给上下文对象 ctx 添加一个 `X-Response-Time` 属性，用来记录处理一个请求需要的时间，那么我们就可以写这样一个中间件：
   ```js
      async function responseTime(ctx, next) {
          const start = Date.now();
          await next();
          const ms = Date.now() - start;
          ctx.set('X-Response-Time', `${ms}ms`);
      }

      app.use(responseTime);
   ```
   在 use() 方法中，传入自定义的中间件，在接收到请求的时候，就能调用这个中间件了。

### 2. 中间件最佳实践 - 中间件配置项

1. 创建一个公共中间件的时候，我们希望可以有一些配置项来控制中间件的行为。那么通常的做法就是在中间件的外层包裹一层函数，外层的函数用来接收配置项，允许用户扩展函数的功能。

2. 实际上就是闭包，内层包裹的函数就是中间件，外层函数的返回值就是这个中间件函数。

3. 举个例子，我们创建一个接收格式化参数的打印日志中间件，代码如下：
   ```js
      function logger(format) {
          format = format || ':method ":url"';

          return async function (ctx, next) {
              const str = format
                  .replace(':method', ctx.method)
                  .replace(':url', ctx.url);

              console.log(str);

              await next();
          };
      }

      app.use(logger());
      app.use(logger(':method :url'));
   ```
   logger 函数的返回值是真正的中间件函数。logger 函数接收 format 参数，这个参数用来格式化日志，而真正用到这个参数的地方是返回值函数 -- 真正的中间件函数。

### 3. 中间件最佳实践 - 命名的中间件

1. 为了方便调试中间件，推荐使用命名的中间件，即返回值函数不使用匿名函数，而是使用命名函数：
   ```js
      function logger(format) {
          return async function logger(ctx, next) {

          };
      }
   ```

### 4. 中间件最佳实践 - 组合多个中间件

1. 有时我们需要将多个中间件组合为一个中间件，方便我们复用或者导出，因此我们可以使用 `koa-compose` 这个中间件来实现组合多个中间件。

2. `koa-compose` 的 github 地址：[koa-compose](https://github.com/koajs/compose)

3. `koa-compose` 安装：`npm install koa-compose`

4. 示例代码：
   ```js
      const compose = require('koa-compose');

      async function random(ctx, next) {
        if ('/random' == ctx.path) {
          ctx.body = Math.floor(Math.random() * 10);
        } else {
          await next();
        }
      };

      async function backwards(ctx, next) {
        if ('/backwards' == ctx.path) {
          ctx.body = 'sdrawkcab';
        } else {
          await next();
        }
      }

      async function pi(ctx, next) {
        if ('/pi' == ctx.path) {
          ctx.body = String(Math.PI);
        } else {
          await next();
        }
      }

      const all = compose([random, backwards, pi]);

      app.use(all);
   ```
   compose() 函数接收一个数组，数组的元素是中间件，compose() 会按照顺序组合这些中间件，最后返回一个新的中间件。

### 5. 中间件最佳实践 - 响应中间件

1. 响应中间件指的是，在这个中间件内部，我们就响应这次请求，不会调用下游中间件。如果想实现响应中间件，那么我们只需要绕过 next() 函数，即不再手动调用 next() 函数。这种情况在路由中间件中比较常见，但是我们可以在任何中间件中这样处理。

2. 假设有三个中间件，会依次调用，其中第二个中间件输出响应 `two`，如下所示：
   ```js
      app.use(async function (ctx, next) {
        console.log('>> one');
        await next();
        console.log('<< one');
      });

      app.use(async function (ctx, next) {
        console.log('>> two');
        ctx.body = 'two';
        await next();
        console.log('<< two');
      });

      app.use(async function (ctx, next) {
        console.log('>> three');
        await next();
        console.log('<< three');
      });
   ```
   在第二个中间件返回响应以后，仍旧调用了 next() 函数，那么下游的第三个中间件还是会被调用。这样就给了第三个中间件操作响应的机会。

3. 还是上面的例子，如果我们在第二个中间件中不再调用 next 函数，那么第二个中间件输出响应还是 `two`，但是第三个中间会被忽略：
   ```js
      app.use(async function (ctx, next) {
        console.log('>> one');
        await next();
        console.log('<< one');
      });

      app.use(async function (ctx, next) {
        console.log('>> two');
        ctx.body = 'two';
        console.log('<< two');
      });

      app.use(async function (ctx, next) {
        console.log('>> three');
        await next();
        console.log('<< three');
      });
   ```

## 9. Koa2 处理 Session

1. Sessionn 是另一种记录客户状态的机制，不同的是 Cookie 保存在客户端浏览器中，而 Session 保存在服务器上。

2. Session 的工作流程
   - 当浏览器访问服务器并发送第一次请求时，服务器端会创建一个 Session 对象，生成一个类似于`key:value`的键值对， 然后将 key（Cookie）返回到浏览器或者客户端，浏览器下次再访问时，携带 key（Cookie），找到对应的 Session（value）。 客户的信息都保存在 Session 中。

3. Koa 中，使用 `koa-session` 实现对 Session 的管理。github 地址：[koa-session](github.com/koajs/session)

4. 安装 `koa-session`：`npm install koa-session --save`

5. 引入 `koa-session`：`const session = require('koa-session');`

6. 配置 Session
   ```js
      // 设置Session
      const session = require('koa-session');
      /**
       * 配置session
       */

        // 加密字符串（salt）
        app.keys = ['some secret hurr'];

        const CONFIG = {
            // cookie的签名
            key: 'koa.sess', /** (string) cookie key (default is koa.sess) */
            /** (number || 'session') maxAge in ms (default is 1 days) */
            /** 'session' will result in a cookie that expires when session/browser is closed */
            /** Warning: If a session cookie is stolen, this cookie will never expire */
            // 过期时间
            maxAge: 86400000,
            // 自动将cookie（与session相关的 cookie）添加到请求头中
            // 默认为true，设置为false，则不会添加，因此服务端获取不到相应的key，也就无法获取session
            autoCommit: true, /** (boolean) automatically commit         headers (default true) */
            overwrite: true, /** (boolean) can overwrite or not         (default true) */
            httpOnly: true, /** (boolean) httpOnly or not (default true) */
            signed: true, /** (boolean) signed or not (default true)         */
            // 每次请求都重新设置session，即重置session的过期时间
            rolling: false, /** (boolean) Force a session identifier cookie to be set on every response. The expiration is reset to the original maxAge, resetting the expiration countdown. (default is false) */
            // 当session要过期的时候，更新session
            renew: false, /** (boolean) renew session when session is nearly expired, so we can always keep user logged in. (default is false)*/
            // 安全的cookie，要求必须是https访问
            secure: false, /** (boolean) secure cookie*/
            sameSite: null, /** (string) session cookie sameSite         options (default null, don't set it) */
        };

        app.use(session(CONFIG, app));
   ```

7. 使用了 koa-session 中间件以后，ctx 对象上会多出一个 session 对象，我们能可以通过这个属性设置和读取 session。方式如下：
   - 设置 Session：`ctx.session.userinfo = 'rose'`
   - 获得 Session：`const userinfo = ctx.session.userinfo`

8. 示例代码：
   ```js
      const Koa = require('koa');
      const app = new Koa();

      const Router = require('@koa/router');
      const router = new Router();

      // 设置Session
      const session = require('koa-session');
      /**
       * 配置session
       */

        // 加密字符串（salt）
        app.keys = ['some secret hurr'];

        const CONFIG = {
            // cookie的签名
            key: 'koa.sess', /** (string) cookie key (default is koa.sess) */
            /** (number || 'session') maxAge in ms (default is 1 days) */
            /** 'session' will result in a cookie that expires when session/browser is closed */
            /** Warning: If a session cookie is stolen, this cookie will never expire */
            // 过期时间
            maxAge: 86400000,
            // 自动将cookie（与session相关的 cookie）添加到请求头中
            // 默认为true，设置为false，则不会添加，因此服务端获取不到相应的key，也就无法获取session
            autoCommit: true, /** (boolean) automatically commit         headers (default true) */
            overwrite: true, /** (boolean) can overwrite or not         (default true) */
            httpOnly: true, /** (boolean) httpOnly or not (default true) */
            signed: true, /** (boolean) signed or not (default true)         */
            // 每次请求都重新设置session，即重置session的过期时间
            rolling: false, /** (boolean) Force a session identifier cookie to be set on every response. The expiration is reset to the original maxAge, resetting the expiration countdown. (default is false) */
            // 当session要过期的时候，更新session
            renew: false, /** (boolean) renew session when session is nearly expired, so we can always keep user logged in. (default is false)*/
            // 安全的cookie，要求必须是https访问
            secure: false, /** (boolean) secure cookie*/
            sameSite: null, /** (string) session cookie sameSite         options (default null, don't set it) */
        };

        app.use(session(CONFIG, app));

        const bodyParser = require('koa-bodyparser');
        
        // 在解析post请求体之前，必须先使用bodyParser中间件
        app.use(bodyParser());
        router.get('/news', async (ctx, next) => {

            // 获取session
            const userinfo = ctx.session.userinfo;
            ctx.body = `
                <h1>新闻</h1>
                <p>userInfo ${userinfo}</p>
            `

        });


      router.get('/', async (ctx, next) => {

          // 配置了koa-session中间件以后，在ctx对象上多出了一个session属性，我们能可以通过这个属性设置和读取session
          // 设置session
          ctx.session.userinfo = 'rose';

          ctx.body = `
              <h1>首页</h1>
          `;
      });

      app.use(router.routes());
      app.use(router.allowedMethods());

      app.listen(7001);
   ```
