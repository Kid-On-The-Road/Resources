# 《JavaScript高级-笔记》

# 回顾

1. CSS的基本选择器

| 选择器分类 | 语法   |
| ---------- | ------ |
| 标签选择器 | 标签名 |
| 类选择器   | .类名  |
| ID选择器   | \#ID   |
| 通用选择器 | *      |

2. CSS的扩展选择器选择元素

| 扩展选择器 | 语法符号                |
| ---------- | ----------------------- |
| 层级选择器 | 父选择器 子孙选择器     |
| 属性选择器 | 标签名[属性名="值"]     |
| 伪类选择器 | 选择器:状态             |
| 并集选择器 | 选择器,选择器           |
| 交集选择器 | 标签名.类名   标签名#ID |

3. 能够说出盒子模型的属性

| 属性    | 作用                 |
| ------- | -------------------- |
| width   | 宽度                 |
| height  | 高度                 |
| margin  | 外边距               |
| padding | 内边距               |
| border  | 边框：线型 颜色 粗细 |

4. 五种数据类型

| 关键字    | 说明     |
| --------- | -------- |
| number    | 数值     |
| boolean   | 布尔     |
| string    | 字符串   |
| object    | 对象     |
| undefined | 未初始化 |

5. JavaScript的组成部分

| JavaScript的组成部分 | 说明                                   |
| -------------------- | -------------------------------------- |
| ECMA Script          | 脚本语言语法基础                       |
| BOM                  | 浏览器对象模型，用于操作浏览器中的对象 |
| DOM                  | 文档对象模型，用于操作网页中元素和属性 |



# 学习目标

1. <font color="red">函数</font>
   1. 能够在JS中定义命名函数和匿名函数
2. <font color="red">事件</font>
   1. 能够使用JS中常用的事件
3. JavaScript的内置对象
   1. 能够使用数组中常用的方法
   2. 能够使用JavaScript对CSS样式进行操作
   3. 能够使用日期对象常用的方法
4. BOM
   1. 能够使用window对象常用的方法
   2. 能够使用location对象常用的方法和属性
   3. 能够使用history对象常用的方法
5. <font color="red">DOM</font>
   1. 能够使用DOM中来查找节点
   2. 能够使用DOM来增删改节点



# 学习内容

## 命名函数和匿名函数

### 目标

命名函数和匿名函数的使用

### 函数的概述

什么是函数：类似于Java中方法，将一部分代码起名放在一个代码块中重用，供其它的代码调用。

两种函数：命名函数和匿名函数

- 命名函数：可以通过名字重复调用

- 匿名函数：通常只使用一次，后期主要用在事件处理函数中。

### <font color="red">命名函数的格式</font>

```javascript
function 函数名(参数列表) {
  代码块
  [return 返回值;]
}
```



### 自定义函数

#### 需求

定义一个函数实现加法功能

#### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>函数</title>
</head>
<body>
<script type="text/javascript">
    /*
       定义函数
      1. 函数前面没有返回类型
      2. 形参前面没有数据类型
      3. 如果函数没有返回值，不需要return，如果有加上return返回值
      4. 函数不调用不执行
     */
    function sum(m, n) {
        alert("函数被调用了");
        return m + n;
    }

    //调用函数
    var result = sum(3,5);
    document.write("结果：" + result + "<br/>");
</script>
</body>
</html>
```



#### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>关于函数的重载和隐藏的数组</title>
</head>
<body>
<script type="text/javascript">
    /**
     * 在JS中没有函数的重载，后面同名的函数会覆盖前面的函数
     * 函数的形参个数与实参个数没有关系
     */
    //定义1个函数
    function sum(a) {
        alert("1个参数");
    }

    function sum(a,b) {
        alert("2个参数");
    }

    function sum(a,b,c) {
        alert("3个参数");
    }

    //调用函数
    sum(2);
</script>
</body>
</html>
```



### 隐藏数组的执行过程

![1571707952136](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP高级/1571707952136.png)



### 案例：在函数内部输出隐藏数组的长度和元素

#### 效果

![1571708212280](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP高级/1571708212280.png) 

#### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>隐藏数组</title>
</head>
<body>
<script type="text/javascript">
    function sum(a, b) {
        var length = arguments.length;   //隐藏数组的长度
        document.write("隐藏数组的长度是：" + length + "<hr/>");

        //隐藏数组
        for (var i = 0; i < arguments.length; i++) {
            document.write("i=" + i + ", 元素值：" + arguments[i] + "<br/>");
        }

        document.write("<hr/>");

        //形参
        document.write("a=" + a + "<br/>");
        document.write("b=" + b + "<br/>");
    }

    //实参个数小于形参的个数
    //sum(1);

    //实参个数大于形参的个数
    sum(4,6,8);
</script>
</body>
</html>
```



### 匿名函数

#### 语法

```javascript
//用于事件的处理，不可重用
元素.on事件=function() {
  代码块
}
```

```javascript
//可以将匿名函数赋值到一个变量中，后期可以通过这个变量名来引用
var 变量名 = function(参数列表) {
  代码块
  [return 返回值];
}

//后期引用
变量名(参数列表)
```



#### 函数调用代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>匿名函数</title>
</head>
<body>
<script type="text/javascript">
    /*
    //可以将匿名函数赋值到一个变量中，后期可以通过这个变量名来引用
    var 变量名 = function(参数列表) {
      代码块
      [return 返回值];
    }

    //后期引用
    变量名(参数列表)
     */
    var sum = function (a, b) {
        alert("调用函数");
        return a + b;
    }

    //调用
    var result = sum(3,5);
    document.write("结果：" + result + "<br/>");
</script>
</body>
</html>
```

### 小结

1. 定义函数的关键字是什么？函数参数和返回值需要指定数据类型吗？

   ```
   function
   不需要
   ```

   

2. 函数是否有重载？

   ```
   没有
   ```

   

3. 每个函数内部都有一个隐藏数组名是：arguments



## 案例：轮播图

### 目标

1. 函数的应用
2. 定时函数的使用

### 方法说明

注：只要是window的方法，可以省略window对象名

