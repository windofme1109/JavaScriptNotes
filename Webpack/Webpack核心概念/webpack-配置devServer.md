<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [配置 devServer](#%E9%85%8D%E7%BD%AE-devserver)
  - [1. 使用 `npm run watch` 命令](#1-%E4%BD%BF%E7%94%A8-npm-run-watch-%E5%91%BD%E4%BB%A4)
  - [2. 使用 `webpack-dev-server`](#2-%E4%BD%BF%E7%94%A8-webpack-dev-server)
    - [1. 使用 devServer 实现前端路由转发](#1-%E4%BD%BF%E7%94%A8-devserver-%E5%AE%9E%E7%8E%B0%E5%89%8D%E7%AB%AF%E8%B7%AF%E7%94%B1%E8%BD%AC%E5%8F%91)
      - [1. 配置前端路由转发（只在本地起作用）](#1-%E9%85%8D%E7%BD%AE%E5%89%8D%E7%AB%AF%E8%B7%AF%E7%94%B1%E8%BD%AC%E5%8F%91%E5%8F%AA%E5%9C%A8%E6%9C%AC%E5%9C%B0%E8%B5%B7%E4%BD%9C%E7%94%A8)
      - [2. 对根路径进行代理](#2-%E5%AF%B9%E6%A0%B9%E8%B7%AF%E5%BE%84%E8%BF%9B%E8%A1%8C%E4%BB%A3%E7%90%86)
    - [2. 使用 devServer 实现代理转发（转发到后端服务器）](#2-%E4%BD%BF%E7%94%A8-devserver-%E5%AE%9E%E7%8E%B0%E4%BB%A3%E7%90%86%E8%BD%AC%E5%8F%91%E8%BD%AC%E5%8F%91%E5%88%B0%E5%90%8E%E7%AB%AF%E6%9C%8D%E5%8A%A1%E5%99%A8)
  - [3. 使用 express 手动实现一个 http 服务](#3-%E4%BD%BF%E7%94%A8-express-%E6%89%8B%E5%8A%A8%E5%AE%9E%E7%8E%B0%E4%B8%80%E4%B8%AA-http-%E6%9C%8D%E5%8A%A1)
  - [4. webpack v5 的变化](#4-webpack-v5-%E7%9A%84%E5%8F%98%E5%8C%96)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 配置 devServer

1. 之前我们如果更改了文件中代码，必须手动的打包文件，然后启动浏览器运行。有没有自动化的方式，检测文件变化，然后自动打包，并刷新浏览器的方法呢？答案是yes。有下面三种方式。

## 1. 使用 `npm run watch` 命令

1. 首先我们要修改package.json文件的内容，将
   ```
    "scripts": {
      "bundle": "webpack"
    }
   ```
   修改为：
   ```json
      "scripts": {
        "watch": "webpack --watch"
      }
   ```

2. 这样，webpack 会自动检索被打包的文件，发现文件有变化，就重新打包。

3. 此时我们使用的命令是：`npm run watch`

## 2. 使用 `webpack-dev-server`

1. webpack-dev-server 是 webpack 提供的一个服务器。我们配置好以后。这个服务器会提供 http 服务。并指定一个 url。服务器监视文件的变化，发现变化就重新打包，并自动刷新浏览器。

2. 相关资料：
   - [Webpack 官方文档：DevServer](https://webpack.js.org/configuration/dev-server/)
   - [DevServer - github 地址](https://github.com/webpack/webpack-dev-server)

3. 安装
   - `npm install webpack-dev-server --save-dev`
   
4. 基本配置
   ```javascript
      // webpack.config.js
      module.exports = {
          mode: 'development',
      
          // 建立映射关系
          devtool: 'cheap-module-eval-source-map',
          // 配置webpack-dev-server
          // webpack提供的一个http服务
          devServer: {
              // 设置服务器的根路径
              // 所有的文件以及目录都必须放在这个根路径下，服务器才可以访问到
              contentBase: './dist',
                // 第一次启动时，自动打开浏览器
              open: true，
              // 配置端口，默认是8080
              port: 8080
          }
      }
   ```
5. 参数说明
   - `devServer`  表示配置 webpack-dev-server。
   - `contentBase` 表示设置服务器的根路径。
   - `open`  属性为true，表示服务器第一次启动时，自动打开浏览器。
   - `port` 用来设置端口号，默认是 8080。
   - 
6. 还可以配置 `proxy` 字段实现代理转发，create-react-app 配置代理就是通过 devServer 实现的。
   
7. 还需要配置 package.json 文件。
   ```json
      "scripts": {
        "watch": "webpack --watch",
        "start": "webpack-dev-server"
      }
   ```
   在 scripts 对象中添加了一句：`"start": "webpack-dev-server"`  

8. 这样，我们可以使用 `npm run start` 命令，启动 webpack-dev-server 服务。

9. 使用 webpack-dev-server 不会生成 dist 目录，将打包后的文件放入了内存之中。这样可以加快打包速度。

### 1. 使用 devServer 实现前端路由转发


#### 1. 配置前端路由转发（只在本地起作用）

1. 我们使用 react-router-dom 的时候，使用 BrowserRouter 设置前端路由，那么进行路由切换的时候，就会向后端发送请求，由于是前端路由，那么后端不会对这个请求进行处理，返回结果肯定是 404，那么怎么解决呢？
     ```javascript
        module.exports = {
          mode: 'development',
          devServer: {
              contentBase: './dist',
              open: true，
              port: 8080,
              historyApiFallback: true,
          }
      }
     ```
2. 设置 `historyApiFallback` 为 `true`，只要后端返回结果是 404，devServer 就会将 index.html 作为结果，转发给浏览器，由于 index.html 引用了我们设置好的前端路由，那么自然会根据当前地址栏的地址渲染相应的组件，间接实现了前端路由跳转。

3. `historyApiFallback` 这个配置项使用 html5 history api，就可以实现上面的效果。

4. BrowserRouter 底层就是基于 html5 history api，所以可以使用这个配置项。

5. `historyApiFallback` 的属性值还可以设置为对象，如下所示：
     ```javascript
        module.exports = {
          mode: 'development',
          devServer: {
              contentBase: './dist',
              open: true，
              port: 8080,
              historyApiFallback: {
                  rewrites: [
                      { from: /^\/$/, to: '/views/landing.html' },
                      { from: /^\/subpage/, to: '/views/subpage.html' },
                      { from: /./, to: '/views/404.html' },
                      {
                        from: /^\/libs\/.*$/,
                        to: function(context) {
                            return '/bower_components' + context.parsedUrl.pathname;
                        }
                      }
                  ]
              },
          }
      }
     ```

6. 配置项说明：
   - `rewrites` 字段相当于是重定向。
   - `from` 表示我们访问的路径。
   - `to` 表示目标路径，也就是如果我们访问的`from` 代表的路径，那么就重定向到 `to` 代表的路径。
   - `to` 的值还可以设置为函数，函数接收 `context` 作为参数，`context` 有三个属性：
     - `parsedUrl`: Information about the URL as provided by the URL module's url.parse.
     - `match`: An Array of matched results as provided by String.match(...).
     - `request`: The HTTP request object

#### 2. 对根路径进行代理

1.  默认情况下，`devServer` 不会对根路径进行转发，如果想对根路径进行代理，则必须配置 `index` 字段为空字符串或者 `false`，如下所示：
   ```javascript
          module.exports = {
              mode: 'development',
              devServer: {
                  index: '',
              }
          }
   ```

### 2. 使用 devServer 实现代理转发（转发到后端服务器）

1. devServer 使用了 `http-proxy-middleware` 这个包来实现的代理转发。因此，我们可以使用 `http-proxy-middleware` 中的相关配置项来对 devServer 中的代理进行配置。

2. [http-proxy-middleware - github 主页](https://github.com/chimurai/http-proxy-middleware)

3. [http-proxy-middleware 配置项](https://github.com/chimurai/http-proxy-middleware#options) 

4. 基本的代理转发
   ```javascript
      module.exports = {
          devServer: {
              proxy: {
                  '/react/api': 'http://www.dell-lee.com'
              }
          }
      }
   ```
5. 这样实现的是最基本的代理转发。路径是相对路径，所以任何以 `/react/api` 开头的接口，都会被代理到 `http://www.dell-lee.com/react/api/` 下：`/react/api/header.json` ---> `http://www.dell-lee.com/react/api/header.json`。

6. Proxy 的其他配置：
     ```javascript
         module.exports = {
              mode: 'development',
              devServer: {
                 proxy: {
                     changeOrigin: true,
        
                     headers: {
                        host: 'www.dell-lee.com',
                        cookies: ''
                     },

                     secure: false,
                     context: ['/auth', '/api'],
                     target: 'http://localhost:3000',
                  }
              }
          }
     ```       
   - `changeOrigin` 在进行代理转发的时候，代理会将原始的请求头带上。设置了 `changeOrigin` 为 `true`，则不会携带原始的请求头，一般情况下，都会设置上这个字段。
   - `headers` 值为对象。配置代理的请求头，代理会带着这个请求头发送到服务器端。
   - `secure` 布尔值，配置是否对 https 生效。
   - `context` 数组，如果我又多个接口，都想代理到 `http://localhost:3000` 下，那么将这多个接口放到一个数组中，并将数组赋给 `context`
   - `target` 代理的目标地址。

7. 对接口做的高级配置
   ```javascript
      module.exports = {
             devServer: {
                proxy: {
                  '/react/api': {
                         target: 'http://www.dell-lee.com',

                         bypass: function(req, res, proxyOptions) {
                             if (req.headers.accept.indexOf('html') !== -1) {
                                 console.log('Skipping proxy for browser request.');
                                 return '/index.html';
                              }
                         },
                         pathRewrite: {
                             'header.json': 'demo.json'
                         }
                     }
                }
             }
      }
   ```
   - `target` 配置代理的目标地址。
   - `pathRewrite` 用来改写我们我们接口的一些内容。比如说，我们真正想请求的接口地址是：`/react/api/header.json`，但是这个接口目前不可用，只能使用 `/react/api/demo.json` 这个接口，但是我们又不能直接改动接口，所以使用 `pathRewrite` 这个字段，将 `header.json` 重写为 `demo.json`，表面上发送的请求是 `/react/api/header.json`，实际上请求的是 `/react/api/demo.json`。`pathRewrite` 必须是一个对象或者函数。如果是对象的话，那么key 是需要改写的路径，支持字符串形式的正则，如 `'^\api'`，表示以 `\api` 开头的路径。而其 value 就是重写后的路径。
   - `bypass` 对代理做一些精细的控制，值为函数。拦截请求或者响应可以访问 `request`、`response`、`proxyOption`。返回值有下面三种情况：
     - 返回 `null` 或 `undefined`，使用原来的代理处理请求。
     - 返回 `false`，给这次请求返回一个 `404` 错误。
     - 返回一个 `path`，作为新的代理路径。

8. **注意**：需要代理的路径，如果我们将其配置为一个对象，那么必须具有 target 属性，表示代理的目标 host。如下所示：
   ```js
      proxy: {
          '\api': {
              target: 'http://localhost:8008',
              // other config
     
          }
      }
   ```

10. proxy 常用配置项总结：
   
    配置项|说明
    :---:|:---:
    target|配置代理的目标地址。
    changeOrigin|在进行代理转发的时候，代理会将原始的请求头带上。设置了 `changeOrigin` 为 `true`，则不会携带原始的请求头，一般情况下，都会设置上这个字段。
    headers|值为对象。配置代理的请求头，代理会带着这个请求头发送到服务器端。
    secure|布尔值，配置是否对 https 生效。
    context|数组，如果我有多个接口，都想代理到 `http://localhost:3000` 下，那么将这多个接口放到一个数组中，并将数组赋给 `context`。
    pathRewrite|用来改写我们我们接口的一些内容。
    bypass|对代理做一些精细的控制，值为函数。拦截请求或者响应可以访问 `request`、`response`、`proxyOption`。

## 3. 使用 express 手动实现一个 http 服务

1. 结合 express 手动实现一个服务器。可以实现自动打包的功能。

2. webpack 可以在 Node 环境下使用，因为 webpack 提供了一系列在 Node 环境下使用的 API。参考文档：[Node Api](https://v4.webpack.js.org/api/node/#installation)

## 4. webpack v5 的变化

1. `devServer` 的变化：
   ```javascript
      module.exports = {
          devServer: {
             contentBase: path.join(__dirname, 'dist'),
             open: true,
             port: 8080
    },
      }
   ```
   - `contentBase` 必须配置绝对路径

2. 启动命令的变化
   ```json
      {
         "scripts": {
             "start:dev": "webpack serve"
         }
      }
   ```
   - 执行的命令是：`"webpack serve"`，而不是 `"webpack-dev-server"`。