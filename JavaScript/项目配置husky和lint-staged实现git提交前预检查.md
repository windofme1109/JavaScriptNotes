# 1. 参考资料

1. [husky 官方文档 - 自定路径](https://typicode.github.io/husky/#/?id=custom-directory)

2. [husky使用总结](https://zhuanlan.zhihu.com/p/366786798)

3. [husky自定义目录钩子的正确使用](https://blog.csdn.net/Banterise/article/details/115206267)

4. [手摸手教你使用最新版husky(v7.0.1)让代码更优雅规范](https://juejin.cn/post/6982192362583752741)

5. [[前端工程化配置] husky + lint-staged 格式化git提交代码](https://juejin.cn/post/7085534305249656862)

6. [为什么 husky 放弃了传统的 JS 配置](https://juejin.cn/post/7000186205224566791)

7. [husky 官方文档](https://typicode.github.io/husky/#/?id=features)

# 2. 基本说明

1. husky 是一个前端工程化工具，可以方便的向我们的项目中添加 git hook，方便在我们使用 git 命令时执行一些操作。如提交前检查、代码格式化、提交信息格式化等。

2. 从 v7.0 开始，husky 的配置方式发生了变化，我们基于 7.0 以后的 husky 进行配置。

3. 为什么 husky 的配置方式会发生不兼容的变化：
   > 在 v4 版本之前 husky的工作方式是这样的：为了能够让用户设置任何类型的git hooks，husky不得不创建所有类型的git hooks
   这样做的好处就是无论用户设置什么类型的git hook，husky都能确保其正常运行。但是缺点也是显而易见的，即使用户没有设置任何git hook，husky也向git中添加了所有类型的git hook。
   在当时 husky 有过这样的设想：有没有可能让husky只添加我们需要的git hook呢？作者尝试过解决这个问题，但是失败了。
   因为husky需要在两个地方进行配置才能完成一个完整的git hook功能。一个是在package.json中配置git hook所要执行的真正命令，一个是在.git/hooks/中配置相对应的git hook。也就是说无论是添加还是删除git hook就要保证在这两个地方同步执行对应的操作。作者无法找到一个可靠的方法来同步这两个地方的配置，因此失败了。
   > 
   > 新版 husky 的工作原理又是什么呢？
   > 
   > 直到 2016 年，Git 2.9引进了core.hooksPath，可以设置Git hooks脚本的目录，这个引进也就是新版husky改进的基础： 可以使用husky install 将 git hooks的目录指定为.husky/
   > 
   >使用husky add命令向.husky/中添加hook
   > 
   >通过这种方式我们就可以只添加我们需要的git hook，而且所有的脚本都保存在了一个地方（.husky/目录下）因此也就不存在同步文件的问题了。

    详情可见：[Why husky has dropped conventional JS config](https://link.juejin.cn/?target=https%3A%2F%2Fblog.typicode.com%2Fhusky-git-hooks-javascript-config%2F)


# 2. 基本配置方式

## 1. 自动配置

1. 注意，下面所有的命令都是在与 .git 目录同级下的目录下执行，因为 husky 需要 .git 中的相关配置项。

2. 使用 `npx` 结合 `husky-init` 命令：
```shell
npx husky-init && npm install       # npm
npx husky-init && yarn              # Yarn 1
yarn dlx husky-init --yarn2 && yarn # Yarn 2+
pnpm dlx husky-init && pnpm install # pnpm
```
2. 会安装 husky，然后调整 package.json，并且创建一个 pre-commit hook。

3. 添加另外一个 hook 函数，请执行：
```shell
npx husky add .husky/commit-msg 'npx --no -- commitlint --edit "$1"'
```
4. 根据我们的需求添加不同的 git hook。

## 2. 手动配置

### 1. 安装

```shell
npm install husky --save-dev
```
### 2. 启用 git hook

1. 在项目的根目录下执行：
```shell
npx husky install
```
2. 如果我们想使用自动化的方式在每次安装完依赖以后自动启用 git hook，那么我们可以在 package.json 中配置一个命令：

```json
"scripts": {
  
   "prepare": "husky install",
   ...
   },
```
3. 设置 `prepare` 命令的目的是：在其他用户安装项目依赖时会自动执行`husky install`；
   
4. 配置完成后，在项目中执行 `npm prepare`（即执行了 `husky install`）。

5. 执行完 `husky install` 命令后，会在根目录下，生成一个 `.husky` 文件夹，里面存放的是和 git hook 相关的内容。

### 3. 添加 pre-commit hook

1. 在项目根目录执行 `npx husky add`：
```shell
npx husky add .husky/pre-commit 'npm run test'
```

2. 执行这一步后，在 .husky 目录下会创建一个 `pre-commit` 文件，内容如下：
```
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npm run test
```

3. 此时，我们如果进行 git 提交，比如说执行 `git commit -m 'test'`，那么在正式提交前，会执行 `npm run test` 这个命令。


# 3. 结合 lint-staged 实现代码提交前检查

## 1. lint-staged

1. `lint-staged` 是一个对 git 暂存区代码进行格式化的工具。可以对代码进行格式化、检查（lint）等操作。

2. 需要注意的是，`lint-staged` 本身不具备代码格式化、代码检查等功能，这些功能依赖于其他工具实现。通过 `lint-staged` 这个工具，实现对暂存区的代码进行处理。因此，在安装 `lint-staged` 前，需要安装 `prettier`、`eslint` 等工具，并完成相关的配置。

3. 使用文档：
    - [npm - lint-staged](https://www.npmjs.com/package/lint-staged)
    - [github - lint-staged](https://github.com/okonet/lint-staged#readme)
    
4. 安装
```shell
npm install lint-staged --save-dev
```

## 2. lint-staged 配置

1. 在 package.json 中，需要配置 `lint-staged` 命令：
```json
{
    "scripts": {
        "lint-staged": "lint-staged"
    },
    "lint-staged": {
        "*.{js,ts}": [
            "prettier --write",
            "eslint --cache --fix",
            "git add"
        ],
        "src/**/*{jsx,tsx}": [
            "eslint --cache --fix",
            "git add"
        ]
    }
}

```

2. 主要在两个方面进行配置：
   - script 中配置 `lint-staged` 指令
   - 新增 `lint-staged` 字段，并配置 lint-staged 的具体任务（假设项目中已配置 prettier 和 eslint）。对哪些文件执行哪些操作。这里可以指定不同的目录下的不同文件，同时命令可以配置多个，多个命令式从前往后执行。

## 3， 结合 husky 实现提交前检查

1. 配置好 lint-staged 以后，需要将 lint-staged 命令添加到 git hook 中。

2. 在项目根目录中执行：
```shell
npx husky add .husky/pre-commit 'npm run lint-staged'
```
3. 执行这一步后，在 .husky 目录下会创建一个 `pre-commit` 文件，内容如下：
```
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npm run lint-staged
```

