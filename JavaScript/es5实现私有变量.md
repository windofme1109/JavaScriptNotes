# ES5 实现私有变量

## 1. 参考资料

1. [JavaScript --用ES5实现实现私有变量](https://www.cnblogs.com/zx-fjs/p/13209174.html)

2. [使用ES5实现私有非单例属性](https://blog.csdn.net/lm278858445/article/details/78595335)

3. [es5 温故而知新 创建私有成员、私有变量、特权变量的方法](https://www.cnblogs.com/CyLee/p/9862384.html)

4. [JavaScript --用ES5实现实现私有变量](https://www.cnblogs.com/zx-fjs/p/13209174.html)

## 2. 实现方法

1. 核心是使用闭包，因为闭包可以访问函数内部的变量。

2. 在函数内部定义一个变量，然后使用闭包的方式，访问这个变量。

3. 代码示例：
   ```js
      function Person(name, age) {
           this.name = name;

           // _age 是 Person 内部的变量，外部无法访问
           let _age = age;

           // 使用闭包来模拟实现私有变量
           this.getAge = function () {
               // 定义一个函数，由于作用域的关系，这个函数内部可以访问外层函数的变量，所以 getAge 内部可以访问 _age
               // 通过调用 getAge，可以访问到构造函数内部的 _age
               return _age;
           }

           this.setAge = function (value) {
               _age = value;
           }
       }


      const p1 = new Person('jack', 20);

      20
      console.log(p1.getAge());

      // undefined
      console.log(p1.age);

      console.log(p1.setAge(25));

      // 25
      console.log(p1.getAge());
   ```
4. 说明：
   1. `_age` 是 `Person` 内部的变量，外部无法访问。
   2. 定义一个函数，由于作用域的关系，这个函数内部可以访问外层函数的变量，所以 `getAge` 内部可以访问 `_age`。
   3. 将 `getAge` 函数赋值给实例属性 `getAge`，那么这个实例属性也是一个方法，通过调用这个方法，就能拿到 `Person` 构造函数内部的 `_age`。
   4. 在外部，直接访问 `_age` 是 `undefined`。
   5. setAge 同理。