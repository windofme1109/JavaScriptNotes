<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Babel 打包 React](#babel-%E6%89%93%E5%8C%85-react)
  - [1. 参考资料](#1-%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99)
  - [2. 基本用法](#2-%E5%9F%BA%E6%9C%AC%E7%94%A8%E6%B3%95)
  - [3. `@babel/preset-react` 的配置项说明](#3-babelpreset-react-%E7%9A%84%E9%85%8D%E7%BD%AE%E9%A1%B9%E8%AF%B4%E6%98%8E)
    - [1. Both Runtimes](#1-both-runtimes)
      - [1. runtime](#1-runtime)
      - [2. development](#2-development)
      - [3. throwIfNamespace](#3-throwifnamespace)
    - [2. React Automatic Runtime](#2-react-automatic-runtime)
      - [1. importSource](#1-importsource)
    - [3. React Classic Runtime](#3-react-classic-runtime)
      - [1. pragma](#1-pragma)
      - [2. pragmaFrag](#2-pragmafrag)
      - [3. useBuiltIns](#3-usebuiltins)
      - [4. useSpread](#4-usespread)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Babel 打包 React

## 1. 参考资料

1. [@babel/preset-react](https://babeljs.io/docs/en/babel-preset-react)

## 2. 基本用法

1. Babel 转换 React，主要是将 JSX 语法书写的代码转换成普通的 js 语法书写的代码。完成这样的转换还需要另外一个 Babel 的预设（preset）模块：`@babel/preset-react`。

2. 安装 `@babel/preset-react`：`npm install --save-dev @babel/preset-react`

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
              ["@babel/preset-react"]
          ]
      }
   ```
   把`@babel/preset-react` 放在 `presets` 这个字段下。

4. **注意**：`@babel/preset-react` 这个配置必须在 `@babel/preset-env` 的后面。因为执行顺序是从下到上的。也就是先通过 `@babel/preset-react` 将 JSX 语法转换为 ES6，在通过 `@babel/preset-env` 将 ES6 转换为 ES5。

5. 示例如下 - JSX：
   ```jsx
      import React, {Component, Fragment} from 'react' ;

       import ReactDOM, {render} from 'react-dom';

       export default class App extends Component {
           render() {

               return (
                   <Fragment>
                       <h1>hello world</h1>
                   </Fragment>
               )
           }
       }

       ReactDOM.render(<App />, document.querySelector('#root'));
   ```
6. 编译后的代码：
   ```js
      "use strict";

       function _typeof(obj) { "@babel/helpers - typeof"; if (typeof Symbol === "function" && typeof Symbol.iterator === "symbol") { _typeof = function _typeof(obj) { return typeof obj; }; } else { _typeof = function _typeof(obj) { return obj && typeof Symbol === "function" && obj.constructor === Symbol && obj !== Symbol.prototype ? "symbol" : typeof obj; }; } return _typeof(obj); }

       require("core-js/modules/es.reflect.construct.js");

       require("core-js/modules/es.object.create.js");

       require("core-js/modules/es.object.define-property.js");

       require("core-js/modules/es.array.iterator.js");

       require("core-js/modules/es.object.to-string.js");

       require("core-js/modules/es.string.iterator.js");

       require("core-js/modules/es.weak-map.js");

       require("core-js/modules/web.dom-collections.iterator.js");

       require("core-js/modules/es.object.get-own-property-descriptor.js");

       require("core-js/modules/es.symbol.js");

       require("core-js/modules/es.symbol.description.js");

       require("core-js/modules/es.symbol.iterator.js");

       Object.defineProperty(exports, "__esModule", {
       value: true
       });
       exports["default"] = void 0;

        require("core-js/modules/es.object.set-prototype-of.js");

        require("core-js/modules/es.object.get-prototype-of.js");

        var _react = _interopRequireWildcard(require("react"));

        var _reactDom = _interopRequireWildcard(require("react-dom"));

        function _getRequireWildcardCache(nodeInterop) { if (typeof WeakMap !== "function") return null; var cacheBabelInterop = new WeakMap(); var cacheNodeInterop = new WeakMap(); return (_getRequireWildcardCache = function _getRequireWildcardCache(nodeInterop) { return nodeInterop ? cacheNodeInterop : cacheBabelInterop; })(nodeInterop); }

        function _interopRequireWildcard(obj, nodeInterop) { if (!nodeInterop && obj && obj.__esModule) { return obj; } if (obj === null || _typeof(obj) !== "object" && typeof obj !== "function") { return { "default": obj }; } var cache = _getRequireWildcardCache(nodeInterop); if (cache && cache.has(obj)) { return cache.get(obj); } var newObj = {}; var hasPropertyDescriptor = Object.defineProperty && Object.getOwnPropertyDescriptor; for (var key in obj) { if (key !== "default" && Object.prototype.hasOwnProperty.call(obj, key)) { var desc = hasPropertyDescriptor ? Object.getOwnPropertyDescriptor(obj, key) : null; if (desc && (desc.get || desc.set)) { Object.defineProperty(newObj, key, desc); } else { newObj[key] = obj[key]; } } } newObj["default"] = obj; if (cache) { cache.set(obj, newObj); } return newObj; }

        function _classCallCheck(instance, Constructor) { if (!(instance instanceof Constructor)) { throw new TypeError("Cannot call a class as a function"); } }

        function _defineProperties(target, props) { for (var i = 0; i < props.length; i++) { var descriptor = props[i]; descriptor.enumerable = descriptor.enumerable || false; descriptor.configurable = true; if ("value" in descriptor) descriptor.writable = true; Object.defineProperty(target, descriptor.key, descriptor); } }

        function _createClass(Constructor, protoProps, staticProps) { if (protoProps) _defineProperties(Constructor.prototype, protoProps); if (staticProps) _defineProperties(Constructor, staticProps); return Constructor; }

        function _inherits(subClass, superClass) { if (typeof superClass !== "function" && superClass !== null) { throw new TypeError("Super expression must either be null or a function"); } subClass.prototype = Object.create(superClass && superClass.prototype, { constructor: { value: subClass, writable: true, configurable: true } }); if (superClass) _setPrototypeOf(subClass, superClass); }

        function _setPrototypeOf(o, p) { _setPrototypeOf = Object.setPrototypeOf || function _setPrototypeOf(o, p) { o.__proto__ = p; return o; }; return _setPrototypeOf(o, p); }

        function _createSuper(Derived) { var hasNativeReflectConstruct = _isNativeReflectConstruct(); return function _createSuperInternal() { var Super = _getPrototypeOf(Derived), result; if (hasNativeReflectConstruct) { var NewTarget = _getPrototypeOf(this).constructor; result = Reflect.construct(Super, arguments, NewTarget); } else { result = Super.apply(this, arguments); } return _possibleConstructorReturn(this, result); }; }

        function _possibleConstructorReturn(self, call) { if (call && (_typeof(call) === "object" || typeof call === "function")) { return call; } else if (call !== void 0) { throw new TypeError("Derived constructors may only return object or undefined"); } return _assertThisInitialized(self); }

        function _assertThisInitialized(self) { if (self === void 0) { throw new ReferenceError("this hasn't been initialised - super() hasn't been called"); } return self; }

        function _isNativeReflectConstruct() { if (typeof Reflect === "undefined" || !Reflect.construct) return false; if (Reflect.construct.sham) return false; if (typeof Proxy === "function") return true; try { Boolean.prototype.valueOf.call(Reflect.construct(Boolean, [], function () {})); return true; } catch (e) { return false; } }

        function _getPrototypeOf(o) { _getPrototypeOf = Object.setPrototypeOf ? Object.getPrototypeOf : function _getPrototypeOf(o) { return o.__proto__ || Object.getPrototypeOf(o); }; return _getPrototypeOf(o); }

        var App = /*#__PURE__*/function (_Component) {
             _inherits(App, _Component);

             var _super = _createSuper(App);

             function App() {
             _classCallCheck(this, App);

             return _super.apply(this, arguments);
        }

        _createClass(App, [{
            key: "render",
            value: function render() {
                 return /*#__PURE__*/_react["default"].createElement(_react.Fragment, null, /*#__PURE__*/_react["default"].createElement("h1", null, "hello world"));
            }
        }]);

         return App;
      }(_react.Component);

      exports["default"] = App;

      _reactDom["default"].render( /*#__PURE__*/_react["default"].createElement(App, null), document.querySelector('#root'));

   ```

## 3. `@babel/preset-react` 的配置项说明

### 1. Both Runtimes

#### 1. runtime

1. 可选值为 `classic` 或者 `automatic`，默认是 `classic`。

2. 这个配置项用来决定使用哪个运行时。

3. 配置为 `automatic`，会自动导入转换 JSX 语法过程中需要的函数。

4. 配置为 `classic`，不会导入任何东西。也就是说，只有使用`React.component` 的时候才会生效。

#### 2. development

1. 布尔值，默认是 false。

2. 这个开关属性用来指定是否为开发环境，如果是开发环境，会加入 `__source` 和 `__self` 之类的属性。

3. 当我们在 `.babelrc.json` 配置了 `env` 字段时，配置这个 `development` 字段比较有用。

#### 3. throwIfNamespace

1. 布尔值，默认是 true。

2. 如果一个 XML 命名空间标签名字已经被使用，那么就会抛出异常，举例如下：`<f:image />`。

3. JSX 标准允许这种写法，实际上是被禁止的，因为 React 中的 JSX 当前不支持这种写法。

### 2. React Automatic Runtime

#### 1. importSource

1. 字符串，默认是 `react`。

2. 当导入函数时，用来替换导入的资源。

3. 不太明白这个字段的作用。

### 3. React Classic Runtime

#### 1. pragma

1. 字符串，默认是 `React.createElement`。

2. 这个配置项的作用就是，使用 `React.createElement` 来生成 JSX 表达式表示的元素。 

3. 还可以设置为 `dom`。此时 `runtime` 字段必须设置为 `classic`。

#### 2. pragmaFrag

1. 字符串，默认是 `React.Fragment`。

2. 如果组件使用 `Fragment` 表达式，那么编译 JSX 的过程中，会使用 `React.Fragment` 进行替换。

3. 还可以设置为 `DomFragment`。此时 `runtime` 字段必须设置为 `classic`。

#### 3. useBuiltIns

1. 布尔值，默认是 false。

2. 如果设置为 true 起到的作用是：如果有的插件需要引入 polyfill，那么会使用原生内置的功能替代这个 polyfill。

#### 4. useSpread

1. 布尔值，默认是 false。

2. 设置为 true 的作用是：当我们使用扩展语法来展开 props 时，对扩展的元素直接使用行内对象来取代 Babel 的扩展 helper 函数或者 `Object.assign` 方法。

3. 举个例子：
   ```jsx
      export default class App extends Component {
             render() {
                 const obj = {
                     style: {},

                 }
                 return (
                     <Fragment>
                         <div  className="test-1" onClick={() => console.log(123)} {...obj}>
                             <h1>hello world</h1>
                         </div>

                     </Fragment>
                 )
             }
         }
   ```
   我们使用扩展语法，将 obj 对象展开，作为 div 的 props 属性。

4. 当我们不设置 useSpread 属性时（默认为 false），编译结果如下（部分）：
   ```js
      var App = /*#__PURE__*/function (_Component) {
          _inherits(App, _Component);

          var _super = _createSuper(App);

          function App() {
          _classCallCheck(this, App);

              return _super.apply(this, arguments);
          }

           _createClass(App, [{
               key: "render",
               value: function render() {
                     var obj = {
                        style: {}
                      };
                     return /*#__PURE__*/_react["default"].createElement(_react.Fragment, null, /*#__PURE__*/_react["default"].createElement("div", _extends({
             className: "test-1",
             onClick: function onClick() {
                  return console.log(123);
             }
             }, obj), /*#__PURE__*/_react["default"].createElement("h1", null, "hello world")));
              }
           }]);

           return App;
      }(_react.Component);
   ```
   `_extends` 是 `Babel` 提供的 helper 函数，用来组合不同的属性到一个对象中，其内部使用了 `Object.assign`。其实现如下：
    ```js
       function _extends() {
            _extends = Object.assign || function (target) {
                for (var i = 1; i < arguments.length; i++) {
                    var source = arguments[i];
                    for (var key in source) {
                        if (Object.prototype.hasOwnProperty.call(source, key)) {
                            target[key] = source[key];
                        }
                    }
                }
                return target;
            };
            return _extends.apply(this, arguments);
        }
    ``` 
5. 设置 useSpread 属性为 true 的时候，编译结果如下：
   ```js
      var App = /*#__PURE__*/function (_Component) {
           _inherits(App, _Component);

           var _super = _createSuper(App);

           function App() {
           _classCallCheck(this, App);

               return _super.apply(this, arguments);
           }

           _createClass(App, [{
           key: "render",
           value: function render() {
           var obj = {
              style: {}
           };
           return /*#__PURE__*/_react["default"].createElement(_react.Fragment, null, /*#__PURE__*/_react["default"].createElement("div", _objectSpread({
               className: "test-1",
               onClick: function onClick() {
                   return console.log(123);
               }
               }, obj), /*#__PURE__*/_react["default"].createElement("h1", null, "hello world")));
              }
       }]);

          return App;
      }(_react.Component);
   ```
   _`objectSpread` 函数将 `obj` 内部的属性和 `div` 标签已有的属性合并为 `div` 的 `props`，实现如下：
   ```js
      function _objectSpread(target) {
          for (var i = 1; i < arguments.length; i++) {
              var source = arguments[i] != null ? arguments[i] : {};
              if (i % 2) {
                  ownKeys(Object(source), true).forEach(function (key) {
                      _defineProperty(target, key, source[key]);
                  });
              } else if (Object.getOwnPropertyDescriptors) {
                  Object.defineProperties(target, Object.getOwnPropertyDescriptors(source));
              } else {
                  ownKeys(Object(source)).forEach(function (key) {
                      Object.defineProperty(target, key, Object.getOwnPropertyDescriptor(source, key));
                  });
              }
          }
          return target;
      }
   ```
   使用的是原生的 Object 对属性进行组合。