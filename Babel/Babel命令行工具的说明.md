<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Babel 的命令行工具的使用](#babel-%E7%9A%84%E5%91%BD%E4%BB%A4%E8%A1%8C%E5%B7%A5%E5%85%B7%E7%9A%84%E4%BD%BF%E7%94%A8)
  - [1. 参考资料](#1-%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99)
  - [2. Babel 命令行工具简介](#2-babel-%E5%91%BD%E4%BB%A4%E8%A1%8C%E5%B7%A5%E5%85%B7%E7%AE%80%E4%BB%8B)
  - [3. Babel 命令行工具的使用](#3-babel-%E5%91%BD%E4%BB%A4%E8%A1%8C%E5%B7%A5%E5%85%B7%E7%9A%84%E4%BD%BF%E7%94%A8)
    - [1. 编译单个文件](#1-%E7%BC%96%E8%AF%91%E5%8D%95%E4%B8%AA%E6%96%87%E4%BB%B6)
      - [1. `npx babel filename`](#1-npx-babel-filename)
    - [2. 编译某个文件夹](#2-%E7%BC%96%E8%AF%91%E6%9F%90%E4%B8%AA%E6%96%87%E4%BB%B6%E5%A4%B9)
    - [3. 忽略某些文件](#3-%E5%BF%BD%E7%95%A5%E6%9F%90%E4%BA%9B%E6%96%87%E4%BB%B6)
  - [4. 编译命令总结](#4-%E7%BC%96%E8%AF%91%E5%91%BD%E4%BB%A4%E6%80%BB%E7%BB%93)
    - [1. 输出类命令](#1-%E8%BE%93%E5%87%BA%E7%B1%BB%E5%91%BD%E4%BB%A4)
    - [2. 插件和预设类命令](#2-%E6%8F%92%E4%BB%B6%E5%92%8C%E9%A2%84%E8%AE%BE%E7%B1%BB%E5%91%BD%E4%BB%A4)
    - [3. 忽略文件](#3-%E5%BF%BD%E7%95%A5%E6%96%87%E4%BB%B6)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Babel 的命令行工具的使用

## 1. 参考资料

1. [@babel/cli使用总结](https://blog.csdn.net/qdmoment/article/details/106218299)

2. [babel-cli](https://babeljs.io/docs/en/babel-cli)

## 2. Babel 命令行工具简介

1. Babel 内置了一个命令行工具：`@babel/cli`。可通过命令行编译文件。

2. 各种可直接调用脚本都存放在 `@babel/cli/bin` 中。包括一个可通过 shell 执行的实用脚本：`babel-external-helpers.js`，以及 Babel cli 主脚本 `babel.js`。

3. Babel 不推荐我们全局安装 `@babel/cli`，而是局部安装：`npm install --save-dev @babel/core @babel/cli`

4. 在安装 `@babel/cli` 之前，项目中需要有 `package.json`，这样能确保我们能正确使用 `npx` 命令。

## 3. Babel 命令行工具的使用

1. 项目结构：
   ```
      5-cli
      |--package.json   
      |--dist
      |--src
         |--index.js
   ```
2. `.babelrc.json` 的内容如下：
   ```json
      {
          "presets": [
              [
                  "@babel/preset-env",
                  {
                      "useBuiltIns": "usage",
                      "modules": false,
                      "targets": {
                          "ie": "6"
                      }
                  }
              ]
          ]
      }
   ```

### 1. 编译单个文件

#### 1. `npx babel filename`

1. 这个命令编译一个 js 文件。将编译后的内容输出到终端。

2. filename 指的是源文件。我们在源文件写一下 ES6 的内容：
   ```js
      // scr/index.js
      const p1 = new Promise((resolve, reject) => {})

      const add = (x, y) => x + y;

      function* gen(x, y) {
         const z = yield x + y;
         return z;

      }
   ```
3. 执行命令：`npx babel scr/index.js`，终端输出如下：
   ```js
      import "regenerator-runtime/runtime.js";

       var _marked = /*#__PURE__*/regeneratorRuntime.mark(gen);

       import "core-js/modules/es6.object.to-string.js";
       import "core-js/modules/es6.promise.js";
       var p1 = new Promise(function (resolve, reject) {});

       var add = function add(x, y) {
       return x + y;
       };

       function gen(x, y) {
           var z;
           return regeneratorRuntime.wrap(function gen$(_context) {
               while (1) {
                   switch (_context.prev = _context.next) {
                       case 0:
                          _context.next = 2;
                          return x + y; 
                       case 2:
                          z = _context.sent;
                          return _context.abrupt("return", z);

                       case 4:
                       case "end":
                           return _context.stop();
                    }
               }
           }, _marked);
       }

   ```

#### 2. `npx babel sourcefile --out-file targetfile`

1. 如果我们想将编译后的文件输出到一个文件中，可以使用 `--out-file` 或者是 `-o` 这个选项。

2. 在终端输入：`npx babel .\src\index.js --out-file dist/index-compiled.js`，会将编译后的内容写入到 `dist` 目录下的 `index-compiled.js` 文件中。

#### 3. `npx babel sourcefile --watch --out-file targetfile`

1. 如果想要文件一发生变化，就自动进行编译，可以在命令中加上 `--watch` 或者 `-w` 选项。表示对源文件进行监视，一旦源文件发生变化，就立刻进行编译。

2. 在终端输入：`npx babel .\src\index.js --watch --out-file dist/index-compiled.js`，那么会监视 src 目录下的 index.js 文件，一旦 index.js 发生变化，就会重新编译。

#### 4. 附带着 source map 一起编译

1. 有些情况下，我们需要 source map 来建立编译后的文件和源文件的联系，而默认的编译都是不带 source map 的。所以如果我们想加上 source map 的话，可以在编译命令中加上 `--source-maps` 或者 `-s`。

2. 在终端输入：`npx babel .\src\index.js --out-file dist/index-compiled.js --source-maps`，这个命令会在 dist 目录下生成与目标 js 文件同名的 `source map` 文件：`index-compiled.js.map`，内容如下：
   ```json
      {
         "version": 3,
         "sources": [
             "../src/index.js"
         ],
         "names": [],
         "mappings": ";;mDAIU,G;;;;;;;;;AAJV,IAAM,EAAE,GAAG,IAAI,OAAJ,CAAY,UAAC,OAAD,EAAU,MAAV,EAAqB,CAAE,CAAnC,CAAX;;AAEA,IAAM,GAAG,GAAG,SAAN,GAAM,CAAC,CAAD,EAAI,CAAJ;AAAA,SAAU,CAAC,GAAG,CAAd;AAAA,CAAZ;;AAEA,SAAU,GAAV,CAAc,CAAd,EAAiB,CAAjB;AAAA;AAAA;AAAA;AAAA;AAAA;AAAA;AACc,iBAAM,CAAC,GAAG,CAAV;;AADd;AACU,UAAA,CADV;AAAA,2CAEW,CAFX;;AAAA;AAAA;AAAA;AAAA;AAAA;AAAA;AAAA;;AAMA,IAAM,GAAG,GAAG,IAAI,GAAJ,EAAZ;AAEA,IAAM,GAAG,GAAG,IAAI,GAAJ,EAAZ",
         "file": "index-compiled.js",
         "sourcesContent": [
             "const p1 = new Promise((resolve, reject) => {})\r\n\r\nconst add = (x, y) => x + y;\r\n\r\nfunction* gen(x, y) {\r\n    const z = yield x + y;\r\n    return z;\r\n\r\n}\r\n\r\nconst map = new Map();\r\n\r\nconst set = new Set();\r\n\r\n"
          ]
       }
   ```
3. 如果想要生成行内的 source map，即 source map 和编译后的目标 js 文件在一起，那么可以在编译命令中加上 `--source-maps inline`。

4. 在终端输入：`npx babel .\src\index.js --out-file dist/index-compiled.js --source-maps inline`，source map 的内容就会在 dist 下的 index-compiled.js 文件中：
   ```js
      import "regenerator-runtime/runtime.js";

       var _marked = /*#__PURE__*/regeneratorRuntime.mark(gen);

       import "core-js/modules/es6.object.to-string.js";
       import "core-js/modules/es6.promise.js";
       import "core-js/modules/es6.map.js";
       import "core-js/modules/es6.string.iterator.js";
       import "core-js/modules/es6.array.iterator.js";
       import "core-js/modules/web.dom.iterable.js";
       import "core-js/modules/es6.set.js";
       var p1 = new Promise(function (resolve, reject) {});

       var add = function add(x, y) {
       return x + y;
       };

       function gen(x, y) {
       var z;
       return regeneratorRuntime.wrap(function gen$(_context) {
       while (1) {
       switch (_context.prev = _context.next) {
       case 0:
       _context.next = 2;
       return x + y;

               case 2:
                 z = _context.sent;
                 return _context.abrupt("return", z);

               case 4:
               case "end":
                 return _context.stop();
             }
           }
       }, _marked);
       }

       var map = new Map();
       var set = new Set();

       //# sourceMappingURL=data:application/json;charset=utf-8;base64,eyJ2ZXJzaW9uIjozLCJzb3VyY2VzIjpbIi4uL3NyYy9pbmRleC5qcyJdLCJuYW1lcyI6W10sIm1hcHBpbmdzIjoiOzttREFJVSxHOzs7Ozs7Ozs7QUFKVixJQUFNLEVBQUUsR0FBRyxJQUFJLE9BQUosQ0FBWSxVQUFDLE9BQUQsRUFBVSxNQUFWLEVBQXFCLENBQUUsQ0FBbkMsQ0FBWDs7QUFFQSxJQUFNLEdBQUcsR0FBRyxTQUFOLEdBQU0sQ0FBQyxDQUFELEVBQUksQ0FBSjtBQUFBLFNBQVUsQ0FBQyxHQUFHLENBQWQ7QUFBQSxDQUFaOztBQUVBLFNBQVUsR0FBVixDQUFjLENBQWQsRUFBaUIsQ0FBakI7QUFBQTtBQUFBO0FBQUE7QUFBQTtBQUFBO0FBQUE7QUFDYyxpQkFBTSxDQUFDLEdBQUcsQ0FBVjs7QUFEZDtBQUNVLFVBQUEsQ0FEVjtBQUFBLDJDQUVXLENBRlg7O0FBQUE7QUFBQTtBQUFBO0FBQUE7QUFBQTtBQUFBO0FBQUE7O0FBTUEsSUFBTSxHQUFHLEdBQUcsSUFBSSxHQUFKLEVBQVo7QUFFQSxJQUFNLEdBQUcsR0FBRyxJQUFJLEdBQUosRUFBWiIsImZpbGUiOiJpbmRleC1jb21waWxlZC5qcyIsInNvdXJjZXNDb250ZW50IjpbImNvbnN0IHAxID0gbmV3IFByb21pc2UoKHJlc29sdmUsIHJlamVjdCkgPT4ge30pXHJcblxyXG5jb25zdCBhZGQgPSAoeCwgeSkgPT4geCArIHk7XHJcblxyXG5mdW5jdGlvbiogZ2VuKHgsIHkpIHtcclxuICAgIGNvbnN0IHogPSB5aWVsZCB4ICsgeTtcclxuICAgIHJldHVybiB6O1xyXG5cclxufVxyXG5cclxuY29uc3QgbWFwID0gbmV3IE1hcCgpO1xyXG5cclxuY29uc3Qgc2V0ID0gbmV3IFNldCgpO1xyXG5cclxuIl19
   ```
### 2. 编译某个文件夹

1. 如果我们需要编译整个项目，那么一个文件一个文件的手动编译肯定不可以，因此，我们需要编译整个文件夹，并将编译后的内容输出到一个新的文件夹。

2. 使用 `--out-dir` 或者 `-d`，可以将整个文件夹的内容进行编译，然后将编译后的文件输入操目标文件夹中，目标文件夹的组织结构后源文件夹相同。

### 3. 忽略某些文件

## 4. 编译命令总结


### 1. 输出类命令

1. 输出类命令总结如下

   命名名|作用|示例
   :---:|:---:|---:
    --out-file|输出文件名称|npx babel script.js --out-file script-compiled.js
   --watch|文件监控|npx babel script.js --watch --out-file script-compiled.js
   --source-maps|生成.js.map文件|npx babel script.js --out-file script-compiled.js --source-maps
   --source-maps inline|在生成的文件中插入source.map注释|	npx babel script.js --out-file script-compiled.js --source-maps inline
   --out-dir|输出文件夹|npx babel src --out-dir lib
   --copy-files|复制文件|npx babel src --out-dir lib --copy-files
   < |通过stdin导入文件|npx babel --out-file script-compiled.js < script.js
   --out-file-extension|指定扩展名称|babel src/ lib/ --out-file-extension .mjs

### 2. 插件和预设类命令

1. 指定编译代码时的插件或者预设的命令

   命名名|作用|示例
   :---:|:---:|---:
    --plugins=|指定plugins|npx babel script.js --out-file script-compiled.js --plugins=@babel/proposal-class-properties,@babel/transform-modules-amd
    --presets=|指定presets|npx babel script.js --out-file script-compiled.js --presets=@babel/preset-env,@babel/flow
    --config-file|指定configPath|npx babel --config-file /path/to/my/babel.config.json --out-dir dist ./src

### 3. 忽略文件

1. 编译过程中忽略文件的命令

   命名名|作用|示例
   :---:|:---:|---:
   --ignore|忽略文件|npx babel src --out-dir lib --ignore "src/**/*.spec.js","src/**/*.test.js"
   --no-copy-ignored|不拷贝忽略文件|npx babel src --out-dir lib --copy-files --no-copy-ignored
   --no-babelrc|忽略.babelrc|npx babel --no-babelrc script.js --out-file script-compiled.js --presets=es2015,react
