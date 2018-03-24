---
layout: "post"
title: "JavaScript-jQuery-Ajax"
date: "2018-03-25 00:08"
---
JavaScript
==========

窗口等待加载完毕再获取标签

```js
<script>
    // 等window 窗口加载完毕 已经显示的 时候
    window.onload = function () {
        // 获取标签
        var oDiv = document.getElementsById('div1')  
    }
</script>

<body>
  <div id='div1'></div>
</body>
```

.语法 修改 class id value 属性

```js
<style>
    .red{
        color: blue;
    }
</style>

<script>
    window.onload = function () {
        // 获取元素标签
        var oInput = document.getElementById('fist')
        // 通过 点语法
        // class 需要写成 classname
        var sClass = oInput.className

        // 写入
        oInput.className = 'red'
        //background-color 改写成 backgroundColor
        oInput.style.backgroundColor = "green"
    }
</script>

<body>
<input style="background-color: red" type="text" class="one" value="搜索" id="first">
</body>
```

如果 变量 改变成 系统默认的 属性 使用 []

```js
<script>
    window.onload = function () {
        // 获取元素
        var oIput = document.getElementById('input1')
        // []
        var sStr = 'fontSize'
        // 设置input表亲的文字大小为 30px
        // oIput.style.fontSize = '30px'
        // 如果 变量 改变成 系统默认的 属性 使用 []
        oIput.sytle[sStr] = '30px'

    }
</script>

<input type="text" id="input1" value="哈哈哈">
```

innerHTML 修改包裹的内容

```js
<script>
    window.onload = function () {
        // 获取div标签
        var oDiv = document.getElementById('div1')
        // 读写包裹的内容
        var sContent = oDiv.innerHTML
        // 修改内容
        oDiv.innerHTML = '修改内容'

    }
</script>

<div id="div1" class="box">div包裹的内容</div>
```

函数的定义与执行

```js
<script>
    window.onload = function () {
        // 函数定义
        function fnTest() {
            alert('函数的定义')
        }
        // 函数执行
        fnTest()

        // 带参数的函数
        function fnSum(a,b) {
            var result = a + b
            alert(result)
        }
        fnSum(1,5)
    }
</script>
```

变量/函数的预解析

```js
<script>
    window.onload = function () {
        fnTest()

        // 变量的预解析:系统给 变量赋值 undefined
        alert(iNumber)

        // 函数的预解析: 如果函数先被执行 后定义 系统会在执行的是偶 检查代码
        function fnTest() {
            alert('函数的预解析')

        }
         var iNumber = 100
    }
</script>
```

匿名函数

```js
<script>
    window.onload = function () {
        // 正常定义函数
        function fnTest() {
            alert('正常函数')
        }

        var oBtn = document.getElementById('btn')
        // 监听按钮点击
        // 匿名函数
        oBtn.onclick = function () {
            alert('匿名函数')
        }

    }
</script>

<body>
    <input type="button" id="btn" value="普通按钮">
</body>
```

if判断语句

```js
<script>
    window.onload = function () {
        var iOne = 100
        var iTwo = 200

        // 单重判断
        if(iOne > iTwo){
            alert('100>200')
        }else {
            alert('222222')
        }

        // 多重判断
        var iNumber = 0
        if (iNumber == 1){
            alert('星期一')
        }else if(iNumber == 2){
            alert('星期二')
        }else if(iNumber == 3){
            alert('星期三')
        }else if(iNumber == 3) {
            alert('星期四')
        }else if(iNumber == 3){
            alert('星期五')
        }else if(iNumber == 3){
            alert('星期六')
        }else if(iNumber == 3){
            alert('星期天')
        }else{
            alert('数据错误')
        }
    }
</script>
```

return 关键字

```js
<script>
    window.onload = function () {
        // return 阻止程序运行 无论是不是在函数内 都可以组织程序向下运行
        function fnTest() {
            alert('111')
            return
            // retrun 下面的代码 被阻止
            alert('2222')
        }
        a = 10
        if (a > 18){
            alert('成年人')
        }else{
            alert('未成年人')
            return
        }

        alert('允许进入')

    }
</script>
```

```js
<script>
    window.onload = function () {
        // 带返回值函数
        function fnSum(a,b) {
            var result = a+b
            return result
        }
        var a = fnSum(20,5)

        if (a > 18){
            alert('成年人')
        }else{
            alert('未成年人')
            return
        }

        alert('允许进入')

    }
</script>
```

