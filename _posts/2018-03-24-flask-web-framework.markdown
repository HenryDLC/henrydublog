---
layout: "post"
title: "Flask-Web(框架一)"
date: "2018-03-24 23:38"
---
引入包:

Flask:主框架包

render\_template:模板包

SQLAlchemy:ORM数据库操作

BaseConverter:转换器父类

```python
from flask import Flask, render_template
from flask_sqlalchemy import SQLAlchemy
from werkzeug.routing import BaseConverter
```

数据库连接:

固定格式:app.config['SQLALCHEMY\_DATABASE\_URI'] = 'mysql://数据库用户名:数据库密码@数据库ip地址:数据库端口号/数据库名'

```python
app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql://root:707116148@127.0.0.1:3306/flask_db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
# 创建数据库类
db = SQLAlchemy(app)
```

ORM操作数据库 增加Author和Book 两个表

更多操作参考:[http://blog.csdn.net/werewolf\_st/article/details/45933949](http://blog.csdn.net/werewolf_st/article/details/45933949)

```python
class Author(db.Model):
    __talbename__ = 'author'

    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(32), unique=True)

    def __init__(self, name):
        self.name = name

class Book(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(32), unique=True)
    author = db.Column(db.Integer, db.ForeignKey('author.id'))

    def __init__(self, title, author):
        self.title = title
        self.author = author
```

```python
if __name__ == '__main__':
    # 创建表
    db.create_all()

    # 添加数据
    author1 = Author('琼瑶')
    db.session.add(author1)

    author2 = Author('金庸')
    db.session.add(author2)

    book1 = Book('射雕英雄传', 2)
    book2 = Book('天龙八部', 2)
    book3 = Book('雪山飞狐', 2)

    book4 = Book('还珠格格', 1)
    book5 = Book('情深深雨蒙蒙', 1)
    book6 = Book('梅花三弄', 1)

    # 添加多条数据
    db.session.add_all([book1, book2, book3, book4, book5, book6])

    # 提交数据
    db.session.commit()
```

路由设置:

@app.route(‘/路由地址/转换器‘)

返回render\_template()

传入参数:html静态文件名,数据

```python
@app.route('/')
def hello_world():
    return render_template('index.html',helow='world',list1 = range(10))

@app.route('/alist')
def authorlist():
    # 查询所有作者信息
    alist = Author.query.all()
    return render_template('author_list.html', alist = alist)

@app.route('/blist/<author_id>')
def booklist(author_id):
    #  查询指定作者的图书
    blist = Book.query.filter(Book.author == author_id)
    return render_template('book_list.html', blist = blist)
```

自定义转换器

继承BaseConverter父类,修改父类方法传入转换器所需的路由参数(转换规则由路由参数写好直接出传入自定义转换器判定)

```python
class RegexConverter(BaseConverter):
    def __init__(self,map, *args):
        super(RegexConverter, self).__init__(map)
        self.regex = args[0]
app.url_map.converters['re'] = RegexConverter
```

### 代码总结:

```python
# coding=utf-8
from flask import Flask, render_template
from flask_sqlalchemy import SQLAlchemy
from werkzeug.routing import BaseConverter

app = Flask(__name__)

# 自定义转换器
class RegexConverter(BaseConverter):
    def __init__(self,map, *args):
        super(RegexConverter, self).__init__(map)
        self.regex = args[0]
app.url_map.converters['re'] = RegexConverter

# 数据库连接
app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql://root:707116148@127.0.0.1:3306/flask_db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
db = SQLAlchemy(app)

class Author(db.Model):
    __talbename__ = 'author'

    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(32), unique=True)

    def __init__(self, name):
        self.name = name

class Book(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(32), unique=True)
    author = db.Column(db.Integer, db.ForeignKey('author.id'))

    def __init__(self, title, author):
        self.title = title
        self.author = author


@app.route('/')
def hello_world():
    return render_template('index.html',helow='world',list1 = range(10))

@app.route('/alist')
def authorlist():
    # 查询所有作者信息
    alist = Author.query.all()
    return render_template('author_list.html', alist = alist)

@app.route('/blist/<author_id>')
def booklist(author_id):
    #  查询指定作者的图书
    blist = Book.query.filter(Book.author == author_id)
    return render_template('book_list.html', blist = blist)

if __name__ == '__main__':
    # # 创建表
    # db.create_all()
    #
    # # 添加数据
    # author1 = Author('琼瑶')
    # db.session.add(author1)
    #
    # author2 = Author('金庸')
    # db.session.add(author2)
    #
    # book1 = Book('射雕英雄传', 2)
    # book2 = Book('天龙八部', 2)
    # book3 = Book('雪山飞狐', 2)
    #
    # book4 = Book('还珠格格', 1)
    # book5 = Book('情深深雨蒙蒙', 1)
    # book6 = Book('梅花三弄', 1)
    #
    # # 添加多条数据
    # db.session.add_all([book1, book2, book3, book4, book5, book6])
    #
    # # 提交数据
    # db.session.commit()

    app.run(debug=True)
```
