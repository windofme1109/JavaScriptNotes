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
4. 