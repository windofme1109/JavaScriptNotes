## react diff 算法复杂度是多少

1. O(n)

2. 正常比较两颗树的差异的算法的复杂度是 O(n^3)

3. React 团队做了两个假设
   - 不同的组件生成的树的结构不同 
   - 可以通过设置 key props 暗示哪些子元素在不同的渲染下能保持稳定

## react diff算法如何实现的,比对复杂度是多少

1. diff 算法的核心是如何高效地比较两颗虚拟 DOM 树

2. 两个假设
   - 不同的组件生成的树的结构不同 
   - 可以通过设置 key props 暗示哪些子元素在不同的渲染下能保持稳定

3. 三个层次：tree diff、component diff 和 element diff

4. tree diff
   - 由于我们很少跨级别修改 DOM 节点，所以 React 只对同级别地节点进行比较。发现节点不存在地时候，就会删除这个节点及其子节点，不再进行比较。
   - React 只考虑同级别节点地位置变换，对于不同级别地节点，只有删除和新建操作。
   - React 官方建议不要进行 DOM 节点跨层级的操作。
5. component diff
   - 同一类型的组件，按照原来的策略，继续比较其子节点。如果 React 判断其为不同类型的节点，则不继续比较，而是重建这个节点及其子节点。
   - 因此 React 允许用户通过 shouldComponentUpdate() 判断该组件是否需要进行diff。

6. element diff 
   - 当节点处在同一级别的时候，React diff 提供了三种节点操作：插入、移动和删除
   - 插入 老集合中没有，新集合中有
   - 移动 新老集合中有，但是位置不同
   - 删除 老集合中有，新集合中没有，或者是老的 component 类型，在新集合中也有，但对应的 element 不同则不能直接复用和更新

8. 经过上面三个步骤的比较，得出最小的改动 —— patches，将这个改动映射到真实的 dom 树上。

9. 同级比较、深度优先

10. 复杂度是 O(n)

## diff 造成的非预期更新如何解决 —— 不太明白

1. shouldComponentUpdate 中进行判断，如果 props 或者 state 没有变换，就 return false，阻止继续更新。

## react 生命周期

1. mount
   - constructor
   - componentWillMount
   - render
   - componentDidMount

2. update
   - componentWillReceiveProps
   - shouldComponentUpdate
   - componentWillUpdate
   - render
   - componentDidUpdate

3. unmount
   - componentWillUnmount

4. 新版的声明周期函数
   - getDerivedStateFromProps
   - 这是一个静态方法，不能调用 this
   - 比较 state 和 props，来判断是否需要更新 state
   - 不需要更新 state，要返回 null

5. 废弃三个声明周期函数
   - componentWillMount
   - componentWillUpdate
   - componentWillRecieveProps

## react 生命周期有了解吗

## setState 更新是同步还是异步

1. 在声明周期函数和 React 自身的事件中调用 setState，setState 的更新是异步的

2. 在原生事件中 setState 是同步更新的

3. 在 setTimeout 中 setState 是同步更新的

## react 中 setState 同步还是异步

## 在哪些生命周期函数中可以调用 setState

1. componentWillMount 中调用 setState，更新的 state 会和初始化的 state 合并

2. componentWillUnmount 中调用无意义

3. componentWillUpdate 和 shouldComponentUpdate 中禁止调用，因为会引起循环渲染

4. componentDidMount 和componentWillReceivePRrops 中正常调用

## react的强制更新有了解吗

1. setState ——> 组件更新
2. props ——> 组件更新
3. forceUpdate ——> 组件更新

## react 函数组件和类组件触发更新的方式有哪些

1. 类组件更新的方式
   - setState ——> 组件更新
   - props ——> 组件更新
   - forceUpdate ——> 组件更新 

2. 函数组件
   - 父组件更新
   - props 变化

3. 父组件更新，其子组件也会跟着更新

## react 在一秒内点击按钮多次(+1),如何获取最后一次的新状态

1. setState 第二个参数是一个回调，在render 完成以后会立即调用，因此可以在这个回调函数中获得最新的状态。

## react合成事件了解吗？

1. 合成事件是 React 自己实现的一套事件机制，抹平了不同的浏览器之间的差异。

