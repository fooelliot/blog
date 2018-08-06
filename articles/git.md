# git 常用命令详解
## Git 是一个很强大的分布式版本控制系统。它不但适用于管理大型开源软件的源代码，管理私人的文档和源代码也有很多优势。
## 1) Git文件操作
```
$ git help [command]                        # 显示command的help
$ git show [$id]                              # 显示某次提交的内容 
$ git checkout  [file]                      # 抛弃工作区修改
$ git checkout .                            # 抛弃工作区修改
$ git add [file]                            # 将工作文件修改提交到本地暂存区
$ git add .                                 # 将所有修改过的工作文件提交暂存区
$ git rm [file]                             # 从版本库中删除文件
$ git rm --cached [file]                    # 从版本库中删除文件，但不删除文件
$ git reset <file> # 从暂存区恢复到工作文件
$ git reset -- . # 从暂存区恢复到工作文件
$ git reset --hard # 恢复最近一次提交过的状态，即放弃上次提交后的所有本次修改
$ git ci --amend # 修改最后一次提交记录
$ git revert <$id> # 恢复某次提交的状态，恢复动作本身也创建次提交对象
$ git revert HEAD # 恢复最后一次提交的状态
$ git status                                      查看当前工作区状态 
$ git config --list                               看所有用户
$ git ls-files                                    查看已经被提交的文件
$ git add [file name]                             添加一个文件到index
$ git add .                                       把当前文件夹下所有文件添加到git
$ git commit -a                                   提交当前repos的所有的改变
$ git commit -v                                   参数-v可以查看看commit的差异
$ git commit -m "message"                         提交到本地并添加提交信息
$ git commit -am "init"                           添加并提交 
$ git log                                         看你commit的日志
$ git diff --cached                               查看尚未暂存的更新
$ git rm --cached [file]                          移除文件(从暂存区中删除)
$ git rm -f [file]                                强行移除版本控制的文件
$ git rm -r --cached [file]                       递归删除-r，是文件夹的时候有用
$ git diff --cached 或 git diff --staged          查看尚未提交的更新
$ git stash                                       暂存当前正在进行的工作
$ git stash push                                  将暂存给push到一个临时空间中
$ git stash list                                  查看所有缓存的代码
$ git stash clear                                 清空缓存区内容
$ git stash apply stash@{1}                       取出指定的缓存代码
$ git stash pop                                   将文件从临时空间pop下来
$ git fetch                                       获取最新版本到本地不会自动merge
```
2）git(branch)操作相关命令
```
$ git branch                            查看分支
$ git branch -D master develop          删除本地库develop
$ git branch -d [name]                  删除已经参与了合并的分支
$ git branch [dev] [master]             在master创建dev分支
$ git branch -v                         查看各个分支最后提交信息
$ git branch -r                         查看远程分支信息
$ git branch -a                         查看所有分支信息
$ git branch -m [aaa] [bbb]             将aaa 重命名为bbb
$ git branch [name]                     创建本地分支
$ git checkout [name]                   切换分支
$ git checkout --track origin/dev       切换到远程dev分支
$ git checkout -b [name]                从当前分支新建并切换到name
$ git checkout -b <new_br> <br>         基于branch创建新的new_branch
$ git checkout -d <branch>              删除某个分支
$ git checkout -D <branch>              强制删除某个分支 (未被合并的分支被删除的时候需要强制)
$ git merge [name] 	                    将名称为[name]的分支与当前分支合并
$ git merge origin/dev                  将分支dev与当前分支进行合并
```
3)git(version)版本回退操作相关命令
```
HEAD ：当前版本
HEAD^ ：上一个版本
$ git log –pretty=oneline               来单行展示历史记录
$ git log –pretty=oneline –abbrev-commit用来显示简化commit id 
$ git reflog                              查看命令历史记录 
$ git checkout .                          撤销所有本地改动代码
$ git reset --hard                        撤销所有本地改动代码
[add 后撤销]
$ git reset HEAD .                        撤销所有add文件 
$ git reset HEAD -filename                撤销单个add文件
commit 后撤销：
$ git reset --soft head                   只回退commit的信息,保留修改代码
$ git reset --hard head^                  彻底回退到上次commit版本,不保留修改代码
$ git reset --hard 远程分支名             本地代码回退到与git远程仓库保持一致
--hard 参数会抛弃当前工作区的修改
--soft 参数的话会回退到之前的版本，但是保留当前工作区的修改，可以重新提交
[git push撤销]
回滚此次push到服务器的代码：
git log查看commit的信息
git revert 以前commit的id
git push 此时本地回滚的代码到服务器就可以了
[git merge 撤销]
$ git checkout 【行merge操作时所在的分支】
$ git reset --hard 【merge前的版本号】
```

