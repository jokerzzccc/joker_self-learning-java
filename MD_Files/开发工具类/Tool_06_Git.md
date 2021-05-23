# Git

# 1、更换电脑后，使用 GIT 的操作

1. 本地 Git 生成 SSH 密钥
2. 去提示的路径 复制密钥内容
3. 登录 GitHub 网页，点击头像，settings -> SSH keys -> 添加新 SSH 密钥
4. 再复制项目的 Clone -> SSH 路径
5. 再到本地 clone 
6. over



```sh
# 1 ,输入之后，直接三次回车，会看到你的 ssh 保存路径，然后 把 id_rsa.pub 的内容复制下来
ssh-keygen -t rsa

```

