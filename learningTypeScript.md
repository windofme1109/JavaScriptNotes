## 学习TypeScript
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
1. 内置对象
   - `JavaScript`中有很多内置对象，它们可以直接在`TypeScript`中当做定义好了的类型。
   - 内置对象是指根据标准在全局作用域（Global）上存在的对象。这里的标准是指 ECMAScript 和其他环境（比如 DOM）的标准。
   - ECMA提供的标准对象包括：Boolean、Number、Date、String等。详细的内容可以查看MDN：[Standard built-in objects](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects)
   -DOM提供的标准对象主要有：Document、HTMLElement、Event、NodeList等。也就是DOM interfaces。详细的内容可以查看MDN：[Document Object Model (DOM)](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)
   - BOM提供的标准对象主要有：Window（最顶层的对象）、location、history等。
   - ECMAScript、DOM、BOM提供的标准对象都在 [TypeScript 核心库的定义文件](https://github.com/Microsoft/TypeScript/tree/master/src/lib) 中。
   - 注意：**Node.js 不是内置对象的一部分**，如果想用`TypeScript`写 Node.js，则需要引入第三方声明文件：`npm install @types/node --save-dev`
### 8. 函数类型
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