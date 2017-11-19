---
layout: "post"
title: "python基础(一)"
date: "2017-11-19 16:16"
---
```python
#conding=utf-8
# -*- coding:utf-8  -*-
```

注释：
===

```python
#单行注释
#单行注释 往往为下面一行代码进行说明

'''
多行注释
多行注释 往往为下面一段代码进行说明
'''
```

注意:

 官方建议:单行注释 \# 空格后再进行注释

 print(“test”) \# 代码后加注释,代码后空两格 \# 空一格再进行注释

 多行注释:单引号 双引号 都可以

变量:
===

python 变量:如果是”双引号”里面的值为:字符串print后面依靠%s取出值

 如果要对字符串进行运算,在非相同变量类型的时候需要进行强制类型转换int(变量名)

```python
username = "henrydu"
password = "00000000"
print("%s %s"%(username,password))
```

type

 :可以获得变量的类型(以字符串的形式输出)

```python
username = "henrydu"
password = "000000"
print(type(username))
print(type(password))
```

运算符:
----

算数运算符

```python
+ # 加法
- # 减法
* # 乘法
/ # 除法
// # 取整数
% # 取余
** # 幂 返回x的y次幂 a**b为10的2次方 输出结果是100
```

复合运算符

```python
+= # c += a 等效于 c = c+a
-= # c -= a 等效于 c = c-a
*= # c *= a 等效于 c = c*a
/= # c /= a 等效于 c = c/a
%= # c %= a 等效于 c = c%a
**= # c ** = a 等效于 c = c**a
//= # c // = a 等效于 c = c//a
```

不同变量之间计算:
---------

```python
# int和int类型计算
a1 = 10
b1 = 20
ret1 = a1 + b1
print(ret1)

# float和float类型计算
a2 = 3.14
b2 = 5.67
ret2 = a2 + b2
print(ret2)

# int和float类型计算
a3 = 100
a4 = 3.14
ret3 = a3 + a4
print(type(ret3))
print(ret3)

# int和Bool类型计算
a4 = 100
b4 = True
ret4 = a4 + b4
my_type = type(ret4)
print(my_type)
print(ret4)

# 字符串类型+计算
first_name = "Henry "
last_name = "Du "
oter_name = "MF"
my_name = first_name + oter_name + last_name
print(my_name)

# 重复拼接字符串
ret5 = first_name * 10
print(ret5)

# 特别注意,字符串不能够直接和数字运算
# ret6 = first_name + 10
# print(ret6)
```

标识符规则:
------

标识符由字母/下划线/数字组成,并且数字不能用于开头

格式化输出:
------

两种格式化输出样式

```python
i = 10
j = 11
print("%d"%i)
print("%d %d"%(i,j))
```

```python
name = "HenryDu "
print("方法1:My name is " + name + "Nice too meet you!")
print("格式化输出:")
print("方法2:My name is %s Nice too meet you!" % name)
stu_number = 20142233127
print("学号:%d" % stu_number)
print("===============++++++======")
# 定义apple价格
apple_price = 9.45
# 买了多少
buy_weight = 7.5
# 总价格
sum = apple_price * buy_weight
print("今天苹果的价格是%.2f,买了%.1f斤,一共花了%.2f元" %
(apple_price,buy_weight,buy_weight))
print("===============+++++++======")
scale = 0.75 * 100
print("比率是:%d%%" % scale)
```

常用的格式符号:

```python
%S # 通过str()字符串转换过来格式化
%d # 整数输出
%o # 八进制整数
%x # 十六进制整数(小写字母)
%f # 浮点实数,%.2f表示输出小数后两位
%% # 输出%号
```

输入input:
--------

```python
stu_name = input("请输入您的姓名")
stu_age = input("请输入您的年龄")
stu_id = input("请输入您的学号")
stu_acount = input("请输入您的身份证号码")
print("===========================")
print("录入成功,请核对您的个人信息")
print("===========================")
print("姓名:%s"%stu_name)
print("年龄:%s"%stu_age)
print("学号:%s"%stu_id)
print("身份证号:%s"%stu_acount)
print("===========================")
```

强制类型转换:
-------

```python
int(x)         将x转换成int类型
Flaot(x)       将x转换成float类型
```

分支语句:
=====

if:
---

if 判断的条件:

 条件成立时,要做的事情

```python
#例子:
age = int(input("请输入你的年龄:"))
if age >= 18:
	print("成年人")
```

关系运算符:
------

```python
==  相等
!=  不相等
>   大于
<   小于
>=  大于等于
<=  小于等于
```

```python
#银行取款

#用户账户余额
acount = 1000

#输入取款金额
cash = int(input("请输入取款金额:"))

if acount < cash:
	print("余额不足!")

if acount >= int(cash):
	acount = acount - int(cash)
	print("取款金额%d元成功,您所剩金额为%d元!"%(cash,acount))


```

逻辑运算符:
------

and 判断语句中,两个或多个条件同时满足 判断为真

or 判断语句中,两个或多个条件满足一个 判断为真

not 判断语句中,两个或多个条件全部满足(不满足) 判断为假(真)

```python
#登录:账户为admin,密码123或者123456,等级不小于3
username = input("请输入用户名")
password = input("请输入密码")
level = int(input("请输入您的等级"))
#逻辑与
if username == "admin" and password == "123456" or "123" and not int(level) < 3:
	print("登陆成功")
```

if-else:
--------

