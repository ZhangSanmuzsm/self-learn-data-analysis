# SQL学习笔记3——SQL中检索数据之条件查询

## 完整语法
* `select 列名 from 表名 where 筛选条件;`
## 筛选条件的分类
1. 简单条件运算符
	* `> < = <> != >= <=` 其中`<>`和`!=`是一个意思
	* `<=>` 安全等于，相当于`=`和`is`的合并，但可读性差，不建议使用
2. 逻辑运算符
	* `&& and || or !  not`
	* SQL在处理OR操作符前，优先处理AND操作符，所以好的习惯是总是使用圆括号显示指明顺序，这样做能消除歧义
3. 模糊查询
	* `like`:一般搭配通配符使用，可以判断字符型或数值型，通配符：`%`任意多个字符(包含0个)，`_`任意单个字符
	* `between and`相当于`[a,b]`闭区间，调换临界值大小关系则结果可能为空
	* `in`相当于`= 第一个 or = 第二个 ...`,所以不能搭配正则匹配使用
	* `is null /is not null`:用于判断null值(不能使用`= null`代替此功能，`is`也不能代替`=`，`is`只用于和`null`搭配)
	* 查询员工名中包含字符`a`的员工信息
	`SELECT * FROM employees WHERE last_name LIKE '%a%';`
	* 查询员工名中第二个字符为_的员工名
	```sql
	SELECT 
		last_name 
	FROM 
		employees 
	WHERE 
		last_name LIKE '_$_%' ESCAPE '$';
	/* 或者之间用\转义，但是推荐上面的方式，这样的转义标识符可以由自己任意定义
	SELECT 
		last_name 
	FROM 
		employees 
	WHERE 
		last_name LIKE '_\_%';
	*/
	```
	* 查询员工作是指定的某些特定工作的员工名及其工作
	```sql
	SELECT 
		last_name,
		job_id
	FROM 
		employees 
	WHERE 
		job_id IN('IT_PROT','AD_VP','AD_PRES');
	```
## 经典问题
1. `select * from 表名;` 与 `select * from 表名 where id like '%%';`的区别是是什么？
	* 前者检索所有，后者检索除`null`类型外的所有