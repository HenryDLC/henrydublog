---
layout: "post"
title: "Python高级"
date: "2019-04-09 13:20"
---

匿名函数
====

生成数,可用list()方法生成列表

python3 返回可迭代对象

python2 返回列表(python2中xrange()函数返回可迭代对象)

range函数
-------

```python
# range(起始值, 截止值-1, 步长)  返回:可迭代对象
a = range(1, 10, 2)
for i in a:
    print(i)a
# >>> 1
# >>> 3
# >>> 5
# >>> 7
# >>> 9
```

lambda匿名函数
----------

lambda 传入参数 : 返回值

```python
# lambda 传入参数 : 返回值

foo = lambda num1, num2: num1 + num2
print(foo(2, 3)) # >>> 5
```

map函数
-----

对列表进行操作

map(函数方法, 数据)

返回可迭代对象

```python
# 对列表进行操作
# map(函数方法, 数据)
# 返回可迭代对象
def foo(x):
    return x ** x


ret = map(foo, [0, 2, 4, 6, 8])
print(list(ret))  # >>> [1, 4, 256, 46656, 16777216]
# 也可写成
print(list(map(lambda x: x ** x, list(range(0, 10, 2)))))  # >>> [1, 4, 256, 46656, 16777216]
```

### 处理多个对象&以最小值数量值进行计算

```python
# 处理多个对象
# 以最小值数量值进行计算
li1 = list(range(1, 6))  # >>> [1, 2, 3, 4, 5]
li2 = list(range(6, 11))  # >>> [6, 7, 8, 9, 10]
li = map(lambda x, y: x ** x + y ** y, li1, li2)
print(list(li))  # >>> [46657, 823547, 16777243, 387420745, 10000003125]

# 以最小值数量值进行计算
li1 = list(range(1, 6))  # >>> [1, 2, 3, 4, 5]
li2 = list(range(6, 9))  # >>> [6, 7, 8]
li = map(lambda x, y: x ** x + y ** y, li1, li2)
print(list(li))  # >>> [46657, 823547, 16777243]
```

filter函数
--------

对数组进行过滤

filter(函数方法, 参数对象)

返回可迭代对象

```python
# 对数组进行过滤
# filter(函数方法, 参数对象)
# 返回可迭代对象
li = range(0, 10)  # >>> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
ret = filter(lambda x: x % 2 == 0, list(li))
print(list(ret))  # >>> [0, 2, 4, 6, 8]
```

reduce函数
--------

python3需要从functools模块中导入,Python2为内建函数

reduce(函数, 参数对象, 起始值)

设置起始值后,reduce会从起始值开始进行累计操作,比如100+range(0, 10, 2)生成的每个元素值

```python
# python3需要从functools模块中导入,Python2为内建函数
from functools import reduce

# 对数据对象进行累计操作
# reduce(函数, 参数对象, 起始值)
li = range(0, 10, 2)
ret = reduce(lambda x, y: x + y, list(li))
print(ret)  # >>> 20
# 设置起始值后,reduce会从起始值开始进行累计操作,比如100+range(0, 10, 2)生成的每个元素值
ret = reduce(lambda x, y: x + y, list(li), 100)
print(ret)  # >>> 120
```

import导入
========

设置路径和import导入要相互配合
------------------

```python
import 同级文件
```

在导入文件时,python会从同级以及同级以上的目录开始寻找需要的文件.寻找的顺序可以通过sys.path方法获得:

```python
import sys
print(sys.path)
'''
['/Users/HenryDu/PycharmProjects/untitled',
'/Users/HenryDu/PycharmProjects/untitled',
'/Users/HenryDu/anaconda3/envs/learn_py/lib/python37.zip',
'/Users/HenryDu/anaconda3/envs/learn_py/lib/python3.7',
'/Users/HenryDu/anaconda3/envs/learn_py/lib/python3.7/lib-dynload',
'/Users/HenryDu/anaconda3/envs/learn_py/lib/python3.7/site-packages',
'/Applications/PyCharm.app/Contents/helpers/pycharm_matplotlib_backend']
'''
```

sys.path返回的是一个列表的数据类型,所以当我们需要导入不同目录的文件时,可以通过向sys.path列表添加路径来使python解释器找到我们需要的文件.

```python
path = './mod2'
sys.path.insert(1, path)
import hihi
hihi.sea_hi()  # >>> hi
```

```text
.
├── mod1
│   ├── __init__.py
│   └── t.py
├── mod2
│   ├── __init__.py
│   ├── __pycache__
│   │   ├── __init__.cpython-37.pyc
│   │   └── hihi.cpython-37.pyc
│   └── hihi.py
└── test.txt
```

对象比较
====

引用比较
----

```python
a = [100]
b = [100]
# 内容相同 引用不同 is
print(id(a))  # >>> 140295644196680
print(id(b))  # >>> 140295644196744

print(a is b)  # >>> False
```

内容比较
----

```python
a = [100]
b = [100]
# 内容是相同的 ==
print(a == b)  # >>> True
```

自定义比较
-----

```python
class Test(object):
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def __eq__(self, other):
        return self.name == other.name and self.age == other.age


T1 = Test('Tom', 50)
# T2 = Test('jack', 49)  # >>> False
T2 = Test('Tom', 50)

print(T1 is T2)  # >>> False

print(T1 == T2)  # T1.__eq__(T2) equals    # >>> True
```

深拷贝/浅拷贝
=======

浅拷贝
---

**只复制第一层数据,更深层次的引用不负责**

变量c直接获取了a和b变量的引用,并不是获取列表类型的值.而变量d通过copy.copy()方法复制了c的引用,同样没有获取c列表类型的值.所以当我们改变a变量的值后,c和d的值也跟着改变了

```python
import copy

a = [1, 2]
b = [2, 3]
c = [a, b]
d = copy.copy(c)

print(a)  # >>> [1, 2]
print(b)  # >>> [2, 3]
print(c)  # >>> [[1, 2], [2, 3]]
print(d)  # >>> [[1, 2], [2, 3]]

a.append([3, 4, 5])
print(a)  # >>> [1, 2, [3, 4, 5]]
print(b)  # >>> [2, 3]
print(c)  # >>> [[1, 2, [3, 4, 5]], [2, 3]]
print(d)  # >>> [[1, 2, [3, 4, 5]], [2, 3]]
```

深拷贝
---

变量c直接获取了a和b变量的引用,并不是获取列表类型的值.而变量d通过copy.deepcopy()方法复制了c的引用,此时获取了c列表类型的值.所以当我们改变a变量的值后,c的值因为是引用a的引用,所以c的值随着改变,而d的值直接获取了a改变前的列表变量值不是引用,所以没有被改变

