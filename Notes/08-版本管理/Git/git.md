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

现在总结一下今天学的两点内容：

1 初始化一个Git仓库，使用git init命令。

2 添加文件到Git仓库，分两步：

* 第一步，使用命令git add <file>，注意，可反复多次使用，添加多个文件；

* 第二步，使用命令git commit，完成。
***
# 时光穿梭机
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

![Alt text](./img/md.jspg "Optional title")
