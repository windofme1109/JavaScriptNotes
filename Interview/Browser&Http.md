## 解释下重绘和回流

1. 回流（reflow）
   - 元素的大小、位置、字体的大小等发生变化，引起页面布局的变化，浏览器渲染部分或者渲染全部的元素，这个过程被称为回流。
   - 引起回流的操作：
     - 浏览器首次加载
     - 浏览器窗口大小发生变化
     - 元素的尺寸或者位置发生变化
     - 元素的字体大小发生变化
     - 元素的内容发生变化（文字数量或者图片数量）
     - 激活伪类
     - 访问某些属性或方法，比如 clientHeight，scrollTop，如 scrollTo() 等

2. 重绘（repaint）
   - 当元素的背景颜色、字体颜色等不影响布局的属性发生变化，浏览器仅仅是局部页面重新渲染，将新的样式赋给元素，这个过程被称为重绘。

3. 回流的代价远远大于重绘，因此要减少回流的次数。

4. 减少回流的操作 - CSS
   - 避免使用 table 布局
   - 尽可能在 DOM 末端改变 class
   - 将动画效果应用在 position 为 absolute 或者 fixed 的元素上
   - 尽量不适用计算属性，如 calc()
   - 避免设置多层内联样式
   
5. 减少回流的操作 - JavaScript
   - 避免循环操作 DOM
   - 避免频繁读取引发回流的属性，如果不可避免，将这个属性缓存起来
   - 将复杂元素设置为固定定位或绝对定位，使其脱离文档流。
   - 避免逐项修改行内样式，最好一次修改 style 样式，或者定义 class，通过修改 class 的方式改变样式。
   
## 重绘和重排了解吗

## 重绘和重排如何做取舍

## 解释下重绘和回流

## 从地址栏输入地址到页面回显，都发生了什么

1. DNS 解析，将域名转换为 IP 地址，获得目标服务器的地址。

2. 发起连接，经过 TCP 三次握手，服务器和浏览器建立连接

3. 服务器向浏览器传送 html、css、js 等内容

4. 浏览器开始解析 html、css 和 js，html 解析为 DOM，css 解析为 CSSOM，二者结合形成 Render Tree。浏览器根据 Render Tree 开始绘制页面。

5. 数据传输完毕后，经过 TCP 四次挥手，连接断开。


## 可能考察的点：

### 1. DNS

### 2. TCP 三次握手

### 3. HTTP / HTTPS / HTTP 2.0

### 4. 浏览器渲染原理

### 5. 浏览器渲染的优化

### 6. defer 和 async 的区别和作用

### 7. TCP 四次挥手

## 浏览器地址栏输入请求地址到页面回显发生了什么

## 从地址栏输入地址到页面回显，都发生了什么

## 浏览器缓存

1. 浏览器会将加载过的资源存储在本地，这样下次请求相同的资源时，就可以不用向服务器请求，而是直接从本地读取，这样大大提高了资源的加载速度。

2. 缓存分为两种，一种是强缓存，一种是协商缓存。
   - 强缓存指的是浏览器直接从本地读取的缓存。且不需要向服务器发送请求。如果我们设置 cache-control 为 max-age 或者设置了 expire，则浏览器会检查当前的缓存是否过期，如果没有过期，则直接从本地读取缓存。max-age 的优先级比 expire 高。
   - 协商缓存指的浏览器不能直接使用本地的缓存，而是需要向服务器发送一次请求，如果服务器返回 304，则表示可以使用本地缓存。协商缓存的控制字段是 cache-control 中的 no-cache。

