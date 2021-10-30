# git学习

git是分布式版本控制系统。

## 分布式和集中式的区别

1. 集中式版本控制系统，版本库是集中存放在中央服务器的，而干活的时候，用的都是自己的电脑，所以要先从中央服务器取得最新的版本，然后开始干活，干完活了，再把自己的活推送给中央服务器。中央服务器就好比是一个图书馆，你要改一本书，必须先从图书馆借出来，然后回到家自己改，改完了，再放回图书馆。
2. 集中式版本控制系统最大的毛病就是必须联网才能工作，如果在局域网内还好，带宽够大，速度够快，可如果在互联网上，遇到网速慢的话，可能提交一个10M的文件就需要5分钟，这还不得把人给憋死啊。
3. 首先，分布式版本控制系统根本没有“中央服务器”，每个人的电脑上都是一个完整的版本库，这样，你工作的时候，就不需要联网了，因为版本库就在你自己的电脑上。既然每个人电脑上都有一个完整的版本库，那多个人如何协作呢？比方说你在自己电脑上改了文件A，你的同事也在他的电脑上改了文件A，这时，你们俩之间只需把各自的修改推送给对方，就可以互相看到对方的修改了。
4. 和集中式版本控制系统相比，分布式版本控制系统的安全性要高很多，因为每个人电脑里都有完整的版本库，某一个人的电脑坏掉了不要紧，随便从其他人那里复制一个就可以了。而集中式版本控制系统的中央服务器要是出了问题，所有人都没法干活了。
5. 在实际使用分布式版本控制系统的时候，其实很少在两人之间的电脑上推送版本库的修改，因为可能你们俩不在一个局域网内，两台电脑互相访问不了，也可能今天你的同事病了，他的电脑压根没有开机。**因此，分布式版本控制系统通常也有一台充当“中央服务器”的电脑，但这个服务器的作用仅仅是用来方便“交换”大家的修改，没有它大家也一样干活，只是交换修改不方便而已。**



## 在本机创建版本库

版本库又名仓库，英文名**repository**，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

所以，创建一个版本库非常简单，首先，选择一个合适的地方，创建一个空目录：

```
$ mkdir mygit  // 创建一个空目录
$ cd mygit	// 进入该目录
$ git init // 第二步，通过`git init`命令把这个目录变成Git可以管理的仓库：
```

![image-20210924093320720](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210924093320720.png)

`git init` 后当前目录下多了一个`.git`的目录，这个目录是Git来跟踪管理版本库的。



## 使用版本库

在创建了仓库的文件夹內新增一些内容，这里我新建了一个txt文件。

接下来将这个文件推送到仓库里需要两步：

1. `git add filename` : 将文件添加到仓库
2. `git commit -m "更新描述"`  : 将文件提交到仓库，`-m`后面输入的是本次提交的说明

为什么Git添加文件需要`add`，`commit`一共两步呢？因为`commit`可以一次提交很多文件，所以你可以多次`add`不同的文件，比如：

```
$ git add file1.txt
$ git add file2.txt file3.txt
$ git commit -m "add 3 files."
```

这时候文件已经被提交到仓库里进行管理了。

当我们修改文件时：

1. `git status` 命令可以捕获到是哪个文件被修改了，但无法查看具体修改了什么内容

![image-20210924102237894](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210924102237894.png)

​			第一个箭头说明了这个修改过的文件没有被提交，第二个箭头说明了哪个文件被修改了

2. `git diff` 可以查看具体的修改内容

![image-20210924102519997](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210924102519997.png)

​			箭头标注的地方就是具体的修改内容

3. 提交修改后的文件的步骤跟提交新文件的步骤一样，都是先`add`再`commit`

   

![image-20210924103014732](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210924103014732.png)

4. `commit`后再用`git status` 查看这时候的状态

   ​	

![image-20210924103103443](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210924103103443.png)

​			显示目前仓库是干净的，即没有需要提交和修改的内容

![image-20210924145914753](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210924145914753.png)

## 版本回退

### `git reset`命令

`git reset`命令存在的问题： **回退后最新的版本就消失了**，当然，如果你记得住版本号的话，也可以通过指定版本号回退到最新版本

![image-20210924111622546](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210924111622546.png)

注意红色框起来的都是版本号，回退时以HEAD为当前版本，HEAD~1或者HEAD^代表回退到上个版本，在cmd中'^'是特殊符号，因此回退到上个版本执行的命令应该为`git reset --hard HEAD~1或者git reset --hard HEAD'^'`。



![image-20210924143708034](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210924143708034.png)

此时我通过`git reset --hard HEAD~1`回退到上个版本，此时的最新版本0844...的版本号已经不存在了，但是从上面的图我们可以让他重新跳转到0844版本

![image-20210924144010017](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210924144010017.png)

但是如果我们忘记了这个版本号或者没有保存截图，那么该如何回退呢？这时候就可以使用`git reflog`

![image-20210924144328600](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210924144328600.png)

`git reflog`可以记录我们每次改变版本号的命令以及对应的版本号，这时候我们就可以找到想要回退的版本



## 撤销修改

有两种情况：

