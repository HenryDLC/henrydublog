---
layout: "post"
title: "SQL-primer"
date: "2017-12-01 18:55"
---
MySQL
=====

### 安装启动MySQL

sudo apt-get install mysql-server

sudo service mysql start

sudo service mysql stop

sudo service mysql restart

### 进入mysql

>mysql -uroot -p
>your_password

配置mysql

vim /etc/mysql/mysql.cnf/mysql.conf.d/mysql.cnf

Bind-address 服务器绑定ip

port 端口 3306

datadir 数据库目录 /var/lib/mysql

general\_log\_file 普通日志 /var/log/mysql/mysql.log

Log-error 错误日志/var/log/mysql/error.log

数据库操作:
------

show databases; \#查看所有数据库

use 数据库名 \#使用数据库

select database(); \#查看当前使用的数据库

create database 数据库名 charset=utf8; \#创建数据库

drop database 数据库名 删除数据库

数据表:
----

show tables; \#查看当前数据库中所有表

desc 表名字 \# 查看表结构

### 修改表:

\# 添加字段

alter talbe 表名 add 列名 类型;

alter table stundets add birthday datetime;

\# 修改字段

alter table 表名 change 原名 新名 类型及约束

alter table students change birthday birth datetime not null;

\# 删除字段

alter table 表名 drop 列名

alter table students drop birth

\# 查看表的创建语句

show create table 表名

show create table students

### 查询表:

\# 查询所有列

select \* from 表名

select \* from students;

\# 查询指定列

select 列1,列2,列3… from 表名

select id,name from studets;

### 增加表:

\# 全列插入

insert into 表名 values(…)