3. 协商缓存的过程：
   - 浏览器第一次请求某个资源，服务器接收到请求后，返回这个资源，并在响应头中设置了 last-modified 和 etag。last-modified 表示上一次修改时间，etag 是根据文件内容生成的文件唯一标识。
   - 如果 cache-control 设置的是 no-cache，那么当浏览器第二次请求这个资源时，会向服务器发送一个请求，在请求头中携带 if-none-match 和 if-modified-since，其中 if-none-match 就是上次服务器返回的响应的 etag 值，而 if-modified-since 就是上次服务器返回的响应的 last-modified 值。
   - 服务器收到请求以后，首先比较资源的 etag 与请求头中的 if-none-match 是否相同，如果相同，则返回 304（not modified），表示资源没有被修改，可以使用缓存。如果不相同，则返回 200（ok），并携带最新的资源，以及最新的 etag 和 last-modified。
   - 如果没有 etag 字段，则比较 last-modified 与 请求头中的 if-modified-since 是否相同，如果相同，则返回 304（not modified），表示资源没有被修改，可以使用缓存。如果不相同，则返回 200（ok），并携带最新的资源，以及最新的 etag 和 last-modified。

4. etag 的优先级比 last-modified 高。

5. last-modified 只精确到秒，如果一个文件在 1s 内频繁改动，就无能为力了。

## 如何设置 js、css 等资源文件不缓存

1. 在工程构建过程中，给资源的名称加上资源的 md5 值，当文件的内容发生变化时，md5 值发生变化，资源的名称也会发生变化，嵌入到 html 中的文件名称也会发生变化。对于浏览器而言，文件名发生变化，相当于资源的 url 变了，浏览器就会请求最新的资源文件。

## 说下 http 缓存,如何实现

## 说一下你知道的浏览器渲染相关的点



## 前端如何做性能优化

1. 图片
   - 图片压缩
   - 使用 css 代替图片
   - 小图片使用 base64 编码
   - 多张图片合并到一张图片
   - 图片懒加载
   - 图片预加载

2. css - 减少回流和重绘的操作
   - css 放在 header 标签中
   - 减少计算操作，如 calc()、`@import` 等
   - 将使用动画的元素设置为固定定位或者绝对定位。
   - 复杂元素使用固定定位或者绝对定位，使其脱离文档流。
   - 压缩 css 文件

3. js
   - 压缩 js 代码
   - js 放在 body 标签后
   - js 减少 DOM 操作，同时合理设置事件监听，充分利用事件委托。
   - 减少 DOM 的层级
   - 避免循环操作 DOM
   - 合理设置 script 标签的 defer 和 async 属性。
   - 避免频繁访问 offsetHeight、scrollTop 等引起回流的属性，如果需要，可以将其缓存起来。

4. http
   - 使用 CDN，将公共的 js 、css 等资源文件放到 CDN 服务器上。
   - 合理使用缓存
   - 使用 http 2.0
   - 小文件合并成大文件，减少请求次数。

5. 防抖

6. 节流

7. 懒加载

8. 预加载

9. 延迟执行
   - 当前页面某些代码不需要执行，因此可以在加载页面的过程中不加载这部分代码，等到空闲的时候，再去请求，这样可以提高页面的加载速度。
   - 可以通过配置 webpack 的 prefetch 或者 preload 实现。这两个字段是 magic comment 的字段，配合 import() 动态导入实现。

## 说下跨域

1. jsonp

2. cors
   - 简单请求
     - 请求方法是：get、post、header 这三个方法之一，请求头是：accept、accept-language 等，content-type 是 application/x-www-form-urlencoded、multipart/form-data 和 text/plain
     - 请求头携带 origin 字段，表示请求的源信息。
     - 直接向服务器发送请求，服务器返回的响应头中包括：access-control-allow-origin 字段，要么是请求头中 origin 字段，要么是 *，表示允许一切跨域。
   - 非简单请求
     - 请求方法是 put、delete 等，有自定义的请求头或者 content-type 是 application/json，那么就是非简单请求。
     - 浏览器首先要发送一个预检请求（preflight），请求方法是 options，请求头会包含两个字段：access-control-request-methods 和 access-control-request-header，表示服务器支持哪些请求头和请求方法。
     - 服务器返回的响应除了 access-control-allow-origin 外，还包括 access-control-allow-method 和 access-control-allow-method。
     - 如果请求方法和请求头在服务器的允许范围内，则浏览器会发送正式的请求。否则会报错。

3. postMessage
   - html 新增的 api。用于不同源的页面的通信。
   - 用法 - 发送消息
     - otherWindow.postMessage
   - 用法 - 接收消息
     - 监听 message 事件：`window.addEventListener('message', function(e) {})`