```python
import copy

a = [1, 2]
b = [2, 3]

c = [a, b]
d = copy.deepcopy(c)

print(a)  # >>> [1, 2]
print(b)  # >>> [2, 3]
print(c)  # >>> [[1, 2], [2, 3]]
print(d)  # >>> [[1, 2], [2, 3]]

a.append([3, 4, 5])
print(a)  # >>> [1, 2, [3, 4, 5]]
print(b)  # >>> [2, 3]
print(c)  # >>> [[1, 2, [3, 4, 5]], [2, 3]]
print(d)  # >>> [[1, 2], [2, 3]]
```

不可变类型拷贝
-------

对于拷贝不可变类型,无论是浅拷贝还是深拷贝,都直接复制引用不创建新的对象

```python
import copy
a  = (1, 2)
b =copy.copy(a)
print(b)  # >>> (1, 2)
print(a is b)  # >>> True

c = copy.deepcopy(a)
print(c)  # >>> (1, 2)
print(a is c)  # >>> True
```

对于拷贝可变类型,即使拷贝不可变类型c是元组,但是c的值是a/b的引用,所以深拷贝还是会对a/b变量做重新复制的处理.解释器会复制a/b两个变量并且添加到新的对象当中赋值给d.所以d和c为False

```python
import copy
a = [1, 2]
b = [3, 4]
c = (a, b)
d = copy.copy(c)
print(d)  # >>> ([1, 2], [3, 4])
print(c is d)  # >>> True

e = copy.deepcopy(c)
print(e)  # >>> ([1, 2], [3, 4])
print(e is c)  # >>> False
```

生成器
===

生成器是一个特殊的迭代器,所以生成器对象也包括\_\_next\_\_方法和\_\_iter\_\_方法.

生成器可以理解成一个存储算法的工具,当我们要计算大量数据时可以先将算法保存在声称其中,需要数据时在逐次的获取数据值.

(括号)创建生成器
---------

利用(括号)可以创建一个生成器,括号内部填写生成式的算法.每调用一次next()方法,获取一次生成式的值,直到迭代完成抛出StopIteration异常,和迭代器用法一样.

```python
data = (x ** 2 for x in range(11))
print(next(data))  # >> 0
print(next(data))  # >> 1
print(next(data))  # >> 4
print(next(data))  # >> 9
print(next(data))  # >> 16
print(next(data))  # >> 25
print(next(data))  # >> 36
```

yield关键字创建生成器
-------------

在函数内,我们可以在函数的执行过程中将数据返回给函数调用者,有点类似于return返回值,但是返回值会将函数停止,yield只是将数据返回并不会将函数停止,每调用一次next()方法,就会继续执行函数下面的代码.

```python
def data():
    for i in range(11):
        data = i ** 2
        yield data


data_s = data()
print(next(data_s))  # >> 0
print(next(data_s))  # >> 1
print(next(data_s))  # >> 4
print(next(data_s))  # >> 9
print(next(data_s))  # >> 16
print(next(data_s))  # >> 25
print(next(data_s))  # >> 36
```

send唤醒
------

调用next()方法对生成器进行迭代时,我们也可以将数据传入到函数内,不用再调用函数时传入也不会停止函数的执行.通过send()方法,我们可以获得next()方法同样的返回值,也可以将需要传入的值传给函数.

```python
def data():
    for i in range(11):
        data = i ** 2
        data += yield data
        print(data)


data_s = data()
print(next(data_s))  # >> 0
# 第一次向生成器传入参数必须为None,不能是其他参数  next(data_s)等价于data_s.send(None)
data_s = data()
data_s.send(None)
data_s.send(1)  # >> 1
data_s.send(1)  # >> 2
data_s.send(1)  # >> 5
data_s.send(1)  # >> 10
data_s.send(1)  # >> 17
data_s.send(1)  # >> 26
data_s.send(1)  # >> 37
```

### 通过异常获取return值

```python
def data():
    for i in range(1):
        data = i ** 2
        data += yield data
        print(data)

    return '程序结束!'


data_s = data()
try:
    print(data_s.send(None))  # >>> 0
    print(data_s.send(1))  # >>> 1
    print(data_s.send(2))  # StopIteration异常
except StopIteration as e:
    print(e)  # >>> 程序结束!
```

迭代器
===

可迭代对象
-----

能被迭代的数据类型,叫做可迭代对象.换言之,能被放进for...in...循环里迭代的对象叫做可迭代对象.

可迭代对象的本质:在一个对象当中有\_\_iter\_\_方法的对象就是可迭代对象



传统遍历方式:

```python
x = [i for i in range(10)]

i = 0
while i < len(x):
    print(x[i])
    i += 1
```

迭代器
---

\_\_iter\_\_()方法返回一个对象(迭代器)

迭代器具备了\_\_next\_\_方法的对象

迭代器的主要作用是维护一套遍历计数和遍历停止的方法.

### for循环的本质

1\. 通过调用\_\_iter\_\_方法获取迭代器

2\. 对迭代器调用\_\_next\_\_方法,进行迭代操作.

如果没有抛出StopIteration异常,表示迭代器没有结束,把获取到的数据元素放到变量中,如果抛出了异常,表示迭代结束,退出执行

3\. 执行循环体

4\. 调到第二步继续执行

```python
i = [1, 2, 3]
# 第一步获取迭代器
iterator = i.__iter__()
# 第四步,跳转到第二步继续执行迭代操作
while True:
    try:
        # 第二步,调用__next__方法进行迭代操作
        num = iterator.__next__()
        # 第三步,执行循环体
        print(num)
    # 判断是否抛出异常
    except StopIteration:
        break
```

### 迭代器总结

一个对象中含有\_\_iter\_\_方法,那么他就是迭代器,迭代器具备了\_\_next\_\_方法.(含有iter和next方法就是迭代器)

```python
i = [1, 2, 3]
iterator = i.__iter__()
print(iterator)  # > <list_iterator object at 0x107e117f0>
print(iterator.__next__())  # > 1
print(iterator.__next__())  # > 2
```

自定义可迭代对象与迭代器
------------

