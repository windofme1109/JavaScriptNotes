# Node 解析 post 请求体

## 1. 参考资料

1. [Node中POST请求的数据解析](https://blog.csdn.net/wu_xianqiang/article/details/92693007)

2. [用了这么久HTTP, 你是否了解Content-Length和Transfer-Encoding ?](https://blog.piaoruiqing.com/2019/09/08/do-you-know-content-length/)

3. [ 用了这么久HTTP, 你是否了解Content-Length? - 博客园](https://www.cnblogs.com/fundebug/p/understand-http-content-length.html)

4. [一文了解文件上传全过程（1.8w字深度解析，进阶必备）](https://juejin.cn/post/6844904106658643982)

5. [上传文件multipart/form-data深入解析](https://juejin.cn/post/6844903810079391757)

6. [Form Data与Request Payload，你真的了解吗？](https://juejin.cn/post/6844904149809627149)

7. [JS : Blob() 转换二进制下载文件流实例](https://juejin.cn/post/6844903700222181384)

8. [1127-buffer定义及常用方法 & 进制转换 & base64转码规则](https://juejin.cn/post/6844903726654865416)

9. [Nodejs 进阶：核心模块 Buffer 常用 API 使用总结](https://juejin.cn/post/6960843000939282469)

10. [JS 的二进制家族：base64、File、Blob、ArrayBuffer 的关系](https://juejin.cn/post/6844904149809627149)

11. [前端下载普通文件与二进制流文件](https://juejin.cn/post/6878912072780873742)

12. [我妈都看得懂的 Buffer 基础](https://juejin.cn/post/6911487429471895560)

13. [[译]一篇帮你彻底弄懂NodeJs中的Buffer](https://juejin.cn/post/6844903688188723208)

14. [javascript处理二进制之ArrayBuffer](https://juejin.cn/post/6844903759064088590)

15. [JS中的二进制数据处理](https://juejin.cn/post/6954281377080541221)

16. [[1.3万字] 玩转前端二进制](https://juejin.cn/post/6846687590783909902)

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


#### 1. 前端请求报文的基本分析

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

4. 浏览器端传输文件是以二进制数据流的形式进行传输，将文件的内容放在 post 请求的报文主体中进行传输。而在 Node 端则是以 Buffer 的形式进行接收。

5. 我们在浏览器端上传一个文件，看看浏览器是如何传输二进制数据的，下面是上传一张图片的 post 请求报文主体的内容：
   ```
      ------WebKitFormBoundaryQRppr4oLReN6qlP0
      Content-Disposition: form-data; name="file"; filename="git.jpg"
       Content-Type: image/jpeg

      // 二进制数据
      ------WebKitFormBoundaryQRppr4oLReN6qlP0--
   ```
6. 而这次请求的 `Content-Type` 是：
   ```
      multipart/form-data; boundary=----WebKitFormBoundaryQRppr4oLReN6qlP0
   ```
7. 对这个报文主体进行分析，我们可以得到下面的结论：
   - 在 content-type 中，除了 `multipart/form-data`，还有 boundary 参数，表示分隔边界，其具体的值为：`----WebKitFormBoundaryGA3mAoqaBJlMKS05` 在请求报文中用来分隔内容的。
   - 报文主体的第一行：`------WebKitFormBoundaryQRppr4oLReN6qlP0`，它在边界分隔字符（boundary）的前面加上了 `--`。因此我们可以寻找符合 `--${boundary}` 这种形式的边界分隔符来寻找一个报文的开始位置。
   - 请求报文的最后一行：`------WebKitFormBoundaryGA3mAoqaBJlMKS05--`，它在边界分隔字符（boundary）的前后都加了 `--`。因此我们可以寻找符合 `--${boundary}--` 这种形式的边界分隔符来寻找一个报文结束位置。
   - 请求报文主体的第二行是 `Content-Disposition`。这个字段是 MIME 协议的一个扩展。一般来说，有两种用法，第一种用法是在常规的 HTTP 应答（http response）中，Content-Disposition 响应头指示回复的内容该以何种形式展示，是以内联的形式（即网页或者页面的一部分），还是以附件的形式下载并保存到本地。第二种用法是在 multipart/form-data 类型的应答消息体中，Content-Disposition 消息头可以被用在 multipart 消息体的子部分中，用来给出其对应字段的相关信息。各个子部分由在 Content-Type 中定义的分隔符分隔。用在消息体自身则无实际意义。因此，在 请求报文中出现的 `Content-Disposition` 的主要作用就是一个消息头，可以给出同上传的文件相关的信息。如 name 表示表单提交过程中设置的属性名（属性值就是上传的文件对象），filename 表示上传的文件名。
   - 请求报文主体的第三行是 `Content-Type`，指示上传文件的具体类型。我们这里表示上传的文件的类型是：`image/jpeg`，是图片类型。
   - 从第四行开始到倒数第二行（最后一行前）之间的内容就是文件的具体的内容，编码的形式是二进制。
    
8. 下面展示了一个在 `multipart/form-data` 格式的报文中使用Content-Disposition 请求报文的情况，来自  [MDN - Content-Disposition](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Disposition)
   ```
      POST /test.html HTTP/1.1
      Host: example.org
      Content-Type: multipart/form-data;boundary="boundary"

      --boundary
      Content-Disposition: form-data; name="field1"

      value1
      --boundary
      Content-Disposition: form-data; name="field2"; filename="example.txt"
      Content-Type: plain/text
      value2
      --boundary--
   ```

#### 2. Node 端收到的 Buffer 数据的基本分析

1. 还是上传同一张图片，我们在看一下 node 端接收的内容是什么样的：
    ```
       <Buffer 2d 2d 2d 2d 2d 2d 57 65 62 4b 69 74 46 6f 72 6d 42 6f 75 6e 64 61 72 79 51 52 70 70 72 34 6f 4c 52 65 4e 36 71 6c 50 30 0d 0a 43 6f 6e 74 65 6e 74 2d ... 26692 more bytes>
    ```
2. node 将二进制数据转为 Buffer 数据。而在 控制台中，Buffer 展示的数据形式是 16 进制，而 2d 则是 `-` 的 ASCII 值的16进制。所以，我们可以发现前 6 个 2d 表示的是 `-`，而请求报文主体中的起始行： `------WebKitFormBoundaryQRppr4oLReN6qlP0` 转换为 16 进制是：`[2d, 2d, 2d, 2d, 2d, 2d, 57, 65, 62, 4b, 69, 74, 46, 6f, 72, 6d, 42, 6f, 75, 6e, 64, 61, 72, 79, 51, 52, 70, 70, 72, 34, 6f, 4c, 52, 65, 4e, 36, 71, 6c, 50, 30]`，与 buffer 中的 前 40 个 16 进制字符相同。

3. 根据上面的分析，Buffer 中的 16 进制数和 post 请求报文主体内容是一一对应的，结合前面对报文主体的分析，我们可以根据 Buffer 中的 16 进制数去找到 boundary 和实际数据所在的位置（索引），对 Buffer 数据进行切分，切分出文件信息和文件内容，最后将文件内容依据文件信息写入磁盘中。
    

#### 4. 解析文件的基本过程

1. 将 Buffer 的二进制内容转换为 body 字符串。目的是方便根据 boundary 进行切分。

2. 以 `--boundary` 作为分隔符，对 body 字符串进行切分。因为我们一次可以上传多个文件，多个文件是以 `--boundary` 为分隔符的。所以我们使用 `--boundary` 为分隔符切分 body 字符串，这样能合在一起的文件分开，使得我们能单独解析每个文件。

3. 对单独的一个文件块进行解析。此时以 `\r\n` 为分隔符对文件块进行拆分，这样可以得到 Content-Disposition 和 Content-Type。

4. 从 Content-Disposition 中找到和文件相关的信息，如文件名：filename。

5. 根据 Content-Type 找出文件的类型。

6. 根据规范，Content-Type 所在行、`--boundary--` 的所在行之间的内容是真正的文件内容，因此，我们需要对 body 字符串再次进行拆分，找出文件内容的起始位置和结束位置，得到真正的文件内容。这个位置可以通过定位 Content-Type 和 `--boundary--` 来确定。

7. 得到文件内容还是字符串，我们需要将其转换成 Buffer 二进制数据，然后将文件内容写入磁盘。



- 第一个参数总是固定不变的 form-data；附加的参数不区分大小写，并且拥有参数值，参数名与参数值用等号('=')连接，参数值用双引号括起来。参数之间用分号(';')分隔。

Content-Disposition: form-data
Content-Disposition: form-data; name="fieldName"
Content-Disposition: form-data; name="fieldName"; filename="filename.jpg"