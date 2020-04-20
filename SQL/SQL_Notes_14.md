# SQL学习笔记14——SQL中其他知识点
## 视图
### 含义
* mysql5.1版本出现的新特性，本身是一个虚拟表，它的数据来自于表，通过执行时动态生成，只保存了sql逻辑，不保存查询结果，和普通表一样使用
* 比如：舞蹈班（领导检查时才生成）和普通班级的对比
### 案例
1. 查询姓张的学生名和专业名
```sql
#保存学生信息和专业信息内连接表为一个视图
CREATE VIEW v1
AS
SELECT stuname,majorname
FROM stuinfo a
INNER JOIN major m
ON s.majorid=m.id;

#从视图中查询姓张的学生信息
SELECT * FROM v1 WHERE stuname LIKE '张%';
```
### 特点
1. 简化sql语句
2. 提高了sql的重用性
3. 保护基表的数据，提高了安全性
4. 应用场景：多个地方用到同样的查询结果且该查询结果使用的sql语句较复杂
### 创建视图
语法
```sql
create view 视图名
as
查询语句;
```
### 修改视图
1. 方式一：
```sql
#该语句其实也可以创建视图
create or replace view 视图名
as
查询语句;
```
2. 方式二：
```sql
alter view 视图名
as
查询语句;
```
### 删除视图
```sql
DROP VIEW 视图名,视图名,...;
```
### 查看视图
```sql
DESC myv3;

SHOW CREATE VIEW myv3;
```
### 更新视图
```sql
CREATE OR REPLACE VIEW myv1
AS
SELECT last_name,email
FROM employees;

#1.插入
INSERT INTO myv1 VALUES('张飞','zf@qq.com');#原始employees表也被插入了这条数据

#2.修改
UPDATE myv1 SET last_name = '张无忌' WHERE last_name = '张飞';#原始表也被改了

#3.删除
DELETE FROM myv1 WHERE last_name = '张无忌';#原始表也被删除了
```
注意：视图一般用于查询的，而不是更新的，所以具备以下特点的视图都不允许更新
①包含分组函数、group by、distinct、having、union 【all】
②join
③常量视图
④where后的子查询用到了from中的表
⑤用到了不可更新的视图
⑥select中包含了子查询
### 视图和表的对比
||创建语法的关键字|是否实际占用物理空间|使用|
|-|-|-|-|
|视图|create view|只是保存了sql逻辑|增删改查，一般不能增删改，也不建议用视图来做增删改
|表|create table|保存了数据|增删改查|
## 变量
### 分类
1. 系统变量
	* 全局变量
		* 作用域：服务器每次启动将会为所有的全局变量赋初始值，针对于所有的会话(连接)有效，但是不能跨重启，想要跨重启需要修改配置文件my.ini
	* 会话变量
		* 作用域：仅仅针对于当前会话（连接）有效
2. 自定义变量
	* 用户变量
	* 局部变量
### 系统变量
说明：变量由系统提供，不是用户定义，属于服务器层面
注意：如果是全局级别，则需要加global，如果是会话级别，则需要加session,如果不写，则默认session
语法：
1. 查看所有的系统变量
```sql
show global|【session】 variables;
```
2. 查看满足条件的部分系统变量
```sql
show global|【session】 variables like '%char%';
```
3. 查看指定的某个系统变量的值
```sql
select @@global.|【session.】系统变量名;
```
4. 为某个系统变量赋值
```sql
#方式1
set global|【session】 系统变量名 = 值;

#方式2
set @@global.|【session.】系统变量名 = 值;
```
### 自定义变量
说明：变量是用户自定义的，不是由系统提供的
使用步骤：
	* 声名
	* 赋值
	* 使用（查看、比较、运算等）
1. 用户变量
	* 作用域：针对于当前会话（连接）有效，同于会话变量的作用域
	* 应用在任何地方，也就是begin end里面或begin end外面
