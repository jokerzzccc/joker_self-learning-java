# Day29-ElasticSearch

版本：Elasticsearch 7.12

2021-04-07

学习视频：狂神说



ElasticSearch7和ElasticSearch6版本差别很大，注意

ES 官网：https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html

SQL类数据库，大数据 搜索特别慢，

ElasticSearch ：主要就是用来全文搜索（百度，GitHub，淘宝电商。。。）

Solr 也是一个全文搜索服务器。



内容：

1. 一个人
2. 货比三家
3. 安装
4. 生态
5. 分词器 ik
6. RestFul 操作ES
7. ES的CRUD
8. SpringBoot 集成 ES (从原理分析)
9. 爬虫爬取数据
10. 实战，模拟全文检索



- 大数据量的情况下，只要用到搜索，就可以用ES

日志数据分析：ELK：Elasticsearch/Logstash/Kibana

# Lucene

重要人物；Doug Cutting

Lucene : 一套信息检索工具包。jar包。不包含：搜索引擎系统。

包含的：索引结构。读写索引的工具。排序。搜索规则。工具类

Lucene 和ES 的关系：

ES 是基于Lucene 做了一些封装和增强。（上手很简单）



# ElasticSearch 概述

## ElasticSearch 简介



## solr 简介



## Lucene简介



## ElasticSearch 和solr比较



# ES 安装

注意：至少jdk1.8

ES 是基于java开发的。ES 的版本和之后对应的java的核心jar包要对应

ES 下载地址：https://www.elastic.co/cn/downloads/elasticsearch

官网下载很慢，可以翻墙快一点。

我下载的版本是 elasticsearch-7.12.0-windows-x86_64.zip

现在先在windows下学习，后面再去linux上部署集群

ELK 三剑客，解压即用

这是web项目，需要前端环境（比如：nodeJS,python）

## windows下安装ES

加压，就行，

启动，就是点击bin/elasticsearch.bat

## 安装可视化界面 ES head

其实，谷歌自带head插件

ES head 下载地址：https://github.com/mobz/elasticsearch-head

步骤：

1. 解压启动

   ```bash
   npm install
   npm run dev
   ```

2. 连接测试发现,存在跨域问题，配置es：elasticsearch.yml



## kibana 安装

kibana  下载地址：https://www.elastic.co/cn/downloads/kibana