| 方法                                | 描述                                                         |
| ----------------------------------- | ------------------------------------------------------------ |
| document.getElementById("id")       | 通过ID获取网页中唯一的元素(标签)                             |
| window.setInterval("函数名()",时间) | 每过一段时间，调用一次指定的函数。时间单位是毫秒             |
| window.setInterval(函数名,时间)     | 两种写法，在字符串中写"函数名()"<br />直接写函数的变量名，没有括号和引号 |

### 需求

每过2秒中切换一张图片的效果，一共5张图片，当显示到最后1张的时候，再次显示第1张。

### 效果

![1551952153233](/assets/1551952153233.png)

### 步骤

1. 创建HTML页面，页面有一个img标签，id为pic，宽度600。
2. body的背景色为黑色，内容居中。
3. 五张图片的名字依次是0~4.jpg，放在项目的img文件夹下，图片一开始的src为第0张图片。
4. 编写函数：changePicture()，使用setInterval()函数，每过3秒调用一次。
5. 定义全局变量：num=1。
6. 在changePicture()方法中，设置图片的src属性为img/num.jpg。
7. 判断num是否等于5，如果等于5，则num=0；否则num++。

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>轮播图</title>
    <style>
        body {
            background-color: black;
            text-align: center;
        }
    </style>
</head>
<body>

<img src="img/0.jpg" width="700" id="pic">

<script type="text/javascript">
    //注：通过id得到元素，在执行这行代码的时候必须要保证元素已经加载了
    var img = document.getElementById("pic");
    var num = 1;   //全局变量

    //替换图，其实就是修改img元素的src属性值
    function changePicture() {
        img.src = "img/" + num + ".jpg";
        num++;
        if (num == 5) {
            num = 0;
        }
    }

    //每过2秒换1张
    //setInterval("changePicture()", 2000);
    setInterval(changePicture, 2000);
</script>

</body>
</html>
```

### 小结

| 方法                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| document.getElementById("id")                                | 通过id得到一个元素<br />注：必须保证在执行这个方法的时候，元素已经加载了。 |
| window.setInterval("函数名()",时间)   <br />window.setInterval(函数名,时间) | 每过一段时间，调用一次函数                                   |



## <font color="red">设置事件的两种方式</font>

### 目标

使用命名函数和匿名函数设置事件

### 什么是事件

当用户在网页上进行各种操作的时候，如：鼠标点击，双击，按下键盘，鼠标移动等。就会激活各种事件，我们可以针对进行事件进行编程，实现与用户的交互功能。

### 设置事件的两种方式

1. 方式一：命名函数      

```html
给按钮设置单击事件，当用户点击此按钮，就会自动调用指定的函数名
<input type="button" onclick="函数名()">
```



2. 方式二：匿名函数 (代码耦合度更低)

```javascript
按钮对象.onclick = function() {
  代码
}
```



### 事件处理案例

#### 需求

有两个按钮，点第1个弹出一个信息框，点第2个弹出另一个信息框。分别使用两种不同的方式激活事件

#### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>事件激活的2种方式</title>
</head>
<body>
<!--命名函数-->
<input type="button" onclick="clickMe()" value="按钮1">

<!--匿名函数-->
<input type="button"  value="按钮2" id="b2">

<script type="text/javascript">
    //命名函数
    function clickMe() {
        alert("我是按钮1");
    }

    //匿名函数：耦合度更低
    document.getElementById("b2").onclick = function () {
        alert("我是按钮2");
    }
</script>
</body>
</html>
```

### 小结

1. 事件处理中命名函数的写法

   ```html
   onclick="clickMe()"
   ```

2. 事件处理中匿名函数的写法

   ```html
   document.getElementById("b2").onclick = function ()
   ```



## 事件介绍：onload、<font color="red">onclick</font>、ondblclick

### 目标

1. 加载完毕事件
2. 鼠标单击
3. 鼠标双击 double

### 加载完成事件

1. 事件名：onload
2. 示例：页面加载完毕以后，才执行相应的JS代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>事件激活的2种方式</title>
</head>
<body>
<script type="text/javascript">
    //命名函数
    function clickMe() {
        alert("我是按钮1");
    }

    //窗口全部加载完毕
    window.onload = function() {
        //匿名函数：耦合度更低
        document.getElementById("b2").onclick = function () {
            alert("我是按钮2");
        }
    }
</script>

<!--命名函数-->
<input type="button" onclick="clickMe()" value="按钮1">

<!--匿名函数-->
<input type="button"  value="按钮2" id="b2">
</body>
</html>
```

#### 以下窗口加载完毕事件哪个是正确的？

```
A. window.onload
B. onload
```



### 鼠标点击事件

1. 单击：onclick
2. 双击：ondblclick

### 案例演示

需求：单击复制文本内容，双击清除文本内容

分析：

1. 复制文本：将t2.value = t1.value
2. 清除文本：将t1和t2的value都设置成空串

![1551952995946](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP高级/1551952995946.png) 

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>单击和双击事件</title>
    <style>
        * {
            margin: 5px;
        }
    </style>
</head>
<body>
<!--name属性给服务器使用的，id属性是给浏览器使用的-->
姓名：<input type="text" name="t1" id="t1"> <br/>
姓名：<input type="text" name="t2" id="t2"> <br/>
<input type="button" value="单击复制/双击清除" id="b1">

<script type="text/javascript">
    //按钮单击事件
     document.getElementById("b1").onclick = function () {
         document.getElementById("t2").value = document.getElementById("t1").value;
     }

     //按钮双击事件
    document.getElementById("b1").ondblclick = function () {
        document.getElementById("t1").value = "";
        document.getElementById("t2").value = "";
    }
</script>
</body>
</html>
```

### 小结

1. 加载完成事件名：onload
2. 单击事件名：onclick
3. 双击事件名：ondblclick



## 事件介绍：鼠标移入和移出

### 目标

1. 鼠标移入
2. 鼠标移出
3. this关键字

### 事件名

1. 移入：onmouseover (over上面)
2. 移出：onmouseout 

### 示例