```python
# 定义一个可迭代对象
class Test_List(object):
    def __init__(self):
        self.container = []

    def add(self, item):
        self.container.append(item)

    def __iter__(self):
        return Test_Iterator(self.container)


# 定义一个迭代器
class Test_Iterator(object):
    def __init__(self, container):
        self.i = 0
        self.container = container

    def __next__(self):
        if self.i < len(self.container):
            item = self.container[self.i]
            self.i += 1
            return item
        else:
            raise StopIteration

    def __iter__(self):
        return self


li = Test_List()
li.add(1)
li.add(2)
li.add(3)

for i in li:
    print(i)

it = li.__iter__()
from collections import Iterator

print(isinstance(it, Iterator))
```

定义完整的迭代器
--------

可迭代对象必然包括一个迭代器,所以迭代器本身就是一个可迭代对象

```python
class Test_List(object):
    def __init__(self):
        self.i = 0
        self.container = []

    def add(self, item):
        self.container.append(item)

    def __next__(self):
        if self.i < len(self.container):
            item = self.container[self.i]
            self.i += 1
            return item
        else:
            raise StopIteration

    def __iter__(self):
        return self
li = Test_List()
li.add(1)
li.add(2)
li.add(3)

for i in li:
    print(i)

it = li.__iter__()
from collections import Iterator

print(isinstance(it, Iterator))
```

闭包
==

语法特征:嵌套函数,外层函数把内层函数的函数对象return返回,就叫闭包(是把函数对象返回,而不是函数结果)

特点:传统套函数,外层和内层的局部变量相互独立.但是闭包中,如果解释器发现返回了一个函数对象认定为闭包后,解释器会将内层函数所需要的变量以及需要的外层变量统保存起来使用,也是内层函数可以使用外层函数,内外层变量都可被内层使用

```python
def outter(num):
    def inner(a):
        print(a+num)

    return inner

fun = outter(100)
fun(1)  # >>> 101
```

内层函数修改外层函数
----------

### python3 nonlocal关键字

```python
# 全局空间
def outter(num):
    # 闭包空间
    def inner(a):
        nonlocal num
        num = a + num
        print(num)

    return inner


fun = outter(100)
fun(1)  # >>> 101

fun(2)  # >>> 103(num值)
```

### python2 利用可可改变类型

```python
# 全局空间
def outter(num):
    # 闭包空间
    num = [num]
    def inner(a):

        num[0] = a + num[0]
        print(num[0])

    return inner


fun = outter(100)
fun(1)  # >>> 101

fun(2)  # >>> 103(num值)
```

作用域-LEGB规则
----------

解释器在查找python变量以及调用时,会遵循由内向外的原则逐层查找

```python
# LEGB规则

# 预加载变量 builtins
# 全局空间 global
a = 100


def outter():
    # 闭包空间 enclosing
    a = 200

    def inner():
        # 局部变量 local
        a = 300
        print(a)

    return inner


fun = outter()
fun()  # >>> 300
```

装饰器
===

本质:装饰器就是一个闭包

### 定义及使用装饰器

```python
import time

# 定义装饰器
def time_log(fun):
    def inner():
        print(time.time())
        fun()
        print(time.time())

    return inner

# 使用装饰器
@time_log
def get_name():
    print('Tom')


get_name()
```

执行过程:

get\_name = time\_log(get\_name) =\> get\_name = inner

get\_name() == inner()

1\. 调用get\_name()

2\. 新建一个和调用装饰函数相同名字的变量及(get\_name)

3\. 调用装饰器函数time\_log(),将调用装饰器的函数作为参数传给装饰器,相当于time\_log(get\_name)

4\. 第三步的time\_log(get\_name)的返回值赋值给第一步创建的和调用装饰器函数同名的变量get\_name

5\. 因为time\_log(get\_name)的返回值是inner,所以在我们调用get\_name()函数时,就相当于调用了装饰器中的inner()函数,输出的结果也与inner()函数的运行结果相同

### 多层装饰器使用

```python
def fun1(f):
    def inner():
        print("fun1 运行")
        f()
        print('fun1 停止')

    return inner


def fun2(f):
    def inner():
        print('fun2 运行')
        f()
        print('fun2 停止')

    return inner


@fun1
@fun2
def say_hello():
    print('hello world')


say_hello()
# >>> fun1 运行
# >>> fun2 运行
# >>> hello world
# >>> fun2 停止
# >>> fun1 停止
```

执行过程:

1\. say\_hello = fun1(fun2(say\_hello))

2\. 调用say\_hello函数,启动fun1()装饰器,相当于直接调用fun1()里面的inner()方法.

3\. 执行print("fun1 运行")

3\. 执行fun1()里面的f()时,获取fun1(fun2(say\_hello))里面的fun2(say\_hello)参数,调用fun2()装饰器

4\. 执行print('fun2 运行')

5\. 执行fun2()下面的f(),获取fun2(say\_hello)里面的say\_hello参数

6\. 执行print('hello world')

7\. 执行fun2(print('hello world'))下面的print('fun2 停止')

8\. 执行fun1(fun2(say\_hello))下面的print('fun1 停止')

### 装饰器的可变参数

```python
def fun1(f):
    def inner(*args, **kwargs):
        print("fun1 运行")
        f(*args, **kwargs)
        print('fun1 停止')

    return inner

@fun1
def age(age):
    print('年龄:{}'.format(age))


age('18')
# >>> fun1 运行
# >>> 年龄:18
# >> fun1 停止
```

### 装饰器返回值处理

如果想要在调用了装饰器后还想要有返回值,那么可以将返回值发送给装饰器,装饰器再得到调用者的返回值后再返回给用户.

```python
def fun1(f):
    def inner(*args, **kwargs):
        print("fun1 运行")
        age = f(*args, **kwargs)
        print('fun1 停止')
        return age

    return inner

@fun1
def age(age):
    return '年龄:{}'.format(age)


print(age('18'))
# >>> fun1 运行
# >> fun1 停止
# >>> 年龄:18
```

带参数的装饰器
-------

### wraps函数

当函数调用装饰器后,实质是调用了装饰器的内嵌函数inner()函数.所以我们查看调用装饰器的函数的某些属性时,会显示inner函数的属性.

wraps函数可以纠正这个现象

调用装饰器的本质:

```python
# encoding: utf-8

# //div[@class='KL4Bh']/img/@src
# //div[@class='_5wCQW']/video/@src


def fun1(f):
    def inner(*args, **kwargs):
        '''inner__doc__'''
        print("fun1 运行")
        age = f(*args, **kwargs)
        print('fun1 停止')
        return age

    return inner


@fun1
def age(age):
    return '年龄:{}'.format(age)


print(age.__name__)
print(age.__doc__)
# >> inner
# >>> inner__doc__
```

wraps函数的调用方法:

