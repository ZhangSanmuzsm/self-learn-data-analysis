# python笔记3——函数
## 含义
函数是存储起来可以再次使用的步骤，可以调用，是一种数学抽象，想象一下数学中的函数，比如`y=f(x)`,只不过我们在这里编写代码来表达这个`f`而已
## 案例
```python
#不带返回值，称为void(non-fruitful) function
def thing():
    print('Hello')
    print('Fun')
 
thing()
print('Zip')
thing()


#带返回值,用return,称为fruitful function
>>> def greet(lang):
...     if lang == 'es':
...         return 'Hola'
...     elif lang == 'fr':
...         return 'Bonjour'
...     else:
...         return 'Hello'
... 
>>> print(greet('en'),'Glenn')
Hello Glenn
>>> print(greet('es'),'Sally')
Hola Sally
>>> print(greet('fr'),'Michael')
Bonjour Michael
>>> 
```
注意：python支持递归定义函数，也就是在定义函数时可以调用自身，但是这种调用必须有停止条件，且不能超出堆栈限定，否则会栈溢出(stack overflow)
## 分类
1. 内置函数：`print(), input(), type(), float(), int() ...`
2. 自定义函数：如案例中的函数

## 函数的参数
1. 位置参数/必选参数
```python
def power(x):
    return x * x
```
2. 默认参数
```python
def power(x, n=2):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x
    return s
```
定义默认参数要牢记一点：默认参数必须指向不变对象,否则可能导致逻辑错误！
3. 可变参数
```python
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
#在函数内部，参数numbers接收到的是一个tuple即元组
>>> calc(1, 2)
5
>>> calc()
0


#也可以这样
>>> nums = [1, 2, 3]
>>> calc(*nums)
14
```
4. 关键字参数
```python
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)
#可变参数允许你传入0个或任意个参数，这些可变参数在函数调用时自动组装为一个tuple。
#而关键字参数允许你传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个dict
#关键字参数有什么用？它可以扩展函数的功能。
#比如，在person函数里，我们保证能接收到name和age这两个参数，但是，如果调用者愿意提供更多的参数，我们也能收到。
#试想你正在做一个用户注册的功能，除了用户名和年龄是必填项外，其他都是可选项，利用关键字参数来定义这个函数就能满足注册的需求。
>>> person('Michael', 30)
name: Michael age: 30 other: {}
>>> person('Bob', 35, city='Beijing')
name: Bob age: 35 other: {'city': 'Beijing'}
>>> person('Adam', 45, gender='M', job='Engineer')
name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}


#也可以这样
>>> extra = {'city': 'Beijing', 'job': 'Engineer'}
>>> person('Jack', 24, **extra)
name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
#**extra表示把extra这个dict的所有key-value用关键字参数传入到函数的**kw参数，kw将获得一个dict，
#注意kw获得的dict是extra的一份拷贝，对kw的改动不会影响到函数外的extra。
```
5. 命名关键字参数
```python
def person(name, age, *, city, job):
    print(name, age, city, job)
#和关键字参数**kw不同，命名关键字参数需要一个特殊分隔符*，*后面的参数被视为命名关键字参数。
#命名关键字参数也可以有默认值，从而在调用时可以缺省，这里不做展示

>>> person('Jack', 24, city='Beijing', job='Engineer')
Jack 24 Beijing Engineer


#如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符*了：
def person(name, age, *args, city, job):
    print(name, age, args, city, job)
#命名关键字参数必须传入参数名，这和位置参数不同。如果没有传入参数名，调用将报错
```

参数组合：在Python中定义函数，可以用必选参数、默认参数、可变参数、关键字参数和命名关键字参数，这5种参数都可以组合使用。但是请注意，参数定义的顺序必须是：必选参数、默认参数、可变参数、命名关键字参数和关键字参数。虽然可以组合多达5种参数，但不要同时使用太多的组合，否则函数接口的可理解性很差。
