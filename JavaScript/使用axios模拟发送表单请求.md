<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Axios 模拟发送表单请求/模拟上传文件](#axios-%E6%A8%A1%E6%8B%9F%E5%8F%91%E9%80%81%E8%A1%A8%E5%8D%95%E8%AF%B7%E6%B1%82%E6%A8%A1%E6%8B%9F%E4%B8%8A%E4%BC%A0%E6%96%87%E4%BB%B6)
  - [1. 参考资料](#1-%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99)
  - [2. 表单请求的基本说明](#2-%E8%A1%A8%E5%8D%95%E8%AF%B7%E6%B1%82%E7%9A%84%E5%9F%BA%E6%9C%AC%E8%AF%B4%E6%98%8E)
  - [3. Axios 模拟发送表单请求](#3-axios-%E6%A8%A1%E6%8B%9F%E5%8F%91%E9%80%81%E8%A1%A8%E5%8D%95%E8%AF%B7%E6%B1%82)
    - [1. 指定 transformRequest](#1-%E6%8C%87%E5%AE%9A-transformrequest)
    - [2. 使用 URLSearchParams](#2-%E4%BD%BF%E7%94%A8-urlsearchparams)
  - [4. Axios 实现文件上传](#4-axios-%E5%AE%9E%E7%8E%B0%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0)
    - [1. 原生表单的方式实现上传](#1-%E5%8E%9F%E7%94%9F%E8%A1%A8%E5%8D%95%E7%9A%84%E6%96%B9%E5%BC%8F%E5%AE%9E%E7%8E%B0%E4%B8%8A%E4%BC%A0)
    - [2. 使用 Axios 实现上传](#2-%E4%BD%BF%E7%94%A8-axios-%E5%AE%9E%E7%8E%B0%E4%B8%8A%E4%BC%A0)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Axios 模拟发送表单请求/模拟上传文件

## 1. 参考资料

1. []()

2. []()

3. []()

## 2. 表单请求的基本说明

1. 在前端页面，我们使用表单提交一个 post 请求，页面的代码如下所示：
```html
   <form action="/submit" method="post" enctype="application/x-www-form-urlencoded">
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
3. 而表单提交的 post 请求的报文格式和 get 请求中的查询字符串相同，如下所示：
   ```
      name=jack&password=12345678&email=apple%40176.com
   ```
4. 其中 `key` 和 `value` 都是经过 `uri` 编码后再使用 `=` 相连，多个 `key=value` 使用 `&` 进行分隔。

5. 通过上面的说明，要模拟一个表单请求，需要有满足以下几点：
   1. 请求方法为 post
   2. 请求头中的 content-type 必须为 `application/x-www-form-urlencoded`
   3. 请求体的数据格式是查询字符串格式（key 和 value 经过 uri 编码）

## 3. Axios 模拟发送表单请求

1. **注意**：如果我们指定了请求方法是 post，那么 Axios 会根据配置项中，data 的类型自动指定请求头中的 Content-Type 的值。例如：data 如果为字符串或者是 URLSearchParams，那么 Content-Type 会被指定为 `application/x-www-form-urlencoede`。如果 data 为纯对象，那么 `Content-Type` 会被指定为 `application/json`，如果 data 是 FormData 的实例，那么 `Content-Type` 会被指定为 `multipart/formdata`。

2. 当前的 axios 版本是：`0.24.0`。

### 1. 指定 transformRequest
 
1. 指定 Axios 中的配置项中的 transformRequest 属性值为处理函数，作用是在发送请求之前转换配置项中的 data 属性中指定的数据。

2. 适用的请求方法是：`put`、`post`、`patch`、`delete`。

3. `transformRequest` 的属性值可以是一个数组，每个数组的元素是一个函数，其中，数组的最后一个函数必须返回一个字符串或者是 `Buffer`、`ArrayBuffer`、`FormData`、`Stream` 的实例。

4. 将 transformRequest 属性值指定为函数，并且这个函数的返回值是查询字符串，如下所示;
   ```js
      function transformData(data, headers) {
          return Object.entries<any>(data).map(item => {
                const name = encodeURIComponent(item[0]);
                const value = encodeURIComponent(item[1]);
                    return `${name}=${value}`;
           }).join('&');
      }
      
      axios.post('/user', {
            data: {
                username: 'jacks',
                password: '123456'
            },
            transformRequest: transformData
      });
   ```
5. 也可以在函数中使用 URLSearchParams 生成一个查询字符串：
   ```js
      function transformData(data, headers) {
          const urlSearchParams = new URLSearchParams();
           Object.entries(data).forEach(item => {
                urlSearchParams.append(item[0], String(item[1]));
           });
           
            return urlSearchParams.toString();
      }
   
      axios.post('/user', {
            data: {
                username: 'jacks',
                password: '123456'
            },
            transformRequest: transformData
      });
   ```
### 2. 使用 URLSearchParams

1. 使用 URLSearchParams 构造查询字符串，然后将 data 指定为查询字符串。
   ```js
      const urlSearchParams = new URLSearchParams();
        const data = {
            name: 'jack',
            password: 12345678,
            email: 'apple@176.com'
        }

        Object.entries(data).forEach(item => {
            urlSearchParams.append(item[0], String(item[1]));
        });
        axios.post('/user', {
            data: urlSearchParams
        });
   ```

## 4. Axios 实现文件上传

### 1. 原生表单的方式实现上传

1. 可以通过表单的方式实现文件上传：
   ```html
      <form action="/submit" method="post" enctype="multipart/form-data">
          <input type="file" >
          <button type="submit">上传</button>
      </form>
   ```
2. 使用表单上传文件，有下面几个要求：
   - 指定 form 元素的 action 为上传的地址
   - 请求方法为 post
   - 还要指定 enctype 为 `multipart/form-data`
   - input 的类型为 file。

3.选定文件，点击上传后，浏览器会自动解析上传的文件，将其放在 post 请求的报文主体上，并指定 post 请求的 Content-Type 为 `multipart/form-data`。请求头中的 `Content-Type` 字段值为 `multipart/form-data`，在 `Content-Type` 中可能还附带如下所示的内容分隔符：
   ```
      Content-Type: multipart/form-data; boundary=----WebKitFormBoundary4Hsing01Izo2AHqv
   ```
   boundary 表示报文主体内容的分隔符，因为我们可能上传多个文件，因此使用 boundary 进行分隔。

4. 假设我们在浏览器端上传一个文件，浏览器解析后的请求报文主体的内容如下：
   ```
      ------WebKitFormBoundaryQRppr4oLReN6qlP0
      Content-Disposition: form-data; name="file"; filename="git.jpg"
       Content-Type: image/jpeg

       
      // 二进制数据
      ------WebKitFormBoundaryQRppr4oLReN6qlP0--
   ```
5. 而这次请求的 `Content-Type` 是：
   ```
      multipart/form-data; boundary=----WebKitFormBoundaryQRppr4oLReN6qlP0
   ```
6. 通过这样的方式，就能实现最基本的文件上传。

7. 使用表单上传文件有一个缺点，就是上传后，页面会刷新，浏览器地址栏中的地址会变成上传文件的地址，这个用户体验就非常不好。

### 2. 使用 Axios 实现上传

1. 使用 Axios 也能实现文件上传。

2. 当我们使用 input 上传文件的时候，会触发 input 元素的 change 事件。在 change 事件的处理函数中，我们能拿到事件对象 event，通过 event.target 获得当前事件的目标元素，这个目标元素有一个 files 属性，就是存放读取到的文件信息。因为可以同时选择多个文件，files 是一个类数组对象，里面存放了多个和上传文件相关的 file 对象。

3. 拿到上传文件信息后，我们需要手动构造 post 请求体的报文主体内容。使用 FormData 构造函数，生成一个 FormData 实例，然后调用这个实例的 append 方法，将 file 对象添加到 FormData 实例中。

4. 接下来是 Axios 的配置，将 Axios 的请求方法 method 设置为 post，然将 data 属性设置为 FormData 实例，这样 Axios 会自动设置请求的 Content-Type 为 multipart/form-data，不需要我们手动设置 Content-Type。

5. 示例代码（jsx 格式） - html 结构：
   ```html
      <div className="moon-upload-file-form">
                    <input onChange={handleInputFile} type="file" multiple={true}/>
      </div>
   ```
6. handleInputFile 实现：
   ```js
      import axios from 'axios';
   
      const handleInputFile = async (e) => {
        const files = e.target.files;
        if (!files) {
            return;
        }

        const fileList = Array.from(files);
        const formData = new FormData();
        fileList.forEach((file, index) => {
            formData.append(`file`, file);
        });
        try {
            const res = await axios({
                url: '/upload/images',
                method: 'post',
                data: formData
            }).then(data => data).catch(err => err);

             console.log(res);
        } catch (e) {
        
        }
        
       

      }
   ```
7. 这样就通过 Axios 实现了文件上传。