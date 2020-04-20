# python笔记4——文件处理
## 打开文件
* open() returns a “file handle” - a variable used to perform operations on the file (Similar to “File -> Open” in a Word Processor)
* 语法
	```python
	handle = open(filename, mode)
	#returns a handle use to manipulate the file
	#filename is a string
	#mode is optional and should be 'r' if we are planning to read the file and 'w' if we are going to write to the file
	```
* newline字符`\n`,注意它是一个字符，不是两个
	```python
	>>> stuff = 'Hello\nWorld!'
	>>> stuff
	'Hello\nWorld!'
	>>> print(stuff)
	Hello
	World!
	>>> stuff = 'X\nY'
	>>> print(stuff)
	X
	Y
	>>> len(stuff)
	3
	#We use a special character called the “newline” to indicate when a line ends 
	#We represent it as \n in strings 
	#Newline is still one character - not two
	```
* A text file can be thought of as a sequence of lines
	* 例子
	```
	From stephen.marquard@uct.ac.za Sat Jan  5 09:14:16 2008
	Return-Path: <postmaster@collab.sakaiproject.org>
	Date: Sat, 5 Jan 2008 09:12:18 -0500
	To: source@collab.sakaiproject.org
	From: stephen.marquard@uct.ac.za
	Subject: [sakai] svn commit: r39772 - content/branches/
	
	Details: http://source.sakaiproject.org/viewsvn/?view=rev&rev=39772
	```
	它实际上是如下字符串，也就是每行末尾有个`\n`这个newline字符
	```
	From stephen.marquard@uct.ac.za Sat Jan  5 09:14:16 2008\n
	Return-Path: <postmaster@collab.sakaiproject.org>\n
	Date: Sat, 5 Jan 2008 09:12:18 -0500\n
	To: source@collab.sakaiproject.org\n
	From: stephen.marquard@uct.ac.za\n
	Subject: [sakai] svn commit: r39772 - content/branches/\n
	\n
	Details: http://source.sakaiproject.org/viewsvn/?view=rev&rev=39772\n
	```
* File Handle as a Sequence
	* A file handle open for read can be treated as a sequence of strings where each line in the file is a string in the sequence
	* We can use the for statement to iterate through a sequence
	* Remember - a sequence is an ordered set
	* 例子
	```python
	#例子1
	xfile = open('mbox.txt')
	for cheese in xfile:
    	print(cheese)
	
	#例子2
	fhand = open('mbox-short.txt')
	for line in fhand:
    if line.startswith('From:') :
        print(line)
	#结果如下，原因是print结尾默认加了一个\n，所以会多出一个空行，解决办法是使用rstrip()
	###From: stephen.marquard@uct.ac.za\n
	###\n
	###From: louis@media.berkeley.edu\n
	###\n
	###From: zqian@umich.edu\n
	###\n
	###From: rjlowe@iupui.edu\n
	###\n
	###...
	
	fhand = open('mbox-short.txt')
	for line in fhand:
	    line = line.rstrip()
	    if line.startswith('From:') :
	        print(line)
	#结果如下
	###From: stephen.marquard@uct.ac.za
	###From: louis@media.berkeley.edu
	###From: zqian@umich.edu
	###From: rjlowe@iupui.edu
	###....
	```
* 也可以读取整个文本
	* We can read the whole file (newlines and all) into a single string
	* 例子
	```python
	>>> fhand = open('mbox-short.txt')
	>>> inp = fhand.read()
	>>> print(len(inp))
	94626
	>>> print(inp[:20])
	From stephen.marquar
	```
## 关闭文件
* 最后一步是调用close()方法关闭文件。文件使用完毕后必须关闭，因为文件对象会占用操作系统的资源，并且操作系统同一时间能打开的文件数量也是有限的：
* 语法
	```python
	handle.close()
	```