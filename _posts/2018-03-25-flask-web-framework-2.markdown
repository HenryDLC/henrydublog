---
layout: "post"
title: "Flask-Web(框架二)"
date: "2018-03-25 00:00"
---
应用上下文
-----

current\_app对象:共享数据的作用

缺点:储存在服务器内存中,服务器挂掉,信息丢失

解决方法:Redis存储

```python
from flask import Flask, current_app, g

app = Flask(__name__)
app.hello = 'world'

@app.route('/')
def index():
    # print app.hello
    current_app.hello = 'hello flask'
    return 'hello world'

@app.route('/show')
def show():
    return current_app.hello

if __name__ == '__main__':
    app.run(debug=True)
```

g对象:

每一个新的app对象,都会产生相应的g对象

注:在请求响应当中会存在一个g对象,但是当请求结束后g对象则被释放掉

```python
from flask import Flask, current_app, g

app = Flask(__name__)
app.hello = 'world'

@app.route('/')
def index():
    # print app.hello
    g.hello = 'hello python'
    return 'hello world'

@app.route('/show')
def show():
    print g.hello
    return 'hello world'

if __name__ == '__main__':
    app.run(debug=True)
```

请求上下文
-----

GET:

```python
@app.route('/get1')
def get1():
    return render_template('get1.html')


@app.route('/get2')
def get2():
    dict = request.args
    a = dict.getlist('a')
    b = dict.getlist('b', 'None')
    c = dict.getlist('c')
    return render_template('get2.html', a=a, b=b, c=c)
```

POST:

```python
@app.route('/post1')
def post1():
    return render_template('post1.html')


@app.route('/post2', methods=['POST'])
def post2():
    dict = request.form
    uname = dict.get('uname')
    upwd = dict.get('upwd')
    ugender = dict.get('ugender')
    uhobby = dict.get('uhobby')
    print uhobby
    return render_template('post2.html', uname=uname, upwd=upwd, ugender=ugender, uhobby=uhobby)
```

响应
--

视图函数 return 返回的必须是报文数据(字符串) 所以不能用return 10 直接返回整形10

解决:可以返回str类型的字符串’10’ 或者 返回 render\_template(‘xxx.html’)

```python
@app.route('/list1')
def list1():
    return '%d'%10
```

重定向
---

直接返回一个重定向的连接地址,浏览器再发送一个新的请求

调用from flask import redirect

```python
from flask import redirect

@app.route('/red')
def red():
    return redirect('/green')
```

JSON
----

调用from flask import jsonify

```python
from flask import jsonify

@app.route('/json1')
def json1():
    return render_template('json1.html')

@app.route('/json2')
def json2():
    return jsonify(title='hello world ')
```

\# json1.html 点击显示按钮 接受json2发出的json数据

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.min.js"></script>
    <script>
        $(function () {
            $('#btnShow').click(function () {
            $.get('/json2',function (data) {
                $('#divShow').html('<h1>'+data.title+'</h1>');
            });
        });
        })
    </script>
</head>
<body>
<input type="button" id="btnShow" value="显示">
<div id="divShow"></div>
</body>
</html>
```

状态码
---

自定义状态码

场景一: 强制抛出404错误 发出json数据,发送成功抛出201状态码

```python
from flask import jsonify, abort

@app.route('/json2')
def json2():
    abort(404)#错误
    # raise Exception()#异常
    return jsonify(title='hello flask'),201
```



场景二:处理404错误

```python
@app.errorhandler(404)
def handle404(exception):
    return render_template('404.html',exception=exception)
```

状态保持
----

cookie

```python
from flask import make_response

@app.route('/cook_set')
def cookie_set():
    response = make_response('ok')
    response.set_cookie('h1', 'hello flask', max_age=60)
    return response

@app.route('/cookie_get')
def cookie_get():
    h1 = request.cookies.get('h1')
    return h1
```

session

```python
from flask import session

@app.route('/session_set')
def session_set():
    session['hello'] = 'world'
    return 'ok'

@app.route('/session_get')
def session_get():
    return session.get('hello', 'None') # 如果没有session则显示None值
```

蓝图
--

项目中进行视图函数管理,便于维护,需要将视图文件分到不同的包或模块中

```python
# -*- coding: utf-8 -*-
# session_set.py

from flask import Blueprint, session

# 新建蓝图对象
# 参数1:功能描述
# 参数2:模块名称
session_set_blueprint = Blueprint('session_set',__name__)

# 蓝图 注册路由
@session_set_blueprint.route('/')
def session_set():
    session['hello'] = 'world'
    return 'ok'
```

```python
# -*- coding: utf-8 -*-
# session_get/session_get.py

from flask import Blueprint, session
session_get_Blueprint = Blueprint('session_get',__name__)

@session_get_Blueprint.route('/')
def session_get():
    return session.get('hello', 'None')
```

注册蓝图:

```python
# -*- coding: utf-8 -*-
from flask import Flask, blueprints

app = Flask(__name__)
app.config['SECRET_KEY'] = 'helloworld'


@app.route('/')
def index():
    # print app.hello
    return 'hello world'

# app中注册蓝图
from session_set import session_set_blueprint
app.register_blueprint(session_set_blueprint, url_prefix='/session_set')

from session_get.session_get import session_get_Blueprint
app.register_blueprint(session_get_Blueprint, url_prefix='/session_get')

if __name__ == '__main__':
    app.run(debug=True)
```

钩子(结合g变量)
---------

before\_first\_request:在处理第一个请求前运行

before\_request:在每次请求前运行

after\_request:如果没有未处理的异常抛出,在每次请求后运行

teardown\_request:在每次请求后运行,即使有未处理的异常抛出

```python
# -*- coding: utf-8 -*-
from flask import Flask, g

app = Flask(__name__)
app.config['SECRET_KEY'] = 'helloworld'


@app.route('/')
def index():
    if g.csrf == 1:
        print 'pass'
    return 'hello world'

@app.before_request
def before():
    g.csrf = 1
    print '-------------before'

@app.after_request
def before(response):
    print '-------------after'
    if g.csrf == 1:
        response.set_cookie('csrf','123')
    return response

@app.teardown_request
def before(exception):
    print '-------------teardown_request'

if __name__ == '__main__':
    app.run(debug=True)
```

场景:

before\_request:用于请求数据预处理

after\_request:用于在未发生异常时进行资源释放

teardown\_request:用于发生异常时记录日志等功能

配置文件
----

所有配置文件详见:<http://www.pythondoc.com/flask/config.html>

将所有配置信息写到独立的.py文件中

开发模式&生产模式 两种配置不同 定义不同的类,继承与Config父类

```python
# -*- coding: utf-8 -*-

class Config:
    DEBUG = False
    SECRET_KEY = 'helloworld'
    SQLALCHEMY_DATABASE_URI = 'mysql://mysql://root:707116148@127.0.0.1:3306/flask_db'
    SQLALCHEMY_TRACK_MODIFICATIONS = False


class DevelopmentConfig(Config):
    DEBUG = True


class ProductConfig(Config):
    SQLALCHEMY_DATABASE_URI = 'mysql://root:707116148@127.0.0.1:3306/flask_db'
    SQLALCHEMY_TRACK_MODIFICATIONS = False
```

调用写好的配置文件

```python
# -*- coding: utf-8 -*-
from flask import Flask
from setting import DevelopmentConfig, ProductConfig

app = Flask(__name__)

app.config.from_object(DevelopmentConfig)


@app.route('/')
def index():
    return 'hello world'


if __name__ == '__main__':
    app.run()
```
