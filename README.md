# Git && Github

## 目录

1. 简介
2. 安装
3. 本地仓库
3. 介绍版本
4. 分支功能
5. 解决冲突
6. 推送到远程仓库

## 简介

- Git 与 SVN 的理念差异

分布式版本控制器 ==> 中央版本控制器

整个项目         ==> 项目快照

多分副本         ==> 一个副本


- Git 与 SVN的概念差异

repository 仓库  ==> snv主文件夹

status 状态      ==> √ ! ?

commit 版本      ==> 和svn一样

branch 分支      ==> 类似公司的dev和release分支


## 下载git

- 官方版本下载:

http://git-scm.com/download/win

http://msysgit.github.com/

- GitHub for Windows:

https://desktop.github.com/

以上两种Git工具，建议都下载来试试， ~~个人使用 Git Shell 比较多，~~ 只用 GitHub 图形界面省事!

## 使用图形界面



## 了解命令行

一般在使用Git命令时，我们还会经常使用到 Mac终端（Terminal）/ Windows命令窗口 (cmd) 命令。掌握以下命令会对终端/窗口操作很用帮助。

| Windows | Mac OS/Linux) | 描述               | 例子                                                     |
| ------- | ------------- | ------------------ | -------------------------------------------------------  |
| mkdir   | mkdir         | 新建目录           | mkdir testdirectory                                      |
| del     | rm            | 删除文档/目录      | del c:\test\test.txt                                     |
| copy    | cp            | 复制文档           | copy <origin>c:\test\test.txt <copy>c:\windows\test.txt  |
| move    | mv            | 移动文档           | move <from>c:\test\test.txt <from>c:\windows\test.txt    |
| cd      | cd            | 修改路径           | cd ./childdir                                            |
| dir     | ls            | 列出所有文档和路径 | dir                                                      |
| exit    | exit          | 离开视窗           | exit                                                     |


## 创建一个本地 Git 仓库 *repository*

完成前面的准备工作后，现在正式开始学习Git的操作。

首先启动 Git Shell ， `cd` 到你想要保存代码的地址，然后运行下面的执行，从远程Git仓库中克隆新建的项目到本地。

    > git clone https://github.com/jiangleo/git.git


 `git clone [url]` clone操作并不只提取最新版本的文件快照，而是将远程 Git 仓库中的每一个文件的每一个版本都拉取到本地。 任何一个本地仓库或者远程仓库的地位都是一样的，它们都是一个全整的包含其历史版本的代码库。任何一处协同工作用的服务器发生故障，事后都可以用任何一个镜像出来的本地仓库恢复。

## 修改更新代码

1. 在本地新建一个 hello.js文件

    alert('hello world!');

此时 hello.js 文件，只是一个普通文件，并不是 Git 文件。我们可以通过 `git status` 查看 Git 仓库的状态。

我们可以查看到 Git 会给我们一个提示

    Untracked files:
      (use "git add <file>..." to include in what will be committed)

            hello.js

Git 提示我们，在本地仓库中，有个一 hello.js 的文件， 他并不是一个 Git 的文件(**Untracked files**) 。


2. 将文件放入暂存区

执行`git add <file>`将文件放到暂存区，

    > git add hello.js

然后执行 `git status` 再来查看仓库状态

    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)

            new file:   hello.js

执行 `git add` 指令后， hello.js 文件已经是一个 git 文件了， 但是这个时候， hello.js 文件还没有被提交。它处于一个待提交的状态 (Changes to be committed)，实际上它放在 git 的暂存区 (**Staged**) 。

ps: 

这个暂存区的概念，比较难理解，这也是 svn 中没有的概念。想象一个场景，使用 svn ，我们修改了 a.js 然后等待 RD 给出接口， 我们并不会把 没有完成的 a.js 代码上传到代码仓库中。这个时候在修改其他文件的过程中，可能会不小心把 a.js 给覆盖了。

如果使用 Git ，我们会把 a.js 保存在 暂存区 **Staged** 中，即使被覆盖，还有会一次挽救的机会。

3. 提交修改

执行 `git commit -m "commit message"` 命令，提交代码并进行备注。

    > git commit -m "test file"

这个时候，我们就真正的将 hello.js 提交到本地仓库中去了。我们可以通过回滚，在历史记录中轻松的找到其历史版本。再次强调下，在 Git 中，本地仓库包含每一个文件的每一个版本，是一个完整且独立的代码仓库，它不需要依赖远程仓库来进行提交和回滚。


这个时候，让我们再次执行 `git status` 命令。

    Your branch is ahead of 'origin/master' by 1 commit.
      (use "git push" to publish your local commits)

    nothing to commit, working directory clean

没有什么可以提交，工作区是干净的。 这意味着该目录下的所有文件都是 Git 仓库的文件，并且没有改动 (**Unmodified**)。


## 理解文件状态

通过 `git add` `git commit -m <message>`2个命令，可以很轻松的将一个修改的文件，提交到本地的 Git 仓库。

前文了解到了文件3种状态 Untracked Modified Staged，现在再次修改 hello.js 文件，来了解文件的另一种状态


    alert('Hello guys!')

这个时候，查看仓库状态

    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   hello.js

    no changes added to commit (use "git add" and/or "git commit -a")

这个时候 Git 提醒我们，文件处于修改状态 (**Modified**)，再执行 `git add`，文件状态变为 **Staged** ，最后执行 `git commit` 命令，文件状态变为 **Unmodified**。

总结一下 Git 文件的4种状态，并对比 SVN 文件状态便于理解

|            | 状态描述                             | 对比 SVN                        |
| ---------- | ------------------------------------ | ------------------------------- |
| Untracked  | 不属于 Git 仓库但是在 Git 目录的文件 | ![SVN](./img/readme/svn_03.png) |
| Unmodified | 文件和本地 Git 仓库文件一样          | ![SVN](./img/readme/svn_01.png) |
| Modified   | 文件有改动                           | ![SVN](./img/readme/svn_02.png) |
| Staged     | 文件放在暂存区                       | N/A                             |


## 推送更新到远程仓库

在 SVN 中有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端连到这台服务器，取出最新的文件或者提交更新。

如前所述，每个 Git 都是一个完整的代码仓库，那么只需要指定一个远程仓库作为主仓库，所有人与这个主仓库进行同步，即可保证所以人的提交更新的代码一致。

最开始，我们创建了一个远程 Github 的仓库，并以此克隆了一个本地仓库，现在将 Github 仓库作为主仓库，并将本地仓库的更新推送到远程仓库。

首先查看下与本地仓库关联的远程仓库 `git remote`

    origin

origin 是远程仓库名字的默认名字，这样可以用其名字来代替远程仓库的地址 https://github.com/jiangleo/git.git

执行 `git push <remote> <branch>` 命令，并用名字 origin 来代替真实的地址。branch 是分支的名，主分支默认名为 master

    git push origin master



## 查看demo




## 理解Git工作流 

http://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-Git-%E5%9F%BA%E7%A1%80

## Git的撤销操作

## 常用操作总结

## 理解分支

## 
