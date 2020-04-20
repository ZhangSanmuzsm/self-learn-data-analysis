# SQL学习笔记5——SQL中的常见函数
## 一、概述
功能：类似于java中的方法，将一组逻辑语句封装在方法体中，对外暴露方法名
好处：提高代码重用性和隐藏实现细节
调用：`select 函数名(实参列表) [from 表名];`
特点：1.叫什么（函数名）2.干什么（函数功能）
分类：1.单行函数 2.分组函数（传递一组值，一般做统计使用，又称为统计函数、聚合函数、组函数）
## 单行函数
### 字符函数
- concat:连接
- substr/substring:截取子串
	```mysql
	#截取从指定索引处后面所有字符
	SELECT SUBSTR('李莫愁爱上了陆展元',7);
	#返回'陆湛远'
	#注意：索引从1开始

	#截取从指定索引处指定字符长度的字符（注意：不是字节长度）
	SELECT SUBSTR('李莫愁爱上了陆展元',1,3);
	#返回'李莫愁'
	SELECT SUBSTR('haha',2,3);
	#返回'aha'
	```
- upper:变大写
- lower：变小写
- replace：替换
	```mysql
	SELECT REPLACE('周芷若张无忌爱上了周芷若','周芷若','赵敏') AS out_put;
	#返回'赵敏张无忌爱上了赵敏'
	```
- length：获取字节长度（注意：不是字符长度）
	```mysql
	SELECT LENGTH('john');
	SELECT LENGTH('张三丰hahaha');
	SHOW VARIABLES LIKE '%char%';
	/*
	utf8或者utf8mb4编码下1个汉字3个字节
	gbk编码下1个汉字2个字节
	*/
	```
- trim:去前后空格
	```mysql
	SELECT TRIM('    张翠山     ') AS out_put;
	SELECT TRIM('a' FROM 'aaaaaaaaa张翠山aaaaaaaa') AS out_put;
	```
- lpad：用指定的字符实现左填充指定字符长度
	```mysql
	SELECT LPAD('殷素素',10,'*') AS out_put;
	#返回'*******殷素素'
	SELECT LPAD('殷素素',2,'*') AS out_put;
	#返回'殷素',说明超过了指定长度会被截断
	```
- rpad：右填充(用法和lpad相同)
- instr:获取子串第一次出现的索引,如果找不到返回0
	```mysql
	SELECT INSTR('杨不悔爱上了殷六侠','殷六侠') AS out_put;
	#返回7
	```
### 数学函数
- ceil:向上取整，返回>=该参数的最小整数
- round：四舍五入
	```mysql
	SELECT ROUND(1.65);
	#返回2
	SELECT ROUND(-1.65);
	#返回-2
	SELECT ROUND(1.567,2);
	#返回1.57
	```
- mod:取模，等价于a-a/b*b，所以结果的正负号和被除数a的正负号一致
- floor：向下取整，返回<=该参数的最大整数
- truncate:截断
	```mysql
	SELECT TRUNCATE(1.699,1);
	#返回1.6
	#第二个参数的数字代表截断保留几位小数
	```
- rand:获取随机数，返回0-1之间的小数
### 日期函数
- now：返回当前日期+时间
- year:返回年
	```mysql
	SELECT YEAR('1998-1-1');
	#返回1998
	```
- month：返回月
- day:返回日
- date_format:将日期转换成字符
	```mysql
	SELECT DATE_FORMAT('1999-4-3','%Y年%m月%d日') AS out_put;
	#返回'1999年04月03日'
	```
- curdate:返回当前日期
- str_to_date:将字符转换成日期
	```mysql
	SELECT STR_TO_DATE('9-13-1999','%m-%d-%Y');
	#返回'1999-09-13'
	```
	格式符 | 功能
	-|-
	%Y | 4位的年份
	%y | 2位的年份
	%m | 月份(01,02,...,11,12)
	%c | 月份(1,2,...,11,12)
	%d | 日(01,02,...)
	%H | 小时(24小时制)
	%h | 小时(12小时制)
	%i | 分钟(00,01,...,59)
	%s | 秒(00,01,...,59)
- curtime：返回当前时间
- hour:小时
- minute:分钟
- second：秒
- datediff:返回两个日期相差的天数
	```mysql
	SELECT DATEDIFF('1999-4-5','1999-4-4') AS out_put;
	#返回1
	SELECT DATEDIFF('1999-4-3','1999-4-4') AS out_put;
	#返回-1
	```
- monthname:以英文形式返回月
### 其他函数
- version 当前数据库服务器的版本
- database 当前打开的数据库
- user当前用户
- md5('字符'):返回该字符的md5加密形式
### 流程控制函数
- if(条件表达式，表达式1，表达式2)：如果条件表达式成立，返回表达式1，否则返回表达式2
- switch-case
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
## 分组函数
### 分类
- max 最大值
- min 最小值
- sum 和
- avg 平均值
- count 计算个数(非NULL值的个数总计)
- 例子
	```mysql
	SELECT SUM(salary) FROM employees;
	SELECT AVG(salary) FROM employees;
	SELECT MIN(salary) FROM employees;
	SELECT MAX(salary) FROM employees;
	SELECT COUNT(salary) FROM employees;
	```
### 特点
1. sum、avg一般用于处理数值型，max、min、count可以处理任何类型
2. 以上分组函数都忽略null值
3. 可以和distinct搭配实现去重的运算
4. `count(*)`:用于统计总行数
5. `count(1)`:相当于加入全1的一列，然后计数，所以还是用于统计总的行数
6. 效率上：MyISAM存储引擎，`count(*)`最高，因为内部寄存器直接存了`count(*)`的值；InnoDB存储引擎，`count(*)`和`count(1)`效率差不多，但大于`count(字段)`
7. 和分组函数一同查询的字段，要求是group by后出现的字段（后面会介绍）