# Promise 包装异步操作

1. 使用 `Promise` 包装异步操作后，这样可以使用 `async/await` 关键字，实现同步操作。

2. `Promise` 是一个构造函数，也是一个对象。当其作为一个构造函数时，接收一个执行器作为参数，这个执行器也是一个函数，执行器内部是一个异步操作。执行器接收两个参数：resolve 和 reject。resolve 和 reject 也是函数，分别对应异步状态是成功和失败时的操作。

3. 在浏览器端，常见的异步操作是 ajax 请求，那么我们使用 Promise 包装 ajax 请求，使之能实现同步操作。

4. 为了简便起见，我们直接使用 jQuery 的 ajax 函数。

5. 我们在浏览器端进行测试，因此使用 cdn 的方式引入 jQuery。如下所示：`<script src="https://cdn.jsdelivr.net/npm/jquery@3.6.0/dist/jquery.min.js"></script>`

6. 使用 cdn 的方式引入 jQuery 后，我们就引入了 jQuery 的全局变量 `$`，因此我们可以通过全局变量 `$` 使用 ajax() 函数。

7. 在另外一个 script 标签内，我们封装一个返回 Promise 的 ajax 函数：
   ```js
      function  myRequest(url, options) {
          return new Promise((resolve, reject) => {
              $.ajax({
                  type: 'GET',
                  url: url,
                  success: function (data) {
                      resolve(data);
                  },
                  error: function (jqxhr, textStatus, error) {
                      reject(error);
                  }
              })
          })
      }

      async function getMail(url) {
          console.log('request starts');
          const res = await myRequest(url);
          console.log('resquest ends');
          console.log(res);
      }

      getMail('http://localhost:8001/mail');
      console.log('async 函数并不是真正的同步');
   ```

8. 使用 koa 启动一个后端服务，用来处理前端的 ajax 请求：