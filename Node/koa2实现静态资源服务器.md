# koa2实现静态资源服务器

## 1. 参考资料

1. [原生koa2实现静态资源服务器](https://chenshenhai.github.io/koa2-note/note/static/server.html)

2. [原生koa2实现静态资源服务器](https://chenshenhai.github.io/koa2-note/note/static/server.html)

3. [对静态文件中间件koa-static的一些理解](https://blog.csdn.net/qq_43624878/article/details/107739956)

4. [使用Node.js原生API写一个web服务器](https://segmentfault.com/a/1190000037604771)

5. [手写koa-static源码，深入理解静态服务器原理](https://segmentfault.com/a/1190000038397728)

## 2. 基本思路

1. 发起一个 http 请求访问 web 服务器上的静态资源，一般响应结果有三种情况：
   - 访问文件，例如 js，css，png，jpg，gif
   - 找不到资源，抛出404错误
   - 访问静态目录
   

2. 我们这里主要处理前两种情况。因为一般情况下，我们访问静态资源，主要是访问文件，如果没有找到文件，就返回 404。

3. 通常情况下，我们需要区分这个请求是请求静态资源还是访问接口，有以下几种方式进行区分：
   - 根据请求路径的开头进行区分。一般情况下，我们都是将静态资源统一放到一个目录下，例如 `public` 或者 `static` 目录下。如果请求的路径是以 `public` 或者 `static` 开头，那么我们就可以判定这个请求访问的是静态资源。
   - 判断请求的是不是文件。一般来说，请求静态资源都是请求文件，而且需要明确指定文件的类型，这个类型通过后缀来区分。所以我们可以通过请求路径是否有后缀（extension）来区分。
   - 直接判断当前请求的路径在文件系统中是否存在。一般来说，请求的静态资源都是文件，那么我们可以直接去判断这个路径指定的文件在文件系统中是否存在，如果存在，就把这次请求作为访问静态资源的请求，继续处理，否则就当作访问接口的请求处理。

4. 前两种方式需要对请求路径进行处理后才能进行判断，而最后一种方式需要将相对路径转换为绝对路径后才能进行判断。

5. 这里使用第二种方式。以请求 js 文件为例，请求的 url 是：`http://www.aaa.com/public/javascripts/api.js` ，url 的最后一定是请求的静态资源的名称，这个名称还包括了扩展名。因此，我们需要提取出这个文件名，以及文件的扩展名。我们需要根据扩展名，在响应中设置正确的 Content-Type，只有正确设置 Content-Type，浏览器才能根据 Content-Type 做对应的处理，如加载图片、解析 css、js 等。

6. 经过判断如果请求的是静态资源，那么我们使用流的方式，读取静态资源，设置好响应头的部分字段，如 Content-Type，将流写入响应中，因为 Node 中，原生的 http 响应 response 也是可写流。当然，也可以将流写入 koa 上下文对象 ctx 的 body 属性中。

7. 通过上面的分析，使用 koa 实现的静态资源服务的步骤如下：
   - 指定静态资源的根目录
   - 对请求路径判断，判断其是不是访问静态资源的路径
   - 对请求的路径进行解析，得到文件名和扩展名
   - 对路径进行处理，使得可以通过这个路径生成可读流
   - 根据扩展名，将响应头中的 Content-Type 设置为指定类型
   - 将可读流写入响应中，可以使用 pipe 方法或直接将可读流赋值给 ctx.body。




## 3. 代码实现 - 简易版 - 不配置静态资源目录

1. 这里实现一个简易版的获得静态资源服务。就是不配置静态资源目录，默认的静态资源目录是 public。所以，请求静态资源的路径应该是以 public 开头。

2. 基本的中间件形式：
   ```js
      import path from 'path';
      import fs from 'fs';

      export default function fileServer () {
          return async function (ctx, next) {

          }
      }
   ```
3. 定义 mine-type：
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
4. 判断这个请求是请求静态资源还是访问接口，这里采用判断扩展名的方式：
   ```js
      const resourcePath = ctx.path;
        
        const pathObj = path.parse(resourcePath);
        let { base, ext, name, dir } = pathObj;
        if (ext === '') {
            // ext 是文件的扩展名，如果 ext 不存在，表示这次请求并不是请求资源，因此直接进入下一个中间件进行处理
            await next();
        } else {
            const rootPath = path.resolve('.');
            const regex = /(\.*)\/?([a-zA-Z0-9\-_]+)\/(.+)/;
            // 去掉 public 前面的多余路径分隔符
            const newPath = resourcePath.replace(regex, `$2/$3`);

            if (newPath.startsWith('public/')) {
                // 进行路径拼接
                const staticResourcePath = path.resolve(rootPath, newPath);
                // 读取资源，并返回响应
                response(ctx, staticResourcePath);

            } else {
                ctx.status = 404;
            }
        }
   ```
   - 使用 `path.parse` 方法对路径字符串进行解析，得到一个对象，对象的属性是路径的重要的元素，包括：
     - `dir` 路径
     - `root` 根路径
     - `base` 完整的文件名称（包含扩展名）
     - `name` 文件名（不包含扩展名）
     - `ext` 文件扩展名
   - 举个例子：
     ```js
         path.parse('/home/user/dir/file.txt');
         
         // { 
         //     root: '/',
         //     dir: '/home/user/dir',
         //     base: 'file.txt',
         //     ext: '.txt',
         //     name: 'file'
         // }
      ```
   - 通过判断 ext 是否为空，确定请求的是不是静态资源，如果不是，就进入下一个中间件。 
   - 使用 `path.resolve` 方法生成根路径。默认根路径就是当前的工作路径，即执行 node 命令所在的目录。 什么是工作路径，比如说，`index.js` 的路径是：`d:/my-project/src/my-components/upload/upload-list/index.js`。在 `my-project` 下执行：`node ./src/my-components/upload/upload-list/index.js`。那么此时的 node 的工作路径就是：`d:/my-project`，而不是 `index.js` 所在的路径。
   - 使用正则去掉路径前的其他字符，如 `/` 和 `.` 等。保证路径是以英文字母开头。
   - 请求静态资源的路径都是以 `public/` 开头，如果如果不是以 `public/` 开头，我们就直接返回 `404`，防止外部使用者探知服务器目录结构。
   - 将根路径和资源的相对路径进行拼接，得到一个绝对路径。
   - 调用 response 方法读取资源，并返回响应。

5. 读取资源，并返回响应 —— response 方法的实现：
   ```js
      function response(ctx: Koa.Context, resourcePath: string) {
          if (exist(resourcePath)) {
              const resource = fs.createReadStream(resourcePath);
              // 得到文件的扩展名（不带点的）
              const fileExt = getExt(ctx.path).toLowerCase();
              // ctx.type = fileExt;
              ctx.type = mimeType[fileExt] ? mimeType[fileExt] : '';
              ctx.body = resource;

          } else {
              ctx.status = 404;
          }
      }
   ```
   - 通过 exist 方法判断 resourcePath 指定的资源是否存在。如果不存在，直接抛出 404。
   - 使用 fs.createReadStream 创建一个可读流。
   - 使用 getExt 得到文件的扩展名（不带点的）。
   - 根据扩展名，在 mimeType 对象中找到对应的 Content-Type，并将 Content-Type 赋值给 ctx.type，ctx.type 内部调用了原生的 res.setHeader 方法，指定了响应头字段：Content-Type。所以直接赋值给 ctx.type，就相当于设置了响应头的 Content-Type。
   - 最后将可读流赋值给 ctx.body。koa 内部，会对 ctx.body 的类型进行判断，如果 是流（可读流），那么就调用可读流的 pipe 方法，将可读流写入原生的 res 中。因为原生的 res 是可写流。

6. 获得文件扩展名的方法 getExt 实现如下：
   ```js
      function getExt(filename:string) {
          const arr = filename.split('.');
          return arr[arr.length - 1];
      }
   ```
7. 判断一个路径指定的资源是否存在的方法 exist 的实现如下：
   ```js
      function exist(path:string) {
          return fs.existsSync(path);
      }
   ```
8. **优化**：实际上，拿到资源的扩展名以后，我们不用根据扩展名去 mimeType 对象中获得对应的 Content-Type。直接将扩展名赋值给 ctx.type 即可。因为 koa 内部会根据扩展名去设置对应的 Content-Type。下面是 koa 中 response.js 中 type 属性的源码：
   ```js
      set type (type) {
          type = getType(type)
          if (type) {
            this.set('Content-Type', type)
          } else {
            this.remove('Content-Type')
          }
      }
   ```
   getType 方法是 `cache-content-type` 提供的方法。这个方法可以根据文件类型获得其对应的 Content-Type。

## 4. 代码实现 - 进阶版 - 配置静态资源目录

1. 指定哪个目录为存放静态资源的目录。如 public、static 等。

2. 如果配置了静态资源目录，那么我们在前端书写请求静态资源的路径时，就不需要写这个静态资源的目录了。如：指定的静态资源的目录是：`public`，css 资源的路径是：`public/assets/css/style.css`，那么前端的请求路径是：`assets/css/style.css`。这样可以有效防止外部使用者探知服务器目录结构。

3. 收到静态资源请求后，我们将配置的目录与请求的静态资源路径进行拼接，得到完整的资源路径。后面就可根据这个路径进行创建可读流等一系列操作，最终返回前端请求的静态资源。

4. 示例代码 -- 拼接路径：
   ```js
      export default function fileServer(root = '.', options) {
    return async function (ctx, next){}
   ```
   
5. 示例代码：
   ```js

   ```