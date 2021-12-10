# 原生 Node 实现静态资源服务

## 1. 参考资料

1. [【Node】搭建一个静态资源服务器](https://segmentfault.com/a/1190000019222794)

2. [使用Node.js原生API写一个web服务器](https://segmentfault.com/a/1190000037604771)

3. [【Node】搭建一个静态资源服务器](https://juejin.cn/post/6844903701446918151)

4. [使用Node.js搭建静态资源服务器](https://www.cnblogs.com/SheilaSun/p/7271883.html)

5. [静态资源为何要使用cdn](https://juejin.cn/post/6983909912993071112)

## 2. 静态资源服务

1. 静态资源，顾名思义，就是静止的、不会变化的资源，即通过不同的请求访问得到的数据都是相同的文件。一般来说，可以在浏览器直接打开这种文件。常见的静态资源包括 html、css、js、图片等，都是静态资源。

2. 一般来说，我们使用 Nginx 服务来托管静态资源。

3. 与静态资源相对，动态资源指的是由服务器动态生成的文件，不同的请求会生成不同的资源文件，如网站中的文件（asp、jsp、php、perl、cgi）、API 接口、数据库交互请求等，都是动态资源。

## 3. Node 与静态资源服务

1. Node 的 http 模块可以启动一个 Web 服务，因此我们可以使这个 Web 服务托管静态资源。

2. 托管静态资源，主要由以下几个方面：
   - 访问根路径 `/`，返回 index.html。
   - 访问指定目录下的 css、js、图片等，能正确返回资源。
   - 如果访问资源不存在，返回 404。

3. 这里使用原生的 Node 模块实现静态资源服务。包括 http、fs、path、url 等模块。

### 1. 启动一个 Web 服务

1. 原生 Node 通过 http 模块的 createServer 启动一个 Web 服务。

2. createServer 接收一个回调函数作为参数，在这个回调中处理请求。

3. createServer 返回一个 server 对象，通过调用 server 对象的 listen 方法，监听一个端口，这样才算真正启动一个 Web 服务。

#### 1. createServer 详解

1. createServer
   - `http.createServer([options][, requestListener])`
   - 参数：options 配置项
     - IncomingMessage：指定使用 http.IncomingMessage 类。当扩展原始的 IncomingMessage 时使用这个配置项。默认就是 http.IncomingMessage 类。createServer 的回调或者 request 事件的回调的的第一个参数 req 就是一个 http.IncomingMessage 类，可以访问请求的一些相关信息。 
     - ServerResponse：指定使用 http.ServerResponse 类。当扩展原始的 ServerResponse 时使用这个配置项。默认就是 http.ServerResponse 类。 createServer 的回调或者 request 事件的回调的的第二个参数 res 就是一个 http.ServerResponse 类，可以对响应进行一些设置。如设置响应数据、设置响应头等信息。
     - insecureHTTPParser：布尔值，当设置为 true 的时候，使用一个接受无效的 http 头的不安全的 http 解析器。应该避免使用不安全的解析器。默认是 false。
     - maxHeaderSize 数字，用来覆盖 `--max-http-header-size` 的值，`--max-http-header-size` 表示服务器能接收的最大的请求头的大小，单位是字节，默认是 16384，16 kb。
   - 参数：`requestListener` 请求处理函数
   - 返回值：`http.Server` 实例。

2. requestListener 函数会自动地被添加到 requset 事件 中，作为 request 事件的处理函数。

#### 2. `http.Server` 实例

1. `http.Server` 类继承自 `net.Server`。

2. `http.Server` 类提供一些和 http 服务相关的方法和事件。

3. createServer 方法的返回值就是 `http.Server` 实例。

##### 1. `connection` 事件

1. 当一个新的 `TCP` 流建立的时候触发这个事件。

2. 回调函数接收一个 `socket` 对象作为参数。

3. 用户还可以显式地触发此事件，以将连接注入 HTTP 服务器。在这种情况下，可以向回调函数中传递任何双工流。

##### 2. `request` 事件

1. 收到 http 请求时触发。在长连接的情况下（请求头中 connection 字段为 keep-alive），可能在一次连接中有多个请求。

2. 回调函数接收 req 和 res 参数。req 参数是 http.IncomingMessage 实例，而 res 则是 http.ServerResponse 实例。

##### 3. listen 方法

1. 启动一个 http 服务，并监听 tcp 连接。这个方法和 net.Server 实例中的 server.listen 一样。

2. 函数调用形式：`server.listen([port[, host[, backlog]]][, callback])`

3. 参数：
   - port：端口号，http 服务的端口号。
   - host：主机名，http 服务的主机名。
   - backlog：listen() 函数的公共参数
   - callback：回调函数，当成功启动 http 服务时执行。

4. listen 方法可以只接收一个 port 参数，其余参数都可省略。

5. 不指定 host 参数，默认的主机名就是：`127.0.0.1`。

6. 可以只指定 port 和 callback 两个参数：`server.listen(port, () => {})`。

#### 3. 启动一个 http 服务

1. 启动一个 http 服务 - 设置 createServer 的回调：
   ```js
      const http = require('http');
      const server = http.createServer(function (req, res) {


       })

       server.listen(10001, function () {
            console.log('server is running');
       });

   ```
2. 启动一个 http 服务 - 监听 request 事件：
   ```js
      const http = require('http');

      const server = http.createServer();

      // 监听 request 事件
      server.on('request', (req, res) => {
      
      });

      server.listen(8000);
   ```

#### 4. http.IncomingMessage 

1. IncomingMessage 对象由 http.Server 或http.ClientRequest 创建，并分别作为第一个参数传递给 `request` 和 `response` 事件的回调函数。它可用于访问请求头、请求数据等。

2. http.IncomingMessage 继承自 stream.Readable，因此 http.IncomingMessage 是一个可读流。解析 http 请求头和请求体（payload）。

3. post 请求会触发流的 data 事件和 end 事件，因此需要 IncomingMessage 实例监听 data 事件和 end 事件来处理 post 请求体。

##### 1. method

1. 获得 http 请求方法。

##### 2. url

1. 请求的 url 字符串。只包含实际 HTTP 请求中存在的url。

2. 这个 url 只包含 path 部分，并不是包含协议、主机名、端口在内的完整 url。

##### 3. headers

1. 请求或者响应头部信息，是一个对象。

2. 键值对头部名称和值，头部字段的名字都是小写。

3. 头部对象示例：
   ```js
      // Prints something like:
      //
      // { 'user-agent': 'curl/7.22.0',
      //   host: '127.0.0.1:8000',
      //   accept: '*/*' }
      console.log(request.headers);
   ```
4. 重复的头部字段的处理：
   - `age`、`authorization`、 `content-length`、`content-type`、`etag`、 `expires`、`from` `host`、`if-modified-since`、`if-unmodified-since`、`last-modified`、`location`、`max-forwards`、`proxy-authorization`、`referer`、`retry-after`、`server` 或者 `user-agent`，这些字段重复，重复的字段会被丢弃，保留一个。
   - set-cookie 总是一个数组，重复的值会被加到数组中。
   - 重复的 cookie 字段，使用 `;`作为分隔符将重复的值和已有的值连接起来。
   - 其他的头部字段，会使用 `,` 作为分隔符将重复的值和已有的值连接。

##### 4. rawHeaders

1. 原始的请求/响应头列表，不对收到的请求头或响应头进行解析，保持其原始的模样。

2. 头部字段名称和值在同一个数组中。因此数组的奇数元素是字段名称，偶数元素是与字段相关的字段值。

3. 举例如下：
   ```js
      // Prints something like:
      //
      // [ 'user-agent',
      //   'this is invalid because there can be only one',
      //   'User-Agent',
      //   'curl/7.22.0',
      //   'Host',
      //   '127.0.0.1:8000',
      //   'ACCEPT',
      //   '*/*' ]
      console.log(request.rawHeaders);
   ```

#### 5. http.ServerResponse

1. http.ServerResponse 对象是由 http 服务器内部创建的，而不是由用户创建的。它作为第二个参数传递给`request` 事件的回调函数。

2. http.ServerResponse 继承自 http.OutgoingMessage，而 http.OutgoingMessage 继承自 stream.Writable，因此 http.ServerResponse 是可写流。可以对其使用可写流的相关操作。


##### 1. setHeader

1. 设置响应的头部信息。通过这个方法给响应头设置单独的头部字段信息。如果将要被发送的头部对象中已经存在着对应的字段，那么字段值会被新设置的替换。

2. `response.setHeader(name, value)`
   - name：头部字段名
   - value：头部字段值
   - 返回值 http.ServerResponse 对象

3. 基本示例：
   ```js
      response.setHeader('Content-Type', 'text/html');
      response.setHeader('Set-Cookie', ['type=ninja', 'language=javascript']);
   ```

5. 当使用 setHeader() 设置头部字段时，它们将与传递给 writeHead() 的任何头部字段合并，两个方法设置同名的字段，那么 writeHead() 设置的字段的优先级更高。
   ```js
      // Returns content-type = text/plain
      const server = http.createServer((req, res) => {
           res.setHeader('Content-Type', 'text/html');
           res.setHeader('X-Foo', 'bar');
           res.writeHead(200, { 'Content-Type': 'text/plain' });
           res.end('ok');
      });
   ```
##### 2. writeHead

1. 设置本次请求的响应头。

2. `writeHead(statusCode[, statusMessage][, headers])`

3. 参数：
   - statusCode：数字，本次响应的状态码
   - statusMessage：字符串，状态码的说明短语，如 `OK`、`NOT  FOUND` 等。
   - headers：对象或者数组，响应头信息。
   - 返回值是 http.ServerResponse 实例

4. headers 参数可以是一个数组，头部字段和字段值在一个数组中。所以，奇数元素是头部字段，偶数元素是字段值。格式和 request.rawHeaders 一样。

5. 返回值是 ServerResponse 实例的引用，因此可以链式调用 ServerResponse 的其他方法。

6. 示例代码：
   ```js
      const body = 'hello world';
       response
          .writeHead(200, {
             'Content-Length': Buffer.byteLength(body),
             'Content-Type': 'text/plain'
           })
           .end(body);
   ```
7. 通常来说，我们只调用一次 writeHead 方法来设置响应头，必须在调用 response.end 方法前调用 writeHead 方法。

8. 当使用 setHeader() 设置头部字段时，它们将与传递给 writeHead() 的任何头部字段合并，两个方法设置同名的字段，那么 writeHead() 设置的字段的优先级更高。

9. 只调用了 response.write 或者 response.end 方法，那么 Node 会自动计算响应头信息并调用这个 writeHead() 函数设置响应头。

##### 3. write

1. 给响应体添加数据。可以多次调用 write 方法来向响应体中添加连续的数据。

2. 因为 ServerResponse 是可写流，那么 write 方法实际上就是可写流的 write 方法。

3. `write(chunk[, encoding][, callback])`

4. 参数：
   - chunk：字符串或者 Buffer 实例，写入响应体的数据。
   - encoding 编码，默认是 utf8。
   - callback 回调函数。
   - 返回值是布尔值。

5. 在 http 模块中，当请求是 HEAD 请求时，将省略响应主体。类似地，204 和 304 响应也不得包括消息正文（响应体）。

6. chunk 可以是字符串或Buffer 实例。如果 chunk 是字符串，则第二个参数指定如何将其编码为字节流。刷新此数据块时将调用指定的 callback 函数。

7. 这是原始 HTTP 响应体，与可能使用的更高级别多部分正文编码无关。

8. 第一次调用 write() 方法，它将向客户端发送缓冲的头信息和响应体的第一个 chunk。第二次调用 write() 方法，Nodejs 假设数据将以流方式传输，那么会单独发送新数据。

##### 4. end

1. end 方法表示所有的响应头和响应体数据已经被发送，服务器应该考虑完成对这次请求的响应。

2. 在处理响应的末尾必须调用 end 方法，以结束本次响应。

3. `end([data[, encoding]][, callback])`

4. 因为 ServerResponse 是可写流，那么 end 方法实际上就是可写流的 end 方法，表示一次可写流的结束。

5. 参数：
   - data：字符串或者 Buffer 实例，向响应体中写入的数据。
   - encoding：编码，默认是 utf8。 
   - callback：回调函数，响应流结束时调用。

6. 如果指定了 data 参数，那么等同于调用：`response.write(data, encoding)` 然后调用 `response.end(callback)`。

### 2. 区分路径

1. 通过 req.url 中进行区分。

2. 通过 url 模块的 parse 方法来解析 req.url。

3. parse 方法解析一个 url，并返回一个 url 对象。url 对象包括：`hash`、`host`、`hostname`、`href`、`origin`、path、`pathname`、`port`、`search`、`searchParams` 等属性。

4. 从 url 对象中取出 pathname 属性（不包括查询字符串）。然后进行判断，如果 pathname 只包括 `/`，那么请求的就是根路径，我们返回的是 index.html，如果以 public 开头，那么说明访问的是静态资源，则我们就返回对应的静态资源。

5. 代码实现如下：
   ```js
      const server = http.createServer(function (req, res) {

           const requestUrlObj = parse(req.url);
           const {query, search, path, pathname} = requestUrlObj;
           console.log(`当前请求的路径是：${path}`);
           
           if (pathname === '/') {
               // some code
           } else if (pathname.startsWith('/public')) {
               // some code
           }
      })
   ```

### 3. 获取并返回静态资源

1. 拿到请求静态资源的路径后，我们需要将其转换成绝对路径。

2. process 模块的 cwd() 方法可以获得 Node 进程的当前的工作路径。可以理解为整个工作路径就是静态资源的根路径。

3. 由于静态资源的路径都是相对路径，所以我们需要将相对路径与 process.cwd() 获得的工作路径进行拼接，得到完整的静态资源的绝对路径。路径拼接使用 path 模块的 join 方法。
   ```js
      const myPath = require('path');
      const root = process.cwd();
      const resourcePath = myPath.join(root, pathname);
   ```
4. 得到静态资源的绝对路径后，需要读取静态资源。有两种方式读取，一种是使用 fs 模块的 readFile 方法，另外一种是使用流的方式。readFile 会将一次性将所有的文件内容写入内存中，然后供程序操作，如果是小文件没什么问题，如果是大文件，那么内存有可能会爆掉，因此这里不使用 readFile 读取静态资源。

5. 使用 fs 模块的 createReadStream 方法创建可读流。因为 Node 中 http response 是可写流，那么可以调用可读流的 pipe 方法，直接将数据写入响应中，无需考虑内存问题。

6. 在将数据写入响应流之前，需要设置响应头的 Content-Type，正确设置响应头的 Content-Type，有助于浏览器正确解析响应体。根据请求静态资源的类型设置 Content-Type。所以，要首先分离出静态资源的扩展名，根据这个扩展名拿到 mime-type，将 mime-type 设置为 Content-Type。

7. 常见的 mime-type：
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
   **注意**：mime-type 的种类非常多，我们这里只是列举了常见类型的 mime-type，实际开发中应该使用 `mime` 或者 `mime-type` 的第三方模块获得静态资源的 mime-type。

8. 代码示例：
   ```js
      const {name, base} = myPath.parse(pathname);
            // base 是包含扩展名的文件名
            // 从文件名中分离出扩展名
            const fileExt = getExt(base);

            // 拿到对应的 Content-Type
            const contentType = mimeType[fileExt] ?  mimeType[fileExt] : '';

            // 这里使用绝对路径创建可读流
            const readImageStream = fs.createReadStream(resourcePath);

            res.writeHead(200, {
                'Content-Type': contentType
            });

            readImageStream.pipe(res);
   ```
9. **注意**：创建可读流的时候，必须使用绝对路径。

### 4. 处理资源不存在的情况

1. 如果不是请求根路径 `/` 或者是请求静态资源（以 public 开头），那么统一处理为 404 的情况。

2. 示例代码：
   ```js
      if (pathname === '/') {
    
      } else if (pathname.startsWith('/public')) {

       // 
      } else if (path === '/favicon.ico') {
        //
      } else {
          res.writeHead(404);
          res.end('<h1>Not Found</h1>');
      }
   ```

### 5. 完整代码

1. 示例代码：
   ```js
      const http = require('http');
      const myPath = require('path');
      const fs = require('fs');
      const {URL, parse} = require('url');

      const server = http.createServer(function (req, res) {

           const requestUrlObj = parse(req.url);
           // console.log(requestUrlObj);

           const {query, search, path, pathname} = requestUrlObj;
           console.log(`当前请求的路径是：${path}`);
           // 得到 node 的进程的工作路径
           const root = process.cwd();
           const resourcePath = myPath.join(root, pathname);

           if (pathname === '/') {

               // 这里使用绝对路径创建可读流
               const readStream = fs.createReadStream(myPath.join(resourcePath, 'public/templates/index.html'));

               res.setHeader('Content-Type', 'text/html');
               res.writeHead(200);
               // res.write('<h1>Node Http Server Application</h1>');
               readStream.pipe(res);
           }
           else if (pathname.startsWith('/public')) {

               if (exist(resourcePath)) {
                   const {name, base} = myPath.parse(pathname);
                   // base 是包含扩展名的文件名
                   // 从文件名中分离出扩展名
                   const fileExt = getExt(base);

                   // 拿到对应的 Content-Type
                   const contentType = mimeType[fileExt] ?  mimeType[fileExt] : '';

                   // 这里使用绝对路径创建可读流
                   const readImageStream = fs.createReadStream(resourcePath);

                   res.writeHead(200, {
                       'Content-Type': contentType
                   });

                   readImageStream.pipe(res);
               } else {
                   res.writeHead(404);
                   res.end();
               }



           } else if (path === '/favicon.ico') {
               res.end();
           } else {
               res.writeHead(404);
               res.end('<h1>Not Found</h1>>');
           }
       })

       server.listen(10001, function () {
       console.log('server is running');
       });


       function getExt(filename) {
          const arr = filename.split('.');
          return arr[arr.length - 1];
       }


       function exist(path) {
           return fs.existsSync(path);
       }

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

### 6. 总结

1. 使用原生 Node 实现的静态资源服务，只支持访问根路径和指定目录的静态资源，不支持对目录的访问。

2. index.html 的内容是固定的，不能动态生成，后面可以考虑使用 html 模板来动态生成 html。

3. 静态资源不带缓存功能。后面可以考虑加上缓存信息。在响应头中加上 etag、last-modified 的信息。

4. 在实际开发中，静态资源服务还是使用 Nginx 实现，Nginx 能提供更加精细的控制，而且配置相对简单。