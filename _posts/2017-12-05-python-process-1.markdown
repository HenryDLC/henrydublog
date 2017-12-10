---
layout: "post"
title: "python-多任务(1)"
date: "2017-12-05 21:41"
---
> 进程/多进程/进程池/阻塞/非阻塞

多任务:让多个任务同时进行

程序 : 代码(不表示代码已经运行)

进程 : 代码运行中,除了代码还包括运行时需要的环境/资源等等

进程:
===

* 运行着的代码，称为进程
* 进程:除了包含代码以外，还包含需要运行需要的系统资源等

注:进程是操作系统分配资源的最小单位。

父进程&子进程:
--------

### fork()函数:

通过fork函数,可以在程序中新创建进程

* os.fork()会创建一个子进程,并将程序信息复制到子进程中去.
* os.fork()父进程和子进程都会得到一个返回值.
  * 子进程返回0
  * 父进程返回子进程的ID号(大于0)
* os.fork()可以创建多个子进程

因为fork()创建子进程,会把父进程的所有资源和状态复制给子进程.父子进程的计数器是一样的,所以都会在cpu中被同时执行…..从而实现多进程的功能

注:因为父子进程是同时进行的,所以程序执行时的输出是没有先后顺序的

> 注:父进程创建子进程,是将父进程所有的信息和资源等复制到子进程开辟的空间中.父子进程资源本身并不共享

例:

```python
import time
import os

ret = os.fork()  # 创建新的进程

if ret == 0:
    for i in range(3):
        print("放音乐")
        time.sleep(0.1)

else:
    for i in range(3):
        print("跳舞")
        time.sleep(0.1)

```

### getpid()&getppid():

操作系统管理进程通过进程编号来维护,进程编号: PID

os.pid():

* 当前程序的进程PID

os.ppid():

* 当前进程的父进程PID

```python
import time
import os

ret = os.fork()  # 创建新的进程

if ret == 0:
    # 子进程
    # 子进程拿到的返回值是0
    print("子进程pid:%s, ppid:%s" % (os.getpid(),os.getppid()))
    for i in range(3):
        print("放音乐")
        time.sleep(0.1)

else:
    # 父进程
    # 父进程中拿到的返回值是创建的子进程的pid
    print("fork方法创建的子进程的进程PID:ret=%s" % ret)
    print("父进程pid:%s, ppid:%s" % (os.getpid(),os.getppid()))
    for i in range(3):
        print("跳舞")
        time.sleep(0.1)
```

回收-子进程资源(阻塞):
-------------

### os.wait():等待子进程结束并结束

os.wait() 两个返回值

* pid 表示回收的子进程PID
* result 表示回收的子进程 结束状态(没有出现异常 返回0)
* pid, result = os.wait() \# 回收子进程资源 阻塞

```python
import time
import os

ret = os.fork()  # 创建新的进程

if ret == 0:
    # 子进程执行
    # 子进程拿到的返回值是0
    for i in range(10):
        print("放音乐")
        time.sleep(0.1)

else:
    # 父进程
    # 父进程中拿到的返回值是创建子进程的pid,大于0
    print("父进程: pid:%d, ppid:%d" % (os.getpid(), os.getppid()))
    print("父进程: 父进程结束...子进程 不受影响")

    # os.wait() 两个返回值
    # pid 表示回收的子进程PID
    # result 表示回收的子进程 结束状态(没有出现异常 返回0)
    pid, result = os.wait() # 回收子进程资源 阻塞
    print("回收的子进程PID:%d,子进程退出的状态:%d" % (pid,result))

    print("子进程结束...")
```

孤儿进程/僵尸进程:
----------

### 孤儿进程:

* 定义:子进程还未结束,父进程结束退出,留下的子进程
* 父进程结束,孤儿进程会被别的进程托管,通常是init进程(pid:0)
* 孤儿进程会被托管的父进程回收

查看孤儿进程PID:

```python
import time, os

ret = os.fork()

if ret == 0:
    print("子进程(孤儿进程)PID:%d, PPID:%d" % (os.getpid(), os.getppid()))
    while True:
        print("子进程在Run....")
        time.sleep(1)

else:
    print("父进程PID:%d,PPID:%d" % (os.getpid(), os.getppid()))
    print("父进程Run....")
    print("父进程.....end..")
```

### 僵尸进程:

定义:父进程持续运行,没有回收已经结束的子进程.子进程所占用的资源没有被回收

```python
import time, os

ret = os.fork()

if ret == 0:
    print("子进程PID:%d, PPID:%d" % (os.getpid(), os.getppid()))
    print("子进程...end...")

else:
    print("父进程PID:%d,PPID:%d" % (os.getpid(), os.getppid()))
    while True:
        print("父进程Run....")
        time.sleep(1)
```

Process类:
---------

> os.fork()方法 内建于操作系统中(OS X & Linux) 其他操作系统并没有该方法

### 创建子进程:

```python
from multiprocessing import Process
import os


def sub_fun():
    """子进程的执行代码"""
    print("子进程PID:%d,PPID:%d" % (os.getpid(), os.getppid()))
    print("子进程...end...")


def main():
    print("父进程PID:%d PPID:%d" % (os.getpid(), os.getppid()))
    # 创建子进程对象
    p = Process(target=sub_fun)
    p.start()  # 创建子进程
    print("父进程:.....Run....")
    p.join()  # 回收子进程
    print("父进程:....END....")


if __name__ == '__main__':
    main()
```