需求：将鼠标移动到img上显示图片，移出则显示另一张图片。图片设置边框，宽500px

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>鼠标的移上和移出</title>
    <style>
        body {
            background-color: black;
            text-align: center;
        }
    </style>
</head>
<body>
<img src="img/0.jpg" width="700" id="girl">

<script type="text/javascript">
    //鼠标移上
    document.getElementById("girl").onmouseover = function () {
        //修改自己的src属性
        //document.getElementById("girl").src = "img/2.jpg";
        this.src = "img/2.jpg";
    }

    //鼠标移出
    document.getElementById("girl").onmouseout = function () {
        this.src = "img/1.jpg";
    }
</script>
</body>
</html>
```

### this关键字的作用

1. 出现在控件的事件方法中：

```html 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>鼠标的移上和移出</title>
    <style>
        body {
            background-color: black;
            text-align: center;
        }
    </style>
</head>
<body>
<!--this表示img对象-->
<img src="img/0.jpg" width="700" id="girl" onmouseover="changePicture(this)">

<script type="text/javascript">
    /*
    //鼠标移上
    document.getElementById("girl").onmouseover = function () {
        //修改自己的src属性
        //document.getElementById("girl").src = "img/2.jpg";
        this.src = "img/2.jpg";
    }
    */

    //命名函数
    function changePicture(img) {
        img.src = "img/3.jpg";
    }

    //鼠标移出
    document.getElementById("girl").onmouseout = function () {
        this.src = "img/1.jpg";
    }
</script>
</body>
</html>
```

2. 出现在匿名函数的代码中：

```javascript
//this相当于事件激活的对象
document.getElementById("girl").onmouseout = function () {
  this.src = "img/1.jpg";
}
```

### 小结

1. 鼠标移入(上)：onmouseover
2. 鼠标移出：onmouseout



## <font color="red">事件介绍：得到焦点、失去焦点、改变事件</font>

### 目标

1. 得到焦点
2. 失去焦点
3. 改变事件

### 事件名

1. 得到焦点：onfocus
2. 失去焦点：onblur

### 焦点事件示例

当文本框失去焦点时，后面的文字显示，得到焦点时后面的文字消失

#### 分析

在文本框后面有一个span标签，如果有主体内容，显示文字，如果是空串不会显示内容。

```
元素.innerText = "字符串"
<span style="color:red"></span>  ==> <span style="color:red">字符串</span>
```

 ![1551953586472](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP高级/1551953586472.png)

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>得到焦点和失去焦点</title>
</head>
<body>
用户名:
<input type="text" id="user" placeholder="请输入用户名"> <span id="info" style="color: red"></span>
<script type="text/javascript">
    //文本框得到焦点事件
    document.getElementById("user").onfocus = function () {
        document.getElementById("info").innerText = "";
    }
    
    //文本框得到失去焦点事件
    document.getElementById("user").onblur = function () {
        document.getElementById("info").innerText = "用户名不能为空";
    }
</script>
</body>
</html>
```

### 改变事件示例

事件名：onchange，主要用于文本框和下载列表框，其中文本框必须要失去焦点以后才会激活change事件。

#### 需求

1. 选中不同的城市，出现一个信息框显示选中城市的值 
2. 用户输入英文字母以后，文本框的字母全部变成大写

#### 效果

![1551953679058](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP高级/1551953679058.png) 

#### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>改变事件</title>
</head>
<body>
城市：
<select id="city">
    <option value="羊城">广州</option>
    <option value="鹏城">深圳</option>
    <option value="旅游">东莞</option>
</select>
<hr/>
英文：
<input type="text" id="eng">

<script type="text/javascript">
    //选项发生变化，显示当前选中的值
    document.getElementById("city").onchange = function () {
        alert(this.value);
    }

    //文本框改变事件(在失去焦点以后激活)
    document.getElementById("eng").onchange = function () {
        this.value = this.value.toUpperCase();    //将字符串转成大写
    }
</script>
</body>
</html>
```

### 小结

1. 得到焦点事件名：onfocus
2. 失去焦点事件名：onblur
3. 改变事件事件名：onchange



## 内置对象：数组对象

### 目标

1. 数组的创建的方式
2. 数组中的方法

### 创建数组的四种方式

注：JS中数组是一个对象

| 创建数组的方式      | 说明                           |
| ------------------- | ------------------------------ |
| new Array()         | 创建一个长度为0的数组          |
| new Array(5)        | 创建一个长度为5的数组          |
| new Array(1,2,5,20) | 指定数组中每个元素创建一个数组 |
| [4,2,10,6]          | 指定数组中每个元素创建一个数组 |

### 数组的特点

1. 每个元素的数据类型可以不同
2. 数组长度是可以变化的
3. 数组中还有方法

### 方法演示案例

#### 效果

![1571716039003](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP高级/1571716039003.png) 

#### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>数组的特点</title>
</head>
<body>
<script type="text/javascript">
    //1. 数组中每个元素的类型可以不同
    var arr = [5, 2, 10, 6];
    arr[1] = true;
    arr[2] = "abc";
    document.write(arr + "<hr/>");

    //2. 数组的长度可以变化
    arr[5] = 11;
    for (var i = 0; i < arr.length; i++) {
        document.write(arr[i] + "&nbsp;");
    }

    document.write("<hr/>");
    arr.length = 3;   //数组只剩前面3个元素
    document.write(arr + "<hr/>");

    //3. 数组中还包含方法
    //concat() 拼接1个或多个数组
    var a1 = [2, 5, 6];
    var a2 = [3, 10, 7];
    var a3 = a1.concat(a2, 99);
    document.write(a3 + "<br/>");

    //reverse() 数组进行反转
    a3.reverse();
    document.write(a3 + "<hr/>");

    //join(分隔符)：将一个数组通过分隔符拼接成一个字符串，功能与split()相反的
    var str = a3.join("^_^");
    document.write("字符串：" + str + "<hr/>");

    /*
    sort() 排序，默认是按字符串的大小排序，字符串是比较ASCII码值
    1. 大写字母 < 小写字母
    2. 数字 < 字母
    3. 先比较第1个字符，谁大谁就大。如果相同，则比较第2个，谁大谁就大，以此类推。
     */
    document.write(("ac" > "abcdefg") + "<br/>");   //true
    document.write(("x" > "ASLKDJFLASDKFJ") + "<br/>");  //true
    document.write(("28O3U" > "2834328974") + "<hr/>");  //true

    var arr = ["Rose","Jack", "newboy","Tom"];  //Jack, Rose, Tom, newboy
    document.write("排序前：" + arr + "<br/>");
    arr.sort();
    document.write("排序后：" +  arr + "<hr/>");

    //对数字进行排序，默认是按字符串的大小排序
    var arr = [1000, 35, 200, 9, 28];
    document.write("排序前：" + arr + "<br/>");
    arr.sort();
    document.write("排序后：" +  arr + "<hr/>");
    //如果按数字排序，必须指定比较器，类似于Java中Comparator，根据第一个参数小于、等于或大于第二个参数分别返回负整数、零或正整数。
    arr.sort(function (a,b) {
        return a - b;   //升序
    });
    document.write("升序：" +  arr + "<hr/>");
    arr.sort(function (a,b) {
        return b - a;   //降序
    });
    document.write("降序：" +  arr + "<hr/>");

    //pop() 删除数组中最后一个元素，并且返回这个元素
    var arr = [1000, 35, 200, 9, 28];
    document.write("删除前：" + arr + "<br/>");
    var number = arr.pop();
    document.write("被删除的数：" + number + "<br/>");
    document.write("数组：" + arr + "<hr/>");

    //push() 在数组最后添加1个或多个元素
    arr.push(66,88);
    document.write("添加后数组：" + arr + "<br/>");
</script>
</body>
</html>
```

