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
