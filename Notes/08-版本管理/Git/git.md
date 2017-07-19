# Git 教程
https://www.liaoxuefeng.com/

markdown 语法：

http://www.appinn.com/markdown/
***

## Git安装
### Linux下安装

首先，你可以试着输入git，看看系统有没有安装Git：

```
$ git
The program 'git' is currently not installed. You can install it by typing:
sudo apt-get install git
$ yum install git-core -y
```
### 在Windows上安装Git

[github下载](https://git-for-windows.github.io/ "Git")

安装完成后，在开始菜单里找到“Git”->“Git Bash”

安装完成后，还需要最后一步设置，在命令行输入：
```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

## 创建版本库

什么是版本库呢？版本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

所以，创建一个版本库非常简单，首先，选择一个合适的地方，创建一个空目录：
```
HUI@HUI-THINK220 MINGW64 ~
$ mkdir /d/git/
$ cd /d/git/
$ pwd
/d/git
```
`pwd`命令用于显示当前目录。在我的Mac上，这个仓库位于`/Users/michael/learngit`。

如果你使用Windows系统，为了避免遇到各种莫名其妙的问题，请确保目录名（包括父目录）不包含中文。

第二步，通过git init命令把这个目录变成Git可以管理的仓库：
```
$ git init
Initialized empty Git repository in D:/git/.git/
$ ls -ah
./  ../  .git/
```
瞬间Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），细心的读者可以发现当前目录下多了一个`.git`的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。

如果你没有看到`.git`目录，那是因为这个目录默认是隐藏的，用`ls -ah`命令就可以看见。

### 把文件添加到版本库
<<<<<<< HEAD


![Alt text](./img/md.jspg "Optional title")

test
=======
第一步，用命令git add告诉Git，把文件添加到仓库：

`D:\MyDoc\GitHub\Notes\08-版本管理\Git [master +1 ~0 -0 !]> git add .\git.md`

第二步，用命令git commit告诉Git，把文件提交到仓库：
```
D:\MyDoc\GitHub\Notes\08-版本管理\Git [master +1 ~0 -0 | +3 ~0 -0 !]> git commit -m "git file git.md"
[master (root-commit) d1ee4a3] git file git.md
 1 file changed, 56 insertions(+)
 create mode 100644 "Notes/08-\347\211\210\346\234\254\347\256\241\347\220\206/Git/git.md"
D:\MyDoc\GitHub\Notes\08-版本管理\Git [master +3 ~0 -0 !]>
```
### 小结

> 现在总结一下今天学的两点内容：

> 1 初始化一个Git仓库，使用git init命令。

> 2 添加文件到Git仓库，分两步：

> * 第一步，使用命令git add <file>，注意，可反复多次使用，添加多个文件；

> * 第二步，使用命令git commit，完成。

***
## 时光穿梭机
之前田间了git.md文件，运行`git status`
```
D:\MyDoc\GitHub\Notes\08-版本管理\Git [master +3 ~1 -0 !]> git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   git.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        img/
        "../../30-\346\226\207\346\234\254\347\274\226\350\276\221/"
        "../../\350\277\220\347\273\264\345\267\245\345\205\267\344\275\223\347\263\273.md"

no changes added to commit (use "git add" and/or "git commit -a")
D:\MyDoc\GitHub\Notes\08-版本管理\Git [master +3 ~1 -0 !]>
```

* `git status`确认仓库状态
* `git diff git.md`对比修改前后

### 小结

> 要随时掌握工作区的状态，使用git status命令。

> 如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。


## 版本回退

`git log`令显示从最近到最远的提交日志，如果嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数：

```
D:\MyDoc\GitHub\Notes\08-版本管理\Git [master +3 ~0 -0 !]> git log
commit 7ba4071416ba21e4d7e3fd578278bcf07ed0c280
Author: wanghui <wanghui3w@139.com>
Date:   Tue Jul 18 11:03:24 2017 +0800

    4

commit 4ce15acc90b56f162328288bc348f31d6a0f672e
Author: wanghui <wanghui3w@139.com>
Date:   Tue Jul 18 10:47:34 2017 +0800

    第三次

commit cf3fa8e4f566225e1c804daae94d4d68dc438d1b
Author: wanghui <wanghui3w@139.com>
Date:   Tue Jul 18 10:41:03 2017 +0800

    第二次commit

commit 01c42de30c64591323d07092db35a13496be17d2
Author: wanghui <wanghui3w@139.com>
Date:   Tue Jul 18 10:39:00 2017 +0800

    git test file get-test.txt

commit d1ee4a38ee3a3d785e2ff1c803d9fc9303a2e7a8
Author: wanghui <wanghui3w@139.com>
Date:   Tue Jul 18 10:14:57 2017 +0800

    git file git.md
D:\MyDoc\GitHub\Notes\08-版本管理\Git [master +3 ~0 -0 !]>
```

```
D:\MyDoc\GitHub\Notes\08-版本管理\Git [master +3 ~0 -0 !]> git log --pretty=oneline
7ba4071416ba21e4d7e3fd578278bcf07ed0c280 4
4ce15acc90b56f162328288bc348f31d6a0f672e 第三次
cf3fa8e4f566225e1c804daae94d4d68dc438d1b 第二次commit
01c42de30c64591323d07092db35a13496be17d2 git test file get-test.txt
d1ee4a38ee3a3d785e2ff1c803d9fc9303a2e7a8 git file git.md
D:\MyDoc\GitHub\Notes\08-版本管理\Git [master +3 ~1 -0 !]>
```


首先，Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，也就是最新的提交7ba4071416ba21e4d7e3fd578278bcf07ed0c280（注意提交ID每个人不一样），上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。现在，我们要把当前版回退到上一个版本，就可以使用`git reset`命令：

```
$ git reset --hard HEAD^
```

只要上面的命令行窗口还没有被关掉，你就可以顺着往上找啊找啊，找到那个append GPL的commit id是3628164...，于是就可以指定回到未来的某个版本：

```
git reset --hard 3628164
```
Git的版本回退速度非常快，因为Git在内部有个指向当前版本的HEAD指针，当你回退版本的时候，Git仅仅是把HEAD从指向上个版本标签，然后顺便把工作区的文件更新了。所以你让HEAD指向哪个版本号，你就把当前版本定位在哪。

Git提供了一个命令`git reflog`用来记录你的每一次命令：
```
D:\MyDoc\GitHub\Notes\08-版本管理\Git [master +3 ~1 -0 !]> git reflog
7ba4071 HEAD@{0}: commit: 4
4ce15ac HEAD@{1}: commit: 第三次
cf3fa8e HEAD@{2}: commit: 第二次commit
01c42de HEAD@{3}: commit: git test file get-test.txt
d1ee4a3 HEAD@{4}: commit (initial): git file git.md
D:\MyDoc\GitHub\Notes\08-版本管理\Git [master +3 ~1 -0 !]>
```

### 小结

现在总结一下：

> HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。

> * 穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。

> * 要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。


## 工作区和暂存区

**工作区（Working Directory）：** 就是你在电脑里能看到的目录。

**暂存区（Stage）：** 通过`git add` 后的文件，暂时放在暂存区

**版本库（Repository）：** 工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。

前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

`git add`命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行`git commit`就可以一次性把暂存区的所有修改提交到分支。一旦提交后，如果你又没有对工作区做任何修改，那么工作区就是“干净”的，暂存区是空的。

### 小结

> 暂存区是Git非常重要的概念，弄明白了暂存区，就弄明白了Git的很多操作到底干了什么。

## 管理修改
### 小结

> 现在，你又理解了Git是如何跟踪修改的，每次修改，如果不add到暂存区，那就不会加入到commit中。

## 撤销修改

**回到上一个修改：** `git reset HEAD readme.txt`

**清空缓存区：** `git checkout -- readme.txt`（注意两个减号）,可以丢弃工作区的修改。

命令`git checkout -- readme.txt`意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：

* 一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

* 一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次git commit或git add时的状态。

### 小结

> * 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

> * 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD file`，就回到了场景1，第二步按场景1操作。

> * 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

## 删除文件

### 第一种：

1. 操作系统下直接删除`rm test.txt`，如果需要找回`git checkout -- test.txt`
2. `git rm test.txt`
3. `git commit -m "delete test"`

### 第二种
1. `git rm test.txt`
2. `git commit -m "delete test"`

### 小结
> `git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。命令`git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。

***

## 远程仓库

注册GitHub账号。由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：

#### 第1步：创建SSH Key

```
$ ssh-keygen -t rsa -C "wanghui3w@139.com"
```
#### 第2步：登陆GitHub

打开“Account settings”，“SSH Keys”页面，然后点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容。

git出现问题，参考了：

http://blog.csdn.net/laner415/article/details/52966890

https://my.oschina.net/uniquejava/blog/165140

```
问题（Non-fast-forward）的出现原因在于：git仓库中已经有一部分代码，所以它不允许你直接把你的代码覆盖上去。于是你有2个选择方式：

1，强推，即利用强覆盖方式用你本地的代码替代git仓库内的内容

git push -f

2，先把git的东西fetch到你本地然后merge后再push

$ git fetch

$ git merge

这2句命令等价于

$ git pull
```
### 添加远程仓库

登陆GitHub，然后，在右上角找到“Create a new repo”按钮，创建一个新的仓库，然后执行命令，将本地库与远程库关联，并执行推送：
```
$ git remote add origin git@github.com:michaelliao/learngit.git
$ git push -u origin master
```

#### 小结

要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`；

关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！

### 从远程库克隆

首先，登陆GitHub，创建一个新的仓库，勾选Initialize this repository with a README，这样GitHub会自动为我们创建一个README.md文件。
```
$ git clone git@github.com:michaelliao/gitskills.git
Cloning into 'gitskills'...
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (3/3), done.

$ cd gitskills
$ ls
README.md
```

如果有多个人协作开发，那么每个人各自从远程克隆一份就可以了。

你也许还注意到，GitHub给出的地址不止一个，还可以用`https://github.com/michaelliao/gitskills.git`这样的地址。实际上，Git支持多种协议，默认的git://使用ssh，但也可以使用https等其他协议。

使用https除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用ssh协议而只能用https。

***

## 分支管理

### 创建与合并分支

首先，我们创建dev分支，然后切换到dev分支：

```
$ git checkout -b dev
Switched to a new branch 'dev'
```

`git checkout`命令加上-b参数表示创建并切换，相当于以下两条命令：
```
$ git branch dev
$ git checkout dev
Switched to branch 'dev'
```
然后，用`git branch`命令查看当前分支：
```
$ git branch
* dev
  master
```

然后提交：
```
$ git add readme.txt
$ git commit -m "branch test"
[dev fec145a] branch test
 1 file changed, 1 insertion(+)
```
现在，dev分支的工作完成，我们就可以切换回master分支：
```
$ git checkout master
Switched to branch 'master'
```
把dev分支的工作成果合并到master分支上：
```
$ git merge dev
Updating d17efd8..fec145a
Fast-forward
 readme.txt |    1 +
 1 file changed, 1 insertion(+)
```
合并完成后，就可以放心地删除dev分支了：
```
$ git branch -d dev
Deleted branch dev (was fec145a).
```

#### 小结

Git鼓励大量使用分支：

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`

创建+切换分支：`git checkout -b <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`

### 解决冲突
准备新的feature1分支，继续我们的新分支开发，在feature1分支上修改并提交，切换到master分支修改并提交，这种情况下，Git无法执行“快速合并”，只能试图把各自的修改合并起来，但这种合并就可能会有冲突：
```
$ git merge feature1
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.
```
Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容，我们修改如下后保存再提交：
```
$ git add readme.txt
$ git commit -m "conflict fixed"
[master 59bc1cb] conflict fixed
```
用带参数的git log也可以看到分支的合并情况：
```
$ git log --graph --pretty=oneline --abbrev-commit
*   59bc1cb conflict fixed
|\
| * 75a857c AND simple
* | 400b400 & simple
|/
* fec145a branch test
```

### 分支管理策略

通常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。
![branch](./img/branch.png)
