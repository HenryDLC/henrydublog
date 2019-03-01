变量
==

### 定义变量:

```python
a = 1
b = 2
c = 1.23
d = 'test'

# 多个变量赋值
x,y,z=1,'2',3.0
```

### 变量名规则:

* 变量名的长度不受限制，但其中的字符必须是字母、数字、或者下划线（\_），而不能使用空格、连字符、标点富豪号、引号或其他字符。
* 变量名的第一个字符不能是数字，而必须是字母或下划线。
* Python区分大小写。
* 不能将Python关键字用作变量名。

输入:

```python
Temp = input("输入想出入的字符:")
print(Temp)
```

### 输出:

```python
print('test')
```

数据类型:
=====

字符型:
----

### 格式化:

%d %s %f:

```python
# 字符串:
a = "xxx"
print("我宣你:%s" % a)

# %d数字型:
a = 10
b = 20
print("a = %d" % a)
print("%d + %d = %d" % (a, b, a + b))

# 浮点型:
a = 3.1415926
print("圆周率:%f" % a)

# 字典匹配 %s 字符型
a = {"name":"xxx"}
print("我宣你:%(name)s" % a)

```

深入阅读:<https://www.jb51.net/article/134290.htm>

format:

```python
a = "xxx"
print("我宣你:{}".format(a))

li = ['henry',18]
dict = {'name':'henry','age':18}
# 位置参数
print("我是:{},今年{}岁".format("Henry","18"))
# 关键字参数
print("我是:{name},今年{age}岁".format(name="Henry",age=18))
# 索引参数
print("我是:{1},今年{0}岁".format("18","Henry"))
# 列表参数
print("我是:{},今年{}岁".format(*li))
# 字典参数
print("我是:{name},今年{age}岁".format(**dict))

# 填充与格式化
# :[填充字符][对齐方式 <^>][宽度]
# 右对齐
print('{0:*>10}'.format(10))
# 左对齐
print('{0:*<10}'.format(10))
# 居中对齐
print('{0:*^10}'.format(10))

# 精度与进制
print('{0:.2f}'.format(1 / 3))
print('{0:b}'.format(10))
# 八进制
print('{0:o}'.format(10))
# 16进制
print('{0:x}'.format(10))
# 千分位格式化
print('{:,}'.format(111111111))
```

### 编码:

decode():解码

encode():编码

注:py2默认ASCII码，py3默认的utf8

因为py2默认的是ASCII进行编码,所以当程序一旦出现中文就会无法解码,所以py2的程序中要在开头添加一行注释表明该py2文件要使用UTF-8进行编码

```python
#-*-coding:utf-8-*-
```

Python2

Unicode —\> Str

```python
# -*-coding:utf-8-*-

s = u'中文'
print type(s)

s = s.encode('utf-8')
print type(s)
```

GBK -Unicode-\>UTF-8

注: 将一个str直接编码城另一种编码,py解释会先将str编译成unicode编码再进行目标编码的转换.但由于py2默认是ascii编码,所以我们需要声明str字符串的编码格式

```python
# 声明编码格式
# 方法一
import sys
reload(sys) # Python2.5 初始化后会删除 sys.setdefaultencoding 这个方法，我们需要重新载入
sys.setdefaultencoding('utf-8')

# 方法二
s = '中文'
s.decode('utf-8').encode('gb18030')
```

```python
# -*-coding:utf-8-*-
# GBK -Unicode->UTF-8
s = u'中文'
print type(s)

s = s.encode('gbk')
s = s.decode('gbk').encode('utf-8')
print type(s)
```

Str —\> Unicode

```python
# -*-coding:utf-8-*-


s = u'中文'
print type(s)

s = s.encode('gbk')
s = unicode(s, 'gbk')
print type(s)
```

忽略错误编码

```python
# -*-coding:utf-8-*-


s = u'中文'
print type(s)

s = s.encode('gbk')
# decode("编码"默认:strict抛出异常)
# ignore: 忽略 非法字符
# replace:用?取代非法字符
# xmlcharrefreplace: 则使用XML的字符引用
s.decode('gbk', 'ignore').encode('utf-8')
print type(s)
```



深入阅读:

<https://www.jianshu.com/p/5682a0e0a9ba>

<https://www.cnblogs.com/jinhaolin/p/5128973.html>

python3

Str\<—\> Bytes

```python
s = '字符串'

s = s.encode()
print(type(s),s)
```

Bytes \<—\> Str

```python
s = '字符串'
s = s.encode()
print(type(s),s)

s = s.decode()
print(type(s),s)
```

总结:

py2和py3在编码上最大的区别是py2默认的编码方式是Unicode而py3则是UTF-8.所以当我们在py2中想要将一个字符串转换成另一种编码,首先要将这个字符串decode解码成Unicode之后再eccode编码成指定类型.而在py3中默认是UTF-8编码,所以我们想将字符串转换成bytes类型,不再是解码而是直接编码成bytes,反之想将bytes转换成utf-8则需要decode解码.

深度阅读:<https://foofish.net/how-python3-handle-charset-encoding.html>

### 求长:

```python
s = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
T = len(s)
print(T)  # >>> 26
```

### 截取(切片):

```python
s = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
T = s[3:6]
print(T)  # >>> DEF
```

### 查找子串:

```python
# find() 查到返回索引,否则返回-f
s = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
T = s.find('EFG')
print(T)  # >>> 4

# index() 查到返回索引,否则抛出异常
s = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
T = s.index('2')
print(T)  # >>> ValueError: substring not found
```

### 拼接:

```python
s1 = 'ABCDEFGHIJKLMN'
s2 = 'OPQRSTUVWXYZ'
T = ','.join(s1+s2)
print(T)    # >>> A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z
```

### 替换:

```python
s1 = 'A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z'
T = s1.replace('D,E,F,G,H', 'd,e,f,g,h')
print(T)  # >>> A,B,C,d,e,f,g,h,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z
```

大小写转换:

```python
s1 = 'ABc dEf'
print(s1.upper())          # 把所有字符中的小写字母转换成大写字母
print(s1.lower())          # 把所有字符中的大写字母转换成小写字母
print(s1.capitalize())     # 把第一个字母转化为大写字母，其余小写
print(s1.title())          # 把每个单词的第一个字母转化为大写，其余小写

'''
>>>
ABC DEF
abc def
Abc def
Abc Def
'''
```

### 去空:

```python
s1 = '  ABc dEf  '

T = s1.strip()  # 把头和尾的空格去掉
print(T)
T = s1.lstrip()  # 把左边的空格去掉
print(T)
T = s1.rstrip()  # 把右边的空格去掉
print(T)

# 把所有的空个去掉
T = s1.replace(' ','')
print(T)
```

