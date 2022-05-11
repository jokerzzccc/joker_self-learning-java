# 目录

[TOC]



# Git

2021/03/01

==学习路径：==

- GIT 详细参考文档
  - （所有命令都有）：https://git-scm.com/docs
- 中文 Git  书籍：https://git-scm.com/book/zh/v2
- 常用命令讲解的博客：https://juejin.cn/post/6974184935804534815
- https://www.ruanyifeng.com/blog/2014/06/git_remote.html



# 1、更换电脑后，使用 GIT 的操作

1. 本地 Git （打开 git bash 页面）生成 SSH 密钥
2. 去提示的路径 复制密钥内容
3. 登录 GitHub 网页，点击头像，settings -> SSH keys -> 添加新 SSH 密钥
4. 再复制项目的 Clone -> SSH 路径
5. 再到本地 clone 
6. over



```sh
# 1 ,输入之后，直接三次回车，会看到你的 ssh 保存路径，然后 把 id_rsa.pub 的内容复制下来
ssh-keygen -t rsa

```



# 2、Git 修改 提交用户名

```bash
git config --global user.name "用户名"
git config --global user.email "邮箱"
```

如果，出现了，报错，Git 不知道选择哪个用户名时，使用：

```sh
git config --global --replace-all user.name "用户名"
```



- 根据我的体验，如果，是 GitHub 账户的邮箱，就会是 GitHub 的用户名。



# 3、.gitignore 文件使用

- 就是定义 ==忽略==上传哪些文件的。
- 官方参考：https://git-scm.com/docs/gitignore
- 参考博客：https://juejin.cn/post/6864948698695073799





## 常见命令

以下是一些帮助你正确设置 `.gitignore` 文件的基本规则：

1. 任何以哈希（`#`）开头的行都是注释。
2. `\` 字符可以转义特殊字符。
3. `/` 字符表示该规则只适用于位于同一文件夹中的文件和文件夹。
4. 星号（`*`）表示任意数量的字符（零个或更多）。
5. 两个星号（`**`）表示任意数量的子目录。
6. 一个问号（`?`）代替零个或一个字符。
7. 一个感叹号（`!`）会反转特定的规则（即包括了任何被前一个模式排除的文件）。
8. 空行会被忽略，所以你可以用它们来增加空间，使你的文件更容易阅读。
9. 在末尾添加 `/` 会忽略整个目录路径。



注意：

有两种类型的 `.gitignore` 文件：

- **本地**：放在 Git 仓库的根目录下，只在该仓库中工作，并且必须提交到该仓库中。
- **全局**：放在你的主目录根目录下，影响你在你的机器上使用的每个仓库，不需要提交。

## 使用的地方

常见就是：

1. 含有敏感信息的文件
2. 编译出的代码，如 `.dll` 或 `.class`。
3. 系统文件，如 `.DS_Store` 或 `Thumbs.db`。
4. 含有临时信息的文件，如日志、缓存等。
5. 生成的文件，如 `dist` 文件夹。



# 4、Git 常见命令

## 常用流程

上传：

```sh
git add .
git commit -m "update"
# 也可以直接 git push
git push origin master

```

拉取：

```sh
git pull 
```



# 5、git 查看本地所有用户

```sh
# 查看本地所有配置
git config --list

# 查看当前全局用户的配置
git config --global  --list

# 查看当前仓库的配置信息
git config --local  --list
```



# 6 、合并远程仓库分支

```sh
# 新建一个与远程仓库分支同名的分支
git branch <new_branch>
# 或者直接，先本地新建一个 分支 ,并切换到该分支
git branch -b <new_branch>
# 在新分支下。拉取对应远程仓库代码，
git pull origin <remote_branch>
# 再切回 原分支，
git checkout <old_branch>
# 直接 将 new_branch 合并到 old_branch
git merge <new_branch>
# 如果 ，出现了冲突 conflict ，就去对应的文件修改，修改之后
git add .
git commit
# 可以把新建的 分支删掉
git branch -D <new_branch>
```



# 7、删除分支

## 7.1 删除本地分支

```sh
# 
git branch -d <branch_name>
# 
git branch -D <branch_name>

