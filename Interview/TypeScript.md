# TypeScript Interview

## 1. 什么是 TypeScript

1. typescript 是 JavaScript 的超集，在 JavaScript 的基础上，添加了静态类型检查。

2. typescript 支持 ES6 以及更新的语法，支持面向对象编程的概念，如类、接口、继承、泛型等。Typescript 并不直接在浏览器上运行，需要编译器编译成纯 JavaScript 来运行。

## 2. 优势和缺点

### 1. 优点

1. 快速简单，易于学习。

2. 编译时提供错误检查，在代码运行前就会进行错误提示。

3. 支持所有的 JS 库。

4. 支持 ES6，提供了 ES6 所有优点和更高的生产力。

5. 使用继承提供可重用性。

6. 有助于代码结构。

7. 通过定义模块来定义命名空间。

### 2. 缺点

1. 需要长时间的来编译代码。

2. 在使用第三方库时，需要有三方库的定义文件，并不是所有三方库都提供了定义文件，提供的定义文件是否准确也值得商榷。



## 3. TypeScript 中的数据类型 

### 1. 基本数据类型

1. number

2. string

3. boolean

4. null

5. undefined

6. never
   - never 是指没法正常结束返回的类型，一个必定会报错或者死循环的函数会返回这样的类型。
     ```ts
        // throw error 返回值是never
        function foo(): never { throw new Error('error message') }
        // 这个死循环的也会无法正常退出  
        function foo(): never { while(true){} }  
        // Error: 这个无法将返回值定义为never，因为无法在静态编译阶段直接识别出
        function foo(): never { let count = 1; while(count){ count ++; } }  
     ```
    - never 有如下特性
      - 在一个函数中调用了返回 never 的函数后，之后的代码都会变成 deadcode
        ```ts
           function test() {
               // 这里的 foo 指上面返回 never 的函数
               foo();
               // Error: 编译器报错，此行代码永远不会执行到 		
               console.log(111); 	
           } 
        ```
      - 无法把其他类型赋给 never：
        ```ts
           let n: never;
           let o: any = {};
           // Error: 不能把一个非 never 类型赋值给never类型，包括 any
           n = o;  
        ```
7. any
   - any 表示任意值，用来表示允许赋值为任意类型。
   - 一个变量被定义为 any，允许变量在运行过程中，类型的变化。
   - 也可以调用任意的属性和方法。

### 2. 用户自定义数据类型

1. tuple —— 元组
   1. 元组合并了不同类型的对象，而数组（Array）则是用来合并同种类型的对象
   2. 注意与 python 中的元组的概念进行区分，在 python 中，元组与列表类似，都是线性表，主要区别是元内容不可变。
   3. 元组有以下几个特点：
      - 内容可变，但是类型不可变。
      - 初始化可以不用赋值。赋值的时候，数量和对应的数据类型必须对应正确。
      - 可以使用 push() 和 pop() 方法。

2. enum —— 枚举
   1. 作用：枚举类型用于取值被限定在一定范围内的场景，比如一周只能有七天，颜色限定为红绿蓝等。
   2. 枚举使用 enum 关键字定义。
   3. 枚举成员会被赋值为从 0 开始递增的数字，同时也会对枚举值到枚举名进行反向映射。

## 4. interface —— 接口

1. 接口的作用：
   - 接口是对行为的抽象，而具体如何行动需要由类（class）去实现（implements）。
   - 在TypeScript中，接口常常用来对对象的形状（shape）进行约束。

2. 定义对象、数组、函数、类等。

3. 接口可以相互继承。

4. 接口可以继承类。

5. 可选属性与额外检查。

6. 使用 `?` 定义可选属性。

7. 使用 `readonly` 定义只读属性。


## 5. generic —— 泛型

1. 泛型代表的是泛指某一类型，更像是一个类型变量。由尖括号包裹 `<T>`。

2. 主要作用是创建逻辑可复用的组件，即事先不知道我们具体使用什么类型约束，此时就可以使用泛型，等到具体使用的时候再替换为具体的类型。

3. 泛型可以作用在函数、类、接口上。

4. 示例：
   ```ts
        // 函数
        function greet<T>(name: T) {}

        // 类
        class createObj<T> {
            name: T
        }

        // 接口
        interface IF<T> {
            name: T
        }
   ```