```js
<script>
    window.onload = function () {
        // 阻止系统默认行为 submit
        oForm = document.getElementById('form')
        oForm.onsubmit = function () {
            return false
        }

    }
</script>


<body>
    <form action="" id="form">
        <input type="text" name="name">
        <input type="submit" id="sub">
    </form>
</body>
```

条件运算符

```js
<script>
    window.onload = function () {
        var iNumber = 20
        var sNumber = '20'

        // 全等于 先判断类型在判断值
        if (iNumber === sNumber){
            alert('True')
        }else{
            alert('False')
        }

        // 等于 只判断值 不判断类型
        if (iNumber == sNumber){
            alert('True')
        }else{
            alert('False')
        }

        // 大于小于等于大于等于小于等于 与或非.... 与其他语言相同

    }
</script>
```

显示隐藏div块案例

```js
<style>
    .box{
        width: 200px;
        height: 200px;
        background: red;

        /* 默认隐藏*/
        display: none;
    }
</style>

<script>
    window.onload = function () {
        // 获取元素
        var oBtn = document.getElementById('btn')
        var oDiv = document.getElementById('div1')
        // 监听 btn
        oBtn.onclick = function () {
            // 判断隐藏
            var bJudege = oDiv.style.display
            if (bJudege == 'none' || !(bJudege )){ // 隐藏
                // 按钮文字隐藏
                // 改变div的display:block
                oBtn.value = '隐藏'
                oDiv.style.display = 'block'
            }else{
                oBtn.value = '显示'
                oDiv.style.display = 'none'
            }
        }


    }
</script>

<div>
    <input type="button" value="显示" id="btn">
    <div class="box" id="div1"></div>
</div>
```

数组-定义

```js
<script>
    window.onload = function () {
        // 实例化对象
        var aOneArry = new Array(1,2,3,'a','b')
        // 批量创建
        var aTwoArry = [1,2,3,'a','b']
         // 多维数组
        var aThreeArry = [[1,2,3,],['a','b']]
        alert(aThreeArry[1][0])
    }
</script>
```

数组-操作

```js
<script>
    window.onload = function () {
        var aOneArry = [1,2,3,'a','b','c']
        // 增
        aOneArry.push('f')
        // 删 最后一个开始删
        aOneArry.pop()
        // 改
        aOneArry[0] = 'k'
        // 查
        aOneArry[4]

        // splice 参数:开始位置(角标) 删除个数 其余参数都为增加元素
        aOneArry.splice(1,4,'a','b','c','a')
        alert(aOneArry)

        // reverse反转 颠倒顺序
        aOneArry.reverse()

        // 将数组拆分成 字符串
        // aOneArry.join() 有逗号
        // aOneArry.join('') 没有逗号
        // aOneArry.join('_') 元素之间加 _ 下划线
        var sStr = aOneArry.join("")
        alert(sStr)

        //数组的元素个数 length
        var iCount = aOneArry.length
        alert(iCount)

        // 元素第一次出现的角标
        // 一个列表中 出现多次相同元素 可以找到特定元素在数组中第一次出现的位置
        var iIndex = aOneArry.indexOf('a')
        alert(iIndex)


    }
</script>
```

```js
// 遍历数组&数组画三角
<style>
    .box{
        width: 500px;
        height: 500px;
        border: 1px solid black;
        margin:auto;

        text-align: center;
    }
</style>
<script>
    window.onload = function () {
        // 循环
        for (var i = 0; i < 3; i++){
            alert('12')
        }

        // 遍历数组
        var aOneArray = ['1',2,3]
        for (var j = 0; j < aOneArray.length; j++){
            var sValue = aOneArray[j]
            alert(sValue)
        }

        // 金字塔
        var oDiv = document.getElementById('div1')
        var sStr = ""
        for (var c = 0; c < 20; c++){
            for (var z = 0; z < c; z++){
                sStr += '*'
            }
            sStr += "<br>"
        }

        oDiv.innerHTML = sStr

    }
</script>

<body>
    <div class="box" id="div1"></div>
</body>
```

去重

```js
<script>
    window.onload = function () {
        var aOneArray = [1,2,,2,3,3,'a','b','c','c','d',1,2,3]

        // 遍历 循环
        // 角标       0   1   2   3   4   5
        // indexOf() 0   0   2   2   4   4
        var aNewArry = []
        for (var index = 0; index < aOneArray.length; index++){
            var element = aOneArray[index]
                if (index == index.indexOf(element)){
                    aNewArry.push(element)
                }


        }

    }
</script>
```

