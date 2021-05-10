<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [HTTP 请求方法](#http-%E8%AF%B7%E6%B1%82%E6%96%B9%E6%B3%95)
  - [1. HTTP 常见请求方法](#1-http-%E5%B8%B8%E8%A7%81%E8%AF%B7%E6%B1%82%E6%96%B9%E6%B3%95)
    - [1. GET](#1-get)
    - [2. POST](#2-post)
    - [3. PUT](#3-put)
    - [4. DELETE](#4-delete)
    - [5. OPTIONS](#5-options)
    - [6. HEAD](#6-head)
    - [7. TRACE](#7-trace)
    - [8. CONNECT](#8-connect)
  - [2. get 与 post 的区别](#2-get-%E4%B8%8E-post-%E7%9A%84%E5%8C%BA%E5%88%AB)
    - [1. 使用上的区别](#1-%E4%BD%BF%E7%94%A8%E4%B8%8A%E7%9A%84%E5%8C%BA%E5%88%AB)
    - [2. 其他区别](#2-%E5%85%B6%E4%BB%96%E5%8C%BA%E5%88%AB)
    - [3. 参考资料](#3-%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# HTTP 请求方法

## 1. HTTP 常见请求方法

### 1. GET

1. 向指定的资源发出“显示”请求，使用 GET 方法应该只用在读取数据上，而不应该用于产生“副作用”的操作中。

2. 所谓的副作用，指的是会对数据产生影响，如增加、删除、修改等操作。对应的实际操作就是提交订单、购物车添加内容等。

### 2. POST

1. 指定资源提交数据，请求服务器进行处理（例如提交表单或者上传文件）。数据被包含在请求文本中。这个请求可能会创建新的资源或者修改现有资源，或两者皆有。

### 3. PUT

1. 向指定资源位置上传其最新内容。

### 4. DELETE

1. 请求服务器删除 Request-URI 所标识的资源。

### 5. OPTIONS

1. 使服务器传回该资源所支持的所有HTTP请求方法。用 `*` 来代替资源名称，向 Web 服务器发送 OPTIONS 请求，可以测试服务器功能是否正常运作。

### 6. HEAD

1. 与 GET 方法一样，都是向服务器发出指定资源的请求，只不过服务器将不传回资源的本文部分，它的好处在于，使用这个方法可以在不必传输全部内容的情况下，就可以获取其中关于该资源的信息（原信息或称元数据）。

### 7. TRACE

1. 显示服务器收到的请求，主要用于测试或诊断。

### 8. CONNECT

1. HTTP/1.1 中预留给能够将连接改为通道方式的代理服务器。通常用于 SSL 加密服务器的链接（经由非加密的 HTTP 代理服务器）。

## 2. get 与 post 的区别

### 1. 使用上的区别

1. GET 后退按钮/刷新无害，POST 数据会被重新提交（浏览器应该告知用户数据会被重新提交）。

2. GET 书签可收藏，POST 为书签不可收藏。

3. GET 能被缓存，POST 不能缓存 。

4. GET 编码类型：`application/x-www-form-urlencode`，POST 编码类型`application/x-www-form-urlencoded` 或 `multipart/form-data`。为二进制数据使用多重编码。

5. GET 历史参数保留在浏览器历史中。POST 参数不会保存在浏览器历史中。

6. GET 对数据长度有限制，当发送数据时，GET 方法向 URL 添加数据；URL 的长度是受限制的（URL 的最大长度是 2048 个字符）。POST 无限制。GET 只允许 ASCII 字符。POST没有限制。也允许二进制数据。与 POST 相比，GET 的安全性较差，因为所发送的数据是 URL 的一部分。在发送密码或其他敏感信息时绝不要使用 GET。

7. GET 请求将发送的数据放到 url 中，以 `?` 开头的，使用 `key=value` 的形式表示键值对，多个键值对使用 `&` 连接。例如：`http://www.text.com/index.html?name=curry&age=25`。POST 请求将数据放到请求体中，编码方式多样，如 json，二进制数据等。

8. POST 比 GET 更安全，因为参数不会被保存在浏览器历史或 web 服务器日志中。GET的数据在 URL 中对所有人都是可见的。POST的数据不会显示在 URL 中。


### 2. 其他区别

1. 根据 http 规范，GET 请求用于信息的获取，同时应该是安全和幂等性的。
   - 安全指的是 GET 请求只用于数据的获取，而不对数据进行修改。即 GET 请求是没有副作用，对应到数据库的增删改查操作就是查询操作。
   - 幂等性，指的是对同一个 url 进行多次请求，返回的结果应该是一样的。

2. 根据 http 规范，POST 表示可能修改变服务器上的资源的请求。
   - 改变服务器上面的资源，指的是增加、编辑等操作。即 POST 请求是具有副作用的。
   - POST 请求是非幂等性的。

### 3. 参考资料

1. [HTTP｜GET 和 POST 区别？网上多数答案都是错的！](https://www.jianshu.com/p/fd67b576365d)

2. [GET 和 POST 到底有什么区别？](https://www.zhihu.com/question/28586791)

3. [HTTP中GET与POST的区别，99 %的人都理解错了](https://zhuanlan.zhihu.com/p/114846445)

4. [浅谈HTTP中Get与Post的区别](https://www.cnblogs.com/hyddd/archive/2009/03/31/1426026.html)

5. [都9102年了，还问GET和POST的区别](https://segmentfault.com/a/1190000018129846)
 
