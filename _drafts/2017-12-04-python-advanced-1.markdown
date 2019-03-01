---
layout: "post"
title: "python高级语法1(生成器/迭代器/闭包/装饰器)"
date: "2017-12-04 16:10"
---


# 生成器:
====

不直接生成,保存计算方式.每一次用创建一个,减轻内存消耗

## 列表生成式

> a = [x\*2 for x in range(1000000000)]

### 构造生成器:

```python
# 创建生成器(方法一)
b = (x * 2 for x in range(1000000000))
# 生成器 每次只生成一个(内存中保存的是计算的方法)
c = next(b)
print(c)

d = next(b)
print(d)

e = next(b)
print(e)

print("###########")


# # 创建生成器(方法二) yield 创建生成器对象
# 调用生成器:next() 或 a.__next__()

# 斐波那契数列
def creatNum(num):
	a, b = 0, 1
	print('---start-----')
	for i in range(num):
		# print(b)
		yield b
		a, b = b, a + b
	print('---stop-----')


a = creatNum(100)
# 迭代生成器对象(避免多次next(),出现异常)
for nums in a:
	print(nums)


```

### .send()方法:

```python
# 首先把上一次挂起的yield语句的返回值通过参数设定，从而实现与生成器方法的交互
# 第一次调用生成器,不能使用.send()方法,因为第一次yield没被启用,所以无法将参数传给yield
# 第一次使用yield生成器,可以使用.send(None)
def test():
	i = 0
	while i < 5:
		temp = yield i
		print(temp)
		i += 1

t = test()
temp = t.__next__()
print(temp)
temp = t.send('a')
print(temp)
```

### 示例(协程:多任务处理):

```python
# 示例(协程:多任务处理):
def test1():
	while True:
		print("--1--")
		yield None


def test2():
	while True:
		print("--2--")
		yield None

t1 = test1()
t2 = test2()
while True:
	t1.__next__()
	t2.__next__()
```

# 迭代器
===

for 循环可以遍历可迭代对象和迭代器

iter()方法,传入可迭代对象,返回迭代器

迭代器:同时具备:\_\_iter\_\_ 和 \_\_next\_\_ 方法

可被迭代的不一定是可迭代对象,也可能是生成器(或迭代器)

可迭代对象(容器):
----------

* List
* Tuple
* dict
* Set
* str

### 判断是否可被迭代:

```python
# 通过dir()方法查询
# 可迭代对象:具备了__iter__方法的对象
li = []
print(dir(li))

# 判断是否是可迭代对象:
from collections import Iterable

print(isinstance(li ,Iterable))
```

### 迭代器:iter()方法

```python
from collections import Iterable

a = [1, 2, 3, 4, 5]
b = iter(a) # 把列表转成 迭代器
for i in b:
	print(i)

print(isinstance(b ,Iterable))
```

### 自定义迭代对象和迭代器:

迭代器:

>同时具备:\_\_iter\_\_ 和 \_\_next\_\_ 方法

```python
class my_list(object):
	"""自定义的可迭代对象,容器"""

	# 数据(属性)
	# 类属性:每一个对象都可以使用的数据
	# 对象(实例)属性:每一个对象,都有独有的属性
	def __init__(self):
		self.container = []

	def add(self,item):
		"""向对象中添加数据"""
		self.container.append(item)

	def __iter__(self):
		"""用来返回迭代器对象"""
		return MyIterator(self.container)



class MyIterator(object):
	"""自定义迭代器"""
	def __init__(self,container):
		self.i = 0
		self.container = container

	def __next__(self):
		"""对迭代器每次迭代的时候被调用"""
		if self.i < len(self.container):
			item = self.container[self.i]
			self.i += 1
			return item
		else:
			raise StopIteration

	# 迭代器:同时具备:__iter__ 和 __next__ 方法
	# __iter__方法:返回自身,迭代器本身是可迭代对象
	def __iter__(self):
		return self




my_list = my_list()
my_list.add(100)
my_list.add(200)
my_list.add(300)

for num in my_list:
	print(num)

test = my_list.__iter__()
from collections import Iterator
print(isinstance(test,Iterator))


# 属性:
# 类属性:每一个对象都可以使用的数据
# 对象(实例)属性:每一个对象,都有独有的属性

# 函数 方法

# 对象方法:
def object_fun(self):
	# 通过self可以操作对象属性 类属性
	pass

# 类方法
@classmethod
def class_fun(cls):
	# 通过cls可以使用类属性
	pass

# 静态方法
def static_fun():
	# 如果不通过类名,那么类属性和对象属性 都无法使用
	pass

```

