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
### 5. defer 和 async 的区别和作用
### 6. TCP 四次挥手

## 浏览器地址栏输入请求地址到页面回显发生了什么

## 从地址栏输入地址到页面回显，都发生了什么

## 浏览器缓存

etag 和 if-none-match

last-modified 和 if-modified-since

## 说下 http 缓存,如何实现

## 说一下你知道的浏览器渲染相关的点


## 说下跨域

1. jsonp

2. cors

3. postMessage


## 前端如何做性能优化

## post 请求有几种方式触发

## get 和 post 的区别

## http 状态码

## post 的 ContentType 类型有哪些

1. application/x-www-form-urlencoded
2. multipart/form-data
3. application/json
4. text/plain
(application/x-www-form-urlencoded/multipart/form-data/text/plain)

## 说一下 http 和 https 的区别

## 说下 tls 握手

## https的 tls 了解吗

## http中间人劫持了解吗?如何解决呢?(说了 https)

## 为什么https可以做到避免中间人劫持?(说了加密层 tls)

## 展开说一下 tls 握手(对称,非对称,对称+非对称的组合)

## 加密套件指的是?(举例了 AES )

