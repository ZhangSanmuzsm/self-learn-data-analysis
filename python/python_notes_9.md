# python笔记9——Python Objects
# Object Oriented Programming OOP
* 面向对象编程——Object Oriented Programming，简称OOP，是一种程序设计思想。OOP把对象作为程序的基本单元，一个对象包含了数据和操作数据的函数
* 之前学的是面向过程编程
* 一些术语
	* Class - a template
	* Method or Message - A defined capability of a class 
	* Field or attribute- A bit of data in a class
	* Object or Instance - A particular instance of a class 
* 例子
	```python
	 # This is the template for making PartyAnimal objects
	 class PartyAnimal:
	    x = 0 # Each PartyAnimal object has a bit of data
	
	    def party(self) :
	      self.x = self.x + 1
	      print("So far",self.x)
	
	 an = PartyAnimal() # Construct a PartyAnimal object and store in an
	
	 an.party() # 等价于 PartyAnimal.party(an)
	 an.party()
	 an.party()

	#We can use dir() to find the “capabilities” of our newly created class.
	print("Type", type(an))
	print("Dir ", dir(an)) #返回可用的方法和属性
	```
* Object Lifecycle
	* Objects are created, used, and discarded
	* We have special blocks of code (methods) that get called
		* At the moment of creation (constructor)
		* At the moment of destruction (destructor)
	* Constructors are used a lot 
		* The primary purpose of the constructor is to set up some instance variables to have the proper initial values when the object is created
		```python
		class PartyAnimal:
		   x = 0
		
		   def __init__(self):
		     print('I am constructed')
		
		   def party(self) :
		     self.x = self.x + 1
		     print('So far',self.x)
		
		   def __del__(self):
		     print('I am destructed', self.x)
		
		an = PartyAnimal()
		an.party()
		an.party()
		an = 42
		print('an contains',an)


		#Constructors can have additional parameters.  
		#These can be used to set up instance variables for the particular instance of the class 
		#(i.e., for the particular object).
		class PartyAnimal:
		   x = 0
		   name = ""
		   def __init__(self, z):
		     self.name = z
		     print(self.name,"constructed")
		
		   def party(self) :
		     self.x = self.x + 1
		     print(self.name,"party count",self.x)
		
		s = PartyAnimal("Sally")
		j = PartyAnimal("Jim")
		
		s.party()
		j.party()
		s.party()

		```
	* Destructors are seldom used
	* The constructor and destructor are optional. The constructor is typically used to set up variables. The destructor is seldom used.
* Inheritance 继承
	* When we make a new class - we can reuse an existing class and inherit all the capabilities of an existing class and then add our own little bit to make our new class
	* Another form of store and reuse
	* Write once - reuse many times
	* The new class (child) has all the capabilities of the old class (parent) - and then some more
	* ‘Subclasses’ are more specialized versions of a class, which inherit attributes and behaviors from their parent classes, and can introduce their own.  
	* 例子
	```python
	class PartyAnimal:
	   x = 0
	   name = ""
	   def __init__(self, nam):
	     self.name = nam
	     print(self.name,"constructed")
	
	   def party(self) :
	     self.x = self.x + 1
	     print(self.name,"party count",self.x)
	
	class FootballFan(PartyAnimal):
	   points = 0
	   def touchdown(self):
	      self.points = self.points + 7
	      self.party()
	      print(self.name,"points",self.points)
	s = PartyAnimal("Sally")
	s.party()
	
	j = FootballFan("Jim")
	j.party()
	j.touchdown()
	# FootballFan is a class which extends PartyAnimal. It has all the capabilities of PartyAnimal and more.
	```
# Summary 总结
* Object Oriented programming is a very structured approach to code reuse
* We can group data and functionality together and create many independent instances of a class