### 分割:

```python
s1 = 'A,B,C,d,e,f,g,h,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z'
T = s1.split(',', 3)
print(T)  # >>> ['A', 'B', 'C', 'd,e,f,g,h,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z']
```

常用方法
----

```python
# 删除 string 字符串末尾的指定字符（默认为空格）
s = 'aaaaaI love you aaaa'
print(s.rstrip('a'))  # >>> aaaaaI love you

# 返回一个原字符串右对齐,并使用空格填充至长度 width 的新字符串。如果指定的长度小于字符串的长度则返回原字符串
print(s.rjust(50, '0'))  # >>> 000000000000000000000000000000aaaaaI love you aaaa

s2 = 'you'
print(s.rindex(s2))  # >>> 12
```



数字型
---

### 整形:

整型(Int) - 通常被称为是整型或整数，是正或负整数，不带小数点

```python
a = 10
```

### 长整型:

长整型(long integers) - 无限大小的整数，整数最后是一个大写或小写的L

注:long长整型旨在python2中出现,python3统一为int型

```python
a = 10L
print(a)
print(type(a))

# >>> 10
# >>> <type 'long'>
```

### 浮点型:

浮点型(floating point real values) - 浮点型由整数部分与小数部分组成，浮点型也可以使用科学计数法表示（2.5e2 = 2.5 x 102 = 250）

```python
a = 3.1415926
print(a)

# >>> 3.1415926
```

### 复数:

复数(complex numbers) - 复数由实数部分和虚数部分构成，可以用a + bj,或者complex(a,b)表示， 复数的实部a和虚部b都

列表
--

### 增:

```python
li = ['a', 'b', 12, 3.12, 'ab']

# 增
li.append(3, 'add')
print(li)
```

### 删:

```python
li = ['a', 'b', 12, 3.12, 'ab']

# 删
li.pop(1)
print(li)
```

### 改:

```python
li = ['a', 'b', 12, 3.12, 'ab']

# 改
li[1] = 'c'
print(li)
```

### 查:

```python
li = ['a', 'b', 12, 3.12, 'ab']

# 查
print(li)
```

### 切片:

num\_li[起始:结束-1:步长]

num = num\_li[-2:] 负数表示从列表反方向(从右往左)查找

```python
num_li = ['a', 'b', 'c', 'd', 'e', 'f']

# 切片
num = num_li[-2:]
print(num)  # ['e', 'f']
num = num_li[2:]
print(num)  # ['c', 'd', 'e', 'f']

num = num_li[:-4]
print(num)  # ['a', 'b']
num = num_li[:4]
print(num)  # ['a', 'b', 'c', 'd']

num = num_li[1:3]
print(num)  # ['b', 'c']

num = num_li[-3:-1]
print(num)  # ['d', 'e']

num = num_li[::2]
print(num)  # ['a', 'c', 'e']
num = num_li[::-2]
print(num)  # ['f', 'd', 'b']

```



### 遍历:

```python
num_li = ['a', 'b', 'c', 'd', 'e', 'f']
num_li2 = [('a', 'b'), ('c', 'd'), ('e', 'f'), ('g', 'h'), ('i', 'j')]
# 迭代
for i in num_li:
    print(i)
'''a
b
c
d
e
f
'''

for i, j in num_li2:
    print(i, j)
'''
a b
c d
e f
g h
i j
'''    

num_li = ['a', 'b', 'c', 'd', 'e', 'f']
# 角标迭代
for index, value in enumerate(num_li):
    print(index, v)
'''
0 a
1 b
2 c
3 d
4 e
5 f
'''    
```

### 常用方法:

```python
list1 = [0, 1, 2, 3, 4, 5, 5, 5,51]
list2 = [6, 7]

# 获取列表元素个数
print(len(list1)) # >>> 9
# 获取列表最大元素
print(max(list1)) # >>> 51
# 获取列表最小元素
print(min(list1)) # >>> 0

# 统计某个元素在列表中出现的次数
print(list1.count(5)) # >>> 3

# 在末尾添加另一个序列的多个值
list1.extend(list2)
print(list1) # >>> [0, 1, 2, 3, 4, 5, 5, 5, 51, 6, 7]

# 查找某个值第一个匹配项的索引位置
print(list1.index(51)) # >>> 8

# 查找某个值的第一次所在的位置并删除
list1.remove(51)
print(list1) # >>> [0, 1, 2, 3, 4, 5, 5, 5, 6, 7]

# 反向列表元素
list1.reverse()
print(list1) # >>> [7, 6, 5, 5, 5, 4, 3, 2, 1, 0]

# 对列表进行排序 reverse = True 降序， reverse = False 升序（默认)
list1.sort(reverse=False)
print(list1) # >>> [0, 1, 2, 3, 4, 5, 5, 5, 6, 7]

```

### 字典:

增:

```python
dict_1 = {'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5}

# 增
dict_1['f'] = 6
print(dict_1) # >>> {'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5, 'f': 6}
```

删:

```python
dict_1 = {'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5}

# 删
del dict_1['f']
print(dict_1) # >>> {'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5}
```

改:

```python
dict_1 = {'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5}

# 改
dict_1['c'] = 6
print(dict_1) # >>> {'a': 1, 'b': 2, 'c': 6, 'd': 4, 'e': 5}
```

查:

```python
dict_1 = {'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5}

# 查
print(dict_1['b']) # >>> 2
```

遍历:

```python
dict_1 = {'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5}

# 遍历

# 1）遍历key值
for key in dict_1.keys():
    print(key, '', dict_1[key])
# >>> a  1
# >>> b  2
# >>> c  3

# 2）遍历value值
for value in dict_1.values():
    print(value)
# >>> 1
# >>> 2
# >>> 3
    
# 3）遍历字典项
for kv in dict_1.items():
    print(kv)
# >>> ('a', 1)
# >>> ('b', 2)
# >>> ('c', 3)
    
# 4）遍历字典健值
for key, value in dict_1.items():
    print(key, '', value)
# >>> a  1
# >>> b  2
# >>> c  3    
```

常用方法:

