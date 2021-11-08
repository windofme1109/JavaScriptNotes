# 使用 Node 开发命令行工具

## 1. 参考资料

1. [Linux Shell命令的基本格式](http://c.biancheng.net/view/3163.html)

2. [一个符合GNU标准的命令行的组成格式](https://www.jianshu.com/p/0a61481087dc)

3. [linux常用命令格式](https://www.cnblogs.com/951767619x/p/14328825.html)

4. [命令行语法字符](https://ftpdocs.broadcom.com/cadocs/0/CA%20ARCserve%20Backup%20r16%205-CHS/Bookshelf_Files/HTML/caabhelp/cl_cmd_line_syntax_char.htm)

5. [命令行界面 (CLI)、终端 (Terminal)、Shell、TTY，傻傻分不清楚？](https://segmentfault.com/a/1190000016129862)

6. [Linux 命令格式](https://blog.csdn.net/liang19890820/article/details/52512744)

7. []()

8. []()

## 2. GNU 和 Linux 命令规范

### 1. GNU 命令规范

1. 实际上，我们在终端输入的命令还是有一定的标准格式的。一个 GNU 规定的命令行的格式如下所示：
   ![](./img/GNU-CLI.jpg)
   图片来源：[一个符合GNU标准的命令行的组成格式](https://www.jianshu.com/p/0a61481087dc)

2. 一个完整的Terminal命令主要由4部分组成：
   - 命令名（Executable): `git`
   - 子命令（Command）: `push`
   - 选项（Options）: `--no-pager` 和 `-v` 都是
   - 参数（arguments）: `origin` 和 `master`

3. 这里说一下选项（Options）。从形式上来说，`Options`分成两种形式：简短形式和完整形式。
   - 简短形式：一般由一个连接符`-`后面跟一个字母组成，如：
   `ls -l -a -t # -l, -a, -t 都是简短形式的Option`
   几个简短形式的 `Options` 可以合并写成一个：
`ls -lat` 效果等同于 `ls -l -a -t`。
   - 完整形式：一般由两个连接符 `--` 开头，接着是一个或多个完整的单词，如果有多个单词，那么中间用一个连接符连接，如上面的 `--no-pager`。

4. 从功能上来讲，`Options` 一般有两种。一种是`switch`：开关，即用来开启（enable）或者是关闭（disable）一些 `feature`，关闭（disable）的这个选项一般以 `--no` 开头，如上面的 `--no-pager`，就是用来禁用 `pager` 这个 `feature` 的。除此之外的另外一种是 `flag`。`switch` 一般没有参数，`flag` 则一般有参数。

5. 如果一个 `flag` 有参数，那么一般简短形式的 flag 跟它的参数之间由一个空格分开。而完整形式的 `flag` 则用一个 `=` 连接它的参数，如：
   ```js
      curl -X POST http://www.google.com # POST是-X的参数
      curl --request=POST http://www.google.com # POST 是 --request的参数。
   ```
   这里要分清楚的是 `Options` 的参数和这整个命令的参数，在上面的例子中，POST 是选项 `-X`（或 `--request`）的参数，而 `http://www.google.com` 则是这整个命令的参数。

### 2. Linux 命令

1. Linux 中的命令一般会遵守一定的格式，如下所示：
   ```shell
      command [options] [arguments]
   ```
2. 每个字段的含义如下：
   - `command` 命令：即命令名称。
   - `options` 命令选项：用于调整命令的功能。命令不同，选项的个数和内容会有所不同；要实现的命令功能不同，选项的个数和内容也会有所不同。
   - `arguments` 命令参数：是命令处理的对象，通常情况可以是文件名、目录、或用户名。

3. 以 `ls` 这个命令为例，我们在终端中直接敲入这个命令会列出当前目录下的内容。`ls` 命令后面不加选项和参数也能执行，不过只能执行最基本的功能。
   ![](img/linux-ls.png)

4. 在命令后面加入参数后可以显示更加丰富的数据。例如：`ls -l`。`-l` 的作用是：长数据串列出，包含文件的属性与权限等等数据。
   ![img.png](img/linux-ls-l.png)

5. 选项又分为短格式选项：`-a` 和长格式选项：`--all`。短格式选项是英文的简写，用一个连字符（`-`）调用，长格式选项是英文完整单词，一般用两个连字符（`--`）调用。多个单词中间使用一个连字符连接（不是所有选项都有长格式）。
   - 短格式 `-a` 的调用形式：
     ![img.png](img/linux-ls-a.png)
   - 长格式 `--all` 的调用形式：
     ![img.png](img/linux-ls-all.png)

6. 选项可以多个并用。例： `ls -a -l`
   - `-a`：全部的文件，连同隐藏( 开头为 . 的文件) 一起列出来。
   - `-l`：长数据串列出，包含文件的属性与权限等等数据。
     ![img.png](img/linux-ls-a-l.png)

7. 选项可以合在一起写（短格式的命令），如：`ls -al`：
   ![img.png](img/linux-ls-al.png)

### 3. GNU 与 Linux 命令规范在开发命令行工具中的作用

1. 实际上，`Linux` 的命令和 `GNU` 的命令的格式基本类似。`Linux` 命令格式更倾向于 `Linux` 系统本身提供的命令。而 `GNU` 的命令格式更倾向于命令行（Command Line Interface）的格式。如 `git`、`npm`、`npx` 等都是符合 `GNU` 格式命令行工具。

2. 因此我们开发命令行工具应该使用 GNU 格式的命令规范。

## 3. Node 开发基础的命令行工具

1. 初始化一个项目：`node-cli-demo-1`：
   ```shell
      mkdir node-cli-demo-1
      cd node-cli-demo-1
      npm init -y
   ```
2. 在 `node-cli-demo-1` 下新建一个 bin 目录，然后在 bin 目录新建一个 word2img.js 文件，word2img.js 内容如下：
   ```js
      #!/usr/bin/env node
      console.log('hello world');
   ```
   `#!/usr/bin/env node` 这句话的作用是用来向系统指明这个脚本文件的解释器是 node。

3. 修改项目下的 package.json 文件：
   ```json
      {
         "name": "node-cli-demo-1",
         "version": "1.0.0",
         "description": "",
         "bin": {
            "word2img": "./bin/word2img.js"
          },
          "keywords": [],
          "author": "",
          "license": "ISC"
      }
   ```
   主要在 package.json 中添加了 bin 字段，其值为对象，内容如下：
   ```json
      "bin": {
            "word2img": "./bin/word2img.js"
      },
   ```
   `bin` 字段用于映射可执行文件的路径，作用类似于添加环境变量。
4. 我们在当前目录下面的 powerShell 中输入：`./bin/word2img.js`，能得到下面的输出：
   ![img.png](img/word2img-1.png)

5. 基本实现一个一个命令行工具，能实现输入一个命令，而得到一个输出。

6. 但是我们需要输入文件的相对路径，这样不够简介，也不够方便。因此我们需要把相对路径去掉，我们命令行输入 `npm link`：
   ![img.png](img/npm-link.png)

7. `npm link` 的作用是软链接。从上面贴的 `npm link` 执行输出的截图可以看到，就是进行了一个软链接的过程，把当前项目软链到 nodejs 安装目录的 node_modules 文件夹下的同名文件夹，然后再软链接到nodejs安装目录下的同名可执行文件。

8. 以当前项目为例，使用的是 windows 10 系统，打开 `C:\Users\qmr\AppData\Roaming\npm\` 文件夹，可以看到多了三个文件 `word2img` 、`word2img.cmd` 和 `word2img.ps1`，这三个文件都可以用编辑器打开，里面主要是 shell 脚本的内容。同时 `C:\Users\qmr\AppData\Roaming\npm\node_modules\` 文件夹下多了一个 `node-cli-demo-1` 的软连接文件夹，其内容和我们的项目一样。

9. 执行完` npm link` 命令后就可以全局使用我们的命令了，`npm link` 还可以指定项目使用。这里就不再详说了。`npm link` 详解请看：[官方文档 npm-link](https://docs.npmjs.com/cli/v7/commands/npm-link)

10. 此时在终端输入：`word2img`，就能得到和刚刚输入完整路径命令一样的输出了：
    ![img.png](img/npm-link-word2img.png)

## 4. 引入 commander 和 chalk

1. 现在我们的命令行只能直接敲一个命令，输出一个 `hello world`，不具备什么实用性，我们可以看一下 `npm` 这个命令有哪些玩法。

### 1. 常用 npm 命令

1. 查看当前版本：
   ```shell
      npm -v  # npm --version
   ```
2. 查看使用说明：
   ```shell
      node -h  # npm --help
   ```
   如下所示：
   ![img.png](img/npm-h.png)
3. 安装包：
   ```shell
      npm install (with no args, in package dir)
      npm install [<@scope>/]<name>
      npm install [<@scope>/]<name>@<tag>
      npm install [<@scope>/]<name>@<version>
      npm install [<@scope>/]<name>@<version range>
      npm install <tarball file>
      npm install <tarball url>
      npm install <folder>
      alias: npm i
      common options: [-S|--save|-D|--save-dev|-O|--save-optional] [-E|--save-exact] [--dry-run]
   ```
   1. `-S` 或者 `--save` 将安装包信息加入到 `package.json` 中的 `dependencies`（生产阶段的依赖）
   2. `-D` 或者 `--save-dev` 安装包信息将加入到 `package.json` 中的 `devDependencies`（开发阶段的依赖），所以开发阶段一般使用它。
   3. `-O` 或者 `--save-optional` 安装包信息将加入到 `package.json` 中的  `optionalDependencies`（可选阶段的依赖）
   4. `-E` 或者 `--save-exact` 精确安装指定模块版本。
   5. 

4. 初始化一个项目（生成 package.json）：
   ```shell
      npm init [--yes|-y|--scope]
   ```
5. 更新一个包：
   ```shell
      npm update [-g] [<pkg>...]
   ```

6. 管理 npm 的配置内容：
   ```shell
      npm config set <key>=<value> [<key>=<value> ...]
      npm config get [<key> [<key> ...]]
      npm config delete <key> [<key> ...]
      npm config list [--json]
      npm config edit
      npm set <key>=<value> [<key>=<value> ...]
      npm get [<key> [<key> ...]]
      alias: c
   ```
   常用的是 `npm config set registry=xx` 来配置 npm 镜像源。

### 2. 一般命令行工具中的命令使用方式总结

1. 观察 npm 命令的使用方式，并对比之前提到的 GNU 命令的标准格式，我们发现，npm 命令提供了命令名（Executable）、子命令（command）、选项（Options）和参数（Arguments）。

2. 命令的说明中，使用方括号（`[]`）包围的内容是可选的，表示我们在使用这个命令时可以根据我们的需求添加或者不添加这个选项或者参数。如：`npm init [--yes|-y|--scope]`，方括号里面的 `--yes|-y|--scope` 这个选项都是可选的，使用竖线（`|`）表示这个选项可以使用这几个值中的一个。而尖括号（`<>`）包围的内容是必选的。也就是使用这个命令时必须添加的内容。如 `npm config set <key>=<value>` 中，`key` 和 `value` 就是必选的参数。

3. 想要实现这样的功能，就需要解析我们在命令中输入的内容，包括命令、选项和参数。我们可以使用原生 Node 提供的 `process.argv` 来解析我们输入的内容。

4. `process.argv` 是一个数组，包含启动 Node 进程时传入的命令行参数。第一个元素将是 process.execPath。即启动 Node.js 进程的可执行文件的绝对路径名。如果需要访问 argv[0] 的原始值，直接访问 `process.argv0` 这个变量。`process.argv` 的第二个元素将是正在执行的 JavaScript 文件的路径。 而其余元素将是任何其他命令行参数。举例如下：
   ```js
      // process-argv.js
      const {argv} = require('process');

      argv.forEach((item, index) => {
      console.log(`${index}: ${item}`);
      });
   ```
   执行命令：`node process-argv.js one tow=three four hello world`，输出如下：
   ```js
      // 0: C:\Program Files\nodejs\node.exe
      // 1: D:\Front-End\Node-Cli\process-argv.js
      // 2: one
      // 3: tow=three
      // 4: four
      // 5: hello
      // 6: world
   ```
5. 实际上使用 `process.argv` 进行解析我们输入的命令是比较麻烦的，我们需要处理较多的情况。在开发命令行的过程中，解析命令不是我们的工作重点，我们的重点应该根据命令执行不同的操作。

6. 因此，我们使用一个成熟的解析命令的工具：`commander` 来解析我们命令、选项和参数。

7. 有时候，我们发现有一些命令行工具的输入是带颜色的，如使用 `git status`，就会用不同的颜色标记不同的文件的状态：
   ![img.png](img/git-status.png)

8. 如果我们也想要实现这样的效果，需要使用另外一个工具：`chalk`。

## 5. commander 和 chalk 的初步使用

### 1. 增加版本信息和命令使用说明 - 使用 commander 实现

1. 安装 commander：
   - commander：`npm install commander`

2. 在 `bin/word2img.js` 中，添加如下的内容： 
   ```js
      #!/usr/bin/env node
      const { Command } = require('commander');
      const program = new Command();
      program.version(require('../package.json').version).usage('<command> [options]');
      program.parse(process.argv);
   ```
   - `version` 方法是 commander 提供的一个输出版本号的方法，命令的选项是 `-V` 或者是 `--version`。
   - `usage` 方法用来输出这个命令的使用方式的相关内容。命令的选项是 `-h` 或者是 `--help`。
   - `parse` 方法接收的第一个参数是等待解析的字符串数组。那么我们可以传入 `process.argv` 作为参数。这样就可以解析我们的命令。

3. 我们输入命令：`word2img -V`，输出如下：
   ![img.png](img/word2img-V.png)

4. 我们输入命令：`word2img -V`，输出如下：
   ![img.png](img/word2img-h.png)

5. 通过 commander 这个工具，我们开发的命令行工具初步具备了实用性。

### 2. 给输出的文字添加颜色 - 使用 chalk 实现

1. 使用 `git status`，输出的文字中是有一些是有颜色的，比如说需要被提交的文件的名称是绿色的，有修改但是没有提交的文件是红色。如果我们想对部分文字进行着色，那么可以使用 chalk 这个工具。

2. 安装 chalk：
   - chalk：`npm install chalk`

3. 继续修改 `bin/word2img.js`：
   ```js
      #!/usr/bin/env node

      // 创建一个局部的 Command 对象
      const { Command } = require('commander');
      const chalk = require('chalk');
      const path = require('path');

      // console.log('hello world');
      const program = new Command();

      // program.version(require('../package.json').version).usage('<command> [options]');

       program.on('--help', () => {
           console.log();

           console.log(
               ` Run ${chalk.green(
                   `word2img <command> --help`
               )} for detailed usage of given command.
               `
           );

           console.log();
      })
      program.parse(process.argv);
   ```
   - `on` 方法用来监听命令（command）和选项（option）事件，来执行自定义操作。第一个参数是监听的命令或者选项，第二个参数是回调函数，当指定的命令或者选项被输入时，就会执行回调函数。
   - `chalk.green` 会将其接收的字符串以红色格式进行输出。

4. 在终端中输入 `word2img --help`，输出如下：
   ![img.png](img/word2img--help.png)

5. 使用 chalk 这个工具，我们实现了对输出的部分文件进行着色。

## 6. 继续完善我们的命令行工具

1. 下面的内容更多的是参考 [从零开始编写一个node命令行工具](https://juejin.cn/post/6948330334085709855)

2. 我们现在的需求是接收用户输入的字符串，把它转化成一张纯色背景文字居中的图片，那么我们可以大致确定出需要的来自外部的变量有哪些：
   - word：将要被转化成图片的字符串
   - width：图片的宽度
   - height：图片的高度
   - bgColor：图片的背景颜色
   - color：文字的颜色
   - size：文字的大小 font-size
   - family：文字的字体 font-family
   - filename：要下载的图片的文件名
   - filepath：图片保存的路径

3. 上面的变量中，word 实际上是命令的参数（argument），其他值都是选项（options）。参数的内容不是固定的，而用户不一定会输入选项，所以选项应该具有默认值。借助 commander，我们可以这样配置：
   ```js
      program
          .command('new <word>')
          .description('generate a new image use the input word')
          .option('-w --width <width>', 'Set width of the image', 600)
          .option('--height <heihgt>', 'Set height of the image', 200)
          .option('--bgColor <bgColor>', 'Set background-color of the image', '#fff')
          .option('--color <color>', 'Set color of the word', '#000')
          .option('--size <size>', 'Set font-size of the word', 48)
          .option('--family <family>', 'Set font-family of the word', 'Arial')
          .option('--filename <filename>', 'Set filename of the image')
          .option('--filepath <filepath>', 'Set file path to save the image' + "(note that the path doesn\'t contain the file name)", path.join(process.cwd(), 'imgs'))
          .action((word, options) => {
              console.log(word);
              console.log(options);
      })
   ```
   - `command` 方法用来指定一个命令或者子命令。其第一个参数接收一个字符串。这里传入的是 `new <word>`，其中 `new` 就是命令的名称，`<word>`指代这个命令的参数，使用尖括号（`<>`）表示这个参数是必选的。
   - `description` 方法接收一个字符串用来简要描述这个命令的作用
   - `option` 方法用来定义命令的选项。而且也作为选项的说明。每个选项可以有一个段格式（short flag），即以单个连字符开头的单个字母，例如 `-a`、`-l`。或者是一个长格式（long name），即以 2 个连字符开头的完整单词，多个单词使用一个连字符连接，例如：`--save`、`--save-dev`。
     - option 方法的第一个参数是字符串形式的选项的名字，我们可以使用逗号（`,`） 空格（` `）和竖线（`|`）来分隔长格式和短格式的选项，例如：`"-w --width"`，`-s,--save` 或者 `-a|-all`。第一个参数的最后的内容是这个选项的接收的参数（arguments）。如 `-w --width <width>`，最后的`<width>` 就表示 `-w` 这个选项接收的参数，使用尖括号（`<>`）表示这个参数是必要的，也就是如果我们在命令中使用 `-w` 这个选项，那么就必须加上参数。最后字符串中的这个参数也要和前面的选项名称用空格分开。
     - 第二个参数是描述这个选项的作用。
     - 第三个参数这个选项的默认值。
   - `action` 方法来监听用户输入，当用户输入 `new` 命令后会触发回调函数，回调函数的第一个参数是命令的值，第二个参数是上面的选项对象，第三个参数是 command 对象本身。我们可以根据我们输入的命令，来决定执行什么内容。

4. 在终端输入：`word2img new 'hello world'`，终端的输出如下：
   ![img.png](img/word2imgnew.png)

5. 接下来实现生成图片的功能。因为这里的重点是实现命令行工具，如何生成图片不是我们关注的点，因此，我们这里直接贴代码了。

6. 安装 `canvas`：`npm install canvas`

7. 新建 `./utils/newCanvas.js`，内容如下：
   ```js 
      const { createCanvas } = require('canvas')

      exports.newCanvas = function (word, options) {
          const canvas = createCanvas(options.width, options.height)
          const ctx = canvas.getContext('2d')

          // rect
          ctx.fillStyle = options.bgColor
          ctx.fillRect(0, 0, options.width, options.height)
          // word
          ctx.textBaseline = 'middle'
          ctx.textAlign = 'center'
          ctx.font = `${options.size}px ${options.family}`
          ctx.fillStyle = options.color
          ctx.fillText(word, options.width / 2, options.height / 2)

          return {
             canvas,
             ctx,
          }
      }

   ```
8. 新建 `./utils/canvas2img.js`，内容如下：
   ```js
      const fs = require('fs')
      const path = require('path')
      const chalk = require('chalk')

      exports.canvas2img = function (canvas, filename, filepath) {
          const buf = canvas.toBuffer()
          filename = filename || `word2img_${Date.now()}.png`
          const url = path.resolve(filepath, filename)
          fs.writeFile(url, buf, function (err) {
              if (err) {
                  console.log(err)
              } else {
                  console.log(
                    `✨ image generated successfully at: ${chalk.yellow(url)}`
                  )
              }
          })
      }
   ```
9. 继续修改 `./bin/word2img.js`，内容如下：
   ```js
      #!/usr/bin/env node
      // 创建一个局部的 Command 对象
      const { Command } = require('commander');
      const chalk = require('chalk');
      const path = require('path');


      const { newCanvas } = require('../utils/newCanvas');
      const { canvas2img } = require('../utils/canvas2img');

      // console.log('hello world');
      const program = new Command();

      program.version(require('../package.json').version).usage('<command> [options]');

       program.on('--help', () => {
           console.log();

            console.log(
                ` Run ${chalk.green(
                    `word2img <command> --help`
                )} for detailed usage of given command.
                `
            );

           console.log();
       })

       program
           .command('new <word>')
           .description('generate a new image use the input word')
           .option('-w --width <width>', 'Set width of the image', 600)
           .option('--height <heihgt>', 'Set height of the image', 200)
           .option('--bgColor <bgColor>', 'Set background-color of the image', '#fff')
           .option('--color <color>', 'Set color of the word', '#000')
           .option('--size <size>', 'Set font-size of the word', 48)
           .option('--family <family>', 'Set font-family of the word', 'Arial')
           .option('--filename <filename>', 'Set filename of the image')
           .option('--filepath <filepath>', 'Set file path to save the image' + "(note that the path doesn\'t contain the file name)", path.join(process.cwd(), 'imgs'))
           .action((word, options) => {
               // console.log(word);
               // console.log(options);
               const { canvas } = newCanvas(word, options)
              canvas2img(canvas, options.filename, options.filepath);
           })



      program.parse(process.argv);
   ```
10. 此时，我们在终端输入 `word2img new 'hello-word'`，成功在项目根目录生成了图片，并且终端会提示文件的位置信息。
   ![img.png](img/wor2img-newimg.png)

11. 添加一些选项：` word2img new 'JavaScript' --bgColor 'green' --color '#666' --filename 'javascript.png'`，终端输出的信息如下：
   ![img.png](img/word2imgnew-2.png)

12. 至此我们基本实现了一个简单的生成图片的命令行工具。

## 7. commander

1. commander 是一个用来解析处理命令行参数的一个工具。功能非常强大，很多知名的命令行工具都使用了 commander。

2. 安装 commander：`npm install commander`

3. commander 官方文档：[官方文档](https://github.com/tj/commander.js#commands)

4. 使用 - 创建全局的 command 对象：
   ```js
      const { program } = require('commander');
      program.version('0.0.1');
   ```
   这种方式适用于快速搭建一个一个项目。

5. 使用 - 创建局部的 command 对象：
   ```js
      const { Command } = require('commander');
      const program = new Command();
      program.version('0.0.1');
   ```
   在大型项目中，我们可能在不同的地方以不同的形式使用 command 对象，因此，我们创建局部的 Command 对象。

6. program 作为 Command 的实例，其中的每个方法都返回一个 command 对象，这样可以实现链式调用。

### 1. command

1. `command` 方法用来指定一个命令或者子命令。其第一个参数接收一个字符串。例如传入的是 `new <word>`，其中 `new` 就是命令的名称，`<word>`指代这个命令的参数，使用尖括号（`<>`）表示这个参数是必选的。

2. 有两种方式实现响应 command 函数定义的命令：
   - 附在 command 方法后面的 action 函数，接收一个回调函数，在这个回调函数内部处理相关逻辑。
   - 执行一个单独的可执行的 js 文件。

3. command 方法可以接收第二个参数，用来描述第一个参数指定的命令的作用。 当我们指定了command 方法的第二个 description 参数，并调用 command 方法时，这会告诉 Commander 您将为子命令使用独立的可执行文件。Commander 将以子命令（如 pm-install、pm-search）为可执行文件的名称在入口脚本的目录（如 `./examples/pm` 下）中的搜索可执行文件。我们可以使用executableFile 配置选项指定自定义名称。示例如下：
   ```js

   ```
### 2. option

1. `option` 方法用来定义命令的选项。而且也作为选项的说明。每个选项可以有一个段格式（short flag），即以单个连字符开头的单个字母，例如 `-a`、`-l`。或者是一个长格式（long name），即以 2 个连字符开头的完整单词，多个单词使用一个连字符连接，例如：`--save`、`--save-dev`。
2. 
3. option 方法的参数：
   - 第一个参数是字符串形式的选项的名字，我们可以使用逗号（`,`） 空格（` `）和竖线（`|`）来分隔长格式和短格式的选项，例如：`"-w --width"`，`-s,--save` 或者 `-a|-all`。第一个参数的最后的内容是这个选项的接收的参数（arguments）。如 `-w --width <width>`，最后的`<width>` 就表示 `-w` 这个选项接收的参数，使用尖括号（`<>`）表示这个参数是必要的，也就是如果我们在命令中使用 `-w` 这个选项，那么就必须加上参数。最后字符串中的这个参数也要和前面的选项名称用空格分开。
   - 第二个参数是描述这个选项的作用。
   - 第三个参数这个选项的默认值。

### 3. action

1. `action` 方法来监听用户输入，当用户输入 command 方法指定的命令后会触发回调函数，回调函数的第一个参数是命令的值，第二个参数是上面的选项对象，第三个参数是 command 对象本身。我们可以根据我们输入的命令，来决定执行什么内容。

### 7. description

1. `description` 方法接收一个字符串用来简要描述这个命令的作用。一般附在 command 方法后面。

### 3. usage

1.  `usage` 方法用来输出这个命令的使用方式的相关内容。接收一个字符串作为参数，字符串是命令的描述内容。命令的选项是 `-h` 或者是 `--help`。

### 4. name

### 5. on

### 6. version

1. `version` 方法是 commander 提供的一个输出版本号的方法，接收一个字符串作为参数，这个字符串就是版本信息。命令的选项是 `-V` 或者是 `--version`。


### 8. parse

1. `parse` 方法接收的第一个参数是等待解析的字符串数组。那么我们可以传入 `process.argv` 作为参数。这样就可以解析我们的命令。


## 8. chalk

1. chalk 这个包可以给终端的文字添加样式，使得我们在终端输出的信息更加明显，更加具有提示效果。

2. 安装 chalk：`npm install chalk`

3. chalk 官方文档：[官方文档](https://github.com/chalk/chalk#readme)