```python
import functools


def fun1(f):
    # 带参数的装饰器
    @functools.wraps(f)
    def inner(*args, **kwargs):
        '''inner__doc__'''
        print("fun1 运行")
        age = f(*args, **kwargs)
        print('fun1 停止')
        return age

    return inner


@fun1
def age(age):
    '''age__doc__'''
    return '年龄:{}'.format(age)


print(age.__name__)
print(age.__doc__)
# >> age
# >>> age__doc__
```

如果我们不想调用wraps方法,可以通过修改父类方法的方式修改inner\_\_doc\_\_和inner\_\_name\_\_.

```python
def fun1(f):
    def inner(*args, **kwargs):
        '''inner__doc__'''
        print("fun1 运行")
        age = f(*args, **kwargs)
        print('fun1 停止')

        return age

    # 带参数的装饰器
    inner.__name__ = f.__name__
    inner.__doc__ = f.__doc__

    return inner


@fun1
def age(age):
    '''age__doc__'''
    return '年龄:{}'.format(age)


print(age.__name__)
print(age.__doc__)
# >> age
# >>> age__doc__
```

### 定义&使用带参数的装饰器

带参数的装饰器的本质是多层闭包

```python
import time


# 创建带参数的装饰器 功能:True显示时间,False不显示
def set_time(B):
    def fun(f):
        def inner(*args, **kwargs):
            print("运行")
            f(*args, **kwargs)
            if B:
                print(time.time())
            print('停止')

        return inner

    return fun


@set_time(True)
def hello_world():
    print('time is')


@set_time(False)
def hello_world_time():
    print('time is ')


hello_world()

# >>> 运行
# >>> time is
# >>> 1553071469.9675179
# >>> 停止

print('~' * 10)

hello_world_time()
# >>> 运行
# >>> time is
# >>> 停止
```

类装饰器
----

类自带魔法方法\_\_call\_\_,可以使用户通过 类名()进行直接调用这个类.在这个方法中进行编辑后\_\_call\_\_ 等价于装饰器中的ineer()方法,所以我们可以不直接写装饰器方法,而是写装饰器类

```python
import time

# 类装饰器
class log(object):
    def __init__(self, fun):
        self.fun = fun

    def __call__(self, *args, **kwargs):
        print("运行")
        self.fun(*args, **kwargs)
        print(time.time())
        print('停止')


@log
def hello_world():
    print('time is')


hello_world()
# >>> 运行
# >>> time is
# >>> 1553072167.5186949
# >>> 停止
```

动态语言
====

定义一个类并且创建实例对象后,我们还可以通过实例对象往里面添加属性,这是动态语言的特性

```python
class f(object):
    def __init__(self):
        self.a = 10


foo = f()
print(foo.a)  # >>> 10
foo.b = 11
print(foo.b)  # >>> 11
```

动态添加属性和对象方法

```python
class f(object):
    def __init__(self):
        self.a = 10


foo = f()

def say_a(self):
    print(self.a)

# 通过类名添加对象方法 所有对象都可以使用类名添加的对象方法
f.say_a = say_a
foo.say_a() # >>> 10

# 对象名添加对象方法 只有当前对象可以使用动态添加的对象方法 而且需要将类作为参数传进对象方法中,使解释器获得self参数
foo.say = say_a
foo.say(foo) # >>> 10
```

添加类方法

```python
class f(object):
    a = 10
    b = 20


foo = f()


# 动态添加类方法
@classmethod
def class_fun(cls):
    print(cls.a)


f.c = class_fun
f.c()  # >>> 10


# 动态添加静态方法
@staticmethod
def static_fun():
    print('static_fun')


f.d = static_fun
f.d()  # >>> static_fun
```

删除 动态添加

```python
class f(object):
    a = 10
    b = 20


foo = f()
foo.T = 30

def say_hello(cls):
    print(cls.a + cls.b)
foo.say_hello = say_hello


# 动态添加类方法
@classmethod
def class_fun(cls):
    print(cls.a)


f.c = class_fun
f.c()  # >>> 10


# 动态添加静态方法
@staticmethod
def static_fun():
    print('static_fun')


f.d = static_fun
f.d()  # >>> static_fun
#
# 删除
# 对象属性
delattr(foo, "T")
print(foo.T) # >>> AttributeError: 'f' object has no attribute 'T'
# 类属性
del f.a
print(f.a) # >>> AttributeError: type object 'f' has no attribute 'a'
# 对象方法
del foo.say_hello
foo.say_hello(foo) # >>> AttributeError: 'f' object has no attribute 'say_hello'

# 类方法
del f.c
# f.c() # >>> AttributeError: type object 'f' has no attribute 'c'

# 静态方法
del f.d
f.d() # >>> AttributeError: type object 'f' has no attribute 'd'
```

限制'实例对象'动态添加 \_\_slots\_\_

```python
class f(object):
    __slots__ = ('a', 'b')

foo = f()
foo.a = 10
print(foo.a) # >>> 10
foo.c = 10
print(foo.c) # >>> AttributeError: 'f' object has no attribute 'c'

# __slots__ 只能限制实例对象属性,对象属性的动态添加不起作用
f.d = 10
print(f.d) # >>> 10

```

利用私有属性传入参数值
-----------

```python
# 传统方式:利用私有属性传入参数值
class bank(object):
    def __init__(self):
        self.__money = 0

    def set_money(self, num):
        if isinstance(num, int):
            self.__money = num

        else:
            raise Exception("参数错误")

    def get_money(self):
        return self.__money


bank = bank()
bank.set_money(1000)
print(bank.get_money())
```

属性property
----------

property()方法

```python
class bank(object):
    def __init__(self):  
        self.__money = 0

    def set_money(self, num):
        if isinstance(num, int):
            self.__money = num

        else:
            raise Exception("参数错误")

    def get_money(self):
        return self.__money

    money = property(get_money, set_money)


bank = bank()
print(bank.money)
bank.money = 1000
print(bank.money)
```

property装饰器方法

定义两个相同名字的方法,一个为读取一个为设置参数.

```python
class bank(object):
    def __init__(self):
        self.__money = 0

    @property
    def money(self):
        return self.__money

    @money.setter
    def money(self, num):
        if isinstance(num, int):
            self.__money = num

        else:
            raise Exception("参数错误")


bank = bank()
print(bank.money)
bank.money = 1000
print(bank.money)
```

只读只写属性

```python
class bank(object):
    def __init__(self):
        self.__money = 0

    # 如果只有@property装饰器的函数,则默认为只读属性
    @property
    def money(self):
        raise AttributeError('该属性无法读取')


    @money.setter
    def money(self, num):
        if isinstance(num, int):
            self.__money = num

        else:
            raise Exception("参数错误")





bank = bank()
print(bank.money) # >>> AttributeError: 该属性无法读取

bank.money = 1000
print(bank.money)
```

