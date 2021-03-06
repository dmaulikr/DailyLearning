## Git Learning

记录小白学习`Git`的过程，如有错误，希望拍砖指正~

# Git
> Git 作为现在最流行的开源的版本控制系统,  有很多好用开源的工具，`SourceTree`、`Tower`[](https://www.git-tower.com/mac/?source=rd)、`[GitUp](https://github.com/git-up/GitUp)`当然还有`Github`的官方客户端,还有大量的开发者，可以说`Git`是目前用户最多，最火的版本控制系统。

具体什么是`Git`,可以参考[what-is-git](https://www.atlassian.com/git/tutorials/what-is-git)文章,可以说`Git`集成了`SVN`的一些特性（tag, branch），采用了巧妙的设计(本地库)，让并行开发更高效。

## Git workflow

>The Git workflow consists of editing files in the working directory, adding files to the staging area, and saving changes to a Git repository. In Git, we save changes with a commit, which we will learn more about in this lesson.
![](http://oc98nass3.bkt.clouddn.com/14936106948535.jpg)

一个`Git`项目可以被看做一下三个部分：

1. A Working Directory: where you'll be doing all the work: creating, editing, deleting and organizing files.
工作区:  你将在里面完成文件的创建、编辑、删除、整理的所有工作。
2. A Staging Area: where you'll list changes you make to the working directory.
暂存区：
3. A Repository: where Git permanently stores those changes as different versions of the project.
版本库:  `Git`永久的保存着这个项目版本间的不同变化。

![](http://oc98nass3.bkt.clouddn.com/14936109243775.jpg)

### 工作区、暂存区、版本库原理图

![](http://oc98nass3.bkt.clouddn.com/14936261372760.png)

在这个图中，可以看到部分`Git`命令是如何影响工作区和暂存区（`stage`，亦称`index`）的。下面就对这些命令进行简要的说明，而要彻底揭开这些命令的面纱要在接下来的几个章节。
图中左侧为工作区，右侧为版本库。在版本库中标记为`index`的区域是暂存区（`stage`，亦称`index`），标记为`master`的是`master`分支所代表的目录树。
图中可以看出此时HEAD实际是指向`master`分支的一个“游标”。所以图示的命令中出现HEAD的地方可以用master来替换。
图中的`objects`标识的区域为`Git`的对象库，实际位于`.git/objects`目录下，会在后面的章节重点介绍。
当对工作区修改（或新增）的文件执行`git add`命令时，暂存区的目录树被更新，同时工作区修改（或新增）的文件内容被写入到对象库中的一个新的对象中，而该对象的ID被记录在暂存区的文件索引中。
当执行提交操作（`git commit`）时，暂存区的目录树写到版本库（对象库）中，`master`分支会做相应的更新。即`master`最新指向的目录树就是提交时原暂存区的目录树。
当执行`git reset HEAD`命令时，暂存区的目录树会被重写，被vmaster`分支指向的目录树所替换，但是工作区不受影响。
当执行`git rm –cached <file>`命令时，会直接从暂存区删除文件，工作区则不做出改变。
当执行`git checkout .`或者`git checkout – <file>`命令时，会用暂存区全部或指定的文件替换工作区的文件。这个操作很危险，会清除工作区中未添加到暂存区的改动。
当执行`git checkout HEAD .`或者`git checkout HEAD <file>`命令时，会用`HEAD`指向的`master`分支中的全部或者部分文件替换暂存区和以及工作区中的文件。这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改动。

##### Git diff魔法
通过使用不同的参数调用git diff命令，可以对工作区、暂存区、HEAD中的内容两两比较。下面的这个图，展示了不同的git diff命令的作用范围。

![](http://oc98nass3.bkt.clouddn.com/14936265400329.png)

##### Git对象库探秘
原来分支master指向的是一个提交ID（最新提交）。这样的分支实现是多么的巧妙啊：既然可以从任何提交开始建立一条历史跟踪链，那么用一个文件指向这个链条的最新提交，那么这个文件就可以用于追踪整个提交历史了。这个文件就是.git/refs/heads/master文件。
下面看一个更接近于真实的版本库结构图：

![](http://oc98nass3.bkt.clouddn.com/14936266336082.png)


### `Git`命令

用一个命令向改变区(staging)添加多个文件的方法
`git add filename_1 filename_2`

`git add files` 把当前文件放入暂存区域。
`git commit` 给暂存区域生成快照并提交。
`git reset -- files` 用来撤销最后一次git add files，你也可以用git reset` 撤销所有暂存区域文件。
`git checkout -- files` 把文件从暂存区域复制到工作目录，用来丢弃本地修改。

![](http://oc98nass3.bkt.clouddn.com/14936233428927.jpg)

![](http://oc98nass3.bkt.clouddn.com/14936234442203.jpg)

`git commit -a` 相当于运行 git add 把所有当前目录下的文件加入暂存区域再运行。git commit.
`git commit files` 进行一次包含最后一次提交加上工作目录中文件快照的提交。并且文件被添加到暂存区域。
`git checkout HEAD -- files` 回滚到复制最后一次提交。

[图解Git](https://marklodato.github.io/visual-git-guide/index-zh-cn.html#commands-in-detail)

`git checkout HEAD filename`: Discards changes in the working directory.
`git reset HEAD filename`: Unstages file changes in the staging area.
`git reset SHA`: Can be used to reset to a previous commit in your commit history.

#### `remote origin`

1. 添加远程库： `$ git remote add origin`
2. 移除远程库： `$ git remote remove (OrignName)`
3. 查看远程库： `$ git remote -v`(--verbose)

#### `git grep`命令查找
`$ git grep`命令
`git grep -n "要查找的字符串"`
1. -W 查找函数上下文
2. 使用 --count 参数, 只会显示在哪个文件里有几个要查找的字符串, 如下:
 `git grep --count "(defun format "`
 
 `src/format.lisp:1`

可以使用 `$ git help grep` 来查看帮助

#### 设置`git`命令 别名
`$ git config --global alias.st status`
`$ git config --global alias.co checkout`
`$ git config --global alias.ct commit`
`$ git config --global alias.df diff`
`$ git config --global alias.br branch`

#### `Git`删除文件

[git 删除文件](http://www.jianshu.com/p/c3ff8f0da85e)

#### 深入了解`git rese`t命令
![](http://oc98nass3.bkt.clouddn.com/14936268028934.png)

重置命令（`git reset`）是Git最常用的命令之一，也是最危险，最容易误用的命令。来看看git reset命令的用法。
用法一：`git reset [-q] [<commit>] [--] <paths>...`
用法二： `git reset [--soft | --mixed | --hard | --merge | --keep] [-q] [<commit>]`
上面列出了两个用法，其中` <commit> `都是可选项，可以使用引用或者提交`ID`，如果省略 `<commit> `则相当于使用了`HEAD`的指向作为提交`ID`。
上面列出的两种用法的区别在于，第一种用法在命令中包含路径`<paths>`。为了避免路径和引用（或者提交`ID`）同名而冲突，可以在`<paths>`前用两个连续的短线（减号）作为分隔。
第一种用法（包含了路径`<paths>`的用法）不会重置引用，更不会改变工作区，而是用指定提交状态（`<commit>`）下的文件（`<paths>`）替换掉暂存区中的文件。例如命令`git reset HEAD <paths>`相当于取消之前执行的`git add <paths>`命令时改变的暂存区。
第二种用法（不使用路径<paths>的用法）则会重置引用。根据不同的选项，可以对暂存区或者工作区进行重置。参照下面的版本库模型图，来看一看不同的参数对第二种重置语法的影响。

#### 深入了解`git checkout`命令
检出命令（git checkout）是Git最常用的命令之一，同样也很危险，因为这条命令会重写工作区。
```
用法一： git checkout [-q] [<commit>] [--] <paths>...
用法二： git checkout [<branch>]
用法三： git checkout [-m] [[-b|--orphan] <new_branch>] [<start_point>]
```
![](http://oc98nass3.bkt.clouddn.com/14936268612321.jpg)

下面通过一些示例，具体的看一下检出命令的不同用法。
* 命令：`git checkout branch`
检出`branch`分支。要完成如图的三个步骤，更新`HEAD`以指向`branch`分支，以`branch`指向的树更新暂存区和工作区。
* 命令：`git checkout`
汇总显示工作区、暂存区与HEAD的差异。

* 命令：`git checkout HEAD`
 同上.

* 命令：`git checkout – filename`
用暂存区中`filename`文件来覆盖工作区中的`filename`文件。相当于取消自上次执行`git add filename`以来（如果执行过）本地的修改。
这个命令很危险，因为对于本地的修改会悄无声息的覆盖，毫不留情。

* 命令：`git checkout branch – filename`
维持`HEAD`的指向不变。将`branch`所指向的提交中的`filename`替换暂存区和工作区中相应的文件。注意会将暂存区和工作区中的`filename`文件直接覆盖。

* 命令：`git checkout – ` 或写做 `git checkout .`
注意：`git checkout`命令后的参数为一个点（“.”）。这条命令最危险！会取消所有本地的修改（相对于暂存区）。相当于将暂存区的所有文件直接覆盖本地文件，不给用户任何确认的机会！

#### 用reflog挽救错误的重置
如果没有记下重置前master分支指向的提交ID，想要重置回原来的提交真的是一件麻烦的事情（去对象库中一个一个地找）。幸好Git提供了一个挽救机制，通过.git/logs目录下日志文件记录了分支的变更。默认非裸版本库（带有工作区）都提供分支日志功能，这是因为带有工作区的版本库都有如下设置：

```
$ git config core.logallrefupdates
true
```

查看一下`master`分支的日志文件`.git/logs/refs/heads/master`中的内容。下面命令显示了该文件的最后几行。为了排版的需要，还将输出中的40位的`SHA1`提交ID缩短。

```
$ tail -5 .git/logs/refs/heads/master
dca47ab a0c641e Jiang Xin <jiangxin@ossxp.com> 1290999606 +0800    commit (amend): who does commit?
a0c641e e695606 Jiang Xin <jiangxin@ossxp.com> 1291022581 +0800    commit: which version checked in?
e695606 4902dc3 Jiang Xin <jiangxin@ossxp.com> 1291435985 +0800    commit: does master follow this new commit?
4902dc3 e695606 Jiang Xin <jiangxin@ossxp.com> 1291436302 +0800    HEAD^: updating HEAD
e695606 9e8a761 Jiang Xin <jiangxin@ossxp.com> 1291436382 +0800    9e8a761: updating HEAD
```

可以看出这个文件记录了`master`分支指向的变迁，最新的改变追加到文件的末尾因此最后出现。最后一行可以看出因为执行了`git reset –hard`命令，指向的提交`ID`由`e695606`改变为`9e8a761`。
`Git`提供了一个`git reflog`命令，对这个文件进行操作。使用`show`子命令可以显示此文件的内容。

```
$ git reflog show master | head -5
9e8a761 master@{0}: 9e8a761: updating HEAD
e695606 master@{1}: HEAD^: updating HEAD
4902dc3 master@{2}: commit: does master follow this new commit?
e695606 master@{3}: commit: which version checked in?
a0c641e master@{4}: commit (amend): who does commit?
```

使用`git reflog`的输出和直接查看日志文件最大的不同在于显示顺序的不同，即最新改变放在了最前面显示，而且只显示每次改变的最终的`SHA1`哈希值。还有个重要的区别在于使用`git reflog`的输出中还提供一个方便易记的表达式：`<refname>@{<n>}`。这个表达式的含义是引用`<refname>`之前第`<n>`次改变时的`SHA1`哈希值。
那么将引用`master`切换到两次变更之前的值，可以使用下面的命令。
重置`master`为两次改变之前的值。

```
$ git reset --hard master@{2}
```
`HEAD is now at 4902dc3 does master follow this new commit?`

重置后工作区中文件new-commit.txt又回来了。

```
$ ls
new-commit.txt  welcome.txt
```

提交历史也回来了。

```
$ git log --oneline
4902dc3 does master follow this new commit?
e695606 which version checked in?
a0c641e who does commit?
9e8a761 initialized.
```

此时如果再用git reflog查看，会看到恢复master的操作也记录在日志中了。
```
$ git reflog show master | head -5
4902dc3 master@{0}: master@{2}: updating HEAD
9e8a761 master@{1}: 9e8a761: updating HEAD
e695606 master@{2}: HEAD^: updating HEAD
4902dc3 master@{3}: commit: does master follow this new commit?
e695606 master@{4}: commit: which version checked in?
```

### Git Cheat Sheet 
>看了前面那么多命令，是不是头有点晕了？ㄟ( ▔, ▔ )ㄏ平时开发用的到那么多命令吗？
没关系，给你一张好用的常见命令图，忘记了来着看下就行了！

![Git Cheat Sheet](http://oc98nass3.bkt.clouddn.com/2017-07-12-14998489359095.jpg)

### Git分支
1. 查看分支列表
`$ git branch`
2. 新建分支
`$ git branch new_branch_name`
3. 切换到分支
`$ git checkout branch_name`
4. 合并支分支到`master`主分支
`$ git merge branch_name`
5. 删除分支
`$ git branch -d branch_name`

墙裂推荐查看:[3.1 Git 分支 - 分支简介](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%AE%80%E4%BB%8B)
Git 的分支，其实本质上仅仅是指向提交对象的可变指针。 Git 的默认分支名字是 master。 在多次提交操作之后，你其实已经有一个指向最后那个提交对象的 master 分支。 它会在每次的提交操作中自动向前移动。
Git 的 “master” 分支并不是一个特殊分支。 它就跟其它分支完全没有区别。 之所以几乎每一个仓库都有 master 分支，是因为 git init 命令默认创建它，并且大多数人都懒得去改动它。

为了更加形象地说明，我们假设现在有一个工作目录，里面包含了三个将要被暂存和提交的文件。 暂存操作会为每一个文件计算校验和（使用我们在 起步 中提到的 SHA-1 哈希算法），然后会把当前版本的文件快照保存到 Git 仓库中（Git 使用 blob 对象来保存它们），最终将校验和加入到暂存区域等待提交：

Git 是怎么创建新分支的呢？ 很简单，它只是为你创建了一个可以移动的新的指针。 比如，创建一个 testing 分支， 你需要使用 git branch 命令：


![](http://oc98nass3.bkt.clouddn.com/2017-07-13-14999389044006.png)

那么，Git 又是怎么知道当前在哪一个分支上呢？ 也很简单，它有一个名为 HEAD 的特殊指针。 请注意它和许多其它版本控制系统（如 Subversion 或 CVS）里的 HEAD 概念完全不同。 在 Git 中，它是一个指针，指向当前所在的本地分支（译注：将 HEAD 想象为当前分支的别名）。 在本例中，你仍然在 master 分支上。 因为 git branch 命令仅仅 创建 一个新分支，并不会自动切换到新分支中去。
![](http://oc98nass3.bkt.clouddn.com/2017-07-13-14999389256791.png)



### Git撤销方法

1. `git revert <SHA>`
2. `git commit --amend -m "Modify last add message"`
3. 撤销本地的修改`git checkout -- <bad filename>`
4. 重置本地的修改`git reset <last good SHA>`

[Git的各种Undo技巧](https://tonydeng.github.io/2015/07/08/how-to-undo-almost-anything-with-git/)


### Git 冲突

1. 代码冲突`! [rejected] master -> master (non-fast-forward)`的原因以及解决办法：
```
 ! [rejected]        master ->  master (non-fast-forward)  
error: failed to push some refs to 'git@github.com:archermind/LEDTorch.apk-for-Android.git'  
To prevent you from losing history, non-fast-forward updates were rejected  
Merge the remote changes before pushing again.  See the 'Note about  
fast-forwards' section of 'git push --help' for details.  
```
**操作命令：**

*  正确的做法是，在`push`之前`git fetch origin`，将`github`上的新代码拉下来，然后在本地`merge`，如果没有冲突就可以push了，如果有冲突的话要在本地解决冲突后，再`pus`h。具体做法就是：
```
git fetch origin
git merge origin (master)
```
* 这两步其实可以简化为
```
git pull origin master
```

`git-fetch - Download objects and refs from another repository`
`git-merge - Join two or more development histories together`



### Git log


```
Table 3. 限制 git log 输出的选项
选项	说明
-(n)

仅显示最近的 n 条提交

--since, --after

仅显示指定时间之后的提交。

--until, --before

仅显示指定时间之前的提交。

--author

仅显示指定作者相关的提交。

--committer

仅显示指定提交者相关的提交。

--grep

仅显示含指定关键字的提交

-S

仅显示添加或移除了某个关键字的提交

来看一个实际的例子，如果要查看 Git 仓库中，2008 年 10 月期间，Junio Hamano 提交的但未合并的测试文件，可以用下面的查询命令：

$ git log --pretty="%h - %s" --author=gitster --since="2008-10-01" \
   --before="2008-11-01" --no-merges -- t/
5610e3b - Fix testcase failure when extended attributes are in use
acd3b9e - Enhance hold_lock_file_for_{update,append}() API
f563754 - demonstrate breakage of detached checkout with symbolic link HEAD
d1a43f2 - reset --hard/read-tree --reset -u: remove unmerged new paths
51a94af - Fix "checkout --track -b newbranch" on detached HEAD
b0ad11e - pull: allow "git pull origin $something:$current_branch" into an unborn branch
在近 40000 条提交中，上面的输出仅列出了符合条件的 6 条记录。

prev | next

```
### Git Issue
1. [Git - how to track untracked content?](http://stackoverflow.com/questions/4161022/git-how-to-track-untracked-content)


```
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
  (commit or discard the untracked or modified content in submodules)

	modified:   themes/next (modified content, untracked content)
```


2. [git - fatal: Not a valid object name: 'master' - Stack Overflow](https://stackoverflow.com/questions/9162271/fatal-not-a-valid-object-name-master)
That is again correct behaviour. Until you commit, there is no master branch.

You haven't asked a question, but I'll answer the question I assumed you mean to ask. Add one or more files to your directory, and git add them to prepare a commit. Then git commit to create your initial commit and master branch.

没有提交的话是没有`master`分支的，也就无法创建新的分支，只了一次有提交记录后，才创建了`master`分支。




# Git 一些好用的插件😝~
## Gitsome
[Gitsome](https://github.com/donnemartin/gitsome) （2017-05-19）

### 一、查看`Github`上的流行库
`gh trending objective-c  -w -p`
`gh trending swift  -w -b`
`-b`是在浏览器中打开，`-p`是在`shell`中打开,`Github`有时候会抽，建议还是用`-p`
### 二、 查看`github`的通知、库、拉取请求、账户等信息
`gh view`

>View the given notification/repo/issue/pull_request/user index in the terminal or a browser.

>This method is meant to be called after one of the following commands which outputs a table of notifications/repos/issues/pull_requests/users:


```
gh repos
gh search-repos
gh starred

gh issues
gh pull-requests
gh search-issues

gh notifications
gh trending

gh user
gh me
```

栗子~

```
$ gh repos
$ gh view 1

$ gh starred
$ gh view 1 -b
$ gh view 1 --browser
```

## Github

>提到`Git`，我觉得很多人会想到`Github`，甚至很多人以为是同一回事，这里有必要提一下~

![Github知乎](http://upload-images.jianshu.io/upload_images/225323-fe9f3e6a5a6ebcd5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

作为关于世界上最大的"同Xin交友"平台，一些常见的特点就不在此说了，感兴趣的可以参考知乎上的这个问题[怎样使用 GitHub？](https://www.zhihu.com/question/20070065)，这里主要介绍一些`Github`上我觉得很好用的插件

### NO.1 [Zenhub](www.zenhub.com) —— GitHub 中的敏捷开发流~
[ZenHub Guide](https://www.zenhub.com/guides#reporting-with-zenhub)
[Zenhub-boards](https://www.zenhub.com/guides/setup-my-zenhub-boards)
[ZenHub - Agile GitHub Project Management](https://www.zenhub.com/guides/setup-my-zenhub-boards)
[Your first sprint using ZenHub](https://www.zenhub.com/guides/your-first-sprint-using-zenhub#what-goes-into-a-sprint)

#### Zenhub issuse template
![](http://oc98nass3.bkt.clouddn.com/2017-07-11-14997767891391.png)

#### ZenHub Board

![ZenHub Board](http://oc98nass3.bkt.clouddn.com/2017-07-11-14997732663573.jpg)
![Sprint-based workflow](http://oc98nass3.bkt.clouddn.com/2017-07-11-14997745630454.jpg)

* New Issues: 新问题：新问题自动登陆。你应该尽快把他们从这里拖出去。

* Icebox: 冰箱：冰箱代表着一个低优先级的产品待办事项。剩下的问题在冰箱上删除它们有助于避免循环提高重复的问题。冰箱的问题不应采取了团队成员的时间和脑力带宽；把想法放入冰箱管道获取他们出来的方式，帮助团队专注于当前的优先事项。

* Backlog: 待办事项：积压问题不是当前的焦点，但您将在某个时刻对它们进行处理。如果他们没有一个GitHub的里程碑，你可以考虑他们的一部分你的“产品积压”。一旦你添加了一个里程碑，它们就成为你的“待办事项待办事项”（即即将在即将到来的Sprint中完成的任务）的一部分。

* In progress:  在进展中：这里的问题应该有大量的细节，比如估计和需求，因为它们是你团队当前的焦点。这就是“你现在在做什么？”理想情况下，每个团队成员都应该一次只做一件事。在这里的任务应该按照优先受让人加入。

* Review/QA: 评审/ QA：使用评审/ QA专栏对团队开放的问题进行评审和测试。通常这意味着代码已经准备好部署，或者已经处于一个登台环境中。

* Done: 完成：如果你问三个人“完成”意味着什么，你可能会得到三种不同的答案。这就是为什么作为团队讨论你的“完成的定义”是非常重要的！
##### 查看看板

* 这种结构是否只包含我们所需要的，仅此而已？我的老板能看一下这个项目并了解她需要做的一切吗？

* 每一个重要的利益相关者代表吗？在设计，市场营销，或QA的人可以看看这个委员会，并知道确切的地方，他或她的帮助是必要的？

* 我们错过了重复的阶段吗？想想你的团队是如何构建产品的。这一切都是为了让问题在董事会中传播。如果你有一个QA部门，例如，你可能需要一个“准备好QA”的管道。

* “完成”真的完成了吗？我的团队知道并理解我们的定义吗？这种经常错过的步骤是任何敏捷项目的关键部分。

##### Epics


GitHub的问题没有真正的层次；它只是一个简单的列表。深入了解哪些问题是相关的、相互阻碍的、依赖于其他工作的，或者是对项目正在进行的工作的感觉是很难确定的。

zenhub add a crucial layer of hierarchy to your GitHub Issues. 
通过史诗，你可以在发布过程中获得更大的端到端控制权。zenhub史诗帮助束相似的任务为工作主题，以帮助您规划和跟踪更大目标的工作。

##### Issue Dependencies

zenhub依赖帮助团队的问题和故事，当运动项目正得到更好的端到端的能见度。这些信息使团队更了解为什么会发生阻塞，以及需要采取哪些措施来减少风险。


##### 流程

* 新的反馈和想法自动降落在新的问题管道中。

* 产品负责人审查每一个问题，并计算出如果它是可操作的。
注：“产品拥有者”指的是对最终产品和何时进行最终呼叫的人。通常，它是一个项目或产品经理。

* 如果你打算完成一个问题，但它还没有准备好开发（需要更多的细节或者团队没有额外工作的能力），这个问题被拖到了积压。这里的问题还没有一个里程碑。您将在冲刺开始时添加一个。

* 如果它是一个有价值的问题，但你没有计划去解决它在即将到来的冲刺，“冻结”在冰箱管道。

* 如果问题不能解决，就关闭它。如果它真的那么重要，你可以随时重新打开它！

* 问题准备好了吗？在这一点上，您的团队应该添加一些细节，如验收要求和用户故事（从用户利益的角度来描述一个简短的特性描述）。一旦一个评估和一个里程碑被附加到一个问题上，它就正式地成为你的“待办事项积压”（你将在下一次冲刺中处理的东西）。

* 当`Sprint`开始时，只需按里程碑过滤板子，看看需要做些什么。团队成员可以将任务拖到积压的顶部，以指示它们正在工作。简单的！

##### Your first sprint with Zenhub

从熟悉GitHub的里程碑和完成报告，成为更有效的利用zenhub冲刺计划，这将确保你知道插件和冲刺计划的最佳实践与ZenHub出局。

冲刺前要回答的重要问题

* 我们实际能处理多少工作？
* 我们真的能在接下来的两周内完成所有这些工作吗？我们可能从范围中删除哪些问题？
* 您的团队在Sprint计划前应处理的所有问题。但是你如何得到这些问题的答案呢？
你的几次冲刺，为了回答这些问题，最好的办法就是跟你的团队和有关于复杂性的工作你想完全开放的对话，以及确保应对即将到来的关键期限内必须完成。

* 决定什么进入你的冲刺

* 要想在里程碑中添加哪些问题，您需要有一个健康的产品待办事项清单。
* 
这意味着你的问题应该有：

* 估计你和你的团队一起决定

* 一个用户故事，它描述一个任务的对象和原因，以及任何需求。不要担心添加太多的细节-你会发现更多的信息，一旦你开始你的里程碑。

* 根据发行商的业务价值确定优先权

##### 创建使用GitHub的里程碑第一冲刺


为与终点一致的里程碑选择一个截止日期。一个经验法则来决定一个冲刺应该多长时间，如果你还没有测量时间的工作时间，那就是问问自己是否能得到一个新的特性或增强，团队在你所创建的时间范围内完成整个开发周期。

如果2周看起来太短了，那么看看你的问题，问问自己是不是太大了，不值得处理。将工作分解成更小的块不仅提高了它们被运送的可能性，而且消除了延迟发布的潜在缺陷数量。

一旦创建了里程碑，就应该向您的Sprint添加问题了！回到主板选项卡开始。

既然已经将Sprint定义为里程碑，那么您就可以计划在Sprint中完成什么工作了。记住一件事情，为你准备一个冲刺，从来没有一支球队拥有所有的信息需要向前完美的依赖，冲突，或某些bug修复可能出现的紧迫性…所有这些都是计划外的工作。

这些计划中的许多情况可以在计划会议中发现，也称作Sprint计划。在开始第一次冲刺之前，快速交谈是一个讨论的平台。

##### 向Sprint添加用户故事和任务

在GitHub上，一旦你添加了问题的一个里程碑，他们可以被认为是一个冲刺积压`Backlog`的一部分。

Sprint积压`Backlog`与产品积压`product backlog`问题不同，在您的积压管道中没有里程碑的是您的“产品待办事项”——这些事情您最终将处理，但不是您下一个工作的直接冲刺的一部分。Sprint积压是您的团队承诺在接下来的2周时间内完成的所有问题（或者您用来定义自己迭代的时间表）。

在一个项目的开始，估计是最好的猜测。与瀑布开发相反，大多数敏捷团队现在都会在任务和项目中发现更多关于任务的细节。在项目开始的时候你不会知道太多，这没关系。

为什么要估算软件？估算一个任务是有帮助的，当你整理你的Sprint待办事项：给定的预算和固定的时间，你怎么知道哪些问题要处理，如果不是为了估计？

其次，当历史数据配对（如速度图），估计照明如何快速你真的动–具有洞察力的有效项目管理的一个重要GitHub。

#### CircleCI

持续集成 — [Circleci](https://circleci.com/)
后续更新中...

#### Reviewable

代码Review — Reviewable
[Reviewable](https://reviewable.io/) GitHub code reviews done right
后续更新中...
#### Coveralls

[Coveralls](www.coveralls.com)
代码覆盖率 — Coveralls

后续更新中...


## Gitlab-CI

### 参考[GitLab Continuous Integration & Deployment Pipelines](https://about.gitlab.com/features/gitlab-ci-cd/)

1. [使用Gitlab CI进行持续集成](http://www.jianshu.com/p/315cfa4f9e3e)
2. 使用`Travis CI`[Travis CI 自动部署 Hexo 博客到 Github(带主题版)](http://ixiusama.com/2017/01/03/hexo-Automatic-deployment-on-github-theme-next/#more)
3. [GitLab integration #5931](https://github.com/travis-ci/travis-ci/issues/5931)
4. [How do travis-ci and gitlab-ci compare?](http://stackoverflow.com/questions/31338775/how-do-travis-ci-and-gitlab-ci-compare)


## 参考

1. [图解Git](https://marklodato.github.io/visual-git-guide/index-zh-cn.html#commands-in-detail)
2. [**git - the simple guide - no deep shit!**](http://rogerdudler.github.io/git-guide/index.zh.html)
3. [**LearnGitBranching**](http://learngitbranching.js.org/?NODEMO)
4. [**git-flow 备忘清单**](http://danielkummer.github.io/git-flow-cheatsheet/index.zh_CN.html)
5. [Gitsome](https://github.com/donnemartin/gitsome)
6. [Git Recipes](https://github.com/geeeeeeeeek/git-recipes/wiki)

