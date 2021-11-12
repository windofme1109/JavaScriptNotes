<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [npm 常用命令总结](#npm-%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4%E6%80%BB%E7%BB%93)
  - [1. 参考资料](#1-%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99)
  - [2. npm 简介](#2-npm-%E7%AE%80%E4%BB%8B)
  - [3. npm 命令](#3-npm-%E5%91%BD%E4%BB%A4)
  - [3. npm init](#3-npm-init)
  - [4. npm start](#4-npm-start)
  - [5. npm stop](#5-npm-stop)
  - [6. npm install](#6-npm-install)
  - [7. npm uninstall](#7-npm-uninstall)
  - [8. npm update](#8-npm-update)
  - [9. npm ls](#9-npm-ls)
  - [10. npm config](#10-npm-config)
  - [11. npm help](#11-npm-help)
  - [12. npm cache](#12-npm-cache)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# npm 常用命令总结

## 1. 参考资料

1. [npm 官网](https://docs.npmjs.com/about-npm)

2. [npm 常用命令详解](https://www.cnblogs.com/ysk123/p/11655502.html)

## 2. npm 简介

1. NPM 的全称是 Node Package Manager，是随同 NodeJS 一起安装的包管理和分发工具，使用 NPM 能够让 JavaScript 开发者下载、安装、上传以及管理已经安装的包。

2. npm 包含三个不同的部分：
   - npm 网站
   - 命令行工具
   - 仓库

3. 在 npm 的[官网](https://www.npmjs.com)上。我们可以搜索包、设置配置文件以及管理 npm 体验的其他方面。例如，我们可以设置组织来管理对共有或私有包的访问。

4. 在终端使用 npm 命令，这是大多数开发者同 npm 交互的方式。

5. 仓库是一个庞大的基于 JavaScript 开发的软件的数据库，同时被大量的元信息包围。

## 3. npm 命令

1. npm 提供的命令非常多，但是我们常用的命令不是很多，我们将常用的命令以表格的形式列出来，然后下面详细解释每个命令的用法。**注意**：这里的 npm 命令均以 7.0 及以上版本为基础进行讲解。
   
2. 常用的 npm 命令（`npm` 的命令均以 `npm` 开头）：
   
   npm 命令 | 作用
   :---:|:---:
   npm init | 在项目中引导创建一个package.json文件
   npm start | 启动一个模块
   npm stop | 停止一个模块
   npm install |安装包
   npm uninstall |卸载包
   npm update |更新包
   npm ls |查看已经安装的包
   npm config |管理 npm 的配置
   npm help |查看帮助信息
   npm cache |管理包的缓存


## 3. npm init

1. 在项目中引导创建一个 package.json 文件。

2. 基本语法：
   ```shell
      npm init [--yes|-y|--scope]
      npm init <@scope> (same as `npm exec <@scope>/create`)
      npm init [<@scope>/]<name> (same as `npm exec [<@scope>/]create-<name>`)
      npm init [-w <dir>] [args...]
   ```
3. 我们常用的是第一种形式：`npm init` 或者 `npm init -y|--yes`。

4. 使用 `npm init`，以问答的形式来创建一个 package.json 文件，创建过程如下图所示：
   ![](./img/npm-init.gif)
    图片来源：[npm 常用命令详解](https://www.cnblogs.com/ysk123/p/11655502.html)

5. 使用 `npm init -y|--yes`，会省略问答过程，直接创建一个 package.json 文件，里面包含一些基本的配置项。如下所示：
   ```json
      {
          "name": "Node",
          "version": "1.0.0",
          "description": "",
          "main": "index.js",
          "scripts": {
               "test": "echo \"Error: no test specified\" && exit 1"
          },
          "keywords": [],
          "author": "",
          "license": "ISC"
      }

   ```

## 4. npm start

1. `npm start` 用来启动一个项目或者模块。

2. `npm start` 实际上是 `npm run start` 的简写。
3. 基本语法
   ```shell
      npm start [-- <args>]
   ```
4. 这个命令写在 package.json 文件 scripts 的 start 字段中，可以自定义命令来配置一个服务器环境和安装一系列的必要程序，如：
   ```json
     "scripts": {
         "start": "node foo.js"
     }     
   ```
5. 此时在终端中输入 `npm start`命令，相当于使用 node 执行 foo.js 这个文件。

6. 如果 package.json 文件没有设置 start 这个字段，则将直接执行 `node server.js`。

## 5. npm stop
## 6. npm install
## 7. npm uninstall
## 8. npm update
## 9. npm ls
## 10. npm config
## 11. npm help
## 12. npm cache

















