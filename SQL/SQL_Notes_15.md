# SQL学习笔记15——SQL的流程控制结构
## 分类
1. 顺序结构：程序从上往下依次执行
2. 分支结构：程序从两条或多条路径中选择一条去执行
3. 循环结构：程序在满足一定条件的基础上，重复执行一段代码
## 分支结构
1. if函数
	* 功能：实现简单的双分支
	* 语法
	```sql
	IF(表达式1,表达式2，表达式3)
	#执行顺序：如果表达式1成立，则IF函数返回表达式2的值，否则返回表达式3的值
	```
	* 应用：任何地方
2. case结构
	* 情况1：类似于java中的switch语句，一般用于实现等值判断
	* 情况2：类似于jave中的多重IF语句，一般用于实现区间判断
		- case情况1
		```mysql
		case 变量或表达式或字段
		when 常量1 then 值1或语句1;/*值的话末尾不要加分号*/
		when 常量2 then 值2或语句2;
		...
		else 值n或语句n;
		end case;/*值的话末尾不要加case*/
		```
		- case情况2
		```mysql
		case 
		when 条件1 then 值1或语句1;/*值的话末尾不要加分号*/
		when 条件2 then 值2或语句2;
		...
		else 值n或语句n;
		end case;/*值的话末尾不要加case*/
		/* 这种方式可以判断位于某个区间这种条件,而case情况1不行 /*
		```
	* 特点：可以作为表达式，嵌套在其他语句中使用，可以放在任何地方，可以作为独立的语句去使用，此时只能放在begin end中
	* 如果when中的值满足或条件成立，则执行对应的then后面的语句，并且结束case，如果都不满足，则执行else中的语句或值
	* else可以省略，如果else省略了，并且所有的when条件都不满足，则返回NULL
	* 作为独立的语句去使用，放在begin end中的一个案例
	```sql
	CREATE PROCEDURE test_case(IN score INT)
	BEGIN
		CASE
		WHEN score>=90 AND score<=100 THEN SELECT 'A';
		WHEN score>=80 THEN SELECT 'B';
		WHEN score>=60 THEN SELECT 'C';
		ELSE SELECT 'D';
		END CASE;
	END $

	CALL test_case(95)$
	```
3. if结构
	* 功能：实现多重分支
	* 语法
	```sql
	if 条件1 then 语句1;
	elseif 条件2 then 语句2;
	...
	【else 语句n;】
	end if;
	```
	* 只能应用在begin end中
	* 案例
	```sql
	CREATE FUNCTION test_if(score INT) RETURNS CHAR
	BEGIN
		IF score>=90 AND score<=100 THEN RETURN 'A';
		ELSEIF score>=80 THEN RETURN 'B';
		ELSEIF score>=60 THEN RETURN 'C';
		ELSE RETURN 'D';
		END IF;
	END $

	SELECT test_if(86)$
	```
## 循环结构
### 分类
循环类型：
1. while
2. loop
3. repeat

循环控制：
1. iterate类似于continue，继续，结束本次循环，继续下一次
2. leave类似于break，跳出，结束当前所在的循环

位置：
都只能放在begin end中
### while
* 语法
```sql
【标签:】 while 循环条件 do
	循环体;
end while 【标签】;
```
标签是用来做循环控制的，配合iterate和leave使用
### loop
* 语法
```sql
【标签:】 loop
	循环体;
end loop 【标签】;
```
* 可以用来模拟简单的死循环
* 如果想要退出，就得搭配leave
### repeat
* 语法
```sql
【标签:】 repeat
	循环体;
until 结束循环的条件
end repeat 【标签】;
```
有点类似do while
### 案例
1. 批量插入，根据次数插入到admin表中多条记录
```sql
CREATE PROCEDURE pro_while1(IN insertcount INT)
BEGIN
	DECLARE i INT DEFAULT 1;
	WHILE i<=insertcount DO
		INSERT INTO admin(username,password) VALUES(CONCAT('Rose',i),'666');
		SET i=i+1;
	END WHILE;
END $

CALL pro_while1(100)$
```
2. 添加leave语句：批量插入，根据次数插入到admin表中多条记录，如果次数大于20则停止
```sql
TRUNCATE TABLE admin$
CREATE PROCEDURE test_while1(IN insertcount INT)
BEGIN
	DECLARE i INT DEFAULT 1;
	a:WHILE i<=insertcount DO
		INSERT INTO admin(username,password) VALUES(CONCAT('xiaohua',i),'0000');
		IF i>=20 THEN LEAVE a;
		END IF;
		SET i=i+1;
	END WHILE a;
END $

CALL test_while1(100)$
```
3. 添加iterate语句:批量插入，根据次数插入到admin表中多条记录，只插入偶数次
```sql
TRUNCATE TABLE admin$
DROP PROCEDURE test_while1$
CREATE PROCEDURE test_while1(IN insertcount INT)
BEGIN
	DECLARE i INT DEFAULT 0;
	a:WHILE i<=insertcount DO
		SET i=i+1;
		IF MOD(i,2)!=0 THEN ITERATE a;
		END IF;

		INSERT INTO admin(username,password) VALUES(CONCAT('xiaohua',i),'0000');

	END WHILE a;
END $

CALL test_while1(100)$
```