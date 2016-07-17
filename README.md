## git-tips
> git小贴士：git常用命令集合（git的'奇技淫巧'？😱）

## 所有人看过来
1. Fork于[tips](https://github.com/git-tips/tips)项目

2. **一定要先测试命令的效果后，再用于工作环境中，以防造成不能弥补的后果！**

3. 所有的命令都在`git version 2.7.4 (Apple Git-66)`下测试通过（安全可食用😊）

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

## Show all tracked files
```sh
git ls-files -t
```

## Show all untracked files
```sh
git ls-files --others
```

## Show all ignored files
```sh
git ls-files --others -i --exclude-standard
```

## Create new working tree from a repository (git 2.5)
```sh
git worktree add -b <branch-name> <path> <start-point>
```

## Create new working tree from HEAD state
```sh
git worktree add --detach <path> HEAD
```

## Untrack files without deleting
```sh
git rm --cached <file_path>
```


__Alternatives:__
```sh
git rm --cached -r <directory_path>
```

## Before deleting untracked files/directory, do a dry run to get the list of these files/directories
```sh
git clean -n
```

## 强制删除untracked的文件
清空工作区untracked的文件
```sh
git clean -f
```

## 强制删除untracked的目录
清空工作区untracked的目录
```sh
git clean -df
```

## 重命名分支
```sh
git branch -m <new-branch-name>
```

## rebases 'feature' to 'master' and merges it in to master
```sh
git checkout feature && git rebase @{-1} && git checkout @{-2} && git merge @{-1}
```

## Archive the `master` branch
```sh
git archive master --format=zip --output=master.zip
```

## Modify previous commit without modifying the commit message
```sh
git add --all && git commit --amend --no-edit
```

## Prunes references to remote branches that have been deleted in the remote.
```sh
git fetch -p
```


__Alternatives:__
```sh
git remote prune origin
```

## Retrieve the commit hash of the initial revision.
```sh
 git rev-list --reverse HEAD | head -1
```

## 展示简化的commit历史
```sh
git log --pretty=oneline --graph --decorate --all
```

## Deploying git tracked subfolder to gh-pages
```sh
git subtree push --prefix subfolder_name origin gh-pages
```

## Adding a project to repo using subtree
```sh
git subtree add --prefix=<directory_name>/<project_name> --squash git@github.com:<username>/<project_name>.git master
```

## Get latest changes in your repo for a linked project using subtree
```sh
git subtree pull --prefix=<directory_name>/<project_name> --squash git@github.com:<username>/<project_name>.git master
```

## Export a branch with history to a file.
```sh
git bundle create <file> <branch-name>
```

## Import from a bundle
```sh
git clone repo.bundle <repo-dir> -b <branch-name>
```

## Get the name of current branch.
```sh
git rev-parse --abbrev-ref HEAD
```

## Ignore one file on commit (e.g. Changelog).
```sh
git update-index --assume-unchanged Changelog; git commit -a; git update-index --no-assume-unchanged Changelog
```

## Stash changes before rebasing
```sh
git rebase --autostash
```

## Fetch pull request by ID to a local branch
```sh
git fetch origin pull/<id>/head:<branch-name>
```


__Alternatives:__
```sh
git pull origin pull/<id>/head:<branch-name>
```

## Show the most recent tag on the current branch.
```sh
git describe --tags --abbrev=0
```

## Show inline word diff.
```sh
git diff --word-diff
```

## Don’t consider changes for tracked file.
```sh
git update-index --assume-unchanged <file_name>
```

## Undo assume-unchanged.
```sh
git update-index --no-assume-unchanged <file_name>
```

## Clean the files from `.gitignore`.
```sh
git clean -X -f
```

## Restore deleted file.
```sh
git checkout <deleting_commit>^ -- <file_path>
```

## Restore file to a specific commit-hash
```sh
git checkout <commit-ish> -- <file_path>
```

## Always rebase instead of merge on pull.
```sh
git config --global branch.autosetuprebase always
```

## List all the alias and configs.
```sh
git config --list
```

## Make git case sensitive.
```sh
git config --global core.ignorecase false
```

## Add custom editors.
```sh
git config --global core.editor '$EDITOR'
```

## Auto correct typos.
```sh
git config --global help.autocorrect 1
```

## Check if the change was a part of a release.
```sh
git name-rev --name-only <SHA-1>
```

## Dry run. (any command that supports dry-run flag should do.)
```sh
git clean -fd --dry-run
```

## Marks your commit as a fix of a previous commit.
```sh
git commit --fixup <SHA-1>
```

## squash fixup commits normal commits.
```sh
git rebase -i --autosquash
```

## skip staging area during commit.
```sh
git commit --only <file_path>
```

## 展示忽略的文件
```sh
git status --ignored
```

## commit历史中显示Branch1有的，但是Branch2没有commit
```sh
git log Branch1 ^Branch2
```

## reuse recorded resolution, record and reuse previous conflicts resolutions.
```sh
git config --global rerere.enabled 1
```

## Open all conflicted files in an editor.
```sh
git diff --name-only | uniq | xargs $EDITOR
```

## Count unpacked number of objects and their disk consumption.
```sh
git count-objects --human-readable
```

## Prune all unreachable objects from the object database.
```sh
git gc --prune=now --aggressive
```

## Instantly browse your working repository in gitweb.
```sh
git instaweb [--local] [--httpd=<httpd>] [--port=<port>] [--browser=<browser>]
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

## Extract file from another branch.
```sh
git show <branch_name>:<file_name>
```

## List only the root and merge commits.
```sh
git log --first-parent
```

## Change previous two commits with an interactive rebase.
```sh
git rebase --interactive HEAD~2
```

## List all branch is WIP
```sh
git checkout master && git branch --no-merged
```

## Find guilty with binary search
```sh
git bisect start                    # Search start
git bisect bad                      # Set point to bad commit
git bisect good v2.6.13-rc2         # Set point to good commit|tag
git bisect bad                      # Say current state is bad
git bisect good                     # Say current state is good
git bisect reset                    # Finish search

```

## Bypass pre-commit and commit-msg githooks
```sh
git commit --no-verify
```

## List commits and changes to a specific file (even through renaming)
```sh
git log --follow -p -- <file_path>
```

## Clone a single branch
```sh
git clone -b <branch-name> --single-branch https://github.com/user/repo.git
```

## Create and switch new branch
```sh
git checkout -b <branch-name>
```


__Alternatives:__
```sh
git branch <branch-name> && git checkout <branch-name>
```

## Ignore file mode changes on commits
```sh
git config core.fileMode false
```

## Turn off git colored terminal output
```sh
git config --global color.ui false
```

## specific color settings
```sh
git config --global <specific command e.g branch, diff> <true, false or always>
```

## Show all local branches ordered by recent commits
```sh
git for-each-ref --sort=-committerdate --format='%(refname:short)' refs/heads/
```

## Find lines matching the pattern (regex or string) in tracked files
```sh
git grep --heading --line-number 'foo bar'
```

## Clone a shallow copy of a repository
```sh
git clone https://github.com/user/repo.git --depth 1
```

## Search Commit log across all branches for given text
```sh
git log --all --grep='<given-text>'
```

## Get first commit in a branch (from master)
```sh
git log master..<branch-name> --oneline | tail -1
```

## 把暂存区的内容放到工作区中
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
