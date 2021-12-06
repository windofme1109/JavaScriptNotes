# koa2实现静态资源服务器

## 1. 参考资料

1. [原生koa2实现静态资源服务器](https://chenshenhai.github.io/koa2-note/note/static/server.html)

2. [原生koa2实现静态资源服务器](https://chenshenhai.github.io/koa2-note/note/static/server.html)

3. [对静态文件中间件koa-static的一些理解](https://blog.csdn.net/qq_43624878/article/details/107739956)
3. [使用Node.js原生API写一个web服务器](https://segmentfault.com/a/1190000037604771)
3. [手写koa-static源码，深入理解静态服务器原理](https://segmentfault.com/a/1190000038397728)
## 2. 基本思路

1. 发起一个 http 请求访问 web 服务器上的静态资源，一般响应结果有三种情况：
   - 访问文件，例如 js，css，png，jpg，gif
   - 找不到资源，抛出404错误
   - 访问静态目录
   

2. 我们这里主要处理前两种情况。因为一般情况下，我们访问静态资源，主要是访问文件，如果没有找到文件，就返回 404。

3. 通常情况下，我们都是将静态资源统一放到一个目录下，例如 `public` 或者 `static` 目录下。如果请求的路径是以 `public` 或者 `static` 开头，那么我们就可以判定这个请求访问的是静态资源。因此，我们需要手动指定静态资源的根目录。

4. 以请求 js 文件为例，请求的 url 是：`http://www.aaa.com/public/javascripts/api.js` ，url 的最后一定是请求的静态资源的名称，这个名称还包括了扩展名。因此，我们需要提取出这个文件名，以及文件的扩展名。我们需要根据扩展名，在响应中设置正确的 Content-Type，只有正确设置 Content-Type，浏览器才能根据 Content-Type 做对应的处理，如加载图片、解析 css、js 等。

5. 经过判断如果请求的是静态资源，那么我们使用流的方式，读取静态资源，设置好响应头的部分字段，如 Content-Type，将流写入响应中，因为 Node 中，原生的 http 响应 response 也是可写流。当然，也可以将流写入 koa 上下文对象 ctx 的 body 属性中。

6. 通过上面的分析，使用 koa 实现的静态资源服务的步骤如下：
   - 指定静态资源的根目录
   - 对请求路径判断。判断其是不是访问静态资源的路径
   - 对请求的路径进行解析，得到文件名和扩展名
   - 对路径进行处理，使得可以通过这个路径生成可读流
   - 根据扩展名，将响应头中的 Content-Type 设置为指定类型
   - 将可读流写入响应中，可以使用 pipe 方法或直接将可读流赋值给 ctx.body。




## 3. 代码实现

1. 基本的中间件形式：
   ```js
      import path from 'path';
      import fs from 'fs';

      export default function fileServer (root = '/', options) {
          return async function (ctx, next) {

          }
      }
   ```
   root 表示静态资源的根目录，没有配置的话，默认是 `/`。
2. 定义 mine-type：
   ```js
      const mimeType = {
            'css': 'text/css',
            'less': 'text/css',
            'gif': 'image/gif',
            'html': 'text/html',
            'ico': 'image/x-icon',
            'jpeg': 'image/jpeg',
            'jpg': 'image/jpeg',
            'js': 'text/javascript',
            'json': 'application/json',
            'pdf': 'application/pdf',
            'png': 'image/png',
            'svg': 'image/svg+xml',
            'swf': 'application/x-shockwave-flash',
            'tiff': 'image/tiff',
            'txt': 'text/plain',
            'wav': 'audio/x-wav',
            'wma': 'audio/x-ms-wma',
            'wmv': 'video/x-ms-wmv',
            'xml': 'text/xml'
      }
   ```
3. 