```python
dict_1 = {'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5}

# 计算字典键的个数
len_num = len(dict_1)
print(len_num) # >>> 3

# 类型转化为字符串
str_dict = str(dict_1)
print(str_dict) # >>> {'a': 1, 'b': 2, 'c': 3}

# 返回一个字典的浅复制
dict_1.copy()

# 创建一个新字典,seq为键,value里的每个元素的集合为每个键的值
seq = ('I', 'love', 'you')
new_dict = dict.fromkeys(seq, '^_^')
print(new_dict) # >>> {'I': '^_^', 'love': '^_^', 'you': '^_^'}

# 判断键值是否在字典内 python2:dict.has_key()牙龈出血和洗牙两概念吧
key = dict_1.__contains__('a')
print(key) # >>> True

# 将字典键值转换为元祖,以列表形式返回[(key,value),(key,value),(key,value)]
items = dict_1.items()
print(items) # >>> dict_items([('a', 1), ('b', 2), ('c', 3)])

# 以列表返回字典里所有的键
key = dict_1.keys()
print(key) # >>> dict_keys(['a', 'b', 'c'])

# 以列表返回字典里所有的值
value = dict_1.values()
print(value) # >>> dict_values([1, 2, 3])

# 把dict2字典的键值更新到dict1
dict_2 = {'d': 4, 'e': 5}
dict_1.update(dict_2)
print(dict_1) # >>> {'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5}

# 返回指定键的值,如果没有返回default
value = dict_1.get('a')
print(value) # >>> 1

# 返回指定键的值,如果没有则添加键值为default
key_value = dict_1.setdefault('num', None)
print(key_value) # >>> None

# 删除指定键的值(返回值为被删除的值)
item = dict_1.pop('b')
print(item) # >>> 2
print(dict_1) # >>> {'a': 1, 'c': 3, 'd': 4, 'e': 5, 'num': None}

# 随机删字典中的键的值(返回值为被删除的值)
item = dict_1.popitem()
print(item) # >>> ('num', None)
print(dict_1) # >>> {'a': 1, 'c': 3, 'd': 4, 'e': 5}

# 删除字典内所有元素
dict_1.clear()
print(dict_1) # >>> {}

```

### set

创建一个集合:

set()和{}

注意:创建一个空集合set{}, 而不是 { }，因为 { } 是用来创建一个空字典。

```python
# 创建集合 & 去重

s = {'a', 'b', 'c', 'd', 'd', 'e'}
print(s)  # >>> {'a', 'b', 'c', 'd', 'e'}
```

判断是否在集合内

```python
print('a' in s)  # >>> True
print('f' in s)  # >>> False
```

运算

```python
a = set('abracadabra')
b = set('alacazam')
print(a - b)  # >>> {'r', 'b', 'd'}  # 集合a中包含而集合b中不包含的元素
print(a | b)  # >>> {'l', 'm', 'z', 'r', 'c', 'a', 'b', 'd'}  # 集合a或b中包含的所有元素
print(a & b)  # >>> {'a', 'c'} # 集合a和b中都包含了的元素
print(a ^ b)  # >>> {'r', 'l', 'm', 'b', 'd', 'z'} # 不同时包含于a和b的元素
```

增

```python
# 增
s.add('f')
print(s) # >>> {'f', 'e', 'd', 'c', 'a', 'b'}
# 增加多个元素
s.update(('g', 'h', 'i'))
print(s)  # >>> {'a', 'd', 'b', 'c', 'h', 'g', 'i', 'f', 'e'}
```

删

```python
# 删
s.remove('f')
print(s)  # >>> {'g', 'a', 'e', 'i', 'h', 'c', 'd', 'b'}
# 删除一个不存在的元素 不会报错
s.discard('z')
print(s)  # >>> {'g', 'a', 'e', 'i', 'h', 'c', 'd', 'b'}

# 随机删除元素
s.pop()
print(s)  # >>> {'g', 'a', 'e', 'h', 'c', 'd', 'b'}
```

常用方法

```python
s = {'a', 'b', 'c', 'd', 'd', 'e'}
s2 = {'d', 'e', 'f', 'g', 'h'}

# copy()	拷贝一个集合
a = s.copy()
print(a)  # >>> {'c', 'b', 'd', 'e', 'a'}

# clear() 移除集合中的所有元素
print(a.clear())  # >>> None

# difference() 返回多个集合的差集
print(s.difference(s2))  # >>> {'c', 'a', 'b'}

# intersection()	返回集合的交集(返回新的集合)
print(s.intersection(s2))  # >>> {'d', 'e'}

s = {'a', 'b', 'c', 'd', 'd', 'e'}
s2 = {'d', 'e', 'f', 'g', 'h'}

# intersection_update()	删除集合中的元素，该元素在指定的集合中不存在。(不创建新的集合,在本集合中做删除处理)
s.intersection_update(s2)
print(s)  # >>> {'e', 'd'}

s = {'a', 'b', 'c', 'd', 'd', 'e'}
s2 = {'d', 'e', 'f', 'g', 'h'}
# difference_update() 移除集合中的元素，该元素在指定的集合也存在。
s.difference_update(s2)
print(s)  # >>> {'a', 'c', 'b'}

# isdisjoint()	判断两个集合是否包含相同的元素，如果没有返回 True，否则返回 False。
print(s.isdisjoint(s2))  # >>> True

# issubset() 判断指定集合是否为该方法参数集合的子集。
print(s.issubset(s2))  # >>> False  (s{'b', 'a', 'c'} s2{'g', 'd', 'h', 'e', 'f'}

# issuperset()	判断该方法的参数集合是否为指定集合的子集
print(s2.issuperset(s))  # >>> False  (s{'b', 'a', 'c'} s2{'g', 'd', 'h', 'e', 'f'}

s = {'a', 'b', 'c', 'd', 'd', 'e'}
s2 = {'d', 'e', 'f', 'g', 'h'}
# symmetric_difference()	返回两个集合中不重复的元素集合。
print(s.symmetric_difference(s2))  # >>> {'f', 'c', 'h', 'a', 'g', 'b'}

# symmetric_difference_update()	移除当前集合中在另外一个指定集合相同的元素，并将另外一个指定集合中不同的元素插入到当前集合中。
s.symmetric_difference_update(s2)
print(s)  # >>> {'c', 'a', 'b', 'g', 'h', 'f'}

# union()	返回两个集合的并集
print(s.union(s2))  # >>> {'g', 'b', 'c', 'f', 'e', 'a', 'd', 'h'}

```

### 元组

创建元组

```python
tup = ('a', 'b', 'c', 'd')
```

改

```python
tup1 = (1, 2, 3)
tup2 = (4, 5, 6)
tup = tup1 + tup2
print(tup)  # >>> (1, 2, 3, 4, 5, 6)
```

删

```python
del tup
print(tup)  # >>> NameError: name 'tup' is not defined
```

常用方法

```python
tup = (1, 2, 3)

# 计算元组个数
print(len(tup))

# 返回元组中元素最大值
print(max(tup))

# 返回元组中元素最小值
print(min(tup))

# 切片
print(tup[0:1])

# 迭代
for i in tup: print(i)

# 元素是否存在
print(1 in tup)

# 复制(元组后加",")
print(('a',) * 10)

```

