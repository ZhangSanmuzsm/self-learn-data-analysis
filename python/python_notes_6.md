# python笔记6——正则表达式
## 快速指南
```
Python Regular Expression Quick Guide

^        Matches the beginning of a line
$        Matches the end of the line
.        Matches any character
\s       Matches whitespace
\S       Matches any non-whitespace character
*        Repeats a character zero or more times
*?       Repeats a character zero or more times 
         (non-greedy)
+        Repeats a character one or more times
+?       Repeats a character one or more times 
         (non-greedy)
[aeiou]  Matches a single character in the listed set
[^XYZ]   Matches a single character not in the listed set
[a-z0-9] The set of characters can include a range
(        Indicates where string extraction is to start
)        Indicates where string extraction is to end
```
## python中使用正则表达式
* Before you can use regular expressions in your program, you must import the library using `import re`
* You can use `re.search()` to see if a string matches a regular expression, similar to using the `find()` method for strings (匹配则返回一个re.Match对象,再调用其span()方法可以找出起始索引位置和结束索引位置+1组合而成的元组，不匹配则返回`None`)
	* 案例
	```python 
	#1. Using re.search() Like find()
	import re
	
	hand = open('mbox-short.txt')
	for line in hand:
	    line = line.rstrip()
	    if re.search('From:', line) :
	        print(line)
	#hand = open('mbox-short.txt')
	#for line in hand:
	#    line = line.rstrip()
	#    if line.find('From:') >= 0:
	#        print(line)

	#2. Using re.search() Like startswith()
	import re

	hand = open('mbox-short.txt')
	for line in hand:
	    line = line.rstrip()
	    if re.search('^From:', line) :
	        print(line)
	#hand = open('mbox-short.txt')
	#for line in hand:
	#    line = line.rstrip()
	#    if line.startswith('From:') :
	#        print(line)
	
	#3. 找出匹配位置起始索引
	import re
	print(re.search('www', 'www.runoob.com').span())  # 在起始位置匹配
	print(re.search('com', 'www.runoob.com').span())         # 不在起始位置匹配
	# 返回结果
	(0, 3)
	(11, 14)
	```
* You can use `re.findall()` to extract portions of a string that match your regular expression, similar to a combination of `find()` and slicing: `var[5:10]`
	* 案例
	```python
	>>> import re
	>>> x = 'My 2 favorite numbers are 19 and 42'
	>>> y = re.findall('[0-9]+',x)
	>>> print(y)
	['2', '19', '42']
	>>> y = re.findall('[AEIOU]+',x)
	>>> print(y)
	[]
	```
* 贪婪匹配和非贪婪匹配
	```python
	#贪婪匹配
	#The repeat characters (* and +) push outward in both directions (greedy) to match the largest possible string
	>>> import re
	>>> x = 'From: Using the : character'
	>>> y = re.findall('^F.+:', x)
	>>> print(y)
	['From: Using the :']

	#非贪婪匹配
	#Not all regular expression repeat codes are greedy!  If you add a ? character, the + and * chill out a bit...
	>>> import re
	>>> x = 'From: Using the : character'
	>>> y = re.findall('^F.+?:', x)
	>>> print(y)
	['From:']
	```
* 用()标明字符串提取的范围，用于匹配更准确，但不改变想要提取的结果
	```python
	>>> x = 'From stephen.marquard@uct.ac.za Sat Jan  5 09:14:16 2008'
	>>> y = re.findall('\S+@\S+',x)
	>>> print(y)
	['stephen.marquard@uct.ac.za']
	>>> y = re.findall('^From (\S+@\S+)',x)
	>>> print(y)
	['stephen.marquard@uct.ac.za']
	```
* 字符串提取案例
	```python
	#1. Extracting a host name - using find and string slicing
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

	#2. The Double Split Pattern
	line = 'From stephen.marquard@uct.ac.za Sat Jan  5 09:14:16 2008'
	words = line.split()
	email = words[1]
	pieces = email.split('@')
	print(pieces[1])

	#3. The Regex Version
	import re 
	lin = 'From stephen.marquard@uct.ac.za Sat Jan  5 09:14:16 2008'
	y = re.findall('@([^ ]*)',lin)
	print(y)
	
	['uct.ac.za']
	###Look through the string until you find an at sign
	###Match non-blank character
	###Match many of them(0次或多次)
	###Extract the non-blank characters

	#4. Even Cooler Regex Version
	import re 
	lin = 'From stephen.marquard@uct.ac.za Sat Jan  5 09:14:16 2008'
	y = re.findall('^From .*@([^ ]*)',lin)
	print(y)
	
	['uct.ac.za']
	```
* 转义字符：If you want a special regular expression character to just behave normally (most of the time) you prefix it with '\\'
	```python
	>>> import re
	>>> x = 'We just received $10.00 for cookies.'
	>>> y = re.findall('\$[0-9.]+',x)
	>>> print(y)
	['$10.00']
	```