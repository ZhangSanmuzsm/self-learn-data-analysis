# python笔记2——python基础知识
## 输入输出
任何计算机程序都是为了执行一个特定的任务，有了输入，用户才能告诉计算机程序所需的信息，有了输出，程序运行后才能告诉用户任务的结果。

输入是Input，输出是Output，因此，我们把输入输出统称为Input/Output，或者简写为IO。

`input()`和`print()`是在命令行下面最基本的输入和输出，但是，用户也可以通过其他更高级的图形界面完成输入和输出，比如，在网页上的一个文本框输入自己的名字，点击“确定”后在网页上看到输出信息。
## 语法
* python使用缩进来组织代码块，请务必遵守约定俗成的习惯，坚持使用4个空格的缩进。在文本编辑器中，需要设置把Tab自动转换为4个空格，确保不混用Tab和空格。
* python使用#开头表示注释
## 数据类型
1. 数值型
	* 整数：1 2 ...
	* 浮点数：1.1 1.2 ...
2. 字符型
	* 字符串：字符串是以单引号'或双引号"括起来的任意文本，比如'abc'，"xyz"等等。
	* 如果'本身也是一个字符，那就可以用""括起来，比如"I'm OK"
	* 如果字符串内部既包含'又包含"怎么办？可以用转义字符\来标识
	* 转义字符\可以转义很多字符，比如\n表示换行，\t表示制表符，字符\本身也要转义，所以`\\`表示的字符就是\
	* 如果字符串里面有很多字符都需要转义，就需要加很多\，为了简化，Python还允许用r''表示''内部的字符串默认不转义
	* 如果字符串内部有很多换行，用\n写在一行里不好阅读，为了简化，Python允许用'''...'''的格式表示多行内容