### 小结

| 方法名          | 功能                    |
| --------------- | ----------------------- |
| concat()        | 拼接                    |
| reverse()       | 反转                    |
| join(separator) | 将数组连接成一个字符串  |
| sort()          | 排序                    |
| pop()           | 删除最后一个元素        |
| push()          | 添加1个或多个元素到最后 |



## 使用JS修改CSS的样式

### 目标

JS操作CSS的两种方式

### 在JS中操作CSS属性命名上的区别

| CSS中写法 | JS中的写法 | 说明                         |
| --------- | ---------- | ---------------------------- |
| color     | color      | 一个单词是一样的             |
| font-size | fontSize   | 多个单词样式名使用驼峰命名法 |

### 方式一：一条代码修改一个样式

```
元素.style.样式名=样式值;
```



### 方式二：一条代码修改多个样式

```
元素.className = 类名;
前提：预先定义一个类名，在类名中指定一批样式。
```



### 示例：点按钮，修改p标签的字体、颜色、大小

### 效果

![1552049542846](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP高级/1552049542846.png) 

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>使用JS修改CSS样式</title>
    <style>
        .boy {
            font-size: 35px;
            color: blue;
            font-family: 隶书;
        }
    </style>
</head>
<body>
<p id="p1">我是女生</p>
<p id="p2">我是男生</p>
<hr/>
<input type="button" value="改女生" id="b1">
<input type="button" value="改男生" id="b2">

<script type="text/javascript">
    //使用方式一修改样式
    document.getElementById("b1").onclick = function () {
        //每句话改一个样式
        var p1 = document.getElementById("p1");
        p1.style.fontSize = "35px";
        p1.style.color = "red";
        p1.style.fontFamily = "楷体";
    }

    //使用方式二：修改类样式
    document.getElementById("b2").onclick = function () {
        //先设置一个类名
        var p2 = document.getElementById("p2");
        p2.className = "boy";
    }
</script>
</body>
</html>
```

### 网页变化

![1571716664605](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP高级/1571716664605.png) 

### 小结

JS修改CSS样式的两种方式

1. 元素.style.样式名=样式值
2. 元素.className=类名



## 上午的回顾

### 命名函数

```javascript
function 函数名(参数列表) {
   代码块
   [return 返回值];
}
```



### 匿名函数

```javascript
var 变量名=function() {
  代码块
  [return 返回值];
}
```

```javascript
//事件处理函数
元素.on事件名=function() {

}
```



### 事件

1. onload 加载完成
2. onclick 单击
3. ondblclick 双击
4. onmouseover 鼠标移上
5. onmouseout 鼠标移出
6. onfcous 得到焦点
7. onblur 失去焦点
8. onchange 改变事件

### 数组

1. 数组中元素数据类型可以不同
2. 数组的长度可以动态变化
3. 数据中还有一些方法：
   1. concat() 拼接数组
   2. reverse() 反转
   3. join() 将数组拼合并成一个字符串
   4. sort() 排序，默认是按字符串。如果要对数字进行排序，需要指定比较器。
   5. pop() 删除数组中最后一个元素
   6. push() 添加1个或多个元素到数组后面

### 轮播图

```
document.getElementById() 通过id得到一个唯一的元素
window.setInterval("函数名()", 毫秒) 每隔一段时间执行1次指定的函数
window.setInterval(函数名, 毫秒)
```



### 在JS中修改CSS样式

```
元素.style.样式名=样式值
元素.className=类名
```





## 案例：鼠标移动到文字上显示提示文字

### 目标

![1552049859046](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP高级/1552049859046.png) 

### 分析

1. 一开始div不可见
2. 当鼠标移到a标签，使用onmouseover，让div可见。
3. 当鼠标移出a标签，使用onmouseout事件，让div不可见。

### 技术点

| display样式 | 说明           |
| ----------- | -------------- |
| none        | 元素不可见     |
| block       | 以块级元素显示 |
| inline      | 以内联元素显示 |

### 步骤

1)	网页上有一个a标签链接，a标签下有一个不可见的div。
2)	鼠标移到a标签，设置div的样式，让div可见
3)	鼠标移出a标签，设置div的样式，让div不可见。

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>显示信息</title>
    <style>
        #info {
            border: dashed black 1px;
            font-size: 14px;
            width: 170px;
            height: 20px;

            text-align: center;
            /*如果行高和高度相同，则是垂直居中*/
            line-height: 20px;

            border-radius: 5px;
            background-color: lightyellow;
            /*隐藏元素*/
            display: none;
        }

        a:link {
            text-decoration: none;
        }

        a:hover {
            color: red;
        }
    </style>
</head>
<body>
<a href="#" id="link">点我试试</a>
<div id="info">
    点我可以看到精彩内容哦
</div>

<script type="text/javascript">
    //得到a标签
    document.getElementById("link").onmouseover = function () {
        //让div可见
        document.getElementById("info").style.display = "block";
    }

    document.getElementById("link").onmouseout = function () {
        document.getElementById("info").style.display = "none";
    }
</script>
</body>
</html>
```