### 布尔

True

False

条件判断
====

if/else/elif
------------

```python
a = 1
b = 2
if a > b:
    print(a)
elif b > a:
    print(b)  # >>> 2
else:
    print(a + b)
```

逻辑运算符
-----

### 算术运算符

| +  | 加 - 两个对象相加                        | a + b 输出结果 31 |
| -- | ----------------------------------------------- | ------------------ |
| -  | 减 - 得到负数或是一个数减去另一个数 | a - b 输出结果 -11 |
| *  | 乘 - 两个数相乘或是返回一个被重复若干次的字符串 | a * b 输出结果 210 |
| /  | 除 - x 除以 y                                | b / a 输出结果 2.1 |
| %  | 取模 - 返回除法的余数                  | b % a 输出结果 1 |
| ** | 幂 - 返回x的y次幂                         | a**b 为10的21次方 |
| // | 取整除 - 向下取接近除数的整数      | >>> 9//2=4       |

### 比较（关系）运算符

| 运算符 | 描述                                                                                                                     | 实例                |
| ------ | -------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| ==     | 等于 - 比较对象是否相等                                                                                          | (a == b) 返回 False。 |
| !=     | 不等于 - 比较两个对象是否不相等                                                                              | (a != b) 返回 True。 |
| >      | 大于 - 返回x是否大于y                                                                                              | (a > b) 返回 False。 |
| <      | 小于 - 返回x是否小于y。所有比较运算符返回1表示真，返回0表示假。这分别与特殊的变量True和False等价。注意，这些变量名的大写。 | (a < b) 返回 True。 |
| >=     | 大于等于 - 返回x是否大于等于y。                                                                               | (a >= b) 返回 False。 |
| <=     | 小于等于 - 返回x是否小于等于y。                                                                               | (a <= b) 返回 True。 |

### 赋值运算符

| 运算符 | 描述           | 实例                                |
| ------ | ---------------- | ------------------------------------- |
| =      | 简单的赋值运算符 | c = a + b 将 a + b 的运算结果赋值为 c |
| +=     | 加法赋值运算符 | c += a 等效于 c = c + a            |
| -=     | 减法赋值运算符 | c -= a 等效于 c = c - a            |
| *=     | 乘法赋值运算符 | c *= a 等效于 c = c * a            |
| /=     | 除法赋值运算符 | c /= a 等效于 c = c / a            |
| %=     | 取模赋值运算符 | c %= a 等效于 c = c % a            |
| **=    | 幂赋值运算符 | c **= a 等效于 c = c ** a          |
| //=    | 取整除赋值运算符 | c //= a 等效于 c = c // a          |

### 逻辑运算符

| 运算符 | 逻辑表达式 | 描述                                                                  | 实例                  |
| ------ | ---------- | ----------------------------------------------------------------------- | ----------------------- |
| and    | x and y    | 布尔"与" - 如果 x 为 False，x and y 返回 False，否则它返回 y 的计算值。 | (a and b) 返回 20。  |
| or     | x or y     | 布尔"或" - 如果 x 是 True，它返回 x 的值，否则它返回 y 的计算值。 | (a or b) 返回 10。   |
| not    | not x      | 布尔"非" - 如果 x 为 True，返回 False 。如果 x 为 False，它返回 True。 | not(a and b) 返回 False |

### 位运算符

| 运算符 | 描述                                                                                          | 实例                                                                         |
| ------ | ----------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| &      | 按位与运算符：参与运算的两个值,如果两个相应位都为1,则该位的结果为1,否则为0 | (a & b) 输出结果 12 ，二进制解释： 0000 1100                        |
| \|      | 按位或运算符：只要对应的二个二进位有一个为1时，结果位就为1。       | (a \| b) 输出结果 61 ，二进制解释： 0011 1101                        |
| ^      | 按位异或运算符：当两对应的二进位相异时，结果为1                          | (a ^ b) 输出结果 49 ，二进制解释： 0011 0001                        |
| ~      | 按位取反运算符：对数据的每个二进制位取反,即把1变为0,把0变为1。~x 类似于 -x-1 | (~a ) 输出结果 -61 ，二进制解释： 1100 0011， 在一个有符号二进制数的补码形式。 |
| <<     | 左移动运算符：运算数的各二进位全部左移若干位，由"<<"右边的数指定移动的位数，高位丢弃，低位补0。 | a << 2 输出结果 240 ，二进制解释： 1111 0000                        |
| >>     | 右移动运算符：把">>"左边的运算数的各二进位全部右移若干位，">>"右边的数指定移动的位数 | a >> 2 输出结果 15 ，二进制解释： 0000 1111                         |

### 成员运算符

| 运算符 | 描述                                                  | 实例                                            |
| ------ | ------------------------------------------------------- | ------------------------------------------------- |
| in     | 如果在指定的序列中找到值返回 True，否则返回 False。 | x 在 y 序列中 , 如果 x 在 y 序列中返回 True。 |
| not in | 如果在指定的序列中没有找到值返回 True，否则返回 False。 | x 不在 y 序列中 , 如果 x 不在 y 序列中返回 True。 |

### 身份运算符

| 运算符 | 描述                                      | 实例                                                                                       |
| ------ | ------------------------------------------- | -------------------------------------------------------------------------------------------- |
| is     | is 是判断两个标识符是不是引用自一个对象 | x is y, 类似 id(x) == id(y) , 如果引用的是同一个对象则返回 True，否则返回 False |
| is not | is not 是判断两个标识符是不是引用自不同对象 | x is not y ， 类似 id(a) != id(b)。如果引用的不是同一个对象则返回结果 True，否则返回 False。 |

### 运算符优先级

| 运算符                | 描述                                                 |
| ------------------------ | ------------------------------------------------------ |
| **                       | 指数 (最高优先级)                               |
| ~ + -                    | 按位翻转, 一元加号和减号 (最后两个的方法名为 +@ 和 -@) |
| * / % //                 | 乘，除，取模和取整除                         |
| + -                      | 加法减法                                           |
| >> <<                    | 右移，左移运算符                               |
| &                        | 位 'AND'                                              |
| ^ \|                      | 位运算符                                           |
| <= < > >=                | 比较运算符                                        |
| <> == !=                 | 等于运算符                                        |
| = %= /= //= -= += *= **= | 赋值运算符                                        |
| is is not                | 身份运算符                                        |
| in not in                | 成员运算符                                        |
| and or not               | 逻辑运算符                                        |