```sql
#①声明并初始化
##方式1
SET @用户变量名=值;

##方式2
SET @用户变量名:=值;

##方式3
SELECT @用户变量名:=值;

####以上方式都可以不赋值，只做声明


#②赋值(更新用户变量的值)
##方式1：通过SET或SELECT
SET @用户变量名=值;#或
SET @用户变量名:=值;#或
SELECT @用户变量名:=值;

##方式2：通过SELECT INTO
SELECT 字段 INTO @用户变量名
FROM 表;


#③使用（查看用户变量的值、比较、运算等）
SELECT @用户变量名;
```
2. 局部变量
	* 作用域：仅仅在定义它的begin end中有效
	* 应用在begin end中的第一句话！！！
```sql
#①声明
DECLARE 变量名 类型;#只声明
DECLARE 变量名 类型 DEFAULT 值;#声明且赋初值


#②赋值
##方式1：通过SET或SELECT
SET 局部变量名=值;#或
SET 局部变量名:=值;#或
SELECT @局部变量名:=值;

##方式2：通过SELECT INTO
SELECT 字段 INTO 局部变量名
FROM 表;


#③使用（查看用户变量的值、比较、运算等）
SELECT 局部变量名;
```
## 存储过程和函数
* 类似于java中的方法
* 好处
	* 提高代码的重用性
	* 简化操作
### 存储过程
* 含义：一组预先编译好的SQL语句的集合，理解成批处理语句
* 好处
	* 提高代码的重用性
	* 简化操作
	* 减少了编译次数并且减少了和数据库服务器的连接次数，提高了效率
#### 创建语法
```sql
CREATE PROCEDURE 存储过程名(参数列表)
BEGIN
	存储过程体(一组合法的SQL语句)
END
```
注意：
1. 参数列表包含三部分：参数模式 参数名 参数类型，举例：IN stuname VARCHAR(20)
2. 参数模式
	* IN:该参数可以作为输入，也就是该参数需要调用方传入值,IN其实可以省略，下面说的OUT、INOUT则不能省略，最好都不省略
	* OUT:该参数可以作为输出，也就是该参数可以作为返回值
	* INOUT:该参数既可以作为输入又可以作为输出，也就是该参数既需要传入值，又可以返回值