### 定义完整的迭代器:

```python
class Mylist(object):
	def __init__(self):
		self.i = 0
		self.list = []

	def add(self,item):
		self.list.append(item)

	def __next__(self):
		item = self.list[self.i]
		self.i += 1
		return item

	def __iter__(self):
		return self

Mylist = Mylist()
Mylist.add(100)
Mylist.add(200)
Mylist.add(300)

test = Mylist.__iter__()
print(test)
```

# 闭包:
===

### 函数对象:

在python中 万物皆对象

def 创建的函数 是一个函数对象 函数名foo 指向了一个函数

```python
# 在python中 万物皆对象
# def 创建的函数 是一个函数对象 函数名foo 指向了一个函数
def foo(sum):
	"""说明文档"""
	print(sum)


a = dir(foo)
print(a)
a = foo.__name__  # 函数对象名字
print(a)
a = foo.__doc__  # 函数对象 说明文档
print(a)

bar = foo  # 把foo函数对象指向了bar
print(type(bar))  # bar 是函数对象

a = bar.__name__  # bar 指向foo函数,所以bar函数对象,函数名为:foo
print(a)

foo(2)
foo = 1000
print(foo)  # foo 重新指向了一个整形,所以foo不再是函数对象

bar(6)  # bar已然指向最开始的foo函数对象,但是foo已经被重新指向了1000 这个整形

```

```python
# 函数只是一个对象 可以被作为参数 传递
def foo():
	print("hello")

def bar(fun):
	print("hi")
	fun()
	print("end")

bar(foo)
```

闭包概念:
-----

 闭包:函数嵌套中,把内部函数当做变量 直接return返回

```python
def outter(num):
	def inner(a):
		print(a + num)

	return inner

# 闭包:函数嵌套,把函数直接return返回
```

### 自由变量:

闭包引用了外层函数的变量num,外层函数结束后因为被闭包引用所以num变量没有被回收.这种变量叫自由变量

闭包结束之前,都不回收"当前"的自由变量(相当于面向对象类的self,实例属性)

```python
def outter(num):
	def inner(a):
		print(a + num)

	return inner
# 自由变量
fun = outter(100)  # fun = inner
fun(1)
# 自由变量:闭包引用了外层函数的变量num,外层函数结束因为被闭包引用所以num变量没有被回收
fun2 = outter(200)
fun2(1)
fun(2)  # 闭包结束之前,都不回收当前的自由变量(相当于面向对象类的self,实例属性)
```

### 闭包应用:

```python
# 减少 重复参数 的传递工作
def line(a, b):
	def line_sum(x):
		print(a * x + b)

	return line_sum


line1 = line(1, 1)
line1(10)
line1(11)

line2 = line(10,20)
line2(30)

```

### 闭包内层函数修改外层函数变量: nonlocal关键字

nonlocal 逐层 寻找外层变量

python2中没有nonlocal关键字 函数内部修改全局变量 可以使用可变类型声明全局变量 如:列表list = []

```python

def set_counter(num):
	def counter():
		nonlocal num
		num +=1
		print(num)
	return counter

c = set_counter(100)
c()
```

python2中没有nonlocal关键字 函数内部修改全局变量 可以使用可变类型声明全局变量 如:列表list = []

```python
def set_counter(num):
	li = [num]
	def counter():
		li[0] += 1
		print(li[0])
	return counter

counter = set_counter(100)
counter()
```

# 装饰器:
====

装饰器:本质就是一个闭包函数

需要在闭包外层接收一个函数参数

工作流程::

定义一个闭包函数,外层函数需要接收被装饰的函数

在inner底层函数中,调用被装饰的函数.

跟随闭包里的方法逻辑以及调用的被装饰的函数,实现增加原本函数的功能

被装饰函数名 ==\> @装饰器 ==\> 装饰器(被装饰函数) ==\> 被装饰函数 == inner 被return

log\_time\_test ==\> @log\_time ==\> log\_time(log\_time\_test) ==\> log\_time\_test = inner 被return

```python
# 装饰器应用场景-日志信息

import time

# 原始写法
# def _time():
#     a = time.time()
#     li = [x for x in range(10000000)]
#     print("时间日志")
#     b = time.time()
#     print(b - a)
#
# _time()

# 定义装饰器
def log_time(fun):
	def inner():
		a = time.time()
		fun()
		b = time.time()
		print(b - a)
	return inner

# 使用定义好的 装饰器
@log_time
def log_time_test():
	a = time.time()
	li = [x for x in range(10000000)]
	print("时间日志")

log_time_test()
```

