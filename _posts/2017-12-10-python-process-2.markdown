---
layout: "post"
title: "python-多任务(2)"
date: "2017-12-10 13:18"
---
同步/异步/线程/互斥锁通讯队列

同步&异步:
======

参考:<https://www.cnblogs.com/George1994/p/6702084.html>

> 同步:多任务, 多个任务之间执行有先后顺序,必须一个先执行完成之后,另一个才能继续执行,zhi'you'yi'ge'zhu'xian

> 异步:多任务, 多个任务之间执行没有先后顺序,可以同时运行,执行的先后顺序不会有什么影响,存在多条运行主线

### 进程池异步添加任务使用回调函数:

回调函数是由主进程完成的

异步添加任务,还以添加回调函数作为异步任务完成时的提示信息

```python
p.apply_async(water, callback=message) # 异步添加任务
```

```python
from multiprocessing import Pool
import time, os

def water():
    for i in range(5):
        print("烧开水")
    time.sleep(0.3)
    return "水烧开了..."

def message(message):
    """接收异步的结束信息"""
    print("%s" % message)

p = Pool(3)

def game():
    for i in range(5):
        print("打游戏")
        time.sleep(0.3)
p.apply_async(water, callback=message) # 异步添加任务

for i in range(3):
    print("打游戏")
    time.sleep(0.5)

p.close()
p.join()
```

进程间通信-Queue:
------------

消息队列:先进先出

### Queue-阻塞

.put方法,默认为阻塞行为 当数据超过设定队列数,则等待,直到队列有空位值才继续执行

.get() 按添加顺序 取出 每取出一个 则队列自动删除一个

默认阻塞行为,如果队列已满或已空 则等待 直到有新的空位或数据出现 才能继续存入或取出

* 判断队列是否为空或满:
  * 空:empty()
  * 满full()

```python
from multiprocessing import Queue


q = Queue(3) # 表示这个队列最多只能保存3条数据

# 空:empty() 满full()
print("判断队列是否为空%s" % q.empty())
print("判断队列是否为满%s" % q.full())


# .put方法,默认为阻塞行为 当数据超过设定队列数,则等待,直到队列有空位值才继续执行
q.put(100)
print("新添加了一条数据:100")
q.put(200)
print("新添加了一条数据:200")
q.put(300)
print("新添加了一条数据:300")

q.put(400,)
print("新添加了一条数据:400")

# .get() 按添加顺序 取出 每取出一个 则队列自动删除一个
# 默认阻塞行为,如果队列已空 则等待 直到有新的数据出现 才能继续取出
ret = q.get()
print(ret)
ret = q.get()
print(ret)
ret = q.get()
print(ret)

```

### Queue-非阻塞:

方法一:

q.put(400, timeout=3):
表示 最长等待的时间(超时) 单位:s 超时则抛出异常

方法二:

不在使用.put()方法或.get()方法添加取出列表数据:

使用:

* q.put\_nowait()
* q.get\_nowait()

表示如果Queue队列 满或空,则不在等待直接抛出异常

使用Queue 完成进程间通信:
----------------

利用args参数,把queue队列传入两个不同的函数

```python
from multiprocessing import Process, Queue
import time


q = Queue(3) # 表示这个队列最多只能保存3条数据

def process_write(queue):
    """向队列里放入数据"""
    for i in range(3):
        q.put_nowait(i)
        print("子进程1, 放入了一个数据 %d" %i)
        time.sleep(0.5)

def process_read(queue):
    """向队列里取出数据"""
    while not q.empty():
        ret = q.get()
        print("子进程2,从队列中去了一个数据%d" % ret)

p1 = Process(target=process_write, args=(q,))
p2 = Process(target=process_read, args=(q,))

p1.start()
p1.join()

p2.start()
p2.join()
```

### 进程间通讯的实质:

1. 主进程创建了queue通讯队列
2. 主进程 子进程1 子进程2 同时应用了这个queue通讯队列
  1. 子进程1 向queue队列 写入了 数据
  2. 子进程2 向queue队列 读取了刚刚 子进程1 往queue队列 写入的数据
3. 从而 实现 两个进程 之间的相互通讯

进程池之间的进程间通讯-Manger(管理类):
------------------------

利用Manger().Queue() 创建进程池之间的进程通讯队列