3. 布尔型
	* 布尔值只有True、False两种值
	* 布尔值可以用and、or和not运算
	* is和==之间有差别
		* Python中，万物皆对象
		* 每个对象包含3个属性，id，type，value
		* id就是对象地址，可以通过内置函数id()查看对象引用的地址
		* type就是对象类型，可以通过内置函数type()查看对象的类型
		* value就是对象的值
		* 当你想判断a is b的时候，实际上比较的是id(a)==id(b)
		* 所以is判断为ture的时候，它们的类型和值都一定也是一样的，即它们完全一样，实际上就是在计算机内存中是同一个东西
		* 也就是is用于比较id是否相同，==用于比较值是否相同，所以is判断为ture的，==判断也一定为true，反之则不一定
		* 另外还有is的否定is not语句
		* 由于is是比较id，所以is会比==效率高一点（==比较值可能还会做隐式类型转化）
		* 但是is过于苛刻，未必是我们编程时想要的效果，一般is只搭配True、False、None使用
		* 更多细节见[python中is 和== 的区别是啥?](https://www.zhihu.com/question/20179728)
	* in的使用
	```python
	>>> fruit = 'banana'
	>>> 'n' in fruit
	True
	>>> 'm' in fruit
	False
	>>> 'nan' in fruit
	True
	>>> if 'a' in fruit :
	...     print('Found it!')
	...
	Found it!
	>>> 
	```

4. 空值
	* 空值是Python里一个特殊的值，用None表示
5. 其他类型
	* 此外，Python还提供了列表、字典等多种数据类型，还允许创建自定义数据类型
## 常量、变量、表达式、赋值
* 变量命名
	* 必须以字母或下划线开头
	* 只能由字母、数字、下划线组成
	* 大小写敏感
	* 不得使用保留关键字：如False、True、None等
* 基本的运算符：`+ - * / ** % //`
	* 运算优先级：括号、幂 `**`、乘除取余、加减、同级从左至右
	* 注意: `3/2=1.5 3//2=1`
* 例子：
	```python
	a = 1;
	b = a + 1;
	```
* 注意：
	* 对变量赋值x = y是把变量x指向真正的对象，该对象是变量y所指向的。随后对变量y的赋值不影响变量x的指向
	* Python的整数没有大小限制，而某些语言的整数根据其存储长度是有大小限制的，例如Java对32位整数的范围限制在-2147483648-2147483647
	* Python的浮点数也没有大小限制，但是超出一定范围就直接表示为inf（无限大）
## 类型转换
* 使用type()可以查看变量的类型
* 隐式转换：如整数和浮点数作运算，自动将整数转换为浮点数做运算，结果为浮点数
* 显式转换：如int()、float()等函数

## 字符串
* 对于字符串, + 代表拼接 concatenate
* 对于单个字符的编码，Python提供了ord()函数获取字符的整数表示，chr()函数把编码转换为对应的字符
	```python
	>>> ord('A')
	65
	>>> ord('中')
	20013
	>>> chr(66)
	'B'
	>>> chr(25991)
	'文'
	```
* 字符串是不可变类型，但可以索引、切片等，python索引基本都是默认从0开始，包括后面介绍的列表、元组等
	```python
	#We can also look at any continuous section of a string using a colon operator
	#The second number is one beyond the end of the slice - “up to but not including”
	>>> a = 'abc'
	>>> a[0]
	'a'
	>>> a[0:2]
	'ab'
	>>> a[0:3]
	'abc'
	>>>
	
	>>> s = 'Monty Python'
	>>> print(s[0:4])
	Mont
	>>> print(s[6:7])
	P
	>>> print(s[6:20])
	Python
	#If the second number is beyond the end of the string, it stops at the end 
	
	#If we leave off the first number or the last number of the slice, 
	#it is assumed to be the beginning or end of the string respectively
	>>> print(s[:2])
	Mo
	>>> print(s[8:])
	thon
	>>> print(s[:])
	Monty Python

	#还可以使用负数倒着索引切片
	>>> s[-2:]
	'on'
	#还可以指定步长
	>>> s[0:4:2]
	'Mn'
	>>> s[0::2]
	'MnyPto'
	```
* len()函数返回字符串长度，包括后面介绍的列表、元组等也是如此
* 可用于for循环
* 字符串包内的函数，都被做成了build-in函数，可以直接使用
	```python
	>>> greet = 'Hello Bob'
	>>> zap = greet.lower()
	>>> print(zap)
	hello bob
	>>> print(greet)
	Hello Bob
	>>> print('Hi There'.lower())
	hi there
	>>>
	#Python has a number of string functions which are in the string library
	#These functions are already built into every string - we invoke them by appending the function to the string variable
	#These functions do not modify the original string, instead they return a new string that has been altered
	```
* 使用dir()函数可以查看某种class对应的所有方法
	```python
	>>> stuff = 'Hello world'
	>>> type(stuff)
	<class 'str'>
	>>> dir(stuff)
	['capitalize', 'casefold', 'center', 'count', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'format_map', 'index', 'isalnum', 'isalpha', 'isdecimal', 'isdigit', 'isidentifier', 'islower', 'isnumeric', 'isprintable', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'maketrans', 'partition', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']
	```
* str的每种方法对应的详细说明文档可以见[python官方文档](https://docs.python.org/3/library/stdtypes.html#string-methods)
* 常用的str方法
	* 查找find
	```python
	>>> fruit = 'banana'
	>>> pos = fruit.find('na')
	>>> print(pos)
	2
	>>> aa = fruit.find('z')
	>>> print(aa)
	-1
	```
	* 查找和替换replace
	```python
	>>> greet = 'Hello Bob'
	>>> nstr = greet.replace('Bob','Jane')
	>>> print(nstr)
	Hello Jane
	>>> nstr = greet.replace('o','X')
	>>> print(nstr)
	HellX BXb
	>>> 
	#It replaces all occurrences of the search string with the replacement string
	```
	* 去掉字符串(前)(后)“空白”（不仅仅是空格，还有制表符等非打印字符）
	```python
	>>> greet = '   Hello Bob  '
	>>> greet.lstrip()
	'Hello Bob  '
	>>> greet.rstrip()
	'   Hello Bob'
	>>> greet.strip()
	'Hello Bob'
	>>> 
	```
	* split函数(Using split to parse strings)
	```python
	>>> abc = 'With three words'
	>>> stuff = abc.split()
	>>> print(stuff)
	['With', 'three', 'words']
	>>> print(len(stuff))
	3
	>>> print(stuff[0])
	With

	#When you do not specify a delimiter, multiple spaces are treated like one delimiter
	>>> line = 'A lot               of spaces'
	>>> etc = line.split()
	>>> print(etc)
	['A', 'lot', 'of', 'spaces']
	>>>
	>>> line = 'first;second;third'
	>>> thing = line.split()
	>>> print(thing)
	['first;second;third']
	>>> print(len(thing))
	1
	>>> thing = line.split(';')
	>>> print(thing)
	['first', 'second', 'third']
	>>> print(len(thing))
	3
	#You can specify what delimiter character to use in the splitting
	```
	* 字符串开始单词/前缀
	```python
	>>> line = 'Please have a nice day'
	>>> line.startswith('Please')
	True
	>>> line.startswith('p')
	False
	```
	* 案例
	```python
	#解析和提取想要的信息
	>>> data = 'From stephen.marquard@uct.ac.za Sat Jan  5 09:14:16 2008'
	>>> atpos = data.find('@')
	>>> print(atpos)
	21
	>>> sppos = data.find(' ',atpos)
	>>> print(sppos)
	31
	>>> host = data[atpos+1 : sppos]
	>>> print(host)
	uct.ac.za
	```
* In Python 3, all strings are Unicode.更多关于字符串和编码的内容参见[廖雪峰python教程](https://www.liaoxuefeng.com/wiki/1016959663602400/1017075323632896)
## 列表和元组
* 列表和元组很类似，只不过列表是可变类型，元组是不可变类型，列表用`[]`或`list()`创建，元组用`()`或`tuple()`创建
* 索引和切片的方法和字符串类似
* 对于列表和元组，也可以使用+来拼接，使用in或者not in来判断某个元素是否在其中
* 也有很多内置方法，类似字符串，也可用dir()查看所有方法，具体用法也见官方文档
* 对于列表，常用的内置方法
	* 末尾追加append
	* 排序sort（注意list.sort()和sorted(list)的区别，前者直接在原列表基础上排序，后者是创建了一个排序好的copy，原列表没变）
	```python
	>>> friends = [ 'Joseph', 'Glenn', 'Sally' ]
	>>> friends.sort()
	>>> print(friends)
	['Glenn', 'Joseph', 'Sally']
	>>> print(friends[1])
	Joseph
	>>> 
	#The sort method (unlike in strings) means “sort yourself”
	```
	* remove移除第一个匹配项
	```python
	>>> a = [1,2,1,2]
	>>> a.remove(1)
	>>> a
	[2, 1, 2]
	>>> a.remove(1)
	>>> a
	[2, 2]
	```
* 其他内置函数可以以list为参数
	* 例子
	```python
	>>> nums = [3, 41, 12, 9, 74, 15]
	>>> print(len(nums))
	6
	>>> print(max(nums))
	74
	>>> print(min(nums))
	3
	>>> print(sum(nums))
	154
	>>> print(sum(nums)/len(nums))
	25.6
	```
* 列表生成的简便方法
	```python
	>>> c = {'a':10, 'b':1, 'c':22}
	
	>>> print( sorted( [ (v,k) for k,v in c.items() ] ) )
	
	[(1, 'b'), (10, 'a'), (22, 'c')]
	```
## 条件判断
* 语法：
	```python
	if <条件判断1>:
	    <执行1>
	elif <条件判断2>:
	    <执行2>
	elif <条件判断3>:
	    <执行3>
	else:
	    <执行4>
	#elif是else if的缩写
	```
* 案例：
	```python
	if x:
	    print('True')
	#只要x是非零数值、非空字符串、非空list等，就判断为True，否则为False。
	```
* 比较运算：`> < >= <= == != `
* `try / except` 结构
	* 语法：
	```python
	try:
		语句1
	except:
		语句2
	#尝试运行语句1，如果语句1可以跑通，则执行语句1，否则转到去执行语句2
	```
## 循环
* 循环是让计算机做重复任务的有效的方法。
* python中有两种循环：for循环和while循环
* 案例
	```python
	#while循环
	n = 5
	while n > 0 :
	    print(n)
	    n = n – 1
	print('Blastoff!')
	print(n)
	
	#for循环
	for i in [5, 4, 3, 2, 1] :
    	print(i)
	print('Blastoff!')
	```
* 循环控制：break跳出整个循环、continue跳出本轮循环，直接进入下一轮循环