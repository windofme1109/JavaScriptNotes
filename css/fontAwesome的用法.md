# FontAwesome 的使用说明

### 1. 基本说明

1. FontAwesome 在 v5 版本中使用了 svg 作为图标的基础，而不是 Font Icon。

2. 参考资料：
   - [react-fontawesome](https://github.com/FortAwesome/react-fontawesome)
   - [Font Awesome React](https://fontawesome.com/how-to-use/on-the-web/using-with/react)


### 2. 安装

1. `npm i --save @fortawesome/fontawesome-svg-core`

2. `npm install --save @fortawesome/free-solid-svg-icons`

3. `npm i --save @fortawesome/react-fontawesome`


4. `npm install --save @fortawesome/free-brands-svg-icons`

### 3. 使用

1. 单独使用和全局使用的优缺点

   使用方式|优点|缺点
   |:---:|:---:|:---:|
   单独使用|组件内使用，只打包使用的图标|当有较多的组件时，单独导出图标比较麻烦
   全局使用|在初始化的模块（App）中单独导入一次，将图标加入图标库中，就不需要在每个组件中导入图标|会打包不需要的图标，影响性能

#### 1. 单独使用

1. 示例代码：
   ```javascript
      import ReactDOM from 'react-dom'
      import { FontAwesomeIcon } from '@fortawesome/react-fontawesome'
      import { faCoffee } from '@fortawesome/free-solid-svg-icons'

      const element = <FontAwesomeIcon icon={faCoffee} />

      ReactDOM.render(element, document.body)
   ```

#### 2. 全局使用

1. 在 app.js 中：
   ```javascript
      import ReactDOM from 'react-dom';
      import { library } from '@fortawesome/fontawesome-svg-core';
      import { fab } from '@fortawesome/free-brands-svg-icons';
      import { faCheckSquare, faCoffee } from '@fortawesome/free-solid-svg-icons';

    library.add(fab, faCheckSquare, faCoffee)
   ```
2. 一般我们在 app.js 中引入图标库：`library`。

3. 我们向 `library` 中传入两个内容：
   - fab：表示 `@fortawesome/free-brands-svg-icons` 模块中的所有图标。因此可以使用字符串形式的图标名称引用图标。
     > fab: which represents all of the brand icons in @fortawesome/free-brands-svg-icons. So any of the brand icons in that package may be referenced by icon name as a string anywhere else in our app. For example: "apple", "microsoft", or "google".
   - faCheckSquare and faCoffee：我们可以在组件中通过字符串引用的图标。
     > faCheckSquare and faCoffee: Adding each of these icons individually allows us to refer to them throughout our app by their icon string names, "check-square" and "coffee", respectively.
    
    - 如果我们想使用 `@fortawesome/free-solid-svg-icons` 这个模块下的所有图标，而不是一个一个引入，可以这样用：`import { fas } from '@fortawesome/free-solid-svg-icons';` `fas` 与前面的 `fab` 是一样的，表示 `@fortawesome/free-solid-svg-icons` 模块下的所有图标，因此可以使用字符串形式的图标名称引用图标。

4. 示例 -- 同时引入 `@fortawesome/free-solid-svg-icons` 和 `@fortawesome/free-brands-svg-icons` 下的所有图标：
   ```javascript
      import ReactDOM from 'react-dom';
      import { library } from '@fortawesome/fontawesome-svg-core';
      import { fab } from '@fortawesome/free-brands-svg-icons';
      import { fas } from '@fortawesome/free-solid-svg-icons';

       library.add(fab, fas)
   ```
4. 在组件中使用
   ```javascript
      import React from 'react'
      import { FontAwesomeIcon } from '@fortawesome/react-fontawesome'

      export const Beverage = () => (
        <div>
          <FontAwesomeIcon icon="check-square" />
          Your <FontAwesomeIcon icon="coffee" /> is hot and ready!
        </div>
      )
   ```
#### 3. 图标属性

1. Size
   ```jsx
      <FontAwesomeIcon icon="coffee" size="xs" />
      <FontAwesomeIcon icon="coffee" size="lg" />
      <FontAwesomeIcon icon="coffee" size="6x" />
   ```

2. [Fixed-Width Icons](https://fontawesome.com/how-to-use/on-the-web/styling/fixed-width-icons)
   ```jsx
      <FontAwesomeIcon icon="coffee" fixedWidth />
   ```

3. Inverse
   ```jsx
      <FontAwesomeIcon icon="coffee" inverse />
   ```

4. [Icons in a List](https://fontawesome.com/how-to-use/on-the-web/styling/icons-in-a-list)
   ```jsx
      <FontAwesomeIcon icon="coffee" listItem /> 
   ```


5. [Rotating Icons](https://fontawesome.com/how-to-use/on-the-web/styling/rotating-icons)
   ```jsx
      <FontAwesomeIcon icon="coffee" rotation={90} />
      <FontAwesomeIcon icon="coffee" rotation={180} />
      <FontAwesomeIcon icon="coffee" rotation={270} /> 
   ```


6. Flip horizontally, vertically, or both:
   ```jsx
      <FontAwesomeIcon icon="coffee" flip="horizontal" />
      <FontAwesomeIcon icon="coffee" flip="vertical" />
      <FontAwesomeIcon icon="coffee" flip="both" />
   ```


7. [Animating Icons](https://fontawesome.com/how-to-use/on-the-web/styling/animating-icons)
   ```jsx
      <FontAwesomeIcon icon="spinner" spin />
      <FontAwesomeIcon icon="spinner" pulse />  
   ```


8. [Bordered Icons](https://fontawesome.com/how-to-use/on-the-web/styling/bordered-pulled-icons)
   ```jsx
      <FontAwesomeIcon icon="coffee" border />
   ```


9. [Pulled Icons](https://fontawesome.com/how-to-use/on-the-web/styling/bordered-pulled-icons)
   ```jsx
      <FontAwesomeIcon icon="coffee" pull="left" />
      <FontAwesomeIcon icon="coffee" pull="right" />
   ``` 


10. [Swap Opacity](https://fontawesome.com/how-to-use/on-the-web/styling/duotone-icons)
    - This feature is only available when using Duotone Icons. 
    ```jsx
       <FontAwesomeIcon icon={['fad', 'coffee']} />
       <FontAwesomeIcon icon={['fad', 'coffee']} swapOpacity />
    ```



11. Adding Classes Yourself
    - You can add classes for your own project purposes and styling to any component using the className property.
    ```jsx
         <FontAwesomeIcon icon="spinner" className="highlight" />
    ```


12. [Power Transforms](https://fontawesome.com/how-to-use/on-the-web/styling/power-transforms)
    ```jsx
       <FontAwesomeIcon icon="coffee" transform="shrink-6 left-4" />
<FontAwesomeIcon icon="coffee" transform={{ rotate: 42 }} />
    ```


13. [Masking Icons](https://fontawesome.com/how-to-use/on-the-web/styling/masking)
    ```jsx
       <FontAwesomeIcon icon="coffee" mask={['far', 'circle']} />
    ```


14. [Layering Icons](https://fontawesome.com/how-to-use/on-the-web/styling/layering)
    ```jsx
       <span className="fa-layers fa-fw">
            <FontAwesomeIcon icon="square" color="green" />
            <FontAwesomeIcon icon="check" inverse transform="shrink-6" />
      </span>  
    ```


15. [Using SVG Symbols](https://fontawesome.com/how-to-use/on-the-web/advanced/svg-symbols)
    ```jsx
       <FontAwesomeIcon icon="coffee" symbol />
       <FontAwesomeIcon icon="coffee" symbol="beverage-icon" />
    ```





