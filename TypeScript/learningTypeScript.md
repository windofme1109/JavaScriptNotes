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
    - [6. 接口（interface）](#6-%E6%8E%A5%E5%8F%A3interface)
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
    - [4. 枚举（Enum）](#4-%E6%9E%9A%E4%B8%BEenum)
    - [5. 类（Class）](#5-%E7%B1%BBclass)
    - [6. 类与接口](#6-%E7%B1%BB%E4%B8%8E%E6%8E%A5%E5%8F%A3)
    - [7. 泛型](#7-%E6%B3%9B%E5%9E%8B)
    - [8. 声明合并](#8-%E5%A3%B0%E6%98%8E%E5%90%88%E5%B9%B6)
    - [9. 交叉类型](#9-%E4%BA%A4%E5%8F%89%E7%B1%BB%E5%9E%8B)
    - [10. 类型映射 —— Partial](#10-%E7%B1%BB%E5%9E%8B%E6%98%A0%E5%B0%84--partial)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

------

# 学习TypeScript
## 一、基础
### 1. TypeScript是什么
1. TypeScript官网：[TypeScript: Typed JavaScript at Any Scale.](https://www.typescriptlang.org/)

2. TypeScript教程：[TypeScript入门教程](https://ts.xcatliu.com/introduction/hello-typescript.html)
3. TypeScript资料汇总：[awesome-typescript](https://github.com/semlinker/awesome-typescript)

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
- 作用：
  - 用于标识 TypeScript 项目的根路径。
  - 用于配置 TypeScript 编译器。
  - 用于指定编译的文件。
- 重要字段
  - `files` - 设置要编译的文件的名称。
  - `include` - 设置需要进行编译的文件，支持路径模式匹配。
  - `exclude` - 设置无需进行编译的文件，支持路径模式匹配。
  - `compilerOptions` - 设置与编译流程相关的选项。
- `compilerOptions` 选项  
  `compilerOptions` 支持很多选项，常见的有 `baseUrl`、 `target`、`baseUrl`、 `moduleResolution` 和 `lib` 等。  
  compilerOptions 每个选项的详细说明如下：
  ```json
     {
       "compilerOptions": {
     
         /* 基本选项 */
         "target": "es5",                       // 指定 ECMAScript 目标版本: 'ES3' (default), 'ES5', 'ES6'/'ES2015', 'ES2016', 'ES2017', or 'ESNEXT'
         "module": "commonjs",                  // 指定使用模块: 'commonjs', 'amd', 'system', 'umd' or 'es2015'
         "lib": [],                             // 指定要包含在编译中的库文件
         "allowJs": true,                       // 允许编译 javascript 文件
         "checkJs": true,                       // 报告 javascript 文件中的错误
         "jsx": "preserve",                     // 指定 jsx 代码的生成: 'preserve', 'react-native', or 'react'
         "declaration": true,                   // 生成相应的 '.d.ts' 文件
         "sourceMap": true,                     // 生成相应的 '.map' 文件
         "outFile": "./",                       // 将输出文件合并为一个文件
         "outDir": "./",                        // 指定输出目录
         "rootDir": "./",                       // 用来控制输出目录结构 --outDir.
         "removeComments": true,                // 删除编译后的所有的注释
         "noEmit": true,                        // 不生成输出文件
         "importHelpers": true,                 // 从 tslib 导入辅助工具函数
         "isolatedModules": true,               // 将每个文件做为单独的模块 （与 'ts.transpileModule' 类似）.
     
         /* 严格的类型检查选项 */
         "strict": true,                        // 启用所有严格类型检查选项
         "noImplicitAny": true,                 // 在表达式和声明上有隐含的 any类型时报错
         "strictNullChecks": true,              // 启用严格的 null 检查
         "noImplicitThis": true,                // 当 this 表达式值为 any 类型的时候，生成一个错误
         "alwaysStrict": true,                  // 以严格模式检查每个模块，并在每个文件里加入 'use strict'
     
         /* 额外的检查 */
         "noUnusedLocals": true,                // 有未使用的变量时，抛出错误
         "noUnusedParameters": true,            // 有未使用的参数时，抛出错误
         "noImplicitReturns": true,             // 并不是所有函数里的代码都有返回值时，抛出错误
         "noFallthroughCasesInSwitch": true,    // 报告 switch 语句的 fallthrough 错误。（即，不允许 switch 的 case 语句贯穿）
     
         /* 模块解析选项 */
         "moduleResolution": "node",            // 选择模块解析策略： 'node' (Node.js) or 'classic' (TypeScript pre-1.6)
         "baseUrl": "./",                       // 用于解析非相对模块名称的基目录
         "paths": {},                           // 模块名到基于 baseUrl 的路径映射的列表
         "rootDirs": [],                        // 根文件夹列表，其组合内容表示项目运行时的结构内容
         "typeRoots": [],                       // 包含类型声明的文件列表
         "types": [],                           // 需要包含的类型声明文件名列表
         "allowSyntheticDefaultImports": true,  // 允许从没有设置默认导出的模块中默认导入。
     
         /* Source Map Options */
         "sourceRoot": "./",                    // 指定调试器应该找到 TypeScript 文件而不是源文件的位置
         "mapRoot": "./",                       // 指定调试器应该找到映射文件而不是生成文件的位置
         "inlineSourceMap": true,               // 生成单个 soucemaps 文件，而不是将 sourcemaps 生成不同的文件
         "inlineSources": true,                 // 将代码与 sourcemaps 生成到一个文件中，要求同时设置了 --inlineSourceMap 或 --sourceMap 属性
     
         /* 其他选项 */
         "experimentalDecorators": true,        // 启用装饰器
         "emitDecoratorMetadata": true          // 为装饰器提供元数据的支持
       }
     }
  ```
  
  
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
1. 联合类型（Unoin Types）。指的是表示取值可以为多种类型中的一种。
2. 联合类型使用|分隔不同的类型。示例代码：
   ```typescript
      let myLuckyNumber: string | number ;
      myLuckyNumber = 'six' ;
      myLuckyNumber = 6 ;
      console.log(myLuckyNumber) ;
   ```
   `myLuckyNumber`的取值只能是`string`或`number`二者之一。赋值其他类型会报错。
   ```typescript
      // error TS2322: Type 'true' is not assignable to type 'string | number'.
      // myLuckyNumber = true ;
   ```
3. 当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们只能访问此联合类型的所有类型里共有的属性或方法。
   ```typescript
      // function getLength(something: string|number): number {
      // 报错 error TS2339: Property 'length' does not exist on type 'string | number'.
      // length属性并不是string和number共有的属性
      //     return something.length ;
      // }
   ```  
   length属性并不是string和number共有的属性，所以会报错。
   ```typescript
      function getString(something: string | number): string {
          // toString()是string和number类型共有的方法，所以不会报错
          return something.toString() ;
      }
      
      console.log(getString('abcdefg')) ;
      console.log(getString(135789)) ;
   ```  
   `toString()`是`number`和`string`共有的方法，所以不会报错。
4. 联合类型的变量在被赋值的时候，会根据类型推论的规则推断出一个类型。
   ```typescript
      let mln: string | number ;
      mln = 'seven' ;
      console.log(mln.length) ;
      mln = 7 ;
      // mln被赋值为7，TypeScript推断其类型为number，但是number并没有length这个属性，所以会报错
      // error TS2339: Property 'length' does not exist on type 'number'.
      // console.log(mln.length) ;
      // 7
      console.log(mln.toString()) ;
   ```

### 6. 接口（interface）
1. 使用接口来定义对象的类型。
   - 接口是对行为的抽象，而具体如何行动需要由类（classes）去实现（implement）。
   - 在TypeScript中，接口常常用来对对象的形状（shape）进行约束。
2. 定义一个接口，并使用其来约束对象的形状：
   ```typescript
      interface Person {
          name: string,
          age: number
      }
      // 定义一个对象，其类型为Person，tom这个对象的形状必须和Person一样
      // 赋值时，对象的变量的形状必须和接口一致
      let tom: Person = {
          name: 'Tom',
          age: 25
      } ;
      console.log('name', tom.name) ;
   ``` 
3. 对象中的属性的个数和类型必须同接口保持一致，多或者少都不行，属性不一致也不行。
   ```typescript
      // 对应的变量多于接口定义的属性，会报错：
      //  error TS2322: Type '{ name: string; age: number; gender: string; }' is not assignable to type 'Person'.
      //   Object literal may only specify known properties, and 'gender' does not exist in type 'Person'.
       let jack: Person = {
           name: 'Jack',
           age: 25,
           gender: 'male'
       }
      
      // 定义的变量比接口少了一些属性是不允许的
      // 对应的变量多余接口定义的属性，同样会报错：
      // error TS2741: Property 'age' is missing in type '{ name: string; }' but required in type 'Person'.
       let jack2: Person = {
           name: 'Jack'
       }
   ```
4. 可选属性，使用`?`定义一个可选属性，这个属性可以在对象中出现，也可以不出现。
   ```typescript
      interface Student {
          name: string,
          age: number,
          // 使用一个?表示，这是一个可选的属性，表示这个属性可以存在，也可以不存在
          phone?: number
      }
      
      // 可选属性的含义是该属性可以不存在
      let s1: Student = {
          name: '张三',
          age: 20,
      }
   
      let s2: Student = {
          name: '李四',
          age: 25,
          phone: 12345678
      }
      
      console.log(s2.phone) ;
   ```
5. 任意属性。如果希望接口拥有任意属性，那么我们可以使用`[]`定义属性名。如下所示：
   ```typescript
      interface Teacher {
          name: string,
          age?: number,
          // 定义属性名取string类型的值
          // 属性值则是any类型
          [propName: string]: any
      }
      
      let t1: Teacher = {
          name: 'smith',
          gender: 'male'
      }
      
      let t2: Teacher = {
          name: 'rose',
          school: 'bupt',
          age: 25
      }
   ``` 
    任意属性的定义类似与我们在使用对象的时候，如果属性名称是变量，那么就需要通过`[]`方式获取。
6. 一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集。
   ```typescript
      /**
       * 
       * Property 'price' of type 'number' is not assignable to string index type 'string'.
       */
      interface Fruits {
          name: string,
          price?: number,
          [propName: string]: string
      }
      
      let apple: Fruits = {
          name: 'apple',
          price: 10,
          size: 'large'
      
      }
   ```  
   如上例所示，任意属性定义为string类型，那么name和price也必须时string类型的子集（在我看来，就都得是string），而price的类型时number，并不是string的子集，在编译过程中，会报错。
7. 一个接口中只能定义一个任意属性。如果接口中有多个类型的属性，则可以在任意属性中使用联合类型。  
   当时在学习的时候有一个问题：接口中有多个类型的属性，为什么不适用any定义呢？  
   我觉得原因是，精确地限定属性的数据类型。如果是any的话，那么对于我们不需要或者说不允许的类型，起不到限制的作用。
   ```typescript
      interface Fruits {
          name: string,
          price?: number,
          // 接口中只能定义一个任意属性，如果接口中有多个类型的属性（如name为string，price为number），则可以使用联合类型，定义为：string|number
          [propName: string]: string | number
      }
      
      let apple: Fruits = {
          name: 'apple',
          price: 25,
          size: 'large',
          color: 'red'
      }
      
      let orange: Fruits = {
          name: 'orange',
          price: 10,
          size: 'small',
          series: 188
      }
      console.log(orange.series) ;
   ```  
   **注意**：我们在接口中只能定义一个任意属性，但是在对象中，我们就可定义多个属性，只要属性名和属性值同任意属性的定义相同即可。
8. 只读属性。希望对象中的一些字段只能在创建的时候被赋值，那么可以用 `readonly` 定义只读属性。
   ```typescript
      interface Workers {
          // 使用readonly定义一个只读属性，该属性在变量创建时赋值，然后就只读，不能修改
          readonly id: number,
          name: string,
          age?: number,
          [propName: string]: any
      }  
      // 在定义时，给只读属性赋值
      let w1: Workers = {
          id: 10578,
          name: '张三',
          gender: 'male'
      }
      // 只读属性只能在创建变量时赋值时，当试图修改时，就会报错
      // error TS2540: Cannot assign to 'id' because it is a read-only property.
      w1.id = 18942 ;
      // 创建变量时，如果没有给只读属性id赋值，那么会报错：
      // error TS2741: Property 'id' is missing in type '{ name: string; gender: string; }' but required in type 'Workers'.
      let w2: Workers = {
          name: '李四',
          gender: 'female'
      }
   ```
   **注意**：只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候。创建变量时，如果没有给只读属性id赋值，那么会报错。

### 7. 数组类型
1. 数组类型。用来约束数组的成员的类型。
2. 定义方式：
   1. 类型+方括号
      ```typescript
         let arr1: number[] = [1, 1, 2, 3, 5] ;
         // 定义后，数组不允许出现其他类型
         // error TS2322: Type 'string' is not assignable to type 'number'.
         // let arr2: number[] = [1, 2, '3', 5] ;
         // 数组的方法也会对传入的数据进行检查，不符合类型约束的，就会报错
         // error TS2345: Argument of type '"6"' is not assignable to parameter of type 'number'.
         // arr1.push('6') ;
      ```  
      对数组元素的类型进行约束以后，是不能赋值其他类型的，同时如果使用数组方法向数组添加元素，TypeScript也会对传入的数据类型进行检查，一旦是其他类型，就会报错。
   2. 数组泛型（Array Generic）
      ```typescript
         let arr2: Array<number> = [1, 1, 2, 3, 5] ;
      ```
   3. 接口定义
      ```typescript
         interface NumberArray {
            // 规定，索引是数字时，值必须也是数字
            [index: number]: number
         }
         let arr3: NumberArray = [1, 1, 2, 3, 5] ;
      ```
      **注意：通常我们不使用接口的方式定义数组，因为比较复杂。**
3. 类数组。类数组指的是具有数组的length属性，以及索引特性（索引是数字），但是不具备数组的操作方法，如pop，push等的对象。如函数中的`arguments`就是一个类数组对象。
4. 通常使用接口来定义一个类数组对象。
   ```typescript
      // 对于类数组对象，我们必须使用接口进行定义
      function add(): void {
          // 在这个接口中，我们规定索引为数字时，属性值也必须为数字，同时还规定了length和callee属性，
          // 同arguments这个类数组对象所具有的属性是一致的
          let args: {
              [index: number]: number,
              length: number,
              callee: Function
          } = arguments ;
      
      }
   ```  
   我们使用接口约束arguments这个类数组对象时，接口所定义的属性必须同arguments这个类数组对象所具有的属性是一致的。如果不一致，就会报错。
5. 事实上常用的类数组都有自己的接口定义，如 IArguments, NodeList, HTMLCollection 等。
   ```typescript
      function sub(): void {
          // IArguments就是TypeScript定义好的类型
          let args: IArguments = arguments ;
      }
   ```
6. 我们也可以将数组中元素的类型定义为any，表示允许出现任何类型。
   ```typescript
      let arr4: any[] = [1, '2', 3, {name: "apple"}, true] ;
      console.log(arr4[3]) ;
   ```           

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
3. 枚举成员会被赋值为从 0 开始递增的数字，同时也会对枚举值到枚举名进行反向映射。示例代码：
   ```typescript
      // 定义一个枚举类
      // 限定了取值范围
      enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};
      
      // 枚举成员会被赋值为从 0 开始递增的数字，同时也会对枚举值到枚举名进行反向映射
      // 枚举名到枚举值的映射
      console.log(Days['Sun'] === 0);    // true
      console.log(Days['Mon'] === 1);    // true
      console.log(Days['Tue'] === 2);    // true
      console.log(Days['Wed'] === 3);    // true
      console.log(Days['Sat'] === 6);    // true
      
      // 枚举值到枚举名的映射
      console.log(Days[1] === 'Mon');    // true
      console.log(Days[2] === 'Tue');    // true
      console.log(Days[5] === 'Fri');    // true
      console.log(Days[0] === 'Sun');    // true
   ```
4. 手动赋值。可以给枚举类里面的值，手动赋值。未手动赋值的枚举项会接着上一个枚举项递增。示例代码如下：
   ```typescript
      // 手动给Sun赋值为7，Mon赋值为1，那么Mon之后的变量会在Mon的基础上，自动递增，步长为1
      enum Days2 {Sun = 7, Mon = 1, Tue, Wed, Thu, Fri, Sat};
      console.log(Days2['Wed'] === 3);    // true
      console.log(Days2['Sat'] === 6);    // true
   ``` 
5. 如果未手动赋值的枚举项与手动赋值的重复了，TypeScript 在编译过程中是不会察觉到这一点的。  
   如在下面的例子中，枚举项递增到 3 的时候与前面的 Sun 的取值重复了，但是 TypeScript 并没有报错，导致 Days[3] 的值先是 "Sun"，而后又被 "Wed" 覆盖了。也就是说，枚举名到枚举值的映射没有问题，但是枚举值到枚举名的映射，后的的就会将前面的覆盖。所有这里的3只能映射到Wed。**所以使用的时候需要注意，最好不要出现这种覆盖的情况。**
   ```typescript
       enum Days3 {Sun = 3, Mon = 1, Tue, Wed, Thu, Fri, Sat};
       console.log(Days3['Sun'] === 3) ;    // true
       console.log(Days3['Wed'] === 3) ;    // true
       console.log(Days3[3] === 'Sun') ;    // false
       console.log(Days3[3] === 'Wed') ;    // true
   ```
6. 手动赋值的可以是小数，也可也是负数，但是后续未赋值的枚举项的递增赋值仍然是1。
   ```typescript
      enum Days4 {Sun = 7, Mon = 1.5, Tue, Wed, Thu, Fri, Sat};
      console.log(Days4['Mon'] === 1.5) ;    // true
      console.log(Days4['Tue'] === 2.5) ;    // true
      console.log(Days4['Sat'] === 6.5) ;    // true
   ```
7. 常数项和计算项。枚举项有两种类型：常数项（constant member）和计算所得项（computed member），常数项就是我们在定义是就给枚举项赋值或者是未赋值而由系统分配的  
前面的示例代码中的枚举类都是常数项，而计算所得项时需要动态计算，比如一个字符串的长度。
   ```typescript
      // 定义一个计算所得项的枚举类
      // 'Blue'.length就是一个计算所得项
      enum Colors {Red, Green, Blue = 'Blue'.length};
      console.log(Colors[4] === 'Blue') ;    // true
   ```  
   注意，计算想你只能放在枚举类的最后一个位置，如果紧接在计算所得项后面的是未手动赋值的项，那么它就会因为无法获得初始值而报错。
   ```typescript
      // error TS1061: Enum member must have initializer.
      // enum Colors2 {Red = 'Red'.length, Green};
   ```
8. 常数枚举。指的是 const enum 定义的枚举类型。常数枚举与普通枚举的区别是，它会在编译阶段被删除，并且不能包含计算成员。示例代码：
   ```typescript
      // 使用const关键字定义一个常数枚举类
      const enum Directions {Up, Down, Left, Right};
      let d = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
      // 常数枚举与普通枚举的区别是，它会在编译阶段被删除，并且不能包含计算成员
      // 上面语句的编译结果如下所示：
      // var d = [0 /* Up */, 1 /* Down */, 2 /* Left */, 3 /* Right */];
      // 常数枚举只能通过字符串的方式进行访问
      console.log(Directions['Up']) ;
   ```  
   常数枚举只能通过字符串的方式进行访问，不能通过索引的方式。  
   如果包含了计算项，会在编译阶段报错。
   ```typescript
      //  error TS2474: const enum member initializers can only contain literal values and other computed enum values.
      // const enum Directions2 {Up, Down, Left, Right = 'Right'.length};
   ```
9. 外部枚举。外部枚举（Ambient Enums）是使用 declare enum 定义的枚举类型。declare 定义的类型只会用于编译时的检查，编译结果中会被删除。示例代码：
   ```typescript
      declare enum Directions2 {Up, Down, Left, Right};
      // let dir = [Directions2.Up, Directions2.Down, Directions2.Left, Directions2.Right];
      // declare 定义的类型只会用于编译时的检查，编译结果中会被删除
      // 上面的编译结果是：
      // var dir = [Directions2.Up, Directions2.Down, Directions2.Left, Directions2.Right];
   ```  
   外部枚举与声明语句一样，常出现在声明文件中。  
   外部声明也可以和 const 关键字一起使用。示例代码如下：
   ```typescript
      declare const enum Directions3 {
          Up,
          Down,
          Left,
          Right
      };
      let dir3 = [Directions3.Up, Directions3.Down, Directions3.Left, Directions3.Right];
      // 上面的编译结果是：
      // var dir3 = [0 /* Up */, 1 /* Down */, 2 /* Left */, 3 /* Right */];
      // 0
      console.log(dir3[0]) ;
   ``` 
   
### 5. 类（Class）
1. ES6中的类。传统的JavaScript存在类的概念，我们通过构造函数来模拟一个类，并使用原型继承的方式实现继承。  
   在ES6中，引入和class关键字和extends关键字。  
   class用于定义类，extends实现继承。  
2. ES6中类的用法：
   - 属性与方法
     ```typescript
        class Animal {
            public name;
            constructor(name) {
                this.name = name;
            }
            // 类中定义方法不用function关键字
            sayHi() {
                return `hello, ${this.name}`;
            }
        }
        
        let tiger: Animal = new Animal('tiger');
        console.log(tiger.sayHi());
     ```
   - 继承
     ```typescript
        class Cats extends Animal {
            constructor(name) {
                // 必须调用super()方法，相当于调用父类constructor()方法，而且必须放在第一位
                super(name);
                this.name = name;
            }
        
            // 子类自定义自己的方法
            greeting() {
                return `Meow, ${this.name}`;
            }
        }
     
         let kitty: Cats = new Cats('kitty');
         // 子类继承了父类的方法和属性，因此可以使用父类的方法
         console.log(kitty.sayHi());
         console.log(kitty.greeting());
     ```
   - getter和setter。使用 getter 和 setter 可以改变属性的赋值和读取行为。  
     如果定义了setter和getter方法，编译时，必须在最后加上 -t es5，即 `tsc 14-Advanced-Class.ts -t`。  
     表明将ts代码编译为es5即更高版本的js代码。
     ```typescript
        class School {
        
            private _name: string;
            constructor(name) {
                this.name = name;
            }
        
            // 通过实例直接访问name属性的时候，会调用与name属性同名的get方法
            get name() {
                console.log('getter');
                return this._name;
            }
            // 给name属性赋值的时候，会调用与name属性同名的set方法
            set name(newName) {
                console.log('setter');
                this._name = newName;
            }
        
            sayHello() {
                return this._name;
            }
        
        }
        
        // setter
        // getter
        let sch = new School('SSF');
        // SSF 
        console.log(sch.name);
        // setter
        // getter
        sch.name = 'BUPT';
        // BUPT
        console.log(sch.name);
        // setter
        // getter
        sch.name = 'csuft'; 
        // csuft    
        console.log(sch.name);
     ```   
     总结：
       - getter和setter定义的方法，名称必须要同属性名相同。对this.name进行操作就会调用setter/getter，也就是说setter/getter是hook函数，而真实的存储变量是_name。
       - 以下划线（_）开头的变量，我们一般默认为是私有属性。在ES7中统一规定，以#开头的变量为私有属性。
       - 如果我们没有声明`private _name`，而是直接使用`this.name`，如下所示：
         ```typescript
            class School {
                     constructor(name) {
                         this.name = name;
                }
                    
                        // 通过实例直接访问name属性的时候，会调用与name属性同名的get方法
                get name() {
                     console.log('getter');
                     return this.name;
                }
                // 给name属性赋值的时候，会调用与name属性同名的set方法
                set name(newName) {
                     console.log('setter');
                     this.name = newName;
                }
                    
                sayHello() {
                     return this.name;
                }
                    
            }
         // 栈溢出
         let sch = new School('SSF');
         ```  
         会报错：栈溢出。这是因为我们在构造函数中调用`this.name = name`，会去调用set name，在set name方法中，我们又执行this.name = name，进行无限递归，最后导致栈溢出。  
         所以，我们必须使用另外一个变量接收和存储name。也就是前面she声明的`private _name: string`。
   - 静态方法，使用static关键字定义，通过类调用，不能通过实例调用。
     ```typescript
        class Students {
            static isStudent(s) {
                return s instanceof Students;
            }
        
        }
        
        let s1 = new Students();
        // true
        console.log(Students.isStudent(s1));
     ```
3. ES7中类的用法
   - 实例属性。ES7之前，实例属性通过this.xxx定义，ES7 提案中可以直接在类里面定义。
     ```typescript
        class Person {
            // 直接定义实例属性
            name = 'jack';    
            constructor() {
            }     
        }     
        let ps = new Person();
        // jack
        console.log(ps.name);
     ```
   - 静态属性。ES7 提案中可以使用 static 定义一个静态属性，静态属性同静态方法类似，都是通过类访问，而不是通过实例访问。
     ```typescript
        class Numbers {
            static id = 115;
        }
        // 115
        console.log(Numbers.id);
     ```
4. TypeScript对类的支持
   - 修饰符：public。修饰的属性或方法是公有的，可以在任何地方被访问到，默认所有的属性和方法都是 public 的。
     ```typescript
        class Fruits {
            public name: string;
            constructor(name: string) {
                this.name = name;
            }
        
            public favourite() {
                return `I like ${this.name}`;
            }
        
        }
        
        let orange = new Fruits('orange');
        console.log(orange.name);
        console.log(orange.favourite());
     ```
   - 修饰符：private。修饰的属性或方法是私有的，不能在声明它的类的外部访问。
     ```typescript
        class Fruits2 {
            private name: string;
            constructor(name: string) {
                this.name = name;
            }
        
            public favourite() {
                return `I like ${this.name}`;
            }
        
        }
        let orange2 = new Fruits2('orange');
        // Property 'name' is private and only accessible within class 'Fruits2'.
        // console.log(orange2.name);
        console.log(orange2.favourite());
        // Property 'name' is private and only accessible within class 'Fruits2'.
        // orange2.name = 'apple';
     ```  
     使用 private 修饰的属性或方法，在子类中也是不允许访问的。
     ```typescript
        class Apple extends Fruits2 {
            constructor(name) {
                super(name);
                // error TS2341: Property 'name' is private and only accessible within class 'Fruits2'.
                // console.log(this.name);
            }
        }
     ```
   - 修饰符：protected。修饰的属性或方法是受保护的，它和 private 类似，区别是它在子类中也是允许被访问的。
     ```typescript
        class Fruits3 {
            protected name: string;
            constructor(name: string) {
                this.name = name;
            }
        
            public favourite() {
                return `I like ${this.name}`;
            }
        
        }
        
        class Banana extends Fruits3 {
            constructor(name) {
                super(name);
                console.log(this.name);
            }
        }
        
        let ff = new Fruits3('apple');
        //  error TS2445: Property 'name' is protected and only accessible within class 'Fruits3' and its subclasses.
        // console.log(ff.name);
        let fff = new Banana('BANANA');
        // protected修饰的属性和方法，只能被子类继承，但是子类实例还是无法访问这些属性和方法
        // console.log(fff.name);
     ```
   - 使用private修饰构造函数，这个类不能被实例化或者被子类继承。
     ```typescript
        // 当构造函数修饰为 private 时，该类不允许被继承或者实例化
        class Fruits4 {
            protected name: string;
            private constructor(name: string) {
                this.name = name;
            }
        
            public favourite() {
                return `I like ${this.name}`;
            }
        
        }
        
        //error TS2675: Cannot extend a class 'Fruits4'. Class constructor is marked as private.
        // class Mellon extends Fruits4 {
        //     constructor(name) {
        //         super(name);
        //     }
        // }
        // error TS2673: Constructor of class 'Fruits4' is private and only accessible within the class declaration.
        // let fr = new Fruits4('mellon');
     ```
   - 使用protected修饰构造函数，该类只能被继承，不能被实例化。
     ```typescript
        class Fruits5 {
            protected name: string;
            protected constructor(name: string) {
                this.name = name;
            }
        
            public favourite() {
                return `I like ${this.name}`;
            }
        
        }     
        class Mellon extends Fruits5 {
            constructor(name) {
                super(name);
            }
        }
        //  error TS2674: Constructor of class 'Fruits5' is protected and only accessible within the class declaration.
        // let fr = new Fruits5('mellon');
     ```
   - 修饰符和readonly还可以使用在构造函数参数中，等同于类中定义该属性同时给该属性赋值，使得代码更加简洁。
     ```typescript
        class Fruits6 {
            // public name: string;
            // 直接在参数中使用修饰符修饰name，省去在外定义
            constructor(public name) {
                this.name = name;
            }
        }
        let ff6 = new Fruits6('jacks');
        // jacks
        console.log(ff6.name);
     ```
   - readonly。只读属性关键字，只允许出现在属性声明或索引签名或构造函数中。被这个关键字修饰的属性，只能读取，不能设置。  
     **注意如果 readonly 和其他访问修饰符同时存在的话，需要写在其后面。**
     ```typescript
        class Fruits7 {
           readonly name: string;
           private readonly price: number;
           constructor(name: string, price: number) {
                this.name = name;
                this.price = price;
            }
        
            public favourite() {
                return `I like ${this.name}`;
            }
        }
        
        let ff7 = new Fruits7('apple', 55);
        
        console.log(ff7.name);
        // error TS2540: Cannot assign to 'name' because it is a read-only property.
        // ff7.name = 'banana';
     ```
   - 抽象类。抽象类是供其他类继承的基类，抽象类不允许被实例化。抽象类中的抽象方法必须在子类中被实现。在TypeScript中，使用abstract进行定义。
     ```typescript
        abstract class Phone {
            public name: string;
            constructor(name) {
                this.name = name;
            }
        
            // 方法也要用abstract定义
            // 只定义方法名，不定义方法体，这个方法必须在子类中实现
            public abstract brand();
        }
        
        // 抽象类不能被实例化
        // error TS2511: Cannot create an instance of an abstract class.
        // let pp = new Phone('Phone');
        
        class Vivo extends Phone {
            constructor(name) {
                super(name);
                this.name = name;
            }
        
            // 在子类中必须实现抽象类中的brand()方法
            brand() {
                return `the brand is ${this.name}`;
            }
        
            price() {
                return 3000;
            }
        }
        
        let x9 = new Vivo('Vivo');
        // the brand is Vivo
        console.log(x9.brand());
     ```  
     抽象类不能被实例化。  
     抽象类中定义的方法必须在子类中被实现。  
     抽象类中的方法也要用abstract定义。
5. 总结
   - 使用class关键字定义。
   - 使用extends关键字实现继承。
   - 类中定义方法不使用function关键字。
   - 可以定义getter和setter方法，限制存取某个属性的行为。getter和setter方法要与属性同名，但是存储这个属性需要设一个别名。
   - 使用static定义静态方法和属性。
   - 直接在类中定义实例属性，而不使用this.xxx的方式。
   - 修饰符：public、protected和private，用来修饰属性和方法。
   - 只读属性readonly，只能读，不能修改，与修饰符一起使用，放在修饰符后面。
   - 抽象类：abstract，是供其他类继承的基类，抽象类不允许被实例化。抽象类中的抽象方法必须在子类中被实现，抽象类中的方法也必须使用abstract定义。
   
### 6. 类与接口
1. 接口（Interfaces）的作用：
   - 用于对「对象的形状（Shape）」进行描述。
   - 就是对类的一部分行为进行抽象。
2. 接口是对不同的类的一些共同特性进行抽象和封装，使用implements关键字来实现这个接口。
3. 一个类只能有一个父类，但是可以实现多个接口，即单继承，多实现。
   ```typescript
      // 定义一个接口
      interface Alarm {
          // 接口中的方法也必须在实现这个接口的类中去定义方法体
          alert(): void;
      }
      
      class Door {
      
      }
      
      class SecurityDoor extends Door implements Alarm {
          alert() {
              console.log('防盗门发出警报！！！！！！！！！');
          }
      }
      
      class Car implements Alarm {
          alert() {
              console.log('汽车发出警报！！！！！！！！！！！');
          }
      }
      
      let sd = new SecurityDoor() ;
      let audi = new Car();
      sd.alert();
      audi.alert();
   ```
4. 一个类可以实现多个接口。
   ```typescript
      interface Light {
          lightOn(): void;
          lightOff(): void;
      }
      
      class Truck implements Alarm, Light {
          alert() {
              console.log('卡车发出警报！！！！！！！！！！！');
          }
      
          lightOff() {
              console.log('灯关闭了');
          }
      
          lightOn() {
              console.log('灯打开了');
          }
      }
      
      let trs = new Truck();
      trs.alert();
      trs.lightOff();
      trs.lightOn();
   ```
5. 接口与接口之间可以是继承的关系。
   ```typescript
      interface AlarmLight extends Alarm {
          alarmLightOn(): void;
          alramLightOff(): void;
      
      }
   ```
6. 类继承接口。在常见的面向对象的编程语言中，接口时不能继承类的，但是在TypeScript中，接口是可以继承类的。
   ```typescript
      class Point {
          x: number;
          y: number;
          constructor(x: number, y: number) {
              this.x = x;
              this.y = y;
          }
      }
      
      // 接口继承了Point类
      interface Point3D extends Point{
          z: number; 
      }
      
      // 使用接口Point3D来约束对象的形状
      let point3d: Point3D = {x: 1, y: 2, z: 3};
      // {x: 1, y: 2, z: 3} 
      console.log(point3d);
   ```
   - 我们创建的类，既可以当作一个类来使用（使用new关键字），也可以作为一个类型来使用（约束某个变量的类型）。
     ```typescript
        function getPoint(p: Point) {
            console.log('x = ', p.x, 'y = ', p.y);
        } 
        getPoint({x: 10, y: 55});
     ```
   - 我们在创建Point类时，同时也创建了一个名为 Point 的类型（实例的类型），用来约束实例的类型的。
   - 等价于下面的代码：新声明的 PointInstanceType 类型，与声明 class Point 时创建的 Point 类型是等价的。
     ```typescript
        interface PointInstanceType {
            x: number,
            y: number,
        }
        
        function printPoint(p: PointInstanceType) {
            console.log('x = ', p.x, 'y = ', p.y);
        }
        
        printPoint({x: 47, y: 99});
     ```
   - 当我们声明interface NewPoint3D extends Point3D时，实际上，NewPoint3D继承的时Point的实例类型。等价于`interface NewPoint3D extends PointInstanceType`
   - NewPoint3D继承的是接口PointInstanceType。所以「接口继承类」和「接口继承接口」没有什么本质的区别。
     ```typescript
        interface NewPoint3D extends Point {
            z: number,
        }
     ```
   - 声明 Point 类时创建的 Point 类型只包含其中的实例属性和实例方法。也就是说，PointInstanceType没有构造方法，静态属性，静态方法
   - 其实也很好理解，因为PointInstanceType时用来约束变量的，也就是实例，实例时没有静态属性和静态方法的。所以PointInstanceType没有构造方法，静态属性，静态方法，只有实例属性和实例方法。同理，接口在继承类的时候，也是只能继承实例属性和实例方法。
     ```typescript
        class NewPoint {
            // 静态属性，坐标原点
            static origin = new NewPoint(0, 0);
            // 实例属性
            x: number;
            y: number;
        
            /**
             * 定义一个静态方法，用来计算任意一点到坐标原点的距离
             * @param p
             */
            static getDistance(p: NewPoint): number {
                return Math.sqrt(p.x * p.x + p.y * p.y);
            }
            // 构造方法
            constructor(x: number, y: number) {
                this.x = x;
                this.y = y;
            }
        
            /**
             * 定义一个实例方法，用来打印x和y
             */
            printPoint() {
                console.log('x = ', this.x, 'y = ', this.y);
            }
        
        }
        
        interface NewPointInstanceType {
            x: number;
            y: number;
            printPoint(): void;
        }
        
        // 类型NewPoint和类型NewPointInstanceType时等价的
        let ps1: NewPoint;
        let ps2: NewPointInstanceType;
        ps1 = {
            x: 10,
            y: 101,
            printPoint() {
                console.log(`ps1 x = ${this.x}, ps1 y = ${this.y}`);
            }
        }
        
        ps2 = {
            x: 101,
            y: 202,
            printPoint() {
                console.log(`ps2 x = ${this.x}, ps2 y = ${this.y}`);
            }
        }
        
        console.log(ps1.x);
        console.log(ps1.y);
        ps1.printPoint();
        
        console.log(ps2.x);
        console.log(ps2.y);
        ps2.printPoint();
     ```
     
### 7. 泛型
1. 泛型的作用。泛型指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性。
2. 泛型的基本使用
   - 首先定义一个函数，创建一个指定长度并填充了指定值的数组，代码如下：
     ```typescript
        function createArray(length: number, value: any): Array<any> {
            let ret = [];
            for (let i = 0; i < length; i++) {
                ret.push(value);
            }
        
            return ret;
        }
        
        let ret = createArray(4, 'x');
        console.log(ret);
     ```
   - createArray()的返回值类型是之前提到过的数组泛型.数组项被约束为any类型，同时value也是any类型，显然，数组项应该是和value同一个类型但是使用any是做不到的，因此这里引入了泛型。下面的代码展示了使用泛型定义函数：
     ```typescript
        function createArrayWithValue<T>(length: number, value: T): Array<T> {
            let ret: T[] = [];
            for (let i = 0; i < length; i++) {
                ret.push(value);
            }    
            return ret;
        }
     ```  
   - 我们在函数名后添加了 <T>，其中 T 用来指代任意输入的类型，在后面的输入 value: T 和输出 Array<T> 中即可使用,表示都是同一种数据类型。  
   - 在使用的时候，我们在函数名的后面指定具体的数据类型：`<string>`，如下所示：
      ```typescript
         let ret2 = createArrayWithValue<string>(4, 'a');
         console.log(ret2);
      ```  
   - 也可以不用指定具体的数据类型，TypeScript会自己进行推断：
      ```typescript
         let ret3 = createArrayWithValue(10, 0);
         console.log(ret3);
      ```
3. 多个类型参数。定义泛型时，可以一次指定多个参数。
   ```typescript
      /**
       * 交换一个元组的两个元素
       * @param tuple
       */
      function swap<T, U>(tuple: [T, U]): [U, T] {
          return [tuple[1], tuple[0]];
      }
      
      console.log(swap(['name', 100]));
   ```
4. 泛型约束
   - 先看一个例子，如下所示：
     ```typescript
      // error TS2339: Property 'length' does not exist on type 'T'.
        function loggingIndentity<T>(arg: T): T {
            console.log(arg.length);
            return arg;
        }
     ```    
   - 函数内部使用泛型变量的时候，由于事先不知道它是哪种类型，所以不能随意的操作它的属性或方法，如上面的代码所示。由于不确定T中是不是存在length属性，所以编译阶段会报错。
   - 所以我们以对泛型进行约束，只允许这个函数传入那些包含 length 属性的变量。这就是泛型约束。
     ```typescript
        interface LengthWise {
            length: number
        }
     ```
   - 让泛型T继承LengthWise，进而对T进行了约束，T中必须有length属性。
     ```typescript
        function loggingIndentity<T extends LengthWise>(arg: T): T {
            console.log(arg.length);
            return arg;
        }     
        let id = loggingIndentity<Array<number>>([1, 2, 3]);
        console.log(id);    
        let sss = loggingIndentity<string>('hello world');
        console.log(sss);
        // 对象没有length属性，所以会报错
        // error TS2345: Argument of type '{ name: number; }' is not assignable to parameter of type 'LengthWise'.
        //   Object literal may only specify known properties, and 'name' does not exist in type 'LengthWise'.
        // let ooo = loggingIndentity({name: 123});
     ```
   - 多个类型参数之间也可以互相约束。
     ```typescript
        function copyFields<T extends U, U>(target: T, source: U): T {
            for (let id in source) {
                // U类型是父类，T类型是子类
                // (<T>source)是向下转型，保证和target的兼容
                target[id] = (<T>source)[id];
            }
            return target;
        }
        // 泛型T继承了U，U中具有的属性，T中一定具有，这样保证了U中不会出现T中不存在的字段
        let x = {a: 1, b: 2, c: 3, d: 4};
        let tar = copyFields(x, {b: 100, d: 250});
        console.log(tar);
     ```
5. 泛型接口。在接口上定义泛型，来约束函数的参数和返回值类型以及对象的形状。
   ```typescript
      interface CreateArrayFunc<T> {
          (length: number, value: T): Array<T>
      
      }
      // 此时在使用泛型接口的时候，需要定义泛型的类型
      let createArr: CreateArrayFunc<string>;
      createArr = function<T>(length: number, value: T): Array<T> {
          let ret: Array<T> = [];
          for (let i = 0; i < length; i++) {
              ret.push(value);
          }
      
          return ret;
      }
      
      let car = createArr(5, 'r');
      console.log(car);
   ```
   **注意：在使用泛型接口的时候，需要定义泛型的类型。**
6. 泛型类。在类上定义泛型。
   ```typescript
      class GenericsNumber<T> {
          zeroValue: T;
          // 对类的函数进行约束
          // 这里要注意，=>不是箭头函数的意思，而用来表示函数的定义，=>左边是输入类型，需要用括号括起来，右边是输出类型
          add: (x: T, y: T) => T;
      }
      
      let myGenerics = new GenericsNumber<number>();
      myGenerics.zeroValue = 0;
      myGenerics.add = (x, y) => x+ y;
      let addRet = myGenerics.add(10, 20);
      console.log(addRet);
      
      // 设置其他类型
      let mg = new GenericsNumber<string>();
      mg.zeroValue = 'zeros';
      mg.add = function (str1, str2) {
          return str1 + str2;
      }
      
      let addRet2 = mg.add('hello ', 'world');
      console.log(addRet2);
   ```  
   在类中如果使用泛型定义的函数，只需要定义输入参数类型和返回值类型，不用定义函数体。
7. 泛型的默认参数。在 TypeScript 2.3 以后，我们可以为泛型中的类型参数指定默认类型。当使用泛型时没有在代码中直接指定类型参数，从实际值参数中也无法推测出时，这个默认类型就会起作用。
   ```typescript
      // 给泛型指定默认类型，这里我们设置其默认类型为string
      function createArrWithVal<T = string>(length: number, value: T): Array<T> {
          let ret: T[] = [];
          for (let i = 0; i < length; i++) {
              ret.push(value);
          }
         
          return ret;
   }
   ```
8. 常见泛型变量的含义：
   - T（Type）：表示一个 TypeScript 类型。
   - K（Key）：表示对象中的键类型。
   - V（Value）：表示对象中的值类型。
   - E（Element）：表示元素类型。

### 8. 声明合并
1. 如果定义了两个或多个相同名字的函数、接口，那么它们会合并成一个类型。
2. 函数重载。
   ```typescript
       function reverse(input: number): number;
       function reverse(input: string): string;
       function reverse(input: number | string): string | number {
           if (typeof input === 'number') {
               return Number(input.toString().split('').reverse().join(''));
           } else if (typeof input === 'string') {
               return input.split('').reverse().join('');
           }
       }
       
       console.log(reverse(321));
       console.log(reverse('1234'));
   ```  
   精确定义写在前面，且只声明，不用写函数体。最后一个函数一定时包含前面所有类型，并定义了函数体。
3. 接口合并。如果我们定义了多个同名的接口，TypeScript会将他们进行合并。
   ```typescript
      interface Alarm {
          weight: number;
      }
      
      interface Alarm {
          price: number;
      
      }
   ```
   - 这两个接口会合并，等价于：
     ```typescript
        interface Alarm {
            weight: number;
            price: number;
        }
     ```
   - 接口合并的属性的类型必须是唯一的。如下所示：
     ```typescript
         // error TS2717: Subsequent property declarations must have the same type.  Property 'price' must be of type 'number', but here has type 'string
         interface Alarm {
              price: string;
              weight: number;
         }
     ```
   - 这里的price声明为string类型，在合并时就会报错，因为前面的price时number类型。
   - 接口中，函数的合并与函数重载相同。
     ```typescript
         interface Light {
              size: number;
              // 定义一个函数需要的形状（输入类型，返回值类型），形式如下所示
              alert(x: string): string;
               
         }
         interface Light {
               price: number;
               alert(x: number): number;
         }
     ```
   - 上面定义的两个接口合并后等价于：
     ```typescript
         interface Light {
            size: number;
            price: number;
            alert(x: number): number;
            alert(x: string): string;
        }
     ```
   - 使用：
     ```typescript
        let ll: Light = {
            size: 55,
            price: 100,
            alert: function<T>(x: T): T {
                return x;
            }
        }
        
        console.log(ll.alert('apple'));
        console.log(ll.alert(1235));
     ```  
     注意：我们在定义ll这个对象中的alert()这个函数时，由于alert()属于函数重载，所以ll中的alert()接收的参数和返回值类型一定要将前面定义的alert()的参数和返回值类型包括进去，所以这里使用了泛型。  
     ps：使用联合类型定义重载alert()会报错，不知道为什么。

      

### 9. 交叉类型

1. 交叉类型是将多个类型合并为一个类型。也就是求并集。

2. 使用`&`连接不同的类型。

3. 示例代码：
   ```ts
      /**
       *
       * 交叉类型
       * 将多个类型合并为一个类型
       */
      
      interface Person {
        name: string;
        age: number;
      }
      
      interface Career {
        location: string;
        dept: string;
        eID: number;
      }
      
      // 使用&连接两个不同的类型
      // p10就同时拥有Person和Career的属性
      let p10: Person & Career = {
        name: "jack",
        age: 25,
        location: "Beijing",
        dept: "销售部",
        eID: 145789,
      };
      
      // {
      //   name: 'jack',
      //   age: 25,
      //   location: 'Beijing',
      //   dept: '销售部',
      //   eID: 145789
      // }
      console.log(p10);
   ```
4. 交叉类型可以解决代码的复用问题。如果我们想同时使用两个类型，而这两个类型中的属性由很多重合的，比如说html元素，此时，我们就可以使用交叉类型，将这个两个类型合并为一个新的类型。

5. 更多关于交叉类型的内容：[TypeScript 交叉类型](http://semlinker.com/ts-intersection-types/)

### 10. 类型映射 —— Partial

1. 将一个接口中的所有属性变为可选属性。

2. 用法
   - 语法：  
     `type newOptionalType = Partial<Type>`
   - `Type`是我们需要转换的类型，返回值`newOptionalType`是一个新的类型，这个类型的属性与Type完全一样，只不过都是可选的（加了?）。
   
3. 这是一个工具类型，我们可以全局使用。

4. 示例代码：
   ```ts
      /**
       *
       * 类型转换 —— Partial
       * 将一个接口中的所有属性变为可选的
       */
      
      interface Apple {
        name: string;
        weight: number;
        color: string;
      }
      
      type OptionalApple = Partial<Apple>;
      
      // 经过Partial的转化，Apple中的属性现在都是可选的
      const a1: OptionalApple = {
        name: "红富士",
      };
   ```
