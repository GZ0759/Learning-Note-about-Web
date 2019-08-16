
# 创建版本库

初始化一个Git仓库，创建一个目录，使用`git init`命令。

该命令使执行所在目录变成Git可以管理的仓库，同时多出跟踪管理版本号的 `.git` 目录。

添加文件到Git仓库，分两步：

1. 使用命令`git add <file>`，注意，可反复多次使用，添加多个文件；
2. 使用命令`git commit -m <message>`，完成。

简单解释一下`git commit`命令，`-m`后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

# 时光机穿梭

要随时掌握工作区的状态，使用`git status`命令。

如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。

## 版本回退

`HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。

上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个^比较容易数不过来，所以写成`HEAD~100`。

穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。

要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

## 工作区和暂存区

工作区（Working Directory）。就是你在电脑里能看到的目录。

版本库（Repository）。工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

所以，`git add`命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行`git commit`就可以一次性把暂存区的所有修改提交到分支。

`git status`查看工作区是否干净。

## 管理修改

用`git diff HEAD -- readme.txt`命令可以查看工作区和版本库里面最新版本的区别。

第一次修改 -> `git add` -> 第二次修改 -> `git commit`

第二次修改的内容没有放入暂存区。

## 撤销修改

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时（已经commit上去了），想要撤销本次提交，使用命令`git reset --hard commit_id`。

## 删除文件

一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用`rm`命令删了。这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，`git status`命令会立刻告诉你哪些文件被删除了。

现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`。

> 小提示：先手动删除文件，然后使用`git rm <file>`和`git add<file>`效果是一样的。

另一种情况是删错了，因为版本库里还有呢，所以可以使用`git checkout -- test.txt`，很轻松地把误删的文件恢复到最新版本。

> 注意：从来没有被添加到版本库就被删除的文件，是无法恢复的！

# 远程仓库

## 通过SSH绑定电脑

第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果没有就需要创建。

```sh
$ ssh-keygen -t rsa -C "youremail@example.com"
```

第2步：登陆 GitHub，点“Add SSH Key”，在Key文本框里粘贴`id_rsa.pub`文件的内容。

为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。

GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

## 添加远程库

要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`；

关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

## 从远程仓库克隆

要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。

```sh
$ git clone git@github.com:michaelliao/gitskills.git
```

Git支持多种协议，包括`https`，但通过`ssh`支持的原生`git`协议速度最快。

从现在起，只要本地作了提交，就可以通过命令：

```sh
$ git push origin master
```

把本地`master`分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！

# 分支管理

## 创建与合并分支

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`

创建+切换分支：`git checkout -b <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`

## 解决冲突

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

`git status`也可以告诉我们冲突的文件。

Git用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容。

用`git log --graph`命令可以看到分支合并图。

## 分支管理策略

通常，合并分支时，如果可能，Git 会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

合并分支时，加上`--no-ff`参数就可以用普通模式合并，因为合并要创建一个新的commit，所以加上`-m`参数，把 commit 描述写进去。因此，合并后的历史有分支，能看出来曾经做过合并。

在实际开发中，我们应该按照几个基本原则进行分支管理：

- 首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

- 干活都在dev分支上。如果版本发布，再把dev分支合并到master上，在master分支发布版本；

- 每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。

## Bug分支

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

Git还提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作。用git status查看工作区，就是干净的（除非有没有被Git管理的文件）。

用git stash list命令看看工作现场。Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

- 用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；

- 用`git stash pop`，恢复的同时把stash内容也删了。

在master分支上修复的bug，想要合并到当前dev分支，但不是所有所有提交，可以用`git cherry-pick <commit>`命令，把bug提交的这次修改“复制”到当前分支，避免重复劳动。

## Feature分支

开发一个新feature，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。使用`-d`参数则会被提醒分支尚未合并。

## 多人协作

当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，远程仓库的默认名称是origin。

查看远程库比较详细的信息，使用`git remote -v`；

本地新建的分支如果不推送到远程，对其他人就是不可见的；

从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；

在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用`git branch (--set-upstream-to=<upstream> | -u <upstream>) [<branchname>]`；

从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

## Rebase

rebase 操作可以把本地未push的分叉提交历史整理成直线；

rebase 的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

# 标签管理

## 创建标签

命令`git tag <tagname>`用于新建一个标签，默认为`HEAD`，也可以指定一个commit id；

命令`git tag -a <tagname> -m "blablabla..."`可以指定标签信息；

命令`git tag`可以查看所有标签。

> 注意：标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签。

## 操作标签

命令`git push origin <tagname>`可以推送一个本地标签；

命令`git push origin --tags`可以推送全部未推送过的本地标签；

命令`git tag -d <tagname>`可以删除一个本地标签；

命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。

## 使用 GitHub

在GitHub上，可以任意Fork开源仓库；

自己拥有Fork后的仓库的读写权限；

可以推送pull request给官方仓库来贡献代码。

# 自定义Git

注意git config命令的--global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

让Git显示颜色，会让命令输出看起来更醒目：

```
$ git config --global color.ui true
```

## 忽略特殊文件

在Git工作区的根目录下创建一个特殊的`.gitignore`文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。

不需要从头写`.gitignore文`件，GitHub已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：https://github.com/github/gitignore

忽略文件的原则是：

忽略操作系统自动生成的文件，比如缩略图等；
忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；
忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

## 配置别名

```
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

## 搭建Git服务器

第一步，安装git：

```
$ sudo apt-get install git
```

第二步，创建一个git用户，用来运行git服务：

```
$ sudo adduser git
```

第三步，创建证书登录：

收集所有需要登录的用户的公钥，就是他们自己的id_rsa.pub文件，把所有公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。

第四步，初始化Git仓库：

先选定一个目录作为Git仓库，假定是/srv/sample.git，在/srv目录下输入命令：

```
$ sudo git init --bare sample.git
```

Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以.git结尾。然后，把owner改为git：

```
$ sudo chown -R git:git sample.git
```

第五步，禁用shell登录：

出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑/etc/passwd文件完成。找到类似下面的一行：

```
git:x:1001:1001:,,,:/home/git:/bin/bash
```

改为：

```
git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
```

这样，git用户可以正常通过ssh使用git，但无法登录shell，因为我们为git用户指定的git-shell每次一登录就自动退出。

第六步，克隆远程仓库：

现在，可以通过git clone命令克隆远程仓库了，在各自的电脑上运行：

```
$ git clone git@server:/srv/sample.git
Cloning into 'sample'...
warning: You appear to have cloned an empty repository.
```