循环
==

while
-----

```python
# 1~100的和:

i = 1
sum = 0
while i <= 100:
    sum = sum + i
    i += 1

print(sum)
n = 100  # >>> 5050
```

for
---

```python
# 循环遍历
li = [1, 2, 3, 4, 5, 6, 7]
for i in li:
    print(i)
```

break/continue
--------------

continue:跳出本次循环(仅本次次)

break:跳出循环

```python
li = [1, 2, 3, 4, 5, 6, 7]
for i in li:
    if i == 3:
        continue
        print('HELLO')
    elif i == 5:
        break
        print('Hi')
# >>> 不输出任何内容
```

pass
----

不做任何动作

推导式
===

列表推导式
-----

```python
print([i ** 2 for i in range(0, 10) if i % 3])  # >>>[1, 4, 16, 25, 49, 64]
```

字典推导式
-----

```python
mcase = {'a': 10, 'b': 34, 'A': 7, 'Z': 3}

mcase_frequency = {k.lower(): mcase.get(k.lower(), 0) + mcase.get(k.upper(), 0) for k in mcase.keys() ifk.lower() in ['a', 'b']}

print(mcase_frequency)  # >>>{'a': 17, 'b': 34}
```

集合推导式
-----

```python
print({x ** 2 for x in [1, 1, 2]})  # >>> {1, 4}
```

函数
==

定义函数时，需要确定函数名和参数个数；

如果有必要，可以先对参数的数据类型做检查；

函数体内部可以用return随时返回函数结果；

函数执行完毕也没有return语句时，自动return None。

函数可以同时返回多个值，但其实就是一个tuple。

定义
--

```python
def my_def():
    return 0
```

调用
--

```python
def my_def():
    return 0

my_def()
```

返回值
---

```python
def my_def():
    return 0


print(my_def())  # >>> 0
```

参数
--

```python
def my_def(num):
    return num


print(my_def(2))  # >>> 2
```

### 缺省参数

```python
# 缺省参数
def my_def(a, b, c):
    return a, b, c


# 缺省参数 指定参数名
def my_def2(b=2, a=1, c=3):
    return a, b, c


print(my_def(1, 2, 3))  # >>> (1, 2, 3)
print(my_def2(a=4, b=5, c=6))  # >>> (4, 5, 6)
```

### 不定长参数

```python
def my_def(*args, **kwargs):
    return args, kwargs


# * 以列表的形式传进去多个值
# ** 以键值对的形式传进多个值
print(my_def([2, 3, 4], a=5, c=6))
```

局部变量
----

```python
# 全局变量
a = 1


def my_def():
    # 局部变量
    b = 1
    return a


print(a)  # >>> 1
print(b)  # >>> NameError: name 'b' is not defined
```

### 修改局部变量global

注意:global 同时声明和赋值(global b = 2) 是不被允许的

```python
# 全局变量
a = 1


def my_def():
    # 局部变量
    global b
    b = 2
    return b


my_def()
print(a)  # >>> 1
print(b)  # >>> 2
```

嵌套
--

```python
# 全局变量
a = 1


def my_def():
    def my_def2():
        print(1)

    my_def2()
    print(2)


my_def()  # >>> 1 2
```

函数文档
----

```python
def add(a, b):
    """
    :param a: 第一个加数
    :param b: 第二个加数
    :return: 返回两个加数的和
    """
    return (a + b)


print(add.__doc__)
# >>>   
#     :param a: 第一个加数
#     :param b: 第二个加数
#     :return: 返回两个加数的和

print(add(1, 2))  # >>> 3
```

模块
==

import
------

```python
import os

listdir = os.listdir('./')
print(listdir)  # >>> ['t.py', '.idea']
```

from .. import ..
-----------------

```python
from flask import Flask

app = Flask(__name__)
app.debug = True
```

面向对象
====

定义类
---

```python
# class 类名:
#     属性
#     方法
```

实例属性
----

### 添加

```python
class Car:
    def push(self):
        print("向前走")

    def stop(self):
        print("停止")


# 实例
audi = Car()
# 添加实例属性
audi.color = 'yellow'
```

### 获取

```python
class Car:
    def push(self):
        print("向前走")

    def stop(self):
        print("停止")


# 实例
audi = Car()
# 添加实例属性
audi.color = 'yellow'

# 获取实例属性
color = audi.color
print(color)  # >>> yellow
```

self变量
------

变量用来接收你给他的引用

```python
class Car:
    def push(self):
        print("向前走")

    def stop(self):
        print("停止")

    def my_car(self):
        print("car name is %s,car color is %s" % (self.name, self.color))


# 实例
audi = Car()
# 添加实例属性
audi.name = 'Audi'
audi.color = 'yellow'

# 方法获取实例属性
audi.my_car()  # >>> car name is Audi,car color is yellow
```

\_\_init\_\_
------------

完成对象的初始化

```python
class Car:
    def __init__(self, new_name, new_color):
        print("__init__方法会被自动调用")
        self.name = new_name
        self.color = new_color

    def push(self):
        print("向前走")

    def stop(self):
        print("停止")

    def my_car(self):
        print("car name is %s,car color is %s" % (self.name, self.color))


# 实例
audi = Car('Audi','yellow')
# 方法获取实例属性
audi.my_car()
# >>> __init__方法会被自动调用
# >>> car name is Audi,car color is yellow

Mercedes_Benz = Car('Mercedes-Benz','pink')
Mercedes_Benz.my_car()
# >>> __init__方法会被自动调用
# >>> car name is Mercedes-Benz,car color is pink
```

\_\_str\_\_
-----------

返回对象的描述信息

注意:\_\_str\_\_方法,必须有return返回值

```python
class Car:
    def __init__(self, new_name, new_color):
        self.name = new_name
        self.color = new_color

    def push(self):
        print("向前走")

    def stop(self):
        print("停止")

    def my_car(self):
        print("car name is %s,car color is %s" % (self.name, self.color))

    def __str__(self):
        return "This is MyCar,name is {}".format(self.name)


# 实例
audi = Car('Audi', 'yellow')

# 如果不自定义对象描述直接打印对象:
# print(audio) 输出信息为内存地址:<__main__.Car object at 0x7fcc8004c898>

# 利用__str__定义对象描述,输出为我们定义好的字符串
# 获取我们自己定义的对象描述信息
print(audi)
```

私有/继承/多态
--------

### 私有属性

定义:\_\_两个下划线开头的变量名,设为私有属性

私有属性:只能在方法内使用,不允许实例对象直接调用

设计:一个私有属性,搭配两个方法.一个负责接收参数并判断过滤合法性,一个负责返回相对应的参数方便使用