### 小结

隐藏元素使用什么样式？

```
display="none","block","inline"
```



## 内置对象：日期对象

### 目标

日期对象方法的使用

### 创建日期对象

```
var 变量名 = new Date();
创建现在的时间，得到浏览器端的时间
```

### 日期对象的方法

![1552052803821](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP高级/1552052803821.png)

### 方法效果

![1571728073788](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP高级/1571728073788.png) 

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>日期对象</title>
</head>
<body>
    <script type="text/javascript">
        //创建日期对象
        var date = new Date();
        document.write(date + "<br/>");
        document.write("年份：" + date.getFullYear() + "<br/>");
        document.write("月份：" + date.getMonth() + "<br/>");
        document.write("第几天：" + date.getDate() + "<br/>");
        document.write("周几：" + date.getDay() + "<br/>");
        //类似于Java中System.currentTimeMillis()
        document.write("1970-1-1到现在相差的毫秒数：" + date.getTime() + "<br/>");
        //得到本地化的时间字符串
        document.write(date.toLocaleString() + "<br/>");
    </script>
</body>
</html>
```

### 小结

说说下面方法的作用

1. getFullYear() 得到年份
2. getDay() 周几
3. getTime() 得到1970-1-1到现在的毫秒数
4. toLocaleString() 得到本地化的日期格式



## BOM：location对象

### 目标

1. 什么是BOM编程
2. location对象的方法和属性

### 什么是BOM

Browser Object Model 浏览器对象模型，用来操作浏览器中各种对象。

![1552052941943](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP高级/1552052941943.png)

### BOM对象

| BOM常用对象 | 作用                                                     |
| ----------- | -------------------------------------------------------- |
| window      | 代表浏览器窗口对象<br />prompt(), alert(), setInterval() |
| location    | 地址栏对象                                               |
| history     | 浏览器的历史记录对象                                     |

### location对象

作用：代表一个地址栏对象

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>location地址栏对象</title>
</head>
<body>
<script type="text/javascript">
    /*
    属性：href
        1. 获取：得到当前访问的地址字符串
        2. 设置：可以进行页面的跳转
        var url = location.href;
        document.write("当前访问的地址是：" + url + "<br/>");
        //可以进行页面的跳转
        alert("注意，我要跳转了!");
        location.href = "http://www.itcast.cn";
     */

    /*
    属性：search
        获取：地址?后面的参数字符串
        var param = location.search;
        document.write("查询字符串：" + param + "<br/>");
     */

    /*
    属性：hash
        获取：# 锚点 的字符串
     */
    var hash = location.hash;
    document.write("锚点：" + hash + "<br/>");
</script>
</body>
</html>
```

细节：

1. 面试题：以下进行页面跳转哪些写法是正确的？

   ```
   A. location.href = "url";
   B. location = "url";
   C. window.location.href = "url";
   D. window.location = "url";
   ```

   ```
   如果给location赋值，默认是给href属性
   location对象其实是window的一个属性
   ```

### location常用方法

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>location的方法</title>
</head>
<body>
<input type="button" value="刷新" onclick="location.reload()">
<input type="button" value="跳转" onclick="location.assign('demo12_css.html')">
<input type="button" value="代替" onclick="location.replace('demo12_css.html')">
<script type="text/javascript">
    /*
    reload()：重新加载当前页面，相当于浏览器上刷新按钮，用在需要刷新页面场景。
     */
    document.write(new Date().toLocaleString() + "<br/>");

    /*
    assign() 功能与location.href是一样的，跳转到另一个页面。记录在历史记录中，可以点后退回来。
    replace() 也可以进行页面跳转，但不会记录在历史记录中。
     */
</script>
</body>
</html>
```

### 小结

1. location学习了哪些属性?

   1. <font color="red">href：跳转地址</font>
   2. search：得到?查询字符串
   3. hash: 得到锚点

2. 学习了哪些方法

   1. reload()：刷新
   2. assign()：跳转，历史记录有会
   3. replace()：跳转，历史记录中没有

3. 它与window是什么关系?

   它是window的一个属性



## BOM：history对象

### 目标

history对象有哪些方法和属性

### 作用

| 方法      | 作用                                                         |
| --------- | ------------------------------------------------------------ |
| forward() | 前进，相当于浏览器上前进按钮。如果这个前进按钮不可用，这个方法也是失效的。 |
| back()    | 后退，相当于浏览器上后退按钮。                               |
| go(数字)  | 相当于前进+后退。<br />如果参数是1，表示前进1个页，如果是-1表示后退一个页面。 |

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>历史对象</title>
</head>
<body>
<a href="demo14_date.html">显示时间</a> <hr/>
<input type="button" value="前进" onclick="history.forward()">
<input type="button" value="前进" onclick="history.go(1)">
</body>
</html>
```

```html
<input type="button" value="后退" onclick="history.back()">
<input type="button" value="后退" onclick="history.go(-1)">
<hr/>
```

### 小结

| 方法      | 作用 |
| --------- | ---- |
| forward() | 前进 |
| back()    | 后退 |



## 案例：倒计时页面跳转

### 目标

window对象与location对象的综合案例应用

### window中计时器有关的方法

| window中的方法                  | 作用                                                         |
| ------------------------------- | ------------------------------------------------------------ |
| setTimeout(函数名, 间隔毫秒数)  | 隔一段时间执行一次指定的函数，执行1次就结束了                |
| clearTimeout(计时器)            | 清除计时器，让上面的函数调用不再起作用                       |
| setInterval(函数名, 间隔毫秒数) | 每隔一段时间执行一次指定的函数，不断的执行<br />返回值：计时器 |
| clearInterval(计时器)           | 清除计时器，让上面的函数调用不再起作用                       |