列表加载数组数据

```js
<script>
    window.onload = function () {
        // 获取列表元素
        var aList = document.getElementById('list')
        // 数组的数据
        var aMovieName = ['movie1','movie2','movie3','movie4','movie5','movie6']
        // 循环遍历 拼接数据
        var sStr = ''
        for (var i = 0; i < aMovieName.length; i++){
            var name = aMovieName[i]
            sStr += "<li>" + name + "</li>"

        }

        // 赋值
        aList.innerHTML= sStr
    }
</script>
```

类型转换

```js
<script>
    window.onload= function () {
        var iOne = 100
        var iTwo = '200'

        // 将字符串 转换类型 + 字符串
        // parseInt
        var iSam = iOne + parseInt(iTwo)

        // js浮点数 需要乘以系数
        var fOne = 0.1
        var fTwo = '0.2'
        var fThree = 0.2
        // iSam = fOne + parseFloat(fTwo)
        iSam = fOne*100 + fThree*100


        // - * / 隐式转换 系统自己判断转换类型
        alert(iSam/100)

    }
</script>
```

NaN : 不是纯数字

```js
<script>
    window.onload = function () {
        var sOne = '123abc'
        var sTwo = 'abc123'

        // NaN : 不是纯数字
        alert(parseInt(sOne)) // 123
        alert(parseInt(sTwo)) // NaN

        alert(isNaN(sTwo)) // 返回True    它是一个'不是纯数字的类型'
    }
</script>
```

调试方法

```js
<script>
    window.onload =function () {

        // 阻止程序的运行
        alert('阻止程序的运行')

        // 控制台输出
        console.log('控制台输出')
        var aOneArray = [1,2,3,'a']
        console.log(aOneArray)

        // 修改网页title
        document.title = 'test'

        // 在body里面显示 占位置
        document.write('test')
    }
</script>
```

全局变量&局部变量

```js
<script>
    window.onload = function () {

        // 全局变量
        var iNumber = 10

        function fnTest() {
            // 局部变量
            var iNumber = 30
        }

        fnTest()
        alert(iNumber)

    }
</script>
```

字符串操作

```js
<script>
    window.onload = function () {
        // 字符串操作
        var sName = '老王'
        var sSon = '小明'
        var sWife = '霜霜'

        var sStr = sName + sSon + sWife + "是不是一家人"
        alert(sStr)

        // 类型转换
        var sOne = 'abc123' // NaN
        // parseInt(sOne)
        var fOne = 0.1
        var fTwo = 0.2
        var sSum = fOne*100 + fTwo*100

        // 截取字符串
        var sTwo = 'abcdef'
        var sTwoNew = sTwo.substring(1,4) // 角标开始位置 ~ 结束位置(不包含结束位置) bcd
        sTwoNew = sTwo.substring(1) // 截取不包含该角标后的所有字符串
        alert(sTwoNew )

        // 数组拆分 字符串 join("")
        // 拆分成数组
        var aOneArray = sTwo.split("")
        console.log(aOneArray)

        // indexOf() 查找字符的角标
        // 如果结果是 -1 说明字符串不存在该字符
        var sThree = '1234567'
        var iIndex = sThree.indexOf("2")
        alert(iIndex)



        // 字符串倒置
        var sStr = '123456789'
        // 字符串拆分成数组
        var aOneArray = sStr.split('')
        // 数字倒置
        var aNewArray = aOneArray.reverse()
        // 数组转换字符串
        var aNewStr = aNewArray.join("")

        // 链式 连写
        var sNewStr = sStr.split("").reverse().join("")

        alert(aNewStr)

    }
</script>
```

定时器

```js
<script>
    window.onload = function () {
        // 执行一次
        // setTimeout(功能,时间)
        function fnAlert() {
            alert('只执行一次的定时器')
        }
        var timer = setTimeout(fnAlert,2000)
        // 清楚计时器 2s时间
        clearTimeout(timer)

        // 反复执行
        // setInterval(功能,时间)
        function fnAlert2() {
            alert('重复执行的计时器')
        }
        var repeatTimer = setInterval(fnAlert2,1000)
        clearInterval(repeatTimer)

        // 第二种写法
        setTimeout(function () {
            alert('执行一次定时器 第二种写法')
        },1000)

    }
</script>
```

