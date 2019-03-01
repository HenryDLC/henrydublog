---
layout: "post"
title: "python(基础四)"
date: "2017-11-19 16:23"
---
面向对象:
=====

类:
--

类:笼统的概念

类名:名字

属性:一组数据(特性)

方法:功能

对象:具体 表明 物体



```python
# 联想:定义函数
"""
def 函数名():
    pass
返回值 return
参数:缺省参数/命名参数/不定长参数
解包
变量交换
"""

# 定义一个类
# class 类名(大驼峰):
#     属性
#     方法

class Cat:
    # 属性

    # 方法(类里面的函数)
    def eat(self):
        print("猫在吃鱼")

    def drink(self):
        print("猫在喝水")

    def itroduce(self):
        # print("我的名字是%s!我的年龄是%d" % (tom.name, tom.age))
        print("我的名字是%s!我的年龄是%d" % (self.name, self.age))

# 创建一个对象的方式:
# 变量名 = 类名()
# tom = Cat()是全局变量
tom = Cat()

# 调用对象方法:
tom.eat()
tom.drink()

# 给对象添加属性
# 对象属性无则添加,有则更改
tom.name = "Tom"
tom.age = 30
tom.age = 31

# 获得tom对象的属性
# print(tom.name)
tom.itroduce()

# 创建另一只猫对象添加属性(创建多个对象)
ding_dang = Cat()
ding_dang.name = "DD"
ding_dang.age = 31
ding_dang.eat()
ding_dang.drink()
ding_dang.itroduce()
```

### self(表示自己):

一个变量,用来接收 给它的引用

\_\_init\_\_方法(对象初始化):
----------------------

### 创建对象的过程:

1.根据cat这个模板 创建一个对象

2.自动 调用一个方法 (\_\_init\_\_方法)

3.将初始化的 对象的引用 返回到创建对象的地方

\_\_str\_\_方法(返回字符串,对象描述):
--------------------------

```python

class Cat:
	def __init__(self,new_name,new_age):
		"""
		自动调用__init__方法 会将刚创建好的对象 当做第一个参数传递给self变量中
		剩余的形参用来接收 创建对象Cat()里面的实参

		将已经初始化完成之后的对象应用 返回到 创建对象的地方
		"""
		print("------init--------")
		self.name = new_name
		self.age = new_age

	def __str__(self):
		"""返回一个字符串,这字符串中往往表示这个对象的描述信息"""
		return "Cat创建出来的对象:%s" % self.name

	def eat(self):
		print("猫在吃鱼")

	def drink(self):
		print("猫在喝水")

	def itroduce(self):
		print("我的名字是%s!我的年龄是%d" % (self.name, self.age))

tom = Cat("Tom",31)
tom.eat()
tom.drink()


# tom.name = "Tom"
# tom.age = 30
# tom.age = 31


tom.itroduce()

ding_dang = Cat("DD",31)
# ding_dang.name = "DD"
# ding_dang.age = 31
ding_dang.eat()
ding_dang.drink()
ding_dang.itroduce()

print("----------分割线------------")
print(tom)
print(ding_dang)
```

\_\_init\_\_: 创建一个对象的时候自动调用

\_\_str\_\_:获取一个对象的描述时 自动调用

示例:(烤地瓜)(self属性)
----------------

```python
class Yam:
    """定义一个地瓜类"""

    def __init__(self):
        """完成初始化设置"""

        num = 100  # 局部变量,调用init方法时,调用结束 局部变量无法使用
        # self.cook_string = "生的" cook_string变量有self属性,在其他方法中可以直接使用
        self.cook_string = "生的"
        self.cook_all_time = 0  # 记录烤的总时间

        self.cook_item = []  # 用来记录所添加的作料

    def __str__(self):
        return "地瓜的状态是:%s,添加的作料有:%s" % (self.cook_string, str(self.cook_item))

    def cook(self, cook_time):
        """完成烤地瓜"""

        self.cook_all_time += cook_time
        if 0 <= self.cook_all_time <= 3:
            self.cook_string = "生的"

        elif 3 < self.cook_all_time <= 5:
            self.cook_string = "半生不熟"
        elif 5 < self.cook_all_time <= 8:
            self.cook_string = "熟了"
        else:
            self.cook_string = "糊了"

    def add_item(self,item):
        # 向"地瓜"对象中 添加参数
        self.cook_item.append(item)

# 创建一个对象
di_gua = Yam()

# 烤地瓜
di_gua.cook(1)
di_gua.cook(1)
di_gua.add_item("盐")
di_gua.cook(1)
di_gua.cook(1)
di_gua.add_item("辣椒")

print(di_gua)
```