垃圾回收
====

引用计数
----

当一个对象的内存空间被一个'容器'所引用,比如`a = 'haha'`对象'haha'被a所引用,那么haha的引用就会被+1,如果`b = a`a的对象被b引用,就再做+1操作,但当我们`del`一个引用,那么haha的对象就会被-1处理,当引用计数归零,python解释器就被将haha的内存地址删除,节省资源.

```python
a = 'haha'
b = a
import sys

print(sys.getrefcount(a))  # >>> 5
del b
print(sys.getrefcount(a))  # >>> 4
```

分代收集
----

垃圾回收无法回收`循环引用`

```python
a = []  # a+1
b = []  # b+1
a.append(b)  # b+1
b.append(a)  # a+1
del a  # a-1
del b  # b-1
# 引用计数 a -> 1 b -> 1
```

我们每次创建一个对象,解释器都会将这个对象挂到一个`链`上,解释器会在特定情况下遍历这个`链`对对象进行回收的处理.解释器一共有三条`链`,分别为:零代链条/一代链条/二代链条.默认情况下,每700次一代链条会执行一次一代链条,每执行10次一代链条会执行一次二代链条.

垃圾回收总结
------

引用计数

* 优点:不会停止程序运行
* 缺点:无法回收循环引用

分代回收

* 优点:解决循环引用
* 缺点:会暂时停止程序运行

gc模块
----

```python
import gc
# 查看当前分代回收计数器 计数值
print(gc.get_count())  # >>> (14, 7, 6)
# 查看分代回收'阀值'
print(gc.get_threshold())  # >>> (700, 10, 10)

# 手动回收垃圾
gc.collect()

# 停止分带回收
gc.disable()
```

多任务
===

多任务状态
------

### 同步与异步

同步: 多任务, 多个任务之间执行的时候要求有先后顺序,必须一个先执行完成之后,另一个才能继续执行,只有一个主线

异步: 多任务,多个任务之间执行没有先后顺序,可以同时运行,执行的先后顺序不会有什么影响,存在的多条运行主线

### 阻塞与非阻塞

阻塞: 从调用者的角度出发,如果在调用的时候,被卡主,不能再继续向下运行,需要等待,就说明是阻塞

非阻塞: 从调用者的角度出发,如果在调用的时候,没有被卡主,能够继续向下运行,无需等待,就说明是非阻塞

进程
--

注:进程是操作系统分配资源的最小单位,进程与进程之间的资源是彼此独立的

创建:通过os模块的fork()方法可以创建一个子进程,并且将子进程的PID返回给父进程,将返回值`0`返回给子进程

资源:创建子进程的同时,会将所有的资源和当前的运行状态完全复制一份给子进程

注意:为了演示下过,我们需要将两个代码块都做sleep(0.1)的处理,每次执行后都可让程序过渡到下个时间片上,如果不对时间处理,在同一时间片上就可以完成循环五遍print()语句的操作

获取当前进程的PID

```python
os.getpid()
```

获取当前进程的父进程PID

```python
os.getppid()
```

阻塞, 等待子进程结束并回收资源.

```python
os.wait()
```

两个返回值:

1\. 子进程的PID

2\. 子进程结束状态(无异常默认为:0)

```python
import os, time

# 创建一个进程(子进程)
ret = os.fork()

if ret == 0:
    # 子进程
    print('我是子进程PID:{},父进程PPID:{}'.format(os.getpid(), os.getppid()))
    for i in range(10):
        print('我是子进程')
        time.sleep(0.1)
    raise Exception('抛出异常状态')

else:
    # 父进程
    print('我是父进程PID:{},父进程PPID:{}'.format(os.getpid(), os.getppid()))
    for i in range(5):
        print('父进程')
        time.sleep(0.1)

    print('父进程"任务"结束')
    pid, result = os.wait()  # 阻塞, 等待子进程结束并回收资源.两个返回值,子进程的PID和子进程结束状态(无异常默认为:0)
    print('子进程结束,pid为{},结束状态{}'.format(pid,result))

# >>> 我是父进程PID:31516,父进程PPID:17239
# >>> 父进程
# >>> 我是子进程PID:31517,父进程PPID:31516
# >>> 我是子进程
# >>> 我是子进程
# >>> 父进程
# >>> 我是子进程
# >>> 父进程
# >>> 我是子进程
# >>> 父进程
# >>> 我是子进程
# >>> 父进程
# >>> 我是子进程
# >>> 父进程"任务"结束
# >>> 我是子进程
# >>> 我是子进程
# >>> 我是子进程
# >>> 我是子进程
# >>> Traceback (most recent call last):
# >>>   File "/Users/HenryDu/PycharmProjects/untitled/main.py", line 18, in <module>
# >>>     raise Exception('抛出异常状态')
# >>> Exception: 抛出异常状态
# >>> 子进程结束,pid为31517,结束状态256
```

### 孤儿进程

父进程结束,子进程还在执行任务

子进程交给操作系统进行处理回收

### 僵尸进程

子进程任务结束,父进程没有结束且没有对子进程进行资源回收

会对系统造成资源浪费,所以要尽量避免僵尸进程存在

Process
-------

### 使用Process类

```python
from multiprocessing import Process
import os, time


# target=None 子进程需要执行的任务函数
# name=None   给子进程取名,通过p.name查看
# args=(), kwargs={}     给子进程需要执行的任务函数传入不定长参数

def process_fun(*args, **kwargs):
    for i in range(10):
        print('helloworld')
        print(args)
        print(kwargs)
        print('子进程pid:{}'.format(os.getpid()     ))
        time.sleep(0.1)


if __name__ == '__main__':
    p = Process(target=process_fun, args=(1,2), kwargs={'A':'a'}, name='test')  # 创建子进程
    p.start()  # 运行子进程
    print('父进程PID:{}'.format(os.getpid()))
    print(p.name)  # 获取子进程名称
    time.sleep(0.1)
    p.terminate() # 无论子进程任务是否完成,立即终止
    p.join()  # 子进程回收资源 阻塞
    print(p.is_alive())  # 查看子进程是否还在执行
    print('父进程结束')

# >>> 父进程PID:47836
# >>> test
# >>> helloworld
# >>> (1, 2)
# >>> {'A': 'a'}
# >>> 子进程pid:47837
# >>> False
# >>> 父进程结束
```

### 继承Process类

