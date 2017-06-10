---

layout: post

title:  "Git-Github"

date:   2017-06-10 10:06:00

categories: Git

---

# Git

## Git介绍

Git是一个**分布式版本控制系统**，最初目的是为更好地管理Linux内核开发而设计  
Git和其他**集中式版本控制系统**不一样，不需要服务器端软件，就可以运作版本控制，**速度快得多**，每个人的电脑上都是一个完整的版本库，直接在本机上获取数据，不必连接到主机端获取数据  
**Git还有强大的分支合并管理功能**  

<br />

## Git工作过程
**工作区（Working Directory）**：就是你在电脑里能看到的目录  
**版本库（Repository）**：工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库  
Git的版本库里存了很多东西，其中最重要的就是称为**stage（或者叫index）的暂存区**，还有Git为我们自动创建的第一个**分支**`master`，以及指向master的一个**指针**叫`HEAD`  

![](https://aswz.github.io/assets/img/Git/Git-Github/Git工作流程.png)

将文件往Git版本库添加时分为两步:  
**第一步是用**`git add`把文件**添加**进去，实际上就是把**文件修改添加到暂存区**  
**第二步是用**`git commit`**提交更改**，实际上就是把**暂存区的所有内容提交到当前分支**  
**每次的修改必须先**`add`**到暂存区再**`commit`**提交到分支，每次提交只会将在暂存区的修改提交到分支**  

<br />

## 版本回退、撤销删除、删除文件

### 版本回退

在Git中可以用`git log`查看提交历史纪录，会看到`commit id`是一大串十六进制的数字  
在Git中，用`HEAD`表示当前版本，`HEAD^`是上一版本，`HEAD^^`是上上一版本，也可以写成`HEAD~2`  
版本回退可以使用`git reset`命令：
```
$ git reset --hard HEAD^
HEAD is now at c2e5a67 add justtest.txt
```

<br />

**注意回到过去版本后历史纪录中未来版本会消失，如果要回到未来版本可以用版本号**`commit id`  
版本号不用写全，只用前几位，Git会自动搜索，但也不能太短，导致查找多个版本号  
```
$ git reset --hard 0e7dd65
HEAD is now at 0e7dd65 add test.txt
```

### 撤销删除

可以用`git checkout -- file`丢弃工作区的修改  
```
$ git checkout -- readme.txt
```
- **如果**`readme.txt`**修改后没有放到暂存区，现在，撤销修改就回到和版本库一模一样的状态**
- **如果**`readme.txt`**已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态**

**总之，就是让这个文件回到最近一次git commit或git add时的状态**

也可以用`git reset HEAD file`命令可以把暂存区的修改撤销掉（unstage），重新放回工作区
```
$ git reset HEAD readme.txt
Unstaged changes after reset:
M       readme.txt
```
`git reset`**命令既可以回退版本，也可以把暂存区的修改回退到工作区**
回退到工作区后再丢弃工作区修改就彻底撤销修改了  
如果提交到版本库就用版本回退，不过前提是没有推送到远程库  

<br />

### 删除文件

要删除版本库文件，先将工作区对应文件删除，再用`git rm file`删除，并用`git commit`提交  
```
$ git rm test.txt
rm 'test.txt'
$ git commit -m "remove test.txt"
[master 69cd8cc] remove test.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 test.txt
```

<br />

如果误删文件也可以用`git checkout`命令恢复到**最新版本**
```
$ git checkout -- test.txt
```
`git checkout`**其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”**

<br />

## 分支管理

在新建版本库的时候都会有一条默认的分支`master`，可以在原分支新建一条互不干扰的分支，也可以进行分支合并  
详细的分支和HEAD指针图解可以参阅[分支图解(廖雪峰老师)](http://http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375840038939c291467cc7c747b1810aab2fb8863508000)  

### 创建、合并、删除分支


