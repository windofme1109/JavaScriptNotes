<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [TSC 编译命令](#tsc-%E7%BC%96%E8%AF%91%E5%91%BD%E4%BB%A4)
  - [1. TSC 编译命令参考资料](#1-tsc-%E7%BC%96%E8%AF%91%E5%91%BD%E4%BB%A4%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99)
  - [2. TSC 编译命令总结](#2-tsc-%E7%BC%96%E8%AF%91%E5%91%BD%E4%BB%A4%E6%80%BB%E7%BB%93)
    - [1. `tsc`](#1-tsc)
    - [2. `tsc --init`](#2-tsc---init)
    - [3. `tsc -w`](#3-tsc--w)
    - [4. `tsc index.ts`](#4-tsc-indexts)
    - [5. `tsc src/*.ts`](#5-tsc-srcts)
    - [6. `tsc --project tsconfig.production.json` 或者是 `tsc -p tsconfig.production.json`](#6-tsc---project-tsconfigproductionjson-%E6%88%96%E8%80%85%E6%98%AF-tsc--p-tsconfigproductionjson)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# TSC 编译命令

## 1. TSC 编译命令参考资料

1. [tsc CLI Options](https://www.typescriptlang.org/docs/handbook/compiler-options.html)

## 2. TSC 编译命令总结

### 1. `tsc`

1. 在项目中执行这个命令，则会根据这个命令，去根据项目根目录下的 `tsconfig.json` 的配置去编译项目。
   
### 2. `tsc --init`

1. 生成tsconfig.json文件。
   
### 3. `tsc -w`

1. `-w` 表示 watch，也就是说，一旦 ts 文件发生变化，自动执行 tsc 命令，帮助我们编译成 js 文件。
   
### 4. `tsc index.ts`

1. 编译指定的 ts 文件
   
### 5. `tsc src/*.ts`

1. 以默认的设置编译指定路径下的 ts 文件，可以使用相对路径

### 6. `tsc --project tsconfig.production.json` 或者是 `tsc -p tsconfig.production.json`

1. 这个命令是用来编译项目。`--project` 或 `-p` 参数，表示项目，`tsconfig.production.json` 则表示，使用哪一个 tsconfig.json 进行编译项目。
2. 一般情况下，我们在根目录下，建立新的 tsconfig.json 文件，如：`tsconfig.production.json`、`tsconfig.build.json` ，根据我们的不同需求，根据不同的配置文件去编译项目。