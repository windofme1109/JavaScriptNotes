# JavaScript 与函数式编程 - 函子

## 1. 参考资料

1. [Functional Programming in JavaScript, Part 1: The Unit](https://marmelab.com/blog/2018/03/14/functional-programming-1-unit-of-code.html)

2. [Functional Programming in JavaScript, Part 2: The Monoid](https://marmelab.com/blog/2018/04/18/functional-programming-2-monoid.html)

3. [Functional Programming in JavaScript, Part 3: Introduction to Functors and Monads](https://marmelab.com/blog/2018/09/26/functional-programming-3-functor-redone.html)

4. [Functional Programming in JavaScript, Part 4: The Art of Chaining Different Monads](https://marmelab.com/blog/2019/03/20/functional-programming-4-art-of-chaining-monad.html)

5. [Functional JavaScript - Functors, Monads, and Promises](https://dev.to/joelnet/functional-javascript---functors-monads-and-promises-1pol)

6. [Functors in JavaScript](https://hackernoon.com/functors-in-javascript-20a647b8f39f)

7. 本文的主要内容来源于前四篇文章。

## 2. monoid

1. `monoid` 是范畴论的概念，中文名称是：幺半群。这个术语的意思是：对于一个由元素组成的集合，当我们使用一个叫做 `concat` 的特殊操作进行对集合中的元素进行组合时，这个集合有下面三个特性：
   1. 对集合中的两个值进行结合后的第三个值，也必是集合中的元素。即 `a`、`b` 是集合中的元素，那么 `concat(a, b)` 也是集合中的元素，在范畴论中，叫做：`magma`。
   2. `concat` 操作必须符合结合律：当 `x`、`y`、`z` 都是集合中的元素时，`concat(x, concat(y, z)) 和 concat(concat(x, y), z)` 必须相同。
   3. 集合中必须有中立元素。如果中立元素和其他值进行组合，这个值不应该发生变化。假设 `neutral` 为中立元素，那么 `concat(element, neutral) == concat(neutral, element) == element`。

2. 根据上面的定义，之前许多常见的操作都是 `monoid`，举例如下：

3. 数字相加
   1. 当我们进行数字相加操作时，我们操作的是 JavaScript 中 `Number` 的实例。相加的结果还是数字，因此这个操作是 `magma`
   2. 数字加法符合结合律
   3. 0 是中立元素，因为 0 加任何数字都等于这个数字

4. 字符串连接
   1. `concat` 方法是字符串的实例方法，我们需要将其转换成纯函数：`const concat = (a, b) => a.concat(b)`。
    使用新的 `concat() `方法组合字符串 a 和 b，组合后的结果还是字符串，因此这个操作是 `magma`。
   2. 字符串连接符合结合律：
      - `concat("hello", concat(" ", "world")); // "hello world"`
      - `concat(concat("hello", " "), "world"); // "hello world"`
   3. `''` 空字符串是中立元素，空字符与任何字符串连接，都是原来的这个字符串。

### 1. 函数组合 - compose

1. 函数组合也是 `monoid`。

2. 首先定义 `compose` 操作，这个操作也是 `magma`。
   ```js
      /**
       接收两个函数作为参数，并将这两个函数组合成新的函数，并返回这个新函数
       执行顺序是从右向左，即先执行 fn2，再执行 fn2
       @param fn1
       @param fn2
       @returns {function(*=): *}
       */
      const compose = (fn1, fn2) => arg => fn1(fn2(arg));
   ```
3. 函数组合符合结合律。
   ```js
      const add5 = a => a + 5;
      const double = a => a * 2;
      const resultIs = a => `result: ${a}`;

      const doubleThenAdd5 = compose(add5, double);
      // 11
      console.log(doubleThenAdd5(3));

      // 被组合的函数的顺序一旦确定，怎么组合对结果没有影响
      const res1 = compose(compose(resultIs, add5), double)(3);
      const res2 = compose(resultIs, compose(add5, double))(3);

      // result: 11
      console.log(res1);
      // result: 11
      console.log(res2);
   ```
4. 中立元素（函数）也存在，也被称为 `identity function`。
   ```js
      const neutral = v => v;

      // 10
      compose(add5, neutral)(5);
      // 10
      compose(neutral, add5)(5);
   ```

### 2. monoid 的用途和优势

1. 在函数式编程中，当我们操作函数的时候，需要将接收两个参数的函数转换成接收任意数量参数的函数。比如说，上面的 `compose()` 函数接收两个参数，但是我们需要 `compose` 接收不定数量的参数

2. `Monoid` 是这个问题的解决方案，我们使用数组的 `reduce` 方法实现函数接收任意数量的参数。

#### 1. 数字相加

1. `add()` 函数只能接收两个参数，那么如果想对任意数量的数字实现相加，该怎么做呢？

2. 答案是使用 `reduce` 方法。
   ```js
      const add = (x, y) => x + y;
      const addArray = arr => arr.reduce(add, 0);

      const nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
      // 55
      console.log(addArray(nums));
      
   ```
   上面的 `addArray` 还是只能接收数组，不能接收任意数量的参数，下面进行改进：
   ```js
      const newAdd = (...args) => args.reduce(add, 0);
      // 55
      console.log(newAdd(1, 2, 3, 4, 5, 6, 7, 8, 9, 10));
   ```
3. 使用剩余参数语法，将传入的参数组合成一个数组，后面调用数组的 `reduce` 方法，就能实现对任意数量参数的相加。

#### 2. 字符串连接

1. 字符串拼接也可以使用 `Monoid` 实现对任意数量的字符串的拼接。
   ```js
      const concat = (a, b) => a.concat(b);
      const concatArray = arr => arr.reduce(concat, '');
      const strArr = ['hello', ' ', 'world'];
      // hello world
      console.log(concatArray(strArr));
   ```
2. 上面的例子还是只能接收数组，下面我们进行改进：
   ```js
      const newConcat = (...args) => args.reduce(concat, 0);
      // hello world
      console.log(newConcat('hello', ' ', 'world'));
   ```
3. 使用剩余参数语法，将传入的字符串参数组合成一个数组，后面调用数组的 `reduce` 方法，就能实现对任意数量的字符串进行拼接。

#### 3. 组合函数

1. 函数组合也可使用 `reduce` 方法进行扩展。
   ```js
       const composeArray = arr => arr.reduce     (compose, v => v);

       const functionArr = [resultIs, add5, double];
       // result: 11
       console.log(composeArray(functionArr)(3));
   ```
2. 我们也可以使用剩余参数语法对组合函数进行改进：
   ```js
      const newCompose = (...args) => {
             return args.reduce(compose, v => v);
          
      } 
        // result: 9
     console.log(newCompose(resultIs, add5, double)(2));
   ```
3. 注意，`compose` 函数的执行顺序是从右向左。

4. 另外一种形式的 `compose` 函数：
   ```js
      function compose() {
          let args = Array.prototype.slice(arguments);
   
          return function (value) {
              args.reverse();
              args.reduce(function(acc, func) {
                  return func(acc);
              }, value);
          }
      }
   ```
5. 更一般的形式：当我们有一个 `monoid` 的时候，将一个接收两个参数的函数：concat，经过 `reduce` 的转换，可以接收一个数组数量的参数：`[value1, value2, value3, ...].reduce(concat, neutral);`

6. 因为 `monoid` 的任何操作符合结合律，那么我们可以将一个数组切分成更小的块，对每一个小数组使用 `concat` 操作。最后使用 `concat` 操作将这些结果重新组合起来。这里的 `concat` 指的是某种操作，比如说相加、字符串连接等。
   ```js
      const concat2 = (x, y) => x + y;
      const bigArray = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

      // 55
      console.log(bigArray.reduce(concat2, 0));

      const result1 = bigArray.slice(0, 5).reduce(concat2, 0);
      const result2 = bigArray.slice(5).reduce(concat2, 0);

      // 55
      console.log(concat2(result1, result2));
   ```
7. 因此我们可以将大的操作拆分成小的操作，最后进行组合。

#### 4. 异步函数组合

1. 我们能创建一个函数来组合任意数量的异步函数（或者返回 promise 的函数），这些只需要生成一个 `monoid` 就可以。
   ```js
      const fetch = require('node-fetch');

      const fetchJoker = async number => fetch(`http://api.icndb.com/jokes/${number}`);
      const toJson = async response => response.json();
      const parseJoker = json => json.value.joke;

      const getJoker = async (number) => {
          return parseJoker(await toJson(await fetchJoker(number)));
      }
   ```
   `node-fetch` 是一个在 node 端可以使用 fetch API 的包，需要使用 npm 安装：`npm install node-fectch@2.0`。**注意**： v3.0 以上的版本的 `node-fetch`，只能使用 ES Module 的方式引入。如果想使用 CommonJS 的方式引入，只能安装 v2.0 的版本。
    ```js
       // Time waits for no man. Unless that man is Chuck Norris.
       getJoker(23).then(console.log);
    ```
2. 组合异步函数的函数也是一个 `monoid`，具有下面三个特性：
   1. 组合后的函数仍然是异步函数
   2. 符合结合律
   3. 存在中立元素

3. 因为组合异步函数的函数也是一个 `monoid`，所以我们可以使用 `reduce` 函数，生成一个接收任意数量参数的函数。
   ```js
      /**
       * 组合异步函数的函数
       * @param fn1 异步函数
       * @param fn2 异步函数，返回值是 Promise
       * @returns {function(*=): *}
       */
       const asyncCompose = (fn1, fn2) => async x => fn1(await fn2(x));
   
       // asyncCompose() 函数符合结合律
       // Time waits for no man. Unless that man is Chuck Norris.
       asyncCompose(parseJoker, asyncCompose(toJson, fetchJoker))(23).then(console.log);
       // // Time waits for no man. Unless that man is Chuck Norris.
       asyncCompose(asyncCompose(parseJoker, toJson), fetchJoker)(23).then(console.log);
   
       // 接收任意数量的函数
       const asyncComposeArray = functionArr => functionArr.reduce(asyncCompose, v => v);
       const asyncComposeArgs = (...args) => args.reduce(asyncCompose, v => v);

       // 函数的执行顺序是从右向左
       const getJoker2 = asyncComposeArgs(parseJoker, toJson, fetchJoker); 
   ```

#### 5. 异步控制流

1. 异步 `compose` 函数存在的一个问题是数据流向是从右向左的，不便于我们阅读。

2. 也就是我们将函数组合成数组，还是作为参数传入异步 `compose` 函数，函数的执行顺序都是从右向左的。

3. 因此，我们创建一个新的函数：`flow()`，功能与 `asyncCompose` 函数相同，但是数据的流向是从左向右
也就是 `pipe` 函数。
   ```js
      const flow = (fn1, fn2) => async x => fn2(await fn1(x));

       const flowArr = functionArr => functionArr.reduce(flow, v => v);
       const flowArgs = (...args) => args.reduce(flow, v => v);
       
       const getJoker3 = flowArgs(fetchJoker, toJson, parseJoker);
       // Time waits for no man. Unless that man is Chuck Norris.
       getJoker3(23).then(console.log);
   ```
4. 函数的执行顺序是从左向右，这样就更具备可读性，在可测试性、封装性和延迟执行方面具有许多优势。

#### 6. monoid 总结

1. `monoid` 相当普通，使用 `reduce` 将他们连接起来使得 `monoid` 更有用。

2. 当我们想组合任意数量的元素的时候，比如展开一个多个层次的对象，或者实现一个异步操作的工作流，我们可以使用 `monoid` 实现。

## 3. functor

1. 函子的定义：_A morphism from a source category to a target category which maps objects to objects and arrows to arrows, in such a way as to preserve domains and codomains (of the arrows) as well as composition and identities._

2. 上面的定义太学术、太官方了。下面我们将通过实例，一步一步的说明什么函子。

3. 我想实现一个加 1 的函数，如下所示：
   ```js
      const increment = v => v + 1;
      const a = 5;
      // 6
      console.log(increment(a));
   ```
4. 如果我们传入 increment 的不是数字，比如说：字符串，对象，那么得到的结果就不对了：
   ```js
      // 51
      console.log(increment('5'));
     // // [object Object]1
      console.log(increment({v: 5}));
   ```
5. 因此我们必须在函数中处理参数不是数字的情况
   ```js
      const increment2 = (v) => {
          if (typeof v !== 'number') {
               return NaN;
          }

          return v + 1;
      }
   
      // 6
      console.log(increment2(5));
      // // NaN
      console.log(increment2('5'));
      // // NaN
      console.log(increment2({v: 5}));
   ```
6. 如果此时我需要将一个数字加倍，我们也需要做同样的事：检查输入参数的类型。
   ```js
      const double = (x) => {
          if (typeof x !== 'number') {
              return NaN;
          }

          return x * 2;
      }
   ```
7. 总结一下，就是每次对一个值进行操作，都有下面几个步骤：
   1. 参数类型对不对
   2. 操作方式对不对
   3. 会不会有异常

8. 在一些情况下，值甚至是不能直接访问，需要回调函数。

9. 所以，我们需要转换一下思路，之前是将值传入函数，如果我们将函数传入一个“值”中呢？

10. 对于一个值而言，我们已经知道调用函数时，会对值做一些确定（处理）：
    1. 值为 null，那么不需要执行函数。
    2. 值的类型不对，跳过传入的函数。
    3. 值是一个 error，不应该执行这个函数，但是执行另外一个处理 error 的函数。
    4. 值是通过异步方式获取的，那么应该是异步结果返回时再调用函数。

11. “值”应该知道传入一个函数时他应该做什么。

12. 上面的就是函子模式的基本思想：一个值的占位符，因此是将一个函数传入一个“值”，而不是值传入函数中。

13. 这就是函数式编程的思想：操作函数，而不是操作值。

14. 我们定义一个数值的占位符：NumberBox
    ```js
       const NumberBox = (number) => {
           return {
               applyFunction: fn => NumberBox(fn(number)),
               value: number
           }
       }

       const res = NumberBox(5);
       const value = res.applyFunction(v => v * 2).applyFunction(v => v+ 1).value;
       // 11
       console.log(value);
    ```
15. 上面的 NumberBox 还是没有对非数值的情况进行处理，所以我们需要对扩展 NumberBox 的功能，保证应用到函数的一定是数值。
    ```js
       const NumberBox = (number) => {
           return {
               applyFunction: fn => {
                   if (typeof number !== 'number') {
                        return NumberBox(NaN);
                   }
                    return NumberBox(fn(number));
               },
               value: number
          }
       }
       const res = NumberBox(5);
       const value = res.applyFunction(v => v * 2).applyFunction(v => v+ 1).value;
       // 11
       console.log(value);
       const res2 = NumberBox({ v: 5 });
       const value2 = res2.applyFunction(v => v * 2).applyFunction(v => v+ 1).value;

       // NaN
       console.log(value2);
    ```
16. 仔细观察，NumberBox 中的 applyFunction 实际很像数组的 map 方法。二者都是将值传递到传入其中的函数中，并将函数的返回值作为取代原来的值，即实现了映射 —— `morphism`。 因此，我们将 applyFunction 重命名为 map，作用与数组的 map 方法类似。

17. 下面的 NumberBox 就是一个函子，是一个值的占位符，对外提供一个 map 方法，接收一个函数，用来实现对值的映射
    ```js
       const NumberBox = (number) => {
           return {
               map: fn => {
                   if (typeof number !== 'number') {
                        return NumberBox(NaN);
                   }
                    return NumberBox(fn(number));
               },
               value: number
           }
       }
    ```

18. 总结一下，函子就是一个包裹值的容器，这个容器有一个 map 方法，因此可以将函数用到这个容器上，我们可以在这个函数中写自己的逻辑，在这种模式下，我们操作的是函数，而不是值。

19. 创建一个类型检查的盒子 TypeBox，来处理类型检查。
    ```js
       /**
        * 用于类型检查的盒子，只有通过检查，才能返回带有传入的值的新的盒子，否则就是带有默认值的盒子
        * @param predicate 是一个类型检查的函数，返回值是布尔值
        * @param defaultValue 类型检查不通过时，应用的值
        * @returns {any}
        * @constructor
         */
         const TypeBox = (predicate, defaultValue) => {
              const TypePredicate = (value) => {
                  return {
                      map: fn => predicate(value) ? TypePredicate(fn(value)) : TypePredicate(defaultValue),
                      value
                  }

              };

             return TypePredicate;


          }

       const NumberBox = TypeBox(v => typeof v === 'number', NaN);

       const res = NumberBox(5);

       const value = res.map(v => v * 2).map(v => v + 1).value;
       // 11
       console.log(value);
       const res2 = NumberBox({v: 5});
       const value2 = res2.map(v => v * 2).map(v => v + 1).value;
       // NaN
       console.log(value2);
    ```
20. 创建一个 StringBox。
    ```js
       const StringBox = TypeBox(v => typeof v === 'string', null);

       const res = StringBox('world');

       const value = res.map(v => 'hello ' + v).map(v => '** ' + v + ' ***').value;

       // ** hello world ***
       console.log(value);
       const res2 = StringBox({v: 5});
       const value2 = res2.map(v => 'hello ' + v).map(v => '** ' + v + ' ***').value;
       // null
       console.log(value2);
    ```
21. map 函数具有组合函数的功能，map 函数没有副作用，只能改变函子中的值。
    ```js
       const v1 = NumberBox(5).map(double).map(increment).value;
       const v2 = NumberBox(5).map(v => increment(double(v))).value;
       // 11
       console.log(v1);
       // 11 
       console.log(v2);
    ```
### 1. Maybe 函子

1. 我们在获取对象中的属性的时候，为了防止对象是 `undefined`，我们需要检查一下对象是否存在，然后再获取属性值：`const street = user && user.address && user.address.street;`

2. 为了避免这种情况，我们引入一个新的函子： Maybe。这个函子的作用是可以处理值不存在（null/undefined）的情况。Maybe 的意思就是也许，可能，就是有可能为空，也有可能非空。
   ```js
      const isNothing = value => value === null || typeof value === 'undefined';

       const Maybe = value => {
           return {
              map: fn => {
                return isNothing(value) ? Maybe(null) : Maybe(fn(value));
              },
              value
           }
      }
   ```
3. 使用 Maybe 函子，就不需要担心对象为空了。
   ```js
      const user = {
         name: "Holmes",
         address: { street: "Baker Street", number: "221B" },
         };


      const v1 = Maybe(user).map(u => u.address).map(a => a.street).value; // 'Baker Street'
      const homelessUser = { name: "The Tramp" };
      const v2 = Maybe(homelessUser).map(u => u.address).map(a => a.street).value; // null

      // Baker Street
      console.log(v1);
      // null
      console.log(v2);
   ```
4. 在末尾调用 value 属性不符合函数式编程的风格，因此我们给这个 Maybe 函子添加一个 getter 函数，用来获取值。如果值是 null/undefined，那么就返回 getter 函数接收的默认值。
   ```js
      const Maybe = value => {
          return {
               map: fn => {
                    return isNothing(value) ? Maybe(null) : Maybe(fn(value));
               },
               value,
               getOrElse: defaultValue => isNothing(value) ? defaultValue : value
          }

      }

       /**
       * 拿到属性值
       * @param key
       * @returns {function(*): *}
       */
       const get = key => value => value[key];

        const getStreet = user => {
            return Maybe(user).map(get('address')).map(get('street')).getOrElse('unkonw address');
        }
        const v1 = getStreet({
            name: "Holmes",
            firstname: "Sherlock",
            address: {
                street: "Baker Street",
                number: "221B",
            },
        });

         const v2 = getStreet({
             name: "Moriarty",
         });

         const v3 = getStreet();


         // Baker Street
         console.log(v1);
         // unkonw address
         console.log(v2);
         // unkonw address
         console.log(v3);
   ```

### 2. Either 函子

1. Either 函子，用于处理异常。函数式编程中，不希望在函数执行过程中抛出异常，因为这个会中断函数的执行过程。我们希望函数不抛出异常，而执行完成以后，返回一个值或者是一个错误。

2. 基于 try/catch，我们希望返回不同的函子：
   1. 没有异常：能继续调用的 map 的函子
   2. 出现异常：值不再发生变化的函子

3. 其中处理没有异常的函子叫做 Right，处理有异常的函子叫做：Left。
   ```js
      /**
       * 不对 value 进行任何处理
       * @param value
       * @returns {any}
       * @constructor
       */
       const Left = value => {
           return {
               map : fn => Left(value),
               value
           }
       }

       // 6
       console.log(Left(6).map(v => v + 2).value);

      const Right = value => {
          return {
              map: fn => Right(fn(value)),
              value
          }
      }
   ```
4. 应用 Either 函子：
   ```js
      const validateEmail = (email) => {
          if (!email.match(/\w+@[a-zA-Z0-9]+\.\w+/)) {
          // 验证不通过，将异常信息传入 Left 函子中
          return Left(new Error("The given email is invalid"));
          }

              // 验证通过，将值传入 Right 函子中
              return Right(email);
          }
          // Email: foo@example.com
          const v1 = validateEmail('foo@example.com').map(v => "Email: " + v).value;
          // Error: The given email is invalid
          const v2 = validateEmail('foo@example').map(v => "Email: " + v).value;

          console.log(v1);
          console.log(v2);
   ``` 
5. validateEmail 或者返回错误或者返回正常的值，但是并没有让我们对异常做一些处理。举个例子，我们可以捕获这个异常并打印错误日志，因此，在 Left 和 Right 函子中加上 catch() 方法。
   ```js
      /**
       * Right 函子中，catch 方法什么也不做
       * @param value
       * @returns {any}
       * @constructor
         */
         const Right = value => {
             return {
                 map: fn => Right(fn(value)),
                 catch: fn => Right(value),
                 value

             }
         }

        /**
          * Left 函子中，catch 方法要执行传入的 fn 函数，并返回一个 Right 函子，方便后面的调用
        * @param value
        * @returns {any}
        * @constructor
          */
          const Left = value => {
          return {
              map: fn => Left(value),
              catch: fn => Right(fn(value)),
              value

              }
          }
        //
        // 5
        console.log(Right(5).catch(err => err.message).value);
        // 出错了
        console.log(Left(new Error('出错了')).catch(err => err.message).value);
    ```
6. 为了更方便的使用，我们将 Left 和 Right 函子组合到一起
   ```js
      const tryCatch = (fn) => value => {
         try {
             return Right(fn(value));
         } catch (err) {
             return Left(err);
         }
      }

      const validateEmail = tryCatch(value => {
          if (!value.match(/\S+@\S+\.\S+/)) {
             throw new Error("The given email is invalid");
          }
           return value;
            });

      const v1 = validateEmail('foo@example.com').map(v => 'Email: ' + v).catch(get('message')).value;
      const v2 = validateEmail('foo@example').map(v => 'Email: ' + v).catch(get('message')).value;

      // Email: foo@example.com
      console.log(v1);
      // The given email is invalid
      console.log(v2);
   ```


### 3. 组合函子

1. 我们现在不验证 Email 字符串了，我们验证对象中的 Email 字符串，如果对象中没有 Email 属性，我们就不验证了。所以需要将 Maybe 函子与 Either 函子结合起来。
   ```js

      const validateUser = user => {
          return Maybe(user).map(get('email')).map(v => validateEmail(v).catch(get('message')));
          }

      Maybe(Right('foo@example.com'))
      const v1 = validateUser({
          firstName: "John",
           email: "foo@example.com",
      });


      // Maybe(Left('The given email is invalid'))
      const v2 = validateUser({
          firstName: "John",
          email: "foo@example",
      });
      // Maybe(null)
      const v3 = validateUser({
          firstName: "John",
      });

      console.log(v1);
      console.log(v2);

      console.log(v3);
   ```
2. 从上面的结果中我们可以看出，Maybe 函子接收的要么是 Left 函子要么是 Right 函子甚至有可能是空值，因此，为了拿到值，我们不得不对 Maybe 函子的值进行测试，如下所示：
   ```js
      const validateUserValue = user => {
          const result = validateUser(user).value;
          if (result === undefined || result === null) {
              return null;
          }

              return result.value;
      }
   ```
3. validateUserValue 中，validateUser(user) 得到的是 Maybe 函子中接收一个 Left 或者 Right 函子。所以其 value 属性也是 Left 或者 Right 函子。

4. 判断是不是空值，如果是空值，就返回 null，不是空值，就返回这个 value 属性

5. validateUserValue 返回的就是一个单一的函子。但是 validateUserValue 的缺点也很明显，首先是判断 value 是否为空。这部分是我们抛弃的内容。其次是返回 value 属性，如果 我们组合多个函子，显然就是：value.value.value ...。这个是不可接受的。

### 4. 引入 chain

1. 我们需要一个方法，能够从一个函子中提取出值，即使这个值被其他函子包裹。
   ```js
      const Maybe = value => {
          return {
              map: fn => {
                  return isNothing(value) ? Maybe(null) : Maybe(fn(value));
             },
              // flatten 方法总是会返回一个 Maybe 函子
             flatten: () => isNothing(value) ? Maybe(null) : Maybe(value.value),
             getOrElse: defaultValue => isNothing(value) ? defaultValue : value
          }

       }

       const validateUser = user => {
           return Maybe(user).map(get('email')).map(v => validateEmail(v).catch(get('message'))).flatten().getOrElse('the user has no email');

       }

      const v1 = validateUser({
          firstName: 'John',
          email: 'foo@example.com',
      });

      const v2 = validateUser({
          firstName: 'John',
          email: 'foo@example',
      });

      const v3 = validateUser({
          firstName: 'John',
      });



      // foo@example.com
      console.log(v1);
      // The given email is invalid
      console.log(v2);
      // the user has no email
      console.log(v3);
   ```
2. 使用 flatten 方法，将一个函子展平了，因此我们能直接获得 Maybe 函子中的值。

3. 但是这种方式还不是很理想，在函数式编程中，我们经常需要 map 和 flatten，或者使用 flattenMap。但是 flattenMap 描述了程序做了什么，但是并没有描述目的是什么。因为这个操作是用来串联函子的，因此我们叫这个操作为 chain
   ```js
      const Maybe = value => {
          return {
              map: fn => {
                   return isNothing(value) ? Maybe(null) : Maybe(fn(value));
              },
              // flatten 方法总是会返回一个 Maybe 函子
              flatten: () => isNothing(value) ? Maybe(null) : Maybe(value.value),

             // chain 方法用来串联函子
             // 在 chain 方法中，调用 map，展开一个嵌套的函子
             chain: function (fn) {
                 return this.map(fn).flatten()
             } ,

              getOrElse: defaultValue => isNothing(value) ? defaultValue : value
          }

      }

        const validateUser = user => {
        return Maybe(user)
                  .map(get('email'))
                  .chain(v => validateEmail(v).catch(get('message')))
                  .getOrElse('the user has no email');

        }

      // foo@example.com
      const v1 = validateUser({
          firstName: 'John',
          email: 'foo@example.com',
      });

      // The given email is invalid
      const v2 = validateUser({
          firstName: 'John',
          email: 'foo@example',
      });
      // the user has no email
      const v3 = validateUser({
          firstName: 'John',
      });

      console.log(v1);
      console.log(v2);
      console.log(v3);
   ```

### 5. 总结

1. 函子这个概念来自于函数式编程，通过函子，我们能将函数应用到一个值上，因此我们不需要检查值得类型、是否会抛出异常等。

2. 通过函子构造函数得到一个函子：函子构造函数接收一个值，然后返回一个对象，这个对象包裹了这个值，并且拥有多个函数，这个对象就被称为函子。

3. 有很多类型的函子，每个函子有不一样的功能：
   1. TypeBox：检查值的类型
   2. Maybe：检测值是否为空
   3. Either：代码分支和异常处理

## 4. monad
1. Monad，中文译名为单子，定义是：_A monad is just a monoid in the category of endofunctors._

2. 通过前面对函子的介绍，现在我们可以引入单子：单子就是使用 chain 方法展开自己的函子。因此，Maybe 函子就是一个单子

3. chain 方法必须遵守下面几个规则：
   1. Left identity
      - `Monad(x).chain(f) === f(x); // f being a function returning a monad`
      - f 是一个返回 Monad 的函数
      - 向 chain 方法中传入 f，与直接向 f 中传入 x 的结果是一样的，返回的是同一个函子。
   2. Right identity
      - `Monad(x).chain(Monad) === Monad(x);`
      - 向 chain 方法中传入 Monad，应该返回同样的 Monad，因为 chain 是没有副作用的。
   3. Associativity
      - `monad.chain(f).chain(g) == monad.chain(x => f(x).chain(g)); // f and g being functions returning a monad`
      - f 和 g 是返回 monad 的函数
      - monad 符合结合律，这样保证，我们不仅能链式调用chain，也能嵌套调用 chain。

4. 假设一种情况，就是如果我们在 map 函数中，应用一个返回函子的函数，那么最终得到一个值是函子的函子，这种情况下，我们不能直接访问函子中的值了，必须将函子展开才能访问。因此，我们通过展开函子的方式来移除额外的函子，这个过程被称为 chain。

5. **拥有 chain 方法的函子被称为 Monad**。

6. 完整的 Maybe 函子：
   ```js
      const Maybe = value => {
          return {
               isNothing: function() {
                   return value === null || value === undefined;
               },
               map(fn) {
                   return this.isNothing() ? Maybe.Nothing() : Maybe(fn(value));
               },

               flatten() {
                   return this.isNothing() ? Maybe.Nothing() : Maybe(value.value);
               },
               chain(fn) {
                   return this.isNothing() ? Maybe.Nothing() : this.map(fn).flatten();
               },
               getOrElse(defaultValue) {
                   return this.isNothing() ? defaultValue : value;
               },

               value,
           }
       }

       /**
       * Nothing Monad
       * 避免每次检测空值
       * @returns {{isNoting: (function(): boolean)}}
       * @constructor
         */
         Maybe.Nothing = () => {
         return {
         isNoting: () => true,
         map: () => Maybe.Nothing(),
         flatten: () => Maybe.Nothing(),
         chain: (fn) => Maybe.Nothing(),
         getOrElse: defaultValue => defaultValue,
         value: null
         }
         }

   ```
7. 完整的 Either 函子如下：
   ```js
      const Right = value => {
          return {
              map: fn => Right(fn(value)),
              flatten: () => Right(value.value),
              chain(fn) {
              return Right(this.map(fn).flatten());
              },
              catch: fn => Right(value),
              value
              }
          }

         /**
         * Left 函子不会对值进行任何处理
         * @param value
         * @returns {any}
         * @constructor
           */
           const Left = value => {
               return {
                  // 返回 Left 函子本身
                  map: () => Left(value),
                  // 返回 Left 函子本身
                  flatten: () => Left(value),
                  // 返回 Left 函子本身
                  chain(fn) {
                      return Left(value);
                  },
                  catch: fn => Right(fn(value)),
                  value
              }
           }

         const tryCatch = fn => value => {
             try {
                 return Right(fn(value));
             } catch (err) {
                return Left(err);
             }
         }

   ```

8. 应用 Maybe 函子和 Either 函子：
   ```js
      const validateEmail = tryCatch(value => {
          if (!value.match(/\S+@\S+\.\S+/)) {
               throw new Error("The given email is invalid");
           }
            return value;
      });

      const get = key => obj => obj[key];

      const parseMail = obj => {
          // Maybe 函子接收一个对象，这个对象也有可能是空值，如果是空值，那么下面所有的操作都会被忽略，直到调用 getOrElse，最终得到默认值
          return Maybe(obj)
          // 我们要访问 obj 的 mail 属性，如果这个属性存在，那么此时的 Maybe 函子的 value 就变成 mail 值
          // 如果 obj 没有 mail 属性，那么得到的是空值，同样下面所有的操作都会被忽略，直到调用 getOrElse，最终得到默认值
          .map(get('mail'))
          // 接下来我们调用 chain 方法，chain 方法接收的函数中，执行 validateEmail，validateEmail 接收一个字符串，然后会返回一个 Either 函子：
          // - Right(mail) 如果字符串是一个 email
          // - Left(new Error("The given email is invalid"))
          .chain(mail => validateEmail(mail).catch(err => err.message))
           .getOrElse('this user has no email');
       }

        console.log(parseMail({name: 'foo', mail: 'bar@example.com'}));
        console.log(parseMail({name: 'foo', mail: 'bar'}));
        console.log(parseMail({name: 'foo'}));
   ```

9. 调用完 validateEmail 以后，我们将最后的 Left 函子转换成 Right 函子，无论 mail 是否有效，这种转换都没有问题。

10. 我们现在拥有一个值为字符串的 Right 函子。即使我们不调用 catch 方法，一旦 chain 执行完成，Either 函子会被展开
其 value 会传入 Maybe 函子中，所以 chain 方法返回的是一个 Maybe 函子，最终得到结果只有三种：
    1. Nothing()
    2. Maybe(mail)
    3. maybe(new Error("The given email is invalid"))

11. 换句话说，调用 chain 的结果是拿到了 Either 函子的值，而不是 Either 函子，我们无法知道获得是正确的值还是错误信息
当我们在 Maybe 函子中展开一个 Either 函子时，我们丢失了 Either 函子的上下文信息，即 如果是 Left 或者 Right 函子
我们可以知道这是表示什么，但是如果单纯的一个值，我们根本不知道这表示什么。 如下所示：
    ```js
       Maybe(Right("bar@example.com")).flatten();      // Maybe('bar@example.com')
       Maybe(Left(new Error("Invalid mail"))).flatten(); // Maybe(new Error('Invalid mail'))
    ```
12. chain 方法存在的问题就是：给定两个不同类型的 Monad，chain 方法会返回调用 chain 方法的 Monad。chain 实际上是调用了 flatten 方法，而 flatten 方法是将嵌套的 Monad 的值提出来，并将这个值放入当前的 Monad 中。

13. 这种做法使得我们可以链式组合不同的 Monad，但是，这种做法实际上是不对的。我觉得不对的原因是：这样的转换方法将其他类型的函子都隐式的转换成同一个类型的函子，使用起来不够灵活，会丢失值的上下文信息，无法满足我们任意转换的需求。比如说，前面的例子是将 Either 函子统一转换成 Maybe 函子，如果我们需要将 Either 函子转换成其他类型的函子，使用 chain 方法就做不到了。因此要改变 chain 方法和 flatten 的实现方式。

14. 下面就是 Right 函子中的 flatten 和 chain 的新的实现方式：
    ```js
       const Right = value => {
            return {
                map: fn => Right(fn(value)),
                flatten: () => value,
                chain(fn) {
                    return this.map(fn).flatten();
                },
                catch: fn => Right(value),
                value
            }
        }
    ```
15. 在上面的 Right 函子中，flatten 仅仅返回值，不进行展开
chain 方法值调用 map 方法，然后调用 flatten 方法。我们不需要知道怎么将嵌套的 Monad 中的值提取出来，我们仅仅返回它即可

16. 前面使用 Maybe 函子和 Either 函子的方法都失效了，因为 Monad 的类型中途发生了变化，这样我们就不需要展开两个不同类型的函子了。

17. 虽然不应该展开两个不同类型的函子，像是 Maybe 函子和 Either 函子，但是我们可以展开统一类型的函子。举个例子，如果我们有这样的结构：`Maybe(Either(value))`，如果我们能将其转换成 `Maybe(Maybe(value))`。 那么我们就能展开两个 Maybe 函子，因此我们需要一个函数：eitherToMaybe，进行函子之间的转换，将 Either 函子转换成 Maybe。然后调用方式就变成下面这样：
    ```js
        Maybe({
           name: "foo",
           mail: "bar@example.com",
        })
       .map(v => v.mail)
       .map(validateMail)
       .map(eitherToMaybe)
       .flatten();
    ```
    或者更短：
    ```js
       Maybe({
          name: "foo",
          mail: "bar@example.com",
       })
        .map(v => v.mail)
        .map(validateMail)
        .chain(eitherToMaybe);
    ```
    调用 chain 等于 `map + flatten`。

18. 如何实现 eitherToMaybe 呢？如果是 Right 函子，那么就直接返回 Maybe 函子，即 `Right(value) -> Maybe(value)`。如果是 Left 函子，那么返回的是 Nothing()，即 `Left(error) -> nothing()`。因为 Maybe 函子只告诉我们 value 是否为空，所以我们不接收这个错误
19. 下面就是实现：
    ```js
       const eitherToMaybe = either => Maybe(either.catch(err => null).flatten());
    ```
20. 新的调用方式如下：
    ```js
       Maybe({
          name: "foo",
          mail: "bar@example.com",
       })
       .map(v => v.mail)
       .map(validateMail)
       .chain(eitherToMaybe);
       // 返回值就是 Maybe 函子
       // Maybe("bar@example.com");
    ```
    返回是Nothing()：
    ```js
       Maybe({
          name: "foo",
          mail: "bar",
       })
       .map(v => v.mail)
       .map(validateMail)
       .chain(eitherToMaybe);

       // 返回值是一个 nothing()
       // Maybe.Nothing();
    ```

21. 现在实现一个 Maybe 函子到 Either 函子的转换。Maybe 函子接收的值非空，则转变成 Right 函子，即 `Maybe(value) -> Right(value)`。 Maybe 函子接收的值是空值，则变成 Left 函子，即 `Maybe.Nothing() -> Left('no value')`。
    ```js
       const maybeToEither = maybe => {
           return maybe.Nothing() ? Left('no value') : Right(maybe.flatten());
       }
    ```

22. 上面的转换过程叫做自然转换，即 `Natural Transformation`。自然转换就是值改变容器，不改变值，比如说对 Monad 类型的转换就是是自然转换。

23. 如果是自然转换，与顺序无关，比如说：
    ```js
       Either(5)
           .chain(eitherToMaybe)
           .map(v => v * 2);

        // 返回 Maybe(10)
       Either(5)
           .map(v => v * 2)
           .chain(eitherToMaybe);
       // 返回 Maybe(10)
    ```
24. 改进过的 Maybe 函子：
    ```js
       const Maybe = value => {
           return {
               isNothing: () => value === null || value === undefined,
               map(fn) {
                   return this.isNothing() ? Maybe.Nothing() : Maybe(fn(value))
               },
               flatten() {
                    return value;
                },
               chain(fn) {
                   return this.map(fn).flatten();
               },
               getOrElse(defaultValue) {
                   return this.isNothing() ? defaultValue : value;
               },
               value

           }
        }

        Maybe.Nothing = () => {
            return {
               isNoting: () => true,
               map: () => Maybe.Nothing(),
               flatten: () => Maybe.Nothing(),
               chain: (fn) => Maybe.Nothing(),
               getOrElse: defaultValue => defaultValue,
               value: null
           }
       }
    ```
25. 改进过的 Either 函子：
    ```js
       /**
         *
         * @param value
         * @returns {any}
         * @constructor
           */
        const Right = value => {
             return {
                 map: (fn) => Right(fn(value)),
                 flatten: () => value,
                 chain(fn) {
                     return this.map(fn).flatten();
                 },
                 catch: () => Right(value),
             }
        }

         /**
          * Left 函子不对值做任何处理
          * @param err
          * @returns {any}
          * @constructor
          */
           const Left = err => {
               return {
                   map: (fn) => Left(err),
                   flatten: () => err,
                   chain(fn) {
                   return err;
                   },
                   catch: (fn) => Right(fn(err)),
               }
           }

       const tryCatch = fn => value => {
           try {
               return Right(fn(value));
           } catch (err) {
               return Left(err);
           }
       }
    ```
26. 应用：
    ```js
       const validateMail = tryCatch(function(value) {
         if (!value.match(/\S+@\S+\.\S+/g)) {
              throw new Error('Invalid Email');
         }

             return value;
         })

        const get = key => obj => obj[key];

        const parseEmail = user => {
            return Maybe(user)
                   .map(get('mail'))
                   .map(mail => validateMail(mail).catch(err => err.message))
                   .chain(eitherToMaybe)
                   .getOrElse('no email');
        }

       // bar@example.com
       console.log(parseEmail({ name: 'foo', mail: 'bar@example.com' }));
       // Invalid Email
       console.log(parseEmail({ name: 'foo', mail: 'bar@example' }));
       // no email
       console.log(parseEmail({ name: 'foo' }));
    ```
27. 每一个 Monad 应该提供一个函数用将当前的 Monad 转换成其他类型的 Monad。

