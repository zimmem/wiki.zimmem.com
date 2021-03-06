---
layout: post
title:  事务
description: 
tags: Transaction spring
---

# [{{ page.title }}][1]

事务特性
-------

- 原子性 (atomicity)：  
	事务中的操作要么全部成功， 要么全部失败。  
- 一致性 (consistency)：  
	一致性指事务将数据库从一种状态转变为下一种一致的状态。 在事务开始之前和事务结束以后，数据库的完整性约束没有被破坏。
- 隔离性 (isolation)：  
	一个事务的影响在该事务提交前对其他事务都不可见
- 持久性 (durability)：  
	事务一旦提交，其结果就是永久性的。

事务的隔离级别
------------

### 数据库操作过程中很  事务隔离级别可能出现以下几种不确定情况

- 更新丢失（Lost update） 
	两个事务都同时更新一行数据，但是第二个事务却中途失败退出，导致对数据的两个修改都失效了。这是因为系统没有执行任何的锁操作，因此并发事务并没有被隔离开来。
- 脏读（Dirty Reads）  
	一个事务开始读取了某行数据，但是另外一个事务已经更新了此数据但没有能够及时提交。这是相当危险的，因为很可能所有的操作都被回滚。
- 不可重复读（Non-repeatable Reads）
	一个事务对同一行数据重复读取两次，但是却得到了不同的结果。它包括以下情况：  
　　
	1. 事务T1读取某一数据后，事务T2对其做了修改，当事务T1再次读该数据时得到与前一次不同的值。  
	2. 幻读（Phantom Reads）：事务在操作过程中进行两次查询，第二次查询的结果包含了第一次查询中未出现的数据或者缺少了第一次查询中出现的数据（这里并不要求两次查询的SQL语句相同）。这是因为在两次查询过程中有另外一个事务插入数据造成的。

### 为了避免上面出现的几种情况，在标准SQL规范中，定义了4个事务隔离级别

- READ UNCOMMITED  
	允许脏读取，但不允许更新丢失。如果一个事务已经开始写数据，则另外一个数据则不允许同时进行写操作，但允许其他事务读此行数据。该隔离级别可以通过“排他写锁”实现。
- READ COMMITED  
	允许不可重复读取，但不允许脏读取。这可以通过“瞬间共享读锁”和“排他写锁”实现。读取数据的事务允许其他事务继续访问该行数据，但是未提交的写事务将会禁止其他事务访问该行。
- REPEATABLEREAD  
	可重复读取（Repeatable Read）：禁止不可重复读取和脏读取，但是有时可能出现幻影数据。这可以通过“共享读锁”和“排他写锁”实现。读取数据的事务将会禁止写事务（但允许读事务），写事务则禁止任何其他事务。
- SERIALIZABLE  
	提供严格的事务隔离。它要求事务序列化执行，事务只能一个接着一个地执行，但不能并发执行。如果仅仅通过“行级锁”是无法实现事务序列化的，必须通过其他机制保证新插入的数据不会被刚执行查询操作的事务访问到。

Spring 事务
----------

	


[1]:    {{ page.url}}  ({{ page.title }})