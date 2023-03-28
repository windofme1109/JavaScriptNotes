<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [1. 使用git过程中遇到的问题以及解决方式](#1-%E4%BD%BF%E7%94%A8git%E8%BF%87%E7%A8%8B%E4%B8%AD%E9%81%87%E5%88%B0%E7%9A%84%E9%97%AE%E9%A2%98%E4%BB%A5%E5%8F%8A%E8%A7%A3%E5%86%B3%E6%96%B9%E5%BC%8F)
  - [1.1 在使用`git pull`过程中，提示：错误 Please commit your changes or stash them before you merge.](#11-%E5%9C%A8%E4%BD%BF%E7%94%A8git-pull%E8%BF%87%E7%A8%8B%E4%B8%AD%E6%8F%90%E7%A4%BA%E9%94%99%E8%AF%AF-please-commit-your-changes-or-stash-them-before-you-merge)
- [2. git知识积累](#2-git%E7%9F%A5%E8%AF%86%E7%A7%AF%E7%B4%AF)
  - [2.1 git fetch和git pull的用法以及区别](#21-git-fetch%E5%92%8Cgit-pull%E7%9A%84%E7%94%A8%E6%B3%95%E4%BB%A5%E5%8F%8A%E5%8C%BA%E5%88%AB)
  - [2.2 从远端仓库拉取代码到本地仓库（非master分支）](#22-%E4%BB%8E%E8%BF%9C%E7%AB%AF%E4%BB%93%E5%BA%93%E6%8B%89%E5%8F%96%E4%BB%A3%E7%A0%81%E5%88%B0%E6%9C%AC%E5%9C%B0%E4%BB%93%E5%BA%93%E9%9D%9Emaster%E5%88%86%E6%94%AF)
  - [2.3 从远端仓库拉取代码到本地仓库（master分支）](#23-%E4%BB%8E%E8%BF%9C%E7%AB%AF%E4%BB%93%E5%BA%93%E6%8B%89%E5%8F%96%E4%BB%A3%E7%A0%81%E5%88%B0%E6%9C%AC%E5%9C%B0%E4%BB%93%E5%BA%93master%E5%88%86%E6%94%AF)
  - [2.4 同时使用github和gitlab](#24-%E5%90%8C%E6%97%B6%E4%BD%BF%E7%94%A8github%E5%92%8Cgitlab)
  - [2.5 如何在某个分支下建立自己的分支](#25-%E5%A6%82%E4%BD%95%E5%9C%A8%E6%9F%90%E4%B8%AA%E5%88%86%E6%94%AF%E4%B8%8B%E5%BB%BA%E7%AB%8B%E8%87%AA%E5%B7%B1%E7%9A%84%E5%88%86%E6%94%AF)
  - [2.6 修改分支名](#26-%E4%BF%AE%E6%94%B9%E5%88%86%E6%94%AF%E5%90%8D)
  - [2.7 版本回退（简单版）- reset](#27-%E7%89%88%E6%9C%AC%E5%9B%9E%E9%80%80%E7%AE%80%E5%8D%95%E7%89%88--reset)
  - [2.8 版本回退（复杂版）- revert](#28-%E7%89%88%E6%9C%AC%E5%9B%9E%E9%80%80%E5%A4%8D%E6%9D%82%E7%89%88--revert)
  - [2.9 删除某一分支](#29-%E5%88%A0%E9%99%A4%E6%9F%90%E4%B8%80%E5%88%86%E6%94%AF)
  - [2.10 提交所有文件](#210-%E6%8F%90%E4%BA%A4%E6%89%80%E6%9C%89%E6%96%87%E4%BB%B6)
  - [2.11 删除暂存区的文件](#211-%E5%88%A0%E9%99%A4%E6%9A%82%E5%AD%98%E5%8C%BA%E7%9A%84%E6%96%87%E4%BB%B6)
  - [2.12 合并某次提交到另外一个分支上](#212-%E5%90%88%E5%B9%B6%E6%9F%90%E6%AC%A1%E6%8F%90%E4%BA%A4%E5%88%B0%E5%8F%A6%E5%A4%96%E4%B8%80%E4%B8%AA%E5%88%86%E6%94%AF%E4%B8%8A)
  - [2.13 `.ssh` 目录下的 `config` 文件的配置项](#213-ssh-%E7%9B%AE%E5%BD%95%E4%B8%8B%E7%9A%84-config-%E6%96%87%E4%BB%B6%E7%9A%84%E9%85%8D%E7%BD%AE%E9%A1%B9)
  - [2.14 同一台电脑上配置两个 github 账号，并相互不影响](#214-%E5%90%8C%E4%B8%80%E5%8F%B0%E7%94%B5%E8%84%91%E4%B8%8A%E9%85%8D%E7%BD%AE%E4%B8%A4%E4%B8%AA-github-%E8%B4%A6%E5%8F%B7%E5%B9%B6%E7%9B%B8%E4%BA%92%E4%B8%8D%E5%BD%B1%E5%93%8D)
    - [1. 参考资料](#1-%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99)
    - [2. 具体步骤](#2-%E5%85%B7%E4%BD%93%E6%AD%A5%E9%AA%A4)
      - [1. 配置 SSH Key](#1-%E9%85%8D%E7%BD%AE-ssh-key)
      - [2. 配置 .config](#2-%E9%85%8D%E7%BD%AE-config)
      - [在 github 上添加 SSH Key](#%E5%9C%A8-github-%E4%B8%8A%E6%B7%BB%E5%8A%A0-ssh-key)
      - [测试](#%E6%B5%8B%E8%AF%95)
      - [应用](#%E5%BA%94%E7%94%A8)
  - [2.15 kex_exchange_identification: Connection closed by remote host](#215-kex_exchange_identification-connection-closed-by-remote-host)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## 1. 使用git过程中遇到的问题以及解决方式

### 1.1 在使用`git pull`过程中，提示：错误 Please commit your changes or stash them before you merge.
- 原因：本地代码未及时提交，在和远端代码合并时，出现了冲突
- 解决方法：参考文章：[Git冲突：commit your changes or stash them before you can merge.](https://blog.csdn.net/lincyang/article/details/21519333)
  1. 使用`stash`命令
     ```
      git stash
      git pull
      git stash pop  
     ```
  2. 放弃本地修改，直接覆盖
     ```
      git reset --hard
      git pull
     ```
- 使用第二种方法，在使用之前，先将本地写好的代码备份，然后直接从远端的仓库拉过来，将本地代码覆盖，然后再将备份的代码写到文件中去。
     
## 2. git知识积累

### 2.1 git fetch和git pull的用法以及区别

1. 参考资料：[git fetch & pull详解](https://www.cnblogs.com/runnerjack/p/9342362.html)
2. 一张图展示git fetch和git pull的概念：
   - 图示：
   
     ![git-fetch&pull](https://github.com/windofme1109/JavaScriptNotes/blob/master/img/git-fetch%26pull.jpg)
     
     ![](img/git-fetch&pull.jpg)
     
3. `git fetch`
   - 命令的基本格式1：`git fetch <remote> <branch>`，`<remote>`表示远端的主机名，`<branch>`表示远端的分支名称。例如，分支为master，则可以这样写：`git fetch origin master`，如果是其他分支，如develop，可以这样写：`git fetch origin develop`。
   - 命令的基本格式2：` git fetch <remote>`，这个命令将某个远程主机的更新全部取回本地，例如：`git fetch origin`。
   - 作用：git fetch将远端分支的最新内容拉到本地，用户再检查了内容以后，在决定是否与本地分支进行合并。
   - 取回更新后，会返回一个FETCH_HEAD，指的是某个branch在服务器上的最新状态，我们可以在本地通过它查看刚取回的更新信息：`git log -p FETCH_HEAD`，如图所示：
      ![](img/git-log-p.png)
   - 返回的信息包括更新的文件名，更新的作者和时间，以及更新的代码（**红色**部分表示删除，**绿色**部分表示新增）。通过这些信息来判断是否产生冲突，以确定是否将更新merge到当前分支。 
4. `git pull`
   - 命令的基本格式1：`git pull <remote> <branch>`，`<remote>`表示远端的主机名，`<branch>`表示远端的分支名称。例如，分支为master，则可以这样写：`git pull origin master`，如果是其他分支，如develop，可以这样写：`git pull origin develop`。
   - 命令的基本格式2：` git pull`，从远端仓库下载到本地仓库（从哪个分支下，就从远端仓库的那个分支下载）。
   - 作用：将远端分支的最新内容拉取到本地，并进行合并。有可能会出现冲突，需要手动解决。
   - git pull等同于：
     ```
        git fetch origin master
        git merge FETCH_HEAD
     ```
   - 第一条命令的作用是将远端仓库的master分支的最新内容拉取到本地。
   - 第二条命令的作用是将拉取到的最新内容合并到当前所处的本地分支中。
   
   
### 2.2 从远端仓库拉取代码到本地仓库（非master分支）
1. 初始化： `git init`

2. 与远端仓库建类连接：`git remote add origin git@github.com:XXXX/nothing2.git`

3. 把远程分支拉到本地：`git fetch origin dev`，dev为远端分支的名称

4. 在本地新建分支dev并切换到该分支：`git checkout -b dev(本地分支) origin/dev(远端分支)`

5. 把某个分支上的内容都拉取到本地：`git pull origin dev`，dev为远端仓库的名称

### 2.3 从远端仓库拉取代码到本地仓库（master分支）
1. 初始化： `git init`

2. 与远端仓库建类连接：`git remote add origin git@github.com:XXXX/nothing2.git`

3. 把远程分支拉到本地：`git fetch origin master`

4. 把某个分支上的内容都拉取到本地：`git pull origin master`

### 2.4 同时使用github和gitlab
1. 背景：在公司的电脑上需要同时使用github和gitlab，因此需要进行配置。

2. 首先配置全局的用户名和邮箱，这个设置为gitlab的用户名和邮箱，保证在任何项目下都可以连接到gitlab。
   - `git config --global user.name "name"` 全局配置gitlab的用户名
   - `git config --global user.email "email"` 全局配置gitlab的邮箱
   
3. 在需要上传到github的项目下，配置局部的用户名和邮箱，这个设置为github的用户名和邮箱。
   - `git config --local user.name "name"` 局部配置github的用户名
   - `git config --local user.email "email"` 局部配置github的邮箱
   
4. 配置ssh key。这个需要分别进行配置，也就是说，需要配置gitlab和github两个。

5. ssh key 默认生成后保存在 `~/.ssh/`目录下（Windows 10 的路径为：`C:\Users\Administrator\.ssh`），默认为 `id_rsa` 和 `id_rsa.pub` 两个文件，由于我们需要分开配置，配置步骤如下：
   1. `ssh-keygen -t rsa -f ~/.ssh/id_rsa.github -C "xxx@163.com"`，`xxx@163.com`是你的github的邮箱。这条命令在生成公私密钥对的同时制定文件名，表示github专用。`~/.ssh/id_rsa.github`表示生成的文件路径，包括文件名是`id_rsa.github`。
   
   2. `ssh-keygen -t rsa -C "aaa@xxx.com"`，`aaa@xxx.com`是你的gitlab的邮箱。这条命令用于生成默认的gitlab的公私密钥对。**注意：如果在最开始使用git的时候，已经生成了默认的gitlab公私密钥对，则这里不需要执行这个命令。**
   
6. 命令执行完成后，这时~/.ssh目录下会多出id_rsa.github和id_rsa.github.pub两个文件，id_rsa.github.pub 里保存的就是我们要使用的key，这个key就是用来上传到 Github上的。
   
7. 配置 config 文件
   - 在`~/.ssh`目录下，如果不存在config文件，则新建 config文件 ，文件内容添加如下：
      ```
        Host github.com
        IdentityFile ~/.ssh/id_rsa.github
        User xxx
      ```
      - 配置完成后，符合 *.github.com 后缀的 Git 仓库，均采取~/.ssh/id_rsa.github 密钥进行验证，其它的采取默认的。User后面的内容是你的github的用户名。
      
8. 上传public key 到 Github
   1. 登录github
   2. 点击右上方的Accounting settings图标
   3. 选择 SSH key
   4. 点击 Add SSH key
   5. 在出现的界面中填写SSH key的名称，填一个你自己喜欢的名称即可，然后将上面拷贝的`~/.ssh/id_rsa.github.pub`文件内容粘帖到key一栏，在点击“add key”按钮就可以了。
   6. 添加过程github会提示你输入一次你的github密码。
   
9. 验证是否OK
   - 由于每个托管商的仓库都有唯一的后缀，比如 Github的是 git@github.com:*，所以可以这样测试：`ssh -T git@github.com`，提示下面的信息：
   ```
        Hi windofme1109! You've successfully authenticated, but GitHub does not provide shell access.
   ```
   - 表示连接成功了。

### 2.5 如何在某个分支下建立自己的分支
1. 强调一个概念，比如说我们在A分支下建立B分支，指的是B分支使用了当前状态下，A分支的最新的代码，而不是A/B这种层级结构。
2. 我想在非主分支dev下新建一个分支：0831-dev，那么我们可以这样建立：
   1. 创建分支  
      - `git checkout -b 0831-dev`    创建并切换到新的的分支：0831-dev
   2. 定位分支  
      - `git checkout dev` 切换到dev下面，选择dev分支的状态内容  
      - `git pull origin dev`  拉取目标分支dev内容到本地分支
   3. 提交分支
      - `git checkout 0831-dev` 切换到0831-dev分支
      - `git add`
      - `git push -u origin 0831-dev` 将远端的0831-dev分支设置为本地分支0831-dev的上游。
3. 还可以在gitlab或者时github上基于某个分支建立自己的分支，然后将这个分支拉到本地。github或者gitlab上新建分支，都是基于某个分支建立的。

4. 实际上还有一种方法，就是直接在某个分支下，比如说是dev分支下，执行：`git checkout -b 0831-dev`，那么0831-dev就是基于dev这个分支建立的。  
   然后我们修改内容提交后，执行：`git push -u origin 0831-dev`，就能将当前的自己的分支推送到远端仓库。  
   如果我们需要将两个分支合并，使用`git merge`或者在webStorm中进行合并。
  
     
     
### 2.6 修改分支名

1. 假设原分支的名字是 oldName，要修改为 newName。

2. 修改本地分支的名称（未推送到远端）
   - `git branch -m oldName newName`
   
3. 远程分支重命名 （已经推送远程-假设本地分支和远程对应分支名称相同）
   1. 修改本地分支名称
      - `git branch -m oldName newName`
   
   2. 删除远端分支
      - `git push -delete origin oldName`
   
   3. 上传新分支
      - `git push --delete origin oldName`
      
   4. 把修改后的本地分支与远端分支关联
      - `git push --set-upstream-to origin/newName`

### 2.7 版本回退（简单版）- reset

1. 背景：假设我们已经进行了几次提交，但是发现提交有问题，所以我们想回退到某个版本，放弃之前的提交。

2. 可以这样进行版本回退：
   
   1. 查看最近几次提交的哈希值
      - `git log`  
      
      哈希值如下图所示：
      ![](./img/git-log.png)
   
   2. 记录下需要回退到某次提交（版本）的哈希值
   
   3. 版本回退
      - `git reset --hard 哈希值`  
      `--hard` 表示删除前面的提交记录。而不是指针回退到当前的提交。
      
   4. 推送到远端，使得本地与远端保持同步
      - `git push -f origin 分支名`  
      `-f` 表示强制推送到远端，以覆盖之前的提交。

3. 如果只是简单的回退到上个版本，可以使用下面这个命令：
   - `git reset --hard head^`  
   `HEAD^` 表示上一个版本

4. `reset` 主要是将 head 指针回退到某次提交处，这次提交后面的的提交都不要了。 


### 2.8 版本回退（复杂版）- revert

1. `git revert` 是用于回滚某一个版本，以达到撤销该版本的修改的目的。比如，我们提交了三个版本（版本一、版本二、 版本三），突然发现版本二不行（如：有 bug），想要撤销版本二，但又不想影响撤销版本三的提交，就可以用 `git revert` 命令来回滚版本二，生成新的版本四，这个版本四里会保留版本三的东西，但撤销了版本二的东西。如下图所示：
   ![](./img/git-revert-1.png)

2. 适用场景：如果我们想撤销之前的某一版本，但是又想保留该目标版本后面的版本，记录下这整个版本变动流程，就可以用这种方法。

3. 具体步骤：
   1. 查看版本号：`git log`:  
      ![](./img/git-revert-2.png)  
      发现 `git revert 命令 - 3` 这次提交有问题，想要撤销。找到这一次提交的 hash 值：`a3f415087eb98aa96993b1638c92f72ebc2aa94c`
   2. 使用 `git revert -n 版本号` 回滚，并使用 `git commit -m 版本名` 提交。
      1. 回滚：`git revert -n a3f415087eb98aa96993b1638c92f72ebc2aa94c`
         这里可能会出现冲突，那么需要手动修改冲突的文件。而且要 `git add 文件名`。即添加解决完冲突的文件到暂存区，这样才能进行提交。
      2. 提交：`git commit -m "revert git revert 命令 - 3"`
   3. 此时可以用 `git log` 查看本地的版本信息，可见多生成了一个新的版本，该版本反做了 `git revert 命令 - 3` 版本，但是保留了其后面的版本：
      ![](./img/git-revert-3.png)
   4. 将本地提交推送到远端：`git push`。

4. `revert` 命令使得 head 指针继续向前，需要撤销的提交后面的提交依旧保留。

5. 参考资料：
   1. [Git恢复之前版本的两种方法reset、revert（图文详解）](https://blog.csdn.net/yxlshk/article/details/79944535)
   2. [git（五）回退reset与revert](https://blog.csdn.net/LS7011846/article/details/105353309)
   3. [代码回滚：git reset、git checkout和git revert区别和联系](https://www.cnblogs.com/houpeiyong/p/5890748.html)

### 2.9 删除某一分支

1. 某个分支由于某些原因，不想要了，只能删除。比如说我现在在 dev 分支上，想要删除 dev 分支。

2. 切到其他分支，如 release 上，执行：`git checkout release`

3. 删除本地的 dev 分支：`git branch -d dev`

4. 如果删除不了，可以强制删除：`git branch -D dev`

5. 有必要的情况下，删除远端分支（**慎用**）：`git push origin --delete dev`


### 2.10 提交所有文件

1. 添加所有变化：`git add -A`  

2. 添加被修改（modified）和被删除（deleted）文件，不包括新文件（new）：`git add -u`
  
3. 添加新文件（new）和被修改（modified）文件，不包括被删除（deleted）文件：`git add .`  

### 2.11 删除暂存区的文件

1. 有时我们误把文件使用`git add` 添加到暂存区，如果想把它从暂存区移除，但不是删掉文件，可以使用 `reset` 命令。

2. 从暂存区移除文件：`git reset HEAD -- path/to/file`

3. 从暂存区移除文件夹下的所有文件
   - 如果要移除文件夹下的所有文件，可以进入到文件夹，然后对文件夹执行：`git reset HEAD -- .`
   
4. 还可以使用 rm 命令：`git rm --cache file`

### 2.12 合并某次提交到另外一个分支上

1. 有时候，我们不需要合并两个分支，而是将某个分支的一个或几个提交（commit）合并到另外一个分支上，这时候就需要 `cherry-pick` 命令了。

2. 合并步骤（以将 develop 某个提交合并到 master 分支为例）：
   1. 切换到 `develop` 分支，输入 `git log`，查找需要合并的 commit 记录，即 hash 值，如：7fcb3defff。
   2. 切换到 `master` 分支，使用 `git cherry-pick 7fcb3defff` 命令，就把该条提交记录合并到了 `master` 分支，这只是在本地合并到了 `master` 分支。
   3. 使用 `git push` 命令，推送到远端仓库。

### 2.13 `.ssh` 目录下的 `config` 文件的配置项

1. `.ssh` 目录是用来存放同 ssh 加密相关的内容，如生成的公钥、私钥等。

2. 参考文档
   - [使用 SSH config 文件](https://blog.csdn.net/weixin_34273046/article/details/92211773)
   - [使用ssh config配置文件来管理ssh连接](https://www.cnblogs.com/piperck/p/6188984.html)

3. config 是一个配置文件，配置项用来管理多个公钥私钥对、ssh 账号密码等。我们使用 git 时，可能有多个账号，每个账号对应着公钥和私钥，也可能同时使用 github、gitlab 等，这样就需配置文件  config 统一管理这些信息。

4. git 在本地提交到 github 或 gitlab 上时，会读取 `.ssh` 目录下的公钥秘钥信息。如果在 `~/.ssh/` 目录下有 config 文件，则会优先读取 config 的配置信息，否则直接读取 `id_rsa` 和 `id_rsa.pub`。

5. config 中的配置项说明如下：

   配置项|说明
   :---:|:---:
   Host | 别名
   HostName | 主机名
   Port | 端口
   User | 用户名
   IdentityFile | 密钥文件的路径
   IdentitiesOnly | 只接受 SSH key 登录
   PreferredAuthentications | 强制使用 Public Key验证

6. 可以通过 `man ssh_config` 这个命令，查看 `~/.ssh/config` 的语法。

7. 注意：配置了 `User`，`git push` 时就一定要配置相同的用户名
如果不设置 `User`，随意一个 `user.name` 都可以提交 `git push`
如果设置了`User`，`user.name` 必须匹配到 config 中的 `User` 才能提交。

8. config 文件示例：
   ```
      Host github.com
      User windofme1109
      IdentityFile ~/.ssh/id_rsa.github

      Host gitee.com
      User windofme1109
      IdentityFile ~/.ssh/id_rsa.github

      Host outergit.yonyou.com
      User qinmr
      IdentityFile ~/.ssh/id_rsa
      Port 49622
   ```
   
### 2.14 同一台电脑上配置两个 github 账号，并相互不影响

#### 1. 参考资料

1. [git使用中遇到的remote：Permission to xxx denied to xxx问题如何解决](https://blog.csdn.net/lwc863481702/article/details/78542727?locationNum=5&fps=1)

2. [Git 最著名报错 “ERROR: Permission to XXX.git denied to user”终极解决方案](https://www.jianshu.com/p/12badb7e6c10)

3. [一台电脑上的git同时使用两个github账户](https://www.dandelioncloud.cn/article/details/1501140694386421762)

4. [如何在一台电脑上同时使用两个 github 帐号](https://www.jianshu.com/p/0be98d91d294)

5. [同一台电脑上使用两个 github 账号](https://blog.csdn.net/sihai12345/article/details/118858582)

#### 2. 具体步骤

##### 1. 配置 SSH Key
1. 假设说，在一台电脑上，要同时使用 one 和 two 两个 github 账号，注意，两个账号都是 github 账号。在同一台设备上，假设只有账号 one 的 ssh key，即使把账号 one 的 ssh key 配置到 two 的 github 上面，在使用账号 two 进行 git push 操作时，由于 ssh key 的不匹配（同一台设备的不同账号生成的 ssh key 也不同），推送会失败。 所以首先要根据两个不同的账号生成生成不同的 ssh key。
```shell
ssh-keygen -t rsa -f ~/.ssh/id_rsa.one -C "one@xxx.com"
ssh-keygen -t rsa -f ~/.ssh/id_rsa.two -C "two@xxx.com"
```

2. 在 `.ssh` 下生成 4 个文件：
```shell
   id_rsa.one  // 账号 one 的私钥
   id_rsa.two  // 账号 two 的私钥
   id_rsa.one.pub  // 账号 one 的公钥
   id_rsa.two.pub  // 账号 two 的公钥
```

##### 2. 配置 .config

3. 在 `.ssh` 创建一个 `.config` 文件，内容如下：
```shell
      # one 
      Host one.github.com
      HostName github.com
      User one
      IdentityFile ~/.ssh/id_rsa.one

      Host two.github.com
      HostName github.com
      User two
      IdentityFile ~/.ssh/id_rsa.two

```

4. `.config` 文件的配置中，`Host` 参数是主机别名，这个是用来区分当前仓库的 git 地址是属于哪个账号的。User 参数用来匹配当前提交的用户名，IdentityFile 是指定 ssh key 的位置。

5. 就是说，为每个 github 账号单独配置一组配置项，包括：Host、HostName、User、和 IndentityFile。

6. `.config` 文件的配置中，每个账号下的 `Host` 参数会影响 git 的相关命令，主要是仓库 git 地址的问题，如账号 one 配置的 Host 是 `one.github.com`，使用 git clone 命令，克隆的地址是：`git@github.com:one/JavaScriptNotes.git`，那么此时的克隆的地址就要变成：`git@one.github.com:one/JavaScriptNotes.git`。

##### 在 github 上添加 SSH Key

7. 在 `github` 上，添加我们刚刚生成的 ssh key。注意，要分别配置账号 one 和账号 two 的 ssh key。步骤如下：
   - 登陆 github 账号
   - 进入Settings –> SSH and GPG keys
   - 点击 new SSH key 
   - 把 .ssh 下的公钥（.pub文件）的内容添进去，其中 Title 为自定义的名字，Key 为.pub 文件的内容
   - 最后点击 Add SSH key 即可。
![img.png](./img/addSSHKey.png)

##### 测试

8. 输入以下命令进行测试
```shell
ssh -T git@one.github.com
ssh -T git@two.github.com
```
9. 此处 `one.github.com` 和 `two.github.com` 为 `.config` 文件中配置的 `Host`。

10. 运行命令后如果出现 `Hi xxxx! You’ve successfully authenticated, but GitHub does not provide shell access.`，其中 `xxxx` 为 `.config` 文件中配置的User 名字，这种情况下，表示我们已经配置成功了。


##### 应用


1. 远端仓库的地址需要进行修改。原来的写法为：`git@github.com:one的用户名/learngit.git`
2. 现在的写法为：`git@one.github.com:one的用户名/learngit.git`

3. 即将原来的 `github.com` 替换为 `.config` 配置文件中的 `Host` 配置项（与用户名对应的 `Host`）。

4. 以 git clone 命令为例：
   - 原来的写法：
   ```
      git clone git@github.com:two的用户名/learningGit.git
      git clone git@github.com:two的用户名/learningVue.git
   ```
   - 现在的写法：
   ```
      git clone git@one.github.com:one的用户名/learningGit.git
      git clone git@one.github.com:one的用户名/learningVue.git

   ```
5. 还需要同步修改本地仓库中配置的远端仓库的地址：
   - 查看远端仓库的地址：`git remote -v`
   - 删除远端仓库的地址：`git remote rm origin`
   - 添加新的远端仓库的地址：`git remote add origin git@one.github.com:one的用户名/learningGit.git`

6. 为每个本地仓库设置局部的用户名和邮箱：
    - 设置账号 one 的用户名和 email：
      ```shell
         git config user.name "one_name" 
         git config user.email "one_email"
      ```
    - 设置账号 two 的用户名和 email：
      ```shell
         git config user.name "two_name" ; 
         git config user.email "two_email"
      ```

7. 取消全局配置的用户名和邮箱
```shell
   git config --global --unset user.name
   git config --global --unset user.email
```


### 2.15 kex_exchange_identification: Connection closed by remote host

1. 使用 git 从 github 上拉取代码的时候，拉取失败，出现下面的信息：
```bash
kex_exchange_identification: Connection closed by remote host
Connection closed by 20.205.243.166 port 22
fatal: Could not read from remote repository.

Please make sure you have the correct access rights 
```
2. 大意是被远程主机关闭了连接。

3. 回忆最近都干了啥：
   - 很久没有用这台电脑拉取代码了
   - 使用了代理

4. 尝试删除原来的 ssh key，生成一个新的 ssh key，然后添加到 github，但是依旧报错。

5. 上网查询，有人说可能是代理的问题，就是代理禁掉了 github 的 ssh 连接（可能和 22 端口有关）。所以，关掉代理试一试，结果可以正常拉取代码。

6. 在关掉代理的同时，我还删除了 `~\.ssh` 目录下的 known_hosts 文件，我看网上有人说删掉这个文件，然后重启终端，也能正常拉取代码。

7. 出现下面的内容：
```
The authenticity of host 'github.com (20.205.243.166)' can't be established.
ECDSA key fingerprint is SHA256:p2QAMXNIC1TJYWeIOttrVc98/R1BUFWu3/LiyKgUfQM.
Are you sure you want to continue connecting (yes/no/[fingerprint])? y
Please type 'yes', 'no' or the fingerprint: yes
Warning: Permanently added 'github.com,20.205.243.166' (ECDSA) to the list of known hosts.

```

8. 也就是说，确认了 fingerprint 以后，将 github 的一个 host 地址添加到 known_hosts 文件中了。

9. 目前不太知道是哪种方式起了作用。

10. 因为关了代理，访问 github 成了问题，所以，代理还是得开启，那么可能又出现前面的报错，那么此时应该怎么办呢，答案就是在 `~\.ssh` 下增加一个 `config` 文件，添加如下内容：
```
# windofme1109 - github
Host github.com
HostName github.com
# vpn
port 443

```

9. 这样既可以使用代理，又可以从 github 上拉取代码。

10. 然而，我按照上面的方式进行配置，发现又报错了：
```
   kex_exchange_identification: Connection closed by remote host
Connection closed by 20.205.243.166 port 443
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.

```

11. 去掉这个端口的配置，就能正常拉取代码了。

12. 看来应该是删掉 known_hosts 文件起了作用。

13. 总结出现 `kex_exchange_identification: Connection closed by remote host` 的解决方法： 
    - 删除原来的 ssh key，新建一个 ssh key，并添加到 github。
    - 删掉 known_hosts 试一试。
    - 如果使用了代理，那么先关掉代理试一试，然后在 `~\.ssh` 下的 `config` 文件中添加端口为 443。
    

