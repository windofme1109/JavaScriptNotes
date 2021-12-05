# koa-body 的使用

## 1. 参考资料

1. [koa-body 文件上传自定义文件夹及文件名称](http://www.ptbird.cn/koa-body-diy-upload-dir-and-filename.html)
2. [koa2 使用 koa-body 代替 koa-bodyparser 和 koa-multer](http://www.ptbird.cn/koa-body.html)

## 2. koa-body 的说明

1. Koa 本身不提供对 post 请求体的解析，需要我们使用中间件完成解析。

2. post 请求的 Content-Type 主要有：`application/json`、`application/x-www-urlencoded`、`multipart/form-data`。

3. koa-bodyparser 只能解析 Content-Type 为 `application/json` 和 `application/x-www-urlencoded` 的 post 请求。要解析 Content-Type 为 `multipart/form-data` 的 post 请求，还需 koa-multer。

4. koa-body 这个中间件实现了 koa-bodyparser 和 koa-multer 的功能，能够同时支持下列的 Content-Type 的 post 请求：
   - multipart/form-data
   - application/x-www-urlencoded
   - application/json
   - application/json-patch+json
   - application/vnd.api+json
   - application/csp-report
   - text/xml
   
5. 对于上传文件、提交表单、访问接口等常见的 post 请求，使用一个 koa-body 中间件都可以解析。

6. 对于 Content-Type 为 `application/json` 和 `application/x-www-urlencoded` 的 post 请求，koa-body 对 post 传递的 post 请求体数据进行解析，将其解析为键值对，挂载到 `ctx.request.body` 上。

7. Content-Type 为 `multipart/form-data` 的 post 请求，一般是上传文件，将解析后的文件信息挂载到 `ctx.request.files` 上。

## 2. 基本用法

1. 安装：`npm install koa-body`

2. 基本用法：
   ```js
       const Koa = require('koa');
       const koaBody = require('koa-body');

       const app = new Koa();

       app.use(koaBody());
       app.use(ctx => {
           ctx.body = `Request Body: ${JSON.stringify(ctx.request.body)}`;
       });

       app.listen(3000);
   ```
3. 与 koa-router 一起使用，注意，这里引入 koa-body 是全局引入，因为考虑到很多地方都有 post 请求或者是文件上传请求，没必要只在路由级别引入。示例代码如下：
   ```js
      const koaBody = require('koa-body');
      const app = new koa();
      app.use(koaBody());
   
      // router
      const Router  = require('@koa/router');
      Router.post('/users', (ctx) => {
         console.log(ctx.request.body);
         // => POST body
         ctx.body = JSON.stringify(ctx.request.body);
      });
      
      app
       .use(Router.routes())
       .use(Router.allowedMethods());
   ```

## 3. 配置项

1. koa-body 接收配置项作为参数，用来决定 koa-body 解析 post 请求体的表现，用法如下：
   ```js
      const koaBody = require('koa-body');
      const app = new koa();
      
      app.use(koaBody({
           ultipart:true, // 支持文件上传
           encoding:'gzip',
           formidable:{
               uploadDir:path.resolve(__dirname, '../', 'public', 'my-images'), // 设置文件上传目录
               keepExtensions: true,    // 保持文件的后缀
               maxFieldsSize: 2 * 1024 * 1024, // 文件上传大小
               onFileBegin:(name,file) => { // 文件上传前的设置
                    // console.log(`name: ${name}`);
                    // console.log(file);
               },
           }
      }));
   ```
### 1. 基本的配置项

1. 基本的配置项如下表所示：

   参数名|描述|类型|默认值
   :---:|:---:|:---:|:---:
   patchNode|将请求体打到原生 node.js 的 ctx.req 中|Boolean	|false
   patchKoa|将请求体打到 koa 的 ctx.request 中|Boolean|true
   jsonLimit|JSON 数据体的大小限制|String / Integer|1mb
   formLimit|限制表单请求体的大小|String / Integer|56kb
   textLimit|限制 text body 的大小|String / Integer|56kb
   encoding|表单的默认编码|String|utf-8
   multipart|是否支持|multipart-formdate|的表单|Boolean	false
   urlencoded|是否支持 urlencoded 的表单|Boolean|true
   text|是否解析 text/plain 的表单|Boolean|true
   json|是否解析 json 请求体|Boolean|true
   jsonStrict|是否使用 json 严格模式，true 会只处理数组和对象|	Boolean|true
   formidable|配置更多的关于 multipart 的选项|Object|{}
   onError|错误处理|Function|function(){}
   stict|严格模式,启用后不会解析 GET, HEAD, DELETE 请求	|Boolean|true


### 2. formidable 配置项

1. koa-body 解析 Content-Type 为 `multipart/form-data` 的post 请求的能力是由 formidable 这个包提供的，因此 koa-body 中的配置项 formidable 就是用来配置 formidable 的表现的。

2. formidable 的基本配置项：

   参数名|描述|类型|默认值
   :---:|:---:|:---:|:---:
   maxFields|限制字段的数量|Integer|1000
   maxFieldsSize|限制字段的最大大小|Integer|2 * 1024 * 1024
   uploadDir|文件上传的文件夹|String|os.tmpDir()
   keepExtensions|保留原来的文件后缀|Boolean|false
   hash|如果要计算文件的 hash，则可以选择 md5/sha1|String	|false
   multipart|是否支持多文件上传|Boolean|true
   filename|给上传的文件重新命名，如果是函数的话，必须返回字符串，这个字符串会和uploadDir进行拼接|function|undefined
   onFileBegin|文件上传前的一些设置操作|Function	function(name,file){}

3. 有关 formidable 更详细的配置项信息，请查看 formidable 在 github 上面的文档：[formidable - options](https://github.com/node-formidable/formidable)

4. 有用的配置项：
   - uploadDir
   - keepExtensions
   - onFileBegin
   - filename

## 4. 上传文件

1. 配置 formidable 选项，使得 koa 可以解析上传的文件。

2. 基本配置：
   ```js
      app
       .use(koaBody({
           multipart: true,
           formidable: {
               // uploadDir: path.resolve(__dirname, '../', 'public', 'my-images'),
               keepExtensions: true,
               maxFileSize: 100 * 1024 * 1024,
           }
       }));
   ```
   指定了保存上传文件的路径，并且保存文件的原始后缀，设置文件最大体积为 100 M。

3. 上传一个文件文件进行测试：

4.

