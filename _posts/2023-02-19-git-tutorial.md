---
title: git tutorial
date: 2023-02-19
---



# Git Tutorial

git的官方网址是`git-scm.com`，通过他的uri得知git的定位为scm(Software Configuration Management)。

## Configure

### Global Config

Git is a collaborative tool that asks us to provide the name and contact way(email).

```sh
git config --global user.name "username"
git config --global user.email "email"
```

The proxy setting：

```sh
git config --global http.proxy 'http://127.0.0.1:7890'
git config --global https.proxy 'http://127.0.0.1:7890'
```

The global config is stored in `~/.gitconfig`.

### Project Config

Like the global config, remove the `--global` parameter from the content.

### Remote Repository

```shell
git remote
```

你可以添加任意远程仓库，但之后若想提交等操作，需要你对该仓库有权限。

远程仓库一般由源代码托管服务平台提供，类似GitHub、Gitee。每次提交到远程仓库需要验证你的身份，以判断是否有权限。这个验证身份可以是账号密码、公私钥，分别对应着https方式验证、ssh方式验证。

最方便的验证方式是公私钥，这需要你将公钥添加到源代码托管服务平台账号，这里举例GitHub。

#### Generate SSH-Key

如果已有密钥，则不需要生成密钥。

如果没有密钥则使用下面名声生成密钥：

```sh
ssh-keygen -t rsa
```

将生成公私一对密钥，私钥不要泄漏。

#### 添加公钥到GitHub账号

Github->Setting->SSH and GPG keys->new SSH key

将公钥复制粘贴到Key.

#### 验证

````sh
ssh -T git@github.com
````

注意`id_rsa`和`id_rsa.pub`的权限应为`700`。

## 操作

### 帮助

git命令的内容很多，很难覆盖全面，这时候应该多使用帮助。

```shell
# help
git [command] -h
# man
man git
# man specified command, such as `man git-remote`
man git-<command>
```

### 创建本地仓库

```sh
# 创建文件夹
mkdir foo
# 进入文件夹
cd foo
# 初始化本地仓库
git init
```

`git init`会在当前目录创建`.git`文件夹，里面存储着与版本控制的相关信息。

### 忽略文件

#### .gitignore

Git允许你自定义`.gitignore `规则去会略文件的提交。

特别提醒，`.gitignore`只会忽略从未被跟踪的文件，假如这个文件开始被提交过，则无法忽略。

文档链接：https://git-scm.com/docs/gitignore

以下列出常见的：

```.gitignore
# hash符号表示注释
# An asterisk "*" matches anything except a slash. 
# A leading "**" followed by a slash means match in all directories.

# 忽略该文件
.DS_Store

# 目录后可加`/`方便辨认，不加也可，忽略目录及其内容
dist/
node_modules/

# 感叹号，不忽略的内容
# An optional prefix "!" which negates the pattern
!xxx
```

#### update-index --assume-unchanged

Usage:

```sh
git update-index --assume-unchanged <file_name>
```

Cancle the above command: 

```sh
git update-index --no-assume-unchaged <filename>
```

compared with the way with `.gitignore` file, the git project handled with the command will not be shared with another teammate.

List all file that is thought assume unchanged:

```sh
git ls-files -v | grep '^h'
```

### 状态

```shell
git status
```

### 清理

```shell
# 清理未加入到索引的文件
# 常见使用 `git clean -fdx`
# -f file
# -d directory
# -x Don’t use the standard ignore rules
# -n --dry-run Don’t actually remove anything, just show what would be done.
git clean
```

### 更新索引

为了便于理解和统一叫法，本地区域和提交区域分别叫工作区和暂存区。

```shell
# 添加工作区修改添加到暂存区
git add <file>
# 常用命令有
git add .

# 当文件没修改，删除工作区的文件将操作上传到暂存区
git rm <file>
# 上述操作等于
rm <file>
git add .

# 只删除暂存区的内容
git rm --cache <file>
```

### 提交

把暂存区的内容提交到本地仓库

```shell
git commit -m "<something you want to say>
```

关于commit你需要知道几个特殊值

HEAD为最新提交记录

HEAD^第二新

HEAD~n 第n+1新

- Use `~` most of the time — to go back a number of generations, usually what you want
- Use `^` on merge commits — because they have two or more (immediate) parents

### 查看记录

```shell
# 查看当前分支相关提交记录
git log
# 查看所有分支
git log --all
# 查看所有提交
git log --reflog
# ASCII图查看分支关系
git log --graph
# 如果你想查看具体修改的文件
git log --raw <commit_id>
# 仅查看文件
git log --name-only 
# -p can check change of the file.
# -1 can specify the commit.
git log -p -1 <commit> <file>
```

### 查看具体提交信息

相信你提交完后肯定想知道修改了哪些

