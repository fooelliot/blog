# Git 常用命令详解
- Git 是一个很强大的分布式版本控制系统。它不但适用于管理大型开源软件的源代码，管理私人的文档和源代码也有很多优势。

## 1.Git文件操作
```
$ git help [command]                  # 显示command的help
$ git show [$id]                      # 显示某次提交的内容 
$ git checkout  [file]                # 抛弃工作区修改
$ git checkout .                      # 抛弃工作区修改
$ git add [file]                      # 将工作文件修改提交到本地暂存区
$ git add .                           # 将所有修改过的工作文件提交暂存区
$ git rm [file]                       # 从版本库中删除文件
$ git rm --cached [file]              # 从版本库中删除文件，但不删除文件
$ git reset [file]                    # 从暂存区恢复到工作文件
$ git reset -- .                      # 从暂存区恢复到工作文件
$ git reset --mixed HEAD~1            # 修改版本库，修改暂存区，保留工作区
$ git reset --soft HEAD~2             # 修改版本库，保留暂存区，保留工作区
$ git reset --hard HEAD~3             # 修改版本库，修改暂存区，修改工作区
$ git commit --amend                  # 修改最后一次提交记录
$ git revert [$id]                    # 恢复某次提交的状态，恢复动作本身也创建次提交对象
$ git revert HEAD                     # 恢复最后一次提交的状态
$ git status                          # 查看当前工作区状态 
$ git config --list                   # 看所有用户
$ git ls-files                        # 查看已经被提交的文件
$ git commit -a                       # 提交当前repos的所有的改变
$ git commit -v                       # 参数-v可以查看看commit的差异
$ git commit -m "message"             # 提交到本地并添加提交信息
$ git commit -am "init"               # 添加并提交 

$ git log [file]                      # 查看该文件每次提交记录
$ git log -p [file]                   # 查看每次详细修改内容的diff
$ git log -p -2                       # 查看最近两次详细修改内容的diff
$ git log --stat                      # 查看提交统计信息

$ git rm --cached [file]              # 从暂存区中删除文件
$ git rm -f [file]                    # 强行移除版本控制的文件
$ git rm -r --cached [file]           # 递归删除-r，是文件夹的时候有用

$ git diff [file]                     # 比较当前文件和暂存区文件差异
$ git diff [$id] [$id]                # 比较两次提交之间的差异
$ git diff [branch1]..[branch2]       # 在两个分支之间比较
$ git diff --staged                   # 比较暂存区和版本库差异
$ git diff --cached                   # 比较暂存区和工作区差异
$ git diff --stat                     # 仅仅比较统计信息

$ git stash                           # 暂存当前正在进行的工作
$ git stash push                      # 将暂存给push到一个临时空间中
$ git stash list                      # 查看所有缓存的代码
$ git stash clear                     # 清空缓存区内容
$ git stash drop                      # 移除某个贮藏
$ git stash save                      # 指定message
$ git stash show                      # 查看指定stash的diff
$ git stash apply stash@{1}           # 取出指定的缓存代码
$ git stash pop                       # 将文件从临时空间pop下来
$ git fetch origin dev:dev            # 获取最新版本到本地不会自动merge
$ git pull origin master:master       # 获取最新版本到本地会自动merge
```

## 2.Git分支操作相关命令
```
$ git branch                          # 查看分支
$ git branch [dev] [master]           # 在master创建dev分支
$ git branch -v                       # 查看各个分支最后提交信息
$ git branch -r                       # 查看远程分支信息
$ git branch -a                       # 查看所有分支信息
$ git branch -m [aaa] [bbb]           # 将aaa 重命名为bbb
$ git branch [name]                   # 从当前分支创建分支
$ git branch -d [branch]              # 删除某个分支
$ git branch -D [branch]              # 强制删除某个分支
$ git branch --set-upstream-to origin/test master # 本地分支和远程分支关联
$ git branch --set-upstream-to=origin/master help #
$ git branch --merged               # 查看已经被合并到当前分支的分支
$ git branch --no-merged            # 查看尚未被合并到当前分支的分支

$ git commit [$id] -b [new_branch]    # 从历史提交记录创建分支
$ git commit [$id]                    # 从历史提交记录checkout出来，但无分支信息，切换到其他分支会自动删除
$ git checkout [name]                 # 切换分支
$ git checkout --track origin/dev     # 切换到远程dev分支
$ git checkout -b [name]              # 从当前分支新建并切换到name
$ git checkout -b [new_br] [br]       # 基于branch创建新的new_branch
$ git merge origin/dev                # 将分支dev与当前分支进行合并
$ git merge [branch]                  # 将branch分支合并到当前分支
$ git merge origin/master --no-ff # 不要Fast-Foward合并，这样可以生成merge提交
$ git rebase master [branch] # 将master rebase到branch，相当于： git co [branch] && git rebase master && git co master && git merge [branch]
```

