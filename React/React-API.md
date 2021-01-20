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