```python
from multiprocessing import Pool, Manager
import time


q = Manager().Queue()

def process_write():
    """向队列里放入数据"""
    for i in range(3):
        q.put(i)
        print("子进程1, 放入了一个数据 %d" %i)
        time.sleep(0.5)

def process_read():
    """向队列里取出数据"""
    while not q.empty():
        ret = q.get()
        print("子进程2,从队列中去了一个数据%d" % ret)

pool = Pool(3)

pool.apply(process_write)
pool.apply(process_read)

pool.close()
pool.join()
```

线程:
===

 一个程序完成多项任务

> 进程是操作系统分配资源的最小单位
> 线程是操作系统调度执行的最小单位

### 区别:

* 一个程序至少有一个进程,一个进程至少有一个线程
* 线程的划分尺度小于进程(资源比进程少),使得多线程的并发性高
* 进程在执行过程中拥有独立的内存单元,而多个线程共享内存,从而极大地提高了程序的运行效率
* 线程不过能够独立运行

### 优缺点:

* 线程执行开销小,但不利于资源的管理和保护,而进程则相反

多线程:
----

### 导入threading模块创建多线程:

```python
from threading import Thread
import time

def say_hi():
    for i in range(3):
        print("hi how are you ")
    time.sleep(0.5)


t = Thread(target=say_hi)  # 创建线程对象

t.start()  # 开启线程执行

def say_Fine():
    for i in range(3):
        print("i'm fine thank")
    time.sleep(0.3)

t.join()  # 回收线子程资源
```

### 继承Thread类创建多线程:

```python
from threading import Thread
import time


class subThreed(Thread):
    """子线程"""

    def run(self):
        for i in range(3):
            """子线程执行的代码"""
            print("唱歌")
            time.sleep(0.1)


t = subThreed()  # 创建一个线程对象
t.start()  # 开启线程的执行

for i in range(3):
    print("跳舞")
    time.sleep(0.1)

t.join()  # 回收子线程资源

```

### enumerate方法 查看线程信息:

* 列表形式返回进程信息

```python
from threading import Thread, enumerate
import time


class subThreed(Thread):
    """子线程"""

    def run(self):
        for i in range(3):
            """子线程执行的代码"""
            print("唱歌")
            time.sleep(0.1)


t = subThreed()  # 创建一个线程对象
t.start()  # 开启线程的执行
e = enumerate()
print(e)

for i in range(3):
    print("跳舞")
    time.sleep(0.1)

t.join()  # 回收子线程资源
```

返回列表信息:

```python
[<_MainThread(MainThread, started 140736262431552)>, <subThreed(Thread-1, started 123145540018176)>]
```

### 线程状态:

一个线程一共有五个状态,除了新建和死亡两种状态,还有三种状态,这三种状态是循环进入的.

线程切换:

