# Git 基本使用方式

## 1. 基本说明

1. Git，世界上最先进的分布式版本控制管理工具。

2. 我们已经在电脑上安装了 Git。安装过程省略，下面开始介绍Git的基本用法。

3. 注意，我们使用的是 git bash 这个终端工具进行 git 命令操作。不使用 windows 自带的命令提示符。

## 2. 配置 Git

### 1. 设置姓名和邮箱地址

1. 命令：
   - `git config --global user.name "github用户名"`
   - `git config --global user.email "注册github所用的邮箱"`

2. 用自己的 github 用户名和注册邮箱其替换。这个命令设置的是全局的用户名和邮箱，意思就是所有的本地仓库都会使用。如果只想单独设置某个仓库的用户名和邮箱，将 `global` 参数改动为 `local`。

3. 这个命令，会在 `~/.gitconfig` 中以如下形式输出设置文件。这个文件的在 `C:\Users\Administrator.USER-20181123NM` 目录下。

### 2. 设置SSH Key

1. GitHub 上连接已有仓库时的认证，是通过使用了 SSH 的公开密钥认证方式进行的。现在让我们来创建公开密钥认证所需的 SSH Key，并将其添加至 GitHub。

2. `输入：ssh-keygen -t rsa -C "注册github所用的邮箱"`

3. 然后会出现：
`Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/Administrator.USER-20181123NM/.ssh/id_rsa):`

4. 此时按下回车键，会将 SSH Key 保存在 `C:/Users/Administrator.USER-20181123NM/.ssh` 这个目录下，同时提示：
`Created directory '/c/Users/Administrator.USER-20181123NM/.ssh'.`

5. 紧接着会提示：
`Enter passphrase (empty for no passphrase):`
这一步告诉我们要设置一个密码，我们可以不用设置，直接按回车键即可。
`Enter same passphrase again:`
同样，让我们确认密码，由于我们没有输入，所在也是直接按下回车键。

6. 完成上面三步以后，会出现一下结果：
   ```
      Your identification has been saved in /c/Users/Administrator.USER-20181123NM/.ssh/id_rsa.
      Your public key has been saved in /c/Users/Administrator.USER-20181123NM/.ssh/id_rsa.pub.
      The key fingerprint is:
   ```
      `fingerprint值`
   ```
      The key's randomart image is:
      +---[RSA 3072]----+
      |.o  ..+.         |
      |B o. +. .        |
      |oX.. oE.  .      |
      |+o.+..o. o .     |
      |  + + +.S.o+ .   |
      |     o =..+.B    |
      |      o + .+.+   |
      |     . o o o...  |
      |      +o... o.   |
      +----[SHA256]-----+
   ```
7. 添加公开秘钥。在 GitHub 中添加公开密钥，今后就可以用私有密钥进行认证了。



