# Day34-RocketMQ

[toc]

时间：2022-4-12

相关网址：

- RocketMQ 官网：https://rocketmq.apache.org/
- RocketMQ 中文开发者指南文档：https://github.com/apache/rocketmq/tree/master/docs/cn
- RocketMQ GitHub 地址：https://github.com/apache/rocketmq
- 教学博客相关：
  - https://juejin.cn/post/6844904008629354504
  - https://heapdump.cn/article/3801373



# 1、介绍



# 2、安装



# 3、相关命令：

rokcetmq 目录：/usr/local/src/rocketmq-4.9.0

- 启动 nameserver

  - ```sh
    nohup sh /usr/local/src/rocketmq-4.9.0/bin/mqnamesrv > /usr/local/src/rocketmq-4.9.0/logs/mqnamesrv.log 2>&1 &
    ```

- 关闭 nameserver

  - ```sh
    sh /usr/local/src/rocketmq-4.9.0/bin/mqshutdown namesrv
    ```

- 启动 broker :

  - ```sh
    nohup sh /usr/local/src/rocketmq-4.9.0/bin/mqbroker -n 47.117.128.238:9876 autoCreateTopicEnable=true > /usr/local/src/rocketmq-4.9.0/logs/mqbroker.log 2>&1 &
    
    
    nohup sh /usr/local/src/rocketmq-4.9.0/bin/mqbroker -n 47.117.128.238:9876 -c conf/broker_joker.properties > /usr/local/src/rocketmq-4.9.0/logs/mqbroker.log 2>&1 &
    
    ```

- 关闭 broker:

  - ```sh
    sh /usr/local/src/rocketmq-4.9.0/bin/mqshutdown broker
    ```

- 查看是否启动：`jps`





# 4、相关 API



# 5、RocketMQTemplate 的使用



