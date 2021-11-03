# Mobx 使用教程

## 1. 参考资料

1. [Mobx 官方文档](https://mobx.js.org/README.html)

2. [mobx-react 官方文档](https://github.com/mobxjs/mobx-react)

3. [【MobX】MobX 简单入门教程](https://zhuanlan.zhihu.com/p/88374561?from_voters_page=true)

4. [mobx基本概念](https://www.cnblogs.com/mengff/p/9501481.html)

5. [mobx系列(一)-mobx简介](https://blog.csdn.net/smk108/article/details/84777649)

6. [mobx系列(二)-mobx主要概念](https://blog.csdn.net/smk108/article/details/84960159)

7. [mobx系列(三)-在React中使用Mobx](https://blog.csdn.net/smk108/article/details/85053903)

8. [mobx系列(五)-Mobx常见问题及解决方案（1）Mobx使用严格模式](https://blog.csdn.net/smk108/article/details/83185745)

9. [mobx系列(五)-Mobx常见问题及解决方案（2）observable state更新后组件不主动更新问题](https://blog.csdn.net/smk108/article/details/89681367)

10. [[翻译]十分钟介绍Mobx](https://zhuanlan.zhihu.com/p/131604839)

11. [mobx和mobx-react 入门](https://juejin.cn/post/6847902207585566727)

12. [redux 和 mobX对比](https://juejin.cn/post/6844903550846238728)

13. [MobX5&6 + React使用指南](https://juejin.cn/post/6978852090881769486)

14. [MobX入门](https://juejin.cn/post/6844903539475480590)

15. [【MobX】MobX 简单入门教程](https://juejin.cn/post/6844903977553756168)

16. [Mobx基本使用](https://juejin.cn/post/6973548253027500039)

17. [使用 Mobx + Hooks 管理 React 应用状态](https://juejin.cn/post/6844904096441303053)

18. [Hooks 邂逅 Mobx，代码变得更丝滑了](https://juejin.cn/post/6930758273863778317)

19. [react-mobx6+使用案例](https://juejin.cn/post/6978378931489472548)

20. [mobx、mobx-react和mobx-react-lite新手入门](https://juejin.cn/post/6945720333026459656)

21. [MobX 上手指南](https://juejin.cn/post/6921544116094533646)

22. [Mobx React 最佳实践](https://juejin.cn/post/6844903538280103950)

23. [mobx 的原理以及在 React 中应用](https://juejin.cn/post/6979127131104083976#heading-1)

## 2. 基本概念

1. Mobx 是一个用于状态管理的库，它通过透明的函数响应式编程（transparently applying functional reactive programming - TFRP）使得状态管理变得简单和可扩展。MobX背后的哲学很简单:

2. Mobx 的设计哲学是：任何源自应用状态的东西都应该自动地获得。
   > Anything that can be derived from the application state, should be. Automatically.

3. Mobx 的特点如下：
   1. 直观（Straightforward）  
      编写最简单、无样板的代码，捕捉您的意图。正在尝试更新记录字段？使用好的旧 JavaScript 模式进行赋值。在异步进程中更新数据？无需特殊工具，Mobx 这套反应系统将检测您的所有更改，并将其传播到正在使用更改的位置。
      > Write minimalistic, boilerplate free code that captures your intent. Trying to update a record field? Use the good old JavaScript assignment. Updating data in an asynchronous process? No special tools are required, the reactivity system will detect all your changes and propagate them out to where they are being used.
   2. 易于对渲染进行优化（Effortless optimal rendering）   
      只在运行时跟踪数据的所有更改和使用，从而构建一个依赖关系树来捕获状态和输出之间的所有关系。这保证了依赖于你的应用的状态的计算（如React组件）仅在严格需要时运行。无需使用易出错和次优技术（如记忆和选择器）手动优化组件。
      > All changes to and uses of your data are tracked at runtime, building a dependency tree that captures all relations between state and output. This guarantees that computations depending on your state, like React components, run only when strictly needed. There is no need to manually optimize components with error-prone and sub-optimal techniques like memoization and selectors.
   3. 结构自由（Architectural freedom）  
      MobX并非是专用的，允许您在任何 UI 框架之外管理应用程序状态。这使您的代码解耦、可移植，最重要的是易于测试。
      > MobX is unopinionated and allows you to manage your application state outside of any UI framework. This makes your code decoupled, portable, and above all, easily testable.

4. Mobx 包含一些响应式编程（Reactive Programming）的思想。响应式编程简介如下： 
   - 就是一种面向数据流和变化传播的编程范式。只要我们的数据发生变化，那么计算模型会感知到这种变化，同时会自动地将变化地值通过数据流进行传播。
   - 例如对表达式：`a = b + c`的处理，在命令式编程中，会先计算 b + c 的结果，再把此结果赋值给变量 a，因此 b，c 两值的变化不会对变量 a 产生影响。但在响应式编程中，变量 a 的值会随时跟随 b，c 的变化而变化。
   - 电子表格程序就是响应式编程的一个例子。单元格可以包含字面值或类似 `=B1+C1` 的公式，而包含公式的单元格的值会依据其他单元格的值的变化而变化。
   - 有关响应式编程的资料：
     - [Rxjs 响应式编程-第一章：响应式](https://segmentfault.com/a/1190000015966781)
     - [响应式编程](https://blog.csdn.net/zuokaopuqingnian/article/details/89921863)
     - [谈谈响应式编程](https://www.imooc.com/article/76216)
     - [响应式编程到底是什么？](https://zhuanlan.zhihu.com/p/258411008)
     - [(1) 什么是响应式编程——响应式Spring的道法术器](https://blog.csdn.net/get_set/article/details/79455258)

5. Mobx 的概念图如下所示：
   ![](./img/mobx-flow.png)

### 1. State

### 1. Observable State

1. MobX 为现有的数据结构（如数字，字符串，对象，数组和类实例）添加了可观察（observable）的功能。 通过使用 `@observable` 装饰器（ES.Next）来给你的类属性添加注解就可以简单地完成这一切。

2. 什么是可观察呢？Mobx 采用的是观察者模式，其中观察者是使用数据的组件，而被观察的对象是数据，只要数据发生变化，就能立刻通知观察者，观察者收到这种变化以后采取一系列的行动。

3. 使用 `@observable` 对数据进行包装，使其变得可观察。如下所示：
   ```js
      import {observable} from 'mobx';
      class Todo {
          id = Math.random();
          @observable title = "";
          @observable finished = false;
      }
   ```
4. **注意**：任何需要变化的属性（特别是和 ui 渲染相关的属性）都应该使用 `@observable` 进行装饰，这样 Mobx 才能追踪这些数据的变化。

5. `@observable` 提供了一个容器，我们将值装入这个容器中。我们不直接对这个值进行修改，而是使用函数修改这个值，比如像说分发一个 action 修改，使用 @computed 进行修改，等等。这个就是函数式编程中的函子（functor）的思想。

### 2. Computed Value

1. 使用 MobX， 我们可以定义在相关数据发生变化时自动更新的值。 通过 `@computed` 装饰器进行更新。
   ```js
      class TodoList {
          @observable todos = [];
          @computed get unfinishedTodoCount() {
              return this.todos.filter(todo => !todo.finished).length;
          }
      }
   ```

2. 当添加了一个新的 todo 或者某个 todo 的 finished 属性发生变化时，MobX 会确保 unfinishedTodoCount 自动更新。

3. 每当只有在需要计算属性的值的时候，它们才会自动更新。即使用的是惰性求值的方式。也就是说，只有在组件中引用这个计算属性，计算属性才会去执行计算，得到最新的值，然后更新组件。

4. 计算属性实际上是类的 getter 方法，我们可以通过类的实例直接调用这个属性。

### 2. Reaction

1. Reaction 和计算属性很像，但它不是产生一个新的值，而是会产生一些副作用，比如打印到控制台、网络请求、递增地更新 React 组件树以修补 DOM 等。简而言之，Reactions 在响应式编程和命令式编程之间建立沟通的桥梁。

2. 响应式编程中，数据的变化会被自动感知，但是这种数据的变化需要对外输出，比如说修改视图、发起网络请求等，我们需要一种方法去实现这个对外输出，在 Mobx 中，这个就是 Reaction。

3. 到目前为止，最常用的 Reaction 的形式是 UI 组件。注意，Action 和 Reaction 都可能引发副作用。应该从一个清晰、明确的来源中触发副作用，例如在提交表单时发出网络请求，应该从相关事件处理程序显式触发。

4. Mobx 中的几种 Reaction：
   1. autorun: 本质是一个高阶函数，接收一个函数作为参数，autorun 如其名，首次观察状态立即被触发一次，执行接收的函数，状态变化会再次被触发。
   2. autorunAsync: 可以在状态发生改变一定时间后触发，有函数防抖的功能，其他与autorun一致。
   3. when: 可以设置断言，当断言生效时候函数被触发，并且仅仅触发一次。
   4. reaction: 与autorun类似，函数不会立即执行。

### 3. Action

1.Mobx 中的 Action 是一个代码块，用来修改 state。这个和 Redux 中的 Action 不同：Redux 中的 Action 是描述如何修改 state，真正的修改是在 Reducer 中完成的。

2. 在 Mobx 中，用户事件、收到响应数据、定时任务等，都会改变 state，这种改变通过 Action 完成。Action 就像我们向 Excel 中的单元格中输入一个新的值。

3. Mobx 推荐的做法是：修改可观察的数据只能通过 Action 完成。这样 Mobx 可以自动的应用事务（批量更新），轻松实现性能优化。

4. 使用 Action 有助于我们构建代码，并防止在无意中更改状态。在 MobX 术语中，修改状态的方法称为 Action。与视图不同：视图是根据当前状态计算新的信息。每种方法（Action）最多只能达到这两个目标中的一个：要么修改状态，要么修改 state。

### 4. Derivation

1. Derivation 的意思是衍生，即任何能从 state 衍生出来，且没有任何进一步的交互的东西都是 Derivation。

2. Derivation 以许多种形式存在：
   1. 用户接口
   2. 衍生的数据，如剩下的 todos 的数量（由 todos 数组衍生出来的）
   3. 后端集成，如向服务器发送更改服务器内容的数据（post 请求）

3. Mobx 区分下面两种类型的 Derivation：
   1. Computed values（计算值）- 它们是永远可以使用纯函数（pure function）从当前可观察状态中衍生出的值。
 
   2. Reactions（反应）- Reactions 指的是，当状态改变时需要自动触发的副作用。需要有一个桥梁来连接命令式编程（imperative programming）和响应式编程（reactive programming）。

4. 我们刚开始使用 MobX 时，可能倾向于频繁的使用 reactions。 因为我们前面说过，Reaction 和计算属性很像，但它不是产生一个新的值，而是产生副作用。因此，我们应该记住这个**黄金法则**：如果我们想创建一个基于当前状态的值时，请使用 computed。

5. 还是以 excel 表格为例，公式是计算值的衍生。但对于用户来说，能看到屏幕给出的反应则需要部分重绘 GUI。

6. 对于用户而言，能看在屏幕上看到到 state 或者是计算属性（computed value）的变化，需要 reaction 对部分页面进行重绘。

### 5. 原则

1. MobX使用单向数据流，其中操作更改状态，进而更新所有受影响的视图。数据流如下图所示：
   ![](./img/action-state-view.png)

2. 当 state 改变时，所有 Derivation 都会进行原子级的自动更新。因此永远不可能观察到中间值。

3. 所有 Derivation 默认都是同步更新。这意味着例如 Action 可以在改变 state 之后直接可以安全地检查计算值。

4. 计算值（computed value）是延迟更新的。任何不在使用状态的计算值将不会更新，直到需要它进行副作用（I/O）操作时。 如果视图不再使用，那么它会自动被垃圾回收。

5. 所有的计算值（computed value）都应该是纯净的。它们不应该用来改变 state。

## 3. Mobx-react 简介

1. Mobx-react 用来组合 React 组件和 Mobx。类似于 redux-react。

2. Mobx-react 的官方文档：[mobx-react](https://github.com/mobxjs/mobx-react)

3. 提供一个 `obsrever` 装饰器和其他的工具，使用 `obsrever` 装饰器将 React 组件包装为一个观察者，这样就能与 Mobx 中可观察的数据建立联系，当可观察的数据发生变化，观察者组件能收到这种变化，从而引起组件的更新。

4. Mobx-react 版本的选择如下表所示：
   
   NPM 版本|支持的 MobX 版本|支持的 React 版本|是否支持 hooks
   :---:|:---:|:---:|:---:
   v7|6.*|16.8+|Yes
   v6|4._ / 5._ |16.8+|Yes
   v5|4._ / 5._ |0.13|No, but it is possible to use <Observer> sections inside hook based components

5. Mobx-react 的 v6 / v7 版本是对轻量级 mobx-react-lite 的重新包装，并加上了 Mobx-react v5 的下列特性：
   1. `observer` 和 `@observer` 支持类组件
   2. 向 `Provider` 组件或者 `inject` 函数传入 `stores` （使用 `React.createContext` 进行替代）
   3. `PropTypes` 用来描述可观察的数据（考虑使用 `TypeScript` 来代替）
   4. `DisposeUnmount` 函数/装饰器可以轻松清理资源，例如在基于类的组件中创建的 `reaction`。

6. 安装 Mobx-react：`npm install mobx-react --save`

## 4. Mobx-react api 简介

### 1. observer 

1. observer 函数（装饰器）将一个React 组件定义、类组件或者函数组件转换为响应式组件（reactive component）。

2. 转换后的组件将会追踪在副作用函数 render 中使用的可观察的数据，当可观察的数据变化的时候，能自动重新渲染组件。

#### 1. 函数组件

1. 对于函数组件而言，observer 接收的函数组件会自动的被 React.memo 包装。observer 不接收已经被 React.memo 包装的函数式组件。

#### 2. 类组件

1. 使用类组件时，组件内部的 props 和 state 会变成可观察的，因此这个组件会对 render 函数中使用的 props 和 state 的变化做出响应。

2. shouldComponentUpdate 不再被支持。推荐使用 React.PureComponent 扩展类组件。如果有必要，observer 会使用内置的 `React.PureComponent` 对不纯的组件进行打补丁。

#### 3. 用法

1. observer 可以作为函数使用，也可以作为装饰器使用。

2. 对于类组件，既可以使用 `@observer` 对类组件进行装饰，也是将类组件作为参数传入 observer 函数中。如下所示：
   ```js
      import { observer } from "mobx-react"

      // ---- ES6 syntax ----
      const TodoView = observer(
            class TodoView extends React.Component {
               render() {
                   return <div>{this.props.todo.title}</div>
               }
            }
      )

      // ---- ESNext syntax with decorator syntax enabled ----
      @observer
      class TodoView extends React.Component {
           render() {
               return <div>{this.props.todo.title}</div>
           }
      }
   ```
3. 对于函数组件，只能将其作为参数传入 observer 函数中。
   ```js
      // ---- or just use function components: ----
       const TodoView = observer(({ todo }) => <div>{todo.title}</div>)
   ```
4. 什么类型的组件应该使用 `observer` 装饰？
   - 一句话总结：使可观察的数据进行渲染的组件。
   
### 2. Provider 和 inject

1.Provider 是一个组件，接收 store （或者其他内容）作为 props 参数。使用 React 提供的 context 机制，将 store 传入子组件中。可以跨层级使用 store 属性。

2. inject 是一个装饰器，也可以作为函数使用。inject 用来获取 store 对象。

3. inject 实际上是一个高阶组件，将其装饰的组件进行包装。接收一系列的字符串作为参数，使得和字符串同名的 store 属性在被包裹的组件中可用。

4. Provider 接收 stores 属性，我们如果想在子组件中使用，需要在 inject 中传入同名的字符串。比如说，传入 Provider 的 stores 对象名称是 user，即：`<Provider user={stores}>...</Provider>`，那么 inject 应该接收 user 这个字符串，即：`@inject('user')`。这样被 inject 装饰的组件通过 `this.props.user` 就能获取 user 这个 store 中的内容了。

5. Provider 和 inject 的使用示例：
   ```js
      @inject("color")
      @observer
      class Button extends React.Component {
          render() {
              return <button style={{ background: this.props.color }}>{this.props.children}</button>
          }
      }

       class Message extends React.Component {
           render() {
               return (
                  <div>
                    {this.props.text} <Button>Delete</Button>
               </div>
               )
           }
       }

       class MessageList extends React.Component {
           render() {
               const children = this.props.messages.map(message => <Message text={message.text} />)
                return (
                    <Provider color="red">
                        <div>{children}</div>
                    </Provider>
               )
           }
       }
   ```
6. 使用 React.useContext 可以读取传入 Provider 的 stores 对象，我们只需要从 Mobx-react 中导入 MobXProviderContext 这个上下文即可。

7. 如果一个组件需要一个 store，并且通过同名的属性接收一个 store。那么会优先使用这个 store 属性。测试的时候可以利用这一点。

8. 同时使用 `@inject` 和 `@observer`，确保使用顺序：`observer` 应该是内层装饰器，`inject` 是外层装饰器，二者之间可以有别的装饰器。
   ```js
      @inject()
      // ... other decorators
      @observer
      class Comp extends React.Component {}
   ```
9. 被 inject 包装的组件，可以通过 inject 返回的高阶组件的 `wrappedComponent` 属性获得。

10. inject 可以作为函数使用，如下所示：
    ```js
       var Button = inject("color")(
           observer(
               class Button extends Component {
                   /* ... etc ... */
               }
           )
       )
    ```
    也可以接收函数组件：
    ```js
       var Button = inject("color")(
           observer(({ color }) => {
              /* ... etc ... */
           })
       )
    ```
11. 也就是说，inject 和 observer，要么同时为装饰器，要么同时为函数。

12. observer 作为一个函数，接收一个组件，并返回一个组件。将 observer 的返回值传入 inject 函数的返回值中（inject 函数的返回值也是一个高阶组件）。
## 4. 基本使用（v5 版本的 Mobx）—— 与 Mobx-react 结合


## 5. api 参考 —— 以 v5 版本的 Mobx 为例

### 1. 创建 observable

#### 1. observable

1. observable 可以作为函数使用，也可以作为装饰器使用。

2. 作用是：复制一个函数，将其变成可观察的。复制源可以是纯对象、数组、Map 和 Set。默认情况下，observable 是递归使用，即 如果遇到的值是一个对象或者数组，其内部的值也会被传入 observable 中。

3. 基本数据类型（string、number、boolean、null、undefined）不能被 Mobx 转换成可观察的对象。因为 JavaScript 中的基本数据类型是不可变的。但是可以变成包装类型。

4. 无论是是使用装饰器`@observable`，还是将类实例传入 observable 函数中，类实例都不能被自动地转换成可观察的对象。将类的属性变成可观察的类型是类的构造函数的职责。

5. 作为函数：`observable(source, overrides?, options?)`  

6. 参数说明：
   - source
     - 必要参数
     - 需要被克隆的对象，该对象的所以成员会变成可观察的
   - overrides
     - 可选
     - 是一个 map 结构，用来指定哪些成员会被注解（annotation），我的理解是：哪些成员变成可观察的。
   - options
     - 可选
     - 配置项，用来控制 observable 的行为，是一个对象，有下面几个属性：
       - `autoBind`：布尔值。为 true 的话默认使用 `action.bound` 或者 `flow.bound`，而不是使用 `action` 或者 `flow`。不会影响不影响带明确注解的类成员。
       - `deep`：布尔值。为 false 的话默认使用 observable.ref，而不是使用 observable。不会影响不影响带明确注解的类成员。
       - `name`：字符串，给这个对象赋予一个调试的名字，在错误信息或者反射的 API（reflection API）中，会打印这个调试的名字。
       - `proxy`：布尔值。为 false的话强制 `observable(thing)` 使用非代理的方式实现代理。如果对象内部的属性保持稳定，不再发生变化，那么此时配置 proxy 为 false，那么这个对象作为一个非代理的对象更容易调试，性能更好。详情可见：[void-proxies](https://mobx.js.org/observable-state.html#avoid-proxies)

7. observable 的返回值是代理对象，这表示被添加到对象中的属性可以被获取同时也会被变成可观察的。

8. observable 作为函数的使用：
   ```js
      const {observable, autorun} = require('mobx');

       const todos = observable([
          { title: "Spoil tea", completed: true },
          { title: "Make coffee", completed: false }
       ]);
       
       autorun(() => {
           console.log(
               "Remaining:",
                todos
                    .filter(todo => !todo.completed)
                    .map(todo => todo.title)
                    .join(", ")
           )
       })

      // 第一次执行
      // Remaining: Make coffee

      // Remaining: Spoil tea, Make coffee
      todos[0].completed = false;
      // Remaining: Spoil tea
      todos[1].completed = true;
   ```
9. observable 还可以作为装饰器（annotation），使用方式如下：`@observable classProperty = value`

10. 使用装饰器形式的 observable 可以在 ES7 或者 TypeScript 类属性中属性使用，将其转换成可观察的。 `@observable` 可以在实例字段和属性 `getter` 上使用。对于对象的哪部分需要成为可观察的，`@observable` 提供了细粒度的控制。

11. observable 作为装饰器的使用：
    ```js
       import { observable, computed } from "mobx";

        class OrderLine {
            @observable price = 0;
            @observable amount = 1;

             @computed get total() {
                 return this.price * this.amount;
             }
       }
    ```

### 2. Actions

#### 1. action

1. action 可以作为函数使用，也可以作为装饰器使用。

2. action 用在一个意图改变 state 的函数上。也就是我们定义一个用来修改 state 的函数，使用 action 进行包装，那么返回具有同样签名的函数。实际上是用 transaction、untracked 和 allowStateChanges 将函数包裹起来，尤其是 transaction 的自动应用能够优化性能，action 会分批处理变化并只在（最外层的）action 完成后通知计算属性（computed value）和 reaction。 这将确保在 action 完成之前，在 action 期间生成的中间值或未完成的值对应用的其余部分是不可见的。

3. 所有意图修改 state 的行为（函数）必须使用 action 进行包装。

4. 衍生信息的函数，比如执行查找或筛选数据，不应该使用 action 标记，以允许 MobX 跟踪其调用。

5. 使用 action 包装的函数应该是不可枚举的。

6. action 的使用形式如下：
   - `action(fn)`
   - `action(name, fn)`
   - `@action classMethod() {}`
   - `@action(name) classMethod () {}`
   - `@action boundClassMethod = (args) => { body }`
   - `@action(name) boundClassMethod = (args) => { body }`
   - `@action.bound classMethod() {}`

7. `action(fn)` 形式的代码示例如下：
   ```js
      const {observable, autorun, action} = require('mobx');
      const increment = action(state => {
          state.value++
          state.value++
      })

      increment(state)
      // 2
      console.log(state.value);
   ```
8. `action(name, fn)` 形式的代码示例如下：
   ```js
      const {observable, autorun, action} = require('mobx');
      const state = observable({
          str: ['hello', 'world'],
          output: ''
      })

      const join = action('join', function (state, separator) {
      state.output = state.str.join(separator);
      })

      join(state, ' ')

      // hello world
      console.log(state.output);
   ```
9. `@action classMethod() {}` 形式的代码示例如下：
   ```js
      class Stores {
          @observable name = 'foo';
          @observable count = 1;
    
          @action increment() {
              this.count++;
          }
      }
   ```

10. `action.bound` 这个装饰器的作用是用来将函数绑定到当前的实例中，因此函数内部的 this 总是能正确的指向当前实例。`action.bound` 不要和箭头函数一起使用，箭头函数已经是绑定过的并且不能重新绑定。

#### 2. runInAction

1. runInAction 用来创建一个临时的 action，创建后就会被调用。在异步 action中比较有用。

2. runInAction 接收一个函数作为参数，调用 runInAction 会立即将 接收的函数包装为 action，然后执行。

3. 我们需要使用 action 包装一个异步任务，当异步任务完成时，无论是成功还是失败，我们就调用 runInAction，向 runInAction 传入成功或失败的处理函数。

4. runInAction 的用法示例：
   ```js
      const {observable, autorun, action, runInAction} = require('mobx');

       /**
        * 定义一个异步操作
        * @param num
        * @returns {Promise<unknown>}
        */
        const getData = (num) => {
            return new Promise((resolve, reject) => {
                setTimeout(() => {
                    resolve(num);
                }, 2000);
            })
        }

         async function initData(num, state) {
             try {
                 const res = await getData(num);
                 // 异步任务结束，调用 runInAction
                 runInAction(() => {
                     handleNum(res, state);
                 })
             } catch (e) {

             }
         }

         const handleNum = (num, state) => {
             state.num = num;
             console.log(state.num);
         }

         // 使用 action 进行包装
         const initAction = action(initData);
         const state = observable({
             str: ['hello', 'world'],
             output: '',
             num: 0
         });
         initAction(15, state);
         // 2s 后，输出 15
         // 15
   ```

5. runInAction 还有另外一种调用形式：`runInAction(name, func)`。第一个参数是一个参数，第二个参数是函数。

#### 2. flow

1. flow 这个函数（装饰器）用来在异步操作中取代 `async` / `await`，并且支持取消。

2. flow 作为函数，只接收一个 Generator 作为作为参数，在这个 Generator 内部，通过使用 `yield` 关键字，我们能够链式调用 Promise，即使用 yield 替换 await。flow 函数的机制会保证 Generator 或者继续向下执行，或者得到一个 Promise 的 resolve 的结果。

3. 因此使用 flow 不需要像 `async` / `await` 那样获得异步操作的结果后再执行一个 action（通过 runInAction 实现）。
4. 应用步骤如下：
   1. 使用 flow 包装异步函数
   2. 使用 `function* ` 代替 `async`。
   3. 使用 `yield` 替代 `await`。
   
5. 使用 flow 完成异步代码示例：
   ```js
       const {observable, autorun, action, runInAction, flow} = require('mobx');

       /**
        * 定义一个异步操作
        * @param num
        * @returns {Promise<unknown>}
        */
         const getData = (num) => {
             return new Promise((resolve, reject) => {
                  setTimeout(() => {
                      resolve(num);
                  }, 2000);
             })
         }

          function* initData(num, state) {
               try {
                   const res = yield getData(num);
                      state.num = res;
                      console.log(state.num);
               } catch (e) {

                   }
               }
   
      const initDataFlow = flow(initData);
      // 2s 输出 15
      // initDataFlow 的返回值是一个 Promise
      initDataFlow(15, state)
   ```
6. flow 的返回值是一个函数，这个函数接收的参数和 flow 接收的 Generator 函数的参数一致。返回的函数的返回是一个 Promise。如果 Generator 中有返回值，那么会保存在 flow 的返回值函数的返回值 Promise 的 resolve 状态中。示例代码如下：
      ```js
       const {observable, autorun, action, runInAction, flow} = require('mobx');

       /**
        * 定义一个异步操作
        * @param num
        * @returns {Promise<unknown>}
        */
         const getData = (num) => {
             return new Promise((resolve, reject) => {
                  setTimeout(() => {
                      resolve(num);
                  }, 2000);
             })
         }

          function* initData(num, state) {
               try {
                   const res = yield getData(num);
                      state.num = res;
                      return res;
               } catch (e) {

                   }
               }
   
      const initDataFlow = flow(initData);
      
      // initDataFlow 的返回值是一个 Promise
      initDataFlow(15, state).then(res => console.log(res));
      // 2s 输出 15
   ```
7. 使用 flow 的另外一个好处就是可以被取消。flow 返回值函数的返回值是一个 resolve 状态的 Promise，其中的值是 Generator 中最后返回的值。这个返回的 Promise 带有一个 cancel 方法。调用 cancel 方法能中断正在运行的 Generator，并且取消它。`try` / `finally` 能继续执行。
#### 3. flowResult

1. flowResult 是为 TypeScript 用户准备的。能将 flow 函数返回的 Generator 函数转换成 Promise。 

2. 使用 flow 装饰一个 Generator，会使用 Promise 包装返回的 Generator。但是，TypeScript 不会意识到这种转换，因此 flowResult 将使 TypeScript 识别出这种转换。

### 3. Computeds

1. 计算值（computed value）可以用来从其他的可观察的数据中衍生信息。

#### 1. computed

1. computed 既可以作为函数使用，也可以作为装饰器使用。

2. computed 的作用是将一个可观察的值变成另外一个可观察的值，但是不会重复进行计算除非其所依赖的可观察的值发生了变化。

3. 从概念上来说，计算值很像 excel 中的公式，同时减少了我们必须存储的 state 的数量。计算值经过了高度优化，因此 Mobx 非常推荐我们使用计算值。

4. 将 computed 作为装饰器使用：
   ```js
      import {observable, computed} from "mobx";

      class OrderLine {
            @observable price = 0;
           @observable amount = 1;

          constructor(price) {
              this.price = price;
          }

          @computed get total() {
              return this.price * this.amount;
          }
      }
   ```
5. `observable.object` 和 `extendObservable` 都会自动将 getter 属性推导成计算属性，所以下面这样就足够了:
    ```js
       const orderLine = observable.object({
           price: 0,
           amount: 1,
           get total() {
                return this.price * this.amount
           }
       })
    ```
6. 还可以为计算值定义 setter。注意这些 setters 不能用来直接改变计算属性的值，但是它们可以用来作“逆向”衍生。
   ```js
      class Foo {
         @observable length = 2;
         @computed get squared() {
             return this.length * this.length;
         }
         set squared(value) { // 这是一个自动的动作，不需要注解
             this.length = Math.sqrt(value);
         }
     }     
   ```
   **注意**：永远在 getter 之后 定义 setter。

7. computed 还可以直接当做函数来调用。computed 函数接收一个函数，在这个函数内部是对可观察的数据进行操作。在返回的对象上使用 `get()` 来获取计算的当前值，或者使用 `observe(callback)` 来观察值的改变。这种形式的 computed 不常使用，但在某些情况下，你需要传递一个在“box”的计算值时，它可能是有用的。
   ```js
      const name = observable.box('Rose');

      const nameUppercase = computed(() => {
          // get 方法从可观察的 name 中拿到值
          // 对可观察的值操作完以后，要记得将其返回
          return name.get().toUpperCase();
      })
      // observe 是可观察对象的一个方法，接收一个回调函数作为参数
      // 观察当前的对象，对象的属性出现添加、更新、删除操作时分别触发 add、update 和 delete 事件，然后就会调用 observe 中的回调函数
      const output = nameUppercase.observe(change => console.log(change.newValue));

      name.set('Jack')

      // JACK
   ```
8. `computed` 装饰器装饰的函数不需要接收参数。如果你想创建一个能进行结构比较的计算属性时，请使用 `@computed.struct`。

9. 使用计算值的几个实践规则：
   1. 不应该拥有副作用或者是更新其他的可观察的数据
   2. 避免创建和返回新的可观察的值

10. 当使用 computed 作为函数使用时，它接收的第二个选项为配置对象，当然，computed 作为装饰器也可以接收配置对象。配置对象有如下可选参数:
    - name: 字符串, 在 spy 和 MobX 开发者工具中使用的调试名称。
    - context: 在提供的表达式中使用的 this。
    - set: 要使用的 setter 函数。 没有 setter 的话无法为计算值分配新值。 如果传递给 computed 的第二个参数是一个函数，那么就把会这个函数作为 setter。
    - equals: 默认值是 comparer.default。它充当比较前一个值和后一个值的比较函数。如果这个函数认为前一个值和后一个值是相等的，那么观察者就不会重新评估。这在使用结构数据和来自其他库的类型时很有用。例如，一个 computed 的 moment 实例可以使用 (a, b) => a.isSame(b) 。如果想要使用结构比较来确定新的值是否与上个值不同 （并作为结果通知观察者），comparer.deep 十分便利。
    - requiresReaction: 对于非常昂贵的计算值，推荐设置成 true 。如果你尝试读取它的值，但某些观察者没有跟踪该值（在这种情况下，MobX 不会缓存该值），则会导致计算结果丢失，而不是进行昂贵的重新评估。
    - keepAlive: 如果没有任何人观察到，则不要使用此计算值。 请注意，这很容易导致内存泄漏，因为它会导致此计算值使用的每个 observable ，并将计算值保存在内存中！

### 4. Reactions

1. Reaction 是一个需要理解的重要概念，因为它是 MobX 中所有东西结合在一起的地方。Reaction 的目标是模拟自动发生的副作用。它们的意义在于为我们的可观察状态创建消费者，并在相关内容发生变化时自动运行副作用。

2. 实际上，**用于实现 Reaction 的 API 很少用**。它们通常被抽象到其他库（如 mobx-react）或特定于您的应用程序的抽象中。

#### 1. autorun

1. autorun 接收一个副作用函数作为参数，只要 autorun 观察的数据发生变化，其就自动运行，就是执行其接收的函数。当我们创建 autorun 的时候，也会运行一次。只有被 observable 或者 computed 包装的值发生变化，autorun 才会做出相应。

2. autorun 通过在响应式上下文中（reactive context）运行副作用（effect）来工作。在执行所提供的函数期间，MobX 会跟踪所有可观察值和计算值，这些值直接或间接被副作用直读取。函数完成执行完成后，MobX 将收集并订阅所有已读取的可观察的值，并等待它们中的任何一个再次更改。一旦他们这样做，autorun 将再次触发，重复整个过程。

3. 如果在 autorun 接收地回调函数中，观察了某个可观察的值，那么在初始化的时候，autorun 会运行一次，然后只要这个可观察的值发生了变化，autorun 会再次被触发。

4. autorun 的返回值是一个函数，手动调用这个返回值函数，可以终止 autorun 运行。

5. autorun 接收第二个参数，它是一个配置对象，有如下可选的参数:
   - delay: 可用于对效果函数进行去抖动的数字 （以毫秒为单位）。如果是 0（默认值）的话，那么不会进行去抖。
   - name: 字符串，用于在例如像 spy 这样事件中用作此 reaction 的名称。
   - onError: 用来处理 reaction 的错误，而不是传播它们。
   - scheduler: 设置自定义调度器以决定如何调度 autorun 函数的重新运行。

6. 整个过程如下图所示：
   ![](./img/autorun.png)

7. autorun 的用法示例：
   ```js
      const {observable, autorun, action, runInAction, flow, computed} = require('mobx');
      const todos = observable([
          { title: "Spoil tea", completed: true },
          { title: "Make coffee", completed: false }
      ])
      autorun(() => {
          console.log(
              "Remaining:",
              todos
                  .filter(todo => !todo.completed)
                  .map(todo => todo.title)
                  .join(", ")
          )
      })
      // 第一次执行
      // Remaining: Make coffee


      // 第二次执行
      // Remaining: Spoil tea, Make coffee
      todos[0].completed = false;
      // 第三次次执行
      // // Remaining: Spoil tea
      todos[1].completed = true;
   ```
8. autorun 的用法示例 - 2：
   ```js
      const numbers = observable([1,2,3]);


       // 通过 computed 包装的是 numbers 这个可观察的数组，我们对这个数组进行求和
       // sum 就是计算值
       const sum = computed(() => numbers.reduce((a, b) => a + b, 0));
       // 观察的是计算值 sum，只要计算值 sum 发生变化，autorun 就自动运行
       // 返回值是一个函数，手动调用这个返回值函数，可以终止 autorun 运行
       const disposer = autorun(() => console.log(sum.get()));
       // 第一次运行，输出 6
       // 添加元素 4，输出 10
       numbers.push(4);
       // 添加元素 5，输出 15
       numbers.push(5);
       // 添加元素 6，输出 21
       numbers.push(6);

       // 调用 disposer 函数，终止 autorun 运行
       disposer();
       numbers.push(5);
       console.log(numbers)
   ```

#### 2. reaction

1. reaction 类似于 autorun，但提供了更细粒度的控制，可以跟踪具体的可观察的值。它接收两个函数：第一个是 data 函数，这个函数被跟踪并返回这个 data 用作第二个 effect 函数输入的数据。需要注意的是，副作用只对 data 函数中访问的数据起作用，这些数据可能比 effect 函数中实际使用的数据少。

2. 典型的模式是，您在 data 函数的副作用中生成所需的内容，并以这种方式更精确地控制触发效果的时间。默认情况下，必须更改数 data 函数的结果才能触发 effect 函数。与 autorun 不同，副作用不会在初始化时运行，但仅在数据表达式第一次返回新值后才会运行。

3. 示例 1：
   ```js
      const {observable, reaction, autorun} = require('mobx');

       const todos = observable([
          {
             title: '睡觉',
             finish: false
          },
          {
          title: '吃饭',
          finish: false
          },
          {
          title: '学习',
          finish: true
          },
          ]);

         /**
          * reaction 接收两个函数作为参数：第一个函数是 data 函数，第二个是 effect 函数
          * 其中，data 函数的返回值是 effect 函数的参数
          * 通常 data 函数中，我们跟踪的是可观察的数据，一旦可观察的数据发生变化，就会触发 effect 函数
          * 因此，data 函数必须有返回值
          *
          * 与 autorun 不同，reaction 在初始化的时候不会运行，只要观察的数据发生变化才会运行
          *
          */


          /**
           * reaction1 中的 data 函数只观察 todos 的长度，只有 todos 的长度发生变化，才能触发 effect 函数
           * @type {IReactionDisposer}
           */
           const reaction1 = reaction(() => {
               return todos.length;
           }, (length) => {
               console.log("reaction 1:", todos.map(todo => todo.title).join(", "));
           });

          /**
           * reaction2 中的 data 函数只观察 todos 的中每一项的 title 属性，只有 title 发生变化，才能触发 effect 函数
           * @type {IReactionDisposer}
           */
          const reaction2 = reaction(() => {
              return todos.map(item => item.title);
          }, (titles) => {
              console.log("reaction 2:", titles.join(", "));
          });

     /**
      * reaction3 中的 data 函数只观察 todos 的中每一项的 finish 属性，只有 finish 发生变化，才能触发 effect 函数
     * @type {IReactionDisposer}
       */
       const reaction3 = reaction(() => {
          return todos.filter(item => item.finish);
       }, (finish) => {
          console.log("reaction 3:", finish.map(item => item.title).join(', '));
        });

      /**
      * autorun 的回调函数观察的是 todos 中的 title 属性，只要 title 属性发生变化，那么就会触发
      * @type {IReactionDisposer}
        */
        const disposer = autorun(() => {
           const titles = todos.map(item => item.title).join(', ');
           console.log(titles);
        })

      // 向 todos 中添加一个 todo，todos 的长度发生变化，同时 todo 中包含 title 和 finish
      // 因此可以认为是是 title 和 finish 都发生了变化
      // 所以会触发 reaction1、reaction2、reaction3 和 autorun
      // 睡觉, 吃饭, 学习
      // reaction 1: 睡觉, 吃饭, 学习, 打游戏
      // reaction 2: 睡觉, 吃饭, 学习, 打游戏
      // reaction 3: 学习
      // 睡觉, 吃饭, 学习, 打游戏
      // todos.push({
      //     title: '打游戏',
      //     finish: false
      // });

      // 只改变 title 属性，那么只会触发 autorun 和 reaction2
      // 睡觉, 吃饭, 学习
      // reaction 2: 打篮球, 吃饭, 学习
      // 打篮球, 吃饭, 学习
      // todos[0].title = '打篮球';

      // 只改变 finish 属性，那么只会触发 autorun 和 reaction3
      // 睡觉, 吃饭, 学习
      // reaction 3: 吃饭, 学习
      todos[1].finish = true;
   ```
   在上面的示例中，reaction1、reaction2、 reaction3和 autorun 都会对 todos 数组中的 todo 的添加、删除或替换作出反应。但只有 reaction2 reaction3 和 autorun 会对某个 todo 的 title 或 finish 属性的变化作出反应，因为在 reaction2 和 reaction3 中data 函数中使用了 title 和 finish 属性，而 reaction1 的数据表达式没有使用。autorun 追踪完整的副作用，因此它将始终正确触发，但也更容易意外地访问相关数据。

4. 示例 2：
   ```js
      const counter = observable({
          count: 0
      })

      /**
       * reaction 的第二个参数 effect 函数还可以除了接收 data 函数返回值作为参数，还接收第二参数，这个参数可以调用 dispose() 方法，可以终止reaction 函数的执行
       * @type {IReactionDisposer}
       */
        const reaction4 = reaction(() => {
            return counter.count;
        }, (count, reaction) => {
            console.log("reaction 4: invoked. counter.count = " + count);
        reaction.dispose();

      });

      // reaction 4: invoked. counter.count = 1
      counter.count = 1;
      // 没有输出
      // 因为调用一次 reaction 函数后，在 其内部的 effect 函数内部，因为又调用了 dispose 函数，reaction 就被清理了
      // 所以后面 count 再发生变化，reaction 也不再做出响应
      counter.count = 2;

      // 2
      console.log(counter.count);
   ```
   在上面的示例中，reaction4 会对 counter 中的 count 作出反应。 当调用 reaction 时，第二个参数会作为清理函数使用。执行 effect 函数后，dispose 函数也被执行，所以 reaction 被清理，所以，当 count 变为 2 的时候，reaction 也不会做出响应。

5. 粗略地讲，reaction 是 `computed(expression).observe(action(sideEffect))` 或 `autorun(() => action(sideEffect)(expression))` 的语法糖。

6. reaction 接收第三个参数，它是一个参数对象，用来对 reaction 进行配置。有如下可选的参数:
   - fireImmediately: 布尔值，用来标识效果函数是否在数据函数第一次运行后立即触发。默认值是 false。
   - delay: 可用于对效果函数进行去抖动的数字（以毫秒为单位）。如果是 0（默认值） 的话，那么不会进行去抖。
   - equals: 默认值是 comparer.default。如果指定的话，这个比较器函数被用来比较由数据函数产生的前一个值和后一个值。只有比较器函数返回 false 效果函数才会被调用。此选项如果指定的话，会覆盖 compareStructural 选项。
   - name: 字符串，用于在例如像 spy 这样事件中用作此 reaction 的名称。
   - onError: 用来处理 reaction 的错误，而不是传播它们。
   - scheduler: 设置自定义调度器以决定如何调度 autorun 函数的重新运行。

#### 3. when

1. when 观察并运行给定的 predicate 函数，直到返回 true。 一旦返回 true，给定的 effect 函数就会被执行，然后 autorunner（自动运行程序） 会被清理。 

2. when 函数返回一个清理器函数，允许我们手动地取消取消自动运行程序。

3. 示例 1：
   ```js
      class MyResource {
          constructor() {
              when(
                  // 一旦...
                  () => !this.isVisible,
                  // ... 然后
                  () => this.dispose()
             );
         }

          @computed get isVisible() {
              // 标识此项是否可见
          }

          dispose() {
              // 清理
          }
      }

   ```
4. 如果没提供 effect 函数，when 会返回一个 Promise。它与 `async` / `await` 可以完美结合。
   ```js
      async function test() {
          await when(() => that.isVisible)
          // 等等..
      }
   ```

### 5. Configuration

1. Configuration 用来微调我们的 Mobx 实例。

#### 1. configure 

1. configure 是一个函数，接收一个配置对象作为参数。这个配置对象用来配置全局活动的 Mobx 实例。

##### 1. 代理设置（Proxy support）

1. 默认情况下，Mobx 使用 ES6 提供的 Proxy 机制将数组和纯对象变成可观察的数据。代理模式提供了最好的性能和跨环境的一致性。但是，如果我们的目标环境不支持代理，我们就需要禁用 Mobx 中的代理模式。

2. 通过配置项 `useProxies` 来决定是否启用代理：
   ```js
      import { configure } from "mobx"

      configure({
           useProxies: "never"
      })
   ```
3. `useProxies` 有下面几个值：
   - `always`：默认值，在支持代理的环境中使用。如果在不支持的环境中使用这个配置项会报错。
   - `never`：不使用代理模式，Mobx 将会回退到一个非代理模式上。适用于所有的 ES5 的环境。
   - `ifavailable`：实验性质，在支持代理的环境中使用代理模式，不支持代理的环境中回退到非代理模式。

##### 2. 装饰器支持（Decorator support）

1. Mobx 支持装饰器模式。我们需要启用对装饰器的支持才能在项目中使用装饰器，一般情况下使用 TypeScript 或者 Babel 就能实现对装饰器的支持。

2. 为了使我们在开发过程中采用 MobX 提倡的模式，我们需要严格划分 actions、state 和 derivations。Mobx 能够通过在运行时检查代码限制我们的编码模式。为了确保 Mobx 尽可能严格，我需要进行下面的配置：
   ```js
      import { configure } from "mobx"

      configure({
         enforceActions: "always",
         computedRequiresReaction: true,
         reactionRequiresObservable: true,
         observableRequiresReaction: true,
         disableErrorBoundaries: true
      })
   ```
3. 常用的配置项是 `enforceActions`。

###### 1. `enforceActions`

1. 这个配置项的作用是我们必须使用 action 包裹我们的事件处理程序。也就是更新 state 只能通过 action 完成。

2. 可选值：
   - observed：默认值。所有可观察的值通过 action 进行改变。这是 Mobx 推荐的严格模式。
   - never：state 可以在任何地方更改。即不通过 action 也可以改变 state。
   - always：state 只能通过 action 进行更改。在实际运用上也包括创建。
   
3. observed 的优势是：允许我们在 action 之外创建可观察的数据，并且能自由的更改他们。只要他们不会到处使用。也就是小范围内，我们可以自由改变可观察的数据，不受 action 的限制。

4. 原则上，state 应该从一些事件处理程序中创建，我们使用 action 包装事件处理程序，那么 always 模式适用于这种情况。但是在单元测试中不建议使用这个模式。

###### 2. `computedRequiresReaction`

1. 这个配置项的作用是禁止从 action 或者 reaction 外访问任何不可观察的计算值。这样保证了你不会使用 Mobx 不想缓存的计算值。

2. 默认值是 false。设置为 true 开启。

###### 3. `observableRequiresReaction` 

1. 访问任何不可观察的值会发出警告。如果我们想检查在不使用 Mobx 上下文的基础上是否在使用可观察的值，那么就启用这个配置项。

2. 这个配置项会找到任何我们在使用却没有用 observer 包装的值。

3. 默认值是 false，设置为 true 开启。

###### 4. `reactionRequiresObservable`  

1. 这个配置项的作用是：当创建一个 reaction 的时候，如 autorun，如果没有访问任何可观察的值，那么就会发出警告。

2. 使用的这个配置项的目的是：检查我们是否不必要地使用 observer 包装 React 组件、使用 action 包装函数。或者是找到只是忘记让某些数据结构或属性可变成可观察地数据的情况。

3. 默认值是 false，设置为 true 开启。

###### 5. `disableErrorBoundaries` 

1. 默认情况下，Mobx 会捕获并且重新抛出我们代码中的异常。这样可以确保 一个异常中的 reaction 不会阻止其他的可能不相关的 reaction 的计划执行。我理解的意思是，抛出异常，不会阻止其他的不相关的 reaction 的运行。

2. 这种情况会导致异常不会回到原始的出错的代码的地方，导致我们不能使用 try / catch 进行捕获。

3. 通过禁用异常边界，异常可以逃避衍生（exceptions can escape derivations）。我的理解是：不会再衍生过程中捕获异常，而是让异常继续抛出。
4. 这样做这可能会简化调试，但可能会使 MobX 和扩展程序处于不可恢复的中断状态。

5. 这个配置项用于单元测试。

6. 默认是 false，设置为 true 开启这个选项。

###### 6. `safeDescriptors`  

1. Mobx 会使一些字段使不可配置或者使不可写的，这样可以阻止我们做一些Mobx 不支持的事情或者有可能中断代码的行为。但是也可能阻止我们 spying / mocking / stubbing（和测试相关）。

2. 这个配置项 默认是 true，因此我们可以设置其为 true 来禁用这个安全措施。使得一切都是可配置的和可写的。这个不会影响已经存在的可观察的数据。仅仅是配置这个选项后创建的可观察数据会被影响。

3. 注意，只有需要的时候才能被开启，而且不要全局启用这个选项。

##### 3. 其他配置

###### 1. isolateGlobalState

1. 这个配置项的作用是：当同一个环境下，有多个 Mobx 实例在活动的时候，Mobx 会将全局的 state 进行隔离。

2. 当我们使用一个封装了 Mobx 的库，同时我们又在页面中引入了 Mobx，这样同一个页面会有多个 Mobx 实例，此时这个配置项就起了作用。当我们从库中调用：`configure({ isolateGlobalState: true })`，库中的 Mobx 的活动将保持独立。

3. 如果没有此选项，如果多个 MobX 实例处于活动状态，则它们的内部状态将共享。好处是两个实例的可观察数据会一起工作，缺点是它们的 MobX 版本必须匹配。

4. 默认值是 false，设置为 true 开启。

###### 2. reactionScheduler

1. 这个配置项的作用是：设置一个新的函数，用来执行所有 MobX 的 reaction。默认情况下，reactionScheduler 只运行 f reaction （f 为 reactionScheduler 函数接收的值，也是一个函数），没有任何其他行为。这对于基本调试或减缓可视化应用程序更新的 reaction 非常有用。

2. 这个配置项是一个函数：`f => f()`，其接收一个函数作为参数，如下所示：
   ```js
      configure({
          reactionScheduler: (f) => {
              console.log("Running an event after a delay:", f)
              setTimeout(f, 100)
          }
      })
   ```

## 6. api 参考 —— 以 v6 版本的 Mobx 为例

#### 1. makeObservable

1. makeObservable 是一个函数，借助这个函数，我们可以对一个对象的每个属性指定一个声明（annotation），从而将其变成可观察的数据。最重要的声明（annotation）是：
   - observable：定义一个存储 state 的可追踪的属性。
   - action：将一个可以改变 state 的函数标记为 action。
   - computed：标记一个 getter 方法，这个 getter 方法将获得从 state 衍生出新的值，并且这个值会被缓存。

2. 数组、Map 和 Set 也会自动得被转换成可观察的数据。

3. 函数调用形式：`makeObservable(target, annotations?, options?)`

4. 参数说明：
   - target：一个 js 对象，包括类实例
   - annotation：可选，将声明映射到每个类成员上，当使用装饰器的时候，这个参数可以省略。
   - option：可选，配置项，决定 makeObservable 的行为。

5. makeObservable 可以用来追踪已经存在的对象的属性，并且将他们变成可观察的数据。通常来说，makeObservable 在类的构造函数中使用，并且其第一个参数是 this。

6. 在类中使用 makeObservable：
   ```js
      import { makeObservable, observable, computed, action, flow } from "mobx"

      class Doubler {
           value

          constructor(value) {
              makeObservable(this, {
                  value: observable,
                  double: computed,
                  increment: action,
                  fetch: flow
              })
              this.value = value
          }

          get double() {
              return this.value * 2
          }

          increment() {
              this.value++
          }

          *fetch() {
              const response = yield fetch("/api/value")
              this.value = response.json()
          }
      }
   ```

#### 2. makeAuthObservable 

1. 函数调用形式：`makeAutoObservable(target, overrides?, options?)`

2. 参数说明：
   - target：一个 js 对象，包括类实例
   - overrides：可选，一个对象，用来指定哪些成员会被注解（annotation）或者不被注解（指定这个成员属性为 false），我的理解是：哪些成员变成可观察的。
   - options：可选，配置项

3. makeAuthObservable 就像 makeObservable 一样，它默认推断所以属性。我们仍然可以通过 overrides 属性来给某个属性指定注解（annotation）来覆写默认的属性。特别是可以使用 false 属性完全排除已经处理过的属性或方法。

4. makeAutoObservable 函数可以使代码变得更紧凑且更易于维护，因为不必明确提及新成员。 但是 makeAutoObservable 不能用于具有超类或子类的类。

5. 示例代码：
   ```js
      import { makeAutoObservable } from "mobx"

      function createDoubler(value) {
           return makeAutoObservable({
               value,
               get double() {
                   return this.value * 2
               },
               increment() {
                   this.value++
               }
           })
      }
   ```
6. **注意**：上面的例子使用的是构造函数生成一个类，我们也可以使用在类中使用 makeAutoObservable。

7. 自动推断规则：
   - 所有类中自有的（非继承）属性变成可观察（observable）的数据。
   - 使用 `get` 定义的属性，即 getter 变成计算值（computed）。
   - 使用 `set` 定义的属性，即 setter 变成 action。
   - 原型（prototype）上所有的函数变成 autoAction。
   - 原型（prototype）上的所有的 Generator  变成 flow 类型。注意，在一些转译器配置中无法检测到 Generator 函数，如果 flow 没有如预期的那样工作，请确保指定了 flow。
   - 在 overrides 对象中标记为 false 的类成员将不会被注解。例如，像识别符（identifiers）一类的只读字段。

#### 3. makeObservable 和 makeAuthObservable 中的第三个参数：options

#### 4. makeObservable 和 makeAuthObservable 的限制

## 7. Mobx v5 版本和 v6 版本的区别

1. Mobx v5 以及之前版本鼓励我们使用 ES 的装饰器（decorator）来标注变量。将对象变成可观测的值使用 `@observable`，计算值使用 `@computed`，而定义 action 使用 `@action`。

2. 由于装饰器现在并不是 ES 的标准语法，并且装饰器进入标准还需要很长时间，而且看起来进入标准的装饰器语法可能和现在的装饰器语法不一样。因此，从适用性上考虑，在 Mobx v6 版本中移除了装饰器语法，推荐我们使用 `makeObservable` / `makeAutoObservable` 来替代之前的装饰器。

3. 许多现存的代码还是使用了装饰器，而且许多文档和教程也使用了装饰器。那么规则就是我们在 `makeObservable` 中也可以使用像 `observable`、`action` 和 `computed` 一类的装饰器。举例如下：
   ```js
      import { makeObservable, observable, computed, action } from "mobx"

      class Todo {
         id = Math.random()
         @observable title = ""
         @observable finished = false

          constructor() {
              makeObservable(this)
          }

          @action
          toggle() {
              this.finished = !finished
          }
      }

      class TodoList {
          @observable todos = []

          @computed
          get unfinishedTodoCount() {
              return this.todos.filter(todo => !todo.finished).length
          }

          constructor() {
              makeObservable(this)
          }
      }
   ```
4. Mobx 在 v6 版本之前不会要求在类的构造函数中调用 `makeObservable(this)`。但是由于 `makeObservable` 实现的装饰器的实现更简单，更兼容，现在 Mobx 要求这样做。这使得 MobX 根据装饰器中的信息使实例可观察，装饰器取代了 `makeObservable` 的第二个参数。 

5. Mobx 倾向于以这种形式继续支持装饰器。任何存在的基于 Mobx v4/v5 版本的代码可以通过调用 `makeObservable` 来实现对新版本的兼容。

6. 来自 mobx-react 中的 `observer` 函数，既可以作为高阶组件使用，也可以作为装饰器使用，都可以用在类组件上。
   ```js
      @observer
      class Timer extends React.Component {
          /* ... */
      }
   ```

## 8. 项目中如何启用对装饰器的支持

### 1. TypeScript

1. 在 tsconfig.json 中设置下面两个编译选项为 true：
   - `experimentalDecorators`
   - `useDefineForClassFields`

### 1. Babel

1. 安装`@babel/plugin-proposal-class-properties` 和 `@babel/plugin-proposal-decorators babel` 这两个 babel 插件用来对装饰器语法进行编译：`npm i --save-dev @babel/plugin-proposal-class-properties @babel/plugin-proposal-decorators`

2. 在 `babelrc.json` 中进行配置：
   ```json
      {
          "plugins": [
              ["@babel/plugin-proposal-decorators", { "legacy": true }],
              ["@babel/plugin-proposal-class-properties", { "loose": false }]
              // In contrast to MobX 4/5, "loose" must be false!    ^
              ]
      }
   ```

### 3. Create React App

1. create-react-app 在 2.1.1 及以上的版本且使用 TypeScript 为模板的情况下才支持装饰器语法。

2. 旧版本的 create-react-app 或者使用 `npm run eject` 命令将 webpack 配置暴露出来，或者使用 `customize-cra` 这包。