示例:(放家具)(对象指向)
--------------

```python
class home:
    """定义一个'房子类'"""

    def __init__(self, area, address):
        self.total_area = area
        self.area = area
        self.address = address
        self.items = []  # 用来存放所有的家具

    def __str__(self):
        names = []
        areas = []
        for item in self.items:
            names.append(item.name)
        for item in self.items:
            areas.append(item.area)
        # __init__方法里执行"self.items = []",指向了bed1对象
        # item.name没有self方法,所以指向的是被传过来的bed1对象里的name值

        return "房子的总面积是:%d,房子的可用面积是%d,房子的地址是:%s,房子里的家具有:%s,家具的面积是:%d" % \
               (self.total_area,self.area, self.address, str(names), areas[0])

    def add_item(self, item):
        """将物品添加到房子里"""
        # 如果物品的面积大于可用面积 添加不成功
        if item.area < self.area:
            self.items.append(item)
            self.area = self.area - item.area
        else:
            print("物品太大,换个小点的")


class Bed:
    """定义一个'床'类"""

    def __init__(self, name, area):
        self.name = name
        self.area = area


# 1.创建一个房子
my_home_1 = home(230, "北京市朝阳区酒仙桥路七号院5号楼101")

# 2.创建一个家具
bed1 = Bed("双人床", 4)

# 3.把这个家具存放在房子里
my_home_1.add_item(bed1)
print(my_home_1)

# 4.在购买一个床
bed2 = Bed("炕", 15)
my_home_1.add_item(bed2)
print(my_home_1)

```

隐藏对象的属性:(私有属性)
--------------

```python
class Animal:
    def __init__(self):
        # self.age = 0
        # 定义了一个"私有"属性
        # "__"变量名,是"私有属性"
        # 特点:只能在方法中使用,self.__xxx 不允许同对象.__xxx使用
        self.__age = 0

    def set_age(self, age):
        # if 1 <= age <= 20:
        #     self.age = age
        if 1 <= age <= 20:
            self.__age = age

    def get_age(self):
        return self.__age


dog = Animal()

# 如果 类没有定义def age(self): 方法,自动添加对象方法
# dog.age = 1
dog.set_age(4)
# print(dog.__age)
print(dog.get_age())

# 如果加上if 1 <= age <= 20:,利用set_age方法,无法更改超过限定的值
# dog.set_age = 110
dog.set_age(110)
# print(dog.__age)
print(dog.get_age())


# 自动添加的对象方法,可以绕过set_age方法对age值的限定if 1 <= age <= 20:
# dog.age = 110
# print(dog.__age)
```

私有方法:
-----

```python
class Dog:
    """定义一个狗类"""

    def __init__(self, name):
        self.name = name

    # 定义一个私有方法
    def __sit_down(self):
        """坐下"""
        print("%s坐下" % self.name)

    # 定义了一个公有方法
    def sit_down(self, name):
        """判断发号施令的人,是主任坐下..否则不理他"""
        if name == "主人":
            self.__sit_down()  # 在一个方法中,定义另一个方法,必须加self属性
        else:
            print("不理你")


wang_cai = Dog("旺财")
# wang_cai.__sit_down()  # 不允许直接使用, 对象名,私有方法名()
wang_cai.sit_down("主人")
wang_cai.sit_down("xxx")

```
