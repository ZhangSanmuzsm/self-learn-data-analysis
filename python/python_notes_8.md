# python笔记8——Using Web Services
* With the HTTP Request/Response well understood and well supported, there was a natural move toward exchanging data between programs using these protocols
* We needed to come up with an agreed way to represent data going between applications and across networks
* There are two commonly used formats: XML and JSON
* XML:Marking up data to send across the network...
	```xml
	<person>
	   <name>
	      Chuck
	   </name>
	   <phone>
	      303 4456
	   </phone>
	</person>
	```
* JSON:Marking up data to send across the network...
	```json
	{
	  "name" :  "Chuck",
	  "phone" :  "303-4456"
	}
	```
## XML(eXtensible Markup Language)
* Primary purpose is to help information systems share structured data
* It started as a simplified subset of the Standard Generalized Markup Language (SGML), and is designed to be relatively human-legible
* 简单例子
	```xml
	<person>
	  <name>Chuck</name>
	  <phone type="intl">
	     +1 734 303 4456
	   </phone>
	   <email hide="yes" />
	</person>
	#<email hide="yes" />是Self Closing Tag
	#type="intl" hide="yes"是Attribute
	#Chuck +1 734 303 4456是Text Content
	#<person> <name> <phone type="intl">是Start Tag
	#</person> </name> </phone>是End Tag

	#Line ends do not matter.  White space is generally discarded on text elements. 
	#We indent only to be readable.
	<person>
	  <name>Chuck</name>
	  <phone type="intl">+1 734 303 4456</phone>
	  <email hide="yes" />
	</person>
	```
* XML Terminology 术语
	* Tags indicate the beginning and ending of elements
	* Attributes - Keyword/value pairs on the opening tag of XML
	* Serialize / De-Serialize - Convert data in one program into a common format that can be stored and/or transmitted between systems in a programming language-independent manner
* XML as a Tree
	```xml
	<a>
	   <b>X</b>
	   <c>
	      <d>Y</d>
	      <e>Z</e>
	   </c>
	</a>
	```
* XML Text and Attributes
	```xml
	<a> 	
	  <b w="5">X</b>  
	  <c> 	    
	    <d>Y</d>
	    <e>Z</e>	
	  </c>
	</a>
	#a的b节点下有X文本和w属性，其值为"5"
	```
* XML as Paths
	```xml
	<a>
	  <b>X</b>
	  <c>
	    <d>Y</d>
	    <e>Z</e>
	  </c>
	</a>  
	#/a/b        X
	#/a/c/d      Y
	#/a/c/e      Z
	```
* XML Schema
	* Describing a “contract” as to what is acceptable XML
	* Description of the legal format of an XML document
	* Expressed in terms of constraints on the structure and content of documents
	* Often used to specify a “contract” between systems - “My system will only accept XML that conforms to this particular Schema.”
	* If a particular piece of XML meets the specification of the Schema - it is said to “validate”
	* Many XML Schema Languages
		* Document Type Definition (DTD)
		* Standard Generalized Markup Language (ISO 8879:1986 SGML)
		* XML Schema  from W3C - (XSD)
			* It is often called “W3C Schema” because “Schema” is considered generic
			* More commonly it is called XSD because the file names end in .xsd
			* 例子
				```xsd
				# xsd例子1
				<xs:complexType name=”person”>
				  <xs:sequence>
				    <xs:element name="lastname" type="xs:string"/>
				    <xs:element name="age" type="xs:integer"/>
				    <xs:element name="dateborn" type="xs:date"/>
				  </xs:sequence>
				</xs:complexType>
				
				# 下面这个xml符号上面的W3C Schema
				<person>
				   <lastname>Severance</lastname>
				   <age>17</age>
				   <dateborn>2001-04-17</dateborn>
				</person>

				# xsd例子1
				<xs:element name="person">
				  <xs:complexType>
				    <xs:sequence>
				      <xs:element name="full_name" type="xs:string"  
				          minOccurs="1" maxOccurs="1" />
				      <xs:element name="child_name" type="xs:string" 
				            minOccurs="0" maxOccurs="10" />
				    </xs:sequence>
				  </xs:complexType>
				</xs:element>
				
				# 下面这个xml符号上面的W3C Schema
				<person>
				  <full_name>Tove Refsnes</full_name>
				  <child_name>Hege</child_name>
				  <child_name>Stale</child_name>
				  <child_name>Jim</child_name>
				  <child_name>Borge</child_name>
				</person>
				```
			* ISO 8601 Date/Time Format
				* `2002-05-30T09:30:10Z` 最后的Z对于time zone,typically specified in UTC / GMT rather than local time zone
				```xml
				<xs:element name="customer" type="xs:string"/>
				<xs:element name="start" type="xs:date"/>
				<xs:element name="startdate" type="xs:dateTime"/>
				<xs:element name="prize" type="xs:decimal"/>
				<xs:element name="weeks" type="xs:integer"/>


				<customer>John Smith</customer>
				<start>2002-09-24</start>
				<startdate>2002-05-30T09:30:10Z</startdate>
				<prize>999.50</prize>
				<weeks>30</weeks>
				```
