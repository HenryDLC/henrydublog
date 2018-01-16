---
layout: "post"
title: "quantOS量化平台搭建"
date: "2018-01-16 22:11"
---



Tushare
=======

* pip install tushare
* pip install tushare --upgrade

DataCore
========

引用:<https://www.quantos.org/datacore/doc.html>

实时行情:

wget <http://www.quantos.org/downloads/datacore/mdlink-1.2-linux.7z>

数据服务

wget <http://www.quantos.org/downloads/datacore/tktools-linux.7z>

数据工具

wget <http://www.quantos.org/downloads/datacore/tktools-linux.7z>

数据API安装:
--------

依赖:

* pandas
* numpy
* pyzmq
* msgpack\_python
* python-snappy

下载DataApi

git clone <https://github.com/quantOS-org/DataApi.git>

服务端安装mdlink/qms
---------------

服务端依赖:

* zeromq
* protobuf
* JDK8

### mdlink/qms配置文件准备

\# etc/jzs.json

\# json数据 把注释删掉

```json
{
    "global": {
        "mdlink_addr": "tcp://127.0.0.1:8700", // mdlink连接地址
        "qms_addr": "tcp://127.0.0.1:9000" //qms连接地址
    },

    "qms": {
        "id": "qms01", //qms名称
        "check_code": true, //qms重启时，是否对tick文件进行代码检查
        "save_tk": true, //是否保存tick文件，保存tick后，qms在重启时会根据保存的tick还原分钟线，否则重启后会丢失之前的分钟线数据
        "addr": "tcp://0.0.0.0:9000" // qms地址及端口
    },

    "mdlink":
    {
        "mdlink2": { // 配置名称
            "pub_addr": "tcp://0.0.0.0:8700", //行情推送地址及端口
            "do_merge": true, // 是否进行行情合并
            "route": "MERGE", // mdlink类型，MERGE表示为行情合并mdlink
            "time_tolerance": 10000,
            "sources": [ // 上游mdlink行情源及相应地址和端口
                { "addr": "tcp://127.0.0.1:10001", "id": "future1" },
                { "addr": "tcp://127.0.0.1:10002", "id": "stock1" }
            ]
        },

        "future1": {
            "pub_addr": "tcp://0.0.0.0:10001",
            "route": "CTP", // mdlink类型，CTP表示为mdlink_ctp读取的配置
            "source_id": 1, // source编号
            "sources": [ // CTP行情源配置，该配置由期货公司提供
                {
                    "id": "future1",
                    "route": "CTP",
                    "front": [
                        "180.168.146.187:10010",
                        "180.168.146.187:10011"
                    ],
                    "broker": "9999",
                    "investor": "xxxxx",
                    "passwd": "xxxxx",
                    "udp": false,
                    "multicast": false
                }
            ]
        },

        "stock1": {
            "pub_addr": "tcp://0.0.0.0:10002",
            "route": "TDF", // mdlink类型，"TDF"表示为mdlink_tdf读取的配置
            "source_id": 2,
            "sources": [ // TDF行情源配置，该配置由万得宏汇公司提供
                {
                    "id": "stock1",
                    "route": "TDF",
                    "addr": "xxx.xxx.xxx.xxx",
                    "port": xxxx,
                    "username": "TDXXXXXX",
                    "passwd": "xxxxxx",
                    "markets": "SZ-2;SH-2;CF-2"
                }
            ]
        },
        "stock2": {
            "pub_addr": "tcp://0.0.0.0:10003",
            "route": "SINA",
            "source_id": 3,
            "sources": [
                {
                    "id": "stock2",
                    "route": "SINA",
                    "addr": "http://hq.sinajs.cn/list=",
                    "markets": "SZ;SH;",
                    "insttypes": "1;2;3;4;5;100;"
                }
            ]
        },

        "stock3": {
            "pub_addr": "tcp://0.0.0.0:10004",
            "route": "TENCENT",
            "source_id": 4,
            "sources": [
                {
                    "id": "stock3",
                    "route": "TENCENT",
                    "addr": "http://qt.gtimg.cn/q=",
                    "markets": "SZ;SH;",
                    "insttypes": "1;2;3;4;5;100;"
                }
            ]
        }
   }
}
```

