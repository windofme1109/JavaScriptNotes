# Babel 转换 TypeScript

## 1. 参考资料

1. [@babel/preset-typescript](https://babeljs.io/docs/en/babel-preset-typescript)

## 2. 基本用法

1. Babel 使用 `@babel/preset-typescript` 完成对
   TypeScript 的转换。实际上，是将 TS 代码 转换为 ES6 代码，举例如下：
    - TS 代码：
      `const x: number = 10;`
    - 转换后的 ES6 代码：
      `const x = 10;`
    
2. 安装 `@babel/preset-typescript`： `npm install --save-dev @babel/preset-typescript`

3. 安装完成以后，我们需要在`.babelrc.json`文件中进行配置：
   ```json
      {
          "presets": [
        [      
                  "@babel/preset-env",
                  {
                      "useBuiltIns": "usage",
                      "modules": "auto",
                      "corejs": "3.19.0"
                  }
              ],
              [
                  "@babel/preset-typescript"

              ]
          ]
      }
   ```
   注意，`@babel/preset-typescript` 这个插件要放在`@babel/preset-env` 的后面。

4. 转换 typescript 的示例如下：
   ```typescript
      const x: number = 10;

      const s:string = 'hello world';

      function concat(x: string, y:string):string {
      return x.concat(y);
      }


      class Animal<T> {
          private name: string;
          private size: T;

          constructor(name: string, size: T) {

              this.name = name;
              this.size = size;
          }
      }
   ```
5. 转换成 JavaScript 代码如下：
   ```js
      "use strict";

        require("core-js/modules/es.array.concat.js");

        require("core-js/modules/es.function.name.js");

        function _classCallCheck(instance, Constructor) { if (!(instance instanceof Constructor)) { throw new TypeError("Cannot call a class as a function"); } }

        var x = 10;
        var s = 'hello world';

        function concat(x, y) {
            return x.concat(y);
        }

        var Animal = function Animal(name, size) {
            _classCallCheck(this, Animal);

            this.name = name;
             this.size = size;
        };

   ```

## 3. 转换 TSX 

1. `@babel/preset-typescript` 也可以转换 tsx 代码，只需要对 `@babel/preset-typescript` 进行一些配置。

2. 对 `@babel/preset-typescript` 的配置是配置 `isTSX` 和 `allExtensions` 这两个字段。在 `.babelrc.json` 中将其配置为 `true`，如下所示：
   ```json
      {
          "presets": [
        
              [
                  "@babel/preset-typescript",
                  {
                      "isTSX": true,
                      "allExtensions": true
                  }
              ]
          ]
      }
   ```

3. 基本的转换过程就是：tsx -> jsx -> js。因此除了 `@babel/preset-typescript` 和 `@babel/preset-env`，还需要 `@babel/preset-react`。每一个 preset 的作用如下：
   - `@babel/preset-typescript`：将 tsx 代码转换为 jsx 代码。
   - `@babel/preset-react`：将 jsx 代码转换为 js 代码。
   - `@babel/preset-env`：将 js 代码转换适合在目标环境运行的代码。

4. 因此完整的配置是：
   ```json
      {
          "presets": [
              [
                  "@babel/preset-env",
                  {
                      "useBuiltIns": "usage",
                      "modules": "auto",
                      "corejs": "3.19.0"
                  }
              ],
              ["@babel/preset-react"],
              [
                  "@babel/preset-typescript",
                  {
                      "isTSX": true,
                      "allExtensions": true
                  }
              ]
          ]
      }
   ```
5. 原始的 tsx 代码：
   ```tsx
      import React, {FC, Fragment} from 'react';

      interface AppProps {
          title:string
      }

      const App: FC<AppProps> = (props) => {
          const {title, ...restProps} = props;
          return (
              <Fragment>
                  <div>
                     <h1>{title}</h1>
                  </div>
              </Fragment>
         )

      }

      export default App;
   ```
6. 转换后的 js 代码：
   ```js
      "use strict";

       function _typeof(obj) { "@babel/helpers - typeof"; if (typeof Symbol === "function" && typeof Symbol.iterator === "symbol") { _typeof = function _typeof(obj) { return typeof obj; }; } else { _typeof = function _typeof(obj) { return obj && typeof Symbol === "function" && obj.constructor === Symbol && obj !== Symbol.prototype ? "symbol" : typeof obj; }; } return _typeof(obj); }

       require("core-js/modules/es.object.keys.js");

       require("core-js/modules/es.array.index-of.js");

       require("core-js/modules/es.symbol.js");

       require("core-js/modules/es.array.iterator.js");

       require("core-js/modules/es.object.to-string.js");

       require("core-js/modules/es.string.iterator.js");

       require("core-js/modules/es.weak-map.js");

       require("core-js/modules/web.dom-collections.iterator.js");

        require("core-js/modules/es.object.define-property.js");

        require("core-js/modules/es.object.get-own-property-descriptor.js");

        require("core-js/modules/es.symbol.description.js");

        require("core-js/modules/es.symbol.iterator.js");

        Object.defineProperty(exports, "__esModule", {
           value: true
        });
        exports["default"] = void 0;

       var _react = _interopRequireWildcard(require("react"));

       var _excluded = ["title"];

      function _getRequireWildcardCache(nodeInterop) { if (typeof WeakMap !== "function") return null; var cacheBabelInterop = new WeakMap(); var cacheNodeInterop = new WeakMap(); return (_getRequireWildcardCache = function _getRequireWildcardCache(nodeInterop) { return nodeInterop ? cacheNodeInterop : cacheBabelInterop; })(nodeInterop); }

      function _interopRequireWildcard(obj, nodeInterop) { if (!nodeInterop && obj && obj.__esModule) { return obj; } if (obj === null || _typeof(obj) !== "object" && typeof obj !== "function") { return { "default": obj }; } var cache = _getRequireWildcardCache(nodeInterop); if (cache && cache.has(obj)) { return cache.get(obj); } var newObj = {}; var hasPropertyDescriptor = Object.defineProperty && Object.getOwnPropertyDescriptor; for (var key in obj) { if (key !== "default" && Object.prototype.hasOwnProperty.call(obj, key)) { var desc = hasPropertyDescriptor ? Object.getOwnPropertyDescriptor(obj, key) : null; if (desc && (desc.get || desc.set)) { Object.defineProperty(newObj, key, desc); } else { newObj[key] = obj[key]; } } } newObj["default"] = obj; if (cache) { cache.set(obj, newObj); } return newObj; }

      function _objectWithoutProperties(source, excluded) { if (source == null) return {}; var target = _objectWithoutPropertiesLoose(source, excluded); var key, i; if (Object.getOwnPropertySymbols) { var sourceSymbolKeys = Object.getOwnPropertySymbols(source); for (i = 0; i < sourceSymbolKeys.length; i++) { key = sourceSymbolKeys[i]; if (excluded.indexOf(key) >= 0) continue; if (!Object.prototype.propertyIsEnumerable.call(source, key)) continue; target[key] = source[key]; } } return target; }

      function _objectWithoutPropertiesLoose(source, excluded) { if (source == null) return {}; var target = {}; var sourceKeys = Object.keys(source); var key, i; for (i = 0; i < sourceKeys.length; i++) { key = sourceKeys[i]; if (excluded.indexOf(key) >= 0) continue; target[key] = source[key]; } return target; }

      var App = function App(props) {
      var title = props.title,
      restProps = _objectWithoutProperties(props, _excluded);

      return /*#__PURE__*/_react["default"].createElement(_react.Fragment, null, /*#__PURE__*/_react["default"].createElement("div", null, /*#__PURE__*/_react["default"].createElement("h1", null, title)));
      };

      var _default = App;
      exports["default"] = _default;
   ```
7. tsx 代码被转换成 js 代码。如果我们不使用 `@babel/preset-react`，那么只会将 tsx 代码转换成 jsx 代码，如下所示：
   ```jsx
      "use strict";

       function _typeof(obj) { "@babel/helpers - typeof"; if (typeof Symbol === "function" && typeof Symbol.iterator === "symbol") { _typeof = function _typeof(obj) { return typeof obj; }; } else { _typeof = function _typeof(obj) { return obj && typeof Symbol === "function" && obj.constructor === Symbol && obj !== Symbol.prototype ? "symbol" : typeof obj; }; } return _typeof(obj); }

       require("core-js/modules/es.object.keys.js");

       require("core-js/modules/es.array.index-of.js");

       require("core-js/modules/es.symbol.js");

       require("core-js/modules/es.array.iterator.js");

       require("core-js/modules/es.object.to-string.js");

       require("core-js/modules/es.string.iterator.js");

       require("core-js/modules/es.weak-map.js");

       require("core-js/modules/web.dom-collections.iterator.js");

       require("core-js/modules/es.object.define-property.js");

       require("core-js/modules/es.object.get-own-property-descriptor.js");

       require("core-js/modules/es.symbol.description.js");

       require("core-js/modules/es.symbol.iterator.js");

       Object.defineProperty(exports, "__esModule", {
       value: true
       });
       exports["default"] = void 0;

       var _react = _interopRequireWildcard(require("react"));

       var _excluded = ["title"];

       function _getRequireWildcardCache(nodeInterop) { if (typeof WeakMap !== "function") return null; var cacheBabelInterop = new WeakMap(); var cacheNodeInterop = new WeakMap(); return (_getRequireWildcardCache = function _getRequireWildcardCache(nodeInterop) { return nodeInterop ? cacheNodeInterop : cacheBabelInterop; })(nodeInterop); }

        function _interopRequireWildcard(obj, nodeInterop) { if (!nodeInterop && obj && obj.__esModule) { return obj; } if (obj === null || _typeof(obj) !== "object" && typeof obj !== "function") { return { "default": obj }; } var cache = _getRequireWildcardCache(nodeInterop); if (cache && cache.has(obj)) { return cache.get(obj); } var newObj = {}; var hasPropertyDescriptor = Object.defineProperty && Object.getOwnPropertyDescriptor; for (var key in obj) { if (key !== "default" && Object.prototype.hasOwnProperty.call(obj, key)) { var desc = hasPropertyDescriptor ? Object.getOwnPropertyDescriptor(obj, key) : null; if (desc && (desc.get || desc.set)) { Object.defineProperty(newObj, key, desc); } else { newObj[key] = obj[key]; } } } newObj["default"] = obj; if (cache) { cache.set(obj, newObj); } return newObj; }

        function _objectWithoutProperties(source, excluded) { if (source == null) return {}; var target = _objectWithoutPropertiesLoose(source, excluded); var key, i; if (Object.getOwnPropertySymbols) { var sourceSymbolKeys = Object.getOwnPropertySymbols(source); for (i = 0; i < sourceSymbolKeys.length; i++) { key = sourceSymbolKeys[i]; if (excluded.indexOf(key) >= 0) continue; if (!Object.prototype.propertyIsEnumerable.call(source, key)) continue; target[key] = source[key]; } } return target; }

        function _objectWithoutPropertiesLoose(source, excluded) { if (source == null) return {}; var target = {}; var sourceKeys = Object.keys(source); var key, i; for (i = 0; i < sourceKeys.length; i++) { key = sourceKeys[i]; if (excluded.indexOf(key) >= 0) continue; target[key] = source[key]; } return target; }

        var App = function App(props) {
            var title = props.title,
            restProps = _objectWithoutProperties(props, _excluded);

            return <_react.Fragment>
                  <div>
                        <h1>{title}</h1>
                  </div>
            </_react.Fragment>;
        };

        var _default = App;
       exports["default"] = _default;
   ```
8. 因此，转换 tsx 代码时，一定要配置上 `@babel/preset-react`。

## 4. `@babel/preset-typescript` 的配置说明

### 1.  `isTSX`

1. 布尔值，默认为 `false`。

2. 这个配置项的作用是强制启用 JSX 解析，否则会将尖括号 `<>` 看作 TS 的过时的类型断言：`var foo = <string>bar;`。如果设置 `isTSX` 为 `true`，则 `allExtensions` 也必须为 `true`。

### 2. `jsxPragma` 

1. 字符串，默认为 `react`。替换编译 JSX 表达式的时候使用函数。这样我们就知道导入（`import`）不是类型导入，不应该被移除。
### 3.  `allExtensions`

1. 布尔值，默认为 `false`。

2. 设置为 `true` 表示每个文件作为 TS 或者 TSX 格式解析。

### 4.  `allowNamespaces` 

1. 布尔值。

2. 作用是启用对 TS 的命名空间（`namespace`）的编译。要和 `@babel/plugin-transform-typescript` 插件配合使用。在 Babel 的 `v7.6.0` 版本中引入。

### 5.  `allowDeclareFields` 

1. 布尔值，默认为 `false`。

2. 在 Babel 的 `v7.7.0` 版本中引入。设置为 `true` 的时候，使用 `declare` 声明的类变量会被移除。示例如下：
      ```typescript
           class A {
             declare foo: string; // Removed
             bar: string; // Initialized to undefined
             prop?: string; // Initialized to undefined
             prop1!: string // Initialized to undefined
           }
      ```
   注意：这个配置项在 Babel 8 中会被默认启用。

### 6.  `onlyRemoveTypeImports` 

1. 布尔值，默认为 `false`。

2. 这个配置项的作用是只移除 `type-only` 类型的导入（`import`）。当我们使用 TypeScript >= 3.8 版本才可以使用这个配置项。   `type-only imports` 说明：[ type-only imports](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-8.html#type-only-imports-exports)
