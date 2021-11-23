<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Axios 的基本使用](#axios-%E7%9A%84%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8)
  - [1. 参考资料](#1-%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99)
  - [2. axios 的 api](#2-axios-%E7%9A%84-api)
    - [1. axios(config)](#1-axiosconfig)
    - [2. `axios(url[, config])`](#2-axiosurl-config)
    - [3. 请求方法的别名](#3-%E8%AF%B7%E6%B1%82%E6%96%B9%E6%B3%95%E7%9A%84%E5%88%AB%E5%90%8D)
      - [1. axios.request(config)](#1-axiosrequestconfig)
      - [2. axios.get(url[, config])](#2-axiosgeturl-config)
      - [3. axios.delete(url[, config])](#3-axiosdeleteurl-config)
      - [4. axios.head(url[, config])](#4-axiosheadurl-config)
      - [5. axios.options(url[, config])](#5-axiosoptionsurl-config)
      - [6. axios.post(url[, data[, config]])](#6-axiosposturl-data-config)
      - [7. axios.put(url[, data[, config]])](#7-axiosputurl-data-config)
      - [8. axios.patch(url[, data[, config]])](#8-axiospatchurl-data-config)
    - [4. 创建 axios 实例 —— axios.create([config])](#4-%E5%88%9B%E5%BB%BA-axios-%E5%AE%9E%E4%BE%8B--axioscreateconfig)
    - [5. axios 的实例方法](#5-axios-%E7%9A%84%E5%AE%9E%E4%BE%8B%E6%96%B9%E6%B3%95)
      - [1. axios#request(config)](#1-axiosrequestconfig)
      - [2. axios#get(url[, config])](#2-axiosgeturl-config)
      - [3. axios#delete(url[, config])](#3-axiosdeleteurl-config)
      - [4. axios#head(url[, config])](#4-axiosheadurl-config)
      - [5. axios#options(url[, config])](#5-axiosoptionsurl-config)
      - [6. axios#post(url[, data[, config]])](#6-axiosposturl-data-config)
      - [7. axios#put(url[, data[, config]])](#7-axiosputurl-data-config)
      - [8. axios#patch(url[, data[, config]])](#8-axiospatchurl-data-config)
      - [9. axios#getUri([config])](#9-axiosgeturiconfig)
    - [6. 拦截器 —— Interceptors](#6-%E6%8B%A6%E6%88%AA%E5%99%A8--interceptors)
  - [3. 响应的格式（Response Schema）](#3-%E5%93%8D%E5%BA%94%E7%9A%84%E6%A0%BC%E5%BC%8Fresponse-schema)
    - [1. data](#1-data)
    - [2. status](#2-status)
    - [3. statusText](#3-statustext)
    - [4. config](#4-config)
    - [5. headers](#5-headers)
  - [4. 请求配置项说明](#4-%E8%AF%B7%E6%B1%82%E9%85%8D%E7%BD%AE%E9%A1%B9%E8%AF%B4%E6%98%8E)
    - [1. url](#1-url)
    - [2. method](#2-method)
    - [3. baseURL](#3-baseurl)
    - [4. headers](#4-headers)
    - [5. params](#5-params)
    - [6. data](#6-data)
    - [7. timeout](#7-timeout)
    - [8. withCredentials](#8-withcredentials)
    - [9. responseType](#9-responsetype)
    - [10. onUploadProcess](#10-onuploadprocess)
    - [11. onDownloadProcess](#11-ondownloadprocess)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Axios 的基本使用

## 1. 参考资料

1. [axios 中文文档](http://www.axios-js.com/zh-cn/docs/index.html)

2. [axios 官方文档](https://axios-http.com/docs/intro)

3. [axios github 地址](https://github.com/axios/axios)

4. [7.axios拦截器的使用](https://www.jianshu.com/p/2987a589f485)

5. [6.axios的实例创建和模块封装](https://www.jianshu.com/p/c6b1cf2d59f9)

6. [axios的用法详解](https://www.jianshu.com/p/09fd4646f3a2)

7. [axios的用法与功能实现](https://www.jianshu.com/p/93fa30986d07)

## 2. axios 的 api

### 1. axios(config)

1. 接收一个配置对象，实现发送各种请求。返回值是一个 `Promise`，因此可以使用 `then` 方法链式调用，或者使用 `await` 关键字。

2. 发送一个 post 请求：
   ```js
      // Send a POST request
      axios({
          method: 'post',
          url: '/user/12345',
          data: {
              firstName: 'Fred',
              lastName: 'Flintstone'
          }
      });
   ```
3. 发送一个 get 请求：
   ```js
      // GET request for remote image in node.js
      axios({
          method: 'get',
          url: 'http://bit.ly/2mTM3nY',
          responseType: 'stream'
      })
      .then(function (response) {
      response.data.pipe(fs.createWriteStream('ada_lovelace.jpg'))
      });
   ```


### 2. `axios(url[, config])`

1. 指定请求的 url，然后在第二个参数中指定其他配置项。值得说明的是，不指定 config 对象只指定 url，则请求方法默认为 get。

2. 发送一个 get 请求：
   ```js
      // Send a GET request (default method)
      axios('/user/12345');
   ```


### 3. 请求方法的别名

1. 为了使用方便，我们提供了和请求方法同名的 axios api。我们可以直接调用这些方法，而无需去 config 对象中配置请求方法。

#### 1. axios.request(config)
#### 2. axios.get(url[, config])
#### 3. axios.delete(url[, config])
#### 4. axios.head(url[, config])
#### 5. axios.options(url[, config])
#### 6. axios.post(url[, data[, config]])

1. 发送一个 post 请求。

2. 第一个参数是 url，第二个是我们需要通过请求体（request body）传递的参数，值是一个纯对象。第三个参数是其他的配置项。

#### 7. axios.put(url[, data[, config]])
#### 8. axios.patch(url[, data[, config]])

### 4. 创建 axios 实例 —— axios.create([config])

1. 使用 axios.create() 方法创建一个 axios 实例。在创建实例的过程中，可以传入自定义的配置项。其返回值是 axios 的实例，我们可以直接调用这个实例发送请求。

2. 为什么需要使用 axios 实例呢，在实际开发中，我们由很多的接口，每个接口要求的请求方法、参数都不一样，我们可以直接使用 axios 提供的各种请求方法加上参数来发送请求，这样做虽然可以，但是问题是不好统一管理请求，同时必然存在着重复的配置，有一些情况下需要对请求和响应做出进行拦截，而 axios 提供的请求方法不具备这样的能力。基于上面几点，我们使用 axios 的实例来发送请求，而相同的配置放到 axios 的实例中，同时 axios 的实例也提供拦截器，只要是这个实例发出的请求和收到的响应，都可以被拦截。

3. 用法示例：
   ```js
      const instance = axios.create({
          baseURL: 'https://some-domain.com/api',
          timeout: 1000,
          headers: {'X-Custom-Header': 'foobar'}
       });
       // 调用实例发送请求
       instance({
          url: '/info',
          method: 'get'
      }).then(res => {
          console.log(res);
      })
   ```
4. 如上例所示，使用实例发送请求的时候，还可以给实例传入一个配置项对象，这个配置项对象适合接口相关的信息，比如说接口地址（相对路径）、请求方法、请求参数等，实例接收的配置项会和 axios.create() 接收的自定义配置项合并。


### 5. axios 的实例方法

1. axios 的实例也有和请求方法同名的函数。我们创建了 axios 的实例以后，可以直接使用对用的函数来发送请求。

#### 1. axios#request(config)

#### 2. axios#get(url[, config])

#### 3. axios#delete(url[, config])

#### 4. axios#head(url[, config])

#### 5. axios#options(url[, config])

#### 6. axios#post(url[, data[, config]])

#### 7. axios#put(url[, data[, config]])

#### 8. axios#patch(url[, data[, config]])

#### 9. axios#getUri([config])


### 6. 拦截器 —— Interceptors

1. 我们可以在正式发出请求之前，对请求进行拦截，统一对请求做一些处理，然后再将请求发出。而收到响应以后，在其被 then 方法或者 catch 方法处理前，对响应进行拦截，统一对响应做一些处理。处理完成以后，再将响应交由 then 或者 catch 方法进行处理。

2. 拦截请求的作用：
   - 比如 config 中的一些信息不符合服务器的要求，得及时拦截下来更改。
   - 比如每次发送网络请求的时候，都希望在界面中显示一个动态加载的请求图标，就是一直在转圈圈，让用户知道当前页面正在加载数据，准备渲染视图。
   - 比如某些网络请求（比如登录token）,必须携带一些特殊的信息。
   
3. 对请求进行拦截使用：`interceptors.request.use()`，用法如下：
   ```js
      // Add a request interceptor
      axios.interceptors.request.use(function (config) {
           // Do something before request is sent
           return config;
       }, function (error) {
            // Do something with request error
            return Promise.reject(error);
       });

   ```
4. `interceptors.request.use()` 接收两个回调函数作为参数，第一个函数用来处理请求，主要是处理配置对象，处理完成后，将新的配置对象返回，此时拦截过程结束，请求被放行。而第二个回调函数是当请求出现异常的时候进行处理。

5. 对响应进行拦截使用 `interceptors.response.use()`，用法如下：
   ```js
       // Add a response interceptor
      axios.interceptors.response.use(function (response) {
           // Any status code that lie within the range of 2xx cause this function to trigger
           // Do something with response data
           return response;
      }, function (error) {
           // Any status codes that falls outside the range of 2xx cause this function to trigger
          // Do something with response error
          return Promise.reject(error);
      }); 
   ```
6. `interceptors.response.use()` 接收两个回调函数作为参数，第一个函数用来处理成功时的响应，即服务端返回的响应中，状态码以 2 开头的都被认为是成功的响应，因此会被第一个回调函数进行处理。响应被处理完后，一定要将其返回，这样处理后的响应才能被 then 方法接收并进行下一步的处理。而第二个回调函数是处理响应出现异常的状况。当服务端返回的响应中，状态码不以 2 开头的都被认为是异常的响应，这样的响应都会进入到第二个回调函数中进行处理。

7. axios 的实例也可以使用拦截器：
   ```js
      const instance = axios.create();
       // 请求拦截器
      instance.interceptors.request.use(function (config) {
          // 处理配置项
          // ...
          // 放行请求
          return config;
      }, function (err) {
          // 处理请求异常
          return Promise.reject(err);
      });
      // 响应拦截器
      instance.interceptors.response.use(function (response) {
          // 处理 resolve 状态下的响应
          //...
          // 放行响应
          return response;
      }, function (err) {
          // 处理 reject 状态下的响应
          return Promise.reject(err);
      });
   ```
8. 实例的拦截器使用方式与前面介绍的一模一样。

## 3. 响应的格式（Response Schema）

1. 使用 axios 发送请求后，其返回值是一个 Promise，那么 Promise 在成功（resolved）状态下的值是经过 Axios 包装过的。其具体内容如下：
   ```js
      {
          // `data` is the response that was provided by the server
          data: {},

          // `status` is the HTTP status code from the server response
          status: 200,

          // `statusText` is the HTTP status message from the server response
          statusText: 'OK',

          // `headers` the HTTP headers that the server responded with
          // All header names are lower cased and can be accessed using the bracket notation.
          // Example: `response.headers['content-type']`
          headers: {},

          // `config` is the config that was provided to `axios` for the request
          config: {},

          // `request` is the request that generated this response
          // It is the last ClientRequest instance in node.js (in redirects)
          // and an XMLHttpRequest instance in the browser
          request: {}
      }
   ```

### 1. data

1. 服务器返回的数据。

### 2. status

1. 服务器返回的响应中的 http 状态码。

### 3. statusText

1. 服务器返回的响应中的 http 状态信息。

### 4. config

1. 由 axios 提供的和请求相关的配置信息。

### 5. headers

1. 服务器提供的 http 响应头信息。

2. 响应头字段都是小写的，能够通过方括号语法（`[]`）访问。例如：`response.headers['content-type']`。

## 4. 请求配置项说明

1. 使用 axios 发送请求的时候，我们可以配置一些内容，以实现对请求的更加精细的控制。

2. 完整的配置项如下所示：
   ```js
      {
          // `url` is the server URL that will be used for the request
          url: '/user',

          // `method` is the request method to be used when making the request
          method: 'get', // default

          // `baseURL` will be prepended to `url` unless `url` is absolute.
          // It can be convenient to set `baseURL` for an instance of axios to pass relative URLs
          // to methods of that instance.
          baseURL: 'https://some-domain.com/api',

          // `transformRequest` allows changes to the request data before it is sent to the server
          // This is only applicable for request methods 'PUT', 'POST', 'PATCH' and 'DELETE'
          // The last function in the array must return a string or an instance of Buffer, ArrayBuffer,
          // FormData or Stream
          // You may modify the headers object.
          transformRequest: [function (data, headers) {
          // Do whatever you want to transform the data

               return data;
          }],

         // `transformResponse` allows changes to the response data to be made before
         // it is passed to then/catch
         transformResponse: [function (data) {
         // Do whatever you want to transform the data

             return data;
         }],

          // `headers` are custom headers to be sent
          headers: {'X-Requested-With': 'XMLHttpRequest'},

         // `params` are the URL parameters to be sent with the request
         // Must be a plain object or a URLSearchParams object
         // NOTE: params that are null or undefined are not rendered in the URL.
         params: {
             ID: 12345
         },

          // `paramsSerializer` is an optional function in charge of serializing `params`
         // (e.g. https://www.npmjs.com/package/qs, http://api.jquery.com/jquery.param/)
         paramsSerializer: function (params) {
             return Qs.stringify(params, {arrayFormat: 'brackets'})
         },

         // `data` is the data to be sent as the request body
         // Only applicable for request methods 'PUT', 'POST', 'DELETE , and 'PATCH'
         // When no `transformRequest` is set, must be of one of the following types:
         // - string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
         // - Browser only: FormData, File, Blob
         // - Node only: Stream, Buffer
         data: {
             firstName: 'Fred'
         },

          // syntax alternative to send data into the body
         // method post
         // only the value is sent, not the key
         data: 'Country=Brasil&City=Belo Horizonte',

         // `timeout` specifies the number of milliseconds before the request times out.
         // If the request takes longer than `timeout`, the request will be aborted.
         timeout: 1000, // default is `0` (no timeout)

         // `withCredentials` indicates whether or not cross-site Access-Control requests
           // should be made using credentials
          withCredentials: false, // default

          // `adapter` allows custom handling of requests which makes testing easier.
         // Return a promise and supply a valid response (see lib/adapters/README.md).
         adapter: function (config) {
              /* ... */
         },

         // `auth` indicates that HTTP Basic auth should be used, and supplies credentials.
         // This will set an `Authorization` header, overwriting any existing
         // `Authorization` custom headers you have set using `headers`.
         // Please note that only HTTP Basic auth is          configurable through this parameter.
         // For Bearer tokens and such, use `Authorization` custom headers instead.
          auth: {
              username: 'janedoe',
              password: 's00pers3cret'
          },

          // `responseType` indicates the type of data that the server will respond with
          // options are: 'arraybuffer', 'document', 'json', 'text', 'stream'
          //   browser only: 'blob'
          responseType: 'json', // default

          // `responseEncoding` indicates encoding to use for decoding responses (Node.js only)
          // Note: Ignored for `responseType` of 'stream' or client-side requests
          responseEncoding: 'utf8', // default

          // `xsrfCookieName` is the name of the cookie to use as a value for xsrf token
          xsrfCookieName: 'XSRF-TOKEN', // default

          // `xsrfHeaderName` is the name of the http header that carries the xsrf token value
          xsrfHeaderName: 'X-XSRF-TOKEN', // default

          // `onUploadProgress` allows handling of progress events for uploads
          // browser only
          onUploadProgress: function (progressEvent) {
          // Do whatever you want with the native progress event
          },

          // `onDownloadProgress` allows handling of progress events for downloads
          // browser only
          onDownloadProgress: function (progressEvent) {
          // Do whatever you want with the native progress event
          },

          // `maxContentLength` defines the max size of the http response content in bytes allowed in node.js
          maxContentLength: 2000,

          // `maxBodyLength` (Node only option) defines the max size of the http request content in bytes allowed
          maxBodyLength: 2000,

          // `validateStatus` defines whether to resolve or reject the promise for a given
          // HTTP response status code. If `validateStatus` returns `true` (or is set to `null`
          // or `undefined`), the promise will be resolved; otherwise, the promise will be
          // rejected.
          validateStatus: function (status) {
               return status >= 200 && status < 300; // default
          },

          // `maxRedirects` defines the maximum number of redirects to follow in node.js.
          // If set to 0, no redirects will be followed.
          maxRedirects: 5, // default

          // `socketPath` defines a UNIX Socket to be used in node.js.
          // e.g. '/var/run/docker.sock' to send requests to the docker daemon.
          // Only either `socketPath` or `proxy` can be specified.
          // If both are specified, `socketPath` is used.
          socketPath: null, // default

          // `httpAgent` and `httpsAgent` define a custom agent to be used when performing http
          // and https requests, respectively, in node.js. This allows options to be added like
         // `keepAlive` that are not enabled by default.
           httpAgent: new http.Agent({ keepAlive: true }),
           httpsAgent: new https.Agent({ keepAlive: true }),

          // `proxy` defines the hostname, port, and protocol of the proxy server.
          // You can also define your proxy using the conventional `http_proxy` and
          // `https_proxy` environment variables. If you are using environment variables
          // for your proxy configuration, you can also define a `no_proxy` environment
          // variable as a comma-separated list of domains that should not be proxied.
          // Use `false` to disable proxies, ignoring environment variables.
          // `auth` indicates that HTTP Basic auth should be used to connect to the proxy, and
         // supplies credentials.
         // This will set an `Proxy-Authorization` header, overwriting any existing
         // `Proxy-Authorization` custom headers you have set using `headers`.
         // If the proxy server uses HTTPS, then you must set the protocol to `https`.
        proxy: {
            protocol: 'https',
            host: '127.0.0.1',
            port: 9000,
            auth: {
                username: 'mikeymike',
                password: 'rapunz3l'
            }
        },

        // `cancelToken` specifies a cancel token that can be used to cancel the request
       // (see Cancellation section below for details)
       cancelToken: new CancelToken(function (cancel) {
      }),

          // `decompress` indicates whether or not the response body should be decompressed
          // automatically. If set to `true` will also remove the 'content-encoding' header
          // from the responses objects of all decompressed responses
          // - Node only (XHR cannot turn off decompression)
          decompress: true // default

      }
   ```
3. 上面的配置项中，只有 `url` 是必须的，其他的都是可选的，如果没有指定 `method`，那么默认的请求方法就是 `get`。

### 1. url

1. 请求的服务器的地址，一般来说是 path。

### 2. method

1. 请求的方法，默认是 get 方法。不区分大小写。

### 3. baseURL

1. `baseURL`将被添加到 `url` 前面，除非 `url`是绝对的。

2. 可以方便地为 axios 的实例设置`baseURL`，以便将相对 url 传递给该实例的方法。

### 4. headers

1. 自定义的请求头，与原有的请求头进行合并，然后发送到服务端。

### 5. params

1. params 指的是 url 参数，即附在路径后的的查询字符串。

2. params 的值必须是纯对象（plain object）或者是 URLSearchParams 对象。

3. 如果 params 的值是 undefined 或者 null，则会在 url 中出现。

### 6. data

1. data 是要作为请求体（request body）发送的数据。

2. 适用的请求方法有 PUT，POST 和 PATCH。

3. 当没有设置 `transformRequest` 时，必须是以下类型之一：
    - string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
    - Browser only: FormData, File, Blob
    - Node only: Stream

### 7. timeout

1. 指定请求的超时时间，单位是毫秒。默认是 0。

### 8. withCredentials

1. `withCredentials` 指示是否跨域请求应该使用 credentials。

2. 默认是 false。

### 9. responseType

1. 指定服务器应该返回的响应类型。

2. `responseType` 可选的值有：arraybuffer、 document、json、text、stream。浏览器环境下支持 blob。

3. `responseType` 默认值是 `json`。

### 10. onUploadProcess

1. onUploadProcess 允许我们处理上传的进度事件。

2. onUploadProcess 的值是一个函数，这个函数可以理解为是原生的 process 事件的事件处理函数。

### 11. onDownloadProcess

1. onDownloadProcess 允许我们处理下载的进度事件。

2. onDownloadProcess 的值是一个函数，这个函数可以理解为是原生的 process 事件的事件处理函数。