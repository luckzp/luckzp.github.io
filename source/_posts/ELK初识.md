---
title: ELK初识
urlname: ff90s4
date: 2020-09-12 12:18:49 +0800
tags: []
categories: []
---

---

## title: ELK 初识 date: 2020-02-09 22:43:36

tags:

ELK 是**E**lasticsearch，**L**ogstash，**K**ibana 三个组件组合起来的缩写。

**E**lasticsearch 是搜索引擎，**L**ogstash 是收集数据输出到 elasticsearch 中。**K**ibana 是**E**lasticsearch 数据的可视化，类比于 navicat 是 Mysql 数据的可视化。

### 1. ElasticSearch 概念

画一个对比图来类比传统关系型数据库：

- 关系型数据库 -> Databases(库) -> Tables(表) -> Rows(行) -> Columns(列)。
- Elasticsearch -> Indeces(索引) -> Types(类型) -> Documents(文档) -> Fields(属性)。

**索引**

一个索引(index)就像是传统关系数据库中的数据库，它是相关文档存储的地方。

**文档**

就是一个对象 Json 串。

**分片（shards）**

索引可能存储大量可能超过单个节点的硬件限制的数据，需要将索引细分，细分后的部分是分片。

**副本（Replicasedit）**

副本，是对分片的复制。

**倒排索引**

就是通过文档中的字段反查到文档的 id。

ES 的 JSON 文档中的每个字段，都有自己的倒排索引。

组成：单词字典（单词到倒排列表的关系）和倒排列表（记录了单词对应文档的结合）。

### 2. 搭建 ELK 环境读取数据库

1. 下载版本相同的 elasticsearch，logstash，kibana。
2. logstash 安装 logstash-jdbc-input 插件。
3. 配置 logstash.conf

```json
input {
  #beats {
  #  port => 5044
  #}
  jdbc {
      # 数据库  数据库名称为elk，表名为book_table
      jdbc_connection_string => "jdbc:mysql://127.0.0.1:3306/tmall_springboot?characterEncoding=UTF-8"
      # 用户名密码
      jdbc_user => "root"
      jdbc_password => "123456"
      # jar包的位置
      jdbc_driver_library => "E:\ELK\logstash-7.5.2\mysql-connector-java-5.1.30.jar"
      # mysql的Driver
      jdbc_driver_class => "com.mysql.jdbc.Driver"
      jdbc_paging_enabled => "true"
      jdbc_page_size => "100"
      #statement_filepath => "E:\logstash-7.3.0\config\viewlogs.sql"
      statement => "select * from category where id > :sql_last_value"
      schedule => "* * * * *"
      #是否记录上次执行结果, 如果为真,将会把上次执行到的 tracking_column 字段的值记录下来,保存到 last_run_metadata_path 指定的文件中
      record_last_run => true

      #是否需要记录某个column 的值,如果 record_last_run 为真,可以自定义我们需要 track 的 column 名称，此时该参数就要为 true. 否则默认 track 的是 timestamp 的值.
      use_column_value => true

      #如果 use_column_value 为真,需配置此参数. track 的数据库 column 名,该 column 必须是递增的.比如：ID.
      tracking_column => id

      #指定文件,来记录上次执行到的 tracking_column 字段的值
      #比如上次数据库有 10000 条记录,查询完后该文件中就会有数字 10000 这样的记录,下次执行 SQL 查询可以从 10001 条处开始.
      #我们只需要在 SQL 语句中 WHERE MY_ID > :last_sql_value 即可. 其中 :last_sql_value 取得就是该文件中的值(10000).
      #last_run_metadata_path => "E:\ELK\logstash-7.5.2\viewlogs"

      #是否清除 last_run_metadata_path 的记录,如果为真那么每次都相当于从头开始查询所有的数据库记录
      clean_run => false

      #是否将 column 名称转小写
      #lowercase_column_names => false
    }
}

output {

  elasticsearch {
    hosts => ["http://localhost:9200"]
    #按分钟
    #index => "mysql-%{+YYYY.MM.dd.HH.mm}"
    #按小时
    index => "logstashmysql"
    #index => "wdnmd"
    #index => "%{[servicename]}"
    #index => "logstash-%{[fields][document_type]}-%{+YYYY.MM.dd}"
    #index => "%{[@metadata][beat]}-%{[@metadata][version]}-%{+YYYY.MM.dd}"
    #user => "elastic"
    #password => "changeme"
  }
   stdout {
        codec => rubydebug
    }
}
```

在 logstash 当前目录下启动 logstash

```shell
bin/logstash -f logstash.conf
```

### 3. 用 sql 语句查询 elasticsearch

安装的 elasticsearch 是 7.5.2 版本，可以用 sql 语句查询 elasticsearch。

```
POST /_sql?format=txt
{
    "query": "SELECT order_id  FROM goodslist WHERE goods_list like '%抽纸%' group by order_id"
}
```

![](http://ww1.sinaimg.cn/large/aacc02d8gy1gbqj3fulqgj211h0er0vp.jpg#alt=%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20200209223923.png)

SQL 转 DSL

```
POST /_sql/translate?format=txt
{
    "query": "SELECT *  FROM mobile_index WHERE mobile like '%760714206%'"
}
```

![](http://ww1.sinaimg.cn/large/aacc02d8ly1gckaw3oez9j212b0gp759.jpg#alt=DSL.png)
