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





# Git 命令详解

## 1、git rebase

rebase 翻译为**变基**，他的作用和 merge 很相似，用于把一个分支的修改合并到当前分支上。

如下图所示，下图介绍了经过 rebase 后提交历史的变化情况。

![image-20220509005446555](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220509005446555.png)

现在我们来用一个例子来解释一下上面的过程。

假设我们现在有2条分支，一个为 master，一个为 feature/1，他们都基于初始的一个提交 add readme 进行检出分支，之后，master 分支增加了 3.js 和 4.js 的文件，分别进行了2次提交，feature/1 也增加了 1.js 和 2.js 的文件，分别对应以下2条提交记录。

此时，对应分支的提交记录如下。

master 分支如下图：

![image-20220509005510859](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220509005510859.png)

feature/1 分支如下图

![image-20220509005619175](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220509005619175.png)

结合起来看是这样的

![image-20220509005640947](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220509005640947.png)

此时，切换到 feature/1 分支下，执行 `git rebase master`，成功之后，通过 `git log` 查看记录。

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

- `git revert` 撤销某次操作，此操作不会修改原本的提交记录，而是会新增一条提交记录来抵消某次操作。





# THE END

- 结尾跳转