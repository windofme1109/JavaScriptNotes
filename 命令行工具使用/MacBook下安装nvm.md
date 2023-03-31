# MacBook 安装 nvm

## 1. 安装

### 1. 官方推荐的安装方式

1. nvm 的 git 仓库地址：[nvm github](https://github.com/nvm-sh/nvm)

2. 官方推荐使用安装脚本的方式进行安装：
   - 通过 `curl` 命令安装：`curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash`
   - 通过 `wget` 命令安装：`wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash`

3. 注意：通过安装脚本的方式进行安装，可能会有超时的情况导致安装失败。这个可以通过查阅资料解决，这里不再详述。

4. 安装完以后，还需要进行配置。也就是向我们的终端的配置文件中写入下面的内容：
   ```
      export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
      [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
   ```
5. 根据我们使用的终端不同，需要将上面的内容写入不同的配置文件中：
   - 如果是 Mac 自带的终端，则需要写入 `.bash_profile`，如果没有，需要在 `~` 目录下新建一个：`touch .bash_profile`。
   - 如果使用的是 Iterm2 一类的终端，则需要写入 `.zshrc`。如果没有，需要在 `~` 目录下新建一个：`touch .zshrc`。

6. 运行 source 命令，刷新我们刚刚修改好的文件：
   - 如果是 Mac 自带的终端，执行：`source ~/.bash_profile`
   - 如果使用的是 Iterm2 一类的终端，执行：`source ~/.zshrc`

7. 在终端中执行 `command -v nvm`，出现的是 `nvm`，这就说明我们安装成功了。

8. source 命令的作用
   - 刷新当前的 shell 环境
   - 在当前环境使用 source 执行 Shell 脚本
   - 从脚本中导入环境中一个 Shell 函数
   - 从另一个 Shell 脚本中读取变量

### 2. 通过 homebrew 安装

## 2. 常见问题

### 1. `zhs: command not found:nvm`

1. 说明我们没有在 `.zshrc` 中配置相关的内容。那么我们需要在 `.zshrc` 中添加相关内容：
   ```
      export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
      [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
   ```
   然后执行 `source ~/.zshrc` 即可。

### 2. 第一次打开终端执行 nvm 命令，能够成功，第二次打开终端执行 nvm 命令。提示找不到这个命令：`command not found:nvm`

1. 有可能是配置文件的位置不对，或者是没有对应的 shell 的配置文件。

2. shell 的配置文件都在 `~` 目录下。

3. bash 的配置文件是 `.bash_profile` 或者是 `.bashrc`。

4. zsh shell 的配置文件是 `.zshrc`。

5. 修改完配置文件以后，需要使用 source 命令刷新一下 shell。
     