封闭函数

```js
<script>
    window.onload = function () {
        // 匿名函数 -- 直接运行
        // -- 打开 内部可能做一些操作
        (function () {
            alert('匿名函数')
        })()

        ~function () {
            alert('封闭函数')
        }()

        !function () {
            alert('封闭函数2')
        }()

        // 一份代码 多个重名函数 最下面的覆盖以前的
        function fnAlert() {
            alert('666666')
        }
        fnAlert();

        // 避免 重名函数 冲突
        (function fnClose() {
            function fnAlert() {
                alert('23333')
            }
            fnAlert()
        })()

    }
</script>
```

jQuery
======

关联函数库 测试

```js
<body>

// 关联jQuery 函数库 CDN
<script src="https://cdn.bootcss.com/jquery/1.12.2/jquery.min.js"></script>

<script>
    // 测试
    alert($)
</script>

</body>
```

Js & jQuery 对比

```js
<script>

    $(document).ready(function () {
        // 获取元素
        var $div = $('#div1')
        // 获取属性
        var sSize = $div.css("font-size")

        alert('jQuery + ' + sSize)

    })

    window.onload = function () {
        // 获取元素
        var aDiv = document.getElementById('div1')
        // 获取属性
        var sSize = aDiv.style.fontSize

        alert('Js +' + sSize)
    }
</script>
```

jQuery 的css属性操作

```js
<body>

<script>
    // 常用选择器
    // JS 标签 类 ID 层级后代 属性选择器div[class='abs]

    $(document).ready(function () {
        // jquery
        // 标签
        var $element = $('div')

        //类
        $element = $('.para')

        //ID
        $element = $('#spa')

        // 层级选择器
        $element = $('.box div')

        // 属性选择器
        $element = $('div[class = box3]')
        $element.css({'color':'red'})

    })


</script>

<div>div标签</div>
<p class = 'para'>类 段落</p>
<span id="spa">ID span</span>

<div class="box">
    <div>层级选择器</div>
</div>

<div class="box2">box2</div>
<div class="box3">box3</div>


</body>
```

选择器的过滤

```js
<body>

<script>
    $(function () {
        // has 包含x元素 x个元素x
        var $element = $('div').has('p')
        $element.css({'background-color':'red'})

        // not 除了x个标签 外的 div元素
        // 选中的 除了 .box2 的元素外的 所有其余div标签
        $element = $('div').not('.box2')

        // eq(角标) 等于
        var $eq = $('li').eq('2')
        $eq.css({'color':'red'})
    })
</script>
<div class="box">
    <p>段落标签</p>
</div>

<div class="box1">box1</div>
<div class="box2">box2</div>
<div class="box3">box3</div>

<ul class="list">
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
</ul>

</body>
```

选择器的转移

```js
<body>
<script>
    $(function () {
        // 选择器转移
        var $div = $('.box4')

        // 上一个 prev()
        var $element = $div.prev()
        // 上面所有
        $element = $div.prevAll()

        // 下面一个
        $element = $div.next()
        // 下面所有
        $element = $div.nextAll()

        // 除了自己 选中其他所有
        $element = $div.siblings( )

        // 父子关系
        // 父元素 例子中 元素为body
        $element = $div.parent()

        // 子元素 获取的所有子元素
        $element = $div.children()

        //查找里面的子元素
        $element = $div.find('.grandson')
        $element.css({'color':'red'})
    })
</script>

<div class="box1">1</div>
<div class="box2">2</div>
<div class="box3">3</div>

<div class="box4">4
    <div class="child1">子元素1</div>
    <div class="child2">子元素2</div>
    <div class="child3">子元素3
    <div class="grandson">孙子</div>
    </div>
    <div class="child4">子元素4</div>
</div>

<div class="box5">5</div>
<div class="box6">6</div>
<div class="box7">7</div>

</body>
```

length判断元素是否有无,元素总共个数

```js
<body>
<script>
    // length 判断标签是否有无 和标签的个数
    $(function () {
        // 是否有 标签
        // 没有 length == 0
        var iNumber = $('.box3').length

        // 判断个数
        iNumber = $('div').length
        alert(iNumber)

    })
</script>
<div></div>
<div></div>
<div></div>
<div></div>
<div></div>
<div></div>
</body>
```

click事件.this绑定

