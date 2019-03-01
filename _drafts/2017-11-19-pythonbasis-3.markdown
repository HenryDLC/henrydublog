---
layout: "post"
title: "python(基础三)"
date: "2017-11-19 16:23"
---
函数:
===

\# 函数名只能包含字母 数字 下划线,并且数字不能当开头

\# 函数的参数叫形参

```python
def test():
	a = 1
	b = 2
	c = a + b
	return c
test_def = test()
print(test_def)
```

\# 函数内部定义的变量,局部变量,只能在函数内部使用

\# 多个函数中有同名变量,不会产生冲突,作用域不同

\# 在所有函数外部定义的变量,叫做全局变量

\# 再函数内部定义和全局变量同名的变量,就近原则,使用局部变量

\# 能够在函数的内部使用全局变量 可以访问值,不能修改全局变量的值

\# 函数内部修改全局变量,在函数内部使用global关键字声明一下

\# 函数的参数的默认和,也叫缺省参数

\# 函数 如果第二个位置有默认值,从第二个位置开始

例子:
---

```python
# 定义一个加法函数,完成两个数的加减运算,并输出结果

def myAdd(a,b):
	return a + b

# 定义一个函数用来检测输入的年龄是否有效

# 检测函数
def checkAge(age):
	if age >= 1 and age <= 100:
		return True
	else:
		return False

age = int(input("请输入年龄(1-100)"))

# 调用函数检测年龄是否合法
if checkAge(age):
	print("您输入的年龄合法")
else:
	print("您输入的年龄不合法")

# 定义一个函数求圆的面积
def caculateCircleS(r):
	s = 3.14 * r ** 2
	return s

```

```python
# 定义一个函数完成给定范围数累加和

#开始值 和 结束值
def mySum(start,end):
	# 累加和
	num_sum = 0
	if start < end:
		while start <= end:
			num_sum = num_sum + start
			start += 1
	else:
		print("您输入的start大于end!")

	return num_sum

ret = mySum(1, 100)
print(ret)
```

返回值:
----

```python
# 如果一个函数 需要将一个结果 传递给 调用的地方
# 那么就需要return 返回值 返回到 调用函数的那个位置
def test1():
	return 100

result = test1()
print(test1())

```

```python
# 完成两个数的加法 并算出平均值
def sum(num1, num2):
	result = num1 + num2
	return result

def average(average):
	print(average / 2)


a = int(input("请输入一个数字:"))
b = int(input("请输入另一个数字:"))

result = sum(a, b)
average(result)

```

函数类型:
-----

```python
# 函数类型
# 无参数/无返回值
'''
def xxx():
	pass
'''
# 无参数/有返回值
'''
def xxx():
	...
	return 100
'''
# 有参数,无返回值
'''
def xxx(a, b):
	....
'''
# 有参数,有返回值
'''
def(a, b):
	....
	return 100
'''

```

### global关键字 修改全局变量:

```python
# 全局变量的特点:可以在所有的函数中直接使用
# 全局变量的定义:在函数外边定义的变量称之为全局变量

sum_result = 0 # 定义了一个全局变量 它默认值是0

def sum_2_nums():
	# 通过下面一行的global代码,相当于告诉python解释器 在这个函数中的sum_result变量
	# 一定是那个全局的变量,而不是自己在这个函数中定义一个和全局变量相同的变量
	global sum_result
	sum_result = a + b

def avaerage_2_nums():
	print(sum_result/2)

a = int(input("请输入一个数字:"))
b = int(input("请输入另一个数字:"))

# 计算2个值得和
sum_2_nums()
avaerage_2_nums()
```



函数嵌套:
-----

```python
# 定义一个函数完成给定范围数累加和
def checkParam(start,end):
	if start > end:
		return False
	else:
		return True

#开始值 和 结束值
def mySum(start,end):
	# 累加和
	num_sum = 0

	if checkParam(start, end):
		if start < end:
				while start <= end:
					num_sum = num_sum + start
					start += 1
		else:
			pass

	return num_sum

ret = mySum(100, 10)
print(ret)
```

### 函数嵌套调用:

```python
# 打印三条线
def print_1_line():
    print("--------------")

def print_2_line():
    i = 0
    while i < 3:
        print_1_line()
        i += 1
print_2_line()
```

```python
# 返回值调用嵌套函数
def sum_3_nums(a, b, c):
    return a+b+c

def average_3_nums(num1, num2, num3):
    return sum_3_nums(num1, num2, num3)/3

print(average_3_nums(2, 3, 5))v
```

### 全局变量/局部变量:作用域

