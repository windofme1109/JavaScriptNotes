<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [书写声明文件](#%E4%B9%A6%E5%86%99%E5%A3%B0%E6%98%8E%E6%96%87%E4%BB%B6)
  - [1. 参考文献](#1-%E5%8F%82%E8%80%83%E6%96%87%E7%8C%AE)
  - [2. 声明文件需要的新语法](#2-%E5%A3%B0%E6%98%8E%E6%96%87%E4%BB%B6%E9%9C%80%E8%A6%81%E7%9A%84%E6%96%B0%E8%AF%AD%E6%B3%95)
  - [3. 书写声明文件](#3-%E4%B9%A6%E5%86%99%E5%A3%B0%E6%98%8E%E6%96%87%E4%BB%B6)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 书写声明文件

## 1. 参考文献

1. [声明文件](https://ts.xcatliu.com/basics/declaration-files.html)

2. [如何编写 Typescript 声明文件](https://juejin.cn/post/6844903693226082318)

3. [TypeScript 声明文件的书写](https://juejin.cn/post/6844903887686598664)

4. [Typescript 书写声明文件（可能是最全的）](https://juejin.cn/post/6844904034621456398)

5. [TypeScript系列🔥尾声篇, 什么是声明文件(declare)? [🦕全局声明篇]](https://juejin.cn/post/6844903993727008776)

6. [官方文档 - 书写声明文件 - 深入](https://www.tslang.cn/docs/handbook/declaration-files/deep-dive.html)

7. [官方文档 - 书写声明文件 - 结构](https://www.tslang.cn/docs/handbook/declaration-files/library-structures.html)

8. [官方文档 - 书写声明文件 - 使用](https://www.tslang.cn/docs/handbook/declaration-files/consumption.html)

9. [官方文档 - 书写声明文件 - 发布](https://www.tslang.cn/docs/handbook/declaration-files/publishing.html)

10. [官方文档 - 书写声明文件 - 模板](https://www.tslang.cn/docs/handbook/declaration-files/templates.html)

## 2. 声明文件需要的新语法

1. 语法说明，如下表所示：

   语法|说明
   :---:|:---:
   `declare var` | 声明全局变量
   `declare let` | 声明全局变量
   `declare const` | 声明全局常量
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
 
 2. 全局变量都是禁止修改的常量，所以大部分情况都应该使用`const`而不是`var`或`let`。 
 
 3. **注意**：声明语句中只能定义类型，切勿在声明语句中定义具体的实现。
 
 4. `declare class`语句也只能用来定义类型，不能用来定义具体的实现。
 
## 3. 书写声明文件

1. 当我们使用的第三方库没有提供声明文件的时候，需要我们自己去写声明文件。声明文件的内容和使用方式在不同的应用场景下有所不同。主要有以下几种情况：
    - `全局变量`：通过` <script> `标签引入第三方库，注入全局变量
    - `npm 包`：通过 `import foo from 'foo'` 导入，符合 ES6 模块规范
    - `UMD 库`：既可以通过 `<script>` 标签引入，又可以通过 `import` 导入
    - `直接扩展全局变量`：通过 `<script>` 标签引入后，改变一个全局变量的结构
    - `在 npm 包或 UMD 库中扩展全局变量`：引用 npm 包或 UMD 库后，改变一个全局变量的结构
    - `模块插件`：通过 `<script>` 或 `import` 导入后，改变另一个模块的结构
  
### 1. 全局变量

1. 全局变量是最简单的一种场景，之前举的例子就是通过 `<script>` 标签引入 `jQuery`，注入全局变量 `$` 和 `jQuery`。

2. 使用全局变量的声明文件时，如果是以 `npm install @types/xxx --save-dev` 安装的，则不需要任何配置。如果是将声明文件直接存放于当前项目中，则建议和其他源码一起放到 src 目录下（或者对应的源码目录下）：
   ```
      /path/to/project
      ├── src
      |  ├── index.ts
      |  └── jQuery.d.ts
      └── tsconfig.json 
   ```
   如果没有生效，可以检查下 `tsconfig.json` 中的 `files`、`include` 和 `exclude` 配置，确保其包含了 `jQuery.d.ts` 文件。

3. 全局变量的声明文件主要有以下几种语法：
   - `declare var` 声明全局变量
   - `declare function` 声明全局方法
   - `declare class` 声明全局类
   - `declare enum` 声明全局枚举类型
   - `declare namespace` 声明（含有子属性的）全局对象
   - `interface` 和 `type` 声明全局类型


#### 1. `declare var`

1. 在所有的声明语句中，`declare var` 是最简单的，如之前所学，它能够用来定义一个全局变量的类型。与其类似的，还有 `declare let` 和` declare const`，使用 `let` 与使用 `var` 没有什么区别：
   ```js
      
      // src/jQuery.d.ts

      declare let jQuery: (selector: string) => any;

      // src/index.ts

      jQuery('#foo');
      // 使用 declare let 定义的 jQuery 类型，允许修改这个全局变量
      jQuery = function(selector) {
           return document.querySelector(selector);
      };   
   ```
2. 而当我们使用 `const` 定义时，表示此时的全局变量是一个常量，不允许再去修改它的值了：
   ```js
      // src/jQuery.d.ts

      declare const jQuery: (selector: string) => any;

      jQuery('#foo');
      // 使用 declare const 定义的 jQuery 类型，禁止修改这个全局变量
      jQuery = function(selector) {
          return document.querySelector(selector);
      };
      // ERROR: Cannot assign to 'jQuery' because it is a constant or a read-only property.
   ```
3. 一般来说，全局变量都是禁止修改的常量，所以大部分情况都应该使用 `const` 而不是 `var` 或 `let`。

4. 需要注意的是，声明语句中只能定义类型，切勿在声明语句中定义具体的实现5：
   ```js
      declare const jQuery = function(selector) {
          return document.querySelector(selector);
      };
      // ERROR: An implementation cannot be declared in ambient contexts. 
   ```


#### 2. declare function

1. `declare function` 用来定义全局函数的类型。`jQuery` 其实就是一个函数，所以也可以用 `function` 来定义：
   ```js
       // src/jQuery.d.ts

      declare function jQuery(selector: string): any;

      // src/index.ts

      jQuery('#foo');
   ```


2. 在函数类型的声明语句中，函数重载也是支持的6：
   ```js
      // src/jQuery.d.ts

      declare function jQuery(selector: string): any;
      declare function jQuery(domReadyCallback: () => any): any;

      // src/index.ts

      jQuery('#foo');
      jQuery(function() {
          alert('Dom Ready!');
      });
   
   ```

#### 3. declare class

1. 当全局变量是一个类的时候，我们用 declare class 来定义它的类型：
   ```js
      // src/Animal.d.ts

      declare class Animal {
          name: string;
          constructor(name: string);
          sayHi(): string;
      }

      // src/index.ts

      let cat = new Animal('Tom');
   ```


2. 同样的，`declare class` 语句也只能用来定义类型，不能用来定义具体的实现，比如定义 `sayHi` 方法的具体实现则会报错：
   ```js
      // src/Animal.d.ts

     declare class Animal {
         name: string;
         constructor(name: string);
         sayHi() {
             return `My name is ${this.name}`;
         };
         // ERROR: An implementation cannot be declared in ambient contexts.
     }    
   ```


#### 4. declare enum

1. 使用 `declare enum` 定义的枚举类型也称作外部枚举（Ambient Enums），举例如下：
   ```js
      // src/Directions.d.ts

     declare enum Directions {
         Up,
         Down,
         Left,
         Right
     }

     // src/index.ts

     let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
   ```


2. 与其他全局变量的类型声明一致，`declare enum` 仅用来定义类型，而不是具体的值。

3. `Directions.d.ts` 仅仅会用于编译时的检查，声明文件里的内容在编译结果中会被删除。它编译结果是：
    ```js
       var directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
    ```
    其中 Directions 是由第三方库定义好的全局变量。


#### 5. declare namespace§

1. `namespace` 是 ts 早期时为了解决模块化而创造的关键字，中文称为命名空间。

2. 由于历史遗留原因，在早期还没有 ES6 的时候，ts 提供了一种模块化方案，使用 `module` 关键字表示内部模块。但由于后来 ES6 也使用了` module` 关键字，ts 为了兼容 ES6，使用 `namespace` 替代了自己的 `module`，更名为命名空间。

3. 随着 ES6 的广泛应用，现在已经不建议再使用 ts 中的 `namespace`，而推荐使用 ES6 的模块化方案了，故我们不再需要学习 `namespace` 的使用了。

4. `namespace` 被淘汰了，但是在声明文件中，`declare namespace` 还是比较常用的，它用来表示全局变量是一个对象，包含很多子属性。比如 `jQuery` 是一个全局变量，它是一个对象，提供了一个 `jQuery.ajax` 方法可以调用，那么我们就应该使用 `declare namespace jQuery` 来声明这个拥有多个子属性的全局变量。
   ```js
      // src/jQuery.d.ts

      declare namespace jQuery {
          function ajax(url: string, settings?: any): void;
      }

      // src/index.ts

      jQuery.ajax('/api/get_something'); 
   ```

5. 注意，在 `declare namespace` 内部，我们直接使用 `function ajax` 来声明函数，而不是使用 `declare function ajax`。类似的，也可以使用 `const`, `class`, `enum` 等语句：
   ```js
      // src/jQuery.d.ts

      declare namespace jQuery {
          function ajax(url: string, settings?: any): void;
          const version: number;
          class Event {
              blur(eventType: EventType): void
          }
          enum EventType {
              CustomClick
          }
      }

      // src/index.ts

      jQuery.ajax('/api/get_something');
      console.log(jQuery.version);
      const e = new jQuery.Event();
      e.blur(jQuery.EventType.CustomClick); 
   ```

### 6. 嵌套的命名空间

1. 如果对象拥有深层的层级，则需要用嵌套的 `namespace` 来声明深层的属性的类型：
   ```js
      // src/jQuery.d.ts

      declare namespace jQuery {
          function ajax(url: string, settings?: any): void;
          namespace fn {
              function extend(object: any): void;
          }
      }

      // src/index.ts

      jQuery.ajax('/api/get_something');
      jQuery.fn.extend({
          check: function() {
              return this.each(function() {
                  this.checked = true;
              });
          }
      }); 
   ```


2. 假如 `jQuery` 下仅有 `fn` 这一个属性（没有 `ajax` 等其他属性或方法），则可以不需要嵌套` namespace`：
   ```js
      // src/jQuery.d.ts

     declare namespace jQuery.fn {
         function extend(object: any): void;
     }

     // src/index.ts

     jQuery.fn.extend({
         check: function() {
             return this.each(function() {
                 this.checked = true;
             });
         }
     });  
   ```


#### 7. interface 和 type

1. 除了全局变量之外，可能有一些类型我们也希望能暴露出来。在类型声明文件中，我们可以直接使用 interface 或 type 来声明一个全局的接口或类型：
   ```js
      // src/jQuery.d.ts

      interface AjaxSettings {
          method?: 'GET' | 'POST'
          data?: any;
      }
      declare namespace jQuery {
          function ajax(url: string, settings?: AjaxSettings): void;
      }  
   ```
   这样的话，在其他文件中也可以使用这个接口或类型了：
   ```js
      // src/index.ts

     let settings: AjaxSettings = {
         method: 'POST',
         data: {
             name: 'foo'
         }
     };
     jQuery.ajax('/api/post_something', settings);
   ```


`type` 与 `interface` 类似，不再赘述。

#### 8. 防止命名冲突

1. 暴露在最外层的 `interface` 或 `type` 会作为全局类型作用于整个项目中，我们应该尽可能的减少全局变量或全局类型的数量。故最好将他们放到` namespace` 下：
   ```js
      // src/jQuery.d.ts

      declare namespace jQuery {
          interface AjaxSettings {
              method?: 'GET' | 'POST'
              data?: any;
          }
          function ajax(url: string, settings?: AjaxSettings): void;
      }

   ``` 
   注意，在使用这个 `interface` 的时候，也应该加上 `jQuery` 前缀：
   ```js
      // src/index.ts

      let settings: jQuery.AjaxSettings = {
          method: 'POST',
          data: {
              name: 'foo'
          }
      };
      jQuery.ajax('/api/post_something', settings); 
   ```


#### 9. 声明合并

1. 假如 jQuery 既是一个函数，可以直接被调用 jQuery('#foo')，又是一个对象，拥有子属性 jQuery.ajax()（事实确实如此），那么我们可以组合多个声明语句，它们会不冲突的合并起来：
   ```js
      // src/jQuery.d.ts

      declare function jQuery(selector: string): any;
      declare namespace jQuery {
          function ajax(url: string, settings?: any): void;
      }

      // src/index.ts

      jQuery('#foo');
      jQuery.ajax('/api/get_something'); 
   ```

### 2. npm 包

1. 一般我们通过 `import foo from 'foo'` 导入一个 `npm` 包，这是符合 `ES6` 模块规范的。

2. 在我们尝试给一个 npm 包创建声明文件之前，需要先看看它的声明文件是否已经存在。一般来说，npm 包的声明文件可能存在于两个地方：
   - 与该 npm 包绑定在一起。判断依据是 package.json 中有 types 字段，或者有一个 index.d.ts 声明文件。这种模式不需要额外安装其他包，是最为推荐的，所以以后我们自己创建 npm 包的时候，最好也将声明文件与 npm 包绑定在一起。
   - 发布到 `@types` 里。我们只需要尝试安装一下对应的 `@types` 包就知道是否存在该声明文件，安装命令是 `npm install @types/foo --save-dev`。这种模式一般是由于 npm 包的维护者没有提供声明文件，所以只能由其他人将声明文件发布到 @types 里了。

3. 假如以上两种方式都没有找到对应的声明文件，那么我们就需要自己为它写声明文件了。由于是通过 import 语句导入的模块，所以声明文件存放的位置也有所约束，一般有两种方案：
   - 创建一个 `node_modules/@types/foo/index.d.ts` 文件，存放 foo 模块的声明文件。这种方式不需要额外的配置，但是 `node_modules` 目录不稳定，代码也没有被保存到仓库中，无法回溯版本，有不小心被删除的风险，故不太建议用这种方案，一般只用作临时测试。
   - 创建一个 `types` 目录，专门用来管理自己写的声明文件，将 `foo` 的声明文件放到 `types/foo/index.d.ts` 中。这种方式需要配置下 `tsconfig.json` 中的 `paths` 和 `baseUrl` 字段。

4. 目录结构：
   ```
      /path/to/project
      ├── src
      |  └── index.ts
      ├── types
      |  └── foo
      |     └── index.d.ts
      └── tsconfig.json 
   ```
   tsconfig.json 内容：
   ```json
      {
          "compilerOptions": {
              "module": "commonjs",
              "baseUrl": "./",
              "paths": {
                  "*": ["types/*"]
              }
          }
      }
   ```
5. 如此配置之后，通过 import 导入 foo 的时候，也会去 types 目录下寻找对应的模块的声明文件了。

6. 注意 module 配置可以有很多种选项，不同的选项会影响模块的导入导出模式。这里我们使用了 commonjs 这个最常用的选项，后面的教程也都默认使用的这个选项。

npm 包的声明文件主要有以下几种语法：

    export 导出变量
    export namespace 导出（含有子属性的）对象
    export default ES6 默认导出
    export = commonjs 导出模块

export§

npm 包的声明文件与全局变量的声明文件有很大区别。在 npm 包的声明文件中，使用 declare 不再会声明一个全局变量，而只会在当前文件中声明一个局部变量。只有在声明文件中使用 export 导出，然后在使用方 import 导入后，才会应用到这些类型声明。

export 的语法与普通的 ts 中的语法类似，区别仅在于声明文件中禁止定义具体的实现15：

// types/foo/index.d.ts

export const name: string;
export function getName(): string;
export class Animal {
    constructor(name: string);
    sayHi(): string;
}
export enum Directions {
    Up,
    Down,
    Left,
    Right
}
export interface Options {
    data: any;
}

对应的导入和使用模块应该是这样：

// src/index.ts

import { name, getName, Animal, Directions, Options } from 'foo';

console.log(name);
let myName = getName();
let cat = new Animal('Tom');
let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
let options: Options = {
    data: {
        name: 'foo'
    }
};

混用 declare 和 export§

我们也可以使用 declare 先声明多个变量，最后再用 export 一次性导出。上例的声明文件可以等价的改写为16：

// types/foo/index.d.ts

declare const name: string;
declare function getName(): string;
declare class Animal {
    constructor(name: string);
    sayHi(): string;
}
declare enum Directions {
    Up,
    Down,
    Left,
    Right
}
interface Options {
    data: any;
}

export { name, getName, Animal, Directions, Options };

注意，与全局变量的声明文件类似，interface 前是不需要 declare 的。
export namespace§

与 declare namespace 类似，export namespace 用来导出一个拥有子属性的对象17：

// types/foo/index.d.ts

export namespace foo {
    const name: string;
    namespace bar {
        function baz(): string;
    }
}

// src/index.ts

import { foo } from 'foo';

console.log(foo.name);
foo.bar.baz();

export default§

在 ES6 模块系统中，使用 export default 可以导出一个默认值，使用方可以用 import foo from 'foo' 而不是 import { foo } from 'foo' 来导入这个默认值。

在类型声明文件中，export default 用来导出默认值的类型18：

// types/foo/index.d.ts

export default function foo(): string;

// src/index.ts

import foo from 'foo';

foo();

注意，只有 function、class 和 interface 可以直接默认导出，其他的变量需要先定义出来，再默认导出19：

// types/foo/index.d.ts

export default enum Directions {
// ERROR: Expression expected.
    Up,
    Down,
    Left,
    Right
}

上例中 export default enum 是错误的语法，需要使用 declare enum 定义出来，然后使用 export default 导出：

// types/foo/index.d.ts

declare enum Directions {
    Up,
    Down,
    Left,
    Right
}

export default Directions;

针对这种默认导出，我们一般会将导出语句放在整个声明文件的最前面20：

// types/foo/index.d.ts

export default Directions;

declare enum Directions {
    Up,
    Down,
    Left,
    Right
}

export =§

在 commonjs 规范中，我们用以下方式来导出一个模块：

// 整体导出
module.exports = foo;
// 单个导出
exports.bar = bar;

在 ts 中，针对这种模块导出，有多种方式可以导入，第一种方式是 const ... = require：

// 整体导入
const foo = require('foo');
// 单个导入
const bar = require('foo').bar;

第二种方式是 import ... from，注意针对整体导出，需要使用 import * as 来导入：

// 整体导入
import * as foo from 'foo';
// 单个导入
import { bar } from 'foo';

第三种方式是 import ... require，这也是 ts 官方推荐的方式：

// 整体导入
import foo = require('foo');
// 单个导入
import bar = foo.bar;

对于这种使用 commonjs 规范的库，假如要为它写类型声明文件的话，就需要使用到 export = 这种语法了21：

// types/foo/index.d.ts

export = foo;

declare function foo(): string;
declare namespace foo {
    const bar: number;
}

需要注意的是，上例中使用了 export = 之后，就不能再单个导出 export { bar } 了。所以我们通过声明合并，使用 declare namespace foo 来将 bar 合并到 foo 里。

准确地讲，export = 不仅可以用在声明文件中，也可以用在普通的 ts 文件中。实际上，import ... require 和 export = 都是 ts 为了兼容 AMD 规范和 commonjs 规范而创立的新语法，由于并不常用也不推荐使用，所以这里就不详细介绍了，感兴趣的可以看官方文档。

由于很多第三方库是 commonjs 规范的，所以声明文件也就不得不用到 export = 这种语法了。但是还是需要再强调下，相比与 export =，我们更推荐使用 ES6 标准的 export default 和 export。
UMD 库§

既可以通过 <script> 标签引入，又可以通过 import 导入的库，称为 UMD 库。相比于 npm 包的类型声明文件，我们需要额外声明一个全局变量，为了实现这种方式，ts 提供了一个新语法 export as namespace。
export as namespace§

一般使用 export as namespace 时，都是先有了 npm 包的声明文件，再基于它添加一条 export as namespace 语句，即可将声明好的一个变量声明为全局变量，举例如下22：

// types/foo/index.d.ts

export as namespace foo;
export = foo;

declare function foo(): string;
declare namespace foo {
    const bar: number;
}

当然它也可以与 export default 一起使用：

// types/foo/index.d.ts

export as namespace foo;
export default foo;

declare function foo(): string;
declare namespace foo {
    const bar: number;
}
### 3. UMD 库

### 4.直接扩展全局变量

### 5. 在 npm 包或 UMD 库中扩展全局变量
### 5. 模块插件


2. 符合 ES6 规范的声明文件
   - 在一个文件中声明一个模块，模块的名称使用字符串名称
     ```typescript
        // jquery.d.ts
        declare module 'jquery' {
            function $(selector: string): JqueryInstance;
            function $(readyFunc: () => void): void;
            namespace $ {
                namespace fn {
                    // 只能声明函数，不能定义实现
                    class init {
                        sayHi(): void;
                    }
                }
            }
        
            // 既可以在定义变量时直接导出，也可以最后统一导出
            export const onClick = 'CLICK';
        
            // 最后导出使用export
            // 导出多个变量，将多个变量用{}包起来，使用export导出
            // export { $, onClick };
            // 导出一个变量，直接赋值给export
            // export = $;
            // 或者使用export default
            export default $;
        }
     ```
   - 与 ES6 模块化的语法类似。








。
npm 包§





直接扩展全局变量§

有的第三方库扩展了一个全局变量，可是此全局变量的类型却没有相应的更新过来，就会导致 ts 编译错误，此时就需要扩展全局变量的类型。比如扩展 String 类型23：

interface String {
    prependHello(): string;
}

'foo'.prependHello();

通过声明合并，使用 interface String 即可给 String 添加属性或方法。

也可以使用 declare namespace 给已有的命名空间添加类型声明24：

// types/jquery-plugin/index.d.ts

declare namespace JQuery {
    interface CustomOptions {
        bar: string;
    }
}

interface JQueryStatic {
    foo(options: JQuery.CustomOptions): string;
}

// src/index.ts

jQuery.foo({
    bar: ''
});

在 npm 包或 UMD 库中扩展全局变量§

如之前所说，对于一个 npm 包或者 UMD 库的声明文件，只有 export 导出的类型声明才能被导入。所以对于 npm 包或 UMD 库，如果导入此库之后会扩展全局变量，则需要使用另一种语法在声明文件中扩展全局变量的类型，那就是 declare global。
declare global§

使用 declare global 可以在 npm 包或者 UMD 库的声明文件中扩展全局变量的类型25：

// types/foo/index.d.ts

declare global {
    interface String {
        prependHello(): string;
    }
}

export {};

// src/index.ts

'bar'.prependHello();

注意即使此声明文件不需要导出任何东西，仍然需要导出一个空对象，用来告诉编译器这是一个模块的声明文件，而不是一个全局变量的声明文件。
模块插件§

有时通过 import 导入一个模块插件，可以改变另一个原有模块的结构。此时如果原有模块已经有了类型声明文件，而插件模块没有类型声明文件，就会导致类型不完整，缺少插件部分的类型。ts 提供了一个语法 declare module，它可以用来扩展原有模块的类型。
declare module§

如果是需要扩展原有模块的话，需要在类型声明文件中先引用原有模块，再使用 declare module 扩展原有模块26：

// types/moment-plugin/index.d.ts

import * as moment from 'moment';

declare module 'moment' {
    export function foo(): moment.CalendarKey;
}

// src/index.ts

import * as moment from 'moment';
import 'moment-plugin';

moment.foo();

declare module 也可用于在一个文件中一次性声明多个模块的类型27：

// types/foo-bar.d.ts

declare module 'foo' {
    export interface Foo {
        foo: string;
    }
}

declare module 'bar' {
    export function bar(): string;
}

// src/index.ts

import { Foo } from 'foo';
import * as bar from 'bar';

let f: Foo;
bar.bar();

