# SQL学习笔记8——SQL中检索数据之子查询
## 含义
* 嵌套在其他语句内部的select语句称为子查询或内查询，
* 外面的语句可以是insert、update、delete、select等，一般select作为外面语句较多
* 外面如果为select语句，则此语句称为外查询或主查询
## 分类
1. 按出现位置
	* select后面：仅仅支持标量子查询
	* from后面：支持表子查询
	* where或having后面：标量子查询、列子查询、行子查询
	* exists后面（相关子查询）：标量子查询、列子查询、行子查询、表子查询，exists()相当于一个函数，存在则返回1否则返回0，结果可作为布尔值做逻辑运算
2. 按结果集的行列
	* 标量子查询（单行子查询）：结果集为一行一列
	* 列子查询（多行子查询）：结果集为多行一列
	* 行子查询：结果集为多行多列，一般为一行多列
	* 表子查询：结果集为多行多列
## 示例
1. where或having后面
	* 特点
		* 子查询放在小括号内
		* 子查询一般放在条件的右侧
		* 标量子查询，一般搭配单行操作符使用< > >= <= = <>
		* 列子查询，一般搭配多行操作符使用IN ANY/SOME ALL
		* 子查询的执行优先于主查询，主查询的条件用到了子查询的结果
	* 案例
		* 查询谁的工资比Abel高？
		```mysql
		#标量子查询
		USE myemployees;
		SELECT
			* 
		FROM
			employees 
		WHERE
			salary > ( SELECT salary FROM employees WHERE last_name = 'Abel' );
		```
		* 返回location_id是1400或1700的部门编号
		```mysql
		# 列子查询
		SELECT
			last_name 
		FROM
			employees 
		WHERE
			department_id IN ( SELECT DISTINCT department_id FROM departments WHERE location_id IN ( 1400, 1700 ) );
		```
		* 查询员工编号最小并且工资最高的员工信息
		```mysql
		#行子查询
		SELECT *
		FROM employees
		WHERE (employee_id,salary)=(
			SELECT MIN(employee_id),MAX(salary)
			FROM employees
		);
		```
2. select后面
比较容易理解，但要注意仅仅支持标量子查询
3. from后面
将子查询结果充当一张表，要求必须起别名
4. exists后面（相关子查询）
	* 查询没有女朋友的男神信息
	```mysql
	SELECT bo.*
	FROM boys bo
	WHERE NOT EXISTS(
		SELECT boyfriend_id
		FROM beauty b
		WHERE bo.id=b.boyfriend_id
	);
	```