insert into students values(0,'郭靖',1,'蒙古','2017-1-1’);

\# 部分列插入

insert into students(name,age,height) values('黄蓉’,18,180);

### 修改表:

update 表名 set 列1=值, 列2=值… where 条件

update students set isdelete=0 where id=1

删除表:

delete from 表名 where 条件

delete from 表名 where id=1;

### 备份恢复数据库:

\# 备份

mysqldump -uroot -p 数据库名 \> python.sql;

\# 按提示 输入mysql密码

\# 恢复

mysql -uroot -p 数据库名 \< python.sql

\# 按提示 输入mysql密码

MySQL与python交互
--------------

环境搭建:

sudo pip install pymsql

增删改

```python
# 增删改
from pymysql import *

conn = Connect(host='localhost', port=3306, password='mysql', user='root', database='test',charset="utf8")
cs1 = conn.cursor()
count = cs1.execute('insert into students(name) values("你猜")')
count = cs1.execute('update students set name = "随缘" where id=3')
count = cs1.execute('delete from students where id = 2')
print(count)
cs1.close()
conn.commit()
conn.close()
```

查询

```python
from pymysql import *

conn = Connect(port=3306, host='localhost', charset='utf8', user='root', password='mysql', database='test')
cs1 = conn.cursor()
con = cs1.execute('select id,name from students')
resutl = cs1.fetchall()
print(resutl)
cs1.close()
conn.commit()
conn.close()
```

### 综合应用:

```python
from pymysql import *

# 连接mysql
con = Connect(host='localhost', port=3306, user='root', password='mysql', database='test', charset='utf8')
# 获取连接对象
cs1 = con.cursor()
# 增伤改查
ex = cs1.execute('delete from students where id=7')
ex = cs1.execute('delete from students where id=8')
ex = cs1.execute('select id,name from students')
# 获取查询结果
resutl = cs1.fetchall()
print(resutl)
# 关闭连接对象
cs1.close()
# 提交数据库
con.commit()
# 关闭数据库
con.close()

```





MongoDB
=======

三元素:

数据库:

集合:关系型数据库中的表

文档:对应着数据库中的行,由键值对构成,是拓展的json格式

开启mongo:

ubuntu: sudo mongod --dbpath /var/lib/mongodb/ &

Mac: mongod --config /usr/local/etc/mongod.conf &

数据库操作:
------

查看当前数据库: db

查看所有数据库名称: show dbs

切换数据库(数据库不存在时,插入数据,自动创建该数据库) : use 数据库名

删除数据库: db.dropDatabase()

集合命令:
-----

创建集合:

name: 集合名称 options 集合配置

db.createCollection(name.options) —\>

\# 不限制集合大小 db.createCollection("stu”)

\# 限制集合大小 单位字节

db.createCollection(’sub’, {capped: true, size:10})

查看集合:

show collections

删除集合:

db.集合名称.drop()

增删改查操作:
-------

### 插入:

db.集合名称.insert(document)

例子:

db.stu.insert({name:'guojing',gender:1})

s1={\_id:’20171008’,name:’hr’}

s1.gender=0

db.stu.insert(s1)

### 简单查询:

db.集合名称.find()

### 更新:

query:条件查询 类似于where

update:更新操作符 类似于 set

multi:可选参数,表示把所有符合条件的文档全部更新

db.集合名称.update(\<query\>,\<update\>,{multi:\<boolean\>})

\# 全文档更新

db.update({name:’hr’},{name:’mnc'})

\# 指定属性更新:set

db.stu.insert({name:’hr’,gender:0})

db.stu.update({name:’hr’},{set:{name:’hys'}})

\# 修改多条匹配到的数据

db.stu.update({},{$set:{gender:0}},{multi:true})

### 保存:

db.集合名称.save(document)

db.stu.save({\_id:’20101010’,’name’:’HE’,gender:1})

db.stu.save({\_id:’20180101’,’name’:’'hello})

### 删除:

db.集合名称.remove(

 \<query\>,{justOne:\<boolean\>}

)

query:删除的文档的条件

justOne:如果没有true或1,则只删除一条,默认false,表示删除多条

db.stu.remove({gender:0},{justOne:true})

\# 全部删除{}

db.stu.remove({})

### 示例:

db.stu.drop()

db.stu.insert({name:'郭靖',hometown:'蒙古',age:20,gender:true})

db.stu.insert({name:'黄蓉',hometown:'桃花岛',age:18,gender:false})

db.stu.insert({name:'华筝',hometown:'蒙古',age:18,gender:false})

db.stu.insert({name:'黄药师',hometown:'桃花岛',age:40,gender:true})

db.stu.insert({name:'段誉',hometown:'大理',age:16,gender:true})

db.stu.insert({name:'段王爷',hometown:'大理',age:45,gender:true})

db.stu.insert({name:'洪七公',hometown:'华山',age:18,gender:true})

数据查询:
-----

### 基本查询:

find():查询

db.集合名称.find({条件文档})

findOne():查询,只返回第一个

db.集合名称.findOne({条件文档})

方法pretty():将结果格式化

db.集合名称.find({条件文档}).pretty

### 比较运算符:

* 等于,默认是等于判断,没有运算符
* 小于$lt
* 小于或等于$lte
* 大于$gt
* 大于或等于$gte
* 不等于$ne

db.stu.find({name:'郭靖'}) \#查询特定名称的数据

db.stu.find({age:{$gte:18}}) \#查询大于或等于18的数据

### 逻辑运算符:

逻辑与:默认使用逻辑"与"

db.stu.find({age:{$gte:18},gender:true}) \# 多条件查询

逻辑或:使用$or,值为数组,数组中每个元素为json

db.stu.find({$or:[{age:{$gt:18}},{gender:false}]})

and和or一起使用

db.stu.find({$or:[{age:{$gt:20}},{gender:true}],name:’郭靖'})

### 范围运算符:

使用$in,$nin 判断是否在某个范围内

db.stu.find({age:{$in:[18,28]}})

### 自定义函数:

db.stu.find({$where:function(){return this.age \> 30}}) \# 查询年龄大于30的数据

### Limit:用于读取指定数量的文档

参数NUMBER表示要获取文档的条数

如果没有指定参数则显示集合中的所有文档

db.集合名称.find()limit(NUMBER)

示例:db.stu.find()limit(3)

### Skip:用于跳过指定数量的文档

参数NUMBER表示跳过的记录条数,默认0

db.集合名称.find().skip(NUMBER)

示例:db.stu.find().skip(2)

### 一起使用:不分先后顺序

\# 创建数据集

for(i=0;i\<15;i++){

 db.t1.insert({\_id:i})

}

\> db.t1.find().limit(5).skip(2) \# 五条数据,从第二条数据开始

\> db.t1.find().limit(2).skip(5) \# 两条数据,从第五条数据开始

投影:
---

查询特定字段

db.集合名称.find({},{字段名称:1,…})

需要显示的字段,设置为1,不设置不显示.\_id字段默认显示,设置为0则不显示

db.stu.find({},{\_id:0,name:1,gender:1})

排序:
---

sort(),用于对结果进行排序

db.集合名称.find().sort({字段:1,…})

* 参数1:升序排列
* 参数-1:降序排列

\# 根据性别降序,再根据年龄升序

\> db.stu.find().sort({gender:-1,age:1})

### 统计个数:

db.集合名称.find({条件}).count

或者

db.集合名称.count({条件})

\# 统计男生人数

db.stu.count({gender:true})

### 消除重复:

db.集合名称.distinct('驱虫字段’,{条件})

\#查询年龄大于18的数据,来自哪里

db.stu.distinct(‘hometown’,{age:{$gt:18}})

### 备份:

mongodump -h dbhost -d dbname -o dbdirectory

-h 服务器地址

-d 需要备份的数据库名称

-o 备份的数据存放位置,此目录中存放着备份出来的数据

sudo mongodump -h IP地址 -d 数据库名称 -o ~/Desktop/备份路径

### 恢复:

mongorestore -h dbhost -d —dir dbdirectory

-h 服务器地址

-d 需要回复的数据库实例

—dir 备份数据库所在的位置

sudo mongorestore -h IP地址 -d 数据库名称 —dir ~/数据库所在的位置

与Python 交互:
-----------

\# 安装

pip3 install pymongo

\# 导入

from pymongo import \*

```sql
from pymongo import *

if __name__ == '__main__':
    client = MongoClient(host='localhost', port=27017)
    db = client.stu
    # 增
    db.stu.insert_one({'name': 'abc', 'gender': True})

    # 查 一条
    result = db.stu.find_one()
    print(result)
    # 查 多条(字典形式,遍历字典)
    result = db.stu.find({'hometown': '大理'})
    for item in result:
        print(item['name'], item['hometown'])

    # 修改 一条
    db.stu.update_one({'gender': False}, {'$set': {'name': 'hehe'}})
    # 修改 多条
    db.stu.update_many({'gender': True}, {'$set': {'name': 'haha'}})

    # 删除 一条
    db.stu.delete_one({'gender': False})
    # 删除 多条
    db.stu.delete_many({'gender': True})
    print('ok')
```





Redis
=====

启动redis:
--------

/usr/local/bin/redis-server /usr/local/etc/redis.conf &

进入redis:

\> redis\_cli

数据库操作:
------

redis 默认16个数据库,通过0-15 来标识

切换数据库:

\> select 1

\> select 2

\> select 3

数据操作:
-----

redis是key-value的数据结构,每条数据都是一个键值对

键的类型是字符串,键不能重复

值的类型分为:

* 字符串:string
* 哈希:hash
* 列表:list
* 集合:set
* 有序集合:zset

string:
-------

string是redis最基本的数据类型

最大能存储512MB数据

string是二进制安全的,可以存储任何数据,数字/图片等



### 增加/修改:

如果设置的键不存在则添加,存在则修改

set key value

\# 设置键为’py1’ 值为’gj’的数据

set ‘py1’ ‘gj’

设置键值及过期时间,以秒为单位

setex key seconds value

\# setex ‘py1’ 5 ‘guojing’

设置多个键

mset key1 value1 key2 value2 …

\# mset ‘py1’ ‘guojing’ ‘py2’ ‘huangrong’

追加值

append key value

\# append ‘py1’ ‘huangrong’

\> "guojinghuangrong"

### 获取:

根据键获取值,如果不存在返回nil

get key

\# get ‘py1'

根据多个键获取多个值

mget key1 key2…

\# mget ‘py1’ ‘py2’

### 键命令:

查找键 参数支持正则表达式

keys pattern

\# 查看所有键

\> key \*

\# 查看名称中包含a的键

\> keys ‘\*a\*’

\# 判断是否存在,如果存在返回1,不存在返回0

exists key1

\> exists ‘py1’

\> exists ‘py2’

\# 查看键对应的value的类型

type key

\> type ‘py1’

\# 删除键及对应的值

del key1 key2 …

del ‘py1’ ‘py2’

\# 设置过期时间,以秒为单位 如果没有制定过期时间则一直存在,指导使用del溢出

expire key seconds

\> expire ‘py1’ 10

\# 查看有效时间

ttl key

\> ttl ‘py1'



Hash:
-----

用于存储对象,对象结构为属性/值

值得类型为string

### 增加/修改:

设置单个属性

hset key field value

\# 设置键’py1’的属性’name’为’hr'

\> hset ‘py1’ ’name’ ‘hr

设置多个属性:

hmset key field1 value1 field2 value2

\# 设置键’py1’的属性’name’为’dx’/属性’gender’为’1’ 属性’birthday’为’2017-1-1’

hmset ‘py2’ ‘name’ ‘dx’ ‘gender’ ‘1’ ‘birthday’ ‘2017-1-1’



### 获取:

获取指定键所有的属性

hkeys key

\# 获取键’py1'的所有属性

hkeys ‘py1'

获取一个属性的值

hget key field

\# 获取键'py1’属性’name’的值

hget ‘py1’ ‘name'

获取多个属性的值

hmget key field1 field2…

\# 获取键'py1’属性’name’ ‘gender’ ‘birthday’的值

hmget ‘py1’ ‘name’ ‘gender’’birthday'

获取所有属性的值

hvals key

\# 获取键'py1’所有属性的值

hvals 'py1'

### 删除:

删除整个hash键及值,使用del命令

删除属性,属性对应的值被一起使用删除

hdel key field1 field2 …

\# 获取键’py1’的属性’gender’ ‘birthday'

hdel ‘py1’ ‘gender’ ‘birthday’



List
----

列表的元素为string

按照插入顺序排序

### 增加:

lpush key value1 value2…

从键’py1’的列表,左侧加入数据’dx’ ‘xd’

lpush ‘py11’ ‘dx’ ‘xd’

从右侧插入数据

rpush key value1 value2…

\# 从键’py1’的列表右侧加入数据’nd’ ‘bg’

rpush ‘py1’ ’nd’ ‘bg’

在指定元素的前或后插入新元素

linsert key before 或 after 现有元素 新元素

\# 在键为’py1’的列表中元素’nd’前加入’zbt’

linsert ‘py1’ before ‘nd’ ‘zbt'

### 获取:

返回列表里指定范围内的元素

* start/stop 微元素的下标索引
* 索引从左侧开始,第一个元素为0
* 索引可以是负数,表示从尾部开始计数,如-1表示最后一个元素

lrange key start stop

\# 获取键为’py1’ 的列表所有元素

lrange ‘py1’ 0 -1

### 修改:

设置指定索引位置的元素值

* 索引从左侧开始,第一个元素为0
* 索引可以是负数,表示尾部开始计数,如-1表示最后一个元素

lset key index value

\# 修改键为’py1’的列表中下标为1的元素值为’xidu’

lset ‘py1’ 1 ‘xidu'

### 删除

删除指定元素

* 将列表中前count次出现的值为value的元素移除
* count\>0:从头尾移除
* count\<0:从尾往头移除
* count=0:移除所有

lrem key count value

\# 向列表’py1’中加入元素’h0’ ’h1’ ’h2’ ’h0’ ’h1’ ’h3’ ’h0’ ’h1’

rpush 'py1 ’h0’ ’h1’ ’h2’ ’h0’ ’h1’ ’h3’ ’h0’ ’h1’

\# 从’py1'列表右侧开始删除2个元素’h0’

lrem ‘py1’ -2 ‘h0’

\# 查看列表’py1’的所有元素

lrange ‘py1’ 0 -1



Set
---

* 无序集合
* 元素为string类型
* 元素具有唯一性,不重复
* 说明:对于集合没有修改操作

### 增加

添加元素

sadd key member1 member2

\# 向键’py1’的集合中添加元素’yg’ ‘xln’ ‘yzp’

sadd ‘py1’ ‘yg’ ‘xln’ ‘yzp'

### 获取

返回所有的元素

smemers key

\# 获取键’py1’的集合中所有元素

smembers ‘py1’

### 删除

删除指定元素

srem key member

\# 删除键’py1’的集合中元素’yzp’

srem ‘py1’ ‘yzp’



### zset

* sorted set 有序集合
* 元素为string类型
* 元素具有唯一性,不重复
* 每个元素都会关联一个double类型的score,表示权重,通过权重将元素从小到大排序
* 说明:没有修改操作

### 增加

添加

zadd key score1 member1 score2 member2 …

\# 向键’py1’的集合中添加元素’gj’’hr’’xln’,权重分别为1/5/8/3

zadd ‘py1’ 1 ‘gj’ 5 ‘hr’ 8 ‘yg’ 3 ‘xln’

### 获取

返回指定范围内的元素

* start/stop为元素的下标索引
* 索引从左侧开始,第一个元素为0
* 索引可以是负数,表示从尾部开始计数,如-1表示最后一个元素

zrange key start stop

\# 获取键’py1’的集合中所有元素

zrange ‘py1’ 0 -1

返回score值在min和max之间的成员

zrangebyscore ‘py1’ 4 9

返回成员member的score值

zscore key member

\# 获取键’py1’的集合中元素’yg’的权重

zscore ‘py1’ ‘yg’

### 删除

删除指定元素

zrem key member1 member2 …

\# 删除集合’py1' 中元素’yg'

zrem ‘py1’ ‘yg’

删除权重在指定范围的元素

zremrangebyscore key min max

\# 删除集合’py1’中权限在4/9之间的元素

zremrangebyscore ‘py1’ 4 9

总结:
---

### string:

* set
* setex
* mset
* append
* get
* mget

### Key:

* keys
* exists
* type
* delete
* expire
* getrange
* ttl

### Hash

* hset
* hmset
* hkeys
* hget
* hmget
* hvals
* hdel

### List

* lpush
* rpush
* linsert
* lrange
* lset
* lrem

### Set

* sadd
* smembers
* srem

### zset

* zadd
* zrange
* zrangebyscore
* zscore
* zrem
* zremrangebyscore
