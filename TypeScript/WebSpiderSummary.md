<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [使用TypeScript完成爬虫项目的的总结](#%E4%BD%BF%E7%94%A8typescript%E5%AE%8C%E6%88%90%E7%88%AC%E8%99%AB%E9%A1%B9%E7%9B%AE%E7%9A%84%E7%9A%84%E6%80%BB%E7%BB%93)
  - [1. 初始化项目](#1-%E5%88%9D%E5%A7%8B%E5%8C%96%E9%A1%B9%E7%9B%AE)
  - [2. 使用express重构项目](#2-%E4%BD%BF%E7%94%A8express%E9%87%8D%E6%9E%84%E9%A1%B9%E7%9B%AE)
  - [3. 装饰器（Decorator）在项目中的应用](#3-%E8%A3%85%E9%A5%B0%E5%99%A8decorator%E5%9C%A8%E9%A1%B9%E7%9B%AE%E4%B8%AD%E7%9A%84%E5%BA%94%E7%94%A8)
  - [4. 在 React 中使用 TypeScript](#4-%E5%9C%A8-react-%E4%B8%AD%E4%BD%BF%E7%94%A8-typescript)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 使用TypeScript完成爬虫项目的的总结

## 1. 初始化项目

1. 生成 package.json 文件  
   在命令行输入：`npm init -y`，即可生成 package.json 文件
   
2. 生成 tsconfig.json 文件  
   在命令行输入：`tsc --init`，即可生成 tsconfig.json 文件。这一步非常重要，这个文件是有关于 TS 配置的一些内容，生成这个文件，涉及到导入一些模块的时候，TS 才会给出提示。比如说提示缺少声明文件等。

3. 局部安装 TypeScript 和 ts-node  
   局部安装的优势是，项目依赖的所有模块都在 package.json 中。当我们把项目发布后，使用者直接执行 `npm install` 就可以安装所有依赖。项目也能正常运行。而如果是全局安装，那么就一些依赖就不会放在 package.json 中。项目发布后，项目会因为缺少某些依赖而报错。
   
4. 修改 package.json 中的 script 字段  
   在这个字段中加入：`"dev": "ts-node src/crawler.ts"`，当我们运行 `npm run dev` 命令的时候，使用 ts-node 这个命令去执行 src 目录下的 crawler.ts 这个文件。

5. 安装模块的声明文件  
   - 所谓的声明文件，指的是以 `.d.ts` 为结尾的文件。这个文件中包含着这个包或者模块的函数和变量声明。当我们使用包中的函数或者变量时，TS会根据这个文件给出类型的提示，使得我们书写代码非常方便。
   - 不是所有的第三方模块都会自带声明文件。如果没有自带声明文件，那么需要我们手动下载声明文件，也有可能这个模块根本没有声明文件。在TypeScript 2.0 以上的版本，获取类型声明文件只需要使用 npm。类型声明包的名字总是与它们在 npm 上的包的名字相同，但是有@types/前缀， 例如获取cheerio的声明文件：`npm install --save @types/cheerio`。
   - 查找某个包的声明文件的网站：https://microsoft.github.io/TypeSearch/

6. ts 文件编译为 js 文件
   - 我们使用 `ts-node` 这个命令编译并执行了 ts 文件，期间并没有生成 js 文件。但实际上，我们构建项目，需要的是 js 文件。因此我们需要使用 `tsc` 命令，将 ts 文件编译为 js 文件。
   - 如果我们每一次修改 ts 文件后，执行 `tsc` 命令，生成 js 文件，这样很麻烦。所以我们需要配置 `tsc` 命令。
   - 在 package.json 文件中的 `scripts` 中添加：`"build": "tsc -w"`，`tsc` 表示编译 ts 文件，而 `-w` 表示 watch，也就是说，一旦 ts 文件发生变化，自动执行 tsc 命令，帮助我们编译成 js 文件。

7. 修改 tsconfig.json 文件
   - 使用 tsc 命令编辑后的 js 文件通常会和 ts 文件在同一个目录下，在实际的项目开发中，这是不合理的，我们最好将编译后的 js 文件放在一个单独的目录下，因此需要修改 tsconfig.json 文件。
   - 在 `"compilerOptions"` 字段下，将 `"outDir"` 的值设置为：`./build`，表示编译后的js文件放在与 tsconfig 同级的 build 目录下。

8. 自动执行
   - 生成接收文件后，我们需要执行 js 文件。同样我们也需要js文件更新后，就能立即执行。因此我们使用 `nodemon` 这个工具。 在 package.json 文件中的 `scripts` 中添加：`"start": "nodemon node ./build/crawler.js"`。表示，我们使用 nodemon 执行 build 目录下的 crawler.js 文件。
   - 由于 nodemon 监测的是整个项目，但是有一些文件不需要它去监测，比如说我们存储结果的 course.json 文件，我们一旦执行 crawler.js 文件，就会更新 course.json文件，而 course.json 文件一旦更新，又会触发 nodemon 执行 crawler.js 文件，此时 course.json 又会被更新，于是就处于无限循环之中。实际上，我们监测的主要是 js 文件。所以需要配置 nodemon，使其忽略对 course.json 文件的支持。
   - 在 package.json 文件中的 `scripts` 中添加：`"nodemonConfig": {"ignore": ["data/*"]},`，表示忽略data目录下所有文件。

9. 并行执行 `tsc -w` 和 `nodemon`
   - 执行 `tsc -w` 和 `nodemon`，必须开启两个终端，有没有同时执行（并行）这两个命令的方法呢？
   - 答案是有的，安装一个工具：concurrently，安装命令：`npm install concurrently -D`。在 package.json 文件中的 `scripts` 中添加：`"dev": "concurrently \"npm run dev:build\" \"npm run dev:start\""`。
   - concurrently 的文档地址：https://www.npmjs.com/package/concurrently
   - 使用格式：
     1. 命令行：`concurrently "command1 arg" "command2 arg"`
     2. package.json: `"start": "concurrently \"command1 arg\" \"command2 arg\""`

## 2. 使用express重构项目

1. 使用 express 搭建一个服务器。所以要新建一个 index.ts 文件，这个文件是项目的入口文件，所以，我们要在 package.json 中去修改配置项：
   ```json
      {
          "scripts": {
             "dev:build": "tsc -w",
             "dev:start": "nodemon  build/index.js",
             "dev": "tsc && concurrently \"npm run dev:build\" \"npm run dev:start\""
          }
      }
   ```
   命令解释：
   - `"dev:build": "tsc -w"`
   - `"dev:start": "nodemon  build/index.js"`  
     这个命令，表示执行 build 目录下的 index.js 文件。这个文件是我们项目的入口文件，使用 nodemon 去执行。
   - ` "dev": "tsc && concurrently \"npm run dev:build\" \"npm run dev:start\""`  
     为什么要在 concurrently 的前面加上 tsc 命令，因为如果我们直接并行的执行 dev:build 和 dev:start 这两个命令，由于是同时执行，TS 还没有将 index.ts 文件编译完成，nodemon 就会去执行 index.js 文件，此时还没有生成 index.js 文件，所以会提示找不到 index.js 文件这个错误。  
     因此我们要在并行执行那两个命令之前，先对 index.ts 文件进行编译。  
     使用&&连接两个命令，表示先执行 tsc，再执行 concurrently。

2. express 的类型文件 `.d.ts` 文件对类型的描述不准确，很多都是any，无法精确限制类型
   - 解决方法：定义一个接口，使之继承某一个类型，比如说是 Request，然后在这个接口中定义我们需要限定的类型，比如说是 body。
   - 使用时，直接使用这个接口即可。
   - 示例代码：
     ```typescript
        interface BodyRequest extends Request {
             body: {
                // 任意属性
                // 属性为字符串即可
                [prop: string]: string | undefined;
             };
        }
     ```
   - BodyRequest 这个接口，继承了 Reuqest，不仅具有 Request类型的所有内容，同时我们还可以根据自己的需求，增加或者是覆写一下东西，然后使用这个新的 BodyRequest 对变量进行类型限定。
   
3. 当我们使用中间件时，对 req 或者 res 对象做出了修改，但是类型定义并没有改变
   - 解决方法是，我们可以自定义一个 `.d.ts` 的类型文件，按照官方的声明格式，在里面添加我们需要限定的类型。TS 会将相同的全局变量进行合并。例如：我们在项目的根路径下建立一个类型文件：`custom.d.ts`，内容如下：
     ```typescript
        /**
          * 自定义的类型文件，用来扩展Express原本的类型定义
          * 定义方式要与Express的类型定义文件相同
          */
        declare namespace Express {
            interface Request {
                     teacherName: string;
            }
        }
     ```
   - 定义方式与 Express 的官方类型定义文件相同，这样 TS 就会将其合并。这被称为类型融合，我们要找到 express 的核心的类型定义文件：`@types/express-serve-static-core/index.d.ts`，进入这个文件，**仿照这个文件的声明方式进行声明**。

## 3. 装饰器（Decorator）在项目中的应用

1. 装饰器主要用在后端的路由处理中。将一些公共的处理流程，比如：检查是否登录、指定请求方法等，这些功能都可以抽出来，放到装饰器中去做。
2. 想要用好装饰器，必须和反射机制结合。在 TypeScript 中，要使用 reflect-metadata 这个模块实现反射机制。
3. 基本流程就是通过反射机制，获取定义在某个类、方法上的装饰器以及元数据（使用 defineMetadata() 函数定义），根据元数据和装饰器，进行相应的操作。
4. 在项目中使用的装饰器主要是类装饰器和方法装饰器。
5. 对后端进行拆分，按照 MVC 的模式组织代码。统一将前端请求放在 Controller 层中进行处理。我们只需要搞定路由处理函数即可。我们以登录接口为例来说明如何使用装饰器实现 Controller。
   - 安装 `reflect-metadata` 模块：`npm install reflect-metadata --save`
   - 新建两个目录：`controller` 和 `decorator`。其中 `decorator` 目录用来存放装饰器函数。
   - 在 decorator 目录下，新建 decorator.ts 文件，这个文件用来定义装饰器，并且给相应的方法添加元数据。
     ```typescript
        import 'reflect-metadata';
        import { RequestHandler } from 'express';
        
        /**
         * 这个函数的主要作用是将 path 和 method 作为元数据添加到某个成员方法中
         * @param method
         */
        function requestDecorator(method: string) {
            return function (path: string) {
                // 真正的装饰器
                /**
                 * 修饰的是普通方法，target 是类的原型对象  prototype，修饰的是静态方法，则 target 是类的 constructor
                 */
                return function (target: any, key: string) {
                    Reflect.defineMetadata('path', path, target, key);
                    Reflect.defineMetadata('method', method, target, key);
                };
            };
        }
        
        /**
         * 添加多个中间件
         * @param middleware
         */
        export function use(middleware: RequestHandler) {
            return function (target: any, key: string) {
                // 要添加多个中间件，所有 metadataValue 必须是数组的形式
     // 判断中间件数组是否存在，不存在，说明是第一次赋值，则赋值为空数组，否则就直接获取中间数组
                const mws: Array<any> = Reflect.getMetadata('middlewares', target, key)
                    ? Reflect.getMetadata('middlewares', target, key)
                    : [];
        
                // 将新的中间件推入数组中
                mws.push(middleware);
                Reflect.defineMetadata('middlewares', mws, target, key);
                
            };
        }
        export const get = requestDecorator('get');
        export const post = requestDecorator('post');
     ```
     因此，我们可以给类的成员方法添加装饰器。使用如下：
     ```typescript
        import { post, get, use } from '../decorator/request-backup';
        @get('/isLogin')
        isLogin(req: BodyRequest, res: Response) {}
        
        @post('/login')
        login(req: BodyRequest, res: Response) {}
     
         
        @get('/getData')
        @use(test)
        @use(print)
        getData(req: BodyRequest, res: Response): void {}
     ```
     test 和 print 是我们自定义的中间件。
   - 在 decorator 目录下，新建 controller.ts 文件，这个文件实现一个类装饰器，作用有：
     - 获取定义在类中所有成员方式上的元数据和装饰器。
     - 生成路由，包括路径、方法和处理逻辑以及对中间件的处理。
     ```typescript
        import 'reflect-metadata';
        import { Router } from 'express';
        
        enum RequestMethods {
            get = 'get',
            post = 'post',
        }
       
        const router = Router();
        
        /**
         * root 参数，是父路径，而 Controller 内的路由处理函数的装饰器，接收的参数是子路径，二者拼接形成完整的请求路径
         * @param root
         * @constructor
         */
        export function ControllerBackup(root: string) {
            return function (target: new (...args: any[]) => any) {
                // 成员方法都定义在类的原型上
                const targetPrototype = target.prototype;
                for (let key in targetPrototype) {
                    const path = Reflect.getMetadata('path', targetPrototype, key);
                    const finalPath = root === '/' ? path : `${root}${path}`;
                    // 获取定义在某个成员方式上的方法的元数据
                    const method: RequestMethods = Reflect.getMetadata(
                        'method',
                        targetPrototype,
                        key
                    );
                    // 获取定义在某个成员方式上的中间件元数据
                    const middlewares = Reflect.getMetadata(
                        'middlewares',
                        targetPrototype,
                        key
                    );
                    const handle = targetPrototype[key];
                    if (finalPath && method) {
                        if (middlewares && middlewares.length) {
                            // 保证中间件数组一定存在
                            router[method](finalPath, ...middlewares, handle);
                        } else {
                            router[method](finalPath, handle);
                        }
                    }
                }
            };
        }
        
        export default router;
     ```
     这个装饰器是给类使用的，同时还可以接收一个根路径参数，使用如下：
     ```typescript
        import { ControllerBackup } from '../decorator/controller-backup';
        @ControllerBackup('/api')
        export class LoginController {}
     ```
   
## 4. 在 React 中使用 TypeScript

1. 安装
   - 在控制台输入：`npx create-react-app react-project-typescript --template typescript --use-npm`
   - `--template typescript` 表示使用 typescript 作为项目的模板。
   - `--use-npm` 表示使用 npm 安装项目的依赖。不加入这个参数，则默认使用 yarn 的方式安装项目的依赖。
   
2. 书写组件的文件的后缀是 `.tsx`。

3. 函数组件的限定类型是 `React.FC`，如下所示：
   ```tsx
        import React, { FC } from 'react';
   
        export default const App: React.FC = () => {
            return (
                <div className="main">
                    <HashRouter>
                        <Switch>
                            <Route path="/login" component={Login} />
                            <Route path="/" component={Home} />
                        </Switch>
                    </HashRouter>
                </div>
            );
        };
   ```

4. 函数组件结合 hooks 可以实现类组件的效果。
5. 在类组件中，想要限定类型，可以使用泛型。
   ```tsx
      import { FormProps } from 'antd/lib/form/Form';
      import React, {Component} from 'react' ;
      interface Props {
          form: FormProps;
      }
      
      export class Test extends Component<Props> {
          render() {
              let from = this.props.form;
              return <div></div>;
          }
      }
   ```
   通过泛型的形式，明确了 Test 组件接收的 props 参数的类型，也就是说，props 里面会有表单这个参数，这样 TS 既能给出提示，又能进行检查。

6. 学会去找模块提供的类型定义。有很多第三方的模块，如 antd，express 等，有现成的声明文件，里面具有很多类型。因此，为了提高代码的健壮性和可读性，减少程序出错的机率，我们需要进入模块的声明文件中寻找我们需要的类型。一般而言，我们首先寻找后缀是`.d.ts`的声明文件，在这些声明文件中去寻找。

7. 模块书写的小技巧
   - 我们定义了一个 decorator 的模块，目录结构如下：
     ```tsx
        |-- controller
            |-- controller.ts
        |-- request
            |-- request.ts
        |-- index.ts 
        |-- method.ts
     ```
   - 其中，controller.ts 和 request.ts 向外导出若干变量和函数。其他模块如果要使用一个变量，可能就得这样：`import Controller from '../../decorator/controller''`，就要定位到具体文件。如果文件的层级比较深，那么这种引入的方式非常麻烦。
   - 为了解决这个问题，我们可以设置一个入口文件，也就是在 decorator 这个模块的根目录下，建立 `index.ts` 文件，使用 `export from` 语法，将这个模块需要对外导出的变量和函数，统一从index.ts中导出。代码如下：
     ```typescript
        export * from './controller/controller'
        export * from './request/request'
     ```
   - `export-from` 用于聚集模块。但不能在直接使用。例如在index.js 里 `export { a } from 'b.js'`, 变量 a 在index.js 里无法使用。
   - 其他模块引入了 decorator 模块的内容时，会首先进入 index.ts 文件中进行查找。我们在 index.ts 中聚集了 decorator 模块中所有的对外暴露的模块和变量，因此TS能轻易找到我们需要的内容。
   - 实际上，我们观察 node_modules 中的模块，入口文件都是index.js，这个模块需要导出的内容，都在 index.js 文件中。这样我们在使用的时候，多数情况下，直接是：`import from '模块名';`，而不用定位到具体的目录下去引入。