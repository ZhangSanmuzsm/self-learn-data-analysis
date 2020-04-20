# SQL学习笔记6——SQL中检索数据之分组查询
## 语法
```mysql
select 分组函数，分组后的字段
from 表
[where 分组前的筛选]
group by 分组的字段
[having 分组后的筛选]
[order by 排序列表]
```
注意：查询列表必须特殊，要求是分组函数和group by后出现的字段
* group by 分组字段，然后如果查非分组字段默认分组后第一条，没实际意义，只有聚合函数才有意义
## 案例
* 查询每个工种的最高工资
	```mysql
	SELECT MAX(salary),job_id
	FROM employees
	GROUP BY job_id;
	```
* 查询邮箱中包含a字符的每个部门的平均工资
	```mysql
	SELECT AVG(salary),department_id
	FROM employees
	WHERE email LIKE '%a%'
	GROUP BY department_id;
	```
* 查询哪个部门的员工个数>2
	```mysql
	SELECT COUNT(*),department_id
	FROM employees
	GROUP BY department_id
	HAVING COUNT(*)>2;
	```
* 查询每个部门每个工种的员工的平均工资
	```mysql
	SELECT AVG(salary),department_id,job_id
	FROM employees
	GROUP BY job_id,department_id
	/*
	这里GROUP BY后面字段的顺序不影响结果
	*/
	```
## 特点
||	使用关键字	|	筛选的表|	位置
-|-|-|-
分组前筛选|	where		|	原始表	|	group by的前面
分组后筛选|	having		|分组后的结果|	group by的后面

* 分组函数做条件，肯定是放在having子句中
* 能用分组前筛选的优先考虑分组前筛选
* 可以用表达式、函数、别名进行分组
* 可以按多个字段分组，多个字段之间用逗号隔开，没有顺序要求
* 也可以添加排序，排序放在整个分组查询的最后