* python中xml案例
	```python
	#例子1
	import xml.etree.ElementTree as ET
	data = '''<person>
	  <name>Chuck</name>
	  <phone type="intl">
	     +1 734 303 4456
	   </phone>
	   <email hide="yes"/>
	</person>'''
	
	tree = ET.fromstring(data)
	print('Name:',tree.find('name').text)
	print('Attr:',tree.find('email').get('hide'))

	#例子2
	import xml.etree.ElementTree as ET
	input = '''<stuff>
	    <users>
	        <user x="2">
	            <id>001</id>
	            <name>Chuck</name>
	        </user>
	        <user x="7">
	            <id>009</id>
	            <name>Brent</name>
	        </user>
	    </users>
	</stuff>'''
	
	stuff = ET.fromstring(input)
	lst = stuff.findall('users/user')#这时返回一个长为2的列表，分别是这两个<user></user> tag
	print('User count:', len(lst))
	for item in lst:
	    print('Name', item.find('name').text)
	    print('Id', item.find('id').text)
	    print('Attribute', item.get("x"))
	```
## JSON(JavaScript Object Notation)
* python中json案例
	```python
	#案例1
	import json
	data = '''{
	  "name" : "Chuck",
	  "phone" : {
	    "type" : "intl",
	    "number" : "+1 734 303 4456"
	   },
	   "email" : {
	     "hide" : "yes"
	   }
	}'''
	
	info = json.loads(data) #因为JSON外面是{},所以此时转化为了嵌套的字典，可以直接使用python字典相关的语法来操作
	print('Name:',info["name"])
	print('Hide:',info["email"]["hide"])
	
	#案例2
	import json
	input = '''[
	  { "id" : "001",
	    "x" : "2",
	    "name" : "Chuck"
	  } ,
	  { "id" : "009",
	    "x" : "7",
	    "name" : "Chuck"
	  }
	]'''
	
	info = json.loads(input) #因为JSON外面是[],所以此时转化为了一个列表，列表内部元素是嵌套的字典
	print('User count:', len(info))
	for item in info:
	    print('Name', item['name'])
	    print('Id', item['id'])
	    print('Attribute', item['x'])
	```
* JSON represents data as nested “lists” and “dictionaries” (converted in python) (这也是人们更多使用JSON而不是XML的原因，当然各自有各种适用范围，在一些结构化文档中XML还是常用的，而某些配置文件则JSON常用)
## Service Oriented Approach
* Most non-trivial web applications use services
* They use services from other applications
	* Credit Card Charge
	* Hotel Reservation systems
* Services publish the “rules” applications must follow to make use of the service (API)
* Initially - two systems cooperate and split the problem. As the data/service becomes useful - multiple applications want to use the information / application. 所以Web Services就发展起来了
## Application Program Interface(API 应用程序接口)
* The API itself is largely abstract in that it specifies an interface and controls the behavior of the objects specified in that interface. The software that provides the functionality described by an API is said to be an “implementation” of the API.  An API is typically defined in terms of the programming language used to build an application. 
* 案例
	```python
	import urllib.request, urllib.parse, urllib.error
	import json
	
	serviceurl = 'http://maps.googleapis.com/maps/api/geocode/json?'
	
	while True:
	    address = input('Enter location: ')
	    if len(address) < 1: break
	
	    url = serviceurl + urllib.parse.urlencode({'address': address}) #url编码
	
	    print('Retrieving', url)
	    uh = urllib.request.urlopen(url)
	    data = uh.read().decode()
	    print('Retrieved', len(data), 'characters')
	
	    try:
	        js = json.loads(data)
	    except:
	        js = None
	
	    if not js or 'status' not in js or js['status'] != 'OK':
	        print('==== Failure To Retrieve ====')
	        print(data)
	        continue
	
	    lat = js["results"][0]["geometry"]["location"]["lat"]
	    lng = js["results"][0]["geometry"]["location"]["lng"]
	    print('lat', lat, 'lng', lng)
	    location = js['results'][0]['formatted_address']
	    print(location)
	
	#Enter location: Ann Arbor, MI
	#Retrieving http://maps.googleapis.com/maps/api/geocode/json?address=Ann+Arbor%2C+MI
	#Retrieved 1669 characters
	#lat 42.2808256 lng -83.7430378
	#Ann Arbor, MI, USA
	#Enter location:
	```
* API Security and Rate Limiting
	* The compute resources to run these APIs are not “free”
	* The data provided by these APIs is usually valuable
	* The data providers might limit the number of requests per day, demand an API “key”, or even charge for usage
	* They might change the rules as things progress...
## 总结
* Service Oriented Architecture - allows an application to be broken into parts and distributed across a network 
* An Application Program Interface (API) is a contract for interaction
* Web Services provide infrastructure for applications cooperating (an API) over a network - SOAP and REST are two styles of web services
* XML and JSON are serialization formats






