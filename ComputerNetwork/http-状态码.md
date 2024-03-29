<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [http - 状态码](#http---%E7%8A%B6%E6%80%81%E7%A0%81)
  - [1. 状态码基本说明](#1-%E7%8A%B6%E6%80%81%E7%A0%81%E5%9F%BA%E6%9C%AC%E8%AF%B4%E6%98%8E)
  - [2. 2XX](#2-2xx)
  - [3. 3XX](#3-3xx)
  - [4. 4XX](#4-4xx)
  - [5. 5XX](#5-5xx)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# http - 状态码

## 1. 状态码基本说明 

1. 状态码表示了响应的一个状态，可以让我们清晰的了解到这一次请求是成功还是失败，如果失败的话，是什么原因导致的，当然状态码也是用于传达语义的。如果胡乱使用状态码，那么它存在的意义就没有了。

2. 参考资料
   - [常见的HTTP状态码(HTTP Status Code)](https://www.cnblogs.com/gitnull/p/9532129.html)
   - [HTTP状态码详解](https://tool.oschina.net/commons?type=5)
   - [常见的HTTP状态码](https://www.cnblogs.com/xflonga/p/9368993.html)

## 2. 2XX 

1. 以 2 开头的状态码表示成功。

2. `200` OK，表示从客户端发来的请求在服务器端被正确处理。

3. `204` No content，表示请求成功，但响应报文不含实体的主体部分。

4. `205` Reset Content，表示请求成功，但响应报文不含实体的主体部分，但是与 204 响应不同在于要求请求方重置内容。

5. `206` Partial Content，进行范围请求，该请求必须包含 Range 头信息来指示客户端希望得到的内容范围，并且可能包含 If-Range 来作为请求条件。响应头包含 content-range。用于二进制文件的断点续传或下载。

## 3. 3XX

1. 以 3 开头的状态码表示重定向。

2. `301` moved permanently，永久性重定向，表示资源已被分配了新的 URL。

3. `302` found，临时性重定向，表示资源临时被分配了新的 URL。

4. `303` see other，表示资源存在着另一个 URL，应使用 GET 方法获取资源。

5. `304` not modified，表示服务器允许访问资源，但因发生请求未满足条件的情况。

6. `307` temporary redirect，临时重定向，和302含义类似，但是期望客户端保持请求方法不变向新的地址发出请求。

## 4. 4XX 

1. 以 4 开头的状态码表示客户端错误。不是认证信息有问题，就是表示格式或HTTP库本身有问题。客户端需要自行改正。

2. `400` bad request，请求报文存在语法错误。

3. `401` unauthorized，表示发送的请求需要有通过 HTTP 认证的认证信息。

4. `403` forbidden，表示对请求资源的访问被服务器拒绝。

5. `404` not found，表示在服务器上没有找到请求的资源。

## 5. 5XX 

1. 以 5 开头的状态码表示服务器错误。

2. `500` internal server error，表示服务器端在执行请求时发生了错误。

3. `501` Not Implemented，表示服务器不支持当前请求所需要的某个功能。

4. `502` Bad Gateway，只有 HTTP 代理会发送这个响应代码。它表明代理方面出现问题，或者代理与上行服务器之间出现问题，而不是上行服务器本身有问题。若代理根本无法访问上行服务器，响应代码将是 504。

4. `503` service unavailable，表明服务器暂时处于超负载或正在停机维护，无法处理请求。