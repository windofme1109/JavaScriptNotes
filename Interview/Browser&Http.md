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

4. 减少回流的操作：

## 重绘和重排了解吗

## 重绘和重排如何做取舍

## 解释下重绘和回流


## 从地址栏输入地址到页面回显,都发生了什么

## 浏览器地址栏输入请求地址到页面回显发生了什么

## 从地址栏输入地址到页面回显,都发生了什么

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

