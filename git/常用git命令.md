> 绪论:因为经常用到git命令,常用命令时常记不住,所以在此处对<code>常用</code>git命令做个记录

## 新建代码库

#### 在当前目录新建一个git仓库

```vb
$ git init
```

#### 新建一个目录并初始化为git仓库

```vb
$ git init [project-name]
```

#### Clone project

```vb
$ git clone [url]
```

## 配置

#### 显示当前的git配置

```vb
$ git config --list
```

#### 配置git提交的用户信息

```vb
$ git config [--global] user.name "name"
$ git config [--global] user.email "email"
```

## 增加/删除文件

#### 添加指定文件到暂存区

```vb
$ git add [file1] [file2] ...
```

#### 添加指定目录到暂存区

```vb
$ git add [dir]
```

#### 添加当前目录的所有文件到暂存区

```vb
$ git add .
```

#### 添加每个变化前都会要求确认/对于同一文件的多处变化可以实现分次提交

```vb
$ git add -p
```

#### 删除工作分区文件,并将这次删除放入暂存区

```vb
$ git rm [file1] [file2] ...
```

#### 停止追踪指定文件,但该文件会保留在工作区

```vb
$ git rm --cached [file]
```

#### 改名文件,并将这个改名放入暂存区

```vb
$ git mv [file-origin] [file-renamed]
```

## 代码提交

#### 提交暂存区到仓库区

```vb
$ git commit -m [message]
```

#### 提交暂存区的指定文件到仓库区

```vb
$ git commit [file1] [file2] ... -m [message]
```

#### 提交工作区自上次commit后的变化,直接到仓库区

```vb
$ git commit -a
```

#### 提交时显示所有diff信息

```vb
$ git commit -v
```

#### 使用一次新的commit,替代上一次提交

```vb
$ git commit --amend -m [message]
```

#### 重做上一次commit,并包括指定文件的新变化

```vb
$ git commmit -amend [file1] [file2] ...
```

## 分支

#### 列出所有本地分支

```vb
$ git branch
```

#### 列出所有远程分支

```vb
$ git branch -r
```

#### 列出所有本地分支和远程分支

```vb
$ git branch -a
```

#### 新建一个分支,但仍然停留在当前分支

```vb
$ git branch [branch-name]
```

#### 新建一个分支,并切换到该分支

```vb
$ git checkout -b [branch-name]
```

#### 新建一个分支,指向指定commit

```vb
$ git branch [branch] [commit]
```

#### 新建一个分支,与指定的远程分支建立追踪关系

```vb
$ git branch --track [branch] [remote-branch]
```

#### 切换到指定分支,并更新工作区

```vb
$ git checkout [branch-name]
```

#### 切换到上一个分支

```vb
$ git checkout -
```

#### 建立追踪关系,在现有分支与指定的远程分支之间

```vb
$ git branch --set-upstream [branch] [remote-branch]
```

#### 合并指定分支到当前分支

```vb
$ git merge [branch]
```

#### 选择一个commit,合并进当前分支

```vb
$ git cherry-pick [commit]
```

#### 删除分支

```vb
$ git branch -d [branch-name]
```

#### 删除远程分支

```vb
$ git push prigin --delete [branch-name]
$ git branch -dr [remote/branch]
```

## 标签

#### 列出所有tag 

```vb
$ git tag
```

#### 新建一个tag在当前commit

```vb
$ git tage [tag]
```

#### 新建一个tag在指定commit

```vb
$ git tag [tag] [commit]
```

#### 删除本地tag

```vb
$ git tag -d [tag]
```

#### 删除远程tag

```vb
$ git push origin :refs/tags/[tagName]
```

#### 查看tag信息

```vb
$ git show [tag]
```

#### 提交指定tag

```vb
$ git push [remote] [tag]
```

#### 提交所有tag

```vb
$ git push [remote] --tags
```

#### 新建一个分支,指向某个tag