```



## 7.2 删除远程仓库分支





## 7.3 删除追踪分支



# 8、拉取远程仓库另一个分支

```sh
# 1.产看远程最新内容分支
git fetch origin
# 2 查看所有分支（包括本地和远程分支）
git branch -a
# 3 检出远程分支到本地，并切换分支
git checkout -b <branch_name> origin/<branch_name>
# 4 再次查看本地分支,就能看到了
git branch
```





# 9、Git 命令详解

## 1、git rebase

- rebase 翻译为**变基**，他的作用和 merge 很相似，用于把一个分支的修改合并到当前分支上。



如下图所示，下图介绍了经过 rebase 后提交历史的变化情况。

![image-20220509005446555](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220509005446555.png)



- 现在我们来用一个例子来解释一下上面的过程。

假设我们现在有2条分支，一个为 master，一个为 feature/1，他们都基于初始的一个提交 add readme 进行检出分支，之后，master 分支增加了 3.js 和 4.js 的文件，分别进行了2次提交，feature/1 也增加了 1.js 和 2.js 的文件，分别对应以下2条提交记录。

此时，对应分支的提交记录如下。

master 分支如下图：

![image-20220509005510859](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220509005510859.png)

feature/1 分支如下图

![image-20220509005619175](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220509005619175.png)

结合起来看是这样的

![image-20220509005640947](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220509005640947.png)



- 此时，切换到 feature/1 分支下，执行 `git rebase master`，成功之后，通过 `git log` 查看记录。

如下图所示：可以看到先是逐个应用了 mater 分支的更改，然后以 master 分支最后的提交作为基点，再逐个应用 feature/1 的每个更改。

![image-20220509005816540](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220509005816540.png)

所以，我们的提交记录就会非常清晰，没有分叉，上面演示的是比较顺利的情况，但是大部分情况下，rebase 的过程中会产生冲突的，此时，就需要**手动解决冲突**，然后使用依次 `git add ` 、`git rebase --continue ` 的方式来处理冲突，完成 rebase 的过程，如果不想要某次 rebase 的结果，那么需要使用 `git rebase --skip ` 来跳过这次 rebase 操作。

#### git merge 和 git rebase 的区别

不同于 `git rebase` 的是，`git merge` 在不是 fast-forward（快速合并）的情况下，会产生一条额外的合并记录，类似  `Merge branch 'xxx' into 'xxx'` 的一条提交信息。

![image-20220509010356661](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220509010356661.png)

另外，在解决冲突的时候，用 merge 只需要解决一次冲突即可，简单粗暴，而用 rebase 的时候 ，需要依次解决每次的冲突，才可以提交。




## 2、git stash

会有这么一个场景，现在你正在用你的 feature 分支上开发新功能。这时，生产环境上出现了一个 bug 需要紧急修复，但是你这部分代码还没开发完，不想提交，怎么办？

这个时候可以用 `git stash` 命令先把工作区已经修改的文件**暂存**起来，然后切换到 hotfix 分支上进行 bug 的修复，修复完成后，切换回 feature 分支，从堆栈中恢复刚刚保存的内容。

基本命令如下

```bash
git stash //把本地的改动暂存起来
git stash save "message" 执行存储时，添加备注，方便查找。
git stash pop // 应用最近一次暂存的修改，并删除暂存的记录
git stash apply  // 应用某个存储,但不会把存储从存储列表中删除，默认使用第一个存储,即 stash@{0}，如果要使用其他个，git stash apply stash@{$num} 。
git stash list // 查看 stash 有哪些存储
git stash clear // 删除所有缓存的 stash
```







## 3、git revert

- `git revert` 撤销某次操作，此操作不会修改原本的提交记录，而是会**新增一条提交记录来抵消某次操作**。

- 语法：
  -  `git revert <commit-id>` 针对普通 commit
  - `git revert <commit-id> -m` 针对 merge 的 commit



下面就用一个案例来理解一下这个命令:

如下图所示，假设被红框框起来的地方是会引起 bug 的一次提交，在他的提交之后，又进行了2次提交，其中包含了其它同事的提交。

![image-20220511214857898](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220511214857898.png)

此时想把引起提交的 bug 的干掉，执行 `git revert 1121932`，执行操作后，再打开查看日志，如下图所示，可以看到是新增了一条 commit 记录，这个 commit 的产生的 msg 是自动生成的，Revert 开头，后面跟撤回的 commit-msg 信息 之前的 commit 记录并没有消失，此时也达到了代码回退的效果![image-20220511214836195](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220511214836195.png)



此外 git revert 也可以**回滚多次的提交**

- 语法：`git revert [commit-id1] [commit-id2] ...`  注意这是一个**前开后闭区间**，即不包括 commit1 ，但包括 commit2 。



回滚我们的提交有二种方式，

- 一种是上文提到的`git revert`命令外，

- 还可以使用 `git reset` 命令，



> `git revert` 与 `git reset` 的区别

- `git revert` 会新建一条 commit 信息，来撤回之前的修改。

- `git reset` 会直接将提交记录退回到指定的 commit 上。

对于个人的 feature 分支而言，可以使用 `git reset` 来回退历史记录，之后使用 `git push --force` 进行推送到远程，但是如果是在多人协作的集成分支上，不推荐直接使用 `git reset` 命令，而是使用更加安全的 `git revert` 命令进行撤回提交。这样，提交的历史记录不会被抹去，可以安全的进行撤回。







## 4、git fetch

常用命令：

```sh
# 获取远程仓库特定分支的更新
git fetch <远程主机名> <分支名>
# 获取远程仓库所有分支的更新
git fetch --all
```



- 一旦远程主机的版本库有了更新（Git术语叫做commit），需要将这些更新取回本地，这时就要用到`git fetch`命令。

```sh
# 命令将某个远程主机的更新，全部取回本地。
git fetch <远程主机名>
```



`git fetch`命令通常用来查看其他人的进程，因为它取回的代码对你本地的开发代码没有影响。

- 默认情况下，`git fetch`取回所有分支（branch）的更新。

- 如果只想取回特定分支的更新，可以指定分支名。

```sh
git fetch <远程主机名> <分支名>
# 比如，取回`origin`主机的`master`分支。
git fetch origin master
```

所取回的更新，在本地主机上要用"远程主机名/分支名"的形式读取。比如`origin`主机的`master`，就要用`origin/master`读取。



`git branch`命令的

- `-r`选项，可以用来查看远程分支，

- `-a`选项查看所有分支。

- ```sh
  $ git branch -r
  origin/master
  
  $ git branch -a
  * master
    remotes/origin/master
  ```

上面命令表示，本地主机的当前分支是`master`，远程分支是`origin/master`。





取回远程主机的更新以后，可以在它的基础上，使用`git checkout`命令创建一个新的分支。

```sh
# 命令表示，在`origin/master`的基础上，创建一个新分支。
$ git checkout -b <newBrach> origin/master
```





此外，也可以使用`git merge`命令或者`git rebase`命令，在本地分支上合并远程分支。

```sh
# 命令表示在当前分支上，合并`origin/master`。
$ git merge origin/master
# 或者
$ git rebase origin/master
```



- 与 `git pull` 不同的是 `git fetch` 操作仅仅只会拉取远程的更改，不会自动进行 merge 操作。对你当前的代码没有影响



## 5、git pull

- git pull : 远程仓库拉取代码并合并到本地

```sh
# 从远程仓库拉取代码并合并到本地，可简写为 git pull 等同于 git fetch && git merge 
git pull <远程主机名> <远程分支名>:<本地分支名>
# 使用rebase的模式进行合并
git pull --rebase <远程主机名> <远程分支名>:<本地分支名>

