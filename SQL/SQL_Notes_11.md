# SQL学习笔记11——SQL中操作数据之插改删
## 回顾
前面我们学习了SQL中查询数据的一些基础，它们都属于DQL语言，即数据查询语言；接下来我们将学习DML语言，即数据操作语言，主要包含：
* 插入:insert
* 修改:update
* 删除:delete
## 插入语句
### 语法：
```sql
# 方式一：经典的插入
insert into 表名(字段名,...) values(值,...);
# 插入多行
insert into 表名(字段名,...) values(值,...),(值,...),...;
# 方式二
insert into 表名 set 字段=值,字段=值,...;
```
### 特点
1. 插入的值的类型要与列的类型一致或兼容
2. 不可以为NULL的列必须插入值
3. 可以为NULL的列有以下两种方式插入值
	* 插入NULL值
	* 直接省略，默认以NULL填充
4. 列的顺序可以交换，只要值得顺序与之对应即可，这当然就要求了列数和值的个数必须一致
5. 可以省略列名，默认则是所有列，而且列的顺序和表中列的顺序一致
6. 方式一做子查询时必须省略value,方式如下
	```sql
	INSERT INTO customers ( cust_id, cust_name )
	SELECT 1211111121,'1211111121';
	```
### 两种插入方式的对比
||是否支持插入多行|是否支持子查询|
|-|-|-|
|方式一|YES|YES|
|方式二|NO|NO|

## 修改数据
### 修改单表的记录★
#### 语法
```sql
update 表名 set 字段=值,字段=值 【where 筛选条件】;
```
### 修改多表的记录
#### 语法
```sql
# sql92语法
update 表1 别名,表2 别名
set 列=值,...
where 连接条件
and 筛选条件;

# sql99语法
update 表1 别名 
left|right|inner join 表2 别名 
on 连接条件  
set 字段=值,字段=值,... 
【where 筛选条件】;
```
#### 案例
1. 修改没有男朋友的女神的男朋友编号都为2号
```sql
UPDATE boys bo
RIGHT JOIN beauty b
ON bo.id = b.boyfriend_id
SET b.boyfriend_id = 2
WHERE b.id IS NULL;
```
## 删除数据
### 方式一：使用delete
#### 删除单表的记录★
语法:
```sql
delete from 表名 【where 筛选条件】【limit 条目数】
```
案例：
1. 删除手机号以9结尾的女神信息
	```sql
	DELETE FROM beauty WHERE phone LIKE '%9';
	```
#### 删除多表的记录
语法:
```sql
#sql92
delete 表1的别名,表2的别名#总之，这里写要删数据的表的别名
from 表1 别名,表2 别名
where 连接条件
and 筛选条件;


#sql99
delete 表1的别名,表2的别名#总之，这里写要删数据的表的别名
from 表1 别名
inner|left|right join 表2 别名
on 连接条件
where 筛选条件;
```
案例:
1. 删除张无忌的女朋友的信息
	```sql
	DELETE b
	FROM beauty b
	INNER JOIN boys bo
	ON b.boyfriend_id = bo.id
	WHERE bo.boyName = '张无忌';
	```
### 方式二：使用truncate
语法:
```sql
#整表删除
truncate table 表名;
```
### deleta vs truncate
||可否加where|效率|删除后再插入数据自增长列的值起始值|是否有返回值|可否回滚
|-|-|-|-|-|-|
delete|YES|低一点|从断点开始|有|可以
truncate|NO|高一点|从1开始|无|不能