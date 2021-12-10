<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Koa2 常用的 api](#koa2-%E5%B8%B8%E7%94%A8%E7%9A%84-api)
  - [1. 参考资料](#1-%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99)
  - [2. Application](#2-application)
    - [1. use(middleware)](#1-usemiddleware)
    - [2. listen](#2-listen)
    - [3. context](#3-context)
  - [3. Context](#3-context)
    - [1. ctx.req](#1-ctxreq)
    - [2. ctx.res](#2-ctxres)
    - [3. ctx.request](#3-ctxrequest)
    - [4. ctx.response](#4-ctxresponse)
    - [5. ctx.cookies.get(name, [options])](#5-ctxcookiesgetname-options)
    - [6. ctx.cookies.set(name, value, [options])](#6-ctxcookiessetname-value-options)
    - [7. 请求（request）属性的别名](#7-%E8%AF%B7%E6%B1%82request%E5%B1%9E%E6%80%A7%E7%9A%84%E5%88%AB%E5%90%8D)
    - [8. 响应（request）属性和方法的别名](#8-%E5%93%8D%E5%BA%94request%E5%B1%9E%E6%80%A7%E5%92%8C%E6%96%B9%E6%B3%95%E7%9A%84%E5%88%AB%E5%90%8D)
  - [4. Request](#4-request)
    - [1. request.header / request.header =](#1-requestheader--requestheader-)
    - [2. request.headers / request.headers =](#2-requestheaders--requestheaders-)
    - [3. request.method / request.method =](#3-requestmethod--requestmethod-)
    - [4. request.length](#4-requestlength)
    - [5. request.url / request.url =](#5-requesturl--requesturl-)
    - [6. request.originUrl](#6-requestoriginurl)
    - [6. request.origin](#6-requestorigin)
    - [7. request.href](#7-requesthref)
    - [8. request.path / request.path =](#8-requestpath--requestpath-)
    - [9. request.querystring / request.querystring =](#9-requestquerystring--requestquerystring-)
    - [10. request.search / request.search =](#10-requestsearch--requestsearch-)
    - [11. request.host](#11-requesthost)
    - [12. request.hostname](#12-requesthostname)
    - [13. request.URL](#13-requesturl)
    - [14. request.charset](#14-requestcharset)
    - [15. request.query / request.query =](#15-requestquery--requestquery-)
    - [16. request.type](#16-requesttype)
  - [5. Response](#5-response)
    - [1. response.header](#1-responseheader)
    - [2. response.headers](#2-responseheaders)
    - [3. response.socket](#3-responsesocket)
    - [4. response.status / response.status =](#4-responsestatus--responsestatus-)
    - [5. response.message / response.message =](#5-responsemessage--responsemessage-)
    - [6. response.length / response.length =](#6-responselength--responselength-)
    - [7. response.body / response.body =](#7-responsebody--responsebody-)
    - [8. response.get(field)](#8-responsegetfield)
    - [9. response.set(field, value)](#9-responsesetfield-value)
    - [10. response.append(field, value)](#10-responseappendfield-value)
    - [11. response.set(fields)](#11-responsesetfields)
    - [12. response.remove(field)](#12-responseremovefield)
    - [13. response.type / response.type =](#13-responsetype--responsetype-)
    - [14. response.redirect(url, [alt])](#14-responseredirecturl-alt)
    - [15. response.lastModified / response.lastModified =](#15-responselastmodified--responselastmodified-)
    - [16. response.etag](#16-responseetag)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Koa2 常用的 api

## 1. 参考资料

1. [koa - 英文官网](https://koajs.com/)

2. [koa - 中文官网](https://koa.bootcss.com)

## 2. Application

### 1. use(middleware)

1. 接收一个或者多个中间件函数作为参数。

2. app.use() 返回 this, 因此可以链式表达.

3. 用法：
   ```js
      app.use(someMiddleware)
      app.use(someOtherMiddleware)
      app.listen(3000)
   ```
4. 链式调用：
   ```js
      app
         .use(someMiddleware)
         .use(someOtherMiddleware)
         .listen(3000)
   ```

### 2. listen

1. Koa 应用程序不是 HTTP 服务器的1对1展现。 可以将一个或多个 Koa 应用程序组合到一起，这样在单个HTTP服务器是可以运行更大应用程序。

2. listen 方法创建并返回 HTTP Server 实例，将给定的参数传递给原生 Node 的 Server 实例的 listen()。

3. 通常使用 listen 方法创建一个 http 服务。

### 3. context

1. app.context 是从其创建 ctx 的原型。可以通过编辑 app.context 为 ctx 添加其他属性。这种方式在 ctx 添加在整个应用程序中能够使用到的属性或方法非常有用。

2. 举个例子，向 ctx 中添加数据库的引用：
   ```js
      app.context.db = db();

      app.use(async ctx => {
          console.log(ctx.db);
      });
   ```

## 3. Context

1. Koa 中的 Context 将 node 的 request 和 response 对象封装到单个对象中，为编写 Web 应用程序和 API 提供了许多有用的方法。 这些操作在 HTTP 服务器开发中频繁使用，它们被添加到此级别而不是更高级别的框架，这将强制中间件重新实现此通用功能。

2. 每个请求都将创建一个 Context，并在中间件中作为接收器引用，或者 ctx 标识符，如以下所示：
   ```js
      app.use(async ctx => {
           ctx; // 这是 Context
           ctx.request; // 这是 koa Request
           ctx.response; // 这是 koa Response
      });
   ```
3. 为方便起见，许多上下文（context）的访问器和方法直接委托给它们的 ctx.request 或 ctx.response。其他方面它们是相同的。例如 `ctx.type` 和 `ctx.length` 委托给 response 对象，`ctx.path` 和 `ctx.method` 委托给 `request` 对象。

4. **注意**：上下文对象在整个请求期间是不变的，我们在不同的中间件中操作的上下文对象都是一个。

### 1. ctx.req

1. 原生 Node 的 request 对象。

### 2. ctx.res

1. 原生 Node 的 response 对象。

2. 绕过 Koa 的 response 处理是不被支持的。应避免使用以下 node 属性直接处理响应：
   - res.statusCode
   - res.writeHead()
   - res.write()
   - res.end()


### 3. ctx.request

1. Koa 的 request 对象。

### 4. ctx.response

1. Koa 的response 对象。

### 5. ctx.cookies.get(name, [options])

1. 这个方法从请求的 cookies 拿到指定名称的 cookie。

2. signed 参数表示所请求的cookie应该被签名。

3. koa 使用 cookies 模块，其中只需传递参数。

### 6. ctx.cookies.set(name, value, [options])

1. 设置响应的 cookies。

2. name 是 cookie 名称，value 是 cookie 值，options 是 cookie 的配置项。

3. 配置项：
   - maxAge: 一个数字, 表示从 Date.now() 得到的毫秒数.
   - expires: 一个 Date 对象, 表示 cookie 的到期日期 (默认情况下在会话结束时过期).
   - path: 一个字符串, 表示 cookie 的路径 (默认是/).
   - domain: 一个字符串, 指示 cookie 的域 (无默认值).
   - secure: 一个布尔值, 表示 cookie 是否仅通过 HTTPS 发送 (HTTP 下默认为 false, HTTPS 下默认为 true). 阅读有关此参数的更多信息.
   - httpOnly: 一个布尔值, 表示 cookie 是否仅通过 HTTP(S) 发送，, 且不提供给客户端 JavaScript (默认为 true).
   - sameSite: 一个布尔值或字符串, 表示该 cookie 是否为 "相同站点" cookie (默认为 false). 可以设置为 'strict', 'lax', 'none', 或 true (映射为 'strict').
   - signed: 一个布尔值, 表示是否要对 cookie 进行签名 (默认为 false). 如果为 true, 则还会发送另一个后缀为 .sig 的同名 cookie, 使用一个 27-byte url-safe base64 SHA1 值来表示针对第一个 Keygrip 键的 cookie-name=cookie-value 的哈希值. 此签名密钥用于检测下次接收 cookie 时的篡改.
   - overwrite: 一个布尔值, 表示是否覆盖以前设置的同名的 cookie (默认是 false). 如果是 true, 在同一个请求中设置相同名称的所有 Cookie（无论路径或域）是否在设置此Cookie 时从 Set-Cookie 消息头中过滤掉.

### 7. 请求（request）属性的别名

1. 下面的属性和 request 中的属性相同，即可以从上下文对象中访问这些属性，也可以从 request 对象中范围这些属性。

2. 等效的同名属性：
   - ctx.header
   - ctx.headers
   - ctx.method
   - ctx.method=
   - ctx.url
   - ctx.url=
   - ctx.originalUrl
   - ctx.origin
   - ctx.href
   - ctx.path
   - ctx.path=
   - ctx.query
   - ctx.query=
   - ctx.querystring
   - ctx.querystring=
   - ctx.host
   - ctx.hostname
   - ctx.fresh
   - ctx.stale
   - ctx.socket
   - ctx.protocol
   - ctx.secure
   - ctx.ip
   - ctx.ips
   - ctx.subdomains
   - ctx.is()
   - ctx.accepts()
   - ctx.acceptsEncodings()
   - ctx.acceptsCharsets()
   - ctx.acceptsLanguages()
   - ctx.get()

### 8. 响应（request）属性和方法的别名

1. 下面的属性和 response 中的属性相同，即可以从上下文对象中访问这些属性，也可以从 response 对象中范围这些属性。

2. 等效的同名属性：
   - ctx.body
   - ctx.body=
   - ctx.status
   - ctx.status=
   - ctx.message
   - ctx.message=
   - ctx.length=
   - ctx.length
   - ctx.type=
   - ctx.type
   - ctx.headerSent
   - ctx.redirect()
   - ctx.attachment()
   - ctx.set()
   - ctx.append()
   - ctx.remove()
   - ctx.lastModified=
   - ctx.etag=

## 4. Request

### 1. request.header / request.header = 

1. 获得请求头对象，这与 node 中的 http.IncomingMessage 上的 headers 对象相同。

2. header 属性是 get 属性，也是 set 属性。我们可以给 header 属性设置值。如：`request.header = xx`。

### 2. request.headers / request.headers = 

1. 获得请求头对象，是 request.header 的别名。

2. headers 属性是 get 属性，也是 set 属性。我们可以给 headers 属性设置值。如：`request.headers = xx`。

### 3. request.method / request.method = 

1. 获得请求方法。

2. method 属性是 get 属性，也是 set 属性。我们可以给 method 属性设置值。如：`request.method = xx`。对于实现诸如 methodOverride() 的中间件是有用的。

### 4. request.length

1. 如果请求中存在 Content-Length 字段，那么 通过 request.length 能得到这个字段的值。不存在的话，request.length 就是 undefined。

### 5. request.url / request.url = 

1. 得到请求的 url。

2. url 属性是 get 属性，也是 set 属性。我们可以给 url 属性设置值。如：`request.url = xx`。设置请求的 url 对 url 重写有用。

### 6. request.originUrl

1. 得到请求的原始的 url。

### 6. request.origin

1. 获取URL的来源，包括 protocol 和 host。

### 7. request.href

1. 获取完整的请求 URL，包括 protocol，host 和 url。

### 8. request.path / request.path = 

1. 获取请求路径名。

2. path 属性是 get 属性，也是 set 属性。我们可以给 path 属性设置值。如：`request.path = xx`。这样就能设置请求路径名，并在存在时保留查询字符串。

### 9. request.querystring / request.querystring = 

1. 获得原始的查询字符串。

2. querystring 属性是 get 属性，也是 set 属性。我们可以给 querystring 属性设置值。如：`request.querystring = xx`。这样就能设置原始查询字符串。

### 10. request.search / request.search = 

1. 获得原始的查询字符串。

2. search 属性是 get 属性，也是 set 属性。我们可以给 search 属性设置值。如：`request.search = xx`。这样就能设置原始查询字符串。

### 11. request.host

1. 存在时获取主机（hostname:port）。当 app.proxy 是 true 时支持 X-Forwarded-Host，否则使用 Host。

### 12. request.hostname

1. 存在时获取主机名。当 app.proxy 是 true 时支持 X-Forwarded-Host，否则使用 Host。

### 13. request.URL

1. 获取 WHATWG 解析的 URL 对象。

### 14. request.charset

1. 存在时获得请求的字符集，或者是 undefined。

### 15. request.query / request.query = 

1. 解析查询字符串，将其转换成对象。当没有查询字符串时，返回一个空对象。请注意，此 get 属性的 query 不支持嵌套解析。

2. 例如： `color=blue&size=small` 被解析为：
   ```js
      {
          color: 'blue',
          size: 'small'
      }
   ```
3. query 属性是 get 属性，也是 set 属性。我们可以给 query 属性设置值。如：`request.query = {next: '/login'}`。这样将查询字符串设置为给定对象。set 属性的 query 不支持嵌套对象。 

### 16. request.type

1. 获取请求 Content-Type, 不含 `charset` 等参数。注: 这里其实是只获取 mime-type。

## 5. Response

### 1. response.header

1. 响应头对象。

### 2. response.headers

1. 响应头对象。别名是 response.header。

### 3. response.socket

1. 响应套接字。 作为 request.socket 指向 net.Socket 实例。

### 4. response.status / response.status =

1. 获取响应状态。默认情况下，response.status 设置为 404 而不是像 node 的 res.statusCode 那样默认为 200。

2. 通过 status 还可以设置响应的状态码，如：`response.status = 301`。

3. 可用的状态码：
   - 100 "continue"
   - 101 "switching protocols"
   - 102 "processing"
   - 200 "ok"
   - 201 "created"
   - 202 "accepted"
   - 203 "non-authoritative information"
   - 204 "no content"
   - 205 "reset content"
   - 206 "partial content"
   - 207 "multi-status"
   - 208 "already reported"
   - 226 "im used"
   - 300 "multiple choices"
   - 301 "moved permanently"
   302 "found"
   - 303 "see other"
   - 304 "not modified"
   - 305 "use proxy"
   - 307 "temporary redirect"
   - 308 "permanent redirect"
   - 400 "bad request"
   - 401 "unauthorized"
   - 402 "payment required"
   - 403 "forbidden"
   - 404 "not found"
   - 405 "method not allowed"
   - 406 "not acceptable"
   - 407 "proxy authentication required"
   - 408 "request timeout"
   - 409 "conflict"
   - 410 "gone"
   - 411 "length required"
   - 412 "precondition failed"
   - 413 "payload too large"
   - 414 "uri too long"
   - 415 "unsupported media type"
   - 416 "range not satisfiable"
   - 417 "expectation failed"
   - 418 "I'm a teapot"
   - 422 "unprocessable entity"
   - 423 "locked"
   - 424 "failed dependency"
   - 426 "upgrade required"
   - 428 "precondition required"
   - 429 "too many requests"
   - 431 "request header fields too large"
   - 500 "internal server error"
   - 501 "not implemented"
   - 502 "bad gateway"
   - 503 "service unavailable"
   - 504 "gateway timeout"
   - 505 "http version not supported"
   - 506 "variant also negotiates"
   - 507 "insufficient storage"
   - 508 "loop detected"
   - 510 "not extended"
   - 511 "network authentication required"

4. 由于 response.status 默认设置为 404，因此发送没有 body 且状态不同的响应的操作如下：
   ```js
      ctx.response.status = 200;

      // 或其他任何状态
      ctx.response.status = 204;
   ```

### 5. response.message / response.message = 

1. 获取响应的状态消息. 默认情况下, response.message 与 response.status 关联。

2. 还可以给 message 属性赋值。即将响应的状态消息设置为给定值。

### 6. response.length / response.length = 

1. 以数字返回响应的 Content-Length，或者从 ctx.body 推导出来，或者undefined。

2. 还可以给 length 属性赋值。将响应的 Content-Length 设置为给定值。

### 7. response.body / response.body =

1. 获得响应主体。

2. 使用 body 属性设置响应的主体，如：`response.body = xx`。

3. body 可以接受下列内容作为响应体：
   - string
   - Buffer
   - Stream
   - Object | Array，能够被序列化的对象或者数组
   - null

4. 如果没有设置 response.status，那么 Koa 会自动地设置响应状态为 200 或者 204。

5. 设置的 body 类型为 string：`Content-Type` 默认为 `text/html` 或 `text/plain`，同时默认字符集是 utf-8。Content-Length 字段也是如此。

6. 设置的 body 类型为 Buffer：Content-Type 默认为 `application/octet-stream`，并且 Content-Length 字段也是如此。

7. 设置的 body 类型为 Stream：Content-Type 默认为 `application/octet-stream`。每当流被设置为响应主体时，`.onerror` 作为侦听器自动添加到 error 事件中以捕获任何错误。此外，每当请求关闭（甚至过早）时，流都将被销毁。如果你不想要这两个功能，请勿直接将流设为主体。

8. 设置的 body 类型为 Object：Content-Type 默认为 `application/json`。 这包括普通的对象 `{ foo: 'bar' }` 和数组 `['foo', 'bar']`。

### 8. response.get(field)

1. 不区分大小写获得响应头字段 field 的值。

2. 示例：`const etag = ctx.response.get('ETag');`

### 9. response.set(field, value)

1. 设置响应头字段名称和值。

2. 示例：`ctx.set('Cache-Control', 'no-cache');`

### 10. response.append(field, value)

1. 向响应头中添加额外的头字段名称和值。

2. 示例：`ctx.append('Link', '<http://127.0.0.1/>');`

### 11. response.set(fields)

1. 使用对象的形式设置多个请求头字段和值。

2. 示例：
   ```js
      ctx.set({
         'Etag': '1234',
         'Last-Modified': date
      });
   ```
 
### 12. response.remove(field)

1. 移除头部字段 field。

### 13. response.type / response.type = 

1. 获取响应 Content-Type, 不含 `charset` 等参数。注: 这里其实是只获取 mime-type。

2. 还可以给 response.type 指定值，这样就设置了 响应 Content-Type。

3. 可以通过 mime 字符串或文件扩展名给 response.type 设置值，这两种方式都可以正确的设置响应的 Content-Type。

4. 通过 type 属性设置 Content-Type 的示例：
   ```js
      ctx.type = 'text/plain; charset=utf-8';
      ctx.type = 'image/png';
      ctx.type = '.png';
      ctx.type = 'png';
   ```
5. **注意**: 在适当的情况下 Koa 为你选择 charset, 比如 `response.type = 'html'` 将默认是 `utf-8`。如果你想覆盖 charset, 使用 `ctx.set('Content-Type', 'text/html')` 将响应头字段设置为直接值。

### 14. response.redirect(url, [alt])

1. 重定向到指定的 url。

2. 第一个参数可以指定为字符串 `back`，`back` 是特别提供 Referrer 支持的，当 Referrer 不存在时，使用 `alt` 或 `/`。

3. back 这个参数的意义是，如果请求头中提供了 Referrer 字段，Referrer 字段表示当前页面从哪个页面跳转过来的，那么设置 redirect 的第一个参数为 back，将会重定向到 Referrer 指定的地址。

4. 基本用法：
   ```js
      ctx.redirect('back');
      ctx.redirect('back', '/index.html');
      ctx.redirect('/login');
      ctx.redirect('http://google.com');
   ```

6. 要更改 `302` 的默认状态，只需在调用 redirect 之前或之后给 status 赋值。要改变响应体应该在调用 redirect 之后。
   ```js
      ctx.status = 301;
      ctx.redirect('/cart');
      ctx.body = 'Redirecting to shopping cart';
   ```
### 15. response.lastModified / response.lastModified = 

1. 如果存在 lastModified，将以 Date 形式返回。

2. 也可以将 Last-Modified 消息头设置为适当的 UTC 字符串。您可以将其设置为 Date 或日期字符串。
   ```js
      ctx.response.lastModified = new Date();
   ```

### 16. response.etag

1. 设置响应头的 etag 字段。注意，没有 get 属性的 response.etag 属性，即只能设置 response。etag。

2. 示例：`ctx.response.lastModified = new Date();`