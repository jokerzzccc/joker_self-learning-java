# SSH 连接 Linux 配置

始于：2021-06-22

Linux 版本：CentOS 7.7



# SSH 连接步骤

1. 新建一个账户
2. 生成 SSH 密钥
3. 把 SSH 密钥发送到 root 账户
4. 就可以利用 SSH 登录 root 账户了

```sh
# 1 root 下新建一个账户,设置密码
useradd <new_user_name>
passwd <new_user_name>
# 2 切换到新账户
su <new_user_name>
# 3 新建 SSH 密钥,然后，按三个回车就行。ssh 密钥在 /home/new_user_name/.ssh/id_rsa.pub
ssh-keygen
# 4 把密钥保存到 root 账户，
# 也可以直接把新账户的公钥内容，复制到 root账户下的 /root/.ssh/ authorized_keys 里
ssh-copy-id root@<服务器_ip>
# 5 完成，就可以用 ssh 密钥登录 root 账户了
ssh root@<服务器_ip>
```

注意： 

1. root 账户生成的 ssh 密钥在 /root/.ssh/id_rsa.pub
2. 普通账户生成的 ssh 密钥在 /home/<new_user_name>/.ssh/id_rsa.pub
3. 授权的 SSH 密钥 要复制进 /root/.ssh/ authorized_keys ，这样才可以在其它账户通过 ssh 登录进 root 账户。





#  公钥与私钥

- 公钥于私钥总是成对出现
- 公钥加密，私钥解密



# ECS 生成密钥

- 在阿里云控制台》网络安全》密钥对》创建密钥》绑定密钥对》将 ECS 实例绑定密钥对
- 注意：
  - 创建密钥后，要下载私钥（会提示）（一定保存好，只能下载一次）
  - 阿里云服务器，创建密钥对绑定后，会自动禁止 root 账户密码登录

![image-20210623222009048](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210623222009048.png)

## XShell 导入私钥

直接连接服务器，导入刚才下载的私钥，点击确定就好，不用输 ssh 下的密码。