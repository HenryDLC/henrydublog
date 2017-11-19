---
layout: "post"
title: "python基础(二)"
date: "2017-11-19 16:22"
---
循环(while)
---------

```python
#计算所有1-100之间的和
index = 1
my_sum = 0
while index <= 100:
	my_sum = my_sum + index
	index += 1
print("ret = %d" % my_sum)
```

```python
# 计算所有1-100之间所有的奇数
index = 1
my_sum = 0
while index <= 100:
	if index % 2 != 0:
		my_sum = my_sum + index
	index += 1
print("ret = %d" % my_sum)

# 计算所有1-100之间所有的偶数
# 计算1-100的和
index = 1
mysum = 0
while index <= 100:
	if index % 2 == 0:
		mysum = mysum + index
	index += 1
print(mysum)
```

```python
# 计算指定数字范围内的和
index = int(input("请输入一个数"))
index2 = int(input("请输入第二个数"))
my_sum = 0
if index2 >= index:
	while index <= index2:
			my_sum = my_sum + index
			index += 1
print(my_sum)
```

```python
# 创建用户，用名字和账户金额，每次参与压住，猜中金钱翻倍，未猜中，则扣除金钱
```

```python
# 打印直角三角形
i = 1
while i <= 5:
	j = 0
	while j < i:
		print("*",end='')
		j += 1
	print()
	i += 1
```

```python
# 创建用户，用名字和账户金额，每次参与压住，猜中金钱翻倍，未猜中，则扣除金钱

# 初始化信息
import random
name = input("请输入您的姓名:")
money = int(input("请输入您的账户余额!:"))
if money < 1000:
	print("您的金额不足!")
else:
	# 开始赌博
	while money > 0:
		# 下注
		yazhu = int(input("请输入您押注的金额"))
		if yazhu > money:
			print("您的余额不足")
		else:
			# 开始赌博
			num = random.randint(0,5)
			val = int(input("请输入精彩数"))
			if val == num:
				money = money + yazhu
				print("本次押注成功,您的余额为%d" % money)
			else:
				money = money - yazhu
				print("抱歉,押注失败,您的余额为%d" % money)

```

```python
# 打印九九乘法表
i = 1
while i <= 9:
	j = 1
	while j <= i:
		print("%d*%d=%d " % (i,j,j*i),end='')
		j += 1
	print("\n")
	i += 1

```

制表符:
----

```python
\​反斜杠符号
\'​单引号
\"​双引号
\n​换行
\t  横向制表符
\r  回车
```



字符串:
====



字符串名 = “字符串” 或 ‘字符串’

```python
string = "henrydu"
print(type(string))

# 多个字符串相加1)字符串相加2)格式化输出
first_name = "Henry"
last_name = "Du"
name = first_name + last_name
print(name)

print("我的名字是%s" % name)
```

列表:
===

存储多个字符串或多个数据,无需定义多个变量,可以使用列表存储

定义:

 变量名 = [ ]

```python
# 列表无需特意强调数据的类型,可以存储多个不同类型的数据
#一般情况下存储相同数据类型的数据
names = ["张三","李四","王五","赵六"]

```

增删改查:
-----