### 需求

页面上显示一个倒计时5秒的数字，到了5秒以后跳转到另一个页面

### 效果

![1552053355566](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP高级/1552053355566.png) 

### 分析

1. 在页面上创建一个span用于放置变化的数字。
2. 定义一个全局变量为5，每过1秒调用1次自定义refresh()函数
3. 编写refresh()函数，修改span中的数字
4. 判断变量是否为0，如果是0则跳转到新的页面
5. 否则变量减1。并修改span中的数字

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>window对象与location对象的综合案例应用</title>
</head>
<body>
<h1><span id="num">5</span>秒后回到主页</h1>

<script type="text/javascript">
    var num = 5;
    //每过1秒调用一次指定的函数
    window.setInterval(function () {
         num--;
         //修改span中值
        document.getElementById("num").innerText = num;
        //判断是否为0
        if (num == 0) {
            location.href = "http://www.itcast.cn";
        }
    }, 1000);
</script>
</body>
</html>
```

### 小结

1. 每过一段时间执行的方法是：setInterval()
2. 跳转到另一个页面使用：location.href
3. 更新数字显示使用：innerText



## 案例：会动的时钟

### 目标

​	页面上有两个按钮，一个开始按钮，一个暂停按钮。点开始按钮时间开始走动，点暂停按钮，时间不动。再点开始按钮，时间继续走动。

![1552053500102](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP高级/1552053500102.png)

### 步骤

1.	在页面上创建一个h1标签，用于显示时钟，设置颜色和大小。
2.	一开始暂停按钮不可用，设置disabled属性，disabled=true表示不可用。
3.	创建全局的变量，用于保存计时器
4.	为了防止多次点开始按钮出现bug，点开始按钮以后开始按钮不可用，暂停按钮可用。点暂停按钮以后，暂停按钮不可用，开始按钮可用。设置disabled=true。
5.	点开始按钮，在方法内部每过1秒中调用匿名函数，在匿名函数内部得到现在的时间，并将得到的时间显示在h1标签内部。
6.	暂停的按钮在方法内部清除clearInterval()的计时器。

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>会动的时钟</title>
</head>
<body>
<input type="button" value="开始" id="btnStart">
<!--按钮不可用-->
<input type="button" value="暂停" id="btnPause" disabled="disabled">
<hr/>
<h1 id="clock" style="color: darkgreen"></h1>

<script type="text/javascript">
    //页面加载完毕显示当前的时间
    window.onload = function () {
        document.getElementById("clock").innerText = new Date().toLocaleString();
    }

    var timer;  //计时器

    //开始按钮
    document.getElementById("btnStart").onclick = function () {
        /*
        开始按钮不可用: 在JS代码中disabled=true 表示按钮不可用，false表示可用
         */
        document.getElementById("btnStart").disabled = true;
        //暂停按钮可用
        document.getElementById("btnPause").disabled = false;
        //每过1秒中将现在的时间显示在h1中
        timer = setInterval(function () {
            document.getElementById("clock").innerText = new Date().toLocaleString();
        },1000);
    }

    //暂停按钮
    document.getElementById("btnPause").onclick = function () {
        document.getElementById("btnStart").disabled = false;  //可用
        document.getElementById("btnPause").disabled = true;  //不可用
        //清除计时器
        clearInterval(timer);
    }
</script>

</body>
</html>
```

### 小结

1. 清除计时器的方法：clearInterval()
2. 让元素不可用的属性是：disabled=true



## <font color="red">innerHTML和innerText的区别</font>

### 目标

学习innerHTML和innerText的区别

### 语法

| 元素的属性 | 作用                                                         |
| ---------- | ------------------------------------------------------------ |
| innerHTML  | 获得：标签内部的HTML，包含了子标签<br />设置：修改标签内部的HTML，HTML标签是起使用 |
| innerText  | 获得：标签内部的纯文本，不包含任何的标签<br />设置：设置为纯文本，HTML标签是不起使用的。 |



### 案例效果

![1552053767093](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP高级/1552053767093.png)

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>innerHTML和innerText</title>
</head>
<body>
<div id="myDiv">
    <span>
        <a href="#">我是链接</a>
    </span>
</div>
<hr/>
<!--innerHTML和innerText区别-->
<input type="button" id="b1" value="得到HTML">
<input type="button" id="b2" value="得到Text">
<input type="button" id="b3" value="设置HTML">
<input type="button" id="b4" value="设置Text">

<script type="text/javascript">
    //得到HTML
    document.getElementById("b1").onclick = function () {
        alert(document.getElementById("myDiv").innerHTML);
    }

    //得到Text
    document.getElementById("b2").onclick = function () {
        alert(document.getElementById("myDiv").innerText);
    }

    //设置HTML
    document.getElementById("b3").onclick = function () {
        document.getElementById("myDiv").innerHTML = "<h1 style='color: red'>Good good Study, Day Day Up! </h1>";
    }

    //设置Text
    document.getElementById("b4").onclick = function () {
        document.getElementById("myDiv").innerText = "<h1 style='color: red'>Good good Study, Day Day Up! </h1>";
    }
</script>
</body>
</html>
```

### 小结

| 元素的属性 | 作用                 |
| ---------- | -------------------- |
| innerHTML  | 标签起作用           |
| innerText  | 标签不起作用，纯文本 |



## window中的三个对话框方法

### 目标

有哪些弹出的对话框

### 语法

![1552053909245](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP高级/1552053909245.png)

### confirm案例演示

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>确认框</title>
</head>
<body>
<script type="text/javascript">
    //如果点确定，返回true，否则返回false
    var flag = window.confirm("真的要删除吗?");
    if (flag) {
        alert("删除成功");
    } 
</script>
</body>
</html>
```

### 小结

| window中的方法                       | 说明                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| setInterval()<br />setTimeout()      | 每隔一段时间调用一次指定的函数<br />隔一段时间调用一次函数，只调用一次 |
| clearTimeout() <br />clearInterval() | 清除上面的计时器                                             |
| alert()<br />prompt()<br />confirm() | 信息提示框<br />输入框<br />确认框                           |



