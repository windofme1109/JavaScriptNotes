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
  
1. 快排和合并排序的时间复杂度是 O(nlogn)

2. 冒泡、插入、选择的时间复杂度是 O(n^2)

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

1. while 循环
   ```js
      function binarySearch(arr, key) {
          let start = 0;
          let end = arr.length - 1;
          let guess;

          while (start <= end) {
              guess = Math.floor((start + end) / 2);

              if (arr[guess] > key) {
                  end = guess - 1;
              } else if (arr[guess] < key) {
                  start = guess + 1;
              } else {
                  return guess;
              }
          }

         return -1;
     }
   ```

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

1. 版本 1：
   ```js
      function curriedFunc(fn) {
          if (typeof fn !== 'function') {
              throw Error('fn must be function');
          }
          return function curried() {
              const args = Array.prototype.slice.call(arguments);
              if (args.length !== fn.length) {
                  return function () {
                      return curried.apply(this, args.concat(...arguments));
                  }

              }

              return fn.apply(this, args);
          }
      }
   ```
2. 版本 2：
   ```js
      function curriedFunc(fn, params) {
          return function () {
              let args = Array.prototype.slice.call(arguments);

              if (params !== undefined) {
                  args = args.concat(params);
              }

              if (args.length !== fn.length) {
                  return curriedFunc(fn, args);
              }

              return fn.apply(null, args);
          }
      }
   ```

## 18. 函数组合 —— compose

1. 版本 1：
   ```js
      /**
       * 组合函数，将多个单参数函数组合为一个函数
       * 接收多个单参数函数，这些单参数函数的执行顺序是从右向左
       * @param args
       * @returns {function(*=): *}
       */
      const compose = (...args) => {
          // 组合函数从右向左执行
          // 因此需要将其翻转过来
          // 因为 reduce 的执行顺序是从左向右
          args.reverse();

          return function (value) {
              return args.reduce((acc, fn) => {
            return fn(acc)
              }, value)
          }

      }
   ```

## 19. 偏函数 —— partial

1. 版本 1：
   ```js
      const partial = function (fn) {

          // 取出占位的参数
          const fixedArgs = Array.prototype.slice.call(arguments, 1);
          return function () {
              const restArgs = Array.prototype.slice.call(arguments);
              let length = fixedArgs.length;
              // 新建一个空数组，用来接收完整的参数
              // 避免多次调用固定参数后的函数，对 fixedArgs 造成污染
              const args = Array(length);

              let position = 0;

              for (let i = 0; i < length; i++) {
                  // 向 args 数值填充数值
                  // 判断固定的参数中是否有占位参数，如果有，就替换，没有，就直接取固定参数
                  args[i] = fixedArgs[i] === _ ? restArgs[position++] : fixedArgs[i];
             }

             while (position < restArgs.length) {
                 // 固定的参数可能少于 fn 的实际参数个数
                 // restArgs 中除去填补 _，剩下的参数补上
                 args.push(restArgs[position++]);
             }     

             return fn.apply(this, args);
         }
     }
   ```

## 20. 算法 1 : 给定一串数字, 求它全排列结果

1. 不带重复元素的：
   ```js
      function permute(nums) {
          let ret = [];
          let length = nums.length;

          function backtrace(path) {
              if (path.length === length) {
                  return ret.push([...path]);
              }

              for (let i = 0; i < length; i++) {
                  if (!path.includes(nums[i])) {
                      path.push(nums[i]);
                      backtrace(path);
                      path.pop()
                  }
              }
          }

          backtrace([]);
          return ret;
      }
   ```

2. 带重复元素的
   ```js
      function permuteUnique(nums) {
          let ret = [];
          let index = [];
          let length = nums.length;

          function backtrace(path, index) {
              if (path.length === length) {
                  return ret.push([...path]);
              }

              for (let i = 0; i < length; i++) {
                  if ((nums[i] === nums[i - 1] && index.inclues(i - 1)) || index.inclues(i)) {
                      continue;
                  }

                  path.push(nums[i]);
                  index.push(i);
                  backtrace(path, index);
                  path.pop();
                  index.pop();
              }
          }

          backtrace([], index);

          return ret;
      }
   ```

## 21. 算法 2 : 实现类似百度那种联想搜索(模糊匹配)

## 22. 数组去重怎么实现,不用 Set 怎么实现

1. 使用 reduce + sort
   ```js
      function arrWithoutRepeat1(arr) {
          // 排序
          arr.sort((a, b) => a - b);
      
          let ret = arr.reduce((acc, cur) => {
              if (acc.length === 0 || acc[acc.length - 1] !== cur) {
                  acc.push(cur);
              }
      
              return acc;
          }, []);
      
          return ret;
      }
   ```
2. 使用 filter + indexOf
   ```javascript
      function arrWithoutRepeat(arr) {
          return arr.filter((item, index) => {
              // 判断 item 首次出现的位置是否与当前 item 的索引相同
              return index === arr.indexOf(item);
          });
      }
   ```
3. 使用 set
   ```js
      function arrWithoutRepeatUsingSet(arr) {
          return [...new Set(arr)];
      }
   ```
4. 使用 for + indexOf
   ```js
      function arrWithoutRepeat(arr) {
          let ret = [];
      
          for (let i = 0; i < arr.length; i++) {
              if (ret.indexOf(arr[i]) === -1) {
                  ret.push(arr[i]);
              }
          }
      
          return ret;
      }
   ```

## 23. 算法 1 : rgb 转 16 进制函数

1. rgb 是三原色，每个颜色使用 0 - 255 之间的数字表示。

2. 颜色也可以使用 16 进制表示。以 # 开头，一共六位，每两位表示一个颜色。

3. 核心就是实现 10 进制转换为 16进制。

4. 使用 toString。toString 可以接收一个数字作为参数，这个参数可以将指定的数字转换为这个参数指定的进制的数字。
   ```js
      function rgb2Hex(red, green, blue) {
          return [red, green, blue].reduce((acc, cur) => {
              return acc + cur.toString(16);
          }, '#');
      }
   ```

2. 自己实现一个 10 进制转 16 进制的算法：
   ```js
      function rgb2Hex(red, green, blue) {
          return [red, green, blue].reduce((acc, cur) => {
              return acc + toHex(cur);
          }, '#');
      }

      /**
       * 10 进制转 16 进制
       * @param num
       * @param radix
       * @returns {string}
       */
       function toHex(num, radix = 16) {
           let base = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'A', 'B', 'C', 'D', 'E', 'F'];
           let remainder = num % radix;
           let quotient = Math.floor(num / radix);

           let ret = [remainder];


           while (quotient !== 0) {
               remainder = quotient % radix;
               quotient = Math.floor(quotient / radix);
               ret.push(remainder);
           }

          return ret.reverse().map(item => {
              return base[item];
          }).join('');
       }      
   ```

## 24. 算法 2 :
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