```python
from multiprocessing import Process
import os, time


# target=None 子进程需要执行的任务函数
# name=None   给子进程取名,通过p.name查看
# args=(), kwargs={}     给子进程需要执行的任务函数传入不定长参数

class process_fun(Process):
    def __init__(self, *args, **kwargs):
        super(process_fun, self).__init__()
        self.args = args
        self.kwargs = kwargs
    def run(self):
        for i in range(10):
            print('helloworld')
            print(self.kwargs)
            print('子进程pid:{}'.format(os.getpid()     ))
            time.sleep(0.1)


if __name__ == '__main__':
    p = process_fun(args=(1,2), kwargs={'A':'a'})  # 创建子进程
    p.start()  # 运行子进程
    print('父进程PID:{}'.format(os.getpid()))
    time.sleep(0.1)
    p.terminate() # 无论子进程任务是否完成,立即终止
    p.join()  # 子进程回收资源 阻塞
    print(p.is_alive())  # 查看子进程是否还在执行
    print('父进程结束')

# >>> 父进程PID:48265
# >>> helloworld
# >>> {'args': (1, 2), 'kwargs': {'A': 'a'}}
# >>> 子进程pid:48266
# >>> helloworld
# >>> {'args': (1, 2), 'kwargs': {'A': 'a'}}
# >>> 子进程pid:48266
# >>> False
# >>> 父进程结束
```

Pool进程池
-------

### 非阻塞添加任务-进程池

pool.apply\_async(test, (i, )) 非阻塞添加任务

pool.apply\_async(test, (i, )) \# 向进程池添加任务, 非阻塞方式 pool.apply\_async(test, (i, ))其中(i, )表示向task(num)传入参数num值

```python
from multiprocessing import Pool
import os, time


def test(num):
    print('开始执行任务{}'.format(num))
    time.sleep(0.1)
    print('结束执行任务{}'.format(num))


pool = Pool(4)

for i in range(10):
    pool.apply_async(test, (i, ))  # 向进程池添加任务, 非阻塞方式  pool.apply_async(test, (i, ))其中(i, )表示向task(num)传入参数num值

pool.close()  # 关闭进程池,不再接收新的任务
pool.join()  # 回收进程池资源


# >>> 开始执行任务0
# >>> 开始执行任务1
# >>> 开始执行任务2
# >>> 开始执行任务3
# >>> 结束执行任务2结束执行任务1
# >>> 结束执行任务0
# >>>
# >>> 开始执行任务4
# >>> 开始执行任务5
# >>> 开始执行任务6
# >>> 结束执行任务3
# >>> 开始执行任务7
# >>> 结束执行任务4
# >>> 开始执行任务8
# >>> 结束执行任务5
# >>> 结束执行任务6
# >>> 开始执行任务9
# >>> 结束执行任务7
# >>> 结束执行任务8结束执行任务9
```

### 阻塞添加任务-进程池

pool.apply(test, (i, )) \# 向进程池添加任务, 阻塞方式

```python
from multiprocessing import Pool
import os, time


def test(num):
    print('开始执行任务{}'.format(num))
    time.sleep(0.1)
    print('结束执行任务{}'.format(num))


pool = Pool(4)

for i in range(10):
    pool.apply(test, (i, ))  # 向进程池添加任务, 阻塞方式  pool.apply_async(test, (i, ))其中(i, )表示向task(num)传入参数num值

pool.close()  # 关闭进程池,不再接收新的任务
pool.join()  # 回收进程池资源


# >>> 开始执行任务0
# >>> 结束执行任务0
# >>> 开始执行任务1
# >>> 结束执行任务1
# >>> 开始执行任务2
# >>> 结束执行任务2
# >>> 开始执行任务3
# >>> 结束执行任务3
# >>> 开始执行任务4
# >>> 结束执行任务4
# >>> 开始执行任务5
# >>> 结束执行任务5
# >>> 开始执行任务6
# >>> 结束执行任务6
# >>> 开始执行任务7
# >>> 结束执行任务7
# >>> 开始执行任务8
# >>> 结束执行任务8
# >>> 开始执行任务9
# >>> 结束执行任务9
```

### 异步添加任务:callback回调函数

```python
import os, time
from multiprocessing import Pool


def test():
    """烧开水"""
    for i in range(3):
        print('烧开水....')
        time.sleep(0.5)
    return '水烧开了'


def fun_callback(message):
    """水烧开了"""
    print(message)


pool = Pool(3)

pool.apply_async(test, callback=fun_callback)
for i in range(20):
    print('打游戏')
    time.sleep(0.5)

pool.close()
pool.join()
```

Queue进程间通信:消息队列
---------------

消息队列: 先进先出

q.empty() 判断是否为空

q.full() 判断是否为满

q.put(100, timeout=) 添加数据, 设置等待时间

q.get(timeout=) 获取数据, 设置等待时间

q.put\_nowait() 为满则不等待抛出异常

q.get\_nowait() 为空则不等待抛出异常

```python
from multiprocessing import Queue

q = Queue(3)  # 初始化队列,设置最多可以添加3条信息

ret = q.empty()
print('判断是否为空{}'.format(ret))
ret = q.full()
print('判断队列是否为满{}'.format(ret))

q.put('消息1')
print('插入第一条消息,成功')
q.put('消息2')
print('插入第二条消息,成功')
q.put('消息3')
print('插入第三条消息,成功')

ret = q.empty()
print('判断是否为空{}'.format(ret))
ret = q.full()
print('判断队列是否为满{}'.format(ret))

try:
    q.put('消息4', timeout=3)
except :
    print('队列已满,无法继续添加')

ret = q.get()
print('第一条消息:{}'.format(ret))
ret = q.get()
print('第二条消息:{}'.format(ret))
ret = q.get()
print('第三条消息:{}'.format(ret))
try:
    ret = q.get(timeout=3)
    print('第四条消息:{}'.format(ret))
except:
    print('队列已空')

# >>> 判断是否为空True
# >>> 判断队列是否为满False
# >>> 插入第一条消息,成功
# >>> 插入第二条消息,成功
# >>> 插入第三条消息,成功
# >>> 判断是否为空False
# >>> 判断队列是否为满True
# >>> 队列已满,无法继续添加
# >>> 第一条消息:消息1
# >>> 第二条消息:消息2
# >>> 第三条消息:消息3
# >>> 队列已空
# >>> 判断是否为空True
# >>> 判断队列是否为满False
```

### 使用队列进行进程间通信