```vb
$ git checkout -b [branch] [tag]
```

## 查看信息

#### 查看有变更的文件

```vb
$ git status
```

#### 显示当前分支的版本历史

```vb
$ git log
```

#### 显示commit历史,以及每一次commit发生变更的文件

```vb
$ git log --stat
```

#### 搜索提交历史,根据关键字

```vb
$ git log -S [keyword]
```

#### 显示某个commit之后的所有变动,每个commit占据一行

```vb
$ git log [tag] HEAD --pretty=format:%s
```

#### 显示某个commiy之后的所有变动,其提交说明必须符合搜索条件

```vb
$ git log [tag] HEAD --grep feature
```

#### 显示某个文件的版本历史,包括文件改名

```vb
$ git log --follow [file]
$ git whatchanged [file]
```

#### 显示指定文件的每一次diff

```vb
$ git log -p [file]
```

#### 显示过去5次提交

```vb
$ git log -5 --pretty --online
```

#### 显示所有提交过的用户,按提交次数排序

```vb
$ git shortlog -sn
```

#### 显示指定文件是什么人在什么时间修改过

``` vb
$ git blame [file]
```

#### 显示暂存区和工作区的差异

```vb
$ git diff
```

#### 显示暂存区和上一次commit的差异

```vb
$ git diff --cached [file]
```

#### 显示工作区与当前分支最新commit之间的差异

```vb
$ git diff [first-branch] ... [second-branch]
```

#### 显示今天你写了多少行代码

```vb
$ git diff --shortstat "@{0 day ago}"
```

#### 显示某次提交的元数据和内容变化

```vb
$ git show [commit]
```

#### 显示某次提交发生变化的文件

```vb
$ git show --name-only [commit]
```

#### 显示某次提交时,某个文件的内容

```vb
$ git show [commit]:[filename]
```

#### 显示当前分支的最近几次提交

```vb
$ git reflog
```

## 远程同步

#### 下载远程仓库的所有变动

```
$ git fetch [remote]
```

#### 显示所有远程仓库

```vb
$ git remote -v
```

#### 显示某个远程仓库的信息

```vb
$ git remote show [remote]
```

#### 增加一个新的远程仓库,并命名

```vb
$ git remote add [shortname] [url]
```

#### 取回远程仓库的变化,并与本地分支合并

```vb
$ git pull [remote] [branch]
```

#### 上传本地指定分支到远程仓库

```vb
$ git push [remote] [branch]
```

#### 强行推送当前分支到远程仓库,即使有冲突

```vb
$ git push [remote] --force
```

#### 推送所有分支到远程仓库

```vb
$ git push [remote] --all
```

## 撤销

#### 恢复暂存区的指定文件到工作区

```vb
$ git checkout [file]
```

#### 恢复某个commit的指定文件到暂存区和工作区

```vb
$ git checkout [commit] [file]
```

#### 恢复暂存区的所有文件到工作区

```vb
$ git checkout .
```

#### 重置暂存区的指定文件,与上次commit保持一致,但工作区不变

```vb
$ git reset [file]
```

#### 重置暂存区与工作区,与上一次commit保持一致

```vb
$ git reset --hard
```

#### 重置当前分支的指针为指定commit,同时重置暂存区,但工作区不变

```vb
$ git reset [commit]
```

#### 重置当前分支的HEAD为指定commit,同时重置暂存区和工作区,与指定commit一致

```vb
$ git reset --hard [commit]
```

#### 重置当前HEAD为指定commit,但保持暂存区和工作区保持不变

```vb
$ git reset --keep [commit]
```

#### 新建一个commit,用来撤销指定commit,后者的所有变化都将被前者抵消,并应用到当前分支

```vb
$ git revert [commit]
```

#### 暂时将未提交的变化移出,稍后再移入

```vb
$ git stash
$ git stash pop
```

## 其他

#### 生成可供发布的压缩包

```vb
$ git archive
```