## 3.Git版本回退操作相关命令
```
HEAD ：当前版本
HEAD^ ：上一个版本
$ git log                               # 查看commit的信息
$ git log --pretty=oneline              # 单行展示历史记录
$ git log --oneline                     # 单行简单展示
$ git reflog                            # 查看命令历史记录 
$ git checkout .                        # 撤销所有本地改动代码
$ git checkout [file]                   # 撤销所有本地改动代码
$ git reset HEAD .                      # 撤销所有add文件 
$ git reset HEAD [file]                 # 撤销单个add文件
$ git reset --soft HEAD                 # 只回退commit的信息,保留修改代码
$ git reset --hard HEAD^                # 彻底回退到上次commit版本,不保留修改代码
$ git reset --hard [branch]             # 本地代码回退到与git远程仓库保持一致
--hard 参数会抛弃当前工作区的修改
--soft 参数的话会回退到之前的版本，但是保留当前工作区的修改，可以重新提交
$ git revert                            # 以前commit的id
```

## Git标签操作相关命令
$ git tag                       # 查看版本
$ git tag [name]                # 创建版本
$ git tag -d [name]             # 删除版本
$ git tag -r                    # 查看远程版本
$ git push origin [name]        # 创建远程版本(本地版本push到远程)
$ git push origin :refs/tags/[name]删除远程版本
$ git pull origin --tags        # 合并远程仓库的tag到本地
$ git push origin --tags        # 上传本地tag到远程仓库
$ git tag -a [name] -m [message]# 创建带注释的tag

## Git子模块(submodule)相关操作命令
添加子模块：$ git submodule add [url] [path]
如：$git submodule add git://github.com/soberh/ui-libs.git src/main/webapp/ui-libs
初始化子模块：$ git submodule init  ----只在首次检出仓库时运行一次就行
更新子模块：$ git submodule update ----每次更新或切换分支后都需要运行一下
删除子模块：（分4步走哦）
1) $ git rm --cached [path]
2) 编辑“.gitmodules”文件，将子模块的相关配置节点删除掉
3) 编辑“ .git/config”文件，将子模块的相关配置节点删除掉
4) 手动删除子模块残留的目录

## Git补丁管理
git diff ] ../sync.patch # 生成补丁
git apply ../sync.patch # 打补丁
git apply --check ../sync.patch #测试补丁能否成功

7)Git远程分支管理
$ git pull          # 抓取远程仓库所有分支更新并合并到本地
$ git pull --no-ff  # 抓取远程仓库所有分支更新并合并到本地，不要快进合并
$ git fetch origin  # 抓取远程仓库更新
$ git merge origin/master # 将远程主分支合并到本地当前分支
$ git clone --track origin/branch # 跟踪某个远程分支创建相应的本地分支
$ git clone -b [local_branch] origin/[remote_branch] # 基于远程分支创建本地分支，功能同上
$ git push # push所有分支
$ git push origin master # 将本地主分支推到远程主分支
$ git push -u origin master # 将本地主分支推到远程(如无远程主分支则创建，用于初始化远程仓库)
$ git clone [git@xxx]                          $ 克隆远程仓库
$ git remote -v                                $ 查看远程服务器地址和仓库名称
$ git remote show [name]                       $ 查看远程服务器仓库状态
$ git remote add [name] [url]                  $ 添加远程仓库地址
$ git remote rm [repository]                   $ 删除远程仓库
$ git remote set-url --push [name] [newUrl]    $ 修改远程仓库
$ git pull [rName] [lName]                     $ 拉取远程分支
$ git push [rName] [lName]                     $ 推送远程分支
$ git push origin [lname]:[rname]              $ 本地分支push到远程
$ git push origin -d [name]                    $ 删除远程分支-d也可以用--delete
$ git push -u origin master # 客户端首次提交
$ git remote set-head origin master # 设置远程仓库的HEAD指向master分支
$ git branch --set-upstream master origin/master

# 9.Git忽略一些文件、文件夹不提交
在仓库根目录下创建名称为“.gitignore”的文件，写入不需要的文件夹名或文件，每个元素占一行即可，如
target
bin
*.imi
.project
.classpath
.mvn
========================
-----------------------------------------------------------
$ mkdir WebApp
$ cd WebApp
$ git init
$ touch README
$ git add README
$ git commit -m 'first commit'
$ git remote add origin git@github.com:fooellot/WebApp.git
$ git push -u origin master