```python
from multiprocessing import Queue, Process
import time

q = Queue(3)


def process_write():
    for i in range(3):
        q.put(i)
        print('子进程1写入了一个数据,成功')
        time.sleep(0.1)


def process_red():
    while not q.empty():
        ret = q.get()
        print('子进程2获取了一个数据:{}'.format(ret))
        time.sleep(0.1)


p1 = Process(target=process_write)
p2 = Process(target=process_red)

p1.start()
p2.start()
p1.join()
p2.join()

# >>> 子进程1写入了一个数据,成功
# >>> 子进程2获取了一个数据:0
# >>> 子进程1写入了一个数据,成功
# >>> 子进程2获取了一个数据:1
# >>> 子进程1写入了一个数据,成功
# >>> 子进程2获取了一个数据:2
```

### 进程池的进程间通讯

需要使用`q = Manager().Queue(N)`进行队列的创建

```python
from multiprocessing import Pool, Manager
import time

q = Manager().Queue(3)


def process_write():
    for i in range(3):
        q.put(i)
        print('子进程1写入了一个数据,成功')
        time.sleep(0.1)


def process_red():
    while not q.empty():
        ret = q.get()
        print('子进程2获取了一个数据:{}'.format(ret))
        time.sleep(0.1)


pool = Pool(2)
pool.apply_async(process_write)
pool.apply_async(process_red)

pool.close()
pool.join()



# >>> 子进程1写入了一个数据,成功
# >>> 子进程2获取了一个数据:0
# >>> 子进程1写入了一个数据,成功
# >>> 子进程2获取了一个数据:1
# >>> 子进程1写入了一个数据,成功
# >>> 子进程2获取了一个数据:2
```

线程
==

> 进程是操作系统`分配资源`的最小单位

> 线程是操作系统`调度执行`的最小单位

创建线程
----

### Thread模块

```python
from threading import Thread
import time


def test():
    for i in range(5):
        print('洗碗')
        time.sleep(0.1)


t = Thread(target=test)

t.start()

for i in range(10):
    print('看电视')
    time.sleep(0.1)

t.join()

# >>> 洗碗看电视
# >>>
# >>> 洗碗
# >>> 看电视
# >>> 洗碗
# >>> 看电视
# >>> 洗碗
# >>> 看电视
# >>> 洗碗
# >>> 看电视
# >>> 看电视
# >>> 看电视
# >>> 看电视
# >>> 看电视
# >>> 看电视
```

### 继承Thread类创建线程

```python
from threading import Thread
import time


class test(Thread):
    def run(self):
        for i in range(5):
            print('洗碗')
            time.sleep(0.1)


t = test()

t.start()

for i in range(10):
    print('看电视')
    time.sleep(0.1)

t.join()

# >>> 洗碗看电视
# >>>
# >>> 洗碗
# >>> 看电视
# >>> 洗碗
# >>> 看电视
# >>> 洗碗
# >>> 看电视
# >>> 洗碗
# >>> 看电视
# >>> 看电视
# >>> 看电视
# >>> 看电视
# >>> 看电视
# >>> 看电视
```

### enumerate()查看线程信息

```python
from threading import Thread, enumerate
import time


class test(Thread):
    def run(self):
        for i in range(5):
            print('洗碗')
            time.sleep(0.1)


t = test()

t.start()

# enumerate() 查看线程信息
print(enumerate()) #  >>> [<_MainThread(MainThread, started 4586436032)>, <test(Thread-1, started 123145409781760)>]

for i in range(10):
    print('看电视')
    time.sleep(0.1)

t.join()

# >>> 洗碗看电视
# >>>
# >>> 洗碗
# >>> 看电视
# >>> 洗碗
# >>> 看电视
# >>> 洗碗
# >>> 看电视
# >>> 洗碗
# >>> 看电视
# >>> 看电视
# >>> 看电视
# >>> 看电视
# >>> 看电视
# >>> 看电视
```

### 多线程修改全局变量

```python
from threading import Thread, enumerate
import time

num = 10


def test_1():
    global num
    print('线程1 num={}'.format(num))
    num += 1
    print('线程1 num={}'.format(num))


def test_2():
    global num
    print('线程2 num={}'.format(num))
    num += 1
    print('线程2 num={}'.format(num))


t1 = Thread(target=test_1)
t2 = Thread(target=test_2)

t1.start()
t1.join()

t2.start()
t2.join()

# >>> 线程1 num=10
# >>> 线程1 num=11
# >>> 线程2 num=11
# >>> 线程2 num=12
```

### 多线程-竞争问题

```python
from threading import Thread, enumerate
import time

num = 10


def test_1():
    for i in range(1000000):
        global num
        num += 1


def test_2():
    for i in range(1000000):
        global num
        num += 1


t1 = Thread(target=test_1)
t2 = Thread(target=test_2)

t1.start()
t2.start()

t1.join()
t2.join()

print(num)

# >>>  1330024
```

### 互斥所-解决线程竞争问题

建议互斥所锁住的资源越少越好

```python
from threading import Thread, enumerate, Lock
import time

num = 10

mutex = Lock()
def test_1():
    for i in range(1000000):
        global num
        mutex.acquire()
        num += 1
        mutex.release()


def test_2():
    for i in range(1000000):
        global num
        mutex.acquire()
        num += 1
        mutex.release()


t1 = Thread(target=test_1)
t2 = Thread(target=test_2)

t1.start()
t2.start()

t1.join()
t2.join()

print(num)

# >>>  2000010
```

### 非阻塞方式使用-互斥所

```python
from threading import Thread, enumerate, Lock
import time

num = 10

mutex = Lock()
def test_1():
    for i in range(1000000):
        while 1:
            global num
            ret = mutex.acquire(False)
            if ret:
                num += 1
                mutex.release()
                break
            else:
                print('唱歌')


def test_2():
    for i in range(1000000):
        while 1:
            global num
            ret = mutex.acquire(False)
            if ret:
                num += 1
                mutex.release()
                break
            else:
                print('跳舞')

t1 = Thread(target=test_1)
t2 = Thread(target=test_2)

t1.start()
t2.start()

t1.join()
t2.join()

print(num)

# >>> 唱歌
# >>> 唱歌
# >>> 唱歌
# >>> 唱歌
# >>> 唱歌
# >>> 跳舞
# >>> 唱歌
# >>> 跳舞
# >>> 跳舞
# >>> 跳舞
# >>> 跳舞
# >>> 跳舞
# >>> 跳舞
# >>> 唱歌
# >>> 唱歌
# >>> 唱歌
# >>> 唱歌
# >>> 唱歌
# >>> 唱歌
# >>> 唱歌
# >>> 2000010
```

