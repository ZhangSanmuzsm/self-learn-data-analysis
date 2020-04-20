# SQL学习笔记13——SQL中事务控制语言TCL
* 事务控制语言TCL：Transaction Control Language
* 事务：一条或多条sql语句组成一个执行单位，一组sql语句要么都执行要么都不执行
* MySQL中的存储引擎：
	* 概念：在MySQL中的数据用各种不同的技术存储在文件(或内存)中
	* 通过show engines;来查看MySQL支持的存储引擎
	* 在MySQL中用得最多的存储引擎有：innodb，myisam，memory等，其中innodb支持事务，而myisam，memory等不支持事务
## 特点（ACID）
- A 原子性：一个事务是不可再分割的整体，要么都执行要么都不执行
- C 一致性：一个事务可以使数据从一个一致状态切换到另外一个一致的状态
- I 隔离性：一个事务不受其他事务的干扰，多个事务互相隔离的
- D 持久性：一个事务一旦提交了，则永久的持久化到本地
## 分类
- 隐式（自动）事务：没有明显的开启和结束，本身就是一条事务可以自动提交，比如insert、update、delete
- 显式事务：具有明显的开启和结束，前提：必须先设置自动提交功能为禁用(set autocommit=0;只针对当前事务有效，也就是每次都得set一下)
## 事务的使用步骤
* 使用显式事务：
	1. 开启事务
	```sql
	set autocommit=0;
	start transaction;#可以省略
	```
	2. 编写事务中的sql语句（select insert update delete）（注意：DDL语言，drop等没有事务之说），设置回滚点：savepoint 回滚点名;
	3. 结束事务：commit;提交事务 rollback;回滚事务 rollback to 回滚点名;回滚到指定的地方
## 演示事务的使用步骤
```sql
#开启事务
set autocommit=0;
start transaction;
#编写事务中的sql语句
UPDATE account SET balance=500 WHERE username='张无忌';
UPDATE account SET balance=1500 WHERE username='赵敏';
#结束事务
COMMIT;

#开启事务
set autocommit=0;
start transaction;
#编写事务中的sql语句
UPDATE account SET balance=1000 WHERE username='张无忌';
UPDATE account SET balance=1000 WHERE username='赵敏';
#回滚事务
ROLLBACK;
```
## 并发事务
1. 事务的并发问题是如何发生的？
多个事务同时操作同一个数据库的相同数据时
2. 并发问题都有哪些？
脏读：一个事务读取了其他事务还没有提交的数据，读到的是其他事务“更新”的数据
不可重复读：一个事务多次读取，结果不一样
幻读：对于两个事务T1,T2,T1从一个表中读取了一个字段，然后T2在该表中插入了一些新的行，之后，如果T1再次读取同一个表，就会多出几行
3. 如何解决并发问题
通过设置隔离级别来解决并发问题
## 演示事务的隔离级别
* select @@transaction_isolation;查看当前默认隔离级别
* set 【session】 transaction isolation level read uncommitted设置当前mysql连接的隔离级别为read uncommitted（省略session可能会导致不能立马生效）（也可以用set 【session】 transaction_isolation = 'read-uncommitted',后面讲变量时会讲解 ）
* set global transaction isolation level read committed;设置数据库系统的全局的隔离级别（一般需要重启才生效）

||脏读|不可重复读|幻读|备注|
|-|-|-|-|-|
|read uncommitted（读未提交数据）|×|×|×|允许事务读取未被其他事务提交的变更，脏读、不可重复读和幻读的问题都会出现|
|read committed（读已提交数据）|√|×|×|只允许事务读取已经被其他事务提交的变更|
|repeatable read（可重复读）|√|√|×|确保事务可以多次从一个字段中读取相同的值，在这个事务持续期间，禁止其他事务对这个字段进行更新|
|serializable（串行化、序列化）|√|√|√|确保事务可以从一个表中读取相同的行，在这个事务持续期间，禁止其他事务对该表执行插入、更新和删除操作，所有并发问题都可以避免，但性能十分低下|

## 演示savepoint的使用
```sql
SET autocommit=0;
START TRANSACTION;
DELETE FROM account WHERE id=25;
SAVEPOINT a;#设置保存点
DELETE FROM account WHERE id=28;
ROLLBACK TO a;#回滚到保存点
```
## 演示delete和truncate在事务使用时的区别
```sql
SET autocommit=0;
START TRANSACTION;
DELETE FROM account;
ROLLBACK;
#delete可以回滚
SET autocommit=0;
START TRANSACTION;
TRUNCATE TABLE account;
ROLLBACK;
#truncate没法回滚了
```