```python
# 定义了一个全局变量
# 作用域:从开始定义的位置开始算起 一直到整个程序的末尾
g_a = 100  # 为了避免和局部变量名字相同,一般会在全局变量名前面添加 g_


def test1():
    # 局部变量的作用域:从定义这个局部变量开始算起 到 整个函数结束
    # print(a)
    a = 200  # 如果在函数中看到了一个变量名=xx 并且变量名和全局变量名相同,那么在没有用global进行声明的时候,那么相当于定义了一个局部变量,仅仅是名字和全局变量相同罢了
    print(a)  # 如果函数里没有定义局部变量a那么就会到全局变量中找a

test1()
print(g_a)
```

### return高级用法:

```python
# return 多返回值封装成"列表"字典"元组"
# 返回值 返回多个值
def get_my_inof():
	"""获取多个信息"""
	hight = 178
	weight = 100
	age = 18
	return[hight, weight, age]
	# return {"hight":hight,"weight":weight,"age":age}
	# return hight, weight, age """return返回多个值,默认是元组(hight, weight, age)"""

print(get_my_inof())
```

### 缺省参数(默认值):

```python
def sum_nums(a, b= 20): # 形参 可以直接默认使用"默认值"
	result = a + b
	return result
print(sum_nums(100, b=200)) # 实参传给形参,如果形参有默认值,实参替换形参进行计算
# 缺省参数必须往后靠 def sum_nums(a, b= 20) b是缺省参数,必须在a的后面(a, b= 20)

```

### 不定长参数\*args/\*\*kwargs:

```python
# 不定长参数*args
def sum_nums(a, b, c=0, *args):
	result = a + b + c
	print("--"*10)
	print(a)
	print(b)
	print(c)
	print(args)
	# 遍历args 元组的和
	for num in args:
		result += num
	print(result)

sum_nums(11, 22)
sum_nums(11, 22, 33,)
sum_nums(11, 22, 33, 44)
sum_nums(11, 22, 33, 44, 55)
sum_nums(11, 22, 33, 44, 55, 66)

```

```python
# *args 必须在第二个元素开始,不能作为第一个
# **kwargs 字典的形式 保存额外的命名参数
# (args/kwargs可以随意命名.*/**是不定长参数符号)
def sum_nums(a, *args, b=11, c=22, **kwargs):
	print(a)
	print(b)
	print(c)
	print(args)
	print(kwargs)
# 第一个形参必须与第一个实参相互对应
sum_nums(100, 200, 300, 400, b=500, c=1000, d=3.14)
```

### 解包/拆包:

```python
# 解包/拆包
# 把给sum_nums函数的实参 原封传递给test函数
def test(*args, **kwargs):
	print("-----in the test func-----")
	print(args)
	print(kwargs)

def sum_nums(a, b=11, c=22, *args, **kwargs):
	print(a)
	print(b)
	print(c)
	print(args)
	print(kwargs)
	# test(args, kwargs)  # 相当于test((400, 500, 600),{"d":3.14})
	test(*args, **kwargs)  # test(400, 500, 600, d=3.14)

sum_nums(100, 200, 300, 400, 500, 600, d=3.14)
```

### 拆包:两个变量值交换

```python
a = 4
b = 5
a, b = b, a
print(a)
print(b)
```

```python
a, b = 11, 22  # a=11 b=22
print(a, b)
a, b = (11, 22)  # a=11 b=22
print(a, b)
a, b = [11, 22]  # a=11 b=22
print(a, b)

# 拆包
def get_my_info():
    high = 178
    weight = 100
    age = 18
    return high, weight, age

print(get_my_info())
high, wight, age = get_my_info()
print(high)
print(wight)
print(age)
```

### 引用:

```python
# 在python中 a=10 b=a a和b的值是相同的 在内存中 并不真的存了两个值相同的数
# 而是只存了一次数,a/b两个变量同时引用指向了这个内存地址而已
# add_self函数中,a和b的变量值都是数组[11,22],所以b执行了b+=b时,a和b打印出来都是[11,22,11,22]
def add_self(b):
    b += b
    print(b)

a = [11, 22]
add_self(a)
print(a)
# 下面的函数结果a输出[11, 22]b输出[11, 22, 11, 22].
# b += b表示b进行自加并没有声明新的变量
# b = b+b 表示 声明b变量,把b+b的和赋给了b

# 原则:变量名 = xxx
#      为新的声明变量
def add_self(b):
    # b += b
    b = b+b
    print(b)

a = [11, 22]
add_self(a)
print(a)
```

### 引用(数字/字符串/元组)不可变类型:

