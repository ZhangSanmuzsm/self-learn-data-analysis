# SQL学习笔记10——SQL中检索数据之联合查询
## 含义
union：合并、联合，将多次查询结果合并成一个结果
## 语法
查询语句1
union 【all】
查询语句2
union 【all】
...
## 案例
1. 查询部门编号>90或邮箱包含a的员工信息
	```sql
	SELECT * FROM employees WHERE email LIKE '%a%' OR department_id>90;
	```
	等价于
	```sql
	SELECT * FROM employees WHERE email LIKE '%a%'
	UNION
	SELECT * FROM employees WHERE department_id>90;
	```
## 意义
1. 将一条比较复杂的查询语句拆分成多条语句
2. 尤其适用于要查询的结果来自多个表，且多个表没有直接的连接关系，但查询的信息一致时
3. 要求多条查询语句查询的列数是一致的
4. 要求多条查询语句查询的每一列的类型和顺序最好是一致的，并且最后合并显示的列名会是第一个查询语句查询的列名
5. union关键字默认去重，要保留重复项，可以使用union all

## 查询总结
语法：
```sql
select 查询列表    ⑦
from 表1 别名       ①
连接类型 join 表2   ②
on 连接条件         ③
where 筛选          ④
group by 分组列表   ⑤
having 筛选         ⑥
order by排序列表    ⑧
limit 起始条目索引，条目数;  ⑨
```