4）版本(tag)操作相关命令
查看版本：$ git tag
创建版本：$ git tag [name]
删除版本：$ git tag -d [name]
查看远程版本：$ git tag -r
创建远程版本(本地版本push到远程)：$ git push origin [name]
删除远程版本：$ git push origin :refs/tags/[name]
合并远程仓库的tag到本地：$ git pull origin --tags
上传本地tag到远程仓库：$ git push origin --tags
创建带注释的tag：$ git tag -a [name] -m [message]

5) 子模块(submodule)相关操作命令
添加子模块：$ git submodule add [url] [path]
如：$git submodule add git://github.com/soberh/ui-libs.git src/main/webapp/ui-libs
初始化子模块：$ git submodule init  ----只在首次检出仓库时运行一次就行
更新子模块：$ git submodule update ----每次更新或切换分支后都需要运行一下
删除子模块：（分4步走哦）
1) $ git rm --cached [path]
2) 编辑“.gitmodules”文件，将子模块的相关配置节点删除掉
3) 编辑“ .git/config”文件，将子模块的相关配置节点删除掉
4) 手动删除子模块残留的目录


6）Git暂存管理
git stash # 暂存
git stash list # 列所有stash
git stash apply # 恢复暂存的内容
git stash drop # 删除暂存区


7)Git远程分支管理
git pull # 抓取远程仓库所有分支更新并合并到本地
git pull --no-ff # 抓取远程仓库所有分支更新并合并到本地，不要快进合并
git fetch origin # 抓取远程仓库更新
git merge origin/master # 将远程主分支合并到本地当前分支
git co --track origin/branch # 跟踪某个远程分支创建相应的本地分支
git co -b <local_branch> origin/<remote_branch> # 基于远程分支创建本地分支，功能同上
git push # push所有分支
git push origin master # 将本地主分支推到远程主分支
git push -u origin master # 将本地主分支推到远程(如无远程主分支则创建，用于初始化远程仓库)
git push origin <local_branch> # 创建远程分支， origin是远程仓库名
git push origin <local_branch>:<remote_branch> # 创建远程分支
git push origin :<remote_branch> #先删除本地分支(git br -d <branch>)，然后再push删除远程分支

$ git clone [git@xxx]                             克隆远程仓库
$ git remote -v                                   查看远程仓库
$ git remote show [name]                          查看远程仓库
$ git remote add [name] [url]                     添加远程仓库
$ git remote rm [name]                            删除远程仓库
$ git remote set-url --push [name] [newUrl]       修改远程仓库
$ git pull [rName] [lhName]                       拉取远程分支
$ git push [rName] [lName]                        推送远程分支
$ git push origin [lname]:[rname]                 本地分支push到远程
$ git push origin -d [name]                       删除远程分支-d也可以用--delete

8)Git远程仓库管理

git remote -v # 查看远程服务器地址和仓库名称

git remote show origin # 查看远程服务器仓库状态

git remote add origin git@ github:robbin/robbin_site.git # 添加远程仓库地址

git remote set-url origin git@ github.com:robbin/robbin_site.git # 设置远程仓库地址(用于修改远程仓库地址) git remote rm <repository> # 删除远程仓库

创建远程仓库

git clone --bare robbin_site robbin_site.git # 用带版本的项目创建纯版本仓库

scp -r my_project.git git@ git.csdn.net:~ # 将纯仓库上传到服务器上

mkdir robbin_site.git && cd robbin_site.git && git --bare init # 在服务器创建纯仓库

git remote add origin git@ github.com:robbin/robbin_site.git # 设置远程仓库地址

git push -u origin master # 客户端首次提交

git push -u origin develop # 首次将本地develop分支提交到远程develop分支，并且track

git remote set-head origin master # 设置远程仓库的HEAD指向master分支

也可以命令设置跟踪远程库和本地库

git branch --set-upstream master origin/master

git branch --set-upstream develop origin/develop

9）忽略一些文件、文件夹不提交
在仓库根目录下创建名称为“.gitignore”的文件，写入不需要的文件夹名或文件，每个元素占一行即可，如
> target
> bin
> *.imi
> .project
> .classpath
> .mvn
=====================
-----------------------------------------------------------
mkdir WebApp
cd WebApp
git init
touch README
git add README
git commit -m 'first commit'
git remote add origin git@github.com:daixu/WebApp.git
git push -u origin master