```python
# python中(数字/字符串/元组)为不可变类型
# 不可变类型:无法再原有内存地址中改变值
# 下面代码:
#         a = 100 id是4297640064
#         a += a  id是4297643264
#         a += a操作后 等同于:a = 200
In [1]: a = 100

In [2]: id(a)
Out[2]: 4297640064

In [3]: a += a

In [4]: id(a)
Out[4]: 4297643264

# 而其他类型如"数组"类型是可变类型
# 可以再原有内存地址中改变值
# a = [11, 22] 内存地址是4397524168
# a += a       内存地址是4397524168
# 两个内存地址 是相等的 都同样指向了a
In [6]: a = [11, 22]

In [7]: id(a)
Out[7]: 4397524168

In [8]: a += a

In [9]: id(a)
Out[9]: 4397524168


# 如果b = a 那么a做出改变,b也同样做出相同改变
In [10]: b = a

In [11]: id(b)
Out[11]: 4397524168

In [12]: a += a

In [13]: id(a)
Out[13]: 4397524168

In [14]: id(b)
Out[14]: 4397524168

In [15]: print(b)
[11, 22, 11, 22, 11, 22, 11, 22]
```

### 匿名函数:

 Lambda 形参:执行的功能

```python
func = lambda a, b: a+b
result = func(11, 22)
print(result)
```

```python
# 匿名函数应用(一)
# 回调
def call_func(a, b, func):
    return func(a, b)

result = call_func(11, 22, lambda x, y: y+x)
print(result)
```

```python
# 匿名函数应用(二)
# python为什么是解释型语言:
# python可以在运行过程中,输入代码改变执行结果(eval)
def call_func(a, b, func):
    return func(a, b)

func_code = input("请输入一个匿名函数:")
print(func_code)
print(type(func_code))

# python3 把字符串转换成 代码: eval
# "lambda x, y: x+y" ---> lambda x, y: x+y
func_code = eval(func_code)

result = call_func(11, 22, func_code)
print(result)
```

```python
# 匿名函数应用(三)
# 多字典key排序
info_list = [{"name":"xxxsdf","age":18}, {"name":"aaaa","age":28}]

info_list.sort(key=lambda x:x['age'])

print(info_list)
```

综合训练:
-----

```python
import random

#　创建用户信息
username = "司马狗剩"
usermoney = 1000


# 显示用户信息
def showPerson():
    print("玩家姓名:%s 玩家金额:%d" % (username, usermoney))


# 押注函数，返回两个结果，一个押注的金额　一个押注的倍率
def setGameMoney():
    #保存输入金额
    money = 0
    myrate = 0
    while True:
        print("请输入押注的金额(1-%d):" % usermoney, end="")
        money = int(input())
        if money >= 1 and money <= usermoney:
            break
        else:
            print("余额不足，请重新输入!")


    # 计算合理倍率
    rate = usermoney // money

    while True:
        print("请输入押注的倍率(1-%d):" % rate, end="")
        myrate = int(input())
        if myrate >= 1 and myrate <= rate:
            break
        else:
            print("您的倍率错误，请重新输入!")

    return money, myrate


# 赌博函数 money押注的金额　rate倍率
def game(money, rate):
    # 先随机产生一个竞猜数字(1-5)
    num = random.randint(1, 5)
    # 让用户输入一个竞猜数字(1-5)
    val = int(input("请输入竞猜数字:"))
    # 显示竞猜数字
    print("系统产生数字为:%d, 您的竞猜数字为:%d!" %(num, val))
    # 判断胜负
    global usermoney
    if num == val:
        # 胜利
        usermoney = usermoney + money * rate
    else:
        # 失败
        usermoney = usermoney - money * rate


# 测试游戏
def test():
    while usermoney > 0:
        # 显示账户信息
        showPerson()
        # 押注
        money, rate = setGameMoney()
        # 开始赌博
        game(money, rate)
        print("-----本局结束------")


# 执行测试函数
test()
```

文件操作:
======

