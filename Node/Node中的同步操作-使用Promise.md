<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [使用 Promise 包装 Node 中的异步操作](#%E4%BD%BF%E7%94%A8-promise-%E5%8C%85%E8%A3%85-node-%E4%B8%AD%E7%9A%84%E5%BC%82%E6%AD%A5%E6%93%8D%E4%BD%9C)
  - [1. File System](#1-file-system)
    - [1. `Promise` 包装回调函数](#1-promise-%E5%8C%85%E8%A3%85%E5%9B%9E%E8%B0%83%E5%87%BD%E6%95%B0)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 使用 Promise 包装 Node 中的异步操作

1. 使用 `Promise` 包装 Node 中的异步操作后，这样可以使用 `async/await` 关键字，实现同步操作。

## 1. File System

1. fs 模块用于操作文件。14.17.6 的版本的 node 的 fs 模块的文档地址是：[Node.js v14.17.6 documentation - fs](https://nodejs.org/dist/latest-v14.x/docs/api/fs.html)

2. fs 支持三种操作模式：回调、同步和 `Promise`。回调模式不用说，这是 node 中最基础的异步操作的实现方式。而对于 fs 模块，每一个涉及到文件的操作，都有同步和异步两种方法，如读取文件，有异步的 readFile() 和同步的 readFileSync()。

### 1. `Promise` 包装回调函数

1. `Promise` 是一个构造函数，也是一个对象。当其作为一个构造函数时，接收一个执行器作为参数，这个执行器也是一个函数，执行器内部是一个异步操作。执行器接收两个参数：resolve 和 reject。resolve 和 reject 也是函数，分别对应异步状态是成功和失败时的操作。

2. fs 模块的回调操作模式：
   ```js
      const fs = require('fs');

      fs.readFile('./a.txt', 'utf-8', (err, data) => {
          if (err) {
              return console.log(err);
          }

          console.log(data);
      })
   ```
3. 使用 `Promise` 包裹 fs 的异步操作：
   ```js
      const fs = require('fs');
      function myReadFileSync(path, encode='utf-8', flag = 'r') {
          return new Promise((resolve, reject) => {
              fs.readFile(path, {encode, flag}, function(err, content) {
                  if (err) {
                      return reject(err);
                  }
                  
                  if (encode === 'utf-8') {
                      content = content.toString();
                  }

                  resolve(content);
              })
          })
      }


      async function getTxt(path) {
          const textContent = await myReadFileSync(path);

          console.log(textContent);
      }
      // this is a test file!!! 
      getTxt('./a.txt');
   
   ```
4. 使用 `Promise` 包装 fs 的异步操作，就可以使用 `async/await` 关键字实现同步操作。

### 2. 直接使用 fs 模块的 `Promise` 模式

1. fs 也支持 `Promise` 模式，使用方式两种：`then` 回调函数和  `async/await`。

2. 要想使用 Promise 模式。引入 fs 模块的方式也要发生变化：
   ```js
      // v14.17.6 支持下面这两种引入 Promise 的方式
      // const fs = require('fs').promises;
      const fs = require('fs/promises');
      
      // v12.18.2 只支持下面这种方式
      // const fs = require('fs').promises; 
   ```
2. `then` 回调函数：
   ```js
      // v14.17.6 支持下面这两种引入 Promise 的方式
      // const fs = require('fs').promises;
      const fs = require('fs/promises');
      
      // v12.18.2 只支持下面这种方式
      // const fs = require('fs').promises;
      
      fs.readFile('../text/a.txt', 'utf-8')
        .then(content => {
            // this is a test file!!!
            console.log(content);
        }, err => {
            console.log(err);
        });
   ```

3. 在 `Promise` 模式下，fs 操作文件的返回值都是 Promise，因此我们可以直接使用 `async/await`。示例如下：
   ```js
      // v14.17.6 支持下面这两种引入 Promise 的方式
      // const fs = require('fs').promises;
      const fs = require('fs/promises');
      
      // v12.18.2 只支持下面这种方式
      // const fs = require('fs').promises;
      
      async function myReadFile(path, encoding='utf-8', flag = 'r') {
          const content = await fs.readFile(path, {encoding, flag});

          console.log(content);
      }
      // this is a test file!!!
      myReadFile('../text/a.txt');
   ```
4. `Promise` 模式省去了使用 `Promise` 构造函数包装的这一步骤，因此推荐直接使用 fs 的 `Promise` 模式。