* 遇到堵塞行为(如:time.slepp(5)
* 线程会第一时间切换到其他线程,直到解除堵塞,又回到就绪状态后再进入运行状态…..

三种状态互相切换:

1. 就绪
2. 等待
3. 运行

```python
from threading import Thread
import time

def say_hi():
    for i in range(3):
        print("Hi how are you ")
        time.sleep(5)

t = Thread(target=say_hi)
t.start()

for i in range(10):
    print("主线程")

t.join()
```

### 多线程修改全局变量:

多线程在修改全局变量时,修改的全局变量是同一个变量

```python
from threading import Thread
import time


num = 100
def fun1():
    global num
    print("num的初始值%d" % num)
    num += 10
    print("线程1修改的num值为:%d" % num)

def fun2():
    global num
    print("num的初始值%d" % num)
    num += 10
    print("线程2修改的num值为:%d" % num)

t1 = Thread(target=fun1)
t2 = Thread(target=fun2)

t1.start()
t1.join()

t2.start()
t2.join()
```

### 多线程资源竞争现象:

多线程中,每个线程都会循环’就绪-等待-运行’三种状态

多线程在使用同一资源时,会存在资源竞争问题

一个线程在没完全结束,系统可能会剥夺线程操控权给其他线程.

其他线程对同一资源进行操作后,当被剥夺操控权的线程再次继续执行调用统一资源

这一资源有可能会被重复调用

案例:

t1线程对num值自增操作,当num = 0时,t1还未来得及对num自增.t1线程被剥夺操控权

t2线程对num进行进行num += 1 操作.

t1重新获得操控权后,继续执行num = 0 自增…..此时 num = 0 自增 1,这调代码被执行两次…num 已然等于1….

```python
from threading import Thread

num = 0


def fun1():
    global num
    for i in range(1000000):
        num += 1


def fun2():
    global num
    for i in range(1000000):
        num += 1


t1 = Thread(target=fun1)
t2 = Thread(target=fun2)

t1.start()
t2.start()

t1.join()
t2.join()

print("num被两线程自增后值为:")
print(num)
```

互斥锁(Lock):
==========

### mutex.acquire():


* 判断锁的当前状态,如果锁出于上锁状态,默认出于阻塞等待
* 直到锁变成未上锁状态,才能继续执行
* 如果发现锁出于未上锁状态,则将所设置为上锁

mutex.release()

* 释放锁, 表示将锁设置为打开状态

### 优点:

* 确保了某段关键代码只能由一个线程从头到尾完整的执行

### 缺点:

* 阻止了多线程并发执行,包含锁的某段代码实际上只能以单线程模式执行,效率下降
* 由于可以存在多个锁,不同的线程保持有不同的锁,并试图获取对方持有的锁时,可能会造成死锁

> 注:建议互斥锁锁住的资源越少越好

```python
from threading import Thread, Lock

num = 0

mutex = Lock() # 创建互斥锁对象


def fun1():
    global num
    for i in range(1000000):

        mutex.acquire()
        # 判断锁的当前状态,如果锁出于上锁状态,默认出于阻塞等待
        # 直到锁变成未上锁状态,才能继续执行
        # 如果发现锁出于未上锁状态,则将所设置为上锁
        num += 1
        mutex.release() # 释放锁, 表示将锁设置为打开状态


def fun2():
    global num
    for i in range(1000000):
        mutex.acquire()
        num += 1
        mutex.release()


t1 = Thread(target=fun1)
t2 = Thread(target=fun2)

t1.start()
t2.start()

t1.join()
t2.join()

print("num被两线程自增后值为:")
print(num)
```

### 非阻塞方式使用互斥锁:

result = mutex.acquire(blocking=False):

* 通过blocking参数,如果设置为True表示acpuire函数属于阻塞方式,否则,非阻塞方式

acquire返回值的含义:

* 如果返回True, 表示上锁成功 (锁原本属于未上锁状态, 此次调用成功将锁设置为上锁状态)
* 如果返回False, 表示上锁失败(锁原本属于上锁状态, 此次调用再想上锁不能完成)

```python
from threading import Thread, Lock
import time

num = 0

mutex = Lock() # 创建互斥锁对象


def fun1():
    global num
    for i in range(1000000):

        while True:
            # 通过blocking参数,如果设置为True表示acpuire函数属于阻塞方式,否则,非阻塞方式
            result = mutex.acquire(blocking=False)  # 可以写成:mutex.acquire(False)
            # acquire返回值的含义
            # 如果返回True, 表示上锁成功 (锁原本属于未上锁状态, 此次调用成功将锁设置为上锁状态)
            # 如果返回False, 表示上锁失败(锁原本属于上锁状态, 此次调用再想上锁不能完成)
            if result:
                num += 1
                mutex.release()
                break
            else:
                time.sleep(0.1)


def fun2():
    global num
    for i in range(1000000):
        while True:
            result = mutex.acquire(blocking=False)
            if result:
                num += 1
                mutex.release()
                break
            else:
                time.sleep(0.1)



t1 = Thread(target=fun1)
t2 = Thread(target=fun2)

t1.start()
t2.start()
```

多线程-不共享数据:
----------

### 局部变量是否要加锁:

局部变量中,两个线程资源不共享:

* 局部变量是属于局部空间内的,虽然保存在一个进程之内部
* 但因为是局部空间,所以两个线程之间访问的是不同的局部空间的东西

```python
from threading import Thread
import time


class MyThread(Thread):
    def __init__(self, num):
        super(MyThread, self).__init__()
        self.num = num

    def run(self):
        self.num += 1
        print("线程(%s),num:%d" % (self.name, self.num))
        self.num += 1
        print("线程(%s),num:%d" % (self.name, self.num))


if __name__ == '__main__':
    t1 = MyThread(100)
    t2 = MyThread(200)

    t1.start()
    t2.start()

    t1.join()
    t2.join()
```

死锁:
---

两个线程都被锁住,都无法继续进行

```python
from threading import Thread, Lock
import time

mutex1 = Lock()
mutex2 = Lock()

def fun1():
    mutex1.acquire()
    print("线程1 锁住了mutex1")
    time.sleep(0.1)
    mutex2.acquire()
    print("线程1 锁住了mutex2")
    print("线程1 hello")

    mutex1.release()
    mutex2.release()

def fun2():
    mutex2.acquire()
    print("线程2 锁住了mutex2")
    time.sleep(0.1)
    mutex1.acquire()
    print("线程1 锁住了mutex1")
    print("线程1 hello")

    mutex1.release()
    mutex2.release()

t1 = Thread(target=fun1)
t2 = Thread(target=fun2)

t1.start()
t2.start()

t1.join()
t2.join()
```

### 避免死锁:

* 添加超时时间

* 程序设计时尽量避免(银行家算法)

添加超时时间:

```python
from threading import Thread, Lock
import time

mutex1 = Lock()
mutex2 = Lock()

def fun1():
    while True:
        mutex1.acquire()
        print("线程1 锁住了mutex1")
        time.sleep(0.1)
        # result = mutex2.acquire(False)
        result = mutex2.acquire(timeout=0.5)  # timeout指明acquire最长超时时间
        if result:
            # 表示mutex2成功上锁
            print("线程1 锁住了mutex2")
            print("线程1 hello")
            mutex1.release()
            mutex2.release()
            break
        else:
            # 表示mutex2上锁失败
            mutex1.release()  # 将mutex1释放, 保证别人都能执行
            time.sleep(0.1)

def fun2():
    mutex2.acquire()
    print("线程2 锁住了mutex2")
    time.sleep(0.1)
    mutex1.acquire()
    print("线程1 锁住了mutex1")
    print("线程1 hello")

    mutex1.release()
    mutex2.release()

t1 = Thread(target=fun1)
t2 = Thread(target=fun2)

t1.start()
t2.start()

t1.join()
t2.join()
```

同步应用:
-----

通过锁的方式,达到多个线程同步顺序运行的目的:

```python
from threading import Thread, Lock
import time

mute1 = Lock()
mute2 = Lock()
mute3 = Lock()

def fun():
    while True:
        mute1.acquire() # 阻塞
        print("线程1 执行")
        mute2.release() # 释放锁2 线程2继续执行
        time.sleep(0.1)

def fun2():
    while True:
        mute2.acquire() # 阻塞
        print("线程2 执行")
        mute3.release() # 释放锁3 线程3继续执行
        time.sleep(0.1)

def fun3():
    while True:
        mute3.acquire() # 阻塞
        print("线程3 执行")
        mute1.release() # 释放锁1 线程1继续执行
        time.sleep(0.1)

t1 = Thread(target=fun)
t2 = Thread(target=fun2)
t3 = Thread(target=fun3)

# 初始化 锁 锁2锁3上锁 不能执行
mute2.acquire()
mute3.acquire()

t1.start()
t2.start()
t3.start()

t1.join()
t2.join()
t3.join()
```

生产者消费者模型:
---------

* 为什么要使用生产者和消费者模式

在线程世界里，生产者就是生产数据的线程，消费者就是消费数据的线程。在多线程开发当中，如果生产者处理速度很快，而消费者处理速度很慢，那么生产者就必须等待消费者处理完，才能继续生产数据。同样的道理，如果消费者的处理能力大于生产者，那么消费者就必须等待生产者。为了解决这个问题于是引入了生产者和消费者模式。

* 什么是生产者消费者模式

生产者消费者模式是通过一个容器来解决生产者和消费者的强耦合问题。生产者和消费者彼此之间不直接通讯，而通过阻塞队列来进行通讯，所以生产者生产完数据之后不用等待消费者处理，直接扔给阻塞队列，消费者不找生产者要数据，而是直接从阻塞队列里取，阻塞队列就相当于一个缓冲区，平衡了生产者和消费者的处理能力。

这个阻塞队列就是用来给生产者和消费者解耦的。纵观大多数设计模式，都会找一个第三者出来进行解耦，

### Queue的说明

1. 对于Queue，在多线程通信之间扮演重要的角色
2. 添加数据到队列中，使用put()方法
3. 从队列中取数据，使用get()方法
4. 判断队列中是否还有数据，使用qsize()方法

代码示例:

```python
from threading import Thread
import queue, time

q = queue.Queue()


class Producer(Thread):
    def run(self):
        count = 0
        while True:
            if q.qsize() < 50:
                for i in range(3):
                    count += 1
                    msg = "产品 %d" % count
                    q.put(msg)
                    print("生产者:%s 生产了一个数据:%s" % (self.name, msg))
                else:
                    time.sleep(1)


class Customer(Thread):
    def run(self):
        while True:
            if q.qsize() > 20:
                for i in range(3):
                    msg = q.get()
                    print("消费者%s 消费了一个数据%s" % (self.name, msg))
            else:
                time.sleep(0.00000000001)


for i in range(3):
    p = Producer()
    p.start()

for i in range(5):
    c = Customer()
    c.start()
```
