# Babel 的命令行工具的使用

## 1. 参考资料

1. [@babel/cli使用总结](https://blog.csdn.net/qdmoment/article/details/106218299)

2. [babel-cli](https://babeljs.io/docs/en/babel-cli)
## 2. Babel 命令行工具简介

1. Babel 内置了一个命令行工具：`@babel/cli`。可通过命令行编译文件。

2. 各种可直接调用脚本都存放在 `@babel/cli/bin` 中。包括一个可通过 shell 执行的实用脚本：`babel-external-helpers.js`，以及 Babel cli 主脚本 `babel.js`。

3. Babel 不推荐我们全局安装 `@babel/cli`，而是局部安装：`npm install --save-dev @babel/core @babel/cli`

4. 在安装 `@babel/cli` 之前，项目中需要有 `package.json`，这样能确保我们能正确使用 npx 命令。

## 3. Babel 命令行工具的使用


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