## 6. class - 类
1. ES6 中的类。传统的 JavaScript 存在类的概念，我们通过构造函数来模拟一个类，并使用原型继承的方式实现继承。  

2. 在 ES6 中，引入和 class 关键字和 extends 关键字。  

3. class 用于定义类，extends 实现继承。

4. 在 TypeScript 中，增加了三个关键字：private、protected 和 public。
   - private 修饰的属性或方法是私有的，不能在声明它的类的外部访问。
   - protected 修饰的属性或方法是受保护的，它和 private 类似，区别是它在子类中也是允许被访问的。
   - public 修饰的属性或方法是公有的，可以在任何地方被访问到，默认所有的属性和方法都是 public 的。

5. TypeScript 支持抽象类。抽象类是供其他类继承的基类，抽象类不允许被实例化。抽象类中的抽象方法必须在子类中被实现。在 TypeScript 中，使用 abstract 进行定义。
   - 抽象类不能被实例化。  
   - 抽象类中定义的方法必须在子类中被实现。  
   - 抽象类中的方法也要用 abstract 定义。

6. 一个类只能有一个父类，但是可以实现个接口，即单继承，多实现。 

## 7. 声明文件

1. 当我们使用的第三方库没有提供声明文件的时候，需要我们自己去写声明文件。

2. 声明文件以 `.d.ts` 结尾。

3. 使用 Declare 关键字来书写声明文件。可以声明该模块，甚至可以直接声明一个值为 any 的同名的变量，然后我们就可以在代码中直接使用该三方库了。

4. 我们可以直接安装别人写好的第三方声明文件：`npm install @types/xxx`

## 8. tsconfig.json 的作用

1. 用于标识 TypeScript 项目的根路径。
2. 用于配置 TypeScript 编译器。
3. 用于指定编译的文件。
4. 重要字段
   - files - 设置要编译的文件的名称。
   - include - 设置需要进行编译的文件，支持路径模式匹配。
   - exclude - 设置无需进行编译的文件，支持路径模式匹配。
   - compilerOptions - 设置与编译流程相关的选项。常见的有 baseUrl、 target、baseUrl、 moduleResolution 和 lib 等。
  

## 9. 声明合并

1. 声明合并是编译器将2个或多个同名声明合并为一个，合并后的声明拥有被合并声明的所有特性。

2. 目前除了类不能与其他类和变量合并外，其他声明都是可以相互合并的。

3. 函数、接口都可以进行类型合并。

## 10. 交叉类型 —— &


1. 交叉类型是将多个类型合并为一个类型。也就是求并集。

2. 使用 `&` 连接不同的类型。
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
3. 交叉类型可以解决代码的复用问题。如果我们想同时使用两个类型，而这两个类型中的属性由很多重合的，比如说 html 元素，此时，我们就可以使用交叉类型，将这个两个类型合并为一个新的类型。

## 11. 类型映射 —— Partial

1. 将一个接口中的所有属性变为可选属性。

2. 用法
   - 语法：  
     `type newOptionalType = Partial<Type>`
   - `Type`是我们需要转换的类型，返回值`newOptionalType`是一个新的类型，这个类型的属性与Type完全一样，只不过都是可选的（加了 `?`）。
   
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
## 12. 属性移除 —— Omit

1. 语法：`Omit<Type, Keys>`

2. 接收一个类型（type），然后选择忽略这个类型中的一个属性（Keys），返回忽略这个属性的类型（type）。

3. 这是一个工具类型，我们可以全局使用。

4. 示例：
   ```ts
      interface Todo {
          title: string;
          description: string;
          completed: boolean;
      }
      // 从 Todo 中移除了 description 属性
      // 得到了一个不包含 description 的新类型
      type TodoPreview = Omit<Todo, "description">;
      // 
      const todo: TodoPreview = {
          title: "Clean room",
          completed: false,
      }; 

## 13. 类型断言

1. 类型断言对运行没有什么影响，仅供编译器使用。

2. 向编译器提供我们所希望的分析代码的提示。

3. 表示断言的两种方式：
   - <类型>变量
   - 变量 as 类型 （在 tsx 中只能使用这种方式）
