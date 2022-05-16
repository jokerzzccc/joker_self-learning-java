# Tool_13_Jenkins

[toc]

时间：2022年5月11日

参考博客：

- 官网：https://www.jenkins.io/zh/
- Jenkins 用户手册 :https://www.jenkins.io/zh/doc/
- Jenkins 使用教学博客：
  - https://cloud.tencent.com/developer/article/1829940
  - https://segmentfault.com/a/1190000038464808





# 1、Jenkins 基本信息

## 1.1 是什么

> Jenkins是一个`开源软件`项目，是基于`java`开发的一种`持续集成`工具，用于监控持续重复的工作，旨在提供一个开放易用的软件平台，使软件的持续集成变成可能。

简单来说，它就是一个 **持续集成** 的工具！

### 1.1.1  持续集成

**持续集成**（Continuous Integration），简称 **CI**。频繁地将代码集成到主干之前，必须通过自动化测试，只要有一个测试用例失败，就不能集成。通过持续集成，团队可以快速从一个功能到另外一个功能。

![image-20220516234424779](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220516234424779.png)

**好处：**

+ 降低风险，由于持续集成不断去构建，编译和测试，可以很早发现问题
+ 减少重复性的工作
+ 持续部署，提供可部署单元包
+ 持续交付可供使用的版本



### 1.1.2 Jenkins 持续集成

![image-20220516234755514](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220516234755514.png)

我们先通过这张图来看到 **Jenkins** 在其中起到的作用：

+ 首先，开发人员将代码提交到 **Git** 仓库
+ 然后 **Jenkins** 使用 **Git** 插件来拉取 **Git** 仓库的代码，然后配合 **JDK、Maven** 等软件完成代码编译，测试、审查、、测试和打包等工作
+ 最后 **Jenkins** 将生成的 **jar/war** 推送到 **测试/生产 服务器** ，供用户访问

整套步骤下来，作为开发人员我们只需要提交下代码，剩下的工作都交给了 **Jenkins** ，真是美滋滋，怎么没有早点上这个工具的车！