3. 如果存储过程体仅仅只有一句话，begin end可以省略，存储过程体中的每条SQL语句的结尾要求必须加分号，存储过程的结尾可以使用DELIMITER重新设置，语法是DELIMITER 结束标记，如DELIMITER $,调用时也得使用新的结束标记，这样做的目的是让程序知道你的存储过程的结束位置,如果多个存储过程想使用相同的结束标记，DELIMITER可以只设置一次,注意这个更改后，后面的语句结束标记也不再是分号，而是自己定义的结束标记，可以自己再次使用DELIMITER ;改回来
4. 调用in模式的参数：call sp1（‘值’）;
调用out模式的参数：set @name; call sp1(@name);select @name;
调用inout模式的参数：set @name=值; call sp1(@name); select @name;
#### 调用语法
```sql
CALL 存储过程名(实参列表);
```
1. 演示调用空参的存储过程
```sql
use girls;
SELECT * FROM admin;
DELIMITER $
CREATE PROCEDURE myp1()
BEGIN
	INSERT INTO admin(username,`password`)
	VALUES('LILY',0000),('ROSE',0000);
END $
CALL myp1()$
SELECT * FROM admin;
```
2. 演示调用in模式参数的存储过程
```sql
#案例1.创建存储过程实现根据女神名查男神信息
CREATE PROCEDURE myp2(IN beautyname VARCHAR(20))
BEGIN
	SELECT bo.*
	FROM boys bo
	RIGHT JOIN beauty b ON bo.id = b.boyfriend_id
	WHERE b.`name`=beautyname;
END $

CALL myp2('小昭')$

#案例2.创建存储过程实现查用户是否登录成功
CREATE PROCEDURE myp3(IN username VARCHAR(20),IN password VARCHAR(20))
BEGIN
	DECLARE result INT DEFAULT 0;#声明并初始化局部变量
	
	SELECT COUNT(*) INTO result#赋值
	FROM admin
	WHERE admin.username = username
	AND admin.password = password;#需要加表限制，否则就近原则都是输入参数(是局部变量)的username、password

	SELECT IF(result>0,'成功','失败');#使用
END $

CALL myp3('张飞','8888')$
```
3. 演示调用out模式参数的存储过程
```sql
#案例1，根据女神名，返回对应的男神名
CREATE PROCEDURE myp4(IN beautyname VARCHAR(20), OUT boyname VARCHAR(20))
BEGIN
	SELECT bo.boyname INTO boyname
	FROM boys bo
	INNER JOIN beauty b ON bo.id = b.boyfriend_id
	WHERE b.`name`=beautyname;
END $


SET @bname$#其实可以省略
CALL myp4('小昭',@bname)$
SELECT @bname$

#案例2，根据女神名，返回对应的男神名和男神魅力值
CREATE PROCEDURE myp5(IN beautyname VARCHAR(20), OUT boyname VARCHAR(20), OUT usercp INT)
BEGIN
	SELECT bo.boyname, bo.usercp INTO boyname, usercp
	FROM boys bo
	INNER JOIN beauty b ON bo.id = b.boyfriend_id
	WHERE b.`name`=beautyname;
END $

CALL myp5('小昭',@bname,@usercp)$
SELECT @bname,@usercp$
```
4. 演示调用inout模式参数的存储过程
```sql
#案例1：传入a和b两个值，最终a和b都翻倍并返回
CREATE PROCEDURE myp8(INOUT a INT, INOUT b INT)
BEGIN
	SET a = a*2;
	SET b = b*2;
END $

SET @m=10$
SET @n=20$
CALL myp8(@m,@n)$
SELECT @m,@n$
```
#### 存储过程的删除
语法：
```sql
DROP PROCEDURE 存储过程名#删存储过程只能一次删除一个
#不允许DROP PROCEDURE p1,p2;
```
#### 查看存储过程的信息
语法：
```sql
SHOW CREATE PROCEDURE myp2;
# 不支持 DESC myp2;这种方式只是查看表，包含虚拟表等，如视图
```
#### 修改存储过程
没办法修改存储过程的begin end内的SQL语句，一般也不去修改存储过程，想要改一般是通过直接删除原来的存储过程，然后重新定义新的存储过程

### 函数
* 含义：一组预先编译好的SQL语句的集合，理解成批处理语句
* 好处
	* 提高代码的重用性
	* 简化操作
	* 减少了编译次数并且减少了和数据库服务器的连接次数，提高了效率
* 区别：存储过程可以有0个返回，也可以有多个返回，适合做批量插入、批量更新；函数有且仅有1个返回，适合做处理数据后返回一个结果
#### 创建语法
```sql
CREATE FUNCTION 函数名(参数列表) RETURNS 返回类型
BEGIN
	函数体
END
```
注意：
* 参数列表包含两部分：参数名 参数类型
* 函数体肯定会有return语句，如果没有会报错，建议return语句放最后
* 当函数体中仅有一句话，则可以省略begin end
* 像存储过程一样使用delimiter语句设置结束标记

#### 调用语法
```sql
SELECT 函数名(参数列表)
```
案例：
1. 无参有返回
```sql
#返回公司的员工个数
CREATE FUNCTION myf1() RETURNS INT
BEGIN
	DECLARE c INT DEFAULT 0;#定义变量
	SELECT COUNT(*) INTO c#赋值
	FROM employees;
	RETURN c;
END $

SELECT myf1()$
```
2. 有参有返回
```sql
#根据员工名，返回它的工资
CREATE FUNCTION myf2(empname VARCHAR(20)) RETURNS DOUBLE
BEGIN
	SET @sal=0;#定义用户变量
	SELECT salary INTO @sal#赋值
	FROM employees
	WHERE last_name = empname;

	RETURN @sal;
END $

SELECT myf2('kochhar')$
```

#### 查看函数
```sql
SHOW CREATE FUNCTION myf3;
```
#### 删除函数
```sql
DROP FUNCTION myf3;
```