### 通过继承创建子类(封装创建子进程):

```python
from multiprocessing import Process
import os


class SubProcess(Process):
    """创建Process子类"""
    def __init__(self, a, b):
        super(SubProcess,self).__init__()   # 执行父类Process默认的__init__方法
        self.a = a
        self.b = b

    def run(self):
        """子进程的执行代码"""
        print("子进程PID:%d,PPID:%d" % (os.getpid(), os.getppid()))
        print("子进程功能 计算器: %d + %d = %d" % (self.a, self.b, self.a+self.b))
        print("子进程...end...")


def main():
    print("父进程PID:%d PPID:%d" % (os.getpid(), os.getppid()))
    # 创建子进程对象
    p = SubProcess(1, 2)
    p.start()  # 创建子进程
    print("父进程:.....Run....")
    p.join()  # 回收子进程
    print("父进程:....END....")

if __name__ == '__main__':
    main()
```

多进程:
====

> 阻塞:一心一意做一件事

> 非阻塞:一心多用,同时做很多事

进程池:
----

> pool = Pool(4)

> 创建进程池对象,参数表名这个进程池中包含4个进程

> 每创建一个子进程,对去寻找一次任务名(任务名:函数变量,不是任务本身)

> **注:先创建任务,再定义子进程**

### 非阻塞:

> pool.apply\_async()

```python
# 非阻塞 使用进程池
from multiprocessing import Pool
import time
import os


def test(num):
    """定义的任务代码"""
    print("子进程PID:%d Run...Code...执行任务:%d" % (os.getpid(), num))
    time.sleep(0.1)
    print("子进程PID:%d END...Code...执行任务:%d" % (os.getpid(), num))



# 创建进程池对象,参数表名这个进程池中包含4个进程(每创建一个子进程,对去寻找一次任务名(任务名:函数变量,不是任务本身))
pool = Pool(4)

for i in range(10):
    pool.apply_async(test, (i,)) # 向进程池添加任务 非阻塞

print("所有任务都已添加到进程池中.....")

pool.close() # 关闭进程池对象, 表示不能再向进程池添加任务

pool.join() # 回收进程池
```
```
> 所有任务都已添加到进程池中.....
> 子进程PID:4880 Run...Code...执行任务:0
> 子进程PID:4881 Run...Code...执行任务:1
> 子进程PID:4882 Run...Code...执行任务:2
> 子进程PID:4883 Run...Code...执行任务:3
> 子进程PID:4881 END...Code...执行任务:1
> 子进程PID:4880 END...Code...执行任务:0
> 子进程PID:4880 Run...Code...执行任务:4
> 子进程PID:4881 Run...Code...执行任务:5
> 子进程PID:4882 END...Code...执行任务:2
> 子进程PID:4883 END...Code...执行任务:3
> 子进程PID:4883 Run...Code...执行任务:6
> 子进程PID:4882 Run...Code...执行任务:7
> 子进程PID:4880 END...Code...执行任务:4
> 子进程PID:4881 END...Code...执行任务:5
> 子进程PID:4880 Run...Code...执行任务:8
> 子进程PID:4881 Run...Code...执行任务:9
> 子进程PID:4882 END...Code...执行任务:7
> 子进程PID:4883 END...Code...执行任务:6
> 子进程PID:4880 END...Code...执行任务:8
> 子进程PID:4881 END...Code...执行任务:9
```

### 阻塞:

> pool.apply()

```python
# 非阻塞 使用进程池
from multiprocessing import Pool
import time
import os


def test(num):
    """定义的任务代码"""
    print("子进程PID:%d Run...Code...执行任务:%d" % (os.getpid(), num))
    time.sleep(0.1)
    print("子进程PID:%d END...Code...执行任务:%d" % (os.getpid(), num))



# 创建进程池对象,参数表名这个进程池中包含4个进程(每创建一个子进程,对去寻找一次任务名(任务名:函数变量,不是任务本身))
pool = Pool(4)

for i in range(10):
    pool.apply(test, (i,)) # 创建进程池对象,参数表名这个进程池中包含4个进程(每创建一个子进程,对去寻找一次任务名)

print("所有任务都已添加到进程池中.....")

pool.close() # 关闭进程池对象, 表示不能再向进程池添加任务

pool.join() # 回收进程池
```

```
> 子进程PID:4873 Run...Code...执行任务:0
> 子进程PID:4873 END...Code...执行任务:0
> 子进程PID:4874 Run...Code...执行任务:1
> 子进程PID:4874 END...Code...执行任务:1
> 子进程PID:4875 Run...Code...执行任务:2
> 子进程PID:4875 END...Code...执行任务:2
> 子进程PID:4876 Run...Code...执行任务:3
> 子进程PID:4876 END...Code...执行任务:3
> 子进程PID:4873 Run...Code...执行任务:4
> 子进程PID:4873 END...Code...执行任务:4
> 子进程PID:4874 Run...Code...执行任务:5
> 子进程PID:4874 END...Code...执行任务:5
> 子进程PID:4875 Run...Code...执行任务:6
> 子进程PID:4875 END...Code...执行任务:6
> 子进程PID:4876 Run...Code...执行任务:7
> 子进程PID:4876 END...Code...执行任务:7
> 子进程PID:4873 Run...Code...执行任务:8
> 子进程PID:4873 END...Code...执行任务:8
> 子进程PID:4874 Run...Code...执行任务:9
> 子进程PID:4874 END...Code...执行任务:9
> 所有任务都已添加到进程池中.....
```
