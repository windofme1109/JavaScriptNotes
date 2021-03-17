<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [toString 与 valueOf 的用法总结](#tostring-%E4%B8%8E-valueof-%E7%9A%84%E7%94%A8%E6%B3%95%E6%80%BB%E7%BB%93)
  - [1. toString](#1-tostring)
    - [1. 基本说明](#1-%E5%9F%BA%E6%9C%AC%E8%AF%B4%E6%98%8E)
    - [2. boolean](#2-boolean)
    - [3. number](#3-number)
    - [4. object](#4-object)
    - [5. function](#5-function)
    - [6. array](#6-array)
    - [7. date](#7-date)
    - [8. RegExp](#8-regexp)
    - [9. Error类型](#9-error%E7%B1%BB%E5%9E%8B)
  - [2. valueOf](#2-valueof)
    - [1. 基本说明](#1-%E5%9F%BA%E6%9C%AC%E8%AF%B4%E6%98%8E-1)
    - [2. 不同类型的对象调用 valueOf()](#2-%E4%B8%8D%E5%90%8C%E7%B1%BB%E5%9E%8B%E7%9A%84%E5%AF%B9%E8%B1%A1%E8%B0%83%E7%94%A8-valueof)
  - [3. toString() 与 valueOf() 的调用时机](#3-tostring-%E4%B8%8E-valueof-%E7%9A%84%E8%B0%83%E7%94%A8%E6%97%B6%E6%9C%BA)
    - [1. 应用场景](#1-%E5%BA%94%E7%94%A8%E5%9C%BA%E6%99%AF)
    - [2. 字符串转换](#2-%E5%AD%97%E7%AC%A6%E4%B8%B2%E8%BD%AC%E6%8D%A2)
    - [3. 数值转换](#3-%E6%95%B0%E5%80%BC%E8%BD%AC%E6%8D%A2)
    - [4. 布尔值转换](#4-%E5%B8%83%E5%B0%94%E5%80%BC%E8%BD%AC%E6%8D%A2)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# toString 与 valueOf 的用法总结

## 1. toString

### 1. 基本说明

1. 除了 null 和 undefined，number、boolean、string 和 object，包括 Date、RegExp、Array 以及 Function，都有 toString() 方法。toString() 可以将其他类型转换为字符串。

### 2. boolean

1. 示例：
   ```javascript
      // 'true'
      true.toString();
      // false
      false.toString();
   ```

### 3. number
   
1. 当一个数字调用 toString() 转换为字符串时，toString() 方法还可以接收一个 radix 参数，表示将数字转换为其他进制形式的数字。默认是 10 进制。

2. 整数直接调用 toString() 形式，会报错，提示无效标记，因为整数后的点会被识别为小数点。
    ```javascript
         // Uncaught SyntaxError: Invalid or unexpected token
         100.toString();
    ```
3. 正确的做法是加上小括号：
    ```javascript
          // '100'
          (100).toString();
    ```
4. NaN 和 infinity 以及正浮点：
    ```javascript
          // '1.78'
          1.78.toString();
          // 'NaN'
          NaN.toString();
          //'Infinity'
          Infinity.toString();
    ```
5. 负浮点数或加 `+` 号的正浮点数直接调用 toString()，相当于先运行 toString() 方法，再添加正负号，转换为数字：
    ```javascript
          // -1.5
          -1.5.toString()
          // 1.8
          +1.8.toString()
          // "number"
          typeof +1.8.toString()
    ```
6. 带符号的浮点数调用 toString() 相当于没有转换。

### 4. object

1. 对象 Object 类型及自定义对象类型加括号返回 [object Object]。
   
2. 对象不能直接调用 toString()，会报错。因此调用时需要加上小括号。

3. 示例：
   ```javascript
      // Uncaught SyntaxError: Unexpected token '.'
      {a: 1}.toString()
      // "[object Object]"
      ({a: 1}).toString()
      // "function Object() { [native code] }"
      Object.toString()
      
      function Person() {
          this.name = 'aaa'
      }

      let p = new Person()
      // "[object Object]"
      p.toString()
      
   ```

4. 对象的 toString() 可以用来进行类型识别，返回代表该对象的 [object 数据类型] 字符串表示。注意：**Object.prototype.toString() 可以识别标准类型及内置对象类型，但不能识别自定义类型**。
   ```javascript
      // [object String]
      console.log(Object.prototype.toString.call('111'));
      // [object Number]
      console.log(Object.prototype.toString.call(111));
      // [object Boolean]
      console.log(Object.prototype.toString.call(true));
      // [object Object]
      console.log(Object.prototype.toString.call({a: 123}));
      // [object Function]
      console.log(Object.prototype.toString.call(function a(){}));
      // [object RegExp]
      console.log(Object.prototype.toString.call(/\d/));
      // [object Array]
      console.log(Object.prototype.toString.call([]));
      // [object Date]
      console.log(Object.prototype.toString.call(new Date));
      
     function Person() {
          this.name = 'aaa'
      }

      let p = new Person()
      // [object Object]
      console.log(Object.prototype.toString.call(p));
   ```

5. 根据 toString() 的特性，我们可以定义一个通用的类型判别函数：
   ```javascript
      function type(obj) {
          let t = Object.prototype.toString.call(obj);
          return t.slice(8, t.length - 1).toLowerCase();
      } 


      // string
      console.log(type('111'));
      // number
      console.log(type(111));
      // boolean
      console.log(type(true));
      // object
      console.log(type({a: 123}));
      // function
      console.log(type(function a() {}));
      // regexp
      console.log(type(/\d/));
      // array
      console.log(type([]));
      // date
      console.log(type(new Date));
   ```

6. 除了类型识别之外，还可以进行其他识别，如识别 arguments 或 DOM 元素：
   ```javascript
      // [object Arguments]
      (function(){
    console.log(Object.prototype.toString.call(arguments));})()
      // [object HTMLDocument]
      console.log(Object.prototype.toString.call(document));
   ```

### 5. function

1. 函数调用 toString()，返回的是函数的源代码。

2. 当我们对一个自定义函数调用 toString() 方法时，可以得到该函数的源代码；如果对内置函数使用 toString() 方法时，会得到一个 `[native code]` 字符串。因此，可以使用 toString() 方法来区分自定义函数和内置函数。
   ```javascript
      function add(a, b) {
          return a + b;
      }
      // function add(a, b) {
      //     return a + b;
      // }
      console.log(add.toString());
      // function max() { [native code] }
      console.log(Math.max.toString());
   ```

### 6. array

1. 数组直接调用 toString()，得到的是数组中每个值的字符串形式拼接而成的一个以逗号分隔的字符串。
   ```javascript
      // 1,2,3
      console.log([1, 2, 3].toString());
      // ''
      console.log([].toString());
   ```

### 7. date

1. 时间日期类型直接调用 toString()，返回表示当前时区的时间的字符串表示：
   ```javascript
      // "Wed Mar 17 2021 11:13:46 GMT+0800 (中国标准时间)"
      (new Date).toString();
   ```

### 8. RegExp 
1. 正则表达式类型直接调用 toString()，返回正则表达式字面量的字符串表示：
   ```javascript
      // "/\d/"
      (/\d/).toString()
   ```

### 9. Error类型

1. 示例：
   ```javascript
      
      Error.toString();//"function Error() { [native code] }"
      
      RangeError.toString();//"function RangeError() { [native code] }"
      
      ReferenceError.toString();//"function ReferenceError() { [native code] }"
      
      SyntaxError.toString();//"function SyntaxError() { [native code] }"
      
      TypeError.toString();//"function TypeError() { [native code] }"
      
      URIError.toString();//"function URIError() { [native code] }"      
   ```


## 2. valueOf

### 1. 基本说明

1. valueOf() 是 Object 原型上的方法，因此会被 Object 后面的每个对象继承。

2. 每个内置的核心对象都会覆盖此方法以返回适当的值。如果对象没有原始值，则 valueOf() 将返回对象本身。

### 2. 不同类型的对象调用 valueOf() 

1. Boolean —— 返回布尔值
   ```javascript
      // true
      true.valueOf();
   ```
2. Array —— 返回数组本身
   ```javascript
      // [1, 2, 3]
      [1, 2, 3].valueOf()
   ```
3. Date —— 存储的时间是从 1970 年 1 月 1 日午夜开始计的毫秒数 UTC
   ```javascript
      // 1615960542098
      (new Date).valueOf()

   ```
4. Function —— 函数的源代码
   ```javascript
      function s() {
          return 0;
      }
      // ƒ s() {return 0;}
      s.valueOf();
    ```
5. Number —— 数字本身
   ```javascript
      // 45
      (45).valueOf()
      // 4.5
      4.5.valueOf()
    ```
6. Object —— 对象本身
   ```javascript
      // {a: 1}
      ({a: 1}).valueOf()
      function Person() {
          this.name = 'aaa'
      }
      let p = new Person()
      // Person {name: "aaa"}
      p.valueOf()
          
    ```
7. String —— 字符串本身
   ```javascript
      // 'asbs'
      'asbs'.valueOf();
   ```
8. Math 和 Error 对象每 valueOf() 方法。

## 3. toString() 与 valueOf() 的调用时机

### 1. 应用场景

1. javaScript 对象在以下三种场景中会转换为原始数据类型（primitives）：
   - Numeric
   - String
   - Boolean

### 2. 字符串转换

1. 当需要一个对象的字符串形式表示时，就会发生字符串转换。如 `alert(obj)`、`console.log(obj)` 等。`String(obj)` 也会做显示转换。 

2. 转换规则：
   1. 存在 toString() 方法，并且返回基本数据，则使用toString()的返回值（所有的对象都有toString() 方法，也就是说正常情况转换执行到这里就结束了）。
   2. 如果 valueOf() 存在并且返回基本数据，则使用 valueOf() 返回的值
   3. 否则抛出异常。

### 3. 数值转换

1. 数值转换在以下两种情况发生：
   1. 需要一个数字的功能：比如 `Math.sin(obj)`，·,包括一元算数运算符：`+object` (`+` 会强制转换 `object` 为 `Number` 类型)。
   2. 在比较中，如 `obj == 'join'`，包括 `>` 或者 `<` 。
`===` 是例外，因为它不允许做任何类型转换（and also non-strict equality when both arguments are objects, not primitives: obj1 == obj2）。`===` 首先比较类型，在比较值的大小。
   3. 使用 `Number(obj)` 时也会有明确的数值转换。

2. 转换规则：
   1. 存在 valueOf() 方法并且返回基本数据类型，则使用 valueOf() 的值。
   2. 不存在 valueOf() 方法，存在 toString() 方法并且返回基本数据类型，则使用 toString() 的值
   3. 二者都不存在，则抛出异常。

3. 内置对象中，Date 同时支持数值转换和字符串转换：
   ```javascript
      alert( new Date() ) // The date in human-readable form
      alert( +new Date() ) // Microseconds till 1 Jan 1970
   ```
4. 但是大部分对象没有 valueOf() 方法，这意味着大部分数值转换由 toString() 实现，更意味着数值转换并不一定得到 Number 类型的数据。

### 4. 布尔值转换

1. 布尔值转换发生在一个布尔值上下文中，如：`if(obj)`，`while(obj)` 等。

2. `Boolean(obj)` 同样会有显式的布尔值转换。

3. 转换规则：

   原始值|转换为true的值|转换为false的值
   |:---:|:---:|:---:|
   Boolean|true|false
   String|任何非空字符串| ''(空字符串)
   Number|任何非 0 数值| 0
   Object|任何对象|null
   undefined|N/A|undefined
   null|N/A|null



