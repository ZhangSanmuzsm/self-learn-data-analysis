# python笔记5——字典
* Dictionaries are Python’s most powerful data collection
* Dictionaries allow us to do fast database-like operations in Python
* Dictionaries have different names in different languages
	* Associative Arrays - Perl / PHP
	* Properties or Map or HashMap - Java
	* Property Bag - C# / .Net
* 字典也是可变类型
* 注意对比字典和列表
* 例子
	```python
	>>> purse = dict()
	>>> purse['money'] = 12
	>>> purse['candy'] = 3
	>>> purse['tissues'] = 75
	>>> print(purse)
	{'money': 12, 'tissues': 75, 'candy': 3}
	>>> print(purse['candy'])
	3
	>>> purse['candy'] = purse['candy'] + 2
	>>> print(purse)
	{'money': 12, 'tissues': 75, 'candy': 5}
	# Lists index their entries based on the position in the list
	# Lists index their entries based on the position in the list
	# So we index the things we put in the dictionary with a “lookup tag”
	# Dictionaries are like lists except that they use keys instead of numbers to look up values

	#字典的创建也有两种方式{}或者dict()函数
	>>> jjj = { 'chuck' : 1 , 'fred' : 42, 'jan': 100}
	>>> print(jjj)
	{'jan': 100, 'chuck': 1, 'fred': 42}
	>>> ooo = { }
	>>> print(ooo)
	{}
	>>>
	```
* One common use of dictionaries is counting how often we “see” something
* It is an error to reference a key which is not in the dictionary
* We can use the in operator to see if a key is in the dictionary
	* 例子
	```python
	>>> ccc = dict()
	>>> print(ccc['csev'])
	Traceback (most recent call last):
	  File "<stdin>", line 1, in <module>
	KeyError: 'csev'
	>>> 'csev' in ccc
	False
	```
* 同样地，自带了很多方法，使用dir()函数可查询，下面介绍一些常用方法
	* get()方法，尝试取得某个key，返回对应值，如果没有，则返回我们预设的默认值
	```python
	#    if name in counts:
	#       x = counts[name]
	#    else :
	#       x = 0
	x = counts.get(name, 0)
	
	counts = dict()
	names = ['csev', 'cwen', 'csev', 'zqian', 'cwen']
	for name in names :
	    counts[name] = counts.get(name, 0) + 1
	print(counts)
	
	# 实现对文本各个单词计数
	counts = dict()
	line = input('Enter a line of text:')
	words = line.split()
	
	print('Words:', words)
	print('Counting...’)
	
	for word in words:
	    counts[word] = counts.get(word,0) + 1
	print('Counts', counts)
	```
	*
* Even though dictionaries are not stored in order, we can write a for loop that goes through all the entries in a dictionary - actually it goes through all of the keys in the dictionary and looks up the values
```python
>>> counts = { 'chuck' : 1 , 'fred' : 42, 'jan': 100}
>>> for key in counts:
...     print(key, counts[key])
... 
jan 100
chuck 1
fred 42

# You can get a list of keys, values, or items (both) from a dictionary
>>> jjj = { 'chuck' : 1 , 'fred' : 42, 'jan': 100}
>>> print(list(jjj))
['jan', 'chuck', 'fred']
>>> print(jjj.keys())
['jan', 'chuck', 'fred']
>>> print(jjj.values())
[100, 1, 42]
>>> print(jjj.items())
[('jan', 100), ('chuck', 1), ('fred', 42)]

# We loop through the key-value pairs in a dictionary using *two* iteration variables
# Each iteration, the first variable is the key and the second variable is the corresponding value for the key
>>> jjj = { 'chuck' : 1 , 'fred' : 42, 'jan': 100}
>>> for aaa,bbb in jjj.items() :
    	print(aaa, bbb)
jan 100
chuck 1
fred 42
```
* 于是结合上面的内容，我们可以写出如下程序实现对一个文本出现的所有单词计数，并找出其中出现频率最高的单词及其出现次数
	```python
	name = input('Enter file:')
	handle = open(name)
	
	counts = dict()
	for line in handle:
	    words = line.split()
	    for word in words:
	        counts[word] = counts.get(word,0) + 1
	
	bigcount = None
	bigword = None
	for word,count in counts.items():
	    if bigcount is None or count > bigcount:
	        bigword = word
	        bigcount = count
	
	print(bigword, bigcount)
	```
* 更进一步，我们可以获得前10个出现次数最高的单词及其次数
	```python
	fhand = open('romeo.txt')
	counts = {}
	for line in fhand:
	    words = line.split()
	    for word in words:
	        counts[word] = counts.get(word, 0 ) + 1
	
	lst = []
	for key, val in counts.items():
		newtup = (val, key) 
	    lst.append(newtup)
	
	lst = sorted(lst, reverse=True)
	
	for val, key in lst[:10] :
	    print(key, val)
	```