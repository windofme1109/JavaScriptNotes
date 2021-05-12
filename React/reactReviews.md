# React 相关知识总结


## 1. 生命周期

### 1. 参考资料

1. [React生命周期及setState原理分析](https://www.jianshu.com/p/e09cbecca1d1)
2. [你真的了解 React 生命周期吗](https://juejin.cn/post/68449040212332380241)
3. [我对 React V16.4 生命周期的理解](https://juejin.cn/post/6844904199923187725)
4. [React v16.3之后的组件生命周期函数](https://zhuanlan.zhihu.com/p/38030418)
5. [React新生命周期--getDerivedStateFromProps](https://www.jianshu.com/p/50fe3fb9f7c3)

### 2. 旧版生命周期

1. React 的组件在渲染过程中可以分为以下三个阶段：
   - 加载（mount）
   - 更新（update）
   - 卸载（unmount）

2. 相应的，组件的生命周期也可以分为三个阶段：
   - 加载（mount）
   - 更新（update）
   - 卸载（unmount）

3. 在 16.3 版本之前的生命周期函数如下图所示：
   ![](./img/life-cycle-1.jpg)

4. 初始化加载阶段
   1. `getDefaultProps()`
      设置默认的 `props`，也可以用 `defaultProps` 设置组件的默认属性，得到的是`component.defaultProps` 中设置的值。
   2. `getInitialState()`（`constructor()`）
      在使用 es6 的 class 语法时是没有这个钩子函数的，可以直接在 constructor 中定义 this.state。或者直接在组件中设置 state。此时可以访问 this.props。
   3. `componentWillMount()`
      组件初始化时只调用，以后组件更新不调用，整个生命周期只调用一次，此时可以修改 state。
   4. `render()`
      最重要的步骤，创建虚拟 DOM，进行 diff 算法，更新 DOM 都在此进行，此时不能修改 state。
   5. `componentDidMount()`
      组件渲染完成后调用，只调用一次。

5. 更新阶段
   1. `componentWillReceiveProps(nextProps)`
      组件初始化时不调用，组件接收新的props时调用。
   2. `shouldComponentUpdate(nextProps, nextState)`
      react性能优化非常重要的一环。组件接受新的 state 或者 props 时调用，我们可以设置在此对比前后两个 props 和 state 是否相同，如果相同则返回 false 阻止更新，因为相同的属性状态一定会生成相同的 dom 树，这样就不需要创造新的 dom 树和旧的 dom 树进行diff算法对比，节省大量性能，尤其是在 dom 结构复杂的时候。
   3. `componentWillUpdate(nextProps, nextState)`
      组件初始化时不调用，只有在组件将要更新时才调用，此时不可以修改state。也不可以做别的事。
   4. `render()`
      组件渲染。
   5. `componentDidUpdate(prevProps, prevState, snapshot)`
      组件初始化时不调用，组件更新完成后调用，此时可以获取dom节点。

6. 卸载阶段
   1. `componentWillUnmount()`
      组件将要卸载时调用，一些事件监听和定时器需要在此清除。

7. 组件通过其定义的几个函数来控制组件在生命周期的各个阶段的行为。

8. 在生命周期中的关键函数：

   1. `constructor(props, context)`
      构造函数，在创建组件的时候调用一次。

   2. `void componentWillMount()`
       在组件挂载之前调用一次。如果在这个函数里面调用setState，本次的render函数可以看到更新后的state，并且只渲染一次。
   3. `void componentDidMount()`
       在组件挂载之后调用一次。这个时候，子组件也都挂载好了，可以在这里使用refs。
       
   4. `void componentWillReceiveProps(nextProps)`
       props是父组件传递给子组件的。父组件发生render的时候子组件就会调用componentWillReceiveProps()（不管props有没有更新，也不管父子组件之间有没有数据交换）。

   5. `bool shouldComponentUpdate(nextProps, nextState)`
       组件挂载之后，每次调用setState后都会调用shouldComponentUpdate判断是否需要重新渲染组件。默认返回true，需要重新render。在比较复杂的应用里，有一些数据的改变并不影响界面展示，可以在这里做判断，优化渲染效率。

   6. `void componentWillUpdate(nextProps, nextState)`
       shouldComponentUpdate返回true或者调用forceUpdate之后，componentWillUpdate会被调用。
   7. `void componentDidUpdate(prevProps, prevState, snapshot)`

       除了首次render之后调用componentDidMount，其它render结束之后都是调用componentDidUpdate。
        componentWillMount、componentDidMount和componentWillUpdate、componentDidUpdate 可以对应起来。区别在于，前者在只有组件装载的时候才会被调用，而后者是每次更新的时候就会被调用。 

   8. `ReactElement render()`
       render是一个React组件所必不可少的核心函数（上面的其它函数都不是必须的）。记住，不要在 render 里面修改 state。

   9. `void componentWillUnmount()`
       组件被卸载的时候调用。一般在componentDidMount里面注册的事件需要在这里删除。

9. 触发更新的几种方式（**注：假设shouldComponentUpdate都是按照默认返回true的方式**）：
   1. 初始化阶段
   2. 调用 this.setState() （并不是调用一次 setState 会触发一次 render，React 可能会合并操作，再批量进行 render）
   3. 父组件发生更新 （一般就是 props 发生改变，但是就算 props 没有改变或者父子组件之间没有数据交换也会触发 render）
   4. 调用 this.forceUpdate()

10. 触发更新的图示：
    ![](./img/render.png)

11. **生命周期函数调用 setState() 的情况**  
    - 无意义使用：`componentWillMount`，`componentWillUnmount`
      在 `componentWillMount` 中使用，会将 setState() 设置的 state 与初始设置的 state 合并。不会引起 render。`componentWillUnmount` 是组件即将要卸载，因此在这里改变 state 也没有意义。
    - 有条件使用：`componentDidUpdate`
    - 禁止使用：`componentWillUpdate`，`shouldComponentUpdate`
      在这两个生命周期函数中使用 setState()，会引起循环渲染，因此不能在这两个生命周期函数中触发 setState()。
    - 正常使用：`componentWIllReceiveProps`，`componentDidMount`

### 3. 新版生命周期

1. 在 16.3 以及后面的版本中，React 引入了新的生命周期函数，同时还废弃了三个生命周期函数。

2. 变化的原因是 React 引入了 Fiber 机制，渲染由同步过程变为了异步过程。在同步渲染过程中，每个生命周期只调用一次，而引入了 Fiber 机制以后，由于是异步渲染过程，有一些生命周期函数可能会被调用多次，比如在 `componentWillMount()` 发送 ajax 请求，异步渲染过程导致 `componentWillMount()` 可能被调用多次，这会导致发送多次 ajax 请求。这肯定不是我们希望的。

3. 废弃的生命周期函数：
   - `componentWillMount()`
   - `componentWillReceiveProps`
   - `componentWillUpdate`

4. 在 16.3 版本中，给这三个生命周期函数加上了 `UNSAFE_` 的前缀，如下：
   - `UNSAFE_componentWillMount`
   - `UNSAFE_componentWillUpdate`
   - `UNSAFE_componentWillReceiveProps`

5. 在 17 版本中，会移除这三个生命周期函数。

6. 仔细观察这三个生命周期函数，发现都是带 will 的，表示这是在 render 前会调用。官方不推荐我们在这几个生命周期函数中进行一些操作，如发送 ajax 请求，setState 等，容易引起 bug。所以直接就废弃了。断绝了我们在这些函数进行操作地念头。

7. 引入的新的生命周期函数：
   - `getDerivedStateFromProps`
   - `getSnapshotBeforeUpdate`

8. 新的生命周期流程：
   ![](./img/16.4-life-cycle.png)

9. 从上面的图中可以看出，`getDerivedStateFromProps` 总是在 render 前被调用。因此之前在 `componentWillReceiveProps` 中进行的一些操作可以转移到 `getDerivedStateFromProps` 中进行。

10. `getDerivedStateFromProps` 的用法
    - static getDerivedStateFromProps(nextProps, prevState)
    - 这是一个静态（static）方法，因此不能使用 this。
    - 从函数名可以看出，这个函数的目的是：将 props 映射为 state。
    - 通过比较 nextProps（最新的 props）与 prevState（当前的 state），如果有变化，需要更新 state，那么就返回一个对象，这个对象里是需要更新的 state。如果 props 的变化不影响 state，那么需要返回一个 null，表示不对 state 进行任何改变。
    - 代码示例：
      ```jsx
         /**
           * 根据 props 来更新 state
           * props 更新、setState()、forceUpdate() 都会调用这个生命周期函数
           * 静态方法，纯函数，不能调用 this，只能做一些一些没有副作用的操作
           * @param nextProps 下一个 props，可以理解为最新的 props
           * @param prevState 前一个阶段的 state
           */
         static getDerivedStateFromProps(nextProps: Readonly<ITransitionProps>, prevState: Readonly<ITransitionState>) {
             console.log('getDerivedStateFromProps', nextProps, prevState);
             // 从 props 中拿到最新的 inAndOut
             const {inAndOut} = nextProps;
             // 通过 setState() 触发这个方法时，会将当前组件的 state 传入
             const {status} = prevState;
             if (inAndOut && status === UNMOUNTED) {
                 return {
                    status: EXITED
                 }
             }

             return null;

        }
      ``` 
## 2. 虚拟 DOM

## 3. setState

## 4. 类组件与函数组件

## 5. 数据传递方式