数据准备程序:

./scripts/prepare\_data.sh

启动程序:

./scripts/start.sh

\# 可以依次启动全部mdlink程序和qms程序。如果需要单独启动某个程序，需要先运行：

. scripts/set-env.sh

运行"./bin/程序名 配置名"或者运行"jztsctrl 程序名 配置名 start"即可启动程序

* ./bin/qms2 qms2 \> qms2.log&
* ./bin/mdlink2 mdlink2 \> mdlink2.log&
* ./bin/mdlink\_ctp future1 \> future1.log&
* ./bin/mdlink\_tdf stock1 \> stock1.log&

或

* jztsctrl qms2 qms2 start
* jztsctrl mdlink2 mdlink2 start
* jztsctrl mdlink\_ctp future1 start
* jztsctrl mdlink\_tdf stock1 start

### DataServer配置准备

\# dataserver-dev.conf

\# json数据 把注释删掉

```json
{
  "mdlink": { // 需要连接的mdlink地址
    "addr": "tcp://127.0.0.1:8700",
    "pub_addr": "tcp://127.0.0.1:8701"
  },

  "qms": { //需要连接的qms地址
    "addr": "tcp://127.0.0.1:9000"
  },

  "frontend" : { // dataserver地址（DataApi用）
    "zmqrpc_addr" : "tcp://0.0.0.0:8910"
  },

  "http_server" : { // websocket访问端口
    "port"      : "8912",
    "doc_root"  : "web/new"
  },

  "his_bar" : { // 历史行情数据文件配置（1.2版本加入）
    "bar1m_path"  : "D:/store/data/raw_data/BarONE/@MKT/@YYYY/@MKT@YYYYMMDD-1M.h5",
    "bar5m_path"  : "D:/store/data/raw_data/BarFIVE/@MKT/@YYYY/@MKT@YYYYMMDD-5M.h5",
    "bar15m_path" : "D:/store/data/raw_data/BarQUARTER/@MKT/@YYYY/@MKT@YYYYMMDD-15M.h5",
    "tick_path"   : "D:/store/data/raw_data/Tick/@MKT/@YYYY/@MKT@YYYYMMDD-tk.h5"
  }  
}
```

### 启动DataServer

启动DataServer前需要确保mdlink和qms已经正常运行。

数据准备程序

./script/prepare\_data.sh

启动DataServer

./script/start.sh



JAQS
====

参考:<https://github.com/quantOS-org/JAQS/blob/master/doc/install.md>

### 安装依赖:

snappy的开发包libsnappy-dev:

* sudo apt-get install libsnappy-dev

安装snappy

* pip install python-snappy

安装JAQS

* pip install jaqs

验证:

```python
import jaqs

Jaqs.__version__
```



TradeSim
========

下载TradeSim源码

git clone <https://github.com/quantOS-org/TradeSim.git>

加载到项目里…

### 安装vnTrader客户端:

git clone <https://github.com/quantOS-org/TradeSim.git>

cd ./TradeSim-master/vnTrader

python vtMain.py



vn.py
=====

参考:<https://github.com/vnpy/vnpy/wiki/Ubuntu%E7%8E%AF%E5%A2%83%E5%AE%89%E8%A3%85>

安装依赖

* sudo apt-get install mongodb
* sudo apt-get install libboost-all-dev
* sudo apt-get install cmake
* sudo apt-get install git
* sudo apt-get install libsnappy-dev
* sudo apt-get install python-snappy

准备编译工具

* apt-get install build-essential
* apt-get install python-dev

获取框架安装脚本

wget <https://github.com/vnpy/vnpy/archive/v1.7.2.zip>

安装脚本

bash install.sh
