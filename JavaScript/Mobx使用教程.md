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
3. 作为函数：`observable(source, overrides?, options?)`  

4. 作为装饰器（annotation）：`@observable` 

### 2. Actions

#### 1. action

#### 1. runInAction

#### 2. flow

### 3. Computeds

#### 1. computed

### 4. Reactions

#### 1. autorun

#### 2. reaction

#### 3. when


### 5. Configuration

#### 1. configure 

## 6. api 参考 —— 以 v6 版本的 Mobx 为例

## 7. Mobx v5 版本和 v6 版本的区别