```python
class Car:
    def __init__(self):
        # 私有属性
        self.__age = 0

    def set_Color(self,color=None):
        if color in ['yellow', 'blue', 'red']:
            self.__color = color

    def get_color(self):
        return self.__color


Audi = Car()
Audi.set_Color('red')
print(Audi.get_color())  # >>> red

Audi.set_Color('pink')
print(Audi.get_color())  # >>> red
```

### 私有方法

隐藏核心代码,无法轻易改变

```python
class Car():
    def __init__(self, car_name):
        self.name = car_name

    def __run(self):
        print(self.name + ' ' + 'run...')

    def run(self, currency):
        if currency > 0:
            self.__run()


Audi = Car('R8')
Audi.run(1)
```

### 继承

单继承

```python
class Action:
    def run(self):
        print('run')

    def stop(self):
        print("stop")


class Car(Action):
    pass


class Bicycle(Action):
    pass


Audi = Car()
Audi.run()  # >>> run
Giant = Bicycle()
Giant.stop()  # >>> stop
```

多层继承

```python
class Action:
    def run(self):
        print('run')

    def stop(self):
        print("stop")


class Car(Action):
    def __init__(self, name):
        self.car_name = name

    def name(self):
        print(self.car_name)


class Bicycle(Action):
    pass


class Audi(Car):
    def quickly(self):
        print("Run quickly.....")


R8 = Audi()
R8.run()  # >>> run
R8.name('R8')  # >>> R8
R8.quickly()  # >> Run quickly.....
```

多继承

```python
class Mercedes_Benz():

    def run(self):
        print('run...')


class Audi():

    def stop(self):
        print('stop...')


class Car(Mercedes_Benz, Audi):

    def Action(self):
        Mercedes_Benz.run(self)
        Audi.stop(self)


car = Car()
car.Action()
# >>> run...
# >>> stop...
```

### 重写父类方法

重写

在定义的类中重新定义一个和父类相同的方法名,这样父类的方法就会被覆盖,实现重写父类方法的效果

```python
class Action:
    def run(self):
        print('run')

    def stop(self):
        print("stop")


class Car(Action):
    def __init__(self, name):
        self.car_name = name

    def name(self):
        print(self.car_name)


class Audi(Car):
    def quickly(self):
        print("Run quickly.....")

    def run(self):
        print('slow run')


R8 = Audi('R8')
R8.run()  # >>> slow run
R8.name()  # >>> R8
R8.quickly()  # >>> Run quickly.....
```

调用被重写的父类方法

```python
class Action:
    def run(self):
        print('run')

    def stop(self):
        print("stop")


class Audi(Action):
    def quickly(self):
        print("Run quickly.....")

    def run(self):
        # 被重写的父类
        print('slow run')
        # 第一种方式调用被重写方法
        Action.run(self)
        # 第二种方式调用被重写方法
        super().run()
        # 第三种方式调用被重写方法
        super(Audi, self).run()


R8 = Audi()
R8.run()
# >>> slow run
# >>> run
# >>> run
# >>> run

R8.quickly()  # >>> Run quickly.....

```

### 多态

python是解释型弱类型语言,所以支持多态的同时不需要特别声明,所以不需要额外关注.

```python
class Mercedes_Benz():

    def run(self):
        print('run...')


class Audi():

    def run(self):
        print('run...')


def car(temp):
    temp.run()


audi = Audi()
car(audi)  # >>> run...

mercedes_benz = Mercedes_Benz()
car(mercedes_benz)  # >>> run...
```

类属性
---

```python
class Student(object):
    # 类属性
    num = 0

    def __init__(self, name):
        # 实例属性
        self.name = name
        # 类对象创建类
        # 类创建实例对象
        # 创建类对象的同时,创建类属性
        # 所以类属性不会随着创建新的实例对象的改变而改变,但实例属性会被__init__初始化
        Student.num += 1  # 调用类属性

    def student_name(self):
        print(self.name)


Tom = Student('Tom')
JC = Student('JC')
Peter = Student('Peter')

print(Student.num)

```

实例对象共享类属性资源
-----------

```python
class Student(object):
    # 类属性
    num = 0

    def __init__(self, name):
        # 实例属性
        self.name = name
        Student.num += 1  # 调用类属性

    def student_name(self):
        print(self.name)
        print(self.num)


Tom = Student('Tom')
JC = Student('JC')
Peter = Student('Peter')

print(Peter.name)  # >>> Peter


# 类属性可以被实例对象直接调用,在实例属性中如果找不到需要调用的属性,那么实例就会在类属性中寻找
# 类属性是各个实例对象中共享的资源
print(Peter.num)  # >>> 3
```

类方法
---

```python
class Student(object):
    # 类属性
    num = 0

    def __init__(self, name):
        # 实例属性
        self.name = name
        Student.num += 1  # 调用类属性

    def student_name(self):
        print(self.name)
        self.num = 100
        print(self.num)

    # 定义类方法,利用@classmethod
    # cls表示:当前的类对象
    @classmethod
    def edit(cls):
        cls.num = 200


Tom = Student('Tom')
JC = Student('JC')
Peter = Student('Peter')

print(Peter.num)  # >>> 3  调用了类属性num的值
Peter.num = 100
print(Peter.num)  # >>> 100  并不是修改了类属性num,而是创建了实例属性num给Peter实例

print(JC.num)  # >>> 3  Peter实例并不是真的改动了类属性, 所以类属性依然为3


# 类属性只能由类方法进行修改
Student.edit()  # 运行类方法
print(Tom.num)  # >>> 200
print(JC.num)  # >>> 200
print(Peter.num)  # >>> 100 因为之前Peter实例对象添加了一个num=100的属性(12行)
```

### 利用类方法批量创建实例对象

```python
class Student(object):
    # 类属性
    num = 0

    def __init__(self, name):
        # 实例属性
        self.name = name
        Student.num += 1  # 调用类属性

    def student_name(self):
        print(self.name)
        self.num = 100
        print(self.num)


    def show(self):
        print('班级一共有多少人?')

    @classmethod
    def creat_student(cls, names):
        objs = []
        for name in names:
            objs.append(cls(name))

        return objs


students = Student.creat_student(['Tom', 'JC', 'Peter'])
students[0].show()  # >> 班级一共有多少人?
print(students[0].num)  # >> 3
```

静态方法
----