```python
# 读取文件
# 打开文件
'''
"r"     只读(默认)
"w"     只写
"a"     追加
'''
# + 加好表示"可读可写"
'''
"r+"    文件存在.可读可写
"w+"    无所谓存不存在,都新建文件.可读可写
"a+"    文件存在,可读可写
'''
# 除了文本文件以外,其他文件都为"二进制文件"(图片/音频)
'''
"rb"     只读(默认)
"wb"     只写
"ab"     追加
'''.
'''
"rb+"    文件存在.可读可写
"wb+"    无所谓存不存在,都新建文件.可读可写
"ab+"    文件存在,可读可写
'''

f = open("./test.py", "r")

# 读取数据
# f.read('''读取多少字节数据就写多少字节数''')
# f.readline('''一次第一行,需要读取几行,写行数''')
# f.readlines('''已列表的形式,全部列出来''')

#f.read/f.readline/f.readlines (文件没有关闭之前,反复用.read方法,文件内数据只能读取一次,并向下逐次读取)
'''
n [1]: f = open("plane.py")

In [2]: f.read(1)
Out[2]: 'n'

In [3]: f.read(1)
Out[3]: 'a'

In [7]: f.readline()
Out[7]: '["a", \'b\', \'c\', \'d\']\n'

In [8]: f.readlines()
Out[8]: ["names = dict(num1='one', nume2='two',\n",
'''

contend = f.read()
print(contend)

# 关闭文件
f.close()

```

```python
# 打开文件/写入文件
# open函数相对/绝对路径都支持
f = open("/Users/henrydu/Documents/plane/test.py", "w")
# a = open("./test.py", "w")

# 写入数据
f.write("print('helloworld!')")

# 关闭文件
f.close()
```

### 示例:复制文件

```python
# 复制文件(简易版)
# 1) 获取要复制的文件名
old_file_name = input("请输入你要打开的文件名")

# 2) 打开这个文件
old_file = open(old_file_name, "r")

# 3) 读取里面的数据
content = old_file.read()

# 4) 创建一个新文件(切片)
# 文件名.find("字符") 作用:查找字符参数在文件名里面的角标
# 文件名.rfind("字符") 作用:查找字符参数在文件名里面的角标(从右往左找)
position = old_file_name.rfind(".")
new_file_name = old_file_name[:position] + "[复件]" + old_file_name[position:]
new_file = open(new_file_name, "w")

# 5) 将读取到的数据写入到这个新文件中
new_file.write(content)

# 6) 关闭所有的文件
old_file.close()
new_file.close()

```

```python
# 复制文件(升级版)(每次读取1kb,写入1kb)
# 复制文件
# 1) 获取要复制的文件名
old_file_name = input("请输入你要打开的文件名")

# 2) 打开这个文件
old_file = open(old_file_name, "r")

# 4) 创建一个新文件
# 文件名.find("字符") 作用:查找字符参数在文件名里面的角标
# 文件名.rfind("字符") 作用:查找字符参数在文件名里面的角标(从右往左找)
position = old_file_name.rfind(".")
new_file_name = old_file_name[:position] + "[复件]" + old_file_name[position:]
new_file = open(new_file_name, "w")

while True:
    # 3) 读取里面的数据
    # content = old_file.readline()
    content = old_file.read(1024)
    if len(content) > 0:
        # 5) 将读取到的数据写入到这个新文件中
        new_file.write(content)
    else:
        break

# 6) 关闭所有的文件
old_file.close()
new_file.close()

```

### 文件定位读写:

```python
# .tell()方法:
# 表示对文件数据已经操作的数据角标
In [15]: f = open("plane.py")

In [16]: f.tell()
Out[16]: 0

In [17]: f.read(1)
Out[17]: 'n'

In [18]: f.tell()
Out[18]: 1
```

```python
# 文件定位读写(python2使用)
# .seek(0, 0/1/2)
# 0/1/2 表示:
# 0:表示文件数据的起始位置
# 1:表示文件数据操作的位置
# 2:表示文件数据的末尾位置
```

### 文件/文件夹操作:

```python
# 调用 os 模块
import os
# 文件重命名
os.rename("test.py", "xxx.py")

# 删除文件
os.remove("xxx.py")

# 创建文件夹
os.mkdir("test")

# 删除文件夹
os.rmdir("test")

# 获取特定目录里所有的文件名(已列表的形式保存)
test = os.listdir("./")

# 获取当前目录里所有的文件名
test_b = os.getcwd()

# 修改当前操作路径
test = os.getcwd()
print(test)

os.mkdir("xxx")
os.chdir("./xxx")
test = os.getcwd()
print(test)

```

### 文件操作示例:批量重命名

```python
import os

# 获取文件夹名
folder_name = input("请输入文件夹")

# 获取文件夹所有文件
fil_names = os.listdir(folder_name)

os.chdir("./xxx")

# 修改(添加)
for fil_name in fil_names:
    os.rename(fil_name, "[战机]" + fil_name)
'''修改(减少)
for fil_name in fil_names:
    postition = fil_name[4:]
    print(postition)
    os.rename(fil_name, postition)
'''

```
