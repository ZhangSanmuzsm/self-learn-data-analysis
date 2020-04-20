# python笔记7——网络编程
## TCP
* Built on top of IP (Internet Protocol)
* Provides a nice reliable pipe
* TCP Connections / Sockets
	* “In computer networking, an Internet socket or network socket is an endpoint of a bidirectional inter-process communication flow across an Internet Protocol-based computer network, such as the Internet.”
* TCP Port Numbers
	* A port is an application-specific or process-specific software communications endpoint
	* It allows multiple networked applications to coexist on the same server
	* There is a list of well-known TCP port numbers
		* Telnet(23)-Login
		* SSH(22)-Secure Login
		* HTTP(80)
		* HTTPS(443)-Secure
		* SMTP(25)-Mail
		* DNS(53)-Domain Name
		* FTP(21)-File Transfer
	* Sometimes we see the port number in the URL if the web server is running on a “non-standard” port. 例如8080端口
* Sockets in Python
	```python
	#Python has built-in support for TCP Sockets
	import socket
	mysock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
	mysock.connect( ('data.pr4e.org', 80) )#前面是主机域名，后面是端口号
	```
## HTTP - Hypertext Transfer Protocol
* The dominant Application Layer Protocol on the Internet
* Invented for the Web - to Retrieve HTML,  Images, Documents, etc.
* Extended to be data in addition to documents - RSS, Web Services, etc.  Basic Concept - Make a Connection - Request a document - Retrieve the Document - Close the Connection
* The HyperText Transfer Protocol is the set of rules to allow browsers to retrieve web documents from servers over the Internet
* Getting Data From The Server
	* Each time the user clicks on an anchor tag with an href= value to switch to a new page, the browser makes a connection to the web server and issues a “GET” request - to GET the content of the page at the specified URL
	* The server returns the HTML document to the browser, which formats and displays the document to the user
## 用python写一个简单的“浏览器”
* 模拟简单的http请求
```python
import socket

mysock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
mysock.connect(('www.py4e.com', 80))
cmd = 'GET http://www.py4e.com/romeo.txt HTTP/1.0\r\n\r\n'.encode()
mysock.send(cmd)

while True:
    data = mysock.recv(512)#512个字节(maybe)为一组接受数据
    if (len(data) < 1):
        break
    print(data.decode(),end='')
mysock.close()
```
## 字符编码
1. ASCII(American Standard Code for Information Interchange)
	* Each character is represented by a number between 0 and 256 stored in 8 bits of memory 
	* We refer to "8 bits of memory as a "byte" of memory 
	* The ord() function tells us the numeric value of a simple ASCII character
	* chr()函数把编码转换为对应的字符
2. Unicode
	* To represent the wide range of characters computers must handle we represent characters with more than one byte
	* UTF-16 – Fixed length - Two bytes
	* UTF-32 – Fixed Length - Four Bytes
	* UTF-8 – 1-4 bytes
		* Upwards compatible with ASCII
		* Automatic detection between ASCII and UTF-8
		* UTF-8 is recommended practice for encoding data to be exchanged between systems
	* In Python 3, all strings are Unicode
	* When we talk to a network resource using sockets or talk to a database we have to encode and decode data (usually to UTF-8)
	* When we read data from an external resource, we must decode it based on the character set so it is properly represented in Python 3 as a string
	* 由于Python的字符串类型是str，在内存中以Unicode表示，一个字符对应若干个字节。如果要在网络上传输，或者保存到磁盘上，就需要把str变为以字节为单位的bytes。
	* Python对bytes类型的数据用带b前缀的单引号或双引号表示：
	```python
	x = b'ABC'
	```
	* len()函数计算的是str的字符数，如果换成bytes，len()函数就计算字节数
## Making HTTP Easier With urllib
Since HTTP is so common, we have a library that does all the socket work for us and makes web pages look like a file
* 例子
	```python
	import urllib.request, urllib.parse, urllib.error
	
	fhand = urllib.request.urlopen('http://data.pr4e.org/romeo.txt')#自动完成了socket连接和请求发送和编码
	for line in fhand:
	    print(line.decode().strip())
	#返回结果如下
	#But soft what light through yonder window breaks
	#It is the east and Juliet is the sun
	#Arise fair sun and kill the envious moon
	#Who is already sick and pale with grief

	#like a file
	import urllib.request, urllib.parse, urllib.error
	
	fhand = urllib.request.urlopen('http://data.pr4e.org/romeo.txt')
	
	counts = dict()
	for line in fhand:
	    words = line.decode().split()
	    for word in words:
	        counts[word] = counts.get(word, 0) + 1
	print(counts)
	```
## Parsing HTML (a.k.a. Web Scraping 爬虫)
### What is Web Scraping?
* When a program or script pretends to be a browser and retrieves web pages, looks at those web pages, extracts information, and then looks at more web pages
* Search engines scrape web pages - we call this “spidering the web” or “web crawling”
### Why Scrape?
* Pull data - particularly social data - who links to who?
* Get your own data back out of some system that has no “export capability”
* Monitor a site for new information
* Spider the web to make a database for a search engine
* 爬虫得遵守相关规定！
### The Easy Way - Beautiful Soup
* You could do string searches the hard way
* Or use the free software library called BeautifulSoup from www.crummy.com
* 安装 `pip install beautifulsoup4`
* 使用 `from bs4 import BeautifulSoup`
* 例子
	```python
	import urllib.request, urllib.parse, urllib.error
	from bs4 import BeautifulSoup
	
	url = input('Enter - ')
	html = urllib.request.urlopen(url).read()#读出整个html页面内容
	soup = BeautifulSoup(html, 'html.parser')#用html解析器解析出soup对象，用于后面进一步提取更细致的信息
	
	# Retrieve all of the anchor tags
	tags = soup('a')#拿出所有<a ...></a>的tags
	for tag in tags:
	    print(tag.get('href', None))#没有则默认返回None
	```
## 总结
* The TCP/IP gives us pipes / sockets between applications
* We designed application protocols to make use of these pipes
* HyperText Transfer Protocol (HTTP) is a simple yet powerful protocol
* Python has good support for sockets, HTTP, and HTML parsing