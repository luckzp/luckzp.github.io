---
title: Mysql实战-学习笔记(2)
date: 2019-10-22 21:58:45
tags: Mysql
---

### 7 行锁功过：怎么减少行锁对性能的影响？

**两阶段锁协议**

在 InnoDB 事务中，行锁是在需要的时候才加上的，但并不是不需要了就立刻释放，而是要等到事务结束时才释放。在一个事务中，把最可能发生锁冲突的SQL语句放在最后，减少锁行的时间。

### 8 事务到底是隔离的还是不隔离的？

**可重复读**

事务 T 启动的时候会创建一个视图 read-view，之后事务 T 执行期间，即使有其他事务修改了数
据，事务 T 看到的仍然跟在启动时看到的一样。

一个事务启动的时候，能够看到所有已经提交的事务结果。但是之后，这个事务执行期间，其他事务的更新对它不可见。

<!--more--> 

**当前读VS快照读**

当事务隔离级别是可重复读时

- 快照读： 读取的是事务开启前的数据，比如 select。

  ```sql
  select * from table …
  ```

- 当前读：总是读取已经提交完成的最新版本。特殊的读操作，插入 / 更新 / 删除操作，属于当前读，处理的都是当前的数据，需要加锁。

  ```sql
  select * from table where ? lock in share mode; S 锁，Gap 锁
  
  select * from table where ? for update; X 锁，Gap 锁
  
  insert; X 锁，Gap 锁
  
  update ; X 锁，Gap 锁
  
  delete; X 锁，Gap 锁
  ```

  读操作通常加共享锁（Share locks，S锁，又叫读锁），写操作加排它锁（Exclusive locks，X锁，又叫写锁）；加了共享锁的记录，其他事务也可以读，但不能写；加了排它锁的记录，其他事务既不能读，也不能写。 

### 9 普通索引和唯一索引，应该怎么选择？

Update语句普通索引会用到change buffer 减少磁盘IO，先把数据记录到change buffer,然后当查询的时候触发merge将数据同步到磁盘上，从而达到比唯一索引快的目的。但是针对于更新完后，立即访问对应的数据页，会增加了change buffer维护代价。

### 13 为什么表数据删掉一半，表文件大小不变？

delete删除数据，但是实际上数据页并没有被删除，而是留着被复用。如果要减小文件大小通过 optimize table t 。通过如下程序实验，先通过存储过程idata新增数据然后查询表文件大小，后delete删除后发现表文件大小没变，最后通过optimize table t 来减少了表文件大小。

```sql
begin
declare i int;
set i=1000;
while(i<=10000)do
insert into user values(i, i);
set i=i+1;
end while;
end
```

```sql
select concat(round(sum(DATA_LENGTH/1024/1024),2),'M') from tables;
CALL idata();
select concat(round(sum(DATA_LENGTH/1024/1024),2),'M') from tables;
DELETE FROM `user` where user_id > 6;
select concat(round(sum(DATA_LENGTH/1024/1024),2),'M') from tables;
optimize table `user`;
```

### 15 临时表

建表语法是 

```sql
create temporary table …
```

临时表只能被创建它的 session 访问，所以在这个 session 结束的时候，会自动删除临时表。

### 16 Join使用注意

1. 小标表作为驱动表。
2. 在决定哪个表做驱动表的时候，应该是两个表按照各自的条件过滤，过滤完成之后，计算参与 join 的各个字段的总数据量，数据量小的那个表，就是“小表”，应该作为驱动表。