增:  

      xxx.append

      xxx.insert(0,x)

      xxx.insert(xxx)
      '''

删:   

      del

      xxx.pop

改:   

      xxx[1] = “xxxxxx”

查:

      in

      not in

```python
# 列表无需特意强调数据的类型,可以存储多个不同类型的数据,一般情况下存储相同数据类型的数据
names1 = ["张三","李四","王五","赵六"]
names2 = ["张三","李四","王五","赵六"]
names3 = names1
# 列表的操作(增	删	改	查)
# 1)增
names1.append("德玛西亚") # 最后一个位置 添加
names1.insert(0, "艾欧尼亚") # 自定位置 添加
names2.extend(names1) # 其他列表数据 直接添加到尾部

print(names1)
print(names2)
print(names3)
# 列表无需特意强调数据的类型,可以存储多个不同类型的数据,一般情况下存储相同数据类型的数据
names1 = ["张三","李四","王五","赵六"]
names2 = ["张三","李四","王五","赵六"]
print(names1)

# 2)删
del names1[1] # del 可以按照列表角标进行删除

names1.pop() # 删除最后一个

xxx.remove(王五) # 删除特定内容

print(names1)

# 3) 改
names1[1] = "德玛西亚"
print(names1)

# 4) 查
# in ----->判断是否存在
# not ---->判断是否不在
find_name = input("请输入你的姓名!")
if find_name in names1:
	print(存在)

url = input("请输入你要访问的网址")
unuserd_url = ["x.com","xx.com","xxx.com","www.youtube.com"]
if url not in unuserd_url:
	print



```

```python
# 如果使用append 或 insert 添加整个列表,它们将被视为一个整体 添加到 列表中 而不是单个数据
names1 = ["张三","李四","王五","赵六"]
names2 = ["艾欧尼亚","德玛西亚"]
names1.append(names2)
print(names1)

```

列表排序:
-----

```python
# 从小到大排序
nums = [5,4,1,3,2]
nums.sort()
print(nums)
# 从大到小排序
nums.sort(reverse=True)
print(nums)

# reverse 逆序
# 与最初定义的字典相反排序
# nums.sort(reverse=True)解释:
'''
nums.sort()作用为从小到大排序,加入命名参数"nums.sort(reverse=True)"之后
逆序了从小到大排列,改为了从大到小排列
'''
nums = [0,1,2,3,4,5]
nums.reverse()
print(nums)
```

示例:
---

### 名片1.0:

```python
# 名片显示1.0
names = []
while True:
	print("=============")
	print("1.增加姓名")
	print("2.删除姓名")
	print("3.修改姓名")
	print("4.查找姓名")
	print("5.显示所有姓名")
	print("6.退出系统")
	print("=============")

	num = input("输入你需要的功能代码:")
	print("")

	if num == "1":
		name = input("请输入增加的姓名:")
		names.append(name)

	elif num == "2":
		del_name = input("请输入删除的姓名:")
		really = input("确认删除么(yes/no)")
		if really == "yes":
			names.remove(del_name)
		else:
			continue

	elif num == "3":
		old_name = input("请输入原名字:")
		position = names.index(old_name)
		new_name = input("请输入新名字")
		names[position] = new_name

	elif num == "4":
		find_name = input("请输入需要查询的名字")
		if find_name in names:
			print("%s 存在!" % find_name)
		else:
			print("%s 不存在!" % find_name)

	elif num == "5":
		print(names)
	elif num == "6":
		flag = input("是否要退出?yes/no")
		if flag == "yes":
			break

```

for循环:遍历列表
----------

for 变量名 in 列表:

 print(列表)

```python
names = ['a','b','c','d']
# 是用while 循环遍历列表
i = 0
while i < len(names):
	print(names[i])
	i += 1

# 用for 循环遍历列表
for name in names:
	print(name,end="")
```

切片:
===

只读取字符串中,某个部分

```python
name = "hello world"
print(name)
# [切片的起始位置:结束位置的(后一个)]
qiepian = name[4:8]
print(qiepian)
# [切片的起始位置:结束位置的(后一个):步长]
qiepian = name[4:0:-1]
print(qiepian)
# 逆序,如果初始和结束位置不写,甚至步长不写
# 默认步长为"1",位置从左到右或从右到左(左右顺序看步长正负)
qiepian = name[::-1]
print(qiepian)

```

字典:
===

变量名 = {key:value,key:value,key,value} \# 键值对

key:数字/小数点/ 字符串/元组 可以当做key使用

 列表/字典/集合 不可以当做key使用

```python
# 存取多个 同样类型的数据 用	"列表"
# 存取多个 不同类型的数据 用	"字典"

# 声明字典:
infor = {"name":"henrydu","age":23,"tel":"18611955214"}

# 取字典数据
print(infor["age"])

# 删除数据
del infor["age"]
print(infor)

# 添加数据
infor["age"] = 23
print(infor)
```

```python
# 声明:
info = {}

# 增
info["key"] = "数据内容"
print(info)

# 删
del info["key"]
print(info)

# 改(同 增加)
info["key"] = "数据内容2"
print(info)

# 查
print(info["key"])
```

字典的操作:
------

```python
# len()方法:
my_info = {"name": "HenryDu", "age": "23"}
# 遍历字典时,只能遍历出key的值:
for temp in my_info:
    print(temp)


# len() 字典中len返回的是"键值对"的个数
print(len('test'))  # len方法测试字符串时,返回值是"字符串"个数
print(len(my_info))  # len方法测试字典时,返回值时"键值对"个数


# xxx.keys和xxx.values用法:
print(my_info.keys())  # xxx.keys 表示取出字典的key值
print(my_info.values())  # xxx.values 表示取出字典的value值


# xxx.items用法
# xxx.items 表示已元组的形式,取出字典的所有内容
print(my_info.items())

# 遍历字典(元组形式)
for temp in my_info.items():
    print(temp)
# 遍历字典(字符串形式)
for temp in my_info.items():
    print(temp[0])
    print(temp[1])
```

示例:
---

### 名片2.0:

列表/字典嵌套:
--------

```python
school_name_list = [
    {"city_name": "北京市", "school_names": ["北京大学", "清华"]},
    {"city_name": "天津市", "school_names": ["南开", "天大"]},
]

for city_info in school_name_list:
    print("城市的名字是:%s, 学校有:%s" % (city_info["city_name"], city_info['school_names']))

# 下面的内容当做了解
my_info = {"name": "dongge",
           "my_wife": "xxx",
           "qinqi": [{"relation": "姑姑家", "xxx": "xxx"}, {"relation": "舅舅家"}]
           }

class_info = [
    {"name": "dongge",
     "my_wife": "xxx",
     "qinqi": [{"relation": "姑姑家", "xxx": "xxx"}, {"relation": "舅舅家"}]
    }
]
```

元组:
===

列表———\>增删改查[ ]

字典———\>增删改查{ }

元组———\>只读( )

```python
money_each_month = (1, 2, 3, 1, 3, 4)

print(type(money_each_month))

nums = [11, 22, 33]
nums[0] = 100

# money_each_month[0] = 100  # 元组 和 列表很像 但是 不支持数据的修改

for temp in money_each_month:
    print(temp)
```

### 注意:一个元组的定义方式:

```python
# 列表,一个元素定义方式
test = ["name"]
print(type(test))

# 元组,一个元素定义方式(元素后面+ "逗号")
test = ("name", )
print(type(test))
```
