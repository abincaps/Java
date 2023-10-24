
# VCS

- 版本控制系统
- 集中式 VCS
- 分布式 VCS

# Git

- 分布式
- 开源

# git config

- `--local` 选项对某个仓库中生效，需要进入仓库，默认缺省是 `--local`
- `--global` 选项对当前用户所有的仓库生效
- `--system` 选项对系统所有登录用户生效

# 显示配置

- `git config --list --local`
- `git config --list --global`
- `git config --list --system`

# 配置用户信息

- `git config --global user.name "abincaps"`  
- `git config --global user.email abincaps@qq.com`
- 没有空格可以不用双引号

# 配置文本编辑器

- `git config --global core.editor vim`

# 在已存在目录中初始化仓库

- `git init` 初始化仓库, 创建一个 .git 子目录

# 更新到仓库

- 工作目录 -> 暂存区 -> 版本历史

# 文件的两种状态

- 已跟踪
	- 指被加入了版本控制的文件
	
- 未跟踪

# 跟踪新文件

- `git add` 

# 添加到暂存区

- 已跟踪文件的内容发生变化，但还没有放到暂存区, 使用 `git add` 添加到暂存区

# 检查文件状态

- `git status`

# 查看提交历史

- `git log`
- `git log --oneline`
- `git log -n5` 最近5次 commit
- `git log --all` 查看所有分支的提交历史
- `git log --graph`

# 分析文件差异

- `git diff` 比较工作目录中当前文件和暂存区域快照之间的差异
- `git diff HEAD` 比较工作目录中当前文件和最后一次提交的差异
- `git diff --cached` `git diff --staged` 比较暂存区和最后一次提交之间的差异
- `git diff [commit] [commit]`

# 取消暂存

- `git reset HEAD` 取消之前使用`git add`添加到暂存区的所有更改, 恢复和 HEAD 一致
- `git restore --staged <file>...` 取消已暂存文件的更改

# 取消工作目录更改

- `git checkout -- <file>` 取消工作目录中文件的更改, 恢复到最后一次提交状态

# 移除文件

- `git rm` 从已跟踪中移除, 并从工作目录中删除

# 移动文件

- `git mv file_from file_to` 从暂存区和工作目录中删除 file_from，在暂存区和工作目录中 git add 

# 跳过使用暂存区域

- `git commit -a` 自动把所有已经跟踪过的文件暂存起来一并提交, 跳过 `git add` 步骤

## 添加远程仓库

- `git remote add <shortname> <url>`

# 分支管理

- `*` 字符表示现在检出的分支, 也就是说，当前 HEAD 指针所指向的分支
- `git branch -v` 显示分支的详细信息
- `git branch -a` 显示所有分支

# 新建分支

- `git checkout -b` 

# 删除分支

- `git branch -d`

# 修改最后一次commit

- `git commit --amend` 更改最新的提交消息
- `git add` 添加到暂存区，`git commit --amend` 来添加遗漏的更改

# 变基

- `git rebase`
- `git rebase -i` 交互式

# 修改commit的message

- `reword <commit>` use commit, but edit the commit message

## 多个commit合并成一个

- `squash <commit>` use commit, but meld into previous commit

# Git对象

- commit 对象
- tree 对象 目录结构和 blob 对象索引， 文件名
- blob 对象 文件快照

# Commit

- tree
- parent
- author
- commiter

# tree

- tree 
- blob
- blob

# detached HEAD 分离头指针

- 分离头指针指 HEAD 指针指向某个特定的 commit ，而不指向一个具体的 branch (分支)

# 保存临时更改

- `git stash` 将当前工作目录中的更改保存到一个临时区域
- `git stash list` 查看已保存的存储
- `git stash apply` 将最新的存储恢复到工作目录中，但不会从存储中移除
- `git stash pop` 将最新的存储恢复到工作目录中，并从存储中移除它