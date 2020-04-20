# SQL学习笔记7——SQL中检索数据之连接查询
## 含义
连接查询又称多表查询，当查询的字段来自于多个表时，就会用到连接查询
## 语法
```mysql
select 字段1，字段2
from 表1，表2,...;
```
* 笛卡尔乘积：当查询多个表时，没有添加有效的连接条件，导致多个表所有行实现完全连接
* 如何解决：添加有效的连接条件
	```mysql
	select 字段1，字段2
	from 表1，表2,...
	where 连接条件;
	```
	例子
	```mysql
	SELECT name,boyname
	FROM boys,beauty
	WHERE beauty.boyfriend_id=boys.id;
	```
## 分类

* 按年代分类：
	* sql92：仅仅支持内连接
	* sql99【推荐使用】：支持内连接+外连接（左外和右外）+交叉连接
* 按功能分类
	* 内连接：等值 非等值 自连接
	* 外连接：左外 右外 全外 交叉连接
## sql92语法
1. 等值连接
	语法：
	```mysql
	select 查询列表
	from 表1 别名,表2 别名
	where 表1.key=表2.key
	【and 筛选条件】
	【group by 分组字段】
	【having 分组后的筛选】
	【order by 排序字段】
	```
	特点：
	①一般为表起别名：提高语句的简洁度、区分多个重名的字段
	②多表的顺序可以调换
	③n表连接至少需要n-1个连接条件
	④等值连接的结果是多表的交集部分
	⑤如果为表起了别名，则查询的字段就不能使用原来的表名去限定
	⑥可以搭配前面介绍的所有子句使用，如排序、分组、筛选
2. 非等值连接
	语法：
	```mysql
	select 查询列表
	from 表1 别名,表2 别名
	where 非等值的连接条件
	【and 筛选条件】
	【group by 分组字段】
	【having 分组后的筛选】
	【order by 排序字段】
	```
3. 自连接
	语法：
	```mysql
	select 查询列表
	from 表 别名1,表 别名2
	where 等值的连接条件
	【and 筛选条件】
	【group by 分组字段】
	【having 分组后的筛选】
	【order by 排序字段】
	```
## SQL99语法
### 语法
```mysql
select 查询列表
from 表1 别名 [连接类型]
join 表2 别名
on 连接条件
[where 筛选条件]
[group by 分组列表]
[having 分组后的筛选]
[order by 排序列表]
[limit 子句];
/*
内连接:连接类型是inner
外连接：
左外：left [outer]
右外：right [outer]
全外：full [outer]
交叉连接: cross
*/
```
### 内连接
```mysql
select 查询列表
from 表1 别名
inner join 表2 别名
on 连接条件
[where 筛选条件]
[group by 分组列表]
[having 分组后的筛选]
[order by 排序列表]
[limit 子句];
```
1. 等值连接
查询员工名、部门名
	```mysql
	SELECT last_name,department_name
	FROM departments d
	INNER JOIN employees e
	on e.department_id = d.department_id
	```
2. 非等值连接
连接条件修改为相应的非等值连接条件即可
3. 自连接
	```mysql
	SELECT e.last_name,m.last_name
	FROM employees e
	JOIN employees m
	ON e.manager_id=m.employee_id;
	```
#### 特点
- 可添加排序、分组、筛选
- inner可以省略
- 筛选条件放在where后面，连接条件放在on后面，提高分离性，便于阅读
- inner join的表必须和from的表有连接条件
### 外连接
#### 特点
- 查询的结果=主表中所有的行，如果从表和它匹配的将显示匹配行，如果从表没有匹配的则显示null
- left join 左边的就是主表，right join 右边的就是主表
  full join 两边都是主表
- 一般用于查询除了交集部分的剩余的不匹配的行
- 左外和右外交换两个表的顺序，可以实现相同的效果
- 全外连接=内连接的结果+表1中有但表2中没有的+表2中有但表1中没有的(没有的用null填充)
- MySQL中不支持全外连接
#### 例子：
1. 查询男朋友不在男神表的女神名
	```mysql
	USE girls;
	SELECT b.name,bo.*
	FROM beauty b
	LEFT OUTER JOIN boys bo
	ON b.boyfriend_id=bo.id
	WHERE bo.id IS NULL;
	/*
	或者按下面等价的方式
	USE girls;
	SELECT b.name,bo.*
	FROM boys bo
	RIGHT OUTER JOIN beauty b
	ON b.boyfriend_id=bo.id
	WHERE bo.id IS NULL;
	*/
	```
### 交叉连接
1. 语法：
```mysql
select 查询列表
from 表1 别名
cross join 表2 别名;
```
2. 特点：
相当于笛卡尔乘积

## SQL92 vs SQL99

||功能|可读性|推荐|
|-|-|-|-|
sql92|少|一般|不推荐
sql99|多|实现了连接条件和筛选条件的分离，可读性较高|推荐
