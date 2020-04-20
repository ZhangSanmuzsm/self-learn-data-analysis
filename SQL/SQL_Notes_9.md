# SQL学习笔记9——SQL中检索数据之分页查询
## 应用场景
当要查询的条目数太多，一页显示不全，需要分页提交sql请求
## 语法
```mysql
SELECT 查询列表
FROM 表1
[连接类型 JOIN 表2
ON 连接条件
WHERE 筛选条件
GROUP BY 分组条件
HAVING 分组后的筛选
ORDER BY 排序字段]
LIMIT offset,size;
/*
offset代表的是起始的条目索引，默认从0开始
size代表的是显示的条目数
*/
```
## 案列
1. 查询前5条员工信息
```mysql
SELECT * FROM employees LIMIT 0,5;
#等价于SELECT * FROM employees LIMIT 5;
```
2. 查询第11条到第25条员工信息
```mysql
SELECT * FROM employees LIMIT 10,15;
```
## 特点
1. limit语句放在查询语句的最后
2. 公式：要显示的页数page，每页的条目数size
	```mysql
	select 查询列表
	from 表
	limit (page-1)*size,size; 
	```