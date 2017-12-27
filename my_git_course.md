![tools/git/git-banner](http://omu538iq8.bkt.clouddn.com/tools/git/git-banner.jpg)

这篇博文是自己在学习git过程中的思考总结。本文仅仅代表个人的看法，如有不妥地方还请本文文末留言。 😊 

<!--more-->

## GIT是什么

`GIT`是一个免费并且开源的分布式版本控制系统，能够高速有效的处理或小或大的项目。(以上的话是自己翻译[github官网](https://git-scm.com/))

至今，自己用过了window系统的`TortoiseSVN`, mac系统的`CornerStone`,最近的大半年也在用`GIT`(主要管理自己的[github项目](https://github.com/reng99))。比较下来，还是GIT优势比较明显<del>，虽然目前为止没有用GIT开发过团队项目</del>。

**GIT跨平台**

`GIT`可以在不同的操作系统中使用。也许你注意到了，我在window上和mac系统上工作的时候是使用两个不同的svn。如果我在linux上工作会不会又是一个呢。

**GIT是分布式版本控制系统,而svn是集中式版本控制系统**

`集中式版本控制系统`是集中放在中央服务器上面的，而团队的人需要从中央服务器上面拉取最新的代码，然后进行开发，最后推送到中央服务器上面，就像串联的电路。而`分布式版本控制系统`没有中央服务器，团队的每个人的电脑就是一个完整的版本库，就好像并联的电路（自我理解）。

`集中式版本控制系统`必须联网才能工作，如果是在局域网内还好，带宽足够大，速度足够快，但是遇到网速慢的话，那心里就一万个羊驼🐑在蹦腾了。

`集中式版本控制系统`安全性比较低，如果中央系统崩溃了，那就有点悲催了。当然你不嫌麻烦，可以定期备份的啦。而`分布式中央系统`就比较安全，团队的每个成员的电脑就是一个完整的版本库。如果其中一个坏掉了，你可以从团队另外一个的人员电脑那里拷贝一份就行了。对了，`GIT`也会有一台中央的机子，主要是为了方便团队的交流，它是可以不存在的。

## GIT安装

`GIT`支持不同的系统，看者可以在链接[https://git-scm.com/downloads](https://git-scm.com/downloads)中，找到和自己电脑系统匹配的`GIT`版本，下载安装包后根据提示进行安装。当然，`GIT`还提供图形界面管理工具，看者也可以在链接中下载`GUI Clients`，如下图所示--
![tools/git/git_install](http://omu538iq8.bkt.clouddn.com/tools/git/git_install.png)
根据提示安装完成后，要验证是否安装成功。看者可打开命令行工具，输入`git --version`命令,如果安装成功，控制台输出安装的版本号（当然，安装前就应该输入git --version查看是否安装了git），我这里安装的`GIT`版本是`2.10.0`。

## GIT配置

`GIT`在使用前，需要进行相关的配置。每台计算机上面只需要配置一次，程序升级的时候会保留配置信息。当然，看者可以在任何时候再次通过运行命令行来修改它们。

### 用户信息

设置`GIT`的用户名称和邮件地址，这个很重要，因为每个`GIT`的提交都会使用这些信息，并且它会写入到每一次的提交中。你可以在自己的仓库中使用`git log`，控制台上面显示的每次的提交都有`Author`字段，它的值就是`用户名称 <邮件地址>`。方便查看某次的提交的负责人是谁。

```bash
$ git config --global user.name "你的用户名"
$ git config --global user.email 你的邮箱地址 
```

⚠️ `GIT`一般和`github`配合使用，看者应该设置用户名称为你的`github`用户名。当然，还有和`gitlab`等配合使用...

⚠️ 如果配置中使用了`--global`选项，那么该命令只需要运行一次，因为之后无论你在该系统上做任何事情，`GIT`都会使用这些信息。但是，当你想针对特定项目使用不同的用户名称与邮件地址的时候，可以在那个仓库目录下运行不使用`global`选项的命令来配置。

### 检查配置信息

通过`git config --list`命令可以列出所有`GIT`能找到的配置。如下：（我的git版本为2.10.0）

```bash
...
user.name=reng99
user.email=1837895991@qq.com
color.ui=true
core.repositoryformatversion=0
core.filemode=true
core.bare=false
...
```

当然，你可以通过`git config <key>`来检查`GIT`的某一项配置。比如`$ git config user.name`。

### 帮助中心

在使用`GIT`的时候，遇到问题寻求帮助的时候，可以运行`git help`或`git --help`或`git`命令来查看。在控制台上会展示相关的帮助啦。

```bash
usage:
	...
start a working area (see also: git help tutorial)
	...
work on the current change (see also: git help everyday)
	...
examine the history and state (see alse: git help revisions)
	...
grow,mark and tweak your common history
	...
collaborate (see also: git help workflows)
	...
```

更加详细的内容，请点击[传送门](https://git-scm.com/docs/git-help)

## 创建版本库 

版本库又名仓库(repository)，可以理解成一个目录，这个目录里面所有文件都可以被`GIT`管理起来，每个文件的修改、删除，`GIT`都能跟踪，以便任何时刻都能可以追踪历史，或者在将来某个时刻可以还原。

创建一个版本库，首先得选择一个存放目录的地方，我这里选择了桌面，并且创建一个空的目录。

```bash
$ cd desktop
$ mkdir -p learngit
$ cd learngit
$ pwd
/Users/reng/desktop/learngit
```

`mkdir -p dirnanme`是创建一个子目录，这里的`-p`确保目录的名称存在，如果目录不存在的就新建一个，如果你确定目录不存在，直接使用`mkdir dirname`就可以了。`pwd(Print Working Directory)`是显示当前目录的整个路径名。

然后，通过命令行`git init`，将创建的目录变成`GIT`可以管理的仓库:

```bash
$ git init 
Initialized empty Git repository in /Users/reng/Desktop/learngit/.git/
```

初始化好仓库后就可以愉快的玩耍了，但是，得先来了解下`GIT`整个工作流程先。

## GIT工作流程

为了更好的学习，自己用`Axure RP 8`粗略的画了下流程图，如下--
![tools/git/git_brief_process](http://omu538iq8.bkt.clouddn.com/tools/git/git_brief_process.png)

本地仓库(repo)包含工作区和版本库,那么什么是工作区和版本库呢？基本的流程又是什么呢？

### 工作区和版本库

我们新建一个仓库，就像我们新建的`learngit`仓库，现在在里面添加一个文件`README.md`，用sublime打开`learngit`目录。此时会出现如下图的情况(当然你设置了其他东西例外)--
![tools/git/working_version_area](http://omu538iq8.bkt.clouddn.com/tools/git/working_version_area.png)
如上图，出现的内容就是工作区（ 电脑上能看到的此目录下的内容），这里工作区只有`README.md`一个文件。工作区有一个隐藏的目录`.git`，这个不算工作区，而是`GIT`的版本库。版本库又包括暂存区和GIT仓库。暂存区是一个文件，保存了下次将提交的文件列表信息，而GIT仓库目录是`GIT`用来保存项目的元数据和对象数据库的地方。这是`GIT`中最重要的部分，从其他计算机克隆仓库的时候，拷贝的就是这里的数据。当执行`git add .`或者`git add path/to/filename`的时候，文件从工作区转到暂存区；执行`git commit -m"here is the message described the file you add"`的时候,文件从缓存区添加到GIT仓库。

### 基本的工作流

基本的`GIT`工作流可以简单总结如下--

1. 在工作区目录中修改文件
2. 暂存区中暂存文件，将文件的快照放入暂存区域
3. 提交更新，找到暂存区域的文件，将快照永久性存储到GIT仓库目录

## 时光机穿梭

到目前为止，在自己创建的本地仓库--`learngit`中已经初具形态了。进入`learngit`，执行`ls`，可看到目前仓库中已有的文件README.md。

```bash
$ cd desktop/learngit
$ ls
README.md
$ cat README.md
## content
```

上面展示了本地`learngit`内的相关的内容。运行下`git status`查看现在的状态。

```bash
$ git status
On branch master
nothing to commit, working tree clean
```

这时候会提示没有内容可以提交，工作区是干净的。因为我之前已经提交(git commit)过了。上面还提示了目前是位于主分支上面，`GIT`在初始化(git init)的时候会自动创建一个`HEAD`指针指向默认`master`分支，也只有一个分支，看者可以通过`git branch`查看。

现在，在`README.md`上添加一些内容。

```bash
## content

### first change
```

此刻再通过`git status`查看当前状态。

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

这时候显示出一堆的东西，告诉我们现在是位于主分支上面，然后告诉我们修改的文件啊，可以使用的命令进行下一步的操纵。那么我们来进行下一步的操作了，`git add . 或者 git add README.md`将修改的文件添加到暂存区域。

```bash
$ git add .
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   README.md
```

对了，有时候需要在添加的之前（执行git add .  或者 git add path/to/filename）的时候，需要看下修改了哪些内容可以执行下`git diff`。那么，现在先回退到修改的前一个版本。

```bash
$ git reset HEAD README.md
Unstaged changes after reset:
M	README.md
$ git checkout -- README.md
$ ls
README.md
$ cat README.md
## content
```

回退正确，现在像上次那样添加内容`### first change`，然后执行命令`git diff`来查看更改的内容。

```bash
$ git diff
diff --git a/README.md b/README.md
index 75759ec..0bc52b9 100644
--- a/README.md
+++ b/README.md
@@ -1 +1,3 @@
-## content
\ No newline at end of file
+## content
+
+### first change
\ No newline at end of file
```

现在就显示了修改前的内容--`-`前为修改前的内容，和修改后的内容--`+`前修改后的内容。查看完之后，觉得没有问题了，就可以进行添加(git add)，提交(git commit)。当然，一般不常用git diff的，因为自己修改的东西自己心里总有点数吧，可能合作中团队的其他人需要查看文件前后的不同点就需要用到`git diff`啦。

### 版本回退

为了方便讲解下版本回退，我先将上面添加的`### first change`提交以下--`git add . && git commit -m "add first change"`。下面通过`git log`就可以查看自己提交的记录了。

```bash
$ git log
commit 5c2639ee54bd7c8ef2cbf186f5dc4798e72a4a67
Author: reng99 <1837895991@qq.com>
Date:   Sun Dec 17 17:11:53 2017 +0800

    init README.md
    
$ git add . && git commit -m "add first change"
[master 0ac49ba] add first change
 1 file changed, 3 insertions(+), 1 deletion(-)
 
$ git log
commit 0ac49bae6ab55df9c05d0770de347665a2568f31
Author: reng99 <1837895991@qq.com>
Date:   Mon Dec 18 15:26:06 2017 +0800

    add first change

commit 5c2639ee54bd7c8ef2cbf186f5dc4798e72a4a67
Author: reng99 <1837895991@qq.com>
Date:   Sun Dec 17 17:11:53 2017 +0800

    init README.md
```

在上面中，自己先执行了`git log`来显示提交的日志，显示只有一条，然后执行了add和commit的命令，打印的内容是现实主分支、commit的id、commit的信息、多少个文件的更改、多少个插入以及多少个删除。之后再次执行`git log`打印日志，显示了两次提交。⚠️ 注意：当提交(commit)的次数较多之后，控制台会显示不下（最多现实4条）那么多的条数，可以通过按键盘的`向上或向下`键查看日志的内容，需要退出查看日志命令的话，在`英文输入法的状态按下q`，意思就是quit(退出)。

版本的回退就是改变`HEAD`指针的指向。通过`git reset --hard HEAD^`返回上一个版本，通过`git reset --hard HEAD^^`返回上上个版本...由此推论，往上100个版本的话就是100个`^`，当然，这样你数到明天也未必数得正确，所以写成`git reset --hard HEAD~100`。另外一种是，你知道提交的id，例如`commit 5c2639ee54bd7c8ef2cbf186f5dc4798e72a4a67`的前7位就是commit的id(5c2639e)，执行`git reset --hard 5c2639e`就回到此版本啦。

```bash
$ reng$ git reset --hard HEAD^
HEAD is now at 5c2639e init README.md
$ git log
commit 5c2639ee54bd7c8ef2cbf186f5dc4798e72a4a67
Author: reng99 <1837895991@qq.com>
Date:   Sun Dec 17 17:11:53 2017 +0800

    init README.md
$ ls
README.md
$ cat README.md
## content
```

现在你已经回到了最初的版本，这里演示的是通过`HEAD`，你也可以通过`commit id`来实现的。执行上面的代码后，`README.md`文件里面只有一`### content`文字内容,但是过了段时间后，你想恢复到原先的版本，通过`git log`命令行，控制台显示的以前的信息，通过它找不到回退前的`commit id`，怎么办？`GIT`提供一个`git reflog`显示提交的历史记录，在那里可以查看提交的id、`HEAD`的指针历史和操作的信息记录。下面演示回退到最新的版本（也就是commit -m "add first change"）--

```bash
$ git log
commit 5c2639ee54bd7c8ef2cbf186f5dc4798e72a4a67
Author: reng99 <1837895991@qq.com>
Date:   Sun Dec 17 17:11:53 2017 +0800

    init README.md
$ git reflog
5c2639e HEAD@{0}: reset: moving to HEAD^
0ac49ba HEAD@{1}: commit: add first change
5c2639e HEAD@{2}: commit (initial): init README.md
$ git reset --hard 0ac49ba
HEAD is now at 0ac49ba add first change
$ ls
README.md
$ cat README.md
## content

### first 
```

现在又回到了最新的版本，又能够愉快的玩耍了。😊

### 管理修改

`GIT`比其他版本控制系统设计优秀，其中一点是--`GIT`跟踪并管理的是修改，而非文件。

下面在`README.md`内添加信息`### second change`。之后看下变化后的文件的状态和差异等。

```bash
$ ls
README.md
$ cat README.md
## content

### first change

#### second change
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
$ git add README.md
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   README.md
```

此时，对`README.md`进行第三次的修改，添加内容`### third change`。

```bash
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   README.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   README.md

$ cat README.md
## content

### first change

#### second change

### third change
$ git commit -m "test file modify"
[master 18f86ba] test file modify
 1 file changed, 3 insertions(+), 1 deletion(-)
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

上面的演示流程是这样的`第一次修改(#### second change) -> git add -> 第二次修改(### third change) -> git commit`。但是最后查看状态的时候(git status)，第二次的修改并没有被提交上去。因为`GIT`管理的是修改，当使用`git add`命令的时候，在工作区的第一次修改被放入暂存区，准备提交，但是在工作区的第二次修改并没有放入到暂存区，而`git commit`是将暂存区的修改提交到`GIT仓库`，所以第二次修改的内容是不会被提交的。这也是说明为什么可以多次添加(git add)，一次提交(git commit)的原因了。

### 撤销修改

文件的撤销修改分成三种情况，一种是修改在工作区的内容，一种是修改在暂存区的内容，另一种是修改在`GIT`仓库的内容。也许会有看者说，不能修改在远程库中的内容吗？有啊，就是`git add`->`git commit`->`git push`将远程仓库的内容覆盖被，不过团队人在克隆远程库下来的时候，还是可以查看到你提交的错误内容的。我们现在只针对本地仓库的三种情况谈下自己的看法--

**情况一：撤销工作区的内容**

在管理修改中，自己的工作区还是没有提交，此时想放弃当前工作区的编辑内容执行`git checkout -- file`。接着上面的内容，我这里的工作区内有的内容是`### third change`，现在我要放弃第三次修改，只要执行`git checkout -- README.md`就可以了。

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
$ ls
README.md
$ cat README.md
## content

### first change

#### second change

### third change
$ git checkout -- README.md
$ cat README.md
## content

### first change

#### second change
$ git status
On branch master
nothing to commit, working tree clean
```

**情况二：撤销暂存区的内容**

当你不但改乱了工作区的某个文件的内容，还添加(git add)到了暂存区时，想丢弃修改，那么得分两步来撤销文件。先是通过`git reset HEAD file`，将暂存区的文件退回到工作区，然后通过`git checkout -- file`放弃修改改文件的内容。为了方便演示，我这里的暂存区没什么内容，所以添加内容`### tentative content`并将它添加到缓存区。之后，演示将缓存区的内容撤回--

```bash
$ cat README.md
## content

### first change

#### second change

### tentative content
$ git add .
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   README.md

$ git reset HEAD README.md
Unstaged changes after reset:
M	README.md
$ git checkout -- README.md
$ cat README.md
## content

### first change

#### second change
$ git status
On branch master
nothing to commit, working tree clean
```

**情况三：撤销GIT仓库的内容**

如果你不仅添加(git add)了内容到暂存区并且提交(git commit)了内容到`GIT仓库`中了。你需要撤销上一次的内容，也就是要回退到上一个版本，执行`git reset --hard HEAD^`就可以啦，详细的内容查看`版本回退`。如下--

```bash
$ git status
On branch master
nothing to commit, working tree clean
$ cat README.md
## content

### first change

#### second change
$ git reset --hard HEAD^
HEAD is now at 0ac49ba add first change
$ cat READMEmd
## content

### first change
```

##  远程仓库

远程仓库的使用能够提高你和团队的工作效率，无论何时何地，团队的人员都可以在联网的情况下将代码进行拉取，修改和更新。因为我是使用`github`来管理项目的，所以我的远程仓库是放在github里面。这里默认看者已经安装了`github`，当然也可以用码云、gitlab等。

### 本地库添加到远程库

这点很容易，登录自己注册的[github](https://github.com/)，如果打不开，请开下VPN。进入自己的首页(https://github.com/username)，点击`+`号创建(new repository)一个名为`learngit`的仓库(注意哦⚠️ 名称是本地仓库已经初始化过的，我这里本地有个同名初始化的learngit仓库)，其他的字段自选来填写。点击`Create repository`创建此远程仓库。紧接着就是进行本地仓库和远程仓库的关联啦，`github`很友好的提示了你怎么进行一个远程仓库的关联。

![tools/git/related_github_step1](http://omu538iq8.bkt.clouddn.com/tools/git/related_github_step1.png)
![tools/git/related_github_step2](http://omu538iq8.bkt.clouddn.com/tools/git/related_github_step2.png)
![tools/git/related_github_step3](http://omu538iq8.bkt.clouddn.com/tools/git/related_github_step3.png)

现在按照上图来关联下远程仓库。

```bash
$ git remote add origin https://github.com/reng99/learngit.git
$ git push -u origin master
Counting objects: 6, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (6/6), 456 bytes | 0 bytes/s, done.
Total 6 (delta 0), reused 0 (delta 0)
To https://github.com/reng99/learngit.git
 * [new branch]      master -> master
Branch master set up to track remote branch master from origin.
```

注意⚠️ 第一次向远程仓库（关联）push的时候是`$ git push -u origin master`，不能忽略`-u`，以后的push不用带`-u`。至此，打开你的github的相关的仓库就可以看到添加了`README.md`文件，我这里地址是`https://github.com/reng99/learngit`，因为我是使用markdown语法写的，控制台显示的内容和仓库的显示内容有所区别啦。<del>(⚠️ 后期我将learngit仓库删除啦，所以你访问链接是找不到这个仓库的，毕竟不想放一个没什么内容的仓库在我的github上)</del>。

### 远程库克隆到本地

从远程仓库克隆东西到本地同样很简单，只需要进入你想克隆的仓库，将仓库的`url`复制下来（当然你也可以复制window.location.href的内容），运行`git clone address`。现在我将本地桌面的`learngit`的仓库删除，然后从远程将`learngit`克隆到本地。

![tools/git/git_clone_repository](http://omu538iq8.bkt.clouddn.com/tools/git/git_clone_repository.png)

```bash
$ cd desktop
$ rm -rf learngit
$ find learngit
find: learngit: No such file or directory
$ git clone https://github.com/reng99/learngit
Cloning into 'learngit'...
remote: Counting objects: 6, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 6 (delta 0), reused 6 (delta 0), pack-reused 0
Unpacking objects: 100% (6/6), done.
```

成功将`gitlearn`从远程克隆下来，接下来又可以愉快的玩耍啦。

## 分支管理

分支管理允许创建另一条线/方向上开发，能够让你在不影响他人工作的情况下，正常的工作。当在自己创建的分支中完成自己的功过后，合并到主分支就行了(git init初始化的时候已经默认创建了master主分支)。一般团队的合作是不在主分支上进行的，个人项目除外（个人理解）。

### 创建分支

当前`learngit`仓库上只有一个分支，那就是`master`分支，看者可以通过`git branch`命令来查看当前的分支，`git branch branchName`命令来创建一个新的分支，我这里创建的是`dev`分支。

```bash
$ cd desktop/learngit
$ git branch
* master
$ git branch dev
$ git branch
  dev
* master
```

现在已经创建了`dev`分支，有两个分支了，分支前面带有一个星号的分支说明是当前的正在工作的分区。执行上面的分支后，可以简单的画下现在的情况了，有个`HEAD`指针指向主分支的最新点，刚才新创建的`dev`分支我这里默认是一个`dev`的指针指向了`dev`分支的最新点。

```bash
.
.		HEAD指针
.		│
├────────*master
└────────dev
		│	
		dev指针	
```

### 切换分支

我们一般是很少在主分支进行工作的，所以在创建出新的分支之后，我们就切换到新的分支进行相关的工作。可以通过`git checkout branchName`切换到已经存在的分支工作，通过分支前面的`*`可查看目前位于哪个分支内。现在我切换到创建的`dev`分支。

```bash
$ git branch
  dev
* master
$ git checkout dev
Switched to branch 'dev'
$ git branch
* dev
  master
```

### 合并分支

在创建好分支后，我们在新的分支上工作完成后，就需要往主分支上进行合并啦。我修改了分支`dev`上的`README.md`的内容，就是添加文字`### new branch content`。合并分支可以分成两个合并的方式，一种是本地合并到materz主分支之后，推送(push)到远程库，一种是直接将分支推送到远程库，在远程库进行合并。

**本地合并推送**

在合并分支前，需要切换到要合并到哪个分支(一般是master主分支)，通过`git merge branchName`将需要的合并的分支合并到当前分支，我是将`dev`分支合并到`master`分支。

```bash
$ git branch
* dev
  master
$ git checkout master
M	README.md
Switched to branch 'master'
Your branch is up-to-date with 'origin/master'.
$ git merge dev
$ Already up-to-date.
$ git add .
$ git commit -m "merge dev branch"
[master d705e73] merge dev branch
 1 file changed, 3 insertions(+), 1 deletion(-)
$ git push origin master
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 282 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/reng99/learngit
   0ac49ba..d705e73  master -> master
```

合并之后,此时，`HEAD`指针就指向了`dev`指针，也就是两者同时指向了`master`主分支的最新处。具体的内容参考[传送门](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375840038939c291467cc7c747b1810aab2fb8863508000)

```bash
.
.			
.			
├────────*master
└────────dev
		│	
		dev指针	 ── HEAD指针
```

**远程库推送合并**

远程库内合并的话，要先将`dev`的分支推送到远程库，然后在远程库进行合并。我这里在`dev`分支上添加了`### add new branch content into again`然后demo演示推送(git push origin dev)以及合并。

```bash
$ git branch
  dev
* master
$ git checkout dev
Switched to branch 'dev'
$ git add .
$ git commit -m "add dev branch commit again"
[dev dc817c4] add dev branch commit again
 1 file changed, 3 insertions(+), 1 deletion(-)
$ git push origin dev
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 300 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/reng99/learngit
 * [new branch]      dev -> dev
```

接下来就是进入[我的远程learngit仓库](https://github.com/reng99/learngit)进行合并，你会看到下面图示的提示。点击`Compare && pull request`，然后写点相关的`comment`（选填），点击`Create pull request`。之后在绿色勾的提示下`Merge pull request`，紧接着点击`Confirm merge`按钮确定合并此分支，这时候返回主分支就可以看到`dev`内合并的内容了(后期我改动了dev的内容)。看者如果看得不明白，自己上手尝试一下呗！

![tools/git/merge_branch](http://omu538iq8.bkt.clouddn.com/tools/git/merge_branch.png)

完成后，你会看到`learngit`仓库的`Pull requests`量为1，`branches`量为2。你可以点击进入分支，在`ALL branches`里面查看分支的具体内容。

### 删除分支

在创建了分支，然后将分支的内容合并到主分支后，分支的使命就完成了，你就可以将分支删除了，这里的删除个人认为可以是两种，一种是本地仓库的分支删除，一种是远程仓库的分支的删除。当然啦，留着分支也没啥，可以留着呗<del>，自己认为有点碍眼</del>。

**本地分支的删除**

在本地的`learngit`的目录下，执行命令行`git branch -D branchName`就可以删除了。我这里删除的是`dev`分支。注意⚠️ ，删除的分支不应该是当前工作的分支，需要切换到其他分支，我这里切换的是`master`分支，毕竟我只有两个分支呢。

```bash
$ git branch
* dev
  master
$ git branch -D dev
error: Cannot delete branch 'dev' checked out at '/Users/reng/desktop/learngit'
$ git checkout master
Switched to branch 'master'
Your branch is up-to-date with 'origin/master'.
$ git branch
  dev
* master
$ git branch -D dev
Deleted branch dev (was dc817c4).
$ git branch
* master
```

**远程库分支的删除**

删除远程库的分支，只要执行`git push origin :branchName`命令就行了。现在我要删除我远程库中的`dev`分支，执行`git push origin :dev`。

```
$ git push origin :dev
To https://github.com/reng99/learngit
 - [deleted]         dev
```
此时，打开我的远程库[learngit](https://github.com/reng99/learngit)，发现之前的`Pull requests`量为0，`branch`量为1。

### 重命名分支

通过`git branch -m oldBranchName newBranchName`来重命名分支。我这里没有分支了，现在创建一个`reng`分支，然后将它重命名为`dev`分支。

```bash
$ git branch
* master
$ git branch reng
$ git branch
* master
  reng
$ git branch -m reng dev
$ git branch
  dev
* master
```

### 解决冲突

在我们开发的时候，不知道分支和分支之间的进度情况是什么，难免会产生冲突。当产生冲突的时候，就得将冲突的内容更正，然后提交。为了方便演示，我将本地的`learngit`删除，重新拉取远程的`gitlearn`仓库(因为我不知道我之前在本地仓库做的修改是啥，对了，我将远程的分支删除了，只剩下master主分支)。克隆下来后，如果还存在本地分支，也将它删除，之后我将在`master`和`dev`分支中重新填充里面的`README.md`的内容。

```bash
$ cd desktop
$ git clone https://github.com/reng99/learngit.git
Cloning into 'learngit'...
remote: Counting objects: 43, done.
remote: Compressing objects: 100% (17/17), done.
remote: Total 43 (delta 4), reused 38 (delta 1), pack-reused 0
Unpacking objects: 100% (43/43), done.
$ cd learngit
$ git branch
* master
$ ls
README.md
$ cat README.md
## master branch content
$ git add .
$ git commit -m "add master branch content"
[master 1cfa0aa] add master branch content
 1 file changed, 1 insertion(+), 1 deletion(-)
$ git push origin master
Counting objects: 3, done.
Writing objects: 100% (3/3), 271 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/reng99/learngit.git
   d2f936f..1cfa0aa  master -> master
$ git branch dev
$ git checkout dev
Switched to branch 'dev'
$ cat README.md
## master branch content

### dev branch content
$ git add .
$ git commit -m "add dev branch content"
[dev 80faf6d] add dev branch content
 1 file changed, 2 insertions(+)
$ git checkout master
Switched to branch 'master'
Your branch is up-to-date with 'origin/master'.
$ cat README.md
## master content

### new master branch content
$ git add .
$ git commit -m "change master content"
[master ec18715] change master content
 1 file changed, 3 insertions(+), 1 deletion(-)
$ git merge dev
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

`README.md`文件中冲突内容--

```bash
<<<<<<< HEAD (当前更改)
## master content

### new master branch content
=======
## master branch content

### dev branch content
>>>>>>> dev (传入更改)
```

手动修改了`README.md`文件中冲突的内容--

```bash
## master branch content
### new master branch content
### dev branch content
```

 然后命令行执行--
 
 ```bash
$ git add .
$ git commit -m "fix confict content"
[master dd848b4] fix confict content
$ git log --graph
*   commit 980788b7690d8bcf14610072fc072460bee7e9f1
|\  Merge: c49d09e 2929dca
| | Author: reng99 <1837895991@qq.com>
| | Date:   Thu Dec 21 11:14:10 2017 +0800
| | 
| |     fix confict content
| |   
| * commit 2929dca91ef8f493adba7744cdad19656538334f
| | Author: reng99 <1837895991@qq.com>
| | Date:   Thu Dec 21 11:11:49 2017 +0800
| | 
| |     add dev branch content
| |   
* | commit c49d09e33e7098d67b59c845d18e9c6f8a8f4fea
|/  Author: reng99 <1837895991@qq.com>
|   Date:   Thu Dec 21 11:12:50 2017 +0800
|   
|       change master content
|  
* commit b07f0be8280e4e437cccf2a3f8fac6beef03ff41
| Author: reng99 <1837895991@qq.com>
| Date:   Thu Dec 21 11:10:51 2017 +0800
| 
:
 ```
 
上面操作过程是，我先从远程库中克隆`learngit`仓库到本地，目前的本地`learngit`的分支只有`master`分支，然后我在`master`分支的`README.md`中添加相关的文字(见代码)，接着把它推送到远程库。然后创建并切换`dev`分支，在`README.md`文件中添加新内容(见代码)，接着将它提交到`GIT仓库`。又切换到`master`分支，修改`README.md`到内容(见代码)，提交到`GIT仓库后`开始执行`merge`命令合并`dev`分支的内容。此时，产生了冲突，这就需要手动将冲突的内容解决，重新`commit`到`GIT仓库`，最后你就可以提交到远程库了(这步我没有演示，也就是git push origin master一行命令行的事情)。最后我还使用`git log ----graph`打印出整个分支合并图(从下往上看)，方便查看。⚠️ 此时退出`git log --graph`是书写英文状态按键盘的`q`键。

说这么多，目的只有一个 --> 产生冲突后，需要手动调整😊

### 分支管理策略

先放上一张分支管理策略图，然后再慢慢讲解相关的内容...

![tools/git/manager_branch_tactics](http://omu538iq8.bkt.clouddn.com/tools/git/manager_branch_tactics.png)

在分支管理中，我们不断的新建分支，开发，合并分支，删除分支的操作。这里需要注意合并分子的操作，之前我们进行分支的时候是直接将`dev`开发的分支使用`git merge dev`进行合并，这样有个缺点：我们看不出分支信息。因为在默认情况下，合并分支的时候，`GIT`是使用了`Fast Foward`的模式，在这种模式下，删除分支后，会丢掉分支的信息。下面我重新克隆下我远程`learngit`仓库，然后创建并更改`dev`分支的信息，使用默认的模式进行合并。

```bash
$ git branch
* master
$ git branch dev
$ git checkout dev
Switched to branch 'dev'
$ cat README.md
## master branch content
### new master branch content
### dev branch content
### new dev branch content
$ git add .
$ git commit -m "add new dev contentt"
[dev 750e1f1] add new dev content
 1 file changed, 1 insertion(+)
$ git checkout master
Switched to branch 'master'
Your branch is up-to-date with 'origin/master'.
$ git merge dev
Updating 980788b..750e1f1
Fast-forward
 README.md | 1 +
 1 file changed, 1 insertion(+)
$ git log --graph
* commit 750e1f17854872eed4d6cff8315e404079ecb18f
| Author: reng99 <1837895991@qq.com>
| Date:   Fri Dec 22 10:05:36 2017 +0800
| 
|     add new dev content
|    
*   commit 980788b7690d8bcf14610072fc072460bee7e9f1
...
```

上面的合并就是将master分支上面的`HEAD`指向`dev`指针，如下：

```bash
# 记录是从上往下
- before merge
	master
	* (begin)
	|
	|
	*
	\
	 \
	  *
	  |
	  |
	  *	 (end)
	 dev
	 
- after merge
	master
	* (begin)
	|
	|
	*
	|
	|
	* 
	|
	|
	* (end)
```

为了保留分支的情况，保证版本演进的清晰，我们就得使用普通模式合并，也就是在`Fast Foward`的模式基础上加上`--no-ff`参数，即`git merge --no-ff branchName`，不过我们一般加上你合并的相关信息，即`git merge --no-ff -m "your msg here" banchName`。现在更改`dev`分支的内容，再进行合并。

```bash
$ git checkout dev
Switched to branch 'dev'
$ cat README.md
## master branch content
### new master branch content
### dev branch content
### new dev branch content
### merge with no-ff
$ git add .
$ git commit -m "add no-ff mode content"
[dev 80b628c] add no-ff mode content
 1 file changed, 2 insertions(+), 1 deletion(-)
$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
$ git merge dev --no-ff -m "merge with no-ff" dev
Merge made by the 'recursive' strategy.
 README.md | 3 ++-
 1 file changed, 2 insertions(+), 1 deletion(-)
$ git log --graph
*   commit 98746d93a9b64ea02b8ff1c7f0fa5e915405c0e6
|\  Merge: 750e1f1 80b628c
| | Author: reng99 <1837895991@qq.com>
| | Date:   Fri Dec 22 14:39:32 2017 +0800
| | 
| |     merge with no-ff
| |   
| * commit 80b628c334618711b77da81fa805ffc246a2cf7d
|/  Author: reng99 <1837895991@qq.com>
|   Date:   Fri Dec 22 14:38:17 2017 +0800
|   
|       add no-ff mode content
|  
* commit 750e1f17854872eed4d6cff8315e404079ecb18f
...
```

使用`--no-ff`参数的普通模式合并，会执行正常合并，在`master`主分支上面会生成一个新的节点，如下（**我上面的分支管理策略图里面的合并就是使用了普通的模式**）：

```bash
# 记录是从上往下
- --no-ff合并
	master
	* (before)
	|
	|
	*
	|\ 
	| \
	|  *dev
	|  |
	|  |
	|  *
	| /
	|/
	* (after) 
```

我们在开发中，分支管理可以分成master主分支、dev开发分支、feature功能分支、release预发布分支、hotfixes修补bug分支。其中功能分支、预发布分支和修补bug分支可以归为`临时分支`。`临时分支`在进行分支的合并之后就可以被删除了。下面就一一讲解自己眼中的各种分支。

#### 主分支master

主分支是在你初始化仓库的时候(git init)，自动生成的一个master分支，删除不了的哦（演示待会给）。主分支是有且仅有一个，也是发布上线的分支，团队合作的最终代码都会在master主分支上面体现出来。也许你也注意到了分支管理策略图里面的主分支会被打上`TAG`的标签，这是为了方便到某个时间段对版本的查找，标签tag的学习总结后面给出。

```bash
# 记录是从上往下
	master
	|
	|
	*(tag 1.0)
	|
	|
	*(tag 1.1)
	|
	|
	*(tag 1.2)
```

下面代码演示下不能放删除master的情况:

```bash
$ cd learngit
$ git branch
  dev
* master
$ git branch -D master
error: Cannot delete branch 'master' checked out at '/Users/reng/desktop/learngit'
```
#### 开发分支develop

在开发的过程中，项目合作者应该保持自己本地有一个开发环境的分支，在进行分支开发之前，需要进行`git pull`拉取`master`主分支的最新内容，或者通过其他的方法。在获取到最新的内容之后才可以进行本地的新功能的开发。在开发完成后将内容`merge`到主分支之后，不用将`dev`分支删除，因为你开发的就是在这里进行，何必删除后再新建一个开发环境的分支呢。

接着上面的情况，我目前已经拥有了`dev`开发分支:

```bash
$ cd learngit
$ git branch
  dev
* master
```

#### 功能（特性）分支feature 

一个软件就是一个个功能叠加起来的，在软件的开发中，我们总不能在主分支开发，将主分支搞乱吧。当然，你可以在dev分支中开发，一般新建功能分支来开发，然后功能开发完再合并到dev分支，之后删除功能分支。需要的时候就可以将`dev`开发分支合并到`master`主分支，这样就随时保证`dev`分支功能的完整性。

下面演示功能分支`user`开发（随便写点内容）的合并（这里也演示了合并到master主分支，跳过了release分支的测试），删除。

```bash
$ git checkout dev
Switched to branch 'dev'
$ git branch user
$ git branch
* dev
  master
  user
$ git checkout user
Switched to branch 'user'
$ cat README.md
## master branch content
### new master branch content
### dev branch content
### new dev branch content
### merge with no-ff
### function user
$ git add .
$ git commit -m "function user was acheive"
[user 26beda3] function user was acheive
 1 file changed, 2 insertions(+), 1 deletion(-)
$ git checkout dev
Switched to branch 'dev'
$ git merge --no-ff -m "merge user feature" user
Merge made by the 'recursive' strategy.
 README.md | 3 ++-
 1 file changed, 2 insertions(+), 1 deletion(-)
$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 3 commits.
  (use "git push" to publish your local commits)
$ git merge --no-ff -m "merge dev branch" dev
Merge made by the 'recursive' strategy.
 README.md | 3 ++-
 1 file changed, 2 insertions(+), 1 deletion(-)
$ git log --graph
*   commit f15a1e9012635fc21e944ab76c4cd4bbd539f82f
|\  Merge: 98746d9 0ca83c6
| | Author: reng99 <1837895991@qq.com>
| | Date:   Fri Dec 22 16:35:43 2017 +0800
| | 
| |     merge dev branch
| |     
| *   commit 0ca83c654df64724743a966f5f0989477e504cbc
| |\  Merge: 80b628c 26beda3
| | | Author: reng99 <1837895991@qq.com>
| | | Date:   Fri Dec 22 16:33:27 2017 +0800
| | | 
| | |     merge user feature
| | |    
| | * commit 26beda3b8246e047f10ac0461ca11d1a6f132819
| |/  Author: reng99 <1837895991@qq.com>
| |   Date:   Fri Dec 22 16:31:41 2017 +0800
| |   
| |       function user was acheive
| |     
* |   commit 98746d93a9b64ea02b8ff1c7f0fa5e915405c0e6
|\ \  Merge: 750e1f1 80b628c
| |/  Author: reng99 <1837895991@qq.com>
:
$ git branch -D user
Deleted branch user (was 26beda3).
$ git branch
  dev
* master
```

#### 预发布分支release

在进行一系列的功能的开发和合并后，在满足迭代目标的时候，就可以打包送测了。这里就需要一个预发布分支release。预发布分支是指在发布正式版本之前（ 即合并到master分支之前，可查看上面分支管理策略图），需要一个有预发布的版本（可以理解为灰度环境）进行测试。

预发布环境是从`dev`分支上面分出来的，预发布结束之后，必须合并到`dev`和`master`分支上面。这里我就不演示了，跟功能分支差不多，就是合并的时候要合并到`dev`和`master`上，这时候`dev`分支和`master`的同步的代码，就不需要将`dev`分支合并到`master`了。最后将预发布分支删除掉。

#### 修复bug分支 bug/hotfixes

在写代码的过程中，由于种种原因 -> 比如功能考虑不周全，版本上线时间有限，产品突然改需求等，我们写的代码就出现一些或大或小的bug或者需要紧急修复。那么我们就可以使用bug分支（其实就是新建一个分支处理bug而已啦，命名随意起的），然后在这个分支上处理编码出现的问题。我在`分支管理策略图`上面已经展示了一种出现bug的情况 -> 就是在测试发布版本看似没问题的情况下，将`release`版本整合到`master`和`dev`中，这时候火眼精金发现了遗留的一个bug，然后新建一个`bug分支`处理，再合并到`master`和`dev`中，之后将`bug分支`移除啦。

在开发的过程中，无论咋样都是这样 : 新建bug分支 -> 把分支合并 -> 删除分支，这里的demo就不演示了，可以参考上面的`功能（特性）分支feature`。

这里需要注意⚠️的一点，当在开发的过程中，开发到一定的程度，需要停下来需改紧急的bug，那么需要停下手头的工作需改bug啦。这时候需要将工作现场储藏(stash功能)起来，等以后回复现场了后接着工作。现在我在原先的`gitlearn`仓库中`README.md`文件文末添加`### modify content`内容来进行演示。

```bash
$ cd desktop
$ cd learngit
$ cat README.md
## master branch content
### new master branch content
### dev branch content
### new dev branch content
### merge with no-ff
$ git status
On branch dev
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
$ git stash
Saved working directory and index state WIP on dev: 80b628c add no-ff mode content
HEAD is now at 80b628c add no-ff mode content
$ git status
On branch dev
nothing to commit, working tree clean
```

然后过段时间(这里省略修改的演示)，代码已经修改好合并后，需要回到最新的内容区域进行工作，这就需要还原最新的内容了，demo如下：

```bash
$ cd learngit
$ git stash list
stash@{0}: WIP on dev: 80b628c add no-ff mode content
$ git stash pop
On branch dev
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{0} (9e85bcc8435ae38c17db59ddc3cd8401af404827)
$ git stash list

```

⚠️ `git stash`不仅可以隐藏工作区的内容，也可以隐藏暂存区的内容。`git stash list`是查看隐藏的列表。`git stash pop`是将隐藏的内容恢复并删除，`git stash pop`相当于`git stash apply && git stash drop`，这里的`git stash apply`是恢复隐藏内容，`git stash drop`是删除隐藏内容。

### 多人协作

简单谈下自己git协作的过程吧。在负责人将搭建好的仓库上传到远程的仓库后（一般是包含了master默认的分支和dev分支），自己将远程仓库克隆到本地，然后在本地的仓库上新建一个`dev分支`，将远程的dev分支重新拉取下`git pull origin dev`，开发完成后就可以提交自己的代码到`远程的dev分支了`，如果提交之前或者之后需要修改bug或者添加新的需求的话，需要新建一个相关的分支并完成开发，将他们合并到`本地dev`分支后上传到`远程dev`分支。如果新建的`远程仓库中只有master分支`，我是这样处理的：依然要在本地新建一个`dev`分支，然后在完成特定版本的开发后，将分支合并到`本地master分支`然后再推送到`远程master`分支，本地的`dev分支`保留哦。我自己比较偏向于第一种情况。

## 标签管理

发布一个版本前，为了唯一确定时刻的版本，我们通常在版本库中打一个标签(tag)，方便在发布版本以后，可以在某个时刻将某个历史的版本提取出来（因为标签tag也是版本库的一个快照）。

### 创建标签

创建标签是默认在你切换的分支最新提交处创建的。我这里在本地桌面的`learngit仓库`的`master分支`上打一个`v1.0标签`。

```bash
$ cd desktop/learngit
$ git branch
* dev
  master
$ git checkout master
Switched to branch 'master'
Your branch is up-to-date with 'origin/master'.
$ git tag
$ git tag v1.0
$ git tag
v1.0
```

当然，你可以在非新commit的地方进行标签。这就需要你查找到需要打标签处的commit的id，然后执行`git tag tagName commitId`。这里我随意找master分支中的commit id进行标签`v0.9`的标签创建。

```bash
$ git log --pretty=oneline --abbrev-commit
f15a1e9 merge dev branch
0ca83c6 merge user feature
26beda3 function user was acheive
98746d9 merge with no-ff
...
```

现在在commit id为 `98746d9`处打标签。

```bash
$ git tag v0.9 98746d9
$ git tag
v0.9
v1.0
```
### 操作标签

在上面创建标签，我们已经有了标签`v0.9 v1.0`。有时候我们标签打错了，需要进行删除，那么就得更改啦，运用`git tag -d tagName`

```bash
$ git tag -d v0.9
Deleted tag 'v0.9' (was 98746d9)
$ git tag
v1.0
$ git tag v0.8 80b628c -m "version 0.8"
$ git tag
v0.8
v1.0
$ git show v0.8
$ git show v0.8
tag v0.8
Tagger: reng99 <1837895991@qq.com>
tag v0.8
Tagger: reng99 <1837895991@qq.com>
Date:   Wed Dec 27 16:07:46 2017 +0800

version 0.8
```

在上面的演示中，我删除了v0.9，然后在创建v0.8的时候追加了打标签的信息，之后使用`git show tagName`查看签名信息。

我们还可以进行分支切换标签，类似于分支的切换，我这里打的两个标签的内容是不同的，我可以通过观察内容的改表来得知时候成功切换标签了。

```bash
$ git tag
v0.8
v1.0
$ git checkout v1.0
HEAD is now at f15a1e9... merge dev branch
$ cat README.md
## master branch content
### new master branch content
### dev branch content
### new dev branch content
### merge with no-ff
### function user
$ git checkout v0.8
Previous HEAD position was f15a1e9... merge dev branch
HEAD is now at 80b628c... add no-ff mode content
$ cat README.md
## master branch content
### new master branch content
### dev branch content
### new dev branch content
### merge with no-ff
```

在确认好标签后，就可以像远程推送标签了，我这里推送`v1.0`。

```bash
$ git push origin v1.0
Total 0 (delta 0), reused 0 (delta 0)
To https://github.com/reng99/learngit.git
 * [new tag]         v1.0 -> v1.0
```

上面是使用`git push origin tagName`推送特定的`tag`到远程库，但是我们能不能推送全部的tag呢？答案是肯定的，看者可以通过`git push origin --tags`进行推送。有时候，我们推送了`tag标签到远程库中了`，现在想删除掉怎么办？这个就略微麻烦点，我们不能像上面提到的删除本地库的标签那样，通过`git tag -d tagName`那样，而是通过`git push origin :refs/tags/tagName`，这里不演示，如果看者感兴趣可以自己来把弄一下哦。

## 参考内容

[廖雪峰官方网站--Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

[易百教程--Git教程](http://wwwman.yiibai.com/git/)

[git官网](https://git-scm.com/)

[分支管理模型图](http://s3.51cto.com/wyfs02/M02/12/44/wKiom1MA0v-horoSAAS4v41ef_U068.jpg)

[Git分支管理策略 - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2012/07/git.html)

<p style="color:red;text-align:center;">完结 @~@</p>