```js
<body>
<script>
    $(function () {
        //设置宽高 背景色
        var $div = $('.box')
        $div.css({
            'width':'200',
            'height':'200',
            'background-color':'red'
        })
        // 监听点击事件 改变颜色
        // js写法
        // $div.onclick = function () {
        //     $div.style.backgroundColor = 'green'
        // }
        $div.click(function () {
            // $div.css({'background':'green'})

            // $(this) --> jquery中 监听的这个对象
            $(this).css({'background':"blue"})
        })

        $('.list li').click(function () {
            // 5个li标签 绑定 6次 点击那次 this 代表哪个
            $(this).css({'color':'red'})

            // index
            // 获取元素的角标
            // 5个li标签 绑定 6次 点击那次 this 代表哪个
            alert($(this).index())

        })

    })
</script>
<div class="box"></div>

<ul class="list">
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
</ul>

</body>
```

操作样式名称

```js
<body>
<style>
    div{
        width: 200px;
        height: 200px;
    }
    .box{
        background-color: red;
    }
    .green{
        background-color: green;
    }
</style>
<script>
    $(function () {
        // 给一个标签 添加类名称
        $('div').addClass('green');
        // 给一个标签 删除他的类名称
        $('div').removeClass('green')
        // 给一个标签 切换 它的类名称
        $('div').toggleClass('box')

    })
</script>

<div class="box"></div>

</body>
```

标签内属性 操作(非css样式)

```js
<body>
<script>

    $(function () {
        // 操作标签内的属性
        // 读取
        var sStr = $('img').prop('src');
        console.log(sStr);
        // 写入
        $('img').prop({'src': 'images/002'});


        var sContent1 = $('input').prop('value')
        // prop 获取value值 可以在获取属性后 用.val()方法简写
        var sContent2 = $('input').val()

        // 获取自定义属性
        alert(sContent2)
        alert($("img").attr("renminbi"));

        // 获取标签 包裹的内容
        var content = $('div').html()
        // 写入
        $('div').html('新加内容')
        console.log(content)


    })

</script>

<img src="../../favicon.ico" alt="图片01" renminbi="10000">
<div class="box">div包裹的内容</div>
<input type="text" class="sContent" value="test">
</body>
```

Ajax&Json
=========

Json书写格式

```json
{
  "code":1,
  "data":{
    "name":"test",
    "age":20
  }
}
```

Ajax请求数据

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<script src="https://cdn.bootcss.com/jquery/1.12.4/jquery.min.js"></script>
<script>
    $(function () {
        $.ajax({
            // 请求地址
            url:"userDate.json",
            // 请求方式 GET POST
            type:"get",
            // 数据格式
            dateType:"json",
            // 请求成功
            success:function (callBackDate) {
                console.log(callBackDate)
                if (callBackDate.code == 1){
                    console.log(callBackDate.date.name)
                }else{
                    console.log("请先注册")
                }
            },
            // 请求失败
            error:(function () {
                alert("请求失败,请稍后再试")
            })
        })
    })
</script>
<body>

</body>
</html>
```

局部刷新

```js
<script>
     $(function () {
         $(".welcome").click(function () {
             // 请求服务器上的数据
             $.ajax({
                 url:"userDate.json",
                 type:"get",
                 dateType:"json",
                 success:function (callBackDate) {
                     console.log(callBackDate)
                     // 如果登陆成功, 替换名字 局部刷新
                     if (callBackDate == 1){
                         // 隐藏 登录 和 注册
                         $(".user_login_btn").hide()
                         // 显示 欢迎
                         $(".user_info").show()
                         // 替换 以前的数据
                         $("user_info em").html(callBackDate.date.name)
                     }
                 },
                 error:function () {
                     alert("请先注册")
                 }

             })

         })
     })
</script>
```

跨域请求 / 非服务器传参

url:域名下面的data.js文件

数据类型是jsonp

```js
<script>
    $(function () {
        $.ajax({
            // 网站:域名下面的data.js文件
            url:"http://localhost:8888/jsonp/data.js",
            type:"get",
            // 数据类型是jsonp
            datatype:"jsonp",
            // 给服务传参
            data:{word:"g"}
            // js资源文件内部数据 对称的名称
            jsonpCallback:"fnCallBackMethod"
        })
            .done(function (jsonpDate) {
                console.log(jsonpDate);
            })
            .error(function () {
                alert("请求失败")
            })
    })
</script>
```
