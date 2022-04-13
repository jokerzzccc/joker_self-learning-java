# 目录

[TOC]



# Git

2021/03/01

==学习路径：==

- GIT 官方参考文档：https://git-scm.com/docs



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
# 2 查看所有分支
git branch -a
# 3 检出远程分支到本地，并切换分支
git checkout -b <branch_name> origin/<branch_name>
# 4 再次查看本地分支,就能看到了
git branch
```



# OVER

- 结尾跳转