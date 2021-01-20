# React 组件的属性

## 1. 基本说明

1. React 无论是 函数组件还是类组件，都拥有一些组件属性，可以给组件添加一些额外的功能，方便我们开发和调试。

2. 参考资料
   - [Class 属性 - defaultProps](https://zh-hans.reactjs.org/docs/react-component.html#defaultprops)
   - [React.Component](https://zh-hans.reactjs.org/docs/react-component.html)
   - [Class 属性 - displayName](https://zh-hans.reactjs.org/docs/react-component.html#displayname)

## 2. 属性

### 1. state
### 2. props
### 3. defaultProps

1. defaultProps 可以为组件添加默认 props。这一般用于 props 未赋值，但又不能为 null 的情况。

2. 示例 -- 类组件
   ```jsx
      class CustomButton extends React.Component {
          // ...
      }

      CustomButton.defaultProps = {
          color: 'blue'
      };
   ```

3. 示例 -- 函数组件
   ```jsx
      function MyButton(props) {
          return <Button>

          </Button>
      }

      MyButton.defaultProps = {
          size: 'large'
      }
   ```
### 4. displayName

1. displayName 字符串多用于调试消息。通常你不需要设置它，因为它可以根据函数组件或 class 组件的名称推断出来。

2. 可以用来标识组件。

3. 从组件的的 type 属性中可以拿到 displayName。

4. 示例：
   ```jsx
      function MenuItem(props) {
          // ...
      }

      MenuItem.displayName = 'MenuItem';

      // 使用
      function Menu(props) {
          React.Children.map(children, function(child, index) {
              const newChild = child as React.FunctionComponentElement<IBasicMenuProps>;
              const {displayName} = newChild.type;

              if (displayName === 'MenuItem') {
                 // return newChild;s
                 return React.cloneElement(newChild, {
                     index: index
                 })

             } else {
                 console.error('warning: Menu has a child which is not a valid MenuItem Component');
             }
      }
   ```