```sh
# 没加commit默认都是HEAD
git show [commit]
# 查看某次提交的某个文件全部内容
git show [commit]:[file_path]
```

### 查看文件的修改

```shell
git blame <file_name>
```

### 对比文件

```sh
# 查看冲突文件
# --diff-filter U 表示 unmerged
git diff --name-only --diff-filter=U
```

### 变基

你可以对历史的提交记录进行修改，让当前项目基于一个全新的历史提交记录。

```
git rebase -i <commit-hash>
# 执行rebase修改
git rebase --continue
# 放弃rebase修改
git rebase --abort).
```

### 分支

git支持多分支

假如你没创建分支，当你git commit执行后，会默认创建master分支。

```shell
# 列出分支
git branch
# 创建分支
git branch <branch-name>
# 更改分支名称
git branch -m <new-branch-name>
# 切换分支
git checkout <branch-name>
# 重新回归当前分支：可以用于放弃当前分支所有修改
git checkout .
# 重置某个文件
git checkout -- <filename>
# 强行切换分支
git checkout -f <branch-name>
# 创建并切换分支
git checkout -b <branch-name>
# 删除分支
git branch -d <branch-name>
# 当你多个远程仓库时，你可以设置默认的仓库
git branch --set-upstream-to=/<remote>/<remote_branch> <branch>
# 指定提交记录创建分支
git checkout commitId -b 本地新branchName 
# 合并分支
# 相同文件在不同分支进行修改并提交，合并时会有冲突，需要手动合并
# 将选中的分支合并到当前分支
git merge <branch merge>
```

### 重置提交/文件

```shell
# 重置到上次提交的状态，修改的文件不会被还原，但会清空索引
git reset <commit-hash> [filename]
# 重置到上次提交的状态，修改的文件不会被还原，但不会清空索引
git reset --soft <commit-hash> [filename]
# 重置到上次提交的状态，修改的文件会被还原，也会清空索引，因此会留下上次版本为创建的文件。
git reset --hard <commit-hash> [filename]
```

### 提交/拉取

假如协同合作，每次 push 之前为了防止冲突需要先 pull 来手机解决冲突。

```shell
# pull
git pull
# push
git push
# 设置当前本地分支的默认远程分支
git push --set-upstream <remote> <remote-branch>
# 远处远程分支
git push origin --delete dev
```

### TAG

可以为指定提交记录打上标签

```sh
# 展示tag
git tag
# -a annotate
# -m message
git tag -a v1.4 -m "my version 1.4"
# 正常推送远程仓库并不会包含tag，需要加上 --tags
git push --tags

# Delete locally
git tag -d <tag_name>
# Delete remotely
git push origin --delete <tag_name>
```

## Q&A

当使用Git提交过程中，把不需要提交的大文件误提交之后，由于`.git`文件夹记录了这个文件导致`.git`文件目录变大，如果删除记录并把`.git`的记录也删除？

TODO

---

当你使用 `git reset --hard` 命令后，虽然跟踪的版本会被回退，但是未跟踪的文件/文件夹不会被回退，如何删除这些？

```sh
# -f file
# -d directory
# -x Don’t use the standard ignore rules
# -n --dry-run Don’t actually remove anything, just show what would be done.
git clean -fdx
```

---

如何删除远程分支

```shell
git push <remote-repo> --delete <branch-name>
```

---

所有文件和文件夹：

```shell
git clean -xdff
```

[谨慎操作] 本命令删除新增的文件和文件夹，如果文件已经已经 git add 到暂存区，并不会删除！

---

如果提交了一个 issue，当提交到 master 分支的时候，提交信息里面可以使用 fix/fixes
/fixed ,close/closes/closed 或者是 resolve/resolves/resolved 等关键词，后面再跟上 issue 号，这样就会关闭这个 issue

举例：

```
$ git commit -m "Fix screwup, fixes #12"
1
```

这将会关闭Issue #12，并且在Issue讨论列表里关联引用这次提交。

---

### commit

```sh
# 显示当前分支相关的提交日志，包括影响当前分支的其他分支提交
git log
# 显示所有分支的提交日志
git log --all
# 显示所有的提交日志，即使该提交所在的分支已经不在
git log --reflog
```

当你创建多个commit，commit会按照创建时间的顺序编排（无论在任意分支），通过`git log`可以发现这个规律。

如果要改变commit地顺序需要使用`git rebase`命令。

删除branch并不会删除commit，tag和commit相关和branch无关。

所以可以认为branch服务于commit，tag服务于commit，commit就是中心。

tag 是按照阿拉伯数字排序的


---

Filename too long in Git for Windows

admin terminal execute it

```
git config --system core.longpaths true
```

ref: https://stackoverflow.com/questions/22575662/filename-too-long-in-git-for-windows

---

