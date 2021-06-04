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

## 8. 算法 1 : 二叉树层序遍历和s形的层序遍历

## 9. 算法 2 : 如何在从左到右升序,从上到下升序,每一行第一个元素大于上一行最后一个元素的二维数组中查找某个数字的索引?如果是一维数组呢?

## 10. 算法 3 : 二分法的实现

## 11. 算法 4 : 单链表反转

##  12. 算法 1 : promise 并发控制

## 13. 算法 2 : 前 k 个元素

## 14. 实现一个带缓存的求阶乘函数

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