```python
# 输入用户名和密码，如果登录成功，则显示学生信息，登录失败则不显示
# 输入成绩，如果成绩大于60，输出您的成绩及格，否则您本次考试不及格

username = "henrydu"
userpassword = "123"
usersex = "男"
score = int(78)

def student():
	print("++++++++++++++++++++++++")
	print(username)
	print(userpassword)
	print(usersex)
	print("++++++++++++++++++++++++")
	print("您的成绩是:")

if input("登录名:") == "henrydu" and input("密码") == "123":
	student()
	if score >= 60:
		print("合格")
	else:
		print("不合格")
else:
	print("用户或密码输入错误!")
```

```python
# 输入数字，判断数的奇偶,如果是偶数输出，否则输出不是偶数
number = int(input("输入数字:"))
if number % 2 == 0:
	print("偶数")
else:
	print("奇数")
```

```python
#输入数字，判断数字的正负性
number = float(input("请输入数字:"))

if number > 0:
	print("正数")
else:
	print("负数")
```

if -elif-else:
--------------

```python
# 输入成绩，当成绩大于90，输出A，80，则输出B，否则输出，您需要都努力了
score = int(input("请输入成绩"))

if score >90:
	print("A")
elif score >= 80 and score < 90:
	print("B")
elif score >= 70 and score < 80:
	print("C")
elif score >= 60 and score < 70:
	print("D")
else:
	print("E")

```

```python
# 输入数字，输出今天是星期几，否则显示您输入的数字无效
day = int(input("请输入数字:"))
if day == 1:
	print("今天是星期一")
elif day == 2:
	print("今天是星期二")
elif day == 3:
	print("今天是星期三")
elif day == 4:
	print("今天是星期四")
elif day == 5:
	print("今天是星期五")
elif day == 6:
	print("今天是星期六")
elif day == 7:
	print("今天是星期七")
else:
	print("输入错误")
```

```python
# 随机产生一个数字，用户输入，比较大小等于

# 引入随机数包,包里存有写好的功能
import random
# 包名.函数(方法功能)
num = random.randint(1,5)

number = int(input("请输入数字"))
if number > num:
	print("赢了")
elif number < num:
	print("输了")
else:
	print("平局")
```

if嵌套:
-----

```python
# 随机产生一个数字，用户输入，比较大小等于

# 引入随机数包,包里存有写好的功能
import random
# 包名.函数(方法功能)
num = random.randint(1,5)

number = int(input("请输入数字"))

if number >= 1 and number <= 5:
	if number > num:
		print("赢了")
	elif number < num:
		print("输了")
	else:
		print("平局")
else:
	print("输入错误:")
```

```python
# 是否年龄够18岁，如果够进入网吧，再判断钱是否够。
age = int(input("请输入年龄:"))
money = float(input("请输入所带金额:"))

if age >=18:
	if money >= 2:
		print("请进!")
	else:
		print("额度不够")
else:
	print("年龄不合法")
```

```python
# 输入账号密码，判断登录。再输入身份信息，判断权限，显示不同菜单
myId = input("请输入账号:")
password = input("请输入密码")
level = int(input("请输入等级"))

if (myId == "h" and password == "woaini") or (myId == "henry" and password == "123"):
	if level >= 3:
		print("管理员登录成功")
		print("1)添加用户")
		print("2)删除用户")
	else:
		print("1)普通用户登录成功")
		print("2)查看信息")
		print("3)修改密码")
else:
	print("用户或密码错误!")
```

```python
# 正数加减法，先判断数字是否大于0，然后判断符号

num1 = int(input("请输入第一个加数:"))
if num1 >=0:
	num2 = int(input("请输入第二个加数:"))
	if num2 >= 0:
		print("请输入加法或减法运算")
		sum = int(input("1加/2减"))
		if sum == 1:
			print("%d + %d = %d" % (num1,num2,(num1 + num2)))
		elif sum == 2:
			print("%d - %d = %d" % (num1,num2,(num1 - num2)))
		else:
			print("输入错误!")

	else:
		print("输入错误!")
else:
	print("输入错误!")
```

```python
# 请输入您要计算的图形编辑:长方形、三角形、圆，然后要求用户输出相应参数，计算

print("面积计算器:")
print("1 矩形(正方形或长方形)")
print("2 圆形")
print("3 三角形")
print("+++++++++++++++++++++++++++")
tuxing = input("请输入图形:")
if tuxing == "1":
	chang1 = float(input("请输入边长"))
	chang2 = float(input("请输入第二条边长"))
	mianji = chang1 * chang2
	print("长方形/正方形的面积是%f" % mianji)

elif tuxing == "2":
	PI = 3.1415926
	r = float(input("请输入半径"))
	yuamianji = PI * r ** 2
	print("圆的面积是%.2f" % yuamianji)

elif tuxing == "3":
	di = float(input("请输入底边长度"))
	gao = float(input("请输入高长度"))
	sanjiaoxingmianji = di * gao / 2
	print("三角形面积是%f" % sanjiaoxingmianji)

else:
	print("输入错误!")

```

break和continue:
---------------

Break:结束整个循环

continue:结束本次循环

```python
i = 0
while i < 5:
	i += 1
	print("=======")
	if i == 3:
		# break
		continue
	print(i)
```

break 和 continue 只对 当前循环有用,不对外循环起作用

```python
j = 0
while j < 3:
	print("============")
	i = 0
	while i < 5:
		i += 1
		print("*******")

		if i == 3:
			break
			# continue

		print(i)
	j += 1
```
