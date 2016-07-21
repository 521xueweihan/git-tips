## git-tips
> git小贴士：git常用命令集合（git的'奇技淫巧'？😱）

## 所有人看过来
1. Fork于[tips](https://github.com/git-tips/tips)项目

2. **一定要先测试命令的效果后**，再用于工作环境中，以防造成不能弥补的后果！**到时候别拿着砍刀来找我**

3. 所有的命令都在`git version 2.7.4 (Apple Git-66)`下测试通过

---

* [Everyday Git in twenty commands or so](#everyday-git-in-twenty-commands-or-so)

## 统一概念
1. 工作区：改动（增删文件和文本）
2. 暂存区：输入命令：`git add 改动的文件名`，此次改动就放到了‘暂存区’
3. 本地仓库：输入命令：`git commit 此次修改的描述`，此次改动就放到了’本地仓库’，每个commit，我叫它为一个‘版本’
4. 远程仓库：输入命令：`git push 远程仓库`，此次改动就放到了‘远程仓库’（github等)
5. commit-id：

## 展示帮助信息
```sh
git help -g
```

## 回到远程仓库的状态
抛弃本地仓库的所有版本(commit)，回到远程仓库的状态。  
```sh
git fetch --all && git reset --hard origin/master
```

## 重设第一个commit
也就是把所有的改动都重新放回工作区，并**清空所有的commit**，这样就可以重新提交第一个commit了
```sh
git update-ref -d HEAD
```

## 展示工作区和最近版本的不同
输出**工作区**和本地中最近的版本(commit)的different(不同)。
```sh
git diff
```

## 展示暂存区和最近版本的不同
输出**暂存区**和本地最近的版本(commit)的different(不同)。
```sh
git diff --cached
```

## 展示暂存区、工作区和最近版本的不同
输出**工作区**、**暂存区** 和本地最近的版本(commit)的different(不同)。
```sh
git diff HEAD
```

## 快速切换分支
```sh
git checkout -
```

## 删除已经合并到master的分支
```sh
git branch --merged master | grep -v '^\*\|  master' | xargs -n 1 git branch -d
```

## 展示所有的分支关联的远程仓库
```sh
git branch -vv
```

## 关联远程分支
关联之后，`git branch -vv`就可以展示关联的远程分支名了，同时推送到远程仓库直接：`git push`，不需要指定远程仓库了。
```sh
git branch -u origin/mybranch
```

## 删除本地分支
```sh
git branch -d <local_branchname>
```

## 删除远程分支
```sh
git push origin --delete <remote_branchname>
```

或者  
```sh
git push origin :<remote_branchname>
```

## 删除本地标签(tag)
```sh
git tag -d <tag-name>
```

## 删除远程标签(tag)
```sh
git push origin :refs/tags/<tag-name>
```

## 放弃工作区的修改
```sh
git checkout <file_name>
```

放弃所有修改：  
```sh
git checkout .
```

## 回到某一个commit的状态，并重新增添一个commit
```sh
git revert <commit-id>
```

## 回到某个commit的状态，并删除后面的commit
和revert的区别：reset命令会抹去某个commit id之后的所有commit
```sh
git reset <commit-id>
```

## 修改上一个commit的描述
```sh
git commit --amend
```

## 查看commit历史
```sh
git log
```

## 显示本地执行过git命令
就像shell的history一样

```
git reflog
```

## 修改作者名
```sh
git commit --amend --author='Author Name <email@address.com>'
```

## 修改远程仓库的url
```sh
git remote set-url origin <URL>
```

## 列出所有远程仓库
```sh
git remote
```

## 列出本地和远程分支
-a参数相当于：all
```sh
git branch -a
```

## 列出远程分支
-r参数相当于：remote
```sh
git branch -r
```

## 查看两个星期内的改动
```sh
git whatchanged --since='2 weeks ago'
```

## 把A分支的某一个commit，放到B分支上
这个过程需要`cherry-pick`命令，[参考](http://sg552.iteye.com/blog/1300713#bc2367928)
```sh
git checkout <branch-name> && git cherry-pick <commit-id>
```

## 给git命令起别名
简化命令

```sh
git config --global alias.<handle> <command>

比如：git status 改成 git st，这样可以简化命令

git config --global alias.st status
```

## 存储当前的修改，但不用提交commit
详解可以参考[廖雪峰老师的git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137602359178794d966923e5c4134bc8bf98dfb03aea3000)
```sh
git stash
```

## 保存当前状态，包括untracked的文件
untracked文件：新建的文件
```sh
git stash -u
```

## 展示所有stashes
```sh
git stash list
```

## 回到某个stash的状态
```sh
git stash apply <stash@{n}>
```

## 回到最后一个stash的状态，并删除这个stash
```sh
git stash pop
```

## 删除所有的stash
```sh
git stash clear
```

## 从stash中拿出某个文件的修改
```sh
git checkout <stash@{n}> -- <file_path>
```

## 展示所有tracked的文件
```sh
git ls-files -t
```

## 展示所有untracked的文件
```sh
git ls-files --others
```

## 展示所有忽略的文件
```sh
git ls-files --others -i --exclude-standard
```

## 强制删除untracked的文件
可以用来删除新建的文件。如果不指定文件文件名，则清空所有工作的untracked文件。`clean`命令，**注意两点**：  
1. clean后，删除的文件无法找回
2. 不会影响tracked的文件的改动，只会删除untracked的文件

```sh
git clean <file_name> -f
```

## 强制删除untracked的目录
可以用来删除新建的目录，**注意**:这个命令也可以用来删除untracked的文件。详情见上一条

```sh
git clean <directory_name> -df
```

## 重命名分支
```sh
git branch -m <new-branch-name>
```

## 展示简化的commit历史
```sh
git log --pretty=oneline --graph --decorate --all
```

## 把某一个分支到导出成一个文件
```sh
git bundle create <file> <branch-name>
```

## 从包中导入分支
新建一个分支，分支内容就是上面`git bundle create`命令导出的内容
```sh
git clone repo.bundle <repo-dir> -b <branch-name>
```

## 执行rebase之前自动stash
```sh
git rebase --autostash
```

## 从远程仓库根据ID，拉下某一状态，到本地分支
```sh
git fetch origin pull/<id>/head:<branch-name>
```

## 展示当前分支的最近的tag
```sh
git describe --tags --abbrev=0
```

## 详细展示一行中的修改
```sh
git diff --word-diff
```

## 清除`.gitignore`文件中记录的文件
```sh
git clean -X -f
```

## 展示所有alias和configs.
```sh
git config --list
```

## 展示忽略的文件
```sh
git status --ignored
```

## commit历史中显示Branch1有的，但是Branch2没有commit
```sh
git log Branch1 ^Branch2
```

## 在commit log中显示GPG签名
```sh
git log --show-signature
```

## 删除全局设置
```sh
git config --global --unset <entry-name>
```

## 新建并切换到新分支上，同时这个分支没有任何commit
相当于保存修改，但是重写commit历史  
```sh
git checkout --orphan <branch_name>
```

## 展示任意分支某一文件的内容
```sh
git show <branch_name>:<file_name>
```

## clone下来指定的单一分支
```sh
git clone -b <branch-name> --single-branch https://github.com/user/repo.git
```

## 创建并切换到该分支
```sh
git checkout -b <branch-name>
```

## 关闭Ignore文件的功能
```sh
git config core.fileMode false
```

## 展示本地所有的分支的commit
最新的放在最上面

```sh
git for-each-ref --sort=-committerdate --format='%(refname:short)' refs/heads/
```

## 在commit log中查找相关内容Search Commit log across all branches for given text
通过grep查找，given-text：所需要查找的字段

```sh
git log --all --grep='<given-text>'
```

## 把暂存区的指定file放到工作区中
```sh
git reset <file-name>
```

## 强制推送
```sh
git push -f <remote-name> <branch-name>
```

## 增加远程仓库
```sh
git remote add origin <remote-url>
```


## 联系我
- 博客园：[削微寒](http://www.cnblogs.com/xueweihan/)
- 邮箱：<a href="mailto:595666367@qq.com">发邮件给我</a>
- 或者直接提Pr，Issues
