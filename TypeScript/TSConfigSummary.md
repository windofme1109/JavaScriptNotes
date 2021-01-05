<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [tsconfig.json 详解](#tsconfigjson-%E8%AF%A6%E8%A7%A3)
  - [1. tsconfig.json 文件基本说明](#1-tsconfigjson-%E6%96%87%E4%BB%B6%E5%9F%BA%E6%9C%AC%E8%AF%B4%E6%98%8E)
  - [2. tsconfig.json字段详解](#2-tsconfigjson%E5%AD%97%E6%AE%B5%E8%AF%A6%E8%A7%A3)
    - [1. 参考：](#1-%E5%8F%82%E8%80%83)
    - [2. include](#2-include)
    - [3. exclude](#3-exclude)
    - [4. files](#4-files)
    - [5. compilerOptions](#5-compileroptions)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# tsconfig.json 详解

## 1. tsconfig.json 文件基本说明

1. 如果我们直接执行 `tsc demo.ts` 命令，会在同级目录下生成 demo.js 文件。在这种情况下，并不会直接读取 tsconfig.json 里面的配置项。

2. 我们如果只执行 `tsc` 命令，首先会去读取 tsconfig.json 里面的配置项，如果存在 `include` 字段，则会去编译这个字段指定的文件，否则会默认编译当前目录下所有的 ts 文件。同时还会读取其他的配置项，如 `compilerOptions`，就是配置编译过程的一些选项。

## 2. tsconfig.json字段详解

### 1. 参考：
   - [tsconfig.json 中文版](https://www.tslang.cn/docs/handbook/tsconfig-json.html)
   - [tsconfig.json 英文版](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)
   - [编译选项](https://www.tslang.cn/docs/handbook/compiler-options.html)
   
### 2. include  

1. 要编译哪些 ts 文件。字段值为数组，数组元素为 ts 文件路径。

### 3. exclude  

1. 不需要编译哪些 ts 文件，字段值为数组，数组元素为 ts 文件路径。
   
### 4. files 
 
1. 与 includes 作用相同。
   
### 5. compilerOptions 
 
1. 这个字段下有非常多的属性，我们这里选择几个常用的比较重要的字段进行说明。

2. module   
   用来指定要使用的模块标准，主要有：'none'、'commonjs'、 'amd'、'system'、'umd'、'es2015'、'es2020' 或者是 'ESNext'。
   
3. removeComments  
   属性值为布尔值。为 `true` 的话，表示编译过程中，将移除所有的注释。`false` 的话，保留所有的注释。 
   
4. noImplicitAny  
   - 有的时候，我们不会显式声明一个变量为 any 类型，而TS无法推断这个变量的类型的时候，就会将其回退为 any 类型，但是有可能会导致一些错误，比如前后数据类型不一致，此时 TS 无法检查出来。
   - noImplicitAny 的属性值为布尔值。作用就是解决隐式的 any 类型问题。为 `true` 的话，表示一旦我们没有显式声明一个变量为 any 类型，而且 TS 推断其为 any 类型，就会给出一个错误。为 `false` 的话，表示不会给出错误提示。

5. strictNullChecks  
   - 强制检查 null 类型  
   - 属性值为布尔值。为 `true` 的话，表示将对元素的赋值情况进行检查，不允许将 null 和 undefined 赋值给基本数据类型，或者元素未赋值，也会报错。为 `false` 的话，表示不对元素的赋值情况进行检查，并允许将 null 赋值给基本数据类型。

6. rootDir  
   这个属性用来控制要编译的ts文件的路径，属性值是路径字符串。当我们执行 `tsc` 命令时，TS就会去编译这个路径下的 ts 文件。

7. outDir  
    这个属性用来控制编译后的ts文件的存放位置。属性值是路径字符串。TS将文件编译为 js 文件，并将 js 文件存放在 outDir 指定的路径下。

8. incremental  
   - 渐进式编译。属性值为布尔值。为 true 的时候，TS 只会编译新增的内容，为 false 时，就全部编译。
   - 设置这个属性为 true 的时候，会在根目录生成 `tsconfig.tsbuildinfo` 文件，里面存放了编译的文件的信息，当TS再次执行编译时，会和这个文件的内容进行比对，相同的内容就不再编译，而是只编译最新的内容。

9. allowJs  
   - 允许 js 文件被编译。默认情况下，TS只编译 ts 文件。如果我们也想将js文件编译，比如说 ES6 编译为 ES5，所以我们就可以使用这个属性来进行控制。
   - 为 true 的时候，执行 tsc 命令的时候，同时编译 js 文件和 ts 文件，为 false 的时候，只编译 ts 文件。

10. checkJs  
    是否对 js 文件进行检查。属性值为布尔值。为 true 的时候，检查js文件，为 false 的时候，不检查 js 文件。

11. noUnusedLocals  
    是否检查有未使用的局部变量。为 true 的话就检查，为 false 的话就不检查。如果存在未使用的变量，就提示，但不会报错。

12. noUnusedParameters  
    是否检查有未使用的函数参数。为 true 的话就检查，为 false 的话就不检查。如果存在未使用的参数，就提示，但不会报错。

13. outFile  
    - 配置这个选项后，会将所有的编译后的 ts 文件放入一个 js 文件中。这样就省去我们引入多个文件的麻烦。  
    - 属性值是文件路径，例如：`./dist/page.js`，就会将所有的编译后的ts文件的内容都放入 dist 目录下的 page.js 文件中。

14. declaration
    - 是否生成 `.d.ts` 类型声明文件，默认为 `false`，不生成类型声明文件，设置为 `true`，生成类型声明文件。

15. moduleResolution
    - 决定如何处理模块。值为 `Node` 或者是 `Classic`（默认）。`Node` 对应的是 Node.js 的模块解析机制。而 `Classic` 是 TypeScript 的默认的解析策略。现在，它存在的理由主要是为了向后兼容，如果 module 字段设置的值为 `AMD`、`System`、 `ES2015` 三者之一，则 moduleResolution 默认为 `Classic`。详细说明请见：[模块解析
](https://www.tslang.cn/docs/handbook/module-resolution.html)

16. allowSyntheticDefaultImports
    - 允许从没有设置默认导出的模块中默认导入。这并不影响代码的输出，仅为了类型检查。
     - 为布尔值。当 module 设置为 `system` 或者 `esModuleInterop` 且 module 不为 `es2015` 和 `esnext` 两者之一时，默认为 false，其余情况设置为 `true` 才起作用。

17. jsx
    - 在 `.tsx` 中启用对jsx语法的支持，值有三个，说明如下表所示：
     
     模式|输入|输出|输出文件扩展名
     |:---:|:---:|:---:|:---:|
     preserve|`<div />`|`<div />`|`.jsx`
     react|`<div />`|`React.createElement("div")`|`.js`
     react-native|`<div />`|`<div />`|`.js`
     
     - `preserve` 表示不会对jsx语法进行编译，保持所有的 jsx，并编译为jsx文件。而 `react` 表示调用 React.createElement() 方法创建 HTML 元素，生成 js 文件。`react-native` 相当于preserve，它也保留了所有的JSX，但是输出文件的扩展名是.js。
     - 详情请参考：[JSX](https://www.tslang.cn/docs/handbook/jsx.html)

18. target 
    - 指定ECMAScript目标版本 `ES3`（默认）， 可选的值有：`ES5`， `"ES6"/ "ES2015"`， `ES2016`， `ES2017`或 `ESNext`。
   