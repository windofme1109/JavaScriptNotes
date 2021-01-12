<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [React](#react)
  - [1. React V16.8 新特性 —— Hooks](#1-react-v168-%E6%96%B0%E7%89%B9%E6%80%A7--hooks)
    - [1. 什么是Hook？](#1-%E4%BB%80%E4%B9%88%E6%98%AFhook)
    - [2. 类组件的缺点](#2-%E7%B1%BB%E7%BB%84%E4%BB%B6%E7%9A%84%E7%BC%BA%E7%82%B9)
    - [3. Hooks优势](#3-hooks%E4%BC%98%E5%8A%BF)
    - [4. useState()](#4-usestate)
    - [5. useEffect()](#5-useeffect)
  - [2. 如何获取组件实例](#2-%E5%A6%82%E4%BD%95%E8%8E%B7%E5%8F%96%E7%BB%84%E4%BB%B6%E5%AE%9E%E4%BE%8B)
  - [3. react-router 原理](#3-react-router-%E5%8E%9F%E7%90%86)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# React

## 1. React V16.8 新特性 —— Hooks

### 1. 什么是Hook？

1. Hooks let you use state and other React features without writing a class.

### 2. 类组件的缺点

1. 状态逻辑复用难
   1. 缺少复用机制
   2. 渲染属性和高阶组件导致层级冗余
   
2. 趋向复杂难以维护
   1. 生命周期函数混杂不相关
   2. 相关逻辑分散在不同的生命周期
   
3. this 指向困扰
   - 内联函数过度创建新句柄
   - 类成员函数不能保证 this
   
### 3. Hooks优势

1. 优化类组件的三大问题
   - 函数组件无 this 问题
   - 自定义 Hooks 方便复用状态逻辑
   - 副作用的关注点分离
   
### 4. useState()

1. state hook 提供了一种可以在 function component 中添加状态的方式。通过 state hook，可以抽取状态逻辑，使组件变得可测试，可重用。开发者可以在不改变组件层次结构的情况下，去重用状态逻辑。更好的实现关注点分离。
  
2. state hook的主要作用就是获取需要的 state 和 更新 state 的方法 
 
3. useState() 用来替换类组件的 state。

4. 要点

   1. 在组件的最顶层按照顺序调用 useState()。
   2. 不能动态调用，如在 if 语句，for 循环中等都不可使用 useState()。

5. useState() 可以接收一个函数，延迟初始化，也就是负责逻辑的转换，从而得到一个新的初始值。

6. 一个 useState() 只负责一个状态。

### 5. useEffect()

1. 数据获取，设置订阅，手动的更改 DOM，都可以称为副作用，可以将副作用分为两种，一种是需要清理的，另外一种是不需要清理的。比如网络请求，DOM 更改，日志这些副作用都不要清理。而比如定时器，事件监听，则需要清理。  

2. 副作用调用时机：
   1. Mount 之后
   2. Update 之后
   3. Unmount 之前
   
3. useEffect() 调用时机 
   - useEffect() 在 render() 之后调用，在不同的 render() 时刻，就相当于使用 componentDidMount、componentDidUpdate 这些回调函数。  

4. useEffect() 还会返回一个回调函数，这个回调函数的执行时机很重要。同useEffect 的调用时机是挂钩的。这个回调函数在组件渲染前调用。严格来说，在前一次的渲染视图被清除之前。相当于 componentWillUnmount。

## 2. 如何获取组件实例

1. 使用 ref 属性。ref 属性可以获取组件中某个元素，如：`<input type="text" ref={input => this.msgInput = input}/>`，就可以通过this.msgInput来得到对应的真实DOM元素。  

2. 下面的内容来自《React 进阶之路》：  
   - React组件也可以定义ref，此时ref的回调函数接收的参数是当前组件的实例，这提供了一种在组件外部操作组件的方式。  
具体代码如下：  
     ```javascript
        class AutoFocusTextInput extends React.Component {
              constructor(props) {
              super(props);
              this.blur = this.blur.bind(this);
        }
        componentDidMount() {
          // 通过ref让input自动获取焦点
          this.textInput.focus();
        }
        // 让input失去焦点
        blur() {
          this.textInput.blur();
        }
        render() {
           return (
              <div>
                <input
                   type="text"
                   ref={(input) => { this.textInput = input; }} />
              </div>
           );
        }
      }

      class Container extends React.Component {
        constructor(props) {
          super(props);
          this.handleClick = this.handleClick.bind(this);
        }
        handleClick() {
          // 通过ref调用AutoFocusTextInput组件的方法
          this.inputInstance.blur();
        }
        render() {
          return (
            <div>
              <AutoFocusTextInput ref={(input) => {this.inputInstance = input}}/>
              <button onClick={this.handleClick}>失去焦点</button>
            </div>
          );
        }
      }
     ```
     在Container 组件内，通过 ref 就可以获取 AutoFocusTextInput 组件的实例对象，并将这个实例对象赋给了 inputInstance。这样就可以通过 inputInstance 调用 AutoFocusTextInput 中的相关方法。
注意，只能为类组件定义 ref 属性，而不能为函数组件定义 ref 属性。

  
## 3. react-router 原理

1. BrowserRouter
   - react-router 的核心组件 <BrowserRouter> 使用了 HTML5 提供的 history API (pushState, replaceState 和 popstate 事件) 来保持 UI 和 URL 的同步。
   - path 中不带 `#`，以 `/` 开头。
   - Router 会创建一个 history 对象，history 用来跟踪 URL，当URL 发生变化时，Router 的后代组件会重新渲染。React Router 中提供的其他组件可以通过 context 获取 history 对象，这也隐含说明了 React Router 中的其他组件必须作为 Router 组件的后代组件使用。但 Router 中只能有唯一的一个子元素。

2. HashRouter
   - 使用 URL 的 hash 部分（即 window.location.hash）来保持 UI 和 URL 的同步。  
   - path 中以 `#` 开头。  
   - 变化的只是 url 中的 hash 部分，因而浏览器认为这还是同一个 url，因此不会像服务器发送请求。
   
3. 路由组件的props对象会有新增加三个属性：location、match 和 history。这个三个属性用来获取 path、查询参数、修改地址栏中的 url 等。
   1. history  
      - 引入的是 history 第三方库，是 react-router 的主要依赖之一。提供了几种不同的管理会话历史的方案。
      - 有下面三个方案：   
         - `browser history` - A DOM-specific implementation, useful in web browsers that support the HTML5 history API
         - `hash history` - A DOM-specific implementation for legacy web browsers
         - `memory history` - An in-memory history implementation, useful in testing and non-DOM environments like React Native.
      - 主要属性：   
        - `length` - (number) The number of entries in the history stack  
        - `action` - (string) The current action (PUSH, REPLACE, or POP)   
        - `location` - (object) The current location. May have the following   properties:
          - `pathname` - (string) The path of the URL  
          - `search` - (string) The URL query string  
          - `hash` - (string) The URL hash fragment  
          - `state` - (object) location-specific state that was provided to e.g. 
      - 主要方法：    
        - `push(path, state)`   
        when this location was pushed onto the stack. Only available in browser and memory history.
        - `push(path, [state])`   
        Pushes a new entry onto the history stack
        - `replace(path, [state]) `   
        Replaces the current entry on the history stack
        - `go(n) ` 
        Moves the pointer in the history stack by n entries
        - `goBack() ` 
        Equivalent to go(-1)
        - `goForward()`  
        Equivalent to go(1)
        - `block(prompt)`  
         Prevents navigation (see the history docs)
      - history 容易发生变化，因此被推荐使用路由组件的 location 属性替代history.location。
   2. location
      - 路由组件的一个属性。用来获取当前组件所在的路径或者曾经的路径或者是将要到达的路径。
   3. match
      - match对象包含路径匹配信息。主要有下面几个属性：
        - params 
        从url中解析出param参数，是一个键值对。 
        - isExact
        - path
        - url
        
4. withRouter()
   - 将非路由组件包装成路由组件。应用场景是：一些非路由，但是还需要使用路由组件的一些属性，如 location、match。所以使用这个函数将非路由组件包装为路由组件。

     

