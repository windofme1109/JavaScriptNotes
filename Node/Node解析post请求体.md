# Node 解析 post 请求体

## 1. 参考资料

1. [Node中POST请求的数据解析](https://blog.csdn.net/wu_xianqiang/article/details/92693007)
1. [用了这么久HTTP, 你是否了解Content-Length和Transfer-Encoding ?](https://blog.piaoruiqing.com/2019/09/08/do-you-know-content-length/)
1. [ 用了这么久HTTP, 你是否了解Content-Length? - 博客园](https://www.cnblogs.com/fundebug/p/understand-http-content-length.html)

## 2. 基本说明

1. Node 的 `http` 模块只对 HTTP 请求报文的头部进行了解析，然后触发 `request` 事件。如果请求中还带有内容（content）部分（如 POST 请求，它具有报头和内容），内容部分需要我们自行接收和解析。

2. 通过报头的 `Transfer-Encoding` 或 `Content-Length` 即可判断请求中是否带有内容。
   
   字段名称|含义
   :---:|:---:
   Transfer-Encoding|指定报文主体的传输编码方式
   Content-Length|报文主体的大小

3. `Content-Length` 这个头字段比较常见，用于描述 `HTTP` 消息实体的传输长度，和 `Content-Type` 息息相关。请求头和响应头都可以看到这个字段。

4. `Transfer-Encoding` 的基本说明：
   - 这个头字段用来确定通信内容的传输方式，如果指定为 chunk 表示把大资源分为多个小块进行传输。这个字段在请求头或者响应头中都可以看到。
   - 通常情况下静态资源等小文件传输时可以指定 `Content-Length` 告知通信双方文件大小，而当传输资源无法确定大小是可以指定 `Transfer-Encoding` 属性进行传输。通信双方无需知道文件大小，这样可以节省内存空间。
   - `Transfer-Encoding` 属性和 `Content-Length` 冲突，不应该同时指定，当指定 `Transfer-Encoding` 头信息后，不应该再指定 `Content-Length` 属性，且 `Transfer-Encoding` 的传输长度会比 `Content-Length` 大，如果同时指定会导致接收数据的不完整性。
   - chunk的格式：包含块头和数据体，二者以及块之间以 CLRF 符号分割，块头包含一个十六进制的数字表明块大小，最后一个块形式为块头为 0 长度的，不带数据体的块，以此表示结束。

## 3. 和 post 请求相关的几种 Content-Type

1. `Content-Type` 指 http/https 发送信息至服务器时的内容编码类型。`Content-Type` 用于表明发送数据流的类型，服务器根据编码类型使用特定的解析方式，获取数据流中的数据。

2. 一般情况下，我们发送 post 请求，是要向服务器传送数据的，或者是文本数据，或者是二进制数据，因此需要指定 `Content-Type`。post 请求常用的 `Content-Type` 及其使用场景如下表所示：
   
   使用场景|`Content-Type`
   :---:|:---:
   访问接口，例如增删改查|`application/json`
   发送表单数据|`application/x-www-form-urlencoded`
   上传文件|`multipart/form-data`

## 4. 解析 post 请求体

1. 当前端发送一个 post 请求时，请求报文的内容部分会触发流（stream）中的 data 事件，我们只需以流的方式处理即可，在收到所有的报文内容后，即请求流结束的时候，会触发 end 事件，我们可以在 end 事件的处理函数中去处理收到的数据。

2. 在监听 data 事件的时候，传入事件处理函数的数据类型是 Buffer，因此我们不能使用 `+=` 的形式拼装数据，这样会乱码的。

3. 针对不同的 post 请求的 `Content-Type`，我们具体解析的方法也不一样。

4. 首先写一个方法，用来判断请求中是否存在报文主体：
   ```js
      function hasBody(req: IncomingMessage) {
          return 'transfer-encoding' in req.headers || 'content-length' in req.headers;
      }
   ```
5. 我们这里使用原生的 http 模块中的 createServer 方法创建一个 http 服务，因此在 createServer 方法的回调函数中接收两个参数：请求对象 —— req、响应对象 —— res。基本的解析过程如下所示：
   ```js
      function parseBody(req: IncomingMessage, res: ServerResponse) {
            const result: Array<Buffer> = [];

            if (hasBody(req)) {
                req.on('data', function(chunk) {
                    result.push(chunk);
                });

                req.on('end', function() {
                    const postBodyData = Buffer.concat(result).toString();
                });
            }


        }
   ```
6. 上面就是通过处理流的方式来解析报文主题的基本方式。注意，这是使用原生 Node 的方式进行解析。而如果使用 Koa 的话，注意：`ctx.req` 是原生的 Node 的 http 中的 `request` 对象，而 `ctx.res` 则是原生的 Node 的 http 中的 `response` 对象，所以需要使用 `ctx.req` 的 `on` 方法去监听 `data` 事件和 `end` 事件。`ctx.request` 是 `Koa` 自己的 `request` 对象，本身没有 `on` 方法。

7. 在 end 事件的处理函数中，我们根据不同的 `Content-Type` 对流数据进行不同的处理。

### 1. 表单数据 —— `application/x-www-form-urlencoded`

1. 在前端页面，我们使用表单提交一个 post 请求，页面的代码如下所示：
   ```html
      <form action="/submit" method="post">
          <label for="username">用户名:</label>
          <input type="text" id="username">
          <br>
          <label for="password">密码:</label>
          <input type="password" id="password">
          <br>
          <label for="email">邮箱:</label>
          <input type="email" id="email">
          <br>
          <input type="submit">
      </form>
   ```
2. 表单请求的 `Content-Type` 是 `application/x-www-form-urlencoded`，如下所示：
   ```
      content-type: application/x-www-form-urlencoded
   ```
3. 因此，我们首先写一个方法，将 `Content-Type` 从请求头中解析出来：
   ```js
      function mimeType(req: IncomingMessage) {
         // 从 headers 中取出 content-type
         const str = req.headers['content-type'] || '';
         // 因为 content-type 出了指定报文类型，还会指定编码、边界等信息，content-type 格式如下：
         // Content-Type：type/subtype ;parameter
         // 
         // type：主类型，任意的字符串，如 text，如果是 * 号代表所有；
         // subtype：子类型，任意的字符串，如 html，如果是 * 号代表所有，用“/”与主类型隔开；
         // parameter：可选参数，如charset，boundary等
         //
         // 如：'content-type': 'application/x-www-form-urlencoded;charset=UTF-8' 或者 'Content-Type': 'multipart/form-data; boundary=----WebKitFormBoundary4Hsing01Izo2AHqv'
         // 所以这里使用 split 方法进行切分，取出第一项的内容，就是我们需要的 content-type
         return str.split(';')[0];
      }
   ```
4. 表单提交的 post 请求的报文格式和 get 请求中的查询字符串相同，如下所示：
   ```
      name=jack&password=12345678&email=apple%40176.com
   ```
   其中 `key` 和 `value` 都是经过 `uri` 编码后再使用 `=` 相连，多个 `key=value` 使用 `&` 进行分隔。

5. 我们可以自己解析这个表单数据，基本思路就是先拆分再转码。为了方便起见，我们这里使用 node 的一个模块：`querystring` 来解析表单数据。

6. 解析代码如下：
   ```js
      function parseBody(req: IncomingMessage, res: ServerResponse) {
          const result: Array<Buffer> = [];
          const isFormData = mimeType(req) === 'application/www-x-form-urlencoded';
          if (hasBody(req) && isFormData) {
              req.on('data', function(chunk) {
                  result.push(chunk);
              });

              req.on('end', function() {
                  const postBodyData = Buffer.concat(result).toString();
            
                  const bodyData = querystring.parse(postBodyData);
            
              });
          }


      }
   ```
7. 使用 querystring 模块中的 parse 方法来解析表单数据，对key 和 value 解码并将其转换为对象。

8. **注意**，Node 官方已经不推荐再新的代码中使用 `querystring` 这个模块的，官方已经将其状态标记为 `legacy`，即废弃状态。官方推荐我们使用 `url` 模块中的 `URLSearchParams` API 取代 `querystring`。

9. 当然，我们可以使用第三方模块 `qs` 实现解析和创建查询字符串。
   - 安装 `qs`：`npm install qs`
   - 解析查询字符串：
     ```js
        var qs = require('qs');
        var assert = require('assert');

        var obj = qs.parse('a=c');
        assert.deepEqual(obj, { a: 'c' });

        
     ```
   - 生成查询字符串：
     ```js
        var qs = require('qs');
        var assert = require('assert');
        var str = qs.stringify(obj);
        assert.equal(str, 'a=c');
     ```

### 2. json 数据 —— `application/json`

1. 在我们访问一些后端接口的时候，接口要求我们传递的请求参数是 json 格式。那么此时的 `Content-Type` 是 `application/json`，请求头中的 `Content-Type` 还可能携带编码信息：
   ```
      Content-Type: application/json;charset=utf-8
   ```

2. 我们使用 axios 发送 post 请求时，在配置项指定了 data 属性后，axios 会自动指定 `Content-Type` 为 `application/json`，同时将 data 字段的属性值 —— 一个纯对象（plain object）转换成 json，作为请求体随着请求发送到后端。如下所示：
   ```js
      axios.post('\login', {
          data: {
              username: 'jack',
              password: '123456'
          }
      })
   ```
   将 data 的属性值转换为 json 字符串：
   ```json
      {
          "username": "jack",
           "password": "123456"
      }
   ```
3. 对于 json 数据，我们使用 JSON.parse 方法进行解析。
   ```js
      function parseBody(req: IncomingMessage, res: ServerResponse) {
          const result: Array<Buffer> = [];
          const isFormData = mimeType(req) === 'application/www-x-form-urlencoded';
          const isJson = mimeType(req) === 'application/json';
          if (hasBody(req)) {
              req.on('data', function(chunk) {
                  result.push(chunk);
              });

              req.on('end', function() {
                  const postBodyData = Buffer.concat(result).toString();
                  let bodyData = {};
                  if (isFormData) {
                      bodyData = querystring.parse(postBodyData);
                  } else if (isJson) {
                      bodyData = JSON.parse(postBodyData)
                  }

              });
          }


      }
   ```

### 3. 上传文件 —— `multipart/form-data`

1. 如果在页面中使用表单实现文件的上传，页面的代码如下所示：
   ```html
      <form action="/upload" method="post" enctype="multipart/form-data">
           <input type="file" name="avatar" id="avatar">
           <input type="submit" />
      </form>
   ```
2. 默认的上传文件使用的请求方法是 post，而请求头中的 `Content-Type` 字段值为 `multipart/form-data`，在 `Content-Type` 中可能还附带如下所示的内容分隔符：
   ```
      Content-Type: multipart/form-data; boundary=----WebKitFormBoundary4Hsing01Izo2AHqv
   ```
3. 上传文件的时候是要区分文本文件和二进制文件，文本文件是要使用 utf-8 编码，文本文件包括：HTML、CSS、JavaScript 等。而二进制文件是要使用 binary 编码，包括：图片、视频、音频等。