1. 这个修改在工作区中，还没被add到暂存区staged
2. 这个修改已经被add到暂存区staged，此时工作区和暂存区都是修改后的文件



针对两种情况有三种撤销方法

从暂存区恢复工作区，只撤销工作区的修改，用暂存区的文件替换工作区的文件

`git resotre --worktree test.txt`

从master恢复暂存区 ，只撤销暂存区，用master的文件替换暂存区的文件

`git restore --staged test.txt`

从master同时恢复工作区和暂存区

`git restore --source=HEAD --staged --worktree test.txt`



## 误删文件

假如你将已经已经`add或者commit`的文件删除了，那么你也可以通过下面3种方法恢复文件

1. 从暂存区恢复工作区，只撤销工作区的修改，用暂存区的文件替换工作区的文件

`git resotre --worktree test.txt`

2. 从master恢复暂存区 ，只撤销暂存区，用master的文件替换暂存区的文件

`git restore --staged test.txt`

3. 从master同时恢复工作区和暂存区

`git restore --source=HEAD --staged --worktree test.txt`



## 添加远程库

实现：让本地仓库与github仓库连接起来，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作



1. 先在github上新建一个仓库Repository
2. 在本地仓库上`git remote add origin git@github.com:账户名/仓库名.git`，将本地仓库和github仓库连接起来，`origin`是远端仓库名
3. 把本地仓库的内容推送到github上 `git push -u origin main` ，由于远程库是空的，我们第一次推送`main`分支时，加上了`-u`参数，Git不但会把本地的`main`分支内容推送的远程新的`main`分支，还会把本地的`main`分支和远程的`main`分支关联起来，在以后的推送或者拉取时就可以简化命令。
4. 之后新增的修改先add->commit到本地仓库后再`git push origin main`



## 克隆远程库

`git clone git@github.com:Zzzccs/远程库名.git`



## 分支

分支在实际中有什么用呢？假设你准备开发一个新功能，但是需要两周才能完成，第一周你写了50%的代码，如果立刻提交，由于代码还没写完，不完整的代码库会导致别人不能干活了。如果等代码全部写完再一次提交，又存在丢失每天进度的巨大风险。

现在有了分支，就不用怕了。你创建了一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。

`HEAD`指向的就是当前分支。

一开始的时候，`master`分支是一条线，Git用`master`指向最新的提交，再用`HEAD`指向`master`，就能确定当前分支，以及当前分支的提交点：![image-20210924195259843](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210924195259843.png)



每次提交，`master`分支都会向前移动一步，这样，随着你不断提交，`master`分支的线也越来越长。

当我们创建新的分支，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上：![image-20210924195318644](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210924195318644.png)

你看，Git创建一个分支很快，因为除了增加一个`dev`指针，改改`HEAD`的指向，工作区的文件都没有任何变化！

不过，从现在开始，对工作区的修改和提交就是针对`dev`分支了，比如新提交一次后，`dev`指针往前移动一步，而`master`指针不变：![image-20210924195334804](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210924195334804.png)

假如我们在`dev`上的工作完成了，就可以把`dev`合并到`master`上。Git怎么合并呢？最简单的方法，就是直接把`master`指向`dev`的当前提交，就完成了合并：![image-20210924195358702](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210924195358702.png)

所以Git合并分支也很快！就改改指针，工作区内容也不变！

合并完分支后，甚至可以删除`dev`分支。删除`dev`分支就是把`dev`指针给删掉，删掉后，我们就剩下了一条`master`分支：![image-20210924195422158](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210924195422158.png)















## 常用命令及作用解释：

`git init`  初始化一个仓库

`git add filename` 把文件添加进去，实际上就是把文件修改添加到暂存区

`git commit -m "更新描述"` 提交更改，实际上就是把暂存区的所有内容提交到当前分支，`-m`后面输入的是本次提交的说明，因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，`git commit`就是往`master`分支上提交更改

`git status` 查看当前仓库的状态，是否有需要提交的内容，是否有修改的内容等等

`git diff` 查看具体修改内容

`git log` 查看完整日记

`git log --pretty=oneline`  查看简洁日记，标红的地方都是commit id，因为Git是分布式的版本控制系统，后面我们还要研究多人在同一个版本库里工作，如果大家都用1，2，3……作为版本号，那肯定就冲突了。

![image-20210924111622546](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210924111622546.png)

`git reset --hard HEAD~x 或者 git reset --hard xxx` 用于回退到指定的版本

`git reflog` 可以记录每次命令

`git remote add origin git@github.com:账户名/仓库名.git`，将本地仓库和github仓库连接起来，`origin`是远端仓库名

`git push origin main` 向远端库推送最新的本地仓库(main分支)信息

`git remote -v`查看远程库信息

`git remote rm <name> `删除远程库，此处的“删除”其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库。远程库本身并没有任何改动。要真正删除远程库，需要登录到GitHub，在后台页面找到删除按钮再删除。

`git branch` 查看分支

`git branch <name>`创建分支

`git checkout <name>`或者`git switch <name>`切换分支

`git checkout -b <name>`或者`git switch -c <name>`创建+切换分支

`git merge <name>`合并某分支到当前分支

`git branch -d <name>` 删除分支