## <font color="red">DOM查找元素的四个方法</font>

### 目标

1. 什么是DOM编程
2. 学习通过DOM查找元素的方法

### 节点与DOM树

Document Object Model 文档对象模型

只要操作DOM树，就可以修改网页的内容

![1552054005396](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP高级/1552054005396.png)

### 查找节点的方法

| 获取元素的方法                             | 作用                     |
| ------------------------------------------ | ------------------------ |
| document.getElementById("id")              | 通过id得到唯一元素       |
| document.getElementsByTagName   ("标签名") | 通过标签名字得到一组元素 |
| document.getElementsByName("name")         | 通过name属性得到一组元素 |
| document.getElementsByClassName("类名")    | 通过类名得到一组元素     |

### 案例：查找节点

#### 步骤

1. 点第1个按钮，给所有tr奇数行和偶数行添加不同的背景色
2. 点击第2个按钮，选中所有商品
3. 点击第3个按钮给所有的a链接添加href属性。

#### 效果

![1568450069477](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP高级/1568450069477.png)

#### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>查询DOM树上节点</title>
    <style>
        table {
            width: 400px;
            /*细边框样式*/
            border-collapse: collapse;
        }
    </style>
</head>
<body>
<input type="button" value="(通过标签名)给奇数行和偶数行添加背景色" id="b1"/>
<hr/>
<table border="1">
    <tr>
        <td>100</td>
        <td>100</td>
        <td>100</td>
    </tr>
    <tr>
        <td>100</td>
        <td>100</td>
        <td>100</td>
    </tr>
    <tr>
        <td>100</td>
        <td>100</td>
        <td>100</td>
    </tr>
    <tr>
        <td>100</td>
        <td>100</td>
        <td>100</td>
    </tr>
    <tr>
        <td>100</td>
        <td>100</td>
        <td>100</td>
    </tr>
    <tr>
        <td>100</td>
        <td>100</td>
        <td>100</td>
    </tr>
</table>
<hr/>

<input type="button" value="(通过name属性)选中所有的商品" id="b2"/>
<hr/>
<!--设置checked = true表示选中-->
<input type="checkbox" name="product"> 自行车
<input type="checkbox" name="product"> 电视机
<input type="checkbox" name="product"> 洗衣机
<hr/>

<input type="button" value="(通过类名)给a添加链接" id="b3"/>
<hr/>
<a class="company">传智播客</a>
<a class="company">传智播客</a>
<a class="company">传智播客</a>

<script type="text/javascript">
    //(通过标签名)给奇数行和偶数行添加背景色
    document.getElementById("b1").onclick = function () {
        var trs = document.getElementsByTagName("tr");  //得到整个表格中所有的tr
        for (var i = 0; i < trs.length; i++) {
            var tr = trs[i];   //表示其中一行
            if (i % 2 == 0) {  //偶数
                tr.style.backgroundColor = "green";
            }
            else {  //奇数
                tr.style.backgroundColor = "yellow";
            }
        }
    }

    //(通过name属性)选中所有的商品
    document.getElementById("b2").onclick = function () {
        var products = document.getElementsByName("product");
        for (var i = 0; i < products.length; i++) {
            var product = products[i];
            //设置checked = true表示选中
            product.checked = true;
        }
    }

    //(通过类名)给a添加链接
    document.getElementById("b3").onclick = function () {
        var companies = document.getElementsByClassName("company");
        for (var i = 0; i < companies.length; i++) {
            var company = companies[i];
            company.href = "http://www.itcast.cn";
        }
    }