2. 将所有的事件挂载到 document 上，利用事件冒泡定位到具体的元素上。

## 这些事件处理函数最终挂载到了哪？

1. 合成事件最终挂载到 document 上

## react中如何阻止冒泡

1. e.stopPropagation() 或 e.preventDefault()

## 选择 hooks 的优点

1. 类组件的缺点
   - 状态逻辑难以复用
   - this 指向问题
   - 不同的逻辑分散在不同的生命周期，管理复杂

2. hook 的优点
   - 函数组件无 this 问题
   - 自定义 Hooks 方便复用状态逻辑
   - 副作用的关注点分离

## 说一下为什么要用 hooks，解决了什么问题

## react 为什么要引入 hooks，解决了哪些问题

## 为什么不要在循环、条件语句或者嵌套方法中调用 Hooks

1. 我们可以在一个组件中定义多个 hook，那么 React 是怎么知道我们调用的是哪个 hook 呢，React 是通过调用顺序来判断我们是调用的哪个 hook。

2. 那么如果在循环、条件语句等调用 hook，会导致 hook 调用的顺序与定义的顺序不一样，这样就会导致数据对应关系出现问题。

## hooks为什么不能在条件或循环中使用,原理清楚吗？

## react hooks 用过哪些

1. useState

2. useEffect

3. useContext

## 说一下类组件和函数组件

1. 函数组件
   - 没有生命周期
   - 没有 state，不能保持数据状态，即是无状态的组件
   - 可以认为函数组件是一个 JavaScript 函数，接收 props 参数，返回值是 React 元素，即只做 UI 渲染。
   - React 从 16.9 版本引入了 hook 函数，因此函数组件也可以有状态。
 
2. 类组件
   - 类组件可以拥有自己的状态
   - 类组件拥有生命周期
   - 类组件继承 React 内置的组件类（React.Component），必须覆写 render() 方法。

## 什么时候用类组件

1. 组件拥有数据状态

2. 需要在组件渲染的不同阶段做一些操作 —— 生命周期

## 类组件如何实现逻辑复用？

1. 高阶组件 —— HOC

2. render props


## 单页面应用和传统服务端渲染的差异比较

1. SPA
   - 整个 web 项目只有一个页面，使用前端路由的机制进行组件之间的切换
   - 优点
     - 前后端分离，使得前端和后端开发人员专注于自己的领域
     - 客户端渲染，传输的数据量小
     - 服务端压力小
     - 交互/相应速度快，因为切换是在前端完成的，不必向后端发送请求
   - 缺点
     - 首屏加载时间慢
     - 对 SEO 不友好
   - 适用场景
     - 对项目性能要求高
     - 页面加载速度快
     - 要求客户端渲染
     - 对 SEO 要求低

2. SSR
   - 在服务端生成组件或者页面的 html 字符串，发送到浏览器以后，浏览器直接渲染 html 字符串
   - 优点
     - 对 SEO 友好
     - 首屏加载时间快
   - 缺点
     - 页面重复加载次数多
     - 开发效率低，因为不是前后端分离开发
     - 数据传输量大，服务器压力大
   - 适用场景
     - 对 SEO 要求高
     - 首屏加载时间快



## 说下 react 的 key

1. 我们在进行列表渲染时，最好给每一个组件添加一个 key 属性，key 不能重复，用来标识这个组件。

2. key 的主要作用保持组件在渲染过程中的稳定，即在 diff 算法中，如果两个组件的 key 一致，那么 React 会认定这个组件没有变化，进而不需要进行比较。从而提高了比较的效率。

3. key 需要保持稳定，一个节点在确定 key 之后就不应当变更 key（除非你希望它重新渲染）。因此我们可以适用数组元素的索引
每条数据的唯一标识等作为 key。

## 说下 react 常用的性能优化手段

1. 渲染列表结构数据的时候，给组件添加 key 属性。

2. 在 shouldComponentUpdate 中判断组件是否需要渲染

3. 继承 PureComponent，React 会自动进行比较 props，从而决定是否需要重新渲染。

4. 事件绑定处理函数时，避免直接写箭头函数，可以事先声明，然后给组件引用。

