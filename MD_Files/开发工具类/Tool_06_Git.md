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
git push origin master

```

拉取：

```sh
git pull 
```





# OVER

- 结尾跳转