</script>
</body>
</html>
```

### 小结

| 获取元素的方法                           | 作用                     |
| ---------------------------------------- | ------------------------ |
| document.getElementsByTagName ("标签名") | 通过标签名得到一组元素   |
| document.getElementsByName("name")       | 通过name属性得到一组元素 |
| document.getElementsByClassName("类名")  | 通过类名得到一组元素     |



## DOM树修改的方法

### 目标

1. DOM树上如何创建节点
2. DOM树上如何添加元素节点
3. 删除DOM树上的元素

### 添加元素的步骤

1. 创建这个元素
2. 将元素挂到DOM树上

### 创建元素的方法

| 创建元素的方法                   | 作用                           |
| -------------------------------- | ------------------------------ |
| document.createElement("标签名") | 创建一个元素，参数是标签的名字 |

### 修改DOM树的方法

| 将元素挂到DOM树上的方法    | 作用                                 |
| -------------------------- | ------------------------------------ |
| 父元素.appendChild(子元素) | 将子元素添加成父元素的最后一个子元素 |
| 父元素.removeChild(子元素) | 父元素删除指定的子元素               |
| 元素.remove()              | 删除自己                             |

### 通过关系找节点

| 遍历节点的属性  | 说明                     |
| --------------- | ------------------------ |
| childNodes      | 得到当前元素的所有子节点 |
| firstChild      | 得到第一个子节点         |
| lastChild       | 得到最后一个子节点       |
| parentNode      | 得到当前元素的父节点     |
| nextSibling     | 得到下一个兄弟节点       |
| previousSibling | 得到上一个兄弟节点       |

### 小结

1. 创建元素：createElement()
2. 添加成最后一个子元素：appendChild()
3. 删除子元素：removeChild()
4. 删除本身： remove()



## 案例：学生信息管理添加

### 目标

1.	当添加按钮“添加一行数据”时，文本框中的数据添加到表格中且文本框置空； 
2.	当点击表格中的“删除”时，该行数据被删除，删除前确认
3.	当点击表格第一行的复选框的时候，下面每一行都选中。当取消的时候，下面每一行都取消。

### 效果

![1552054419916](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP高级/1552054419916.png)

### 步骤

1. 添加的实现：当点击按钮时，得到文本框中的文本，如果文本框为空，则提示错误。
2. 创建tr节点；把整个tr中的内容使用innerHTML直接设置到tr的内部。
3. 把tr追加到tbody元素中；注：tbody无论源代码中是否写了，在浏览器中始终存在。
4. 添加一行之后，清空学号与姓名的文本框内容
5. 删除操作：将当前点击按钮所在的一行tr，从tbody中删除。
6. 如何得到点击按钮所在的行? 先得到按钮的父节点td，td的父节点tr，然后删除。
7. 点击上面的复选框，则下面所有复选框的选择状态和上面的一样。

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>学生信息管理</title>
    <style>
        table {
            border-collapse: collapse;
            width: 400px;
        }

        tr {
            text-align: center;
        }

        td, th {
            padding: 5px;
        }

        th {
            background-color: lightgrey;
        }
    </style>
</head>
<body>
<table border="1">
    <tr>
        <th>
            <input type="checkbox" id="all" onclick="selectAll()">
        </th>
        <th>
            学号
        </th>
        <th>
            姓名
        </th>
        <th>
            操作
        </th>
    </tr>

    <tr>
        <td><input type="checkbox" name="item"></td>
        <td>000001</td>
        <td>潘金莲</td>
        <td>
            <input type="button" value="删除" onclick="deleteRow(this)">
        </td>
    </tr>

    <tr>
        <td><input type="checkbox" name="item"></td>
        <td>000002</td>
        <td>鲁智深</td>
        <td>
            <input type="button" value="删除" onclick="deleteRow(this)">
        </td>
    </tr>
</table>
<hr/>
学号：
<input type="text" id="no">
姓名：
<input type="text" id="name">
<input type="button" id="add" value="添加" onclick="addRow()">
</body>

<script type="text/javascript">
    //添加1行
    function addRow() {
        //1.得到学号和姓名
        var no = document.getElementById("no").value;
        var name = document.getElementById("name").value;
        if (no == "" || name == "") {
            alert("学号或姓名不能为空");
            return;
        }
        //2.创建tr
        var tr = document.createElement("tr");
        //3. 在tr中添加td
        tr.innerHTML = "<td><input type=\"checkbox\" name=\"item\"></td>" +
            "<td>" + no + "</td>" +
            "<td>" + name + "</td>" +
            "<td><input type=\"button\" value=\"删除\" onclick=\"deleteRow(this)\"></td>";
        //4. 把tr添加到tbody中
        var tbody = document.getElementsByTagName("tbody")[0];   //得到第0个元素
        tbody.appendChild(tr);
        //清空文本框
        document.getElementById("no").value = "";
        document.getElementById("name").value = "";
    }

    //删除按钮的父元素的父元素(自杀)
    function deleteRow(btn) {  //参数是按钮
        //按钮 -> td -> tr
        if (confirm("真的要删除这个学生吗?")) {
            btn.parentElement.parentElement.remove();
        }
    }

    //全选全不选
    function selectAll() {
        //通过名字选中下面所有的复选框
        var items = document.getElementsByName("item");
        for (var i = 0; i < items.length; i++) {
            var item = items[i];  //每个复选框
            item.checked = document.getElementById("all").checked;   //true/false
        }
    }
</script>
</html>
```



### 小结

修改元素内部的HTML，使用哪个属性？innerHTML

得到当前元素的父元素使用哪个属性？ parentNode(文本，标签，属性), parentElement(标签)



# 学习总结

1. 函数定义的两种格式

|          | 函数定义的格式                             |
| -------- | ------------------------------------------ |
| 命名函数 | function 函数名(参数列表) { return 返回值} |
| 匿名函数 | 元素.on事件 = function() {}                |



2. 能够使用JS中常用的事件

| 事件名      | 作用     |
| ----------- | -------- |
| onclick     | 单击     |
| ondblclick  | 双击     |
| onload      | 加载     |
| onfocus     | 得到焦点 |
| onblur      | 失去焦点 |
| onchange    | 改变     |
| onmouseover | 鼠标移上 |
| onmouseout  | 鼠标移出 |

3. 能够使用数组中常用的方法

| 方法名          | 功能              |
| --------------- | ----------------- |
| concat()        | 拼接              |
| reverse()       | 反转              |
| join(separator) | 拼接字符          |
| sort()          | 排序              |
| pop()           | 删除最后一个元素  |
| push()          | 添加1个或多个元素 |

4. 能够使用日期对象常用的方法

| 方法名           | 作用     |
| ---------------- | -------- |
| getFullYear()    | 年份     |
| getDay()         | 周几     |
| getTime()        | 毫秒     |
| toLocaleString() | 本地日期 |

5. 能够使用window对象常用的方法

| window中的方法                   | 说明                   |
| -------------------------------- | ---------------------- |
| setInterval()/setTimeout()       | 每隔一段时间执行1次    |
| clearTimeout() / clearInterval() | 删除计时器             |
| alert()/prompt()/confirm()       | 信息框，输入框，确认框 |

6. 能够使用location对象常用的方法和属性

| 属性   | 功能       |
| ------ | ---------- |
| href   | 跳转       |
| search | ? 后面参数 |

| location的方法 | 描述     |
| -------------- | -------- |
| reload()       | 刷新页面 |

7. 能够使用history对象常用的方法

| 方法      | 作用 |
| --------- | ---- |
| forward() | 前进 |
| back()    | 后退 |

8. 能够使用DOM中来查找节点

| 获取元素的方法                           | 作用                     |
| ---------------------------------------- | ------------------------ |
| document.getElementById("id")            | 通过id得到一个元素       |
| document.getElementsByTagName ("标签名") | 通过标签名得到一组元素   |
| document.getElementsByName("name")       | 通过name属性得到一组元素 |
| document.getElementsByClassName("类名")  | 通过类名得到一组元素     |

9. 能够使用DOM来增删改节点

| 创建元素的方法                   | 作用     |
| -------------------------------- | -------- |
| document.createElement("标签名") | 创建元素 |

| 将元素挂到DOM树上的方法    | 作用                 |
| -------------------------- | -------------------- |
| 父元素.appendChild(子元素) | 添加到最后一个子元素 |
| 父元素.removeChild(子元素) | 删除子元素           |
| 元素.remove()              | 删除自己             |

10. 能够使用JavaScript对CSS样式进行操作

```html
元素.style.样式名=样式值
元素.className=类名
```