### 多层装饰器:

在使用多层装饰器时,每一层装饰器必须单起一行

从上到下执行

@装饰器1(@装饰器2(被装饰函数))

```python
def test1(fun):
	def inner():
		print("test1 start")
		fun()
		print("test1 end")

	return inner


def test2(f):
	def inner():
		print("say hello")
		f()
		print("test2 end")

	return inner


# 多层装饰器使用
@test1
@test2
def say_hello():
	print("hello")


say_hello()
```

### 补充:打包/解包

函数调用传参:

打包: 函数不定长参数(可变参数):

> \* 修饰的参数,表示可以接受多传过来的参数, 是元祖类型的变量

> \*\*修饰的参数,表示可以接受多传过来的命名参数,是字典类型的变量,以字典的形式返回变量名和参数

```python
def foo(a, b, *args, **kwargs):
	print(a)
	print(b)
	print(args)
	print(kargs)

foo(1, 2, 3, 4, num=5)
```

解包:

\* 表示把元组里的数据按顺序展开,已对应的位置传递给函数

\*\* 表示把字典里的数据按顺序展开,已对应的位置传递给函数(函数形参名必须和解包字典key一样)

```python
def add(num1, num2):
	print(num1 + num2)

t = (11,22)
# add(t[0], t[1])
add(*t)

d = {"num1":100, "num2":200}
add(**d)
```

### 装饰的可变参数:

当被装饰的函数需要传入参数时,装饰器中的inner函数也需要接收相应的参数

inner函数接收后,需要传递给fun()函数中,才不会影响到被装饰函数的调用结果

```python
def test(fun):
	def inner(*args, **kwargs): # 打包
		print("test1 start")
		fun(*args, **kwargs) # 解包
		print("test1 end")

	return inner

# 被装饰函数,需要传如参数时:
@test
def num(num1, num2):
	print(num1 + num2)


num(100,200)
```

### 装饰器的返回值处理:

函数中如果没有使用return返回参数,默认情况下函数也是有返回值的,默认为None

装饰器装饰函数后,调用函数实际上是调用的inner函数.再inner函数中定义一个变量用来接收被装饰的函数中的返回值, 最后inner函数返回那个变量就可以处理装饰的返回值了.

```python
def test(fun):
	def inner(*args, **kwargs):
		print("test1 start")
		result = fun(*args, **kwargs)
		print("test1 end")
		return result

	return inner

# 被装饰函数,需要传入参数时:
@test
def num(num1, num2):
	return num1 + num2


ret = num(100,200)
print(ret )
```

### 带参数的装饰器:

被装饰器装饰过的函数,因为调用该函数,实质上是在调用装饰器里的inner函数,所以当我们在查看该函数的属性(\_\_inner\_\_/\_\_doc\_\_)时,实际上实在查看inner函数的属性

利用wraps函数:

wraps是用来将inner函数的属性设置为fun的属性值

wraps方法的本质:

* inner.\_\_name\_\_ = fun.\_\_name\_\_
* inner.\_\_doc\_\_ = fun.\_\_doc\_\_

```python
import functools

def test(fun):
	@functools.wraps(fun) # wraps是用来将inner函数的属性设置为fun的属性值
	def inner(*args, **kwargs):
		print("test1 start")
		fun(*args, **kwargs)
		print("test1 end")

	return inner


@test
def say_hello():
	"""function say hello"""
	print("say_hello")

print(say_hello.__name__) # inner.__name__
print(say_hello.__doc__) # inner.__doc__
```

### 多层嵌套闭包:(带参数的装饰器)

装饰器需要接收多个值进行判断或运算,可以设计多层闭包

每一层函数都return返回底层的函数,形成闭包.

上一层接收到的值编程自由变量,提供给inner函数使用

```python
import time


# 带参数的装饰器
def set_log(end_print_flat):

	def log(fun):

		def inner(*args, **kwargs):
			print("start:")
			a = time.time()
			fun(*args, **kwargs)
			if end_print_flat:
				b = time.time()
				print(b - a)
			print("end:")

		return inner

	return log

# 使用带参数的装饰器
@set_log(True)
def say_hello():
	print("hi hello")

say_hello()

print()
print("+++++分割线+++++")
print()

@set_log(False)
def login():
	print(time.time())
	print("login called")


login()
```

参考:

* <https://www.zhihu.com/question/20829330>
* <https://segmentfault.com/a/1190000004461404>
