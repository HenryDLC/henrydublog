---
layout: "post"
title: "Flask-Web(框架四)"
date: "2018-03-25 00:03"
---
新建一个flask项目:
------------

main文件:(程序入口)

```python
from models import db
db.init_app(app)

# 3 创建管理对象
from flask_script import Manager
manager = Manager(app)

# 4 添加迁移命令
from flask_migrate import MigrateCommand,Migrate
Migrate(app,db)
manager.add_command('migrate',MigrateCommand)

# 5 注册蓝图
from html_views import html_blueprint
app.register_blueprint(html_blueprint)


if __name__ == '__main__':
    manager.run()
```

manger文件:(创建flask对象)

```python
# coding=utf-8
from flask import Flask
from werkzeug.routing import BaseConverter

import os
import logging
from logging.handlers import RotatingFileHandler

BASE_DIR = os.path.dirname(os.path.abspath(__file__))


class HtmlConverter(BaseConverter):
    regex = '.*'


def Create_app(Config):
    app = Flask(__name__)
    app.config.from_object(Config)
    app.url_map.converters['html'] = HtmlConverter

    # 日志文件
    logging.basicConfig(level=logging.DEBUG)
    file_log_handler = RotatingFileHandler(os.path.join(BASE_DIR, "logs/ihome.log"), maxBytes=1024 * 1024 * 100,
                                           backupCount=10)
    formatter = logging.Formatter('%(levelname)s %(filename)s:%(lineno)d %(message)s')
    file_log_handler.setFormatter(formatter)
    logging.getLogger().addHandler(file_log_handler)

    return app
```

Config文件:(配置文件)

```python
# coding=utf-8

class Config:
    DEBUG = False
    SQLALCHEMY_DATABASE_URI = "mysql://root:707116148@127.0.0.1:3306/flask_py"
    SQLALCHEMY_TRACK_MODIFICATIONS = True

class DevelopConfig(Config):
    DEBUG = True

class ProductConfig(Config):
    pass
```

models文件:(ORM数据库)

```python
# coding=utf-8

from flask_sqlalchemy import SQLAlchemy
from datetime import datetime
db = SQLAlchemy()

class BaseModel(object):
    create_time = db.Column(db.DATETIME, default=datetime.now())
    update_time = db.Column(db.DATETIME, default=datetime.now(),onupdate=datetime.now())
```

html\_views文件:(路由 静态文件)(开发过程中使用)

```python
# coding=utf-8

from flask  import Blueprint, current_app
html_blueprint = Blueprint('html', __name__)

@html_blueprint.route('/<html:filename>')
def html(filename):
    if not filename:
        filename = 'index.html'

    filename = 'html/' + filename
    html = current_app.send_static_file(filename)
    return html
```

备注:

logs:日志文件

static:静态文件

template:模板文件

api\_v1:apiAPI借口文件

status\_code:状态码文件

status\_code:状态码文件

```python
# coding=utf-8
class RET:
    OK = "0"
    DBERR = "4001"
    NODATA = "4002"
    DATAEXIST = "4003"
    DATAERR = "4004"
    SESSIONERR = "4101"
    LOGINERR = "4102"
    PARAMERR = "4103"
    USERERR = "4104"
    ROLEERR = "4105"
    PWDERR = "4106"
    REQERR = "4201"
    IPERR = "4202"
    THIRDERR = "4301"
    IOERR = "4302"
    SERVERERR = "4500"
    UNKOWNERR = "4501"

ret_map = {
    RET.OK: u"成功",
    RET.DBERR: u"数据库查询错误",
    RET.NODATA: u"无数据",
    RET.DATAEXIST: u"数据已存在",
    RET.DATAERR: u"数据错误",
    RET.SESSIONERR: u"用户未登录",
    RET.LOGINERR: u"用户登录失败",
    RET.PARAMERR: u"参数错误",
    RET.USERERR: u"用户不存在或未激活",
    RET.ROLEERR: u"用户身份错误",
    RET.PWDERR: u"密码错误",
    RET.REQERR: u"非法请求或请求次数受限",
    RET.IPERR: u"IP受限",
    RET.THIRDERR: u"第三方系统错误",
    RET.IOERR: u"文件读写错误",
    RET.SERVERERR: u"内部错误",
    RET.UNKOWNERR: u"未知错误",
}
```
