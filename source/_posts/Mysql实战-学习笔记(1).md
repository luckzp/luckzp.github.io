---
title: Mysql实战-学习笔记(1)
date: 2019-10-18 21:43:08
tags: Mysql
---

### 1. 一条SQL查询语句是如何执行的？

创建数据库连接，SQL经过分析器分析语义，优化器选择合适的索引，选择最佳的执行方案，最后执行器执行。

如果是更新语句，先是查找到这行，然后才执行更新操作。

### 2. 日志系统：一条SQL更新语句是如何执行的？

Mysql中的Binlog有2种模式，statement模式记录的是sql语句，row格式会记录的行的内容，记录2条，更新前和更新后。

Mysql默认执行引擎Innodb的redo log 记录数据页有什么改动。

relog采用2阶段提交保证和binlog的一致性。

**日志系统使用场景**

当误操作后或多搭建一个读库的时候，现在常见的做法就是全量备份和加上Binlog来实现的。如果relog和binlog不一致，会导致恢复的数据库数据不一致或主从数据库不一致的情况发生。

<!--more--> 

**日志系统参数配置**

innodb_flush_log_at_trx_commit 这个参数设置成1 的时候，表示每次事务的 redo log 都直接持久化到磁盘。

sync_binlog 这个参数设置成 1 的时候，表示每次事务的 binlog 都持久化到磁盘。

**日志系统命令**

全量备份命令

```sql
mysqldump -u[username] -p[password]  [database] [table] > backup.sql
```

恢复备份命令

```sql
mysql -u[username] -p[password] [database] < backup.sql
```

当然也可以用Mysql可视化工具进行备份。

### 3. 事务隔离：为什么你改了我还看不见？

在一个事务里，实际上每条记录在更新的时候都会同时记录一条回滚日志操作。记录上的最新值，通过回滚操作，都可以得到前一个状态的值。

**大事务的弊端**

- 大事务会导致记录大量的回滚操作，在这个事务提交之前，数据库里面它可能用到的回滚记录都必须保留，这就会导致大量占用存储空间。
- 大事务还占用锁资源，提高死锁概率，长时间占用连接，也可能拖垮整个库。

 **大事务的排查**

在information_schema库的innodb_trx这个表中查询长事务，比如下面这个语句，用于查找持续时间超过60s的事务。 

```sql
select * from information_schema.innodb_trx where TIME_TO_SEC(timediff(now(),trx_started))>60
```

同时也可以排查 Lock wait timeout exceeded; try restarting transaction 异常，找到相应的线程并kill thread id。 锁等待超时。是当前事务在等待其它事务释放锁资源造成的。可以找出锁资源竞争的表和语句。

### 4 深入浅出索引

**回表**

普通索引查询方式，则需要先搜索 普通索引树，得到一个主键索引的值，再到主键 索引树搜索一次。这个过程称为回表。普通索引树的叶子节点是主键值,主键索引树的叶子节点是数据。所以，普通索引树比主键索引树小很多。

**覆盖索引**

```sql
select ID from T where k between 3 and 5
```

这时只需要查 ID 的值，而 ID 的值已经在 k 索引树上了，因此可以直接提供查询结果，不需要回表。也就是说，在这个查询里面，索引 k 已经“覆盖了”我们的查询需求，我们称为覆盖索引。不再需要回表查整行记录，减少语句的执行时间。

联合索引（a,b）也可以达到覆盖索引的要求。假如现在有个查询是

```sql
select b from T where a = x
```

这样也不用回表了。