```



## 6、git reset

- 参考博客
  - https://www.jianshu.com/p/c2ec5f06cf1a



**reset 的本质：移动 HEAD 以及它所指向的 branch**

实质上，**reset** 这个指令虽然可以用来撤销 **commit** ，但它的实质行为并不是撤销，而是移动 **HEAD** ，并且「捎带」上 **HEAD** 所指向的 **branch**（如果有的话）。也就是说，**reset** 这个指令的行为其实和它的字面意思 "**reset**"（重置）十分相符：它是用来重置 **HEAD** 以及它所指向的 **branch** 的位置的。

而 **reset --hard HEAD^** 之所以起到了撤销 **commit** 的效果，是因为它把 **HEAD** 和它所指向的 branch 一起移动到了当前 **commit** 的父 **commit** 上，从而起到了「撤销」的效果：

![img](https:////upload-images.jianshu.io/upload_images/4428238-6dbab74ae9ad2e1f?imageMogr2/auto-orient/strip|imageView2/2/w/466/format/webp)



Git 的历史只能往回看，不能向未来看，所以把 **HEAD** 和 **branch** 往回移动，就能起到撤回 **commit** 的效果。

所以同理，**reset --hard** 不仅可以撤销提交，还可以用来把 **HEAD** 和 **branch** 移动到其他的任何地方。



```shell
git reset --hard branch2
```

![img](https:////upload-images.jianshu.io/upload_images/4428238-71f7141a3878da7e?imageMogr2/auto-orient/strip|imageView2/2/w/434/format/webp)

### **reset三种模式区别和使用场景**

#### 区别：

1. **--hard**：重置位置的同时，直接将 **working Tree工作目录**、 **index 暂存区**及 **repository** 都重置成目标**Reset**节点的內容,所以效果看起来等同于清空暂存区和工作区。
2. **--soft**：重置位置的同时，保留**working Tree工作目录**和**index暂存区**的内容，只让**repository**中的内容和 **reset** 目标节点保持一致，因此原节点和**reset**节点之间的【差异变更集】会放入**index暂存区**中(**Staged files**)。所以效果看起来就是工作目录的内容不变，暂存区原有的内容也不变，只是原节点和**Reset**节点之间的所有差异都会放到暂存区中。
3. **--mixed（默认）**：重置位置的同时，只保留**Working Tree工作目录**的內容，但会将 **Index暂存区** 和 **Repository** 中的內容更改和reset目标节点一致，因此原节点和**Reset**节点之间的【差异变更集】会放入**Working Tree工作目录**中。所以效果看起来就是原节点和**Reset**节点之间的所有差异都会放到工作目录中。

#### 使用场景:

1. **--hard**：(1) **要放弃目前本地的所有改变時**，即去掉所有add到暂存区的文件和工作区的文件，可以执行 **git reset -hard HEAD** 来强制恢复git管理的文件夹的內容及状态；(2) **真的想抛弃目标节点后的所有commit**（可能觉得目标节点到原节点之间的commit提交都是错了，之前所有的commit有问题）。
2. **--soft**：原节点和**reset**节点之间的【差异变更集】会放入**index暂存区**中(**Staged files**)，所以假如我们之前工作目录没有改过任何文件，也没add到暂存区，那么使用**reset  --soft**后，我们可以直接执行 **git commit** 將 index暂存区中的內容提交至 **repository** 中。为什么要这样呢？这样做的使用场景是：假如我们想合并「当前节点」与「**reset**目标节点」之间不具太大意义的 **commit** 记录(可能是阶段性地频繁提交,就是开发一个功能的时候，改或者增加一个文件的时候就**commit**，这样做导致一个完整的功能可能会好多个**commit**点，这时假如你需要把这些**commit**整合成一个**commit**的时候)時，可以考虑使用**reset  --soft**来让 **commit** 演进线图较为清晰。总而言之，**可以使用--soft合并commit节点**。
3. **--mixed（默认）**：(1)使用完**reset --mixed**后，我們可以直接执行 **git add** 将這些改变果的文件內容加入 **index 暂存区**中，再执行 **git commit** 将 **Index暂存区** 中的內容提交至**Repository**中，这样一样可以达到合并**commit**节点的效果（与上面--soft合并commit节点差不多，只是多了git add添加到暂存区的操作）；(2)移除所有Index暂存区中准备要提交的文件(Staged files)，我们可以执行 **git reset HEAD** 来 **Unstage** 所有已列入 **Index暂存区** 的待提交的文件。(有时候发现add错文件到暂存区，就可以使用命令)。(3)**commit**提交某些错误代码，或者没有必要的文件也被**commit**上去，不想再修改错误再**commit**（因为会留下一个错误**commit**点），可以回退到正确的**commit**点上，然后所有原节点和**reset**节点之间差异会返回工作目录，假如有个没必要的文件的话就可以直接删除了，再**commit**上去就OK了。









# 10、Git 基础信息

- git 的仓库结构：

![image-20220510224816458](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220510224816458.png)



- **Workspace**：**工作区**
  - 就是平时进行开发改动的地方，是当前看到最新的内容，在开发的过程也就是对工作区的操作

- **Index**：**暂存区**
  - 当执行 `git add` 的命令后，工作区的文件就会被移入暂存区，暂存区标记了当前工作区中那些内容是被 Git 管理的，当完成某个需求或者功能后需要提交代码，第一步就是通过 `git add` 先提交到暂存区。

- **Repository**：**本地仓库**
  - 位于自己的电脑上，通过 `git commit` 提交暂存区的内容，会进入本地仓库。

- **Remote**：**远程仓库**
  - 用来托管代码的服务器，远程仓库的内容能够被分布在多个地点的处于协作关系的本地仓库修改，本地仓库修改完代码后通过 `git push` 命令同步代码到远程仓库。



一般来说，Git 的工作流程分为以下几步

```tex
1.在工作区开发，添加，修改文件。
2.将修改后的文件放入暂存区。
3.将暂存区域的文件提交到本地仓库。
4.将本地仓库的修改推送到远程仓库。
```







# THE END

- 结尾跳转