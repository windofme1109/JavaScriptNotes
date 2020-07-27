### 1. 使用git过程中遇到的问题以及解决方式
#### 1.1 在使用`git pull`过程中，提示：错误 Please commit your changes or stash them before you merge.
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

### 1.2 从远端仓库拉取代码到本地仓库（非master分支）
1. 初始化： `git init`

2. 与远端仓库建类连接：`git remote add origin git@github.com:XXXX/nothing2.git`

3. 把远程分支拉到本地：`git fetch origin dev`，dev为远端分支的名称

4. 在本地新建分支dev并切换到该分支：`git checkout -b dev(本地分支) origin/dev(远端分支)`

5. 把某个分支上的内容都拉取到本地：`git pull origin dev`，dev为远端仓库的名称

### 1.3 从远端仓库拉取代码到本地仓库（master分支）
1. 初始化： `git init`

2. 与远端仓库建类连接：`git remote add origin git@github.com:XXXX/nothing2.git`

3. 把远程分支拉到本地：`git fetch origin master`

4. 把某个分支上的内容都拉取到本地：`git pull origin master`

### 1.4 同时使用github和gitlab
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
     