## post 请求有几种方式触发

## get 和 post 的区别

1. 参数长度限制
   - get 对参数长度有限制，因为 get 的参数是放在 url 中的，而 url 的长度受到浏览器的限制。http 协议没有对 url 的长度做出限制。
   - post 请求将参数放到请求体中，因此对长度没有限制。

2. 参数形式
   - get 请求的参数放到路径的后面，以 ? 分隔，形式是 key-value 的形式，多个参数使用 & 符号分隔。
   - post 请求的参数放到请求体中，形式多样。

3. 参数编码
   - get 请求只支持 ascii 字符，对于非 ascii 字符需要转换为 ascii 字符。content-type 是 application/x-www/url-encoded
   - post 请求支持多种编码方式，content-type 通常是：multipart/form-data、application/x-www-form-urlencoded、application/json、text/plain 等。

4. 用途
   - get 请求一般用于获取显示的内容，如展示一个页面，通常对资源没有副作用，即 get 请求是幂等性的。
   - post 请求通常是我们需要对服务器的资源进行某种操作，如登录、删除、注册等操作。post 请求是有副作用的，会对服务器上的资源进行修改。

6. 安全性
   - get 请求将请求参数放到 url 中，对外是可见的，因此不是很安全。
   - post 请求将参数放到请求体中，对外不可见，因此比 get 请求安全。

5. 其他
   - get 请求刷新/后退没有危害，而 post 请求刷新/后退会重新提交数据（浏览器有提示）
   - get 请求可以被缓存，post 请求不能被缓存
   - get 请求可以被收藏，而 post 不可以
   - get 请求可以被保存在浏览器的历史中，而 post 不能

## http 状态码

1. 2xx
   - 200 ok
   - 204 no content
   - 206 partial content

2. 3xx
   - 301 moved permanently
   - 302 found
   - 304 not modified
   - 307 temporary redirect 

3. 4xx
   - 400 bad request
   - 401 unauthorized
   - 403 forbidden
   - 404 not found

4. 5xx
   - 500 interal server error
   - 501 not implemented
   - 502 bad gate
   - 503 service unavailable

## post 的 ContentType 类型有哪些

1. application/x-www-form-urlencoded

2. multipart/form-data

3. application/json

4. text/plain


## 说一下 http 和 https 的区别

1. http 存在的问题
   - 不对通信双方身份进行验证，容易别别人冒充身份
   - 不对报文的完整性进行校验，容易遭到篡改
   - 请求信息明文传输，容易被窃听和获取

2. https 就是 http + ssl / tls，就是在 应用层和传输层加了一个 ssl / tls 层，用来对 http 报文进行加密和解密，同时对通信双方身份进行验证。


## 说下 tls 握手

1. 浏览器发起 https 请求，服务器返回 CA 证书，包含 public key

2. 浏览器验证 CA 证书的合法性，如果证书合法，从证书中提取出 public key，然后生成一个随机的密钥 key

3. 使用 public key 将随机密钥加密，然后发送给服务器

4. 服务器使用 private key 将随机密钥解出来

5. 服务器利用随机密钥加密与客户端之间的通信。

## tls 握手阶段

1. 验证 CA 证书

2. 非对称加密传输会话密钥

3. 对称加密传输通信内容


## https 的 tls 了解吗

1. tls = 对称加密 + 非对称加密 + 散列函数


## http 中间人劫持了解吗 ? 如何解决呢?(说了 https)

## 为什么 https 可以做到避免中间人劫持?(说了加密层 tls)

1. tls 加密

## 展开说一下 tls 握手( 对称, 非对称,对称+非对称的组合)

## 加密套件指的是?(举例了 AES )

## 用到过的前端性能优化

1. React 优化
   - 列表循环渲染加上 key
   - 不使用多余的 props
   - shouldComponentUpdate

2. 使用雪碧图。

3. 小图片使用 base64 格式。

4. 防抖函数。

5. 使用 http 2.0。

6. 将公共的静态资源部署在 CDN 服务器上。

