---
layout: "post"
title: "python常用内建函数&set集合"
date: "2017-12-04 19:00"
---

range函数:
--------

python3:返回可迭代对象

python2:返回列表

range(起始值, 结束值的前一个, 步长)

步长:

range: 步长为负值,不同于切片

rangge: 步长为负值,从起始值开始减去步长 直到减到结束值的前一个.

切片: 步长表示正向或反向输出顺序,不参与运算

```python
# python3:range返回可迭代对象
# python2:range返回列表
# 注:python2中xrange函数相当于python3中的range,返回可迭代对象

a = range(10)  # range(0,10)
b = list(a)
print(b)  # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

a = range(1, 10)  # range(起始值, 结束值的前一个)
b = list(a)
print(b)  # [1, 2, 3, 4, 5, 6, 7, 8, 9]

a = range(1, 10, 2)  # range(起始值, 结束值的前一个, 步长)
b = list(a)
print(b)  # [1, 3, 5, 7, 9]

# range: 步长为负值,不同于切片
# rangge: 步长为负值,从起始值开始减去步长 直到减到结束值的前一个.
# 切片: 步长表示正向或反向输出顺序,不参与运算
a = range(10, 1, -2)  # range(起始值, 结束值的前一个, 步长)
b = list(a)
print(b)  # [10, 9, 8, 7, 6, 5, 4, 3, 2]
```

lambda 匿名函数:
------------

lambda 传入参数 : 返回值

定义一个匿名函数,在函数中传入两个参数,实现两个参数相加的功能

```python
# lambda 匿名函数

def add(num1, num2):
    return num1 + num2


# lambda 表达式 创建一个匿名函数

# lambda 传入参数 : 返回值

a = lambda num1, num2: num1 + num2
b = a(1, 2)

print(b)  # 3
```

map函数:
------

map(函数, 处理的对象)

返回可迭代对象

定义一个列表,里面存有1~5的数利用map函数把列表里的每个元素做+1操作

```python
# map函数 映射

li = list(range(1, 6))

# map(函数, 处理的对象)
# 返回可迭代对象

ret = map(lambda x: x + 1, li)
result = list(ret)

print(result)
```

### map函数处理多个对象:

map函数可以处理多个对象,对象的数量没有限制

lambda匿名函数有多少个形参,map函数就要有对应的实参传入

多个对象 对象元素数不同 以最小相同数 为基准

```python
# map函数 两个列表 对应位置相加操作

li1 = list(range(1, 5))  # [1, 2, 3, 4]
li2 = list(range(5, 7))  # [5, 6, 7 ,8]

a = list(map(lambda x, y: x + y, li1, li2))
print(a)  # [6, 8, 10, 12]

# 多个对象 对象元素数不同 以最小相同数 为基准
li1 = list(range(1, 5))  # [1, 2, 3, 4]
li2 = list(range(5, 7))  # [5, 6]

a = list(map(lambda x, y: x + y, li1, li2))
print(a)  # [6, 8]

```

filter函数:过滤
===========

filter(函数,对象)

判断列表里的元素,找出2的倍数(能被2整除的数)

```python
# filter函数:过滤

li = list(range(1, 9))

# 查找2的倍数
# 不能使用map(lambda x: x % 2 == 0,li) 返回 True 或 False

# filter(函数,对象)
ret = filter(lambda x: x % 2 == 0, li)
result = list(ret)
print(result)  # [2, 4, 6, 8]
```

reduce函数:
---------

reduce函数:累计操作

reduce(函数, 对象)

reduce(函数, 对象, 起始值)

注:因为累计操作,最后只返回一个值,所以不是可迭代对象

```python
# reduce函数
# 注:python3中将reduce从内建函数中移除,需要导入functools模块

from functools import reduce

li = list(range(10, 60, 10))  # [10, 20, 30, 40, 50]

# 两个参数
# reduce(函数, 对象) 累计操作
ret = reduce(lambda x, y: x + y, li)

# lambda x, y: x + y
#        10 20    30
#        30 30    60
#        60 40    100
#       100 50    150
print(ret)

# 三个参数
# reduce(函数, 对象, 起始值) 累计操作
ret = reduce(lambda x, y: x + y, li, 100)
# lambda x, y: x + y
#       100 10    110
#       110 20    130
#       130 30    160
#       160 40    200
#       180 50    250
print(ret)
```

set集合:
------

定义集合:

* set(集合) 数据类型 不允许有重复元素
* 使用{}定义 和字典不同,不是已键值对的方式定义
* set 是可迭代对象

set()方法:

* set(可迭代对象) 帮助筛选重复元素

```python
# set集合与运算

# 定义集合
set_1 = {1, 1, 2, 3, 5, 5, 8, 8, 13}

# set(集合) 数据类型 不允许有重复元素
# 使用{}定义 和字典不同,不是已键值对的方式定义
# set 是可迭代对象
print(set_1)  # {1, 2, 3, 5, 8, 13}

# set()方法
# set(可迭代对象) 帮助筛选重复元素

# 可迭代对象 元素查重
li = [1, 2, 2, 3, 3, 3, 4, 5, 5, 6, 6, 6, 7]

# set(li) # 会将列表迭代成集合 所以需要强制转换类型回列表 实现可迭代对象中元素查重
a = set(li)  # {1, 2, 3, 4, 5, 6, 7}
a = list(a)
print(a)  # [1, 2, 3, 4, 5, 6, 7]
```

### 集合运算:

利用:与或非 异或 求出:

* 交集 &
* 并集 \|
* 差集 -
* 对称差集 ^
  * 不同为True 相同为False

```python
# 集合运算:

a = {1, 2, 3, 4}
b = {3, 4, 5, 6}

# 并集:
# a当中有 '|或' b当中有的所有元素
set_2 = a | b
print(set_2)  # {1, 2, 3, 4, 5, 6}

# 交集
# a当中有 '&与' b当中也有的所有元素
set_3 = a & b
print(set_3)  # {3, 4}

# 差集
# a当中有 '-' b当中"没"有的所有元素
set_4 = a - b
print(set_4)  # {1, 2}
# b当中有 '-' a当中"没"有的所有元素
set_5 = b - a
print(set_5)  # {5, 6}

# 对称差集
# a当中有b当中"没"有的元素 和 b当中有a当中"没"有的所有元素
# ^ 异或 不同为True 相同为False
set_6 = a ^ b # 等价于 (a - b) | (b - a)
print(set_6)  # {1, 2, 5, 6}
```
