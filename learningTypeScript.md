<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [学习TypeScript](#%E5%AD%A6%E4%B9%A0typescript)
  - [一、基础](#%E4%B8%80%E5%9F%BA%E7%A1%80)
    - [1. TypeScript是什么](#1-typescript%E6%98%AF%E4%BB%80%E4%B9%88)
    - [2. 第一个ts程序](#2-%E7%AC%AC%E4%B8%80%E4%B8%AAts%E7%A8%8B%E5%BA%8F)
    - [3. tsconfig.json配置详解](#3-tsconfigjson%E9%85%8D%E7%BD%AE%E8%AF%A6%E8%A7%A3)
    - [4. TypeScript中的数据类型](#4-typescript%E4%B8%AD%E7%9A%84%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B)
    - [5. 联合类型](#5-%E8%81%94%E5%90%88%E7%B1%BB%E5%9E%8B)
    - [6. 接口](#6-%E6%8E%A5%E5%8F%A3)
    - [7. 数组类型](#7-%E6%95%B0%E7%BB%84%E7%B1%BB%E5%9E%8B)
    - [8. 内置对象](#8-%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1)
    - [9. 函数类型](#9-%E5%87%BD%E6%95%B0%E7%B1%BB%E5%9E%8B)
    - [10. 类型断言](#10-%E7%B1%BB%E5%9E%8B%E6%96%AD%E8%A8%80)
    - [11. 声明文件](#11-%E5%A3%B0%E6%98%8E%E6%96%87%E4%BB%B6)
    - [12. 书写声明文件](#12-%E4%B9%A6%E5%86%99%E5%A3%B0%E6%98%8E%E6%96%87%E4%BB%B6)
  - [二、进阶](#%E4%BA%8C%E8%BF%9B%E9%98%B6)
    - [1. 类型别名](#1-%E7%B1%BB%E5%9E%8B%E5%88%AB%E5%90%8D)
    - [2. 字符串字面量类型](#2-%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%AD%97%E9%9D%A2%E9%87%8F%E7%B1%BB%E5%9E%8B)
    - [3. 元组（Tuple）](#3-%E5%85%83%E7%BB%84tuple)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

------

# 学习TypeScript
## 一、基础
### 1. TypeScript是什么
1. TypeScript官网：https://www.typescriptlang.org/

2. TypeScript教程：[TypeScript入门教程](https://ts.xcatliu.com/introduction/hello-typescript.html)
### 2. 第一个ts程序
1. ts文件时以`ts`为后缀的文件。`TypeScript`是`JavaScript`的超集，可以被编译为`js`文件。`TypeScript`添加了数据类型的定义。在变量后使用`:`指定变量的类型，`:`的前后有没有空格都可以。示例代码如下：
   ```typescript
       function sayHello(person: String) {
           return `hello, ${person}` ;
       }
       
       let user = 'Tom' ;
       //user为数组类型，编译时会报错：error TS2345: Argument of type 'number[]' is not assignable to parameter of type 'String'.
       // let user = [1, 2, 3] ;
       console.log(sayHello(user)) ;
   ```
   
2. 将上述文件保存为`01-helloWorld.ts`，然后在当前目录下的终端输入：`tsc 01-helloWorld.ts`，即可将`ts`文件编译为`js`文件。此时，在`01-helloWorld.ts`的同级目录下，就会有一个同名的`js`文件。编译后的`js`文件如下所示：
   ```javascript
      function sayHello(person) {
          return "hello, " + person;
      }
      var user = 'Tom';
      //user为数组类型，编译时会报错：error TS2345: Argument of type 'number[]' is not assignable to parameter of type 'String'.
      // let user = [1, 2, 3] ;
      console.log(sayHello(user));

   ```
   
3. 在编译过程中，会对变量的数据类型进行检查，如果类型不符合，就会编译出错。如我们这样定义：`let user = [1, 2, 3] ;`，则在编译过程中，就会提示：`error TS2345: Argument of type 'number[]' is not assignable to parameter of type 'String'.`，但是仍然会生成`js`文件。

4. 这是因为因为`TypeScript`只会在编译时对类型进行静态检查，如果发现有错误，编译的时候就会报错。而在运行时，与普通的`JavaScript`文件一样，不会对类型进行检查。

5. 如果要在报错的时候终止`js`文件的生成，可以在`tsconfig.json` 中配置`noEmitOnError`即可。
### 3. tsconfig.json配置详解
- 参考文献：[详解TypeScript项目中的tsconfig.json配置](https://www.jianshu.com/p/0383bbd61a6b)
### 4. TypeScript中的数据类型
1. 基本数据类型
   - `string`， 声明一个字符串类型的变量： `let str: string = 'aa' ;`
   - `number`， 声明一个数值类型的变量： `let num: number = 123 ;`
   - `boolean`， 声明一个布尔类型的变量： `let bln: boolean = true ;`
   - `undefined`， 声明一个未定义类型的变量： `let u: undefined = undefined ;`
   - `null`， 声明一个null类型的变量： `let n: null = null ;`
   
2. void
   - 对于一个没有返回值的函数，我们可以声明其返回值类型是void。
   ```typescript
      function printName(name: string): void {
          console.log('name', name) ;
      }
   ```
   
3. 与 void 的区别是，undefined 和 null 是所有类型的子类型。也就是说 undefined 类型的变量，可以赋值给 number 等类型的变量。
   ```typescript
       let num: number = undefined ;
       let str: string = null ;
       console.log('num', num, 'str', str) ;
   ```
   
4. 而将void类型赋值给number等类型的变量，编译过程会报错。
   ```typescript
       // 而 void 类型的变量不能赋值给 number 类型的变量
       // error TS2322: Type 'void' is not assignable to type 'string'.
       let v: void ;
       let s:string = v ;
   ```
5. any

### 5. 联合类型
### 6. 接口
### 7. 数组类型
### 8. 内置对象
1. `JavaScript`中有很多内置对象，它们可以直接在`TypeScript`中当做定义好了的类型。
2. 内置对象是指根据标准在全局作用域（Global）上存在的对象。这里的标准是指 ECMAScript 和其他环境（比如 DOM）的标准。
3. ECMA提供的标准对象包括：Boolean、Number、Date、String等。详细的内容可以查看MDN：[Standard built-in objects](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects)
4. DOM提供的标准对象主要有：Document、HTMLElement、Event、NodeList等。也就是DOM interfaces。详细的内容可以查看MDN：[Document Object Model (DOM)](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)
5. BOM提供的标准对象主要有：Window（最顶层的对象）、location、history等。
6. ECMAScript、DOM、BOM提供的标准对象都在 [TypeScript 核心库的定义文件](https://github.com/Microsoft/TypeScript/tree/master/src/lib) 中。
7. 注意：**Node.js 不是内置对象的一部分**，如果想用`TypeScript`写 Node.js，则需要引入第三方声明文件：`npm install @types/node --save-dev`
### 9. 函数类型
1. 函数有两种定义方式：函数声明（declaration）和函数表达式（expression），在`TypeScript`中，这两种规定类型的方式有所不同。
2. 函数声明（declaration）
   - 函数有输入，有输出，都要进行约束，如果是函数声明的方式，约束起来比较简单。实例代码：
     ```typescript
      function add(x: number, y: number): number {
          return x + y ;
      }
     ``` 
   - **注意，输入多余的（或者少于要求的）参数，是不被允许的。**
3. 函数表达式（expression）
   - 函数表达式相当于是将一个匿名函数的指针赋值给一个变量，所以我们在约束的时候，左右两端都要进行约束。即变量这块进行约束，匿名函数那块也要进行约束。实例代码：
     ```typescript
      let myAdd: (x: number, y: number) => number = function(x: number, y: number): number {
          return x + y ;
      }
     ```
   - 注意：在 TypeScript 的类型定义中，`=>` 用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型。与ES6中的箭头函数（`=>` ）不一样。
4. 用接口定义函数的形状
   - 使用接口的形式，定义一个函数需要的形状（输入类型，返回值类型）。示例代码：
     ```typescript
        interface getLength {
            (str: string): number
        }
        
        //采用函数表达式|接口定义函数的方式时，对等号左侧进行类型限制，
        // 可以保证以后对函数名赋值时保证参数个数、参数类型、返回值类型不变。
        let myFunc: getLength;
        myFunc = function (str: string) {
            return str.length ;
        }
     ```
5. 可选参数
   - 与接口中的可选属性类似，我们用`?`表示可选的参数。示例代码：
     ```typescript
        function buildName(firstName: string, lastName?: string): string {
            if (lastName) {
                return `${firstName} ${lastName}` ;
            } else {
                return firstName ;
            }
        }
        
        let tomcat = buildName('tom', 'Cat') ;
        console.log(tomcat) ;
        let jack = buildName('jack') ;
        console.log(jack) ;
     ```
   - 函数中的可选参数，只能放在参数列表的最后，换句话说，可选参数后面不允许再出现必需参数了。
   - 下面这种形式定义会报错：
     ```typescript
        // 下面这种定义方式会报错：error TS1016: A required parameter cannot follow an optional parameter.
        function buildName2(firstName?: string, lastName: string): string {
             if (lastName) {
                 return `${firstName} ${lastName}` ;
             } else {
                 return lastName ;
             }
        }
        
        let smith = buildName2('Tom', 'Smith') ;
        console.log(smith) ;
     ```
6. 默认参数
   - ES6中，可以为参数设置默认值，TypeScript 会将添加了默认值的参数识别为可选参数。示例代码：
     ```typescript
        function buildName3(firstName: string, lastName: string = 'Cat'): string {
            return firstName + ' ' + lastName ;
        }
        
        console.log(buildName3('tom', 'csgo')) ;
        console.log(buildName3('jack')) ;
     ```
   - 设置了默认参数，此时就不受「可选参数必须接在必需参数后面」的限制了。示例代码：
     ```typescript
        function buildName4(firstName: string = 'Tom', lastName: string): string {
            return firstName + ' ' + lastName ;
        }
     ```
7. 剩余参数
   - ES6中的剩余参数。示例代码：
     ```typescript
        function allNumSum(first, ...rest) {
            // rest是一个数组
            if (rest.length !== 0) {
                var ret = rest.reduce((pre, cur) => {
                    return pre + cur ;
                })
        
                return ret + first ;
            }
        
            return first ;
        }
        
        console.log(allNumSum(1, 2, 3, 4)) ;
        console.log(allNumSum(1, 2)) ;
        console.log(allNumSum(1)) ;

     ```
   - `...rest`是一个数组，所以我们可以用数组的类型来定义rest参数只能是最后一个参数。示例代码如下：
     ```typescript
        function allNumSum2(first: number, ...rest: number[]): number {
            // rest是一个数组
            if (rest.length !== 0) {
                var ret = rest.reduce((pre, cur) => {
                    return pre + cur ;
                })
        
                return ret + first ;
            }
        
            return first ;
        }
        
        console.log(allNumSum2(1, 2, 3, 4, 5)) ;
        console.log(allNumSum2(1, 3)) ;
        console.log(allNumSum2(1)) ;
     ```
8. 函数重载
   - JavaScript是没有重载概念的，定义了多个同名但是参数不同的函数，执行时，只会以最后定义的为主。在TypeScript中，引入了重载。示例代码：
     ```typescript
        function reverse(content: number): number ;
        function reverse(content: string): string ;
        function reverse(content: number|string):number|string {
            if (typeof content === 'number') {
                return Number(content.toString().split('').reverse().join('')) ;
            } else if (typeof content === 'string') {
                return content.split('').reverse().join('') ;
            } else {
                return undefined ;
            }
        }
     ```
   - 我们重复定义了多次函数 reverse，前几次都是函数定义，最后一次是函数实现。
   - TypeScript会从最前面的函数定义开始匹配，如果有多个函数具有包含关系，这里的包含指的是重载，应该把精确的定义写在前面。
   - 也就是说，TypeScript中，主要是函数同名，但是参数和返回值的类型不同，需要精确定义输入类型和输出类型，最后实现。
   - 注意：typescript 的函数重载仅仅是类型重载，不是真正意义上的函数重载。并且真正 implementation 的那个函数需要覆盖你所有类型重载的函数的签名。也就是下面这样定义会报错：
     ```typescript
        // error TS2393: Duplicate function implementation.
        function reverse(content: number): number {
             return Number(content.toString().split('').reverse().join('')) ;
        }
        function reverse(content: string): string {
             return content.split('').reverse().join('') ;
         
        }
     ```
   - 不能定义多个函数体，只能最后定义一个包含前面所有输入类型和输出类型的同名函数以及函数体。
### 10. 类型断言
1. 语法
   - 语法1：`值 as 类型`
   - 语法2：`<类型> 值`
   - 不推荐第二种写法，因为会与React以及ts中的泛型混淆，所以，我们统一使用第一种语法
2. 断言作用1：将一个联合类型断言为其中一个类型
3. 断言作用2：将一个父类断言为更加具体的子类
4. 断言作用3：将一个类型断言为any （**慎用**）
5. 断言作用4：将any断言为具体类型
6. 类型断言的限制
   - TypeScript时结构类型系统，类型之间的比较只会比较它们最终的结构，而忽略定义时的关系。我们定义下面的结构：
     ```typescript
        interface Animal {
            name: string
        }
        
        interface Cat {
            name: string
            run(): void
        
        }
        
        let tom: Cat = {
            name: 'Tom',
            run: () => {
                console.log('run') ;
            }
        }
        let animal: Animal = tom ;
     ```
   - Cat 包含了 Animal 中的所有属性，除此之外，它还有一个额外的方法 run。TypeScript 并不关心 Cat 和 Animal 之间定义时是什么关系，而只会看它们最终的结构有什么关系——所以它与 Cat extends Animal 是等价的。在继承的情况下，子类的实例可以赋值给类型为父类的变量，所以上面的tom可以赋值给类型为Animal的变量。示例代码如下：
     ```typescript
        interface Animal {
            name: string
        }
        
        interface Cat extends Animal{
            run(): void
        }
     ```
   - 当 Animal 兼容 Cat 时，它们就可以互相进行类型断言了，示例代码如下：
     ```typescript
        interface Animal {
            name: string
        }     
        interface Cat {
            name: string
            run(): void
        }    
        function testAnimal(animal: Animal) {     
            return (animal as Cat) ;
        }     
        function testCat(cat: Cat) {     
            return (cat as Animal) ;
        }
     ```
   - 总结：
     - 笼统的说，就是A能兼容B，那么 A 能够被断言为 B，B 也能被断言为 A。
     - 如果B能兼容A，那么 B 能够被断言为 A，A 也能被断言为 B。
     - 所谓的兼容，我的理解是，A兼容B，指的是B具有A所有的属性和方法。这个案例中，`Cat`具有`Animal`所有属性，所以，`Animal`是兼容`Cat`的。
     
7. 类型断言与类型声明的区别
   - 先看一段代码：
     ```typescript
        interface Animal {
            name: string
        }    
        interface Cat {
            name: string
            run(): void
        } 
        let tom: Cat = {
            name: 'tom',
            run: () => {
                console.log('run') ;
            }
        }
        let animal: Animal = tom ;
     ```
   - 在上例中，因为Animal兼容Cat，所以可以将tom直接赋值给animal。
   - 将 animal 断言为 Cat 赋值给 jack，也是可以的：
     ```typescript
        let an: Animal = {
            name: 'Jack'
        }
        
        // an可以被断言成Cat类型，也是因为Animal兼容Cat
        let jack = an as Cat ;
        let animal2 = tom as Animal ;
     ```
   - 如果直接将Animal类型的变量赋值给Cat类型的变量，则会报错：
     ```typescript
        // error TS2741: Property 'run' is missing in type 'Animal' but required in type 'Cat'.
        let jack2: Cat = an ;
     ```
   - 想要将animal类型的anl赋值给类型为Cat的jack2，Cat必须兼容Animal，也就是说，Cat有的，Animal都得有。但是，Animal不具备这样的特性，所以无法赋值给jack2。换一种说法，Animal 可以看作是 Cat 的父类，当然不能将父类的实例赋值给类型为子类的变量。
   - 总结：
     - A能断言为B，只需满足A兼容B或者B兼容A就可以。
     - A赋值给B，则B必须兼容A。
     
8. 使用类型断言的注意事项
   - 不能使用双重断言，即不能这样写：`as any as Foo`，这种不加限制的将一个类型转换为另外一个类型，极有可能在运行时报错。
   - 类型断言不是类型转换，只影响编译时的类型，类型断言的结果在编译完成后，就会被删除。因此类型断言并不能完成真正的类型转换，不会影响变量的类型，要实现真正的类型转换，直接调用类型转换的方法，如`Number()`、`Boolean()`等。
   - 优先使用类型声明和泛型。与类型断言相比较。类型声明更加严格。同时使用泛型，在使用时指定具体的类型效果上比断言好。

### 11. 声明文件
1. 当使用第三方库时，我们需要引用它的声明文件，才能获得对应的代码补全、接口提示等功能。
2. 所谓的声明文件，我的理解就是将这个第三方库中需要对外暴露出来的方法和变量进行声明，这样我们就可以直接引用这些方法和变量。
3. 通常我们会把声明语句放到一个单独的文件中，以jQuery为例，声明文件的名称是：`jQuery.d.ts`，**声明文件必须以`.d.ts`为后缀**。内容如下：
   ```TypeScript
       // src/jQuery.d.ts
       declare var jQuery: (selector: string) => any;
   ```
4. 一般来说，ts 会解析项目中所有的 *.ts 文件，当然也包含以 .d.ts 结尾的文件。所以当我们将 jQuery.d.ts 放到项目中时，其他所有 *.ts 文件就都可以获得 jQuery 的类型定义了:
     ```
        /path/to/project
        ├── src
        |  ├── index.ts
        |  └── jQuery.d.ts
        └── tsconfig.json
     ```
5. 对于第三方的声明文件，推荐使用 `@types` 统一管理第三方库的声明文件。`@types` 的使用方式很简单，直接用 `npm` 安装对应的声明模块即可，以 `jQuery` 举例：`npm install @types/jquery --save-dev`
6. 搜索第三方声明文件的地址：https://microsoft.github.io/TypeSearch/
7. 涉及到的新语法如下表所示：
    
   语法|说明
   :---:|:---:
   `declare var` | 声明全局变量
   `declare function` | 声明全局方法
   `declare class` | 声明全局类
   `declare enum` | 声明全局枚举类型
   `declare namespace` | 声明（含有子属性的）全局对象
   `interface 和 type` | 声明全局类型
   `export` | 导出变量
   `export namespace` | 导出（含有子属性的）对象
   `export default` | ES6 默认导出
   `export =` | commonjs 导出模块
   `export as namespace` | UMD 库声明全局变量
   `declare global` | 扩展全局变量
   `declare module` | 扩展模块
   `/// <reference />` | 三斜线指令
### 12. 书写声明文件
1. 当我们使用的第三方库没有提供声明文件的时候，需要我们自己去写声明文件。声明文件的内容和使用方式在不同的应用场景下有所不同。主要有以下几种情况：
    - `全局变量`：通过` <script> `标签引入第三方库，注入全局变量
    - `npm 包`：通过 `import foo from 'foo'` 导入，符合 ES6 模块规范
    - `UMD 库`：既可以通过 `<script>` 标签引入，又可以通过 import 导入
    - `直接扩展全局变量`：通过 `<script>` 标签引入后，改变一个全局变量的结构
    - `在 npm 包或 UMD 库中扩展全局变量`：引用 npm 包或 UMD 库后，改变一个全局变量的结构
    - `模块插件`：通过 `<script>` 或 `import` 导入后，改变另一个模块的结构
2. 每一种场景下的声明方式等到需要的时候在学。^_^

## 二、进阶
### 1. 类型别名
- 用法：使用 type 创建类型别名
- 作用：类型别名常用于联合类型
- 我的理解：类型别名的作用主要是增加语义性，实际上，新的名字是对类型的一个引用，约束还是原来的类型起作用。示例代码：
  ```typescript
      // 将string类型取另外一个名字——name，接下来我们就可以使用Name代替string，语义性更强
      type Name = string ;
      // 将一个函数取一个别名——NameResolve，等号右侧是TypeScript中的函数的定义，=>左侧表示输入类型，右侧表示输出类型
      type NameResolve = () => string ;
      
      type NameOrResolve = Name | NameResolve ;
      
      function getName(n: NameOrResolve): Name {
          if (typeof n === 'string') {
              return n ;
          } else {
              return n() ;
          }
      }
      
      console.log(getName('tom')) ;
      console.log(getName(() => 'jack')) ;

  ```
### 2. 字符串字面量类型
1. 符串字面量类型用来约束取值只能是某几个字符串中的一个。
2. 也是使用`type`关键字进行定义。使用`|`用来分隔可选的几个字符串。示例代码如下：
   ```typescript
     // 使用type定义一个字符串字面量类型，取值只能是red、green、blue这三个字符串之一
     type colors = 'red' | 'green' | 'blue' ;
     
     function chooseColor(color: colors): string {
         return `The color I choose is ${color}` ;
     }
     
     console.log(chooseColor('red')) ;
     console.log(chooseColor('green')) ;
     console.log(chooseColor('blue')) ;
     
     // yellow不在取值colors约定的取值范围内，所以传入yellow会报错
     // error TS2345: Argument of type '"yellow"' is not assignable to parameter of type 'colors'.
     // console.log(chooseColor('yellow')) ;
   ```
### 3. 元组（Tuple）
1. 元组（Tuple）的概念：合并了不同类型的对象，而数组（Array）则是用来合并同种类型的对象
2. 注意与python中的元组的概念进行区分，在python中，元组与列表类似，都是线性表，主要区别是元内容不可变
3. 元组有以下几个特点：
   - 内容可变，但是类型不可变。
   - 初始化可以不用赋值。赋值的时候，数量和对应的数据类型必须对应正确。
   - 可以使用push()和pop()方法。
4. 元组的定义方式
   - 定义时就赋值
     ```typescript
        // 定义一对值分别为 string 和 number 的元组
        let tom: [string, number] = ['tom', 25];
        console.log(tom[0]);
        console.log(tom[1]);
        
        // 在TypeScript中，元组的内容可以修改
        tom[0] = 'jack';
        console.log(tom[0]);
     ```
   - 先定义后赋值，注意，赋值时，如果元组的类型中没有可选类型，则定义了多少个类型，就必须赋值多少个。
     ```typescript
        // 先定义，后赋值
        let smith: [string, number];
        // 赋值的时候，必须按照指定的类型进行赋值，否则就会报错
        smith = ['Smith', 30];
        // error TS2322: Type 'string' is not assignable to type 'number'.
        // smith = [30, 'Smith'];
     ```
5. 通过索引的方式进行赋值，编译过程不会出错，但是在执行js文件就会出错。 我觉得原因可能是元组在js中表现为数组，有一段TS代码，如下所示，先定义类型，后通过索引的方式进行赋值：
   ```typescript
      let rose: [string, number];
      rose[0] = 'rose';
      rose[1] = 20;
   ```  
   上面的ts代码编译为js代码后，是这样：
   ```javascript
      var rose;
      rose[0] = 'rose';
      rose[1] = 20;
   ```  
   我们并没有将rose定义为数组，所以运行会报错。
6. 元组可以使用解构操作。
   ```typescript
      let smith: [string, number];
      // 赋值的时候，必须按照指定的类型进行赋值，否则就会报错
      smith = ['Smith', 30];
      let [a, b] = smith;
      // Smith
      console.log('a', a);
      // 30
      console.log('b', b);
   ```
7. 元组也可以设置可选元素，用?表示
   ```typescript
      // 元组也可以设置可选元素，用?表示
      let phone: [string, number?];
      phone = ['apple', 5500];
      // the name is apple, and the price is 5500
      console.log(`the name is ${phone[0]}, and the price is ${phone[1]}`);
      // 因为元组的第二个元素是可选的，所以，可以不用定义第二个元素
      phone = ['vivo'];
      // the name is vivo
      console.log(`the name is ${phone[0]}`);
   ```  
   元组设置可选元素的目的是：有一些场景我们需要的元素数量不确定，比如说坐标，可以是一维坐标，二维坐标，还可以是三维坐标，我们不需要定义三个元组，使用可选元素，定义一个即可。示例代码如下：
   ```typescript
      type point = [number, number?, number?];
      const one: point = [1];
      const two: point = [1, 2];
      const three: point = [1, 2, 3];
      console.log('length is', one.length);
      console.log('length is', two.length);
      console.log('length is', three.length);
   ```
8. 元组类型的剩余元素。元组的最后一个元素，可以是剩余参数，这样可以不用限制剩余元素的个数。
   ```typescript
      type multiColors = [number, ...string[]];
      
      let zeroColor: multiColors = [0];
      let oneColor: multiColors = [1, 'red'];
      let twoColors: multiColors = [2, 'red', 'blue'];
      // [ 0 ]
      console.log(zeroColor);
      // [ 1, 'red' ]
      console.log(oneColor);
      // [ 2, 'red', 'blue' ]
      console.log(twoColors);
   ```
9. 在定义函数时我们也可以使用剩余参数语法
   ```typescript
      // 函数传入的参数是一个元组，我们限制其元素的类型为[string, string, number]
      // 同时使用了剩余参数语法
      function useTupleAsRest(...args: [string, string, number]): void {
          let [arg1, arg2, arg3] = args;
            
          console.log('first', arg1);
          console.log('second', arg2);
          console.log('third', arg3);
      }
      // first aa
      // second bb
      // third 25
      useTupleAsRest('aa', 'bb', 25);
   ```
10. 元组类型的展开表达式。若最后一个参数是元组类型的展开表达式，那么这个展开表达式相当于元组元素类型的离散参数序列。因此可以使用展开表达式语法（...）。
    ```typescript
       type Point3D = [number, number, number];
       let p1: Point3D = [2, 5, 8];
       let p2: Point3D = [...p1];
       console.log(p1);
       console.log(p2);
       
       /**
        * 使用剩余参数，接收离散参数序列
        * @param args
        */
       const drawPoints = (...args: Point3D) => {
           console.log('Points', args);
       
       } 
       drawPoints(10, 20, 30);
       // 使用索引的方式访问元组中的元素
       drawPoints(p1[0], p1[1], p1[2]);    
       // 这里使用元组的展开表达式，将其展开为离散形式的参数列表
       // 在drawPoints()使用剩余参数，进行接收
       drawPoints(...p1);
    ```
11. 给元组设置只读类型。可以为任何元组类型加上 readonly 关键字前缀，以使其成为只读元组。
    ```typescript
       // 使用readonly关键字设置Fruits这个元组为只读属性
       type Fruits= readonly [string, string];
       const apple: Fruits = ['apple', 'red'];
       console.log(apple[0], apple[1]);
       
       // 设置了只读属性后，修改某个元素就会报错
       // error TS2540: Cannot assign to '0' because it is a read-only property.
       // apple[0] = 'banana';
    ```
12. 越界元素。越界元素指的是，超出了元组定义时的数量的元素，比如说，定义元素有两个元素，现在要添加第三个元素，这第三个元素就是越界元素，添加越界元素时，元素类型会被限制为元组中每个类型的联合类型。也就是所有类型的一个并集。示例代码如下：
    ```typescript
       let student: [string, number];
       student = ['tom', 114];
       // 可以使用push()方法添加越界元素
       student.push('pku');
       // [ 'tom', 114, 'pku' ]
       console.log(student);
       // 当添加越界元素时，元素类型会被限制为元组中每个类型的联合类型，在这个例子中，必须是：'string | number'
       // Argument of type 'true' is not assignable to parameter of type 'string | number'.
       // student.push(true);
    ```  
    还可以使用pop()方法，移除元组的最后一个元素：
    ```typescript
       let student: [string, number];
       student = ['tom', 114];
       student.push('pku');
       // [ 'tom', 114, 'pku' ]
       console.log(student);
       student.pop();
       // [ 'tom', 114 ]
       console.log(student);
       student.pop();
       // [ 'tom' ]
       console.log(student);
    ```
    
### 4. 枚举（Enum）
1. 作用：枚举类型用于取值被限定在一定范围内的场景，比如一周只能有七天，颜色限定为红绿蓝等。
2. 枚举使用enum关键字定义。
3. 枚举成员会被赋值为从 0 开始递增的数字，同时也会对枚举值到枚举名进行反向映射。
### 5. 类（class）
