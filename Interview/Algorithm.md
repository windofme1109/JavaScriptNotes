## 1. 算法 12345678.01 => 123,456,78.01

1. 正则

2. 字符串切分
   ```js
      function thousandSeparator(num) {

          let str = ('' + num).split('.');

   

          let integer = str[0];
          let ret = [];
          for (let i = 0; i < integer.length; i = i + 3) {
              let part = integer.slice(i, i + 3);
              ret.push(part);
          }
          str[0] = ret.join(',');

          return str.join('.');
      }
   ```
## 2. 算法 12345678.01 => 12,345,678.01

1. 正则
   ```js
      /**
        * 数字使用千分位分隔
        * @param num
        * @returns {string}
        */
        function thousandSeparator2(num) {
            let str = ('' + num).split('.');

            let pattern = /\B(?=(\d{3})+$)/g;

            let newNumberBySeparator = str[0].replace(pattern, ',');

            str[0] = newNumberBySeparator;

            return str.join('.');
        }
   ```
2. 字符串切分
   ```js
       function thousandSeparator2(num) {
          let str = ('' + num).split('.');
          // let pattern = /\B(?=(\d{3})+$)/g;
          // let newNumberBySeparator = str[0].replace(pattern, ',');
          let integer = str[0];
          let newNumberBySeparator = [];

          let remainder = integer.length % 3;

          newNumberBySeparator.push(integer.slice(0, remainder));

          for (let i = remainder; i < integer.length; i = i + 3)       {
              let part = integer.slice(i, i + 3);
              newNumberBySeparator.push(part);
          }

          str[0] = newNumberBySeparator.join(',');

          return str.join('.');
      }
   ```
## 3. 算法题 - 手写快排

## 4. 算法题 - 查找一个字符串中出现次数最多的字符

1. split + reduce
   ```js
      /**
        *
        * 字符串中出现次数最多的字母
        *
        */

        function mostCountLetterOfString(s) {
            let strArr = s.split('');

            let ret = strArr.reduce((acc, cur) => {
                if (acc[cur]) {
                    acc[cur]++
                } else {
            acc[cur] = 1;
                }

                return acc;
            }, {});

            let count = 0;
            let letter = '';

            for (let key of Object.keys(ret)) {
                if (ret[key] > count) {
                    count = ret[key];
                    letter = key;
                }
            }
    
            return letter;
      }

      let str = 'afdkldhjaaaaaerweerwreewrwerwerewrew';

      let result = mostCountLetterOfString(str);

      // e 
      console.log(result);
   ```   
## 5. 算法复杂度层面比较一下快排和其他排序

## 6. 算法 1 : 数组中出现最多的数字
 
1. reduce
   ```js
      /**
        * 
        * 数组中出现次数最多的数字
        * 
        */

       function mostCountNumOfArr(arr) {
           let ret = arr.reduce((acc, cur) => {
               if (acc[cur]) {
                   acc[cur]++
               } else {
                   acc[cur] = 1;
               }

               return acc;
           }, {});

          let count = 0;
          let number = '';

          for (let key of Object.keys(ret)) {
              if (ret[key] > count) {
                  count = ret[key];
                  number = key;
              }
          }

          return number * 1;
      }

      const num = [1, 2, 3, 4, 5, 2, 1, 3, 2, 4, 2, 4, 2, 4, 8, 9, 2, 3, 4, 2, 9, 3, 0, 7, 8, 4, 2, 3, 4];

      let ret = mostCountNumOfArr(num);
      // 2
      console.log(ret);
   ```

## 7. 算法 2 : 斐波那契数列

1. 基础递归版本
   ```js
      /**
       * 基础版 —— 正常递归
       * @param n
       * @returns {number|*}
       * @constructor
       */
      function Fibonacci(n) {
          if (n === 0) {
              return 0;
          }

          if (n === 1) {
              return 1;
          }

          return Fibonacci(n - 1) + Fibonacci(n - 2);
      }
   ```

2. 尾递归版本
   ```js
      /**
        * 尾调用版本
        * 尾递归的实现，往往需要改写递归函数，确保最后一步只调用自身。做到这一点的方法，就是把所有用到的内部变量改写成函数的参数
        * @param n
        * @param first
        * @param second
        * @returns {number}
        * @constructor
        */
         function Fibonacci(n, first = 1, second = 1) {
             if (n <= 1) {
                 return second;
             }
             / 将递推过程放到了计算参数过程中：first + second
             return Fibonacci(n - 1, second, first + second);
         }
   ```