```python
class Student(object):
    # 类属性
    num = 0

    def __init__(self, name):
        # 实例属性
        self.name = name
        Student.num += 1  # 调用类属性

    def student_name(self):
        print(self.name)
        self.num = 100
        print(self.num)
        
    # 定义一个静态方法
    # 静态方法不需要往里面传入self或其他的值
    @staticmethod
    def show():
        print('班级一共有多少人?')


Tom = Student('Tom')
JC = Student('JC')
Peter = Student('Peter')

Peter.show()  # >> 班级一共有多少人?
print(JC.num)  # >> 3

```

工厂模式
----

### 简单工厂模式

简单工厂模式可以理解为流水线又或者是多人作业模式.

三跟人进行分配:

1:维护学生以及学生的行为(1~13行)

2:维护班级学生名单(16~26行)

3:维护'点名’ 这一行为(29~35行)

三个人可以进行接口的制定,并不需要关心对方是如何实现的,直接调用规定好的函数接口就可以.大大减少沟通成本提高效率

```python
class Tom(object):
    def talk_hi(self):
        print('Here!')


class Peter(object):
    def talk_hi(self):
        print('coming!')


class JC(object):
    def talk_hi(self):
        print('yeah!')


class Classes(object):

    def name(self, name):
        if name == 'Tom':
            student = Tom()
        elif name == 'JC':
            student = JC()
        elif name == 'Peter':
            student = Peter()

        return student


class Call_The_Roll(object):
    def __init__(self):
        self.call_the_roll = Classes()

    def name(self, name):
        names = self.call_the_roll.name(name)
        return names

call_the_roll = Call_The_Roll()
student_name = call_the_roll.name('Tom')
student_name.talk_hi()
```

### 工厂方法模式

创建一个父类school,在父类中定义可以实现的功能(创建实例对象的方法),而真正实现动作将全部交给子类,选择了使用了哪个子类，自然也就决定了实际创建的动作是什么。

School类相当于各个子类的总调度,真正实现功能的是子类,但是能够实现什么动作,我们查看父类School便可知道

```python
class School(object):
    # 点名
    def call_The_Roll(self, name):
        pass

    # 上操
    def March(self):
        pass

    # 开会
    def Meeting(self):
        pass

    def school_main(self, name):
        
        # 根据名字喊到
        self.call_the_roll = self.call_The_Roll(name)

        # 上操
        self.march = self.March()

        # 开会
        self.meeting =self.Meeting()

        # 将喊到返回给老师
        return self.call_the_roll


class Tom(object):
    def talk_hi(self):
        print('Here!')


class Peter(object):
    def talk_hi(self):
        print('coming!')


class JC(object):
    def talk_hi(self):
        print('yeah!')


class Classes(object):

    def name(self, name):
        if name == 'Tom':
            student = Tom()
        elif name == 'JC':
            student = JC()
        elif name == 'Peter':
            student = Peter()

        return student


class Call_The_Roll(School):
    def __init__(self):
        self.call_the_roll = Classes()

    def name(self, name):
        names = self.call_the_roll.name(name)
        return names


call_the_roll = Call_The_Roll()
student_name = call_the_roll.name('JC')
student_name.talk_hi()

```

单例模式
----

无论创建多少次实例对象,都只创建一次

### \_\_new\_\_方法

\_\_new\_\_方法:创建一个类,在基类object已经写好了一个\_\_new\_\_方法,作用就是新建一个新的类模板

cls参数:类对象的引用

返回值:创建一个类对象必须调用object.\_\_new\_\_()方法并返回创建成功的类对象.

注意:

python的构造方法分为两步:

\_\_new\_\_创建

\_\_init\_初始化

```python
class Car(object):
    def __new__(cls):
        print('__new__')
        return object.__new__(cls)

    def __init__(self):
        print('__init__')


Audi = Car()
# >>> __new__
# >>> __init__
```

### 创建单例类

重写父类\_\_new\_\_方法,并且定义一个类属性.将类对象的引用作为判断,如果没有这个引用那我们就创建一个,如果已经存在那我们将存在的引用返回给创建的实例.这样我们就无法多次创建新的实例了.

```python
class Car:
    __instance = None

    def __new__(cls):
        if cls.__instance == None:
            cls.__instance = object.__new__(cls)
            return cls.__instance
        else:
            return cls.__instance


Audi = Car()
Mercedes_Benz = Car()

print(Audi)  # >>> <__main__.Car object at 0x7feae003e320>
print(Mercedes_Benz)  # >>> <__main__.Car object at 0x7feae003e320>
```

### 初始化一次

def \_\_new\_\_(cls, \*args, \*\*kwargs):

在创建实例对象时,先运行\_\_new\_\_方法,所以创建时我们传递了cls参数,没有其他位置传递'Audi'这个参数.所以我们需要添加(\*args, \*\*kwargs)不定长参数

\_\_first\_init = True:

和判断是否重复穿件类对象一样,我们添加一个属性,在\_\_init\_\_初始化后,我们将\_\_first\_init = True改为False,这样就不会重复初始化了

```python
class Car:
    __instance = None
    __first_init = True

    def __new__(cls, *args, **kwargs):
        if cls.__instance == None:
            cls.__instance = object.__new__(cls)
            return cls.__instance
        else:
            return cls.__instance

    def __init__(self, car_name):
        if Car.__first_init == True:
            self.name = car_name
            Car.__first_init = False


car = Car('Audi')
car_2 = Car('Mercedes_Benz')

print(car)  # >>> <__main__.Car object at 0x7feae003e320>
print(car_2)  # >>> <__main__.Car object at 0x7feae003e320>

print(car.name)  # >>> Audi
print(car_2.name)  # >>> Audi
```

异常处理/调试
=======

异常
--

### 捕获异常

```python
# try... except...
try:
    open('test.txt')
    a = 1 / 0
except:
    print('程序错误')
# >>> 程序错误
```

捕获特定异常

```python
try:
    open('test.txt')
    a = 1 / 0
except ZeroDivisionError:
    print('程序错误')

# >>> FileNotFoundError: [Errno 2] No such file or directory: 'test.txt'
```

捕获多个异常

```python
try:
    a = 1 / 0
    open('test.txt')
except (ZeroDivisionError,FileNotFoundError) as result:
    print('程序错误')

# >>> 程序错误
```

不同异常处理

```python
try:
    a = 0 / 1
    print(b)
    open('test.txt')
except (ZeroDivisionError, FileNotFoundError) as result:
    print('程序错误')
except NameError:
    print('不同异常处理')

# >>> 不同异常处理
```

获取异常信息

```python
try:
    a = 1 / 0
except (ZeroDivisionError) as result:
    print('分母不能为零')
    print(result)

# >>> 程序错误
# >>> division by zero
```

### 没有异常则执行

```python
try:
    a = 0 / 1
except (ZeroDivisionError) as result:
    print('分母不能为零')
    print(result)
else:
    print('程序执行成功')

# >>> 程序执行成功
```

