# http - 跨域

## 1. 基本说明

1. 浏览器的同源策略：协议、域名和端口三个内容相同，才会被浏览器认为是同源，只要有一个不一致，就会被浏览器认为是不同源，在一些情况下，浏览器会阻止不同源的请求。

2. 同源策略是浏览器为了防范 CSRF，保证用户的安全而设置的一种安全策略。

3. 当前页面下，只能发起同源下的 ajax 请求，对，同源策略主要是针对 ajax 请求的。

4. 参考资料：
   - [JS中的跨域问题及解决办法汇总
](https://blog.csdn.net/lareinalove/article/details/84107476)
   - [html5 postMessage解决跨域、跨窗口消息传递](https://www.cnblogs.com/dolphinX/p/3464056.html)
   - []()
    
## 1. jsonp

1. jsonp 是 JSON with Padding 的简称。所谓的 Padding，就是将 JSON 格式的数据放到被调用的函数中。形成一段可执行的 js 代码。

2. jsonp 的原理就是 `script` 标签不受浏览器同源策略的限制，其 `src` 属性可以设置任意的地址，而不会被浏览器阻止。所以，基于这个原理，我们可以将 `script` 标签的 src 属性设置为我们需要发送 ajax 请求的那个地址，然后，加上查询参数：`?callback=getData`，如下所示：
   `<script src="http://www.aaa.com/test/mapdata?callback=getData"></script>`

3. 服务器接收到这个请求后，从 url 中解析出查询参数：`getData`，然后将 json 数据“放入” `getData` 中，形成一段可执行的 js 代码。以 express 接收并处理响应为例：
   ```javascript
      const express = require("express");
      const app = express();
      app.get('/test/mapdata', function(req, res) {
          const {callback} = req.query;
          
          let resData = `${callback}({"name": "jack", "age": 25})`;

          res.send(resData);
          
      })
   ```

4. 浏览器收到的是一段文本：
   ```
      getData({"name": "jack", "age": 25})
   ```

5. 实际上浏览器会把这段文档作为一段可执行的 js 代码解析并执行。如果我们已经定义好 `getData()` 方法，那么 `{"name": "jack", "age": 25}` 这段 json 数据会作为参数传入 getData() 方法中。这样就实现了跨域请求数据。

6. jsonp 跨域需要前端和后端一起配合才能实现。前端和后端约定好 callback 的值即处理数据的函数名，同时前端要提前定义好处理数据的函数。

7. jsonp 的优缺点：
   - 优点：实现简单，兼容性强，不受浏览器跨域限制。
   - 缺点：只支持 get 请求，不支持 post 请求。


## 2. CORS

### 1. 基本说明
1. CORS 是 Cross-Origin Resource Sharing 的缩写，意思是跨域资源共享，定义了必须在访问跨域资源时，浏览器与服务器应该如何沟通。CORS 背后的基本思想就是使用自定义的 HTTP 头部让浏览器与服务器进行沟通，从而决定请求或响应是应该成功还是失败。

2. 浏览器端，在请求头中加入一个origin字段，表示源信息，例如：`Origin: http://localhost:8888`。

3. 服务器端，在响应头中添加 `Access-Control-Allow-Origin` 字段，值或者为 `*`，或者一个源信息。例如：`Access-Control-Allow-Origin: '*'` 或者`Access-Control-Allow-Origin: 'http://localhost:8888'`

4. 浏览器会根据响应头中的 `Access-Control-Allow-Origin` 这个字段来决定是否禁止这次请求：
   - 如果是*或者和请求头中 `Origin` 字段的源信息相同，则接收这个响应。
   - 如果没有这个字段，或者是其他源信息，则禁止这次响应。

### 2. CORS 预请求 - 简单请求

1. 如果请求信息满足以下两个条件，就是简单请求，简单请求不需要预先发送请求，而是直接发送 CORS 请求。具体来说，就是在头信息之中，增加一个 Origin 字段。 
   1. 默认允许的方法  
      - GET
      - POST
      - HEAD
   2. HTTP的头信息不超出以下几种字段：
     - Accept
     - Accept-Language
     - Content-Language
     - Last-Event-ID
     - Content-Type：只限于三个值：
       - application/x-www-form-urlencoded
       - multipart/form-data
       - text/plain  
2. 如果 Origin 指定的域名在许可范围内，服务器返回的响应，会多出几个头信息字段：  
   ```javascript
      Access-Control-Allow-Origin: http://localhost:8888
      Access-Control-Allow-Credentials: true
      Access-Control-Expose-Headers: FooBar
      Connection: keep-alive
      Date: Wed, 16 Oct 2019 05:27:49 GMT
      Transfer-Encoding: chunked
   ```
3. 字段说明：
   - Access-Control-Allow-Origin  
     该字段是必须的。它的值要么是请求时 Origin 字段的值，要么是一个 *，表示接受任意域名的请求。
   - Access-Control-Allow-Credentials  
     该字段可选。它的值是一个布尔值，表示是否允许发送 Cookie。默认情况下，Cookie 不包括在 CORS 请求之中。设为 true，即表示服务器明确许可，Cookie 可以包含在请求中，一起发给服务器。这个值也只能设为 true，如果服务器不要浏览器发送 Cookie，删除该字段即可。
   - Access-Control-Expose-Headers  
     该字段可选。CORS 请求时，XMLHttpRequest 对象的 getResponseHeader() 方法只能拿到 6 个基本字段：Cache-Control、Content-Language、Content-Type、Expires、Last-Modified、Pragma。如果想拿到其他字段，就必须在Access-Control-Expose-Headers 里面指定。上面的例子指定，getResponseHeader('FooBar')可以返回 FooBar 字段的值。

4. CORS请求默认不发送 Cookie 和 HTTP 认证信息。如果要把Cookie发到服务器，一方面要服务器同意，指定 Access-Control-Allow-Credentials 字段。 
`Access-Control-Allow-Credentials: true`。

5. 另一方面，开发者必须在 AJAX 请求中打开 withCredentials 属性。
      ```javascript
         // ajax配置方法
         var xhr = new XMLHttpRequest();
         xhr.withCredentials = true;
      ```
    
6. 否则，即使服务器同意发送 Cookie，浏览器也不会发送。或者，服务器要求设置 Cookie，浏览器也不会处理。但是，如果省略 withCredentials 设置，有的浏览器还是会一起发送 Cookie。这时，可以显式关闭 withCredentials。`xhr.withCredentials = false;`。

7. 需要注意的是，如果要发送 Cookie，Access-Control-Allow-Origin 就不能设为星号，必须指定明确的、与请求网页一致的域名。同时，Cookie 依然遵循同源政策，只有用服务器域名设置的 Cookie 才会上传，其他域名的 Cookie 并不会上传，且（跨源）原网页代码中的 document.cookie 也无法读取服务器域名下的 Cookie。  
 
### 2. CORS 预请求 - 非简单请求   
  
1. 请求信息不满足以上的两个条件，就是非简单请求。非简单请求是那种对服务器有特殊要求的请求，比如请求方法是PUT或DELETE，或者Content-Type字段的类型是application/json。  

2. 非简单请求的CORS请求，会在正式通信之前，增加一次HTTP查询请求，称为"预检"请求（preflight）。  

3. 浏览器先询问服务器，当前网页所在的域名是否在服务器的许可名单之中，以及可以使用哪些HTTP请求方法和头信息字段。只有得到肯定答复，浏览器才会发出正式的XMLHttpRequest请求，否则就报错。例如，使用fetch方法发送一个请求，并自定义头部信息。
    ```javascript
         fetch('http://localhost:3000', {
             // 设置请求方法
             method: 'POST',
             // 头部信息
             headers: {
                 'X-Test-Cors': '123'
             }
         }).then() ;
    ```

4. 浏览器发现这是一个非简单请求，就会发送一个预请求，要求服务器可以确认这样的请求。我们可以看一下预请求的头部信息：
   ```javascript
       OPTIONS /cors HTTP/1.1
       Access-Control-Request-Headers: x-test-cors
       Access-Control-Request-Method: POST
       Origin: http://localhost:8888
       Referer: http://localhost:8888/
       Sec-Fetch-Mode: no-cors
       User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.87 Safari/537.36
    ```

5. 这个预请求的方法是 Options，表示这个请求是用来询问的。头信息里面，关键字段是 Origin，表示请求来自哪个源。 除了 Origin 字段，"预检"请求的头信息包括两个特殊字段。
    - Access-Control-Request-Method  
      该字段是必须的，用来列出浏览器的CORS请求会用到哪些HTTP方法，上例是POST。
    - Access-Control-Request-Headers  
      该字段是一个逗号分隔的字符串，指定浏览器 CORS 请求会额外发送的头信息字段，上例是 X-Test-Cors。

6. 服务器收到"预检"请求以后，检查了 Origin、Access-Control-Request-Method 和 Access-Control-Request-Headers 字段以后，确认允许跨源请求，就可以做出回应。我们的响应头如下：
    ```javascript
         Access-Control-Allow-Headers: X-Test-Cors
         Access-Control-Allow-Methods: PUT, DELETE, DELETE
         Access-Control-Allow-Origin: http://localhost:8888
         Connection: keep-alive
         Date: Wed, 16 Oct 2019 03:44:05 GMT
         Transfer-Encoding: chunked
    ```
7. 在上面的响应头中，关键的是Access-Control-Allow-Origin字段，表示http://localhost:8888可以请求数据。该字段也可以设为星号，表示同意任意跨源请求。即`Access-Control-Allow-Origin: *`。服务器回应的其他CORS相关字段如下：
    ```javascript
        Access-Control-Allow-Headers: X-Test-Cors
        Access-Control-Allow-Methods: PUT, DELETE, DELETE
        Access-Control-Allow-Credentials: true
        Access-Control-Allow-Max-Age: 1728000
    ```     
8. 每个字段的含义如下：
   - Access-Control-Allow-Methods   
    该字段必需，它的值是逗号分隔的一个字符串，表明服务器支持的所有跨域请求的方法。注意，返回的是所有支持的方法，而不单是浏览器请求的那个方法。这是为了避免多次"预检"请求。
  - Access-Control-Allow-Headers  
    如果浏览器请求包括Access-Control-Request-Headers字段，则Access-Control-Allow-Headers字段是必需的。它也是一个逗号分隔的字符串，表明服务器支持的所有头信息字段，不限于浏览器在"预检"中请求的字段。
  - Access-Control-Allow-Credentials    
    该字段与简单请求时的含义相同。
  - Access-Control-Allow-Max-Age    
    该字段可选，用来指定本次预检请求的有效期，单位为秒。上面结果中，有效期是20天（1728000秒），即允许缓存该条回应1728000秒（即20天），在此期间，不用发出另一条预检请求。

8. 一旦服务器通过了"预检"请求，以后每次浏览器正常的CORS请求，就都跟简单请求一样，会有一个Origin头信息字段。服务器的回应，也都会有一个Access-Control-Allow-Origin头信息字段。

9. 经过“预检”之后，浏览器的正常 CORS 请求如下：
   ```javascript
       Origin: http://localhost:8888
       Referer: http://localhost:8888/
       Sec-Fetch-Mode: cors
       User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.87 Safari/537.36
       X-Test-Cors: 123
   ```
   上面头信息的Origin字段是浏览器自动添加的。  
   同预检的请求头相比，没有了Access-Control-Request-Headers、Access-Control-Request-Methods和Access-Control-Allow-Origin。

10. 而服务器的回应是这样的：
    ```javascript
       Access-Control-Allow-Headers: X-Test-Cors
       Access-Control-Allow-Methods: PUT, DELETE
       Access-Control-Allow-Origin: http://localhost:8888
       Connection: keep-alive
       Date: Wed, 16 Oct 2019 04:47:45 GMT
       Transfer-Encoding: chunked
    ```

## 3. postMessage

1. postMessage 是 html5 引入的，用来解决跨域、跨页面通信的问题。适用场景如下：
   - 页面和其打开的新窗口的数据传递
   - 多窗口之间消息传递
   - 页面与嵌套的 iframe 消息传递
   - 上面三个问题的跨域数据传递

2. 发送消息的 api：`otherWindow.postMessage(message, targetOrigin, [transfer])`
   - otherWindow
     其他窗口的一个引用，比如iframe的contentWindow属性、执行window.open返回的窗口对象、或者是命名过或数值索引的window.frames。
   - message
     将要发送到其他 window 的数据。它将会被结构化克隆算法序列化。这意味着你可以不受什么限制的将数据对象安全的传送给目标窗口而无需自己序列化。
   - targetOrigin
     通过窗口的 `origin` 属性来指定哪些窗口能接收到消息事件，其值可以是字符串 `*`（表示无限制）或者一个URI。在发送消息的时候，如果目标窗口的协议、主机地址或端口这三者的任意一项不匹配targetOrigin提供的值，那么消息就不会被发送；只有三者完全匹配，消息才会被发送。这个机制用来控制消息可以发送到哪些窗口；例如，当用 postMessage 传送密码时，这个参数就显得尤为重要，必须保证它的值与这条包含密码的信息的预期接受者的 `origin` 属性完全一致，来防止密码被恶意的第三方截获。如果你明确的知道消息应该发送到哪个窗口，那么请始终提供一个有确切值的targetOrigin，而不是 `*` 。不提供确切的目标将导致数据泄露到任何对数据感兴趣的恶意站点。
   - transfer 可选
     是一串和 message 同时传递的 Transferable 对象. 这些对象的所有权将被转移给消息的接收方，而发送一方将不再保有所有权。

3. 接收消息，在目标窗口，监听 `message` 事件，如下所示：
   ```javascript

      window.addEventListener("message", function(messageEvent) {
        
        const {origin, data, source} = messageEvent;
        if (origin !== "http://example.org:8080") {
           return;
        }
      });
   ```
   messageEvent 中比较重要的几个属性有：
   - origin
     调用 postMessage  时消息发送方窗口的 origin . 这个字符串由 `协议://域名: 端口号` 拼接而成。例如 `https://example.org` (隐含端口 443)、`http://example.net` (隐含端口 80)、`http://example.com:8080`。请注意，这个 `origin` 不能保证是该窗口的当前或未来 `origin`，因为 `postMessage` 被调用后可能被导航到不同的位置。
   - data
     从其他 window 中传递过来的数据对象
   - source
     对发送消息的窗口对象的引用。您可以使用此来在具有不同 `origin` 的两个窗口之间建立双向通信。