# JavaScript 控制并发请求

## 1. 参考资料

1. [字节跳动面试官：请用JS实现Ajax并发请求控制](https://juejin.cn/post/6916317088521027598)

2. [js实现并发请求控制](https://www.jianshu.com/p/be92059cd7f2)

3. [JS控制并发请求数量](https://blog.csdn.net/Dilomen/article/details/110326270)

4. [js实现并发请求控制](https://www.jianshu.com/p/be92059cd7f2)

## 2. 描述

1. 并发请求控制：有 100 请求，页面一次只能最多发送 10 个请求，如何在最短时间内，将这 100 个请求发送出去

2. 更通用的描述，就是可以手动控制并发请求的数量。

## 3. 实现

### 1. 基本思路

1. 基本思路是任务队列结合递归实现。

2. 首先维护一个任务队列，里面是所有待发送的请求。

3. 根据当前页面允许的最大请求次数 n 将任务队列中前 n 个请求发送出去。

4. 如果哪个请求返回了结果，那么就在其回调中继续发送任务队列的下一个请求。

5. 队列为空，说明请求全部发送完毕。

### 2. 代码实现

#### 1. 基础的Promise + 任务队列 + 递归

1. 基本要求：
   - 要求最大并发数 maxNum
   - 每当有一个请求返回，就留下一个空位，可以增加新的请求
   - 所有请求完成后，结果按照 urls 里面的顺序依次打出

2. 示例代码：
   ```js
      function fetch(url) {
          return new Promise((resolve, reject) => {
               // const xhr = new XMLHttpRequest();
               //
               // xhr.onreadystatechange = function () {
               //     if (xhr.readyState === 4) {
               //         if (xhr.status === 200) {
               //             resolve(xhr.responseText);
               //         } else {
               //             reject()
               //         }
               //     }
               // }
               //
               // xhr.open(url);
               //
               // xhr.send();

               setTimeout(() => {
                   resolve(url);
               }, url);

           })
       }

       function multiRequest(urls = [], maxNum) {
           const len = urls.length;

            // 保存结果的数组
            const result = Array.from({length: len}).fill(false);
            // 当前完成的数量
            let count = 0;
     
         return new Promise((resolve, reject) => {
             while (count < maxNum) {
            next();
        }

        function next() {
            let current = count;
            count++;

            if (current >= len) {
                // 请求全部结束，将 Promise 置为成功状态
                !result.includes(false) && resolve(result);
                // 终止函数执行
                return;
            }
            const url = urls[current];
            console.log(`开始第 ${current} 个请求`, new Date().toLocaleString());
                fetch(url)
                    .then((res) => {
                        console.log(`完成第 ${current} 个请求`, new Date().toLocaleString());
                        result[current] = res;
                        if (current < len) {
                            // 请求没有完成，继续递归
                            next();
                        }
                    })
                    .catch(err => {
                          console.log(`第 ${current} 个请求出错`, new Date().toLocaleString());
                          result[current] = err;
                          if (current < len) {
                              // 请求没有完成，继续递归
                              next();
                          }
                      })
              }
          })
       }
   ```