---
title: Elasticsearch
layout: post
tags:
  - Es
categories:
  - Elasticsearch
cover: 'https://cdn.jsdelivr.net/gh/zjy1006/source/img/xinghong.png'
comments: false
abbrlink: d32d37e5
date: 2020-04-13 15:00:19
updated:
---

# Elasticsearch

[官方文档](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)

## 背景及介绍

### 背景

- Lucene 可以说是当下最先进、高性能、全功能的搜索引擎库——无论是开源还是私有，但它也仅仅只是一个库。为了充分发挥其功能，你需要使用 Java 并将 Lucene 直接集成到应用程序中。 更糟糕的是，您可能需要获得信息检索学位才能了解其工作原理，因为Lucene 非常复杂。
  - Lucene不支持分布式的。数据越大，存不下来。就需要多台服务器存数据，同时多台服务器都要安装Lucene。然后通过代码合并搜索结果。
  - 数据要考虑安全性，一台服务器挂了，那么上面的数据不就消失了。
- 为了解决Lucene使用时的繁复性和保证数据的安全性，于是Elasticsearch便应运而生。它使用 Java 编写，内部采用 Lucene 做索引与搜索，但是它的目标是使全文检索变得更简单，简单来说，就是对Lucene 做了一层封装，它提供了一套简单一致的 RESTful API 来帮助我们实现存储和检索。

### 介绍

- Elasticsearch是一个开源的分布式、RESTful 风格的**搜索**和**数据分析**引擎。它的底层是开源库Apache Lucene。
  - 可以作为一个大型分布式集群（数百台服务器）技术，处理PB级数据，服务大公司；也可以运行在单机上，服务小公司
  - Elasticsearch不是什么新技术，主要是将全文检索、数据分析以及分布式技术，合并在了一起，才形成了独一无二的ES；lucene（全文检索），商用的数据分析软件（也是有的），分布式数据库（mycat）
  - 对用户而言，是开箱即用的，非常简单，作为中小型的应用，直接3分钟部署一下ES，就可以作为生产环境的系统来使用了，数据量不大，操作不是太复杂
  - 数据库的功能面对很多领域是不够用的（事务，还有各种联机事务型的操作）；特殊的功能，比如全文检索，同义词处理，相关度排名，复杂数据分析，海量数据的近实时处理；Elasticsearch作为传统数据库的一个补充，提供了数据库所不能提供的很多功能
  - 摘抄自：链接：https://www.jianshu.com/p/60b242cbd8b4
- 用mysql也可以做到数据的搜索和数据的分析。但是业有专攻，mysql主要是用来做数据的持久化存储和管理的（也就是CRUD）。
- 成为当今全文搜索领域的主流软件之一！

## 全文检索

全文检索是指计算机索引程序通过扫描文章中的每一个词，对每一个词建立一个索引，指明该词在文章中出现的次数和位置，当用户查询时，检索程序就根据事先建立的索引进行查找，并将查找的结果反馈给用户的检索方式。**这个过程类似于通过字典中的检索字表查字的过程。**

#### 全文检索的方法主要分为按字检索和按词检索两种。

##### 按字检索

是指对于文章中的每一个字都建立索引，检索时将词分解为字的组合。对于各种不同的语言而言，字有不同的含义，比如英文中字与词实际上是合一的，而中文中字与词有很大分别。

##### 按词检索

指对文章中的词，即语义单位建立索引，检索时按词检索，并且可以处理同义项等。英文等西方文字由于按照空白切分词，因此实现上与按字处理类似，添加同义处理也很容易。中文等东方文字则需要切分字词，以达到按词索引的目的，关于这方面的问题，是当前全文检索技术尤其是中文全文检索技术中的难点，在此不做详述。

## 倒排索引

搜索引擎的原理就是建立倒排索引。是根据文章内容中的关键字建立索引。

- 分片就是将大段的信息切成小段，每段称为为片，且有一个专有ID，
- 倒排索引就是分片之后，以关键词做KEY，分片ID做VALUE

![](https://gitee.com/zhou_jin_yuan/blogimage/raw/master/img/20200516013445.png)

> - 将每一个要保存的记录拆分成单个的词，并进行记录
> - 如果要检索某一个词或者句子。会按照记录中出现词的频率进行相关性得分计算得出匹配值最高的返回给用户

## ElasticSearch的基础概念

![](https://gitee.com/zhou_jin_yuan/blogimage/raw/master/img/20200516020654.png)

![](https://gitee.com/zhou_jin_yuan/blogimage/raw/master/img/20200516020745.png)

## Docker安装ES

```
docker pull elasticsearch:7.4.2
docker pull kibana:7.4.2

docker images //检查信息
free -m //查看内存
```

```
sudo mkdir -p /mydata/elasticsearch/config
sudo mkdir -p /mydata/elasticsearch/data

echo "http.host: 0.0.0.0" >> /mydata/elasticsearch/config/elasticsearch.yml

chmod -R 777 /mydata/elasticsearch/

docker run --name elasticsearch -p 9200:9200 -p 9300:9300 -e “discovery.type=single-node” -e ES_JAVA_OPTS="-Xms256m -Xmx256m" -v /mydata/elasticsearch/config/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml -v /mydata/elasticsearch/data:/usr/share/elasticsearch/data -v /mydata/elasticsearch/plugins:/usr/share/elasticsearch/plugins -d elasticsearch:7.4.2
```

> 中途踩过的坑：
>
> - main ERROR No Log4j 2 configuration file found. Using default configuration (logging only errors to the console), or user programmatically provided configurations. Set system property 'log4j2.debug' to show Log4j 2 internal initialization logging. See https://logging.apache.org/log4j/2.x/manual/configuration.html for instructions on how to configure Log4j 2
>   Exception in thread "main" SettingsException[Failed to load settings from [elasticsearch.yml]]; nested: ParsingException[Failed to parse object: expecting token of type [START_OBJECT] but found [VALUE_STRING]];
> - 解决：冒号后面加一个空格（http.host: 0.0.0.0）

看到这个就安装成功了：

![](https://gitee.com/zhou_jin_yuan/blogimage/raw/master/img/20200516042935.png)

## Docker快速安装kibana

```
docker run --name kibana -e ELASTICSEARCH_URL=http://192.168.56.10 -p 5601:5601 -d kibana:7.4.2
```

> 中途踩到的坑：
>
> - 访问出现：Kibana server is not ready yet  
>
>   - 解决： docker logs kibana   后发现问题是：Error: No Living connections
>
>   - 将配置文件kibana.yml中的elasticsearch.url改为正确的链接，默认为: http://elasticsearch:9200，改为http://自己的IP地址:9200
>
>     - ```
>       find / -type f -name kibana.yml		//搜索kibana.yml文件位置
>       
>       一下搜索到了三个文件：
>       /var/lib/docker/overlay2/2656a983a8bbd86b59010529be3e8f3cd516224e51757262a1046b1cd6c348cb/diff/usr/share/kibana/config/kibana.yml
>       /var/lib/docker/overlay2/38ccdac20b4715cb79c0f7069792d752932740a3aab31117b8d555abce565899/diff/usr/share/kibana/config/kibana.yml
>       /var/lib/docker/overlay2/43ebf6e1d13cdf4ff3a65abe2009a5012627da137a7c50b1634441e3c020cd5f/merged/usr/share/kibana/config/kibana.yml
>       每个都进去看一下，最终确定是第二个文件。然后改了就行了
>       ```
>
>     - 我的参考：https://blog.csdn.net/fv8023/article/details/96427702和https://blog.csdn.net/qq_35210048/article/details/105962845

