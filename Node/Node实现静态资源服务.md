# 原生 Node 实现静态资源服务

## 1. 参考资料

1. [【Node】搭建一个静态资源服务器](https://segmentfault.com/a/1190000019222794)

2. [使用Node.js原生API写一个web服务器](https://segmentfault.com/a/1190000037604771)

3. [【Node】搭建一个静态资源服务器](https://juejin.cn/post/6844903701446918151)

4. [静态资源为何要使用cdn](https://juejin.cn/post/6983909912993071112)

## 2. 静态资源服务

1. 静态资源，顾名思义，就是静止的、不会变化的资源，即通过不同的请求访问得到的数据都是相同的文件。一般来说，可以在浏览器直接打开这种文件。常见的静态资源包括 html、css、js、图片等，都是静态资源。

2. 一般来说，我们使用 Nginx 服务来托管静态资源。

3. 与静态资源相对，动态资源指的是由服务器动态生成的文件，不同的请求会生成不同的资源文件，如网站中的文件（asp、jsp、php、perl、cgi）、API 接口、数据库交互请求等，都是动态资源。

## 3. Node 与静态资源服务

1. Node 的 http 模块可以启动一个 Web 服务，因此我们可以使这个 Web 服务托管静态资源。

2. 托管静态资源，主要由以下几个方面：
   - 访问根路径 `/`，返回 index..html。
   - 访问指定目录下的 css、js、图片等，能正确返回资源。
   - 如果访问资源不存在，返回 404。

3. 这里使用原生的 Node 模块实现静态资源服务。包括 http、fs、path、url 等模块。

### 1. 启动一个 Web 服务

1. 原生 Node 通过 http 模块的 createServer 启动一个 Web 服务。

2. createServer 接收一个回调函数作为参数，在这个回调中处理请求。

3. createServer 返回一个 server 对象，通过调用 server 对象的 listen 方法，监听一个端口，这样才算真正启动一个 Web 服务。

#### 1. createServer 详解

1. createServer 详解：
   - `http.createServer([options][, requestListener])`
   - 参数：options 配置项
     - IncomingMessage http.IncomingMessage
     - ServerResponse http.ServerResponse 
     - insecureHTTPParser  boolean 
     - maxHeaderSize number 
   - 参数：`requestListener` 请求处理函数
   - 返回值：`http.Server` 实例。

#### 2. `http.Server` 实例

1. `http.Server` 实例的一些方法和事件：
   - connect 事件
   - connection 事件
   - request 事件
   - listen 方法

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
      server.on('request', (request, res) => {
      
      });

      server.listen(8000);
   ```

#### 4. http.IncomingMessage 

1. method
2. url
3. headers
4. rawHeaders


#### 5. http.ServerResponse

1. setHeader
2. setHead
3. write
4. end

### 2. 区分路径

1. 通过 req.url 中进行区分。

2. 通过 url 模块的 parse 方法来解析 req.url。

### 3. 获取并返回静态资源

### 4. 处理资源不存在的情况

