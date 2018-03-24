---
layout: "post"
title: "Flask-Web(框架三)"
date: "2018-03-25 00:03"
---
设计模型类
-----

```python
from flask_sqlalchemy import SQLAlchemy
from setting import DevelopmentConfig # 连接数据库配置文件
db = SQLAlchemy(app)

class Author(db.Model):
    __tablename__ = 'authors'
    id = db.Column(db.INTEGER, primary_key=True)
    name = db.Column(db.String(20))
    email = db.Column(db.String(40))
    # 一对多关系中,在一的这端 定义关联对象
    books = db.relationship('Book', backref='author')

class Book(db.Model):
    id = db.Column(db.INTEGER, primary_key=True)
    title = db.Column(db.String(20))
    info = db.Column(db.String(200))
    author_id = db.Column(db.INTEGER,db.ForeignKey('authors.id'))

```

迁移
--

命令行执行flask脚本操作安装Flask-script

```sh
>> sudo pip install Flask-script
```

重要:

配置PyCharm 中Edit Configurations:

Script parameters: runserver

安装Flask迁移包:Flask-Migrate

```sh
>> sudo pip install Flask-Migrate
```

```python
# -*- coding: utf-8 -*-
from flask import Flask
from setting import DevelopmentConfig
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
# setting配置文件 开发类
app.config.from_object(DevelopmentConfig)

# 数据库
db = SQLAlchemy(app)

# 设计模型类
class Author(db.Model):
    __tablename__ = 'authors'
    id = db.Column(db.INTEGER, primary_key=True)
    name = db.Column(db.String(20))
    email = db.Column(db.String(40))
    # 一对多关系中,在一得这端 定义关联对象
    books = db.relationship('Book', backref='author')

class Book(db.Model):
    id = db.Column(db.INTEGER, primary_key=True)
    title = db.Column(db.String(20))
    info = db.Column(db.String(200))
    author_id = db.Column(db.INTEGER,db.ForeignKey('authors.id'))

# 引入脚本管理类,可以在终端中执行多个脚本命令
from flask_script import Manager
# 引入迁移类
from flask_migrate import Migrate, MigrateCommand

# 脚本管理
manager = Manager(app)

# 向Manger中添加数据库 迁移命令
manager.add_command('db_migrate', MigrateCommand)

# 迁移类 迁移初始化
Migrate(app, db)

@app.route('/')
def index():
    return 'hello world'


if __name__ == '__main__':
    #app.run()
    manager.run()


```

init 初始化操作

```sh
>> python py2flask2.py db_migrate init
```

生成迁移数据

```sh
>> python py2flask2.py db_migrate migrate -m 'init'
```

执行迁移,与数据库交互

```sh
>> python py2flask2.py db_migrate upgrade
```

数据处理
----

```python
from flask import Blueprint, render_template, request, redirect

view_blueprint = Blueprint('view', __name__)
from py2flask2 import *


@view_blueprint.route('/')
def index():
    alist = Author.query.all()
    return render_template('index.html', alist=alist)


@view_blueprint.route('/add', methods=['GET','POST'])
def add():
    if request.method == 'GET':
        return render_template('add.html')
    elif request.method == 'POST':
        dict = request.form
        author = Author(dict.get('uname'), dict.get('uemail'))
        db.session.add(author)
        db.session.commit()
        return redirect('/')

@view_blueprint.route('/update/<aid>',methods=['GET', 'POST'])
def update(aid):
    author = Author.query.get(aid)
    if request.method == 'GET':
        return render_template('update.html', author=author)
    elif request.method == 'POST':
        dict = request.form
        author.name = dict.get('uname')
        author.email = dict.get('uemail')
        db.session.add(author)
        db.session.commit()
        return redirect('/')

@view_blueprint.route('/delete/<tid>', methods=['GET', 'POST'])
def delete(tid):
    author = Author.query.get(tid)
    if request.method == 'GET':
        return render_template('delete.html', author=author)
    elif request.method == 'POST':
        db.session.delete(author)
        db.session.commit()
        return redirect('/')
```

HTML示例:

index.html

```html
<body>
<ul>
<a href="/add">添加作者</a>
<table border="1">
    <tr>
        <th width="50">编号</th>
        <th width="100">姓名</th>
        <th width="200">图书</th>
        <th width="100">邮箱</th>
        <th width="100">编辑</th>
    </tr>
    <tr>
        {% for a in alist %}
    </tr>
    <tr>
        <td>{{ a.id }}</td>
        <td>{{ a.name }}</td>
        <td>
            <ul>
                {% for book in a.books %}
                    <li>{{ book.title }}---{{ book.author.name }}</li>
                {% endfor %}
            </ul>
        </td>
        <td>{{ a.email }}</td>
        <td>
            <a href="/update/{{ a.id }}">修改</a>
            &nbsp;&nbsp;
            <a href="/delete/{{ a.id }}">删除</a>
        </td>
    </tr>
    {% endfor %}
</table>
</ul>
</body>
```

add.html

```html
<body>
<form method="post" action="/add">
    用户名：<input type="text" name="uname"><br>
    邮箱：<input type="text" name="uemail"><br>
    <input type="submit" value="提交">
</form>
</body>
```

delete.html

```html
<body>
<form action="/delete/{{ author.id }}" method="post">
    确定要删除&nbsp;{{ author.name }}（邮箱为：{{ author.email }}）&nbsp;吗？<br>
    <input type="submit" value="确定">
    &nbsp;&nbsp;
    <a href="/">取消</a>
</form>
</body>
```

update.html

```html
<body>
<form method="post" action="/update/{{ author.id }}">
    编号：{{ author.id }}<br>
    用户名：<input type="text" name="uname" value="{{ author.name }}"><br>
    邮箱：<input type="text" name="uemail" value="{{ author.email }}"><br>
    <input type="submit" value="提交">
</form>
</body>
```

数据查询:
-----

```python
@view_blueprint.route('/')
def index():
    # 查询所有
    alist = Author.query.all()
    # 查询编号等于1的
    # alist = Author.query.filter_by(id=1)
    # alist = Author.query.filter(Author.id>=1)
    # 排序
    # alist = Author.query.order_by('-id')

    return render_template('index.html', alist=alist)
```