### 无论是否有异常都执行

```python
try:
    a = 0 / 1
    open('test.txt')
except (ZeroDivisionError) as result:
    print('分母不能为零')
    print(result)
else:
    print('程序执行成功')
finally:
    print('无论是否有异常都执行')
# >>> 无论是否有异常都执行
# >>> FileNotFoundError: [Errno 2] No such file or directory: 'test.txt'
```

异常的传递
-----

内部的异常捕获后无法对这个错误进行处理,所以内部的异常会传递到外面的异常中

注意:调用了一个函数,函数中产生了异常,并不是函数产生了异常,而是函数把异常传给了调用者.是调用者产生了异常

```python
import time

try:
    f = open('test.txt')
    try:
        while True:
            content = f.readline()
            if len(content) == 0:
                break
            time.sleep(2)
            print(content)
        print(1/0)  # 错误代码,内部的异常捕获后无法对这个错误进行处理,所以内部的异常会传递到外面的异常中
    finally:
        f.close()
        print('关闭文件')
except Exception as e:
    print('没有这个文件')  # >>> 没有这个文件
    print(e)  # >>> division by zero
    
# 注意:
# def test():
#     print(1 / 0)

# user =  test()  # 函数中产生的异常传递给了被调用者,所以是user产生了异常
```

抛出自定义的异常
--------

定义异常:

创建一个子类,继承于Exception

raise引发 自定义的异常创建异常类的实例对象,如果自定义的异常类需要实例属性参数则传入相应的参数

except会从Exception基类里找到自定义的异常类createException的实例对象并获得这个异常实例对象的初始化结果和\_\_str\_\_说明结果.我们如果需要可以从Exception获取到异常类实例对象运行后的结果

获取结果(13行)也可改成(except Exception as e:):

except createException as e:

 print(e)

```python
class createException(Exception):
    def __init__(self, type):
        print('{}{}字符串输入错误!'.format(type, ' '))

    def __str__(self):
        return '请输入字符数量大于3的字符串'


try:
    t = 'ab'
    if len(t) < 3:
        raise createException(t)
except createException as e:
    print(e)
# >>> ab 字符串输入错误!
# >>> 请输入字符数量大于3的字符串
```

异常处理中抛出异常
---------

当正常捕获到一个异常后进行正常的处理操作,此时我们可以选择在这个异常区域内(try... except…)选择是否处理,如果不想在本区域内处理异常,可以通过raise关键字把这个异常再一次抛出,如果外层还有try... except...则可以被捕获,再进行处理.

```python
class add(object):
    def __init__(self, bool):
        self.bool = bool

    def add(self, a, b):
        try:
            try:
                return a / b
            except Exception as e:
                if self.bool:
                    print(e)
                else:
                    raise
        except Exception as e:
            return e


calc = add(True)
print(calc.add(0, 1))  # >>> 0.0
calc = add(False)
print(calc.add(1, 0))  # >>> division by zero
```

调试
--

assert expression [, arguments]

assert 表达式 [, 参数]

assert len(lists) \>=5,'列表元素个数小于5'

assert 2==1,'2不等于1'

```python
def num(a, b):
    # 如果断言失败，assert语句本身就会抛出AssertionError：AssertionError: 分母不能为0
    try:
        assert b != 0, '分母不能为0'
        print(a / b)
    except AssertionError:
        print('重新输入!')


num(10, 0)  # >>> 重新输入!
```

IO文件处理
======

参数:open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)

* ile: 必需，文件路径（相对或者绝对路径）。
* mode: 可选，文件打开模式
* buffering: 设置缓冲
* encoding: 一般使用utf8
* errors: 报错级别
* newline: 区分换行符
* closefd: 传入的file参数类型
* opener:

open()参数:不同模式打开文件的完全列表：

| 模式 | 描述                                                                                                                                                             |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| r    | 以只读方式打开文件。文件的指针将会放在文件的开头。这是默认模式。                                                                   |
| rb   | 以二进制格式打开一个文件用于只读。文件指针将会放在文件的开头。这是默认模式。                                                 |
| r+   | 打开一个文件用于读写。文件指针将会放在文件的开头。                                                                                        |
| rb+  | 以二进制格式打开一个文件用于读写。文件指针将会放在文件的开头。                                                                      |
| w    | 打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。 |
| wb   | 以二进制格式打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。 |
| w+   | 打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。 |
| wb+  | 以二进制格式打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。 |
| a    | 打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。 |
| ab   | 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。 |
| a+   | 打开一个文件用于读写。如果该文件已存在，文件指针将会放在文件的结尾。文件打开时会是追加模式。如果该文件不存在，创建新文件用于读写。 |
| ab+  | 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。如果该文件不存在，创建新文件用于读写。 |

打开文件
----

```python
# 打开文件
f = open('test.txt', 'w')
```

写入文件
----

```python
# 打开文件
f = open('test.txt', 'w')
# 写入文件
f.write('helloworld')
```

读取文件
----

```python
# 打开文件
f = open('test.txt', 'r')
# 读取文件
content = f.read()
f.close()

print(content)
```

关闭文件
----

```python
# 打开文件
f = open('test.txt', 'w')
# 写入文件
f.write('helloworld')
# 关闭文件
f.close()
```

常用方法
----

```python
f = open('test.txt', 'r+')
# 1
# 2
# 3
# 4
# 5
# 6
# 7

# 刷新文件内部缓冲，直接把内部缓冲区的数据立刻写入文件, 而不是被动的等待输出缓冲区写入。
f.flush()

# 读取整行，包括"\n"字符。
print(f.readline())  # >>> 1

# 读取所有行并返回列表，若给定sizeint > 0，则是设置一次读多少字节，这是为了减轻读取压力。
print(f.readlines())  # >>> ['2\n', '3\n', '4\n', '5\n', '6\n', '7']

# 设置文件当前位置
print(f.seek(2))  # >>> 2

# 返回文件当前位置。
print(f.tell())  # >>> 2

# 截取文件，截取的字节通过size指定，默认为当前文件位置。
f.truncate()
print(f.read())

# 将字符串写入文件，返回的是写入的字符长度。
f = open('test.txt', 'w')
f.write('hello')

# 向文件写入一个序列字符串列表，如果需要换行则要自己加入每行的换行符。
seq = ['\n', 'abcd\n', 'efgh']
f.writelines(seq)
f.close()
# hello
# abcd
# efgh
```

自动打开关闭文件
--------

```python
with open('test.txt', 'r') as f:
    r = f.read()

print(r)  # >>> 123456
```