死锁
--

```python
from threading import Thread, enumerate, Lock
import time

mutex1 = Lock()
mutex2 = Lock()


def fun1():
    mutex1.acquire()
    print('线程1 锁住了mutex1')
    time.sleep(0.1)

    mutex2.acquire()
    print('线程2 锁住了mutex2')
    time.sleep(0.1)

    mutex1.release()
    mutex2.release()


def fun2():
    mutex2.acquire()
    print('线程2 锁住了mutex2')
    time.sleep(0.1)

    mutex1.acquire()
    print('线程2 锁住了mutex1')
    time.sleep(0.1)

    mutex1.release()
    mutex2.release()


t1 = Thread(target=fun1)
t2 = Thread(target=fun2)

t1.start()
t2.start()

t1.join()
t2.join()

# >>> 线程1 锁住了mutex1
# >>> 线程2 锁住了mutex2
```

### 设置超时或非阻塞-解决死锁

```python
from threading import Thread, enumerate, Lock
import time

mutex1 = Lock()
mutex2 = Lock()


def fun1():
    while 1:
        mutex1.acquire()
        print('线程1 锁住了mutex1')
        time.sleep(0.1)

        ret = mutex2.acquire(timeout=1)
        if ret:
            print('线程2 锁住了mutex2')
            time.sleep(0.1)

            mutex1.release()
            mutex2.release()
            break
        else:
            mutex1.release()
            time.sleep(0.1)

def fun2():
    while 1:
        mutex2.acquire()
        print('线程2 锁住了mutex2')
        time.sleep(0.1)

        ret = mutex1.acquire(timeout=1)
        if ret:
            print('线程2 锁住了mutex1')
            time.sleep(0.1)

            mutex1.release()
            mutex2.release()
            break
        else:
            mutex2.release()
            time.sleep(0.1)


t1 = Thread(target=fun1)
t2 = Thread(target=fun2)

t1.start()
t2.start()

t1.join()
t2.join()

# >>> 线程1 锁住了mutex1
# >>> 线程2 锁住了mutex2
# >>> 线程1 锁住了mutex1
# >>> 线程2 锁住了mutex2
# >>> 线程1 锁住了mutex1
# >>> 线程2 锁住了mutex2
# >>> 线程2 锁住了mutex1
# >>> 线程1 锁住了mutex1
# >>> 线程2 锁住了mutex2> 段落引用> 段落引用
```

### 多线程同步-互斥锁

```python
from threading import Thread, enumerate, Lock
import time

mutex1 = Lock()
mutex2 = Lock()
mutex3 = Lock()


def fun1():
    while 1:
        mutex1.acquire()
        print('线程1 执行')
        mutex2.release()
        time.sleep(0.1)


def fun2():
    while 1:
        mutex2.acquire()
        print('线程2 执行')
        mutex3.release()
        time.sleep(0.1)


def fun3():
    while 1:
        mutex3.acquire()
        print('线程3 执行')
        mutex1.release()
        time.sleep(0.1)


t1 = Thread(target=fun1)
t2 = Thread(target=fun2)
t3 = Thread(target=fun3)

mutex2.acquire()
mutex3.acquire()

t1.start()
t2.start()
t3.start()

t1.join()
t2.join()
t3.join()

# >>> 线程1 执行
# >>> 线程2 执行
# >>> 线程3 执行
# >>> 线程1 执行
# >>> 线程2 执行
# >>> 线程3 执行
# >>> 线程1 执行
# >>> 线程2 执行
# >>> 线程3 执行
```

生产者与消费者模型
---------

```python
from queue import Queue
from threading import Thread
import time

q = Queue()


class Producer(Thread):
    '''生产者线程'''

    def run(self):
        count = 0
        while 1:
            if q.qsize() < 50:
                for i in range(3):
                    count += 1
                    msg = '产品{}'.format(count)
                    q.put(msg)
                    print('生产者{},生产了一个数据{}'.format(self.name, msg))
            time.sleep(0.5)


class Customer(Thread):
    '''消费者'''

    def run(self):
        while 1:
            if q.qsize() > 20:
                for i in range(2):
                    msg = q.get()
                    print('消费者{} 消费了一个数据'.format(self.name, msg))
            time.sleep(0.5)


for i in range(3):
    p = Producer()
    p.start()

for i in range(5):
    c = Customer()
    c.start()

# >>> 生产者Thread-1,生产了一个数据产品1
# >>> 生产者Thread-1,生产了一个数据产品2
# >>> 生产者Thread-1,生产了一个数据产品3
# >>> 生产者Thread-2,生产了一个数据产品1
# >>> 生产者Thread-2,生产了一个数据产品2
# >>> 生产者Thread-2,生产了一个数据产品3
# >>> 生产者Thread-3,生产了一个数据产品1
# >>> 生产者Thread-3,生产了一个数据产品2
# >>> 生产者Thread-3,生产了一个数据产品3
# >>> 生产者Thread-2,生产了一个数据产品4
# >>> 生产者Thread-2,生产了一个数据产品5
# >>> 生产者Thread-2,生产了一个数据产品6
# >>> 生产者Thread-1,生产了一个数据产品4
# >>> 生产者Thread-1,生产了一个数据产品5
# >>> 生产者Thread-1,生产了一个数据产品6
# >>> 生产者Thread-3,生产了一个数据产品4
# >>> 生产者Thread-3,生产了一个数据产品5
# >>> 生产者Thread-3,生产了一个数据产品6
# >>> 生产者Thread-1,生产了一个数据产品7
# >>> 生产者Thread-1,生产了一个数据产品8
# >>> 生产者Thread-1,生产了一个数据产品9
# >>> 生产者Thread-2,生产了一个数据产品7
# >>> 生产者Thread-2,生产了一个数据产品8
# >>> 生产者Thread-2,生产了一个数据产品9
# >>> 生产者Thread-3,生产了一个数据产品7
# >>> 生产者Thread-3,生产了一个数据产品8
# >>> 生产者Thread-3,生产了一个数据产品9
# >>> 消费者Thread-5 消费了一个数据
# >>> 消费者Thread-8 消费了一个数据
# >>> 消费者Thread-5 消费了一个数据
# >>> 消费者Thread-7 消费了一个数据
# >>> 消费者Thread-7 消费了一个数据
# >>> 消费者Thread-6 消费了一个数据
# >>> 消费者Thread-6 消费了一个数据
# >>> 消费者Thread-8 消费了一个数据
```
思维导图总结
--------
![image](https://github.com/HenryDLC/learn_python/raw/master/python基础/learn_python.png)