3. 循环版本
   ```js
      /**
      * 循环版本
      * @param n
      * @returns {number}
      * @constructor
      */
      function Fibonacci(n) {
         let first = 0;
         let second = 1;
         let sum = second;
         if (n <= 1) {
             return sum;
         }

         for (let i = 2; i <= n; i++) {
             sum = first + second;
             first = second;
             second = sum;
         }     

         return sum;
      }
   ```
## 8. 算法 1 : 二叉树层序遍历和s形的层序遍历

## 9. 算法 2 : 如何在从左到右升序,从上到下升序,每一行第一个元素大于上一行最后一个元素的二维数组中查找某个数字的索引?如果是一维数组呢?

## 10. 算法 3 : 二分法的实现

## 11. 算法 4 : 单链表反转

##  12. 算法 1 : promise 并发控制

## 13. 算法 2 : 前 k 个元素

## 14. 实现一个带缓存的求阶乘函数

1. 闭包
   ```js
      function cachedFactorial() {
          let cache = {};

          return function factorial(n) {
              if (cache[n]) {
                  return cache[n];
              }

              let ret;

              if (n <= 1) {
                  return n;
              } else {
                  ret = n * factorial(n - 1);
              }

              cache[n] = ret;

              return ret;

          }

      }
   ```

## 15. 带缓存的斐波那契数列

1. 闭包
   ```js
      function cachedFibonacci() {
         let cached = {};

         return function fibonacci(n) {
             if (cached[n]) {
                 return cached[n];
             }

             let ret;

             if (n <= 1) {
                 return n;
             } else {
                 ret = fibonacci(n - 1) + fibonacci(n - 2);
             }

             cached[n] = ret;

             return ret;
         }

     }
   ```


## 16. 缓存函数

1. 闭包 + 函数也是一个对象
   ```js
      /**
        * 记忆函数，用来缓存函数的结果，仿照 understore 里面的 memoize 函数的写法
        * @param fn
        * @returns {function(*): *}
        */
         function memorized(fn) {
             const memorized = function (key) {
                 let cache = memorized.cache;
                 if (!cache[key]) {
                     cache[key] = fn.apply(this, arguments);
                 }
                 return cache[key];

             }
             memorized.cache = {};

             return memorized;
         }
         // 如果要用好记忆函数的缓存功能，如果是递归调用，同时是数值计算的化，递归的形式最好如下所示，即将返回值函数写成递归形式
         const memorizedFib = memorized(function(n) {
              return n <= 1 ? n : memorizedFib(n - 1) + memorizedFib(n - 2);
          });
         
          const memorizedFac = memorized(function(n) {
             return n <= 1 ? 1 : n * memorizedFac(n - 1);
          });
   ```

2. 单纯闭包
   ```js
      /**
       * 单纯使用闭包实现缓存函数功能
       * @param fun
       * @returns {function(): *}
       */
       function cacheFunc(fun) {
           let cache = {};

           return function() {
               let key = arguments[0];

               if (!cache[key]) {
                   cache[key] = fun.apply(this, arguments);
               }

               return cache[key];
           }
       }

       const cachedFib = cacheFunc(function(n) {
          return n <= 1 ? n : cachedFib(n - 1) + cachedFib(n - 2);
       })
   ```

## 17. 函数柯里化 —— curried

## 18. 函数组合 —— compose

## 19. 偏函数 —— partial

## 15. 算法 1 : 给定一串数字, 求它全排列结果

## 16. 算法 2 : 实现类似百度那种联想搜索(模糊匹配)

## 17. 数组去重怎么实现,不用 set 怎么实现

## 18. 算法 1 : rgb 转 16 进制函数

## 19. 算法 2 :
   ```javascript
      //实现一个retry函数
      //如果fn返回成功,则打印一下,最终结果成功
      //如果fn返回失败,则打印times下,最终结果失败
      retry(fn,times)
      retry(() => {
        console.log('doing')
        return Promise.reject(Error('done'))
      }, 3)
      
      retry(() => {
        console.log('doing')
        return Promise.resolve('done')
      }, 3)
   ```
