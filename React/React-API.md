<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [React API](#react-api)
  - [1. 基本说明](#1-%E5%9F%BA%E6%9C%AC%E8%AF%B4%E6%98%8E)
  - [2. API 参考](#2-api-%E5%8F%82%E8%80%83)
    - [1. React.Children.map](#1-reactchildrenmap)
    - [2. React.cloneElement](#2-reactcloneelement)
    - [3. ReactDOM.createPortal](#3-reactdomcreateportal)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# React API

## 1. 基本说明

1. React 顶层的 API，即可以通过 React 直接调用的 API。

2. 参考资料：
   - [React 顶层 API](https://zh-hans.reactjs.org/docs/react-api.html)

3. 使用方式
   ```jsx
     import React from 'react';
   ```
   - 可以使用 `React` 这个全局对象调用各种顶层 API 了。

## 2. API 参考

### 1. React.Children.map

1. 调用形式：`React.Children.map(children, function[child, index])`

2. 参数说明：
   - 第一个参数是 children，表示子节点。第二个参数是一个回调函数
   - 回调函数接收两个参数，第一个参数是子节点，第二个参数是索引

3. 作用：children 表示所有的子节点，如果 children 是一个数组，它将被遍历并为数组中的每个子节点调用该回调函数。如果子节点为 null 或是 undefined，则此方法将返回 null 或是 undefined，而不会返回数组

4. **注意**：如果 children 是一个 Fragment 对象，它将被视为单一子节点的情况处理，而不会被遍历。

### 2. React.cloneElement

1. 调用形式：`React.cloneElement(element, [props], [...children])`

2. 参数说明：
   - element 是被拷贝的元素
   - props 需要添加到拷贝后的元素的 props 属性
   - children 向被拷贝的元素添加 children 子节点
3. 作用：以 element 元素为样板克隆并返回新的 React 元素。返回元素的 props 是将新的 props 与原始元素的 props 浅层合并后的结果。新的子元素将取代现有的子元素，而来自原始元素的 key 和 ref 将被保留

### 3. ReactDOM.createPortal


1. 参考资料：
   - [react portal](https://www.jianshu.com/p/0771f1643aa3)
   - [react 插槽(Portals)](https://www.cnblogs.com/yadiblogs/p/10121538.html)
   - [Portals](https://reactjs.org/docs/portals.html)

2. 调用形式：`ReactDOM.createPortal(child, container)`

3. 参数说明：
   - child 要渲染的子元素
   - container 容器元素，任何一个有效的 DOM 节点都可以作为 child 的容器

4. 这个函数的主要作用是实现将子节点渲染到父组件DOM层次结构之外的DOM节点。

5. 对于 portal 的一个典型用例是当父组件有 `overflow: hidden` 或 `z-index` 样式，但你需要子组件能够在视觉上 “跳出(break out)” 所在的容器。例如，对话框、hovercards 以及提示框。所以一般 react 组件里的模态框，就是这样实现的，都会使用 createPortal() 将其挂载到 body 元素下层。

6. 使用 createPortal() 可以保留节点的原来的上下文信息。虽然节点的渲染位置变了，但是节点原有的父节点等上下文信息不变。

7. 事件冒泡和普通 react 子节点一样，是因为 portal 仍然存在于 React tree 中，而不用考虑其在真实的 DOM tree 中的位置。

8. 示例
   ```jsx
      import React, {FC, Fragment, Component} from 'react';
      import ReactDOM from 'react-dom';
      interface IPortalProps {
      }

      export default class Portal extends Component<IPortalProps> {
          private node: HTMLDivElement;
          constructor(props: IPortalProps) {
              super(props);
              this.node = document.createElement('div');
              document.body.appendChild(this.node);
          }

          render() {
              const {children} = this.props;

              return ReactDOM.createPortal(
                  children,
                  this.node
              );
          }
      }
   ```