5. 尽量少用不可控的refs、DOM操作，减少直接的 DOM 操作

6. 提高组件的可复用性，降低组件之间的耦合程度

7. 对于单纯的 UI 展示，使用函数组件完成

8. 使用函数组件 + hook 代替类组件

## 前端页面性能优化

## react使用心得

## react15 和 react16 更新机制的差异

1. React 15 是同步更新，比如说调用生命周期函数、计算比较虚拟 DOM，执行渲染等，这些过程都是同步的，也就是说，一旦更新开始，React 就一路运行到底，直到整个过程结束。

2. 同步更新会带来性能隐患，比如渲染一个比较复杂的组件，由于js是单线程运行，那么整个渲染期间，主线程都在执行同步的渲染任务，浏览器就会失去响应，出现假死状态，对于用户体验非常不好。

3. React Fiber 核心思想：React 渲染的过程可以被中断，可以将控制权交回浏览器，让位给高优先级的任务，浏览器空闲后再恢复渲染。

3. React 16 引入了 fiber 机制，把一个耗时长的任务分成很多小片，每一个小片的运行时间很短，虽然总时间依然很长，但是在每个小片执行完之后，都给其他任务一个执行的机会，这样唯一的线程就不会被独占，其他任务依然有运行的机会。

4. Fiber 机制中，一次更新过程会分成多个分片完成，所以完全有可能一个更新任务还没有完成，就被另一个更高优先级的更新过程打断，这时候，优先级高的更新任务会优先处理完，而低优先级更新任务所做的工作则会完全作废，然后等待机会重头再来。

## 为什么 react16 架构升级后就能中断更新,根据什么决定是否中断

1. 引入了 fiber 机制，把一个耗时长的任务分成很多小片，每一个小片的运行时间很短，虽然总时间依然很长，但是在每个小片执行完之后，都给其他任务一个执行的机会，这样唯一的线程就不会被独占，其他任务依然有运行的机会。

2. 每个小任务都有优先级，执行时间到了，就将控制权让出来，给其他任务机会。

## 说下 react-redux 干了什么事

1. 去除 redux 与 React 组件的耦合

2. 将 store 注入组件中，使得组件轻松的拿到全局状态，方便组件间的通信。

3. 将 UI 组件变为容器组件

4. connect() 函数 将 React 组件和 Redux 的 store 连接起来，生成一个容器组件，负责数据管理和业务逻辑。

5. Provider 组件用来包裹根组件，接收 store 对象，并将其注入组件中。

## mapStateToProps 的第二个参数作用

1. mapStateToProps 是一个函数，将 state 映射为 props。state 就是 Redux store 中保存的应用状态，它会作为参数传递给 mapStateToProps，props 就是被连接的展示组件的 props。

2. 第一个参数是 state，第二个参数是 ownProps，代表容器组件接收的 props 对象。

3. 使用 ownProps 作为参数后，如果容器组件的参数发生变化，也会引发 UI 组件重新渲染。

## 前端路由实现原理(对比react,vue)

1. HashRouter 与 BrowserRouter
   - HashRouter 监测的是浏览器地址栏的 url 中的 hash 部分，当 hash 值发生变化时，根据当前的 hash 值切换具体的路由组件。当 hash 值变化时，浏览器不会向后端发送请求。
   - 由于 HashRouter 会在路径上添加 /#/，而 /#/ 后面的所有都不会发送到服务器端，即对于服务器而言，路径依旧是localhost:3000，路由切换在前端完成。
   - BrowserRouter 监测的是浏览器地址栏的 url 中的 path 部分，根据具体的 path 来进行路由组件的切换。当地址栏的 path 发生变化时，浏览器会向服务器发送请求。
   - 使用 BrowserRouter 需要再加一层服务器配置(node/nginx),让前端发送的请求映射到对应的 html 文件上。



## react中遇到的坑，怎么解决的

如何实现路由监听
说下 react-router 源码你看完后印象深刻的部分
react权限路由实现


react-router权限路由写一下



如何使用react-dnd完成拖放的,说下主要API

react-dnd拖放的核心API

react 源码看过哪些？

react中调和的部分是在哪个包?有看过实现吗


redux模板语法的改良(使用装饰器)

