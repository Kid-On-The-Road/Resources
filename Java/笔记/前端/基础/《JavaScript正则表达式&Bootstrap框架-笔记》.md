# 《JavaScript正则表达式&Bootstrap框架-笔记》

# 回顾

1. 能够使用JS中常用的事件

   | 事件名      | 作用     |
   | ----------- | -------- |
   | onclick     | 单击     |
   | ondblclick  | 双击     |
   | onload      | 加载完成 |
   | onfocus     | 得到焦点 |
   | onblur      | 失去焦点 |
   | onchange    | 改变     |
   | onmouseover | 鼠标移入 |
   | onmouseout  | 鼠标移出 |

2. 能够使用数组中常用的方法

   | 方法名          | 功能                          |
   | --------------- | ----------------------------- |
   | concat()        | 数组拼接                      |
   | reverse()       | 反转                          |
   | join(separator) | 数组拼接成字符串              |
   | sort()          | 排序，默认是按字符串的ASCII码 |
   | pop()           | 删除最后一个元素              |
   | push()          | 添加1个或多个元素             |

3. 能够使用日期对象常用的方法

| 方法名           | 作用                   |
| ---------------- | ---------------------- |
| getFullYear()    | 得到年份               |
| getDay()         | 星期几                 |
| getTime()        | 1970-1-1到现在的毫秒数 |
| toLocaleString() | 得到本地化的日期和时间 |

4. 能够使用window对象常用的方法

| window中的方法                   | 说明                                    |
| -------------------------------- | --------------------------------------- |
| setInterval()/setTimeout()       | 每隔一段时间执行1次函数，第2个只执行1次 |
| clearTimeout() / clearInterval() | 清除上面的计时器                        |
| alert()/prompt()/confirm()       | 信息框，输入框，确认框                  |

5. 能够使用location对象常用的方法和属性

| 属性   | 功能                           |
| ------ | ------------------------------ |
| href   | 设置值：相当于跳转到另一个页面 |
| search | 得到?后面的参数值              |

| location的方法 | 描述     |
| -------------- | -------- |
| reload()       | 刷新页面 |

6. 能够使用history对象常用的方法

| 方法      | 作用 |
| --------- | ---- |
| forward() | 前进 |
| back()    | 后退 |

7. 能够使用DOM中来查找节点

| 获取元素的方法                           | 作用                       |
| ---------------------------------------- | -------------------------- |
| document.getElementById("id")            | 通过id得到一个元素         |
| document.getElementsByTagName ("标签名") | 通过标签的名字得到一组元素 |
| document.getElementsByName("name")       | 通过name属性得到一组元素   |
| document.getElementsByClassName("类名")  | 通过类名得到一组元素       |

8. 能够使用DOM来增删改节点

| 创建元素的方法                   | 作用         |
| -------------------------------- | ------------ |
| document.createElement("标签名") | 创建一个元素 |

| 将元素挂到DOM树上的方法    | 作用                 |
| -------------------------- | -------------------- |
| 父元素.appendChild(子元素) | 添加成最后一个子元素 |
| 父元素.removeChild(子元素) | 删除子元素           |
| 元素.remove()              | 删除自己             |

9. 能够使用JavaScript对CSS样式进行操作

```html
元素.style.样式名=样式值
元素.className=类名
```



# 学习目标

1. 能够使用正则表达式进行表单的验证
2. Bootstrap框架
   1. 能够创建bootstrap的模板
   2. 能够使用bootstrap的两种布局容器
   3. 能够理解bootstrap的响应式布局的特点
   4. 能够查询文档创建bootstrap的按钮、表格、表单等常用组件
   5. 能够理解bootstrap的栅格系统
   6. 能够查询文档使用bootstrap的导航条
   7. 能够查询文档使用bootstrap的轮播图
3. 能够利用Bootstrap完成黑马旅游的首页



# 学习内容

## 正则表达式的规则

### 目标

了解正则表达式的规则

### 引入

提问：如果要验证一个手机号码，我们以前应该如何写代码？

![1552051283134](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552051283134.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>验证手机号码</title>
</head>
<body>
手机号：
<input type="text" id="tel"/>
<input type="button" id="btn" value="验证"/>
<script type="text/javascript">
    /*
    如果验证通过，返回true，否则返回false。
     */
    function checkPhone() {
        /*
        1. 必须全是数字
        2. 必须是11位
        3. 以1开头，第2个数字不能是012，第3个以后的数字任意
         */
        //1. 得到文本框输入的值
        var tel = document.getElementById("tel").value.trim();
        //2. 使用上面的规则进行判断
        if (isNaN(tel)) {
            return false;
        }
        if (tel.length != 11) {
            return false;
        }
        if (tel.charAt(0) != '1') {
            return false;
        }
        if (tel.charAt(1) == '0' || tel.charAt(1) == '1' || tel.charAt(1) == '2') {
            return false;
        }
        return true;
    }

    document.getElementById("btn").onclick = function () {
        if (checkPhone()) {
            alert("验证通过");
        }
        else {
            alert("验证失败");
        }
    };
</script>
</body>
</html>
```



### 正则表达式作用

1. 简化字符串的验证，判断一个字符串是否匹配指定的规则
2. 查询和搜索字符串的作用



### 规则[了解]

| 符号   | 作用                                                     |
| ------ | -------------------------------------------------------- |
| [a-z]  | []表示1个字符，-表示一个范围。匹配所有的小写字母         |
| [xyz]  | 匹配x或y或z                                              |
| [^xyz] | 在中括号中加上^的符号表示取反，匹配除了xyz之外的任意字符 |
| \d     | 表示数字                                                 |
| \w     | 表示单词，相当于这些字符：[a-zA-Z0-9_]                   |
| .      | 匹配任意的字符，通配符。如果要匹配点号，必须要转义：\\.  |
| ()     | 代表一组，表示后面的操作符对这一组进行操作               |
| {n}    | 前面的字符出现n次   n==x                                 |
| {n,}   | 前面的字符出现大于等于n次  n<=x                          |
| {n,m}  | 前面的字符出现n到m之间的次数，包头包尾  n<=x<=m          |
| +      | 前面的字符出现1到n次                                     |
| *      | 前面的字符出现0到n次                                     |
| ?      | 前面的字符出现0到1次                                     |
| \|     | 或者，几个字符串选择1个                                  |
| ^      | 在最前面表示匹配开头                                     |
| $      | 在最后面表示匹配结束                                     |

### 举例

| 正则表达式 | 匹配字符串                                    |
| ---------- | --------------------------------------------- |
| \d{3}      | 匹配3个数字，在JS中模糊匹配，在Java中精确匹配 |
| ^\d{3}     | 匹配以3个数字开头的字符串                     |
| \d{3}$     | 匹配以3个数字结尾的字符串                     |
| ^\d{3}$    | 精确匹配3个数字                               |
| ab{2}      | 匹配abb                                       |
| ab{2,}     | 匹配abb, abbb, abbbbbbbbbbbbbbbb              |
| ab{3,5}    | abbb, abbbb,abbbbb                            |
| ab+        | ab, abbbbb                                    |
| ab\*       | a,  ab,  abbbbbbbbbb                          |
| ab?        | a, ab                                         |
| hi\|hello  | hi或hello中一个                               |
| (b\|cd)ef  | 匹配bef或cdef                                 |
| ^.{3}$     | 匹配任意的3个字符                             |
| \[^a-zA-Z] | 除了字母之外的字符                            |

### 使用正则表达式的实现

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>验证手机号码</title>
</head>
<body>
手机号：
<input type="text" id="tel"/>
<input type="button" id="btn" value="验证"/>
<script type="text/javascript">
    /*
    如果验证通过，返回true，否则返回false。
     */
    function checkPhone() {
        /*
        1. 必须全是数字
        2. 必须是11位
        3. 以1开头，第2个数字不能是012，第3个以后的数字任意
         */
        //1. 得到文本框输入的值
        var tel = document.getElementById("tel").value.trim();
        //调用正则表达式的方法，如果字符串匹配正则表达式就返回true，否则就返回false
        return /^1[3456789]\d{9}$/.test(tel);
    }

    document.getElementById("btn").onclick = function () {
        if (checkPhone()) {
            alert("验证通过");
        } else {
            alert("验证失败");
        }
    };
</script>
</body>
</html>
```



### 小结

正则表达式主要有哪两个作用？

1. 判断字符串是否匹配某个规则
2. 查询字符串



## <font color="red">创建正则表达式</font>

### 目标

创建正则表达式有哪2种方式

### 方式1

```
//正则表达式是一个内置对象
new RegExp("正则表达式")
```

### 方式2

```
/正则表达式/
```

### 演示代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>正则表达式创建</title>
</head>
<body>
<script type="text/javascript">
    //字符串，在JS中默认是模糊匹配
    var str = "1235";
    //方式一：正则表达式，找3个数字
    //var reg = new RegExp("^\\d{3}$");

    //方式二：
    var reg = /^\d{4}$/;
    //test方法用来判断是否匹配，匹配就返回true
    document.write("是否匹配：" + reg.test(str) + "<br/>");
</script>
</body>
</html>
```

### 两种方式的区别

1. 方式一：正则表达式是做为字符串参数出现的，可以定义成变量，可以进行字符串拼接，相对更加灵活。但\等字符需要转义。
2. 方式二：本身是一个正则表达式对象，\等字符不需要转义。

### 匹配模式

```
i 忽略大小写比较
```

创建模式的2种写法：

```javascript
new RegExp("正则表达式","匹配模式")

/正则表达式/匹配模式
```



### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>正则表达式匹配模式</title>
</head>
<body>
<script type="text/javascript">
    //创建字符串
    var str = "CAT";
    //创建正则表达式并且指定匹配模式 (Regular Expression 正则表达式)
    //var reg = new RegExp("cat","i");  //忽略大小写

    var reg = /cat/i;  //匹配模式

    //调用test方法判断
    var b = reg.test(str);
    //输出判断结果
    document.write(b + "<br/>");
</script>
</body>
</html>
```



### 小结

判断正则表达式与字符串是否匹配的方法是？

```javascript
boolean 正则表达式对象.test("字符串")
如果正则表达式匹配字符串，返回true，否则返回false
```



## <font color="red">案例：使用正则表达式校验注册表单</font>

### 目标

![1552051875066](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552051875066.png)

### onsubmit事件说明

```html
表单在提交的时候激活的事件

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>onsubmit事件</title>
</head>
<body>
<!--
在提交表单之前会先执行onsubmit事件中的函数
如果这个onsubmit事件中函数返回false，可以阻止表单的提交，返回true提交表单
-->

<form action="server" method="get" onsubmit="return checkAll()">
    <input type="submit">
</form>

<script type="text/javascript">
    //如果表单中所有的项都通过验证，提交表单，否则就阻止表单的提交，返回false
    function checkAll() {
        return false;
    }
</script>
</body>
</html>
```



### 案例需求

用户注册，需要进行如下验证，请在JS中使用正则表达式进行验证。

1. 用户名：只能由英文字母和数字组成，长度为4～16个字符，并且以英文字母开头
2. 密码： 大小写字母和数字6-20个字符
3. 确认密码：两次密码要相同
4. 电子邮箱: 符合邮箱地址的格式 /^\w+@\w+(\\.[a-zA-Z]{2,3}){1,2}\$/
5. 手机号：/^1[34578]\d{9}\$/
6. 生日：生日的年份在1900～2009之间，生日格式为1980-5-12或1988-05-04的形式，/^((19\d{2})|(200\d))-(0?[1-9]|1[0-2])-(0?[1-9]|[1-2]\d|3[0-1])$/

### 案例分析

1. 创建正则表达式
2. 得到文本框中输入的值
3. 如果不匹配，在后面的span中显示错误信息，返回false
4. 如果匹配，在后面的span中显示一个打勾图片，返回true
5. 写一个验证表单中所有的项的方法，所有的方法都返回true，这个方法才返回true.

### 代码

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
    <title>验证注册页面</title>
    <style type="text/css">
        body {
            margin: 0;
            padding: 0;
            font-size: 12px;
            line-height: 20px;
        }

        .main {
            width: 525px;
            margin-left: auto;
            margin-right: auto;
        }

        .hr_1 {
            font-size: 14px;
            font-weight: bold;
            color: #3275c3;
            height: 35px;
            border-bottom-width: 2px;
            border-bottom-style: solid;
            border-bottom-color: #3275c3;
            vertical-align: bottom;
            padding-left: 12px;
        }

        .left {
            text-align: right;
            width: 80px;
            height: 25px;
            padding-right: 5px;
        }

        .center {
            width: 280px;
        }

        .in {
            width: 130px;
            height: 16px;
            border: solid 1px #79abea;
        }

        .red {
            color: #cc0000;
            font-weight: bold;
        }

        div {
            color: #F00;
        }

        span {
            color: red;
        }
    </style>

    <script type="text/javascript">
        /*
        验证用户名
         */
        function checkUser() {
            //1.得到文本框中值
            var value = document.getElementById("user").value.trim();
            //2. 创建正则表达式: 由英文字母和数字组成，长度为4～16个字符，并且以英文字母开头
            var reg = /^[a-zA-Z][a-zA-Z0-9]{3,15}$/;
            //3. 判断正则表达式和文本框中值是否匹配
            if (reg.test(value)) {
                //4. 如果匹配返回true，并且在后面显示勾
                document.getElementById("userInfo").innerHTML = "<img src='img/gou.png' width='15'>";
                return true;
            }
            //5. 如果验证失败，返回false，并且在后面出现错误信息
            else {
                document.getElementById("userInfo").innerHTML = "用户名格式错误";
                return false;
            }
        }

        /*
       验证手机
        */
        function checkMobile() {
            //1.得到文本框中值
            var value = document.getElementById("mobile").value.trim();
            //2. 创建正则表达式: 由英文字母和数字组成，长度为4～16个字符，并且以英文字母开头
            var reg = /^1[34578]\d{9}$/;
            //3. 判断正则表达式和文本框中值是否匹配
            if (reg.test(value)) {
                //4. 如果匹配返回true，并且在后面显示勾
                document.getElementById("mobileInfo").innerHTML = "<img src='img/gou.png' width='15'>";
                return true;
            }
            //5. 如果验证失败，返回false，并且在后面出现错误信息
            else {
                document.getElementById("mobileInfo").innerHTML = "手机格式错误";
                return false;
            }
        }

        //验证所有的方法
        function checkAll() {
            return checkUser() && checkMobile();
        }
    </script>
</head>

<body>
<form action="server" method="post" id="myform" onsubmit="return checkAll()">
    <table class="main" border="0" cellspacing="0" cellpadding="0">
        <tr>
            <td><img src="img/logo.jpg" alt="logo"/><img src="img/banner.jpg" alt="banner"/></td>
        </tr>
        <tr>
            <td class="hr_1">新用户注册</td>
        </tr>
        <tr>
            <td style="height:10px;"></td>
        </tr>
        <tr>
            <td>
                <table width="100%" border="0" cellspacing="0" cellpadding="0">
                    <tr>
                        <!-- 不能为空， 输入长度必须介于 5 和 10 之间 -->
                        <td class="left">用户名：</td>
                        <td class="center">
                            <!--文本框失去焦点就验证-->
                            <input id="user" name="user" type="text" class="in" onblur="checkUser()"/>
                            <span id="userInfo"></span>
                        </td>
                    </tr>
                    <tr>
                        <!-- 不能为空， 输入长度大于6个字符 -->
                        <td class="left">密码：</td>
                        <td class="center">
                            <input id="pwd" name="pwd" type="password" class="in"/>
                        </td>
                    </tr>
                    <tr>
                        <!-- 不能为空， 与密码相同 -->
                        <td class="left">确认密码：</td>
                        <td class="center">
                            <input id="repwd" name="repwd" type="password" class="in"/>
                        </td>
                    </tr>
                    <tr>
                        <!-- 不能为空， 邮箱格式要正确 -->
                        <td class="left">电子邮箱：</td>
                        <td class="center">
                            <input id="email" name="email" type="text" class="in"/>
                        </td>
                    </tr>
                    <tr>
                        <!-- 不能为空， 使用正则表达式自定义校验规则,1开头，11位全是数字 -->
                        <td class="left">手机号码：</td>
                        <td class="center">
                            <input id="mobile" name="mobile" type="text" class="in" onblur="checkMobile()"/>
                            <span id="mobileInfo"></span>
                        </td>
                    </tr>
                    <tr>
                        <!-- 不能为空， 要正确的日期格式 -->
                        <td class="left">生日：</td>
                        <td class="center">
                            <input id="birth" name="birth" type="text" class="in"/>
                        </td>
                    </tr>
                    <tr>
                        <td class="left">&nbsp;</td>
                        <td class="center">
                            <input name="" type="image" src="img/register.jpg"/>
                        </td>
                    </tr>
                </table>
            </td>
        </tr>
    </table>
</form>
</body>
</html>

```

### 小结

1. 正则表达式验证字符串的方法是什么？

   ```
   test()
   ```

   

2. 表单提交的时候会激活哪个事件？

   ```
   onsubmit
   ```

   



## Bootstrap框架的概述和优点

### 目标

1. 什么是JS框架
2. bootstrap的作用是什么

### 什么是Bootstrap

1.	一个前端的框架，提高前端的开发效率，制作出更加专业，漂亮的页面。
2.	基于html、css、js技术，只要有这个基础就可以使用Bootstrap

![1552204201045](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552204201045.png)

bootstrap中文官网：http://www.bootcss.com

### Bootstrap的优势

1.	自 Bootstrap 3 起，框架包含了贯穿于整个库的移动设备优先的样式。
2.	Bootstrap支持所有的主流浏览器。如：Internet Explorer、 Firefox、 Opera、 Google Chrome、Safari。
3.	只要您具备 HTML 和 CSS 的基础知识，您就可以开始学习 Bootstrap。
4.	Bootstrap 的响应式 CSS 能够自适应于台式机、平板电脑和手机。

![1552204315056](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552204315056.png)

### 小结

1. 什么是框架?

   框架就是第三方厂商已经写好的半成品，我们在它的基础上完成成品。提高开发效率，降低开发难度。

2. bootstrap是什么？

   前端框架，用于表示层。



## <font color="red">什么是响应式设计</font>

### 目标

什么是响应式设计

### 软件开发的三层架构

1. 表示层
2. 业务层
3. 持久层(数据访问层DAO = Data Access Object)

### 传统的网页

#### 电脑上效果

解决方案：表示层有2套，业务层和数据访问层是一样的。

![1552204416947](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552204416947.png)

#### 手机上效果

![1552204454556](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552204454556.png)

### 响应式设计网页

#### 电脑上效果

![1552204496462](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552204496462.png)

#### 手机上效果

![1552204517381](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552204517381.png)

### 响应式布局特点

同一个网页在不同的设备上自动适配屏幕的分辨率和宽度

### bootstrap支持四类设备

| 四类设备 | 类名的前缀 |
| -------- | ---------- |
| 微型设备 | xs         |
| 小型设备 | sm         |
| 中型设备 | md         |
| 大型设备 | lg         |

### 小结

什么是网页的响应式设计？

```
同一个网页在不同的设备上自动适配屏幕分辩率和宽度
```



## Bootstrap的下载和使用

### 目标

如何在网页中使用bootstrap

### Bootstrap包含的内容

1. 设置全局 CSS 样式；基本的 HTML 元素均可以通过 class 设置样式并得到增强效果。

2. 无数可复用的组件，包括字体图标、下拉菜单、导航、警告框、弹出框等更多功能。
3. jQuery 插件为 Bootstrap 的组件赋予了“生命”。

### 下载

<http://www.bootcss.com>，下载用于生产环境的Bootstrap

![1552205574995](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552205574995.png)

### 目录结构

![1571800208661](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1571800208661.png) 

![1552205695507](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552205695507.png)

### 压缩版与标准版的介绍

- 标准版：bootstrap.js 文件比较大，可读性比较强。
- 压缩版：文件比较小，可读取性差，占资源少。

功能上是一样的

### 小结

说说bootstrap中这几个目录的作用：

1. css：bootstrap做好的样式
2. js：JavaScript的插件
3. font：字体图标

使用的时候将三个目录都复制过去就可以了



## <font color="red">创建Bootstrap的模板</font>

### 目标

创建bootstrap的模板

### 步骤

1. 复制解压出来的三个文件夹到项目中：css, js, fonts
2. 复制jquery框架：jquery-3.2.1.min.js复制到js目录下
3. 复制："起步->基本模板"中代码到网页，修改网页的内容。

- 此模板文件只要创建1次即可，以后可以重用。

### 模板的代码

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <!-- 使用最新的内核来解析当前网页-->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!--
    设置视口参数：默认网页宽度等于设备的宽度，初始缩放比1:1
    视口概念：在浏览器中虚拟的一个网页容器，可以随着设备的不同进行缩放
    -->
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>Bootstrap 101 Template</title>

    <!-- 1. 导入Bootstrap的样式 -->
    <link href="css/bootstrap.css" rel="stylesheet">
</head>
<body>
<h1>你好，世界！</h1>
<hr/>

<!-- 2. 导入jQuery.js文件 (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
<script src="js/jquery-3.2.1.min.js"></script>

<!-- 3. 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
<script src="js/bootstrap.min.js"></script>
</body>
</html>
```

### 小结

创建bootstrap模板的步骤有哪几步？

1. 复制三个文件夹css, font, js到项目中
2. 复制"起步--> 基本模板"，把代码复制过去
3. 复制jquery.js文件到js目录
4. 修改模板文件中三行：bootstrap.css,jquery-3.2.1.min.js,bootstrap.min.js，改成本地地址



## <font color="red">栅格系统的介绍</font>

### 目标

1. 栅格系统的组成
2. 两种布局的容器

### 组成

​	Bootstrap 提供了一套响应式、移动设备优先的流式栅格系统，随着屏幕或视口（viewport）尺寸的增加，系统会自动分为最多12格。栅格系统用于通过一系列的行（row）与列（column）的组合来创建页面布局，你的内容就可以放入这些创建好的布局中。

![1552206012518](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552206012518.png)

### 两种容器

| 两种容器的类样式名 | 说明                                             |
| ------------------ | ------------------------------------------------ |
| container          | 固定宽度的容器，在不同的设备上显示不同的固定宽度 |
| container-fluid    | 在所有的设备上都以100%宽度显示                   |

### 案例效果

![1552206063236](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552206063236.png)

### 代码

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <!-- 使用最新的内核来解析当前网页-->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!--
    设置视口参数：默认网页宽度等于设备的宽度，初始缩放比1:1
    视口概念：在浏览器中虚拟的一个网页容器，可以随着设备的不同进行缩放
    -->
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>两种容器</title>

    <!-- 1. 导入Bootstrap的样式 -->
    <link href="css/bootstrap.css" rel="stylesheet">

    <style>
        div {
            border: 1px solid red;
            height: 100px;
        }
    </style>
</head>
<body>
<div class="container">
    固定宽度的容器
</div>

<div class="container-fluid">
    100%宽度
</div>
</body>
</html>
```

### 设备查询@media

​	通过不同的设备类型和条件定义样式表规则。设备查询让CSS可以更精确作用于不同的设备类型和同一设备的不同条件。设备查询的大部分特性都接受min和max用于表达“大于或等于”和“小于或等于”。打开文件：bootstrap.css，可以看到以下代码：

```css
.container {
  padding-right: 15px;
  padding-left: 15px;   左右内边距是15px
  margin-right: auto;   水平居中
  margin-left: auto;
}
@media (min-width: 768px) {  设备大于或等于768px
  .container {
    width: 750px;  容器的宽度是750px
  }
}
@media (min-width: 992px) {  设备大于或等于992px
  .container {
    width: 970px;   容器的宽度是970px
  }
}
@media (min-width: 1200px) {   设备大于或等于1200px
  .container {
    width: 1170px;    容器的宽度是1170px
  }
}
```

- 结论：如果设备的宽度发生变化，容器的宽度也会发生变化

### 栅格系统有关的类样式名

| 栅格系统         | 描述                                                         | 类似于表格 |
| ---------------- | ------------------------------------------------------------ | ---------- |
| .container       | 固定宽度的容器                                               | table      |
| .container-fluid | 始终显示100%宽度                                             | table      |
| .row             | 一行，必须放在容器中                                         | tr         |
| .col-xx-n        | 一列，一列占多个格子<br />xs: 微型<br />sm: 小型<br />md: 中型<br />lg: 大型<br />如：<br />col-md-4 在中型设备上一列占4个格子<br />col-lg-3 在大型设备上一列占3个格子 | td         |

### 栅格的参数

![1552206304153](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552206304153.png)

### 小结

1. 栅格系统容器：
   1. container
   2. container-fluid
2. 一行的类名：row
3. col-md-4表示什么意思？ 在中型设备上一列占4个格子，一行最多12个格子



## 演示：栅格系统的布局案例

### 目标

通过4个案例学习栅格系统的布局

### 示例1：基本结构

​	在中型设置上显示：2行3列

![1552206379296](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552206379296.png)

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <!-- 使用最新的内核来解析当前网页-->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!--
    设置视口参数：默认网页宽度等于设备的宽度，初始缩放比1:1
    视口概念：在浏览器中虚拟的一个网页容器，可以随着设备的不同进行缩放
    -->
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>Bootstrap 101 Template</title>

    <!-- 1. 导入Bootstrap的样式 -->
    <link href="css/bootstrap.css" rel="stylesheet">

    <style>
        .row div {
            border: 1px solid red;
            height: 100px;
        }
    </style>
</head>
<body>
<!--容器-->
<div class="container">
    <!--    行-->
    <div class="row">
        <div class="col-md-4">
            中型设备
        </div>
        <div class="col-md-4">
            中型设备
        </div>
        <div class="col-md-4">
            中型设备
        </div>
    </div>

    <div class="row">
        <div class="col-md-4">
            中型设备
        </div>
        <div class="col-md-4">
            中型设备
        </div>
        <div class="col-md-4">
            中型设备
        </div>
    </div>
</div>

<!-- 2. 导入jQuery.js文件 (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
<script src="js/jquery-3.2.1.min.js"></script>

<!-- 3. 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
<script src="js/bootstrap.min.js"></script>
</body>
</html>
```

### 示例2：一行超过12格

​	如果1行超过12格：多出来的列就放在下一行

![1552206420279](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552206420279.png)

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <!-- 使用最新的内核来解析当前网页-->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!--
    设置视口参数：默认网页宽度等于设备的宽度，初始缩放比1:1
    视口概念：在浏览器中虚拟的一个网页容器，可以随着设备的不同进行缩放
    -->
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>Bootstrap 101 Template</title>

    <!-- 1. 导入Bootstrap的样式 -->
    <link href="css/bootstrap.css" rel="stylesheet">

    <style>
        .row div {
            border: 1px solid red;
            height: 100px;
        }
    </style>
</head>
<body>
<!--容器-->
<div class="container">
    <!--    行-->
    <div class="row">
        <div class="col-md-4">
            中型设备
        </div>
        <div class="col-md-4">
            中型设备
        </div>
        <div class="col-md-4">
            中型设备
        </div>
        <div class="col-md-4">
            中型设备
        </div>
        <div class="col-md-4">
            中型设备
        </div>
    </div>
</div>

<!-- 2. 导入jQuery.js文件 (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
<script src="js/jquery-3.2.1.min.js"></script>

<!-- 3. 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
<script src="js/bootstrap.min.js"></script>
</body>
</html>
```



### 示例3：不同屏幕的适配

​	在微型，小型，中型设备上显示不同的列数

![1552206467393](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552206467393.png)

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <!-- 使用最新的内核来解析当前网页-->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!--
    设置视口参数：默认网页宽度等于设备的宽度，初始缩放比1:1
    视口概念：在浏览器中虚拟的一个网页容器，可以随着设备的不同进行缩放
    -->
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>Bootstrap 101 Template</title>

    <!-- 1. 导入Bootstrap的样式 -->
    <link href="css/bootstrap.css" rel="stylesheet">

    <style>
        .row div {
            border: 1px solid red;
            height: 100px;
        }
    </style>
</head>
<body>
<!--容器-->
<div class="container">
    <!--    行-->
    <div class="row">
        <div class="col-md-4 col-sm-3 col-xs-6">
            col-md-4 col-sm-3 col-xs-6
        </div>
        <div class="col-md-4 col-sm-3 col-xs-6">
            col-md-4 col-sm-3 col-xs-6
        </div>
        <div class="col-md-4 col-sm-3 col-xs-6">
            col-md-4 col-sm-3 col-xs-6
        </div>
        <div class="col-md-4 col-sm-3 col-xs-6">
            col-md-4 col-sm-3 col-xs-6
        </div>
        <div class="col-md-4 col-sm-3 col-xs-6">
            col-md-4 col-sm-3 col-xs-6
        </div>
        <div class="col-md-4 col-sm-3 col-xs-6">
            col-md-4 col-sm-3 col-xs-6
        </div>
    </div>
</div>

<!-- 2. 导入jQuery.js文件 (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
<script src="js/jquery-3.2.1.min.js"></script>

<!-- 3. 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
<script src="js/bootstrap.min.js"></script>
</body>
</html>
```

### 示例4：显示和隐藏块

​	在微型设备上隐藏，在小型设备上显示

![1552206514114](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552206514114.png)

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <!-- 使用最新的内核来解析当前网页-->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!--
    设置视口参数：默认网页宽度等于设备的宽度，初始缩放比1:1
    视口概念：在浏览器中虚拟的一个网页容器，可以随着设备的不同进行缩放
    -->
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>Bootstrap 101 Template</title>

    <!-- 1. 导入Bootstrap的样式 -->
    <link href="css/bootstrap.css" rel="stylesheet">

    <style>
        .row div {
            border: 1px solid red;
            height: 100px;
        }
    </style>
</head>
<body>
<!--容器-->
<div class="container">
    <!--    行-->
    <div class="row">
        <div class="col-md-4 col-sm-3 col-xs-6 visible-sm">
            我只在小型设备上显示
        </div>
        <div class="col-md-4 col-sm-3 col-xs-6 hidden-xs">
            我在微型设备上隐藏
        </div>
        <div class="col-md-4 col-sm-3 col-xs-6">
            col-md-4 col-sm-3 col-xs-6
        </div>
        <div class="col-md-4 col-sm-3 col-xs-6">
            col-md-4 col-sm-3 col-xs-6
        </div>
        <div class="col-md-4 col-sm-3 col-xs-6">
            col-md-4 col-sm-3 col-xs-6
        </div>
        <div class="col-md-4 col-sm-3 col-xs-6">
            col-md-4 col-sm-3 col-xs-6
        </div>
    </div>
</div>

<!-- 2. 导入jQuery.js文件 (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
<script src="js/jquery-3.2.1.min.js"></script>

<!-- 3. 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
<script src="js/bootstrap.min.js"></script>
</body>
</html>
```

### 小结

| 类样式名             | 作用                  |
| -------------------- | --------------------- |
| .container           | 固定宽度的容器        |
| .container-fluid     | 100%宽度的容器        |
| .row                 | 一行                  |
| .col-xs-n            | 在微型设备上一列占n格 |
| .col-sm-n            | 在小型设备上一列占n格 |
| .col-md-n            | 在中型设备上一列占n格 |
| .col-lg-n            | 在大型设备上一列占n格 |
| .hidden-lg/md/sm/xs  | 在指定设备上隐藏      |
| .visible-lg/md/sm/xs | 只在指定设备上显示    |



## 上午内容的回顾

### 正则表达式使用

创建语法

```
new RegExp("正则表达式","匹配模式 i")
```

```
/正则表达式/匹配模式
```

正则表达式方法

```
boolean test("字符串")
如果匹配返回true，否则返回false
```

案例：使用正则表达式来校验注册表单

```
1. 每个文本框使用onblur事件
2. 在文本框后面添加了span标签，显示信息
3. 在表单上onsubmit事件，如果返回false表单不能提交
```

### Bootstrap框架概述

它是一个前端框架，可以简化前端代码编写。有HTML和CSS的基础知识就可以使用了。

### 什么是响应式设计

同一个网页可以在不同的设备上自动适配分辨率和宽度

### 创建Bootstrap模板步骤

1. 复制三个文件夹：css, js, fonts到项目中
2. 复制jquery.js到js目录下
3. 复制"起步-->基本模板" 代码到网页中
4. 修改网页中三个导入文件，修改成本地文件。

### 栅格系统

```
container：固定宽度的容器
container-fluid：100%宽度容器
row：一行，一行最多12格
col-xs-4：在微型设备上一列占4格， sm, md, lg
visible-xs：只在某种设备上显示
hidden-xs：只在某种设备上隐藏
```



## 全局CSS样式：按钮

### 目标

1. 普通按钮
2. 不同颜色的按钮

### 效果

![1552206592422](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552206592422.png) 

![1552206627547](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552206627547.png) 

### 代码

```html
<div class="container">
    <h2>普通按钮</h2>
    <input type="button" value="这是按钮" class="btn btn-default">
    <input type="reset" value="重置按钮" class="btn btn-default">
    <a href="#" class="btn btn-default">这是链接</a>
    <hr/>

    <h2>有颜色按钮</h2>
    <input type="button" value="这是按钮" class="btn btn-primary">
    <input type="reset" value="重置按钮" class="btn btn-success">
    <a href="#" class="btn btn-danger">这是链接</a>
    <hr/>
    <h2>不同大小</h2>
    <input type="button" value="这是按钮" class="btn btn-primary btn-lg">
    <input type="reset" value="重置按钮" class="btn btn-success btn-sm">
    <a href="#" class="btn btn-danger btn-xs">这是链接</a>
    <hr/>
</div>
```



### 小结

1. 按钮使用哪个类

   ```
   btn
   ```

   

2. 标准按钮使用哪个类

   ```
   btn-default
   ```

   



## 全局CSS样式：图片

### 目标

1. 响应式图片的使用
2. 图片的形状

### 响应式图片

​	通过为图片添加 .img-responsive 类可以让图片支持响应式布局。其实质是为图片设置了 max-width: 100%;、height: auto; 和 display: block; 属性，从而让图片在其父元素中更好的缩放。

### 图片形状

![1552207355159](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552207355159.png)

### 效果

![1552207365241](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552207365241.png)

### 代码

```html
<h2>响应式图片</h2>
<img src="img/01.jpg" class="img-responsive">
<hr/>
<img src="img/01.jpg" class="img-responsive img-circle">
<hr/>
<img src="img/01.jpg" class="img-responsive img-rounded">
<hr/>
<img src="img/01.jpg" class="img-responsive img-thumbnail">
```

### 小结

响应式图片设置哪个类名？

```
img-responsive
```



## 全局CSS样式：表单

### 目标

​	使用表单样式制作如图效果

![1552207576823](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552207576823.png) 

### 表单

![1552207596169](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552207596169.png)

### 代码

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <!-- 使用最新的内核来解析当前网页-->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!--
    设置视口参数：默认网页宽度等于设备的宽度，初始缩放比1:1
    视口概念：在浏览器中虚拟的一个网页容器，可以随着设备的不同进行缩放
    -->
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>添加联系人</title>

    <!-- 1. 导入Bootstrap的样式 -->
    <link href="css/bootstrap.css" rel="stylesheet">
</head>
<body>
<div class="container">
<h2>添加联系人</h2>
<form style="max-width: 400px">
    <div class="form-group">
        <!-- label一个标签, for 表示给哪个表单项使用的 等于表单项的id-->
        <label for="username">姓名</label>
        <input type="text" class="form-control" id="username" placeholder="请输入姓名">
    </div>

    <div class="form-group">
        <label>性别</label>
        <input type="radio" name="sex" id="male" checked="checked"><label for="male">男</label>
        <input type="radio" name="sex" id="female"><label for="female">女</label>
    </div>

    <div class="form-group">
        <label for="birthday">生日</label>
        <input type="date" class="form-control" id="birthday">
    </div>

    <div class="form-group">
        <label for="edu">学历</label>
        <select id="edu" class="form-control">
            <option value="本科">本科</option>
            <option value="硕士">硕士</option>
        </select>
    </div>

    <div class="text-center">
            <input type="submit" value="注册" class="btn btn-primary">
            <input type="button" value="退出" class="btn btn-danger">
    </div>
</form>
</div>
<!-- 2. 导入jQuery.js文件 (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
<script src="js/jquery-3.2.1.min.js"></script>

<!-- 3. 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
<script src="js/bootstrap.min.js"></script>
</body>
</html>
```



### 小结

1. form-control的特点

   ```
   将表单项以100%宽度来显示
   ```

   

2. form-group有什么用

   ```
   分组，设置底部外边距为15px
   ```

   



## 全局CSS样式：表格

### 目标

使用表格制作如图效果

![1552207670959](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552207670959.png)

### 表格的类样式

![1552207696806](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552207696806.png)

### 不同行的颜色

![1552207726252](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552207726252.png)

### 代码

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <!-- 使用最新的内核来解析当前网页-->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!--
    设置视口参数：默认网页宽度等于设备的宽度，初始缩放比1:1
    视口概念：在浏览器中虚拟的一个网页容器，可以随着设备的不同进行缩放
    -->
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>Bootstrap 101 Template</title>

    <!-- 1. 导入Bootstrap的样式 -->
    <link href="css/bootstrap.css" rel="stylesheet">
</head>
<body>

<div class="container">
    <h2>七龙珠</h2>
    <table class="table table-bordered table-hover table-condensed">
        <tr class="success">
            <th>编号</th>
            <th>姓名</th>
            <th>性别</th>
            <th>电话</th>
        </tr>
        <tr>
            <td>1302</td>
            <td>贝吉塔</td>
            <td>男</td>
            <td>19509869504</td>
        </tr>
        <tr>
            <td>5940</td>
            <td class="danger">孙悟空</td>
            <td>男</td>
            <td>13938475687</td>
        </tr>
        <tr>
            <td>6841</td>
            <td>龟仙人</td>
            <td>男</td>
            <td>18501029504</td>
        </tr>
    </table>
</div>

<!-- 2. 导入jQuery.js文件 (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
<script src="js/jquery-3.2.1.min.js"></script>

<!-- 3. 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
<script src="js/bootstrap.min.js"></script>
</body>
</html>
```

### 小结

显示表格边框的类名是什么？

```
table-bordered
```



## 组件：字体图标

### 目标

![1552207778505](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552207778505.png) 

### 代码

```html
<h2>字体图标</h2>
<!--文字就是类名-->
<span class="glyphicon glyphicon-heart" style="font-size: 40px;color: red"></span>我心依旧
<span class="glyphicon glyphicon-camera" style="font-size: 40px;color: blue"></span>我心依旧
<span class="glyphicon glyphicon-tint" style="font-size: 40px;color: green"></span>我心依旧
```

### 小结

每个图标下的文字是什么意思？

```
就是它的类名
```



## 组件：导航条

### 目标

做出如下图所示的导航条

![1552207828085](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552207828085.png)

### 导航条

​	什么是导航条：导航条是在您的应用或网站中作为导航页头的响应式基础组件。它们在移动设备上可以折叠（并且可开可关），且在视口（viewport）宽度增加时逐渐变为水平展开模式。

### 组成

​	整个导航条就是一个nav标签，nav与div功能相同，它是一个语义标签。

![1552207898669](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552207898669.png)

### 属性说明

![1552207925499](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552207925499.png)

### 代码

```html
<nav class="navbar navbar-default">
    <div class="container-fluid">

        <!-- 商标和折叠按钮 -->
        <div class="navbar-header">
            <!--折叠按钮-->
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse"
                    data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
                <span class="glyphicon glyphicon-list"></span>
            </button>
            <a class="navbar-brand" href="#">
                <span class="glyphicon glyphicon-piggy-bank"></span>
            </a>
        </div>

        <!-- 链接 和 表单 -->
        <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">


            <ul class="nav navbar-nav">
                <li class="active"><a href="#">国内游</span></a></li>
                <li><a href="#">出境游</a></li>
                <li><a href="#">地沟游</a></li>
                <li><a href="#">印度神游</a></li>

                <!--下拉菜单-->
                <li class="dropdown">
                    <!--<span class="caret"></span>表示向下箭头-->
                    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button"
                       aria-haspopup="true" aria-expanded="false">港澳游<span class="caret"></span></a>
                    <ul class="dropdown-menu">
                        <li><a href="#">澳门威尼斯</a></li>
                        <li><a href="#">Another action</a></li>
                        <li><a href="#">Something else here</a></li>
                        <!--separator 水平分隔线-->
                        <li role="separator" class="divider"></li>
                        <li><a href="#">Separated link</a></li>
                        <li role="separator" class="divider"></li>
                        <li><a href="#">One more separated link</a></li>
                    </ul>
                </li>
            </ul>

            <!--搜索表单-->
            <form class="navbar-form navbar-right">
                <div class="form-group">
                    <input type="text" class="form-control" placeholder="输入关键字">
                </div>
                <button type="submit" class="btn btn-default">
                    <span class="glyphicon glyphicon-search"></span>
                    搜索
                </button>
            </form>

        </div><!-- /.navbar-collapse -->
    </div><!-- /.container-fluid -->
</nav>
```

### 小结

导航条的作用是什么？

```
导航条是在您的应用或网站中作为导航页头的响应式基础组件。它们在移动设备上可以折叠（并且可开可关），且在视口（viewport）宽度增加时逐渐变为水平展开模式。
```



## 组件：缩略图

### 目标

1. 标准的缩略图
2. 自定义的缩略图

### 什么是缩略图

​	扩展 Bootstrap 的 栅格系统，可以很容易地展示栅格样式的图像、视频、文本等内容。

### 缩略图的类

![1552208034281](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552208034281.png)

### 标准缩略图

![1552208095025](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552208095025.png)

### 自定义缩略图

![1552208114924](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552208114924.png)

### 代码

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <!-- 使用最新的内核来解析当前网页-->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!--
    设置视口参数：默认网页宽度等于设备的宽度，初始缩放比1:1
    视口概念：在浏览器中虚拟的一个网页容器，可以随着设备的不同进行缩放
    -->
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>Bootstrap 101 Template</title>

    <!-- 1. 导入Bootstrap的样式 -->
    <link href="css/bootstrap.css" rel="stylesheet">
</head>
<body>

<div class="container">
    <div class="row">
        <div class="col-xs-6 col-md-3">
            <a href="#" class="thumbnail">
                <img src="img/02.jpg">
            </a>
        </div>
        <div class="col-xs-6 col-md-3">
            <a href="#" class="thumbnail">
                <img src="img/02.jpg">
            </a>
        </div>
        <div class="col-xs-6 col-md-3">
            <a href="#" class="thumbnail">
                <img src="img/02.jpg">
            </a>
        </div>
        <div class="col-xs-6 col-md-3">
            <a href="#" class="thumbnail">
                <img src="img/02.jpg">
            </a>
        </div>
    </div>

    <div class="row">
        <div class="col-sm-6 col-md-4">
            <div class="thumbnail">
                <img src="img/03.jpg">
                <div class="caption">
                    <h3>我是女生</h3>
                    <p>Donec id elit non mi porta gravida at eget metus. Nullam id dolor id nibh ultricies vehicula ut id elit.</p>
                    <p><a href="#" class="btn btn-primary" role="button">原价</a> <a href="#" class="btn btn-default" role="button">折后价</a></p>
                </div>
            </div>
        </div>

        <div class="col-sm-6 col-md-4">
            <div class="thumbnail">
                <img src="img/03.jpg">
                <div class="caption">
                    <h3>我是女生</h3>
                    <p>Donec id elit non mi porta gravida at eget metus. Nullam id dolor id nibh ultricies vehicula ut id elit.</p>
                    <p><a href="#" class="btn btn-primary" role="button">原价</a> <a href="#" class="btn btn-default" role="button">折后价</a></p>
                </div>
            </div>
        </div>

        <div class="col-sm-6 col-md-4">
            <div class="thumbnail">
                <img src="img/03.jpg">
                <div class="caption">
                    <h3>我是女生</h3>
                    <p>Donec id elit non mi porta gravida at eget metus. Nullam id dolor id nibh ultricies vehicula ut id elit.</p>
                    <p><a href="#" class="btn btn-primary" role="button">原价</a> <a href="#" class="btn btn-default" role="button">折后价</a></p>
                </div>
            </div>
        </div>

        <div class="col-sm-6 col-md-4">
            <div class="thumbnail">
                <img src="img/03.jpg">
                <div class="caption">
                    <h3>我是女生</h3>
                    <p>Donec id elit non mi porta gravida at eget metus. Nullam id dolor id nibh ultricies vehicula ut id elit.</p>
                    <p><a href="#" class="btn btn-primary" role="button">原价</a> <a href="#" class="btn btn-default" role="button">折后价</a></p>
                </div>
            </div>
        </div>
    </div>
</div>

<!-- 2. 导入jQuery.js文件 (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
<script src="js/jquery-3.2.1.min.js"></script>

<!-- 3. 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
<script src="js/bootstrap.min.js"></script>
</body>
</html>
```

### 小结

缩略图有哪两种？

```
1. 标准
2. 自定义
```



## JavaScript插件：轮播图

### 目标

创建轮播图

### 轮播图

​	Javascript插件必须要使用到jQuery框架

​	这个组件用于轮播显示一组图片，类似于旋转木马，页面加载就自动运行，也可以通过点击左右的两个箭头向左或向右翻页。

![1552208196569](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552208196569.png)

### 类样式名

![1552208216074](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552208216074.png)

### 相关的属性

![1552208233886](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552208233886.png)

### 代码

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <!-- 使用最新的内核来解析当前网页-->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!--
    设置视口参数：默认网页宽度等于设备的宽度，初始缩放比1:1
    视口概念：在浏览器中虚拟的一个网页容器，可以随着设备的不同进行缩放
    -->
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>Bootstrap 101 Template</title>

    <!-- 1. 导入Bootstrap的样式 -->
    <link href="css/bootstrap.css" rel="stylesheet">
</head>
<body>

<div class="container">

    <div id="carousel-example-generic" class="carousel slide" data-ride="carousel" data-interval="1000">
        <!-- 1. 指示器：指的小圆点 -->
        <ol class="carousel-indicators">
            <!--active表示激活的-->
            <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li>
            <li data-target="#carousel-example-generic" data-slide-to="1"></li>
            <li data-target="#carousel-example-generic" data-slide-to="2"></li>
        </ol>

        <!-- 2. 图片部分 -->
        <div class="carousel-inner" role="listbox">
            <div class="item active">
                <!--图片-->
                <img src="img/03.jpg">
                <!--标题-->
                <div class="carousel-caption">
                    花木兰
                </div>
            </div>

            <div class="item">
                <img src="img/04.jpg">
                <div class="carousel-caption">
                    毛玻璃
                </div>
            </div>

            <div class="item">
                <img src="img/05.jpg">
                <div class="carousel-caption">
                    10块钱3件
                </div>
            </div>
        </div>

        <!-- 3. 左右按钮 -->
        <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
            <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
        </a>
        <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
            <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
        </a>
    </div>

</div>
<!-- 2. 导入jQuery.js文件 (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
<script src="js/jquery-3.2.1.min.js"></script>

<!-- 3. 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
<script src="js/bootstrap.min.js"></script>
</body>
</html>
```

### 小结

轮播图三个组成部分：

```
1. 圆点指示器
2. 中间的图片
3. 左右按钮
```



## 案例：黑马旅游首页-上部

### 目标

​	完成上部的4行

![1552208522794](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552208522794.png)

注：第一行图片的宽度不够，可以设置宽度成100%

### 代码

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <!-- 使用最新的内核来解析当前网页-->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!--
    设置视口参数：默认网页宽度等于设备的宽度，初始缩放比1:1
    视口概念：在浏览器中虚拟的一个网页容器，可以随着设备的不同进行缩放
    -->
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>黑马旅游-首页</title>

    <!-- 1. 导入Bootstrap的样式 -->
    <link href="css/bootstrap.css" rel="stylesheet">
</head>
<body>
<!--上部-->
<header class="container-fluid">
    <!--第1行-->
    <div class="row">
        <!--img-responsive 响应式图片-->
        <img src="img/top_banner.jpg" width="100%" class="img-responsive">
    </div>

    <!-- 第2行-->
    <div class="row text-center">
        <div class="col-md-3">
            <img src="img/logo.jpg">
        </div>
        <div class="col-md-6">
            <img src="img/search.png" width="75%">
        </div>
        <div class="col-md-3">
            <img src="img/hotel_tel.png">
        </div>
    </div>

    <!--第3行：导航条-->
    <div class="row">
        <nav class="navbar navbar-default">
            <div class="container-fluid">
                <!-- 商标和折叠按钮 -->
                <div class="navbar-header">
                    <!--折叠按钮-->
                    <button type="button" class="navbar-toggle collapsed" data-toggle="collapse"
                            data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
                        <span class="glyphicon glyphicon-list"></span>
                    </button>
                    <a class="navbar-brand" href="#">
                        <span class="glyphicon glyphicon-piggy-bank"></span>
                    </a>
                </div>

                <!-- 链接 和 表单 -->
                <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">


                    <ul class="nav navbar-nav">
                        <li class="active"><a href="#">国内游</span></a></li>
                        <li><a href="#">出境游</a></li>
                        <li><a href="#">地沟游</a></li>
                        <li><a href="#">印度神游</a></li>

                        <!--下拉菜单-->
                        <li class="dropdown">
                            <!--<span class="caret"></span>表示向下箭头-->
                            <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button"
                               aria-haspopup="true" aria-expanded="false">港澳游<span class="caret"></span></a>
                            <ul class="dropdown-menu">
                                <li><a href="#">澳门威尼斯</a></li>
                                <li><a href="#">Another action</a></li>
                                <li><a href="#">Something else here</a></li>
                                <!--separator 水平分隔线-->
                                <li role="separator" class="divider"></li>
                                <li><a href="#">Separated link</a></li>
                                <li role="separator" class="divider"></li>
                                <li><a href="#">One more separated link</a></li>
                            </ul>
                        </li>
                    </ul>

                    <!--搜索表单-->
                    <form class="navbar-form navbar-right">
                        <div class="form-group">
                            <input type="text" class="form-control" placeholder="输入关键字">
                        </div>
                        <button type="submit" class="btn btn-default">
                            <span class="glyphicon glyphicon-search"></span>
                            搜索
                        </button>
                    </form>

                </div><!-- /.navbar-collapse -->
            </div><!-- /.container-fluid -->
        </nav>
    </div>

    <!--轮播图-->
    <div class="row">
        <div id="carousel-example-generic" class="carousel slide" data-ride="carousel" data-interval="3000">
            <!-- 1. 指示器：指的小圆点 -->
            <ol class="carousel-indicators">
                <!--active表示激活的-->
                <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li>
                <li data-target="#carousel-example-generic" data-slide-to="1"></li>
                <li data-target="#carousel-example-generic" data-slide-to="2"></li>
            </ol>

            <!-- 2. 图片部分 -->
            <div class="carousel-inner" role="listbox">
                <div class="item active">
                    <!--图片-->
                    <img src="img/banner_1.jpg">
                    <!--标题-->
                    <div class="carousel-caption">
                        花木兰
                    </div>
                </div>

                <div class="item">
                    <img src="img/banner_2.jpg">
                    <div class="carousel-caption">
                        毛玻璃
                    </div>
                </div>

                <div class="item">
                    <img src="img/banner_3.jpg">
                    <div class="carousel-caption">
                        10块钱3件
                    </div>
                </div>
            </div>

            <!-- 3. 左右按钮 -->
            <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
                <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
            </a>
            <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
                <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
            </a>
        </div>
    </div>
</header>
<!--主体-->
<main class="container">
        
</main>
<!--脚部-->
<footer class="container-fluid">

</footer>

<!-- 2. 导入jQuery.js文件 (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
<script src="js/jquery-3.2.1.min.js"></script>
<!-- 3. 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
<script src="js/bootstrap.min.js"></script>
</body>
</html>
```



## 案例：黑马旅游首页-黑马精选

### 目标

![1552300581713](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552300581713.png)

### 步骤

1. 创建一个自定义的样式jx_top，用于row内部的div样式
2. 外上边距15px, 底部边框2px 实线 #ffc900做为下面的线，宽度100%，高度35px，行高35px，外下边距5px。
3. 前面的图标，使用字体图标

### 缩略图

![1552208954216](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552208954216.png)

1. 使用bootstrap的组件==> 缩略图 ==> 自定义内容
2. 每个格子占3列。使用p标签加文字，使用span标签加价格，文字使用红色，一共4格。

### 代码

index.html

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <!-- 使用最新的内核来解析当前网页-->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!--
    设置视口参数：默认网页宽度等于设备的宽度，初始缩放比1:1
    视口概念：在浏览器中虚拟的一个网页容器，可以随着设备的不同进行缩放
    -->
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>黑马旅游-首页</title>

    <!-- 1. 导入Bootstrap的样式 -->
    <link href="css/bootstrap.css" rel="stylesheet">
    <link rel="stylesheet" href="css/index.css">
</head>
<body>
<!--上部-->
<header class="container-fluid">
    <!--第1行-->
    <div class="row">
        <!--img-responsive 响应式图片-->
        <img src="img/top_banner.jpg" width="100%" class="img-responsive">
    </div>

    <!-- 第2行-->
    <div class="row text-center">
        <div class="col-md-3">
            <img src="img/logo.jpg">
        </div>
        <div class="col-md-6">
            <img src="img/search.png" width="75%">
        </div>
        <div class="col-md-3">
            <img src="img/hotel_tel.png">
        </div>
    </div>

    <!--第3行：导航条-->
    <div class="row">
        <nav class="navbar navbar-default">
            <div class="container-fluid">
                <!-- 商标和折叠按钮 -->
                <div class="navbar-header">
                    <!--折叠按钮-->
                    <button type="button" class="navbar-toggle collapsed" data-toggle="collapse"
                            data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
                        <span class="glyphicon glyphicon-list"></span>
                    </button>
                    <a class="navbar-brand" href="#">
                        <span class="glyphicon glyphicon-piggy-bank"></span>
                    </a>
                </div>

                <!-- 链接 和 表单 -->
                <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">


                    <ul class="nav navbar-nav">
                        <li class="active"><a href="#">国内游</span></a></li>
                        <li><a href="#">出境游</a></li>
                        <li><a href="#">地沟游</a></li>
                        <li><a href="#">印度神游</a></li>

                        <!--下拉菜单-->
                        <li class="dropdown">
                            <!--<span class="caret"></span>表示向下箭头-->
                            <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button"
                               aria-haspopup="true" aria-expanded="false">港澳游<span class="caret"></span></a>
                            <ul class="dropdown-menu">
                                <li><a href="#">澳门威尼斯</a></li>
                                <li><a href="#">Another action</a></li>
                                <li><a href="#">Something else here</a></li>
                                <!--separator 水平分隔线-->
                                <li role="separator" class="divider"></li>
                                <li><a href="#">Separated link</a></li>
                                <li role="separator" class="divider"></li>
                                <li><a href="#">One more separated link</a></li>
                            </ul>
                        </li>
                    </ul>

                    <!--搜索表单-->
                    <form class="navbar-form navbar-right">
                        <div class="form-group">
                            <input type="text" class="form-control" placeholder="输入关键字">
                        </div>
                        <button type="submit" class="btn btn-default">
                            <span class="glyphicon glyphicon-search"></span>
                            搜索
                        </button>
                    </form>

                </div><!-- /.navbar-collapse -->
            </div><!-- /.container-fluid -->
        </nav>
    </div>

    <!--轮播图-->
    <div class="row">
        <div id="carousel-example-generic" class="carousel slide" data-ride="carousel" data-interval="3000">
            <!-- 1. 指示器：指的小圆点 -->
            <ol class="carousel-indicators">
                <!--active表示激活的-->
                <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li>
                <li data-target="#carousel-example-generic" data-slide-to="1"></li>
                <li data-target="#carousel-example-generic" data-slide-to="2"></li>
            </ol>

            <!-- 2. 图片部分 -->
            <div class="carousel-inner" role="listbox">
                <div class="item active">
                    <!--图片-->
                    <img src="img/banner_1.jpg">
                    <!--标题-->
                    <div class="carousel-caption">
                        花木兰
                    </div>
                </div>

                <div class="item">
                    <img src="img/banner_2.jpg">
                    <div class="carousel-caption">
                        毛玻璃
                    </div>
                </div>

                <div class="item">
                    <img src="img/banner_3.jpg">
                    <div class="carousel-caption">
                        10块钱3件
                    </div>
                </div>
            </div>

            <!-- 3. 左右按钮 -->
            <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
                <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
            </a>
            <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
                <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
            </a>
        </div>
    </div>
</header>
<!--主体-->
<main class="container">
    <div class="row">
            <div class="jx_top">
                <span class="glyphicon glyphicon-camera" style="font-size: 25px; color: green;"></span> 黑马精选
            </div>
    </div>

    <div class="row">
        <div class="col-md-3">
            <div class="thumbnail">
                <img src="img/05.jpg">
                <div class="caption">
                    <p>上海直飞三亚5天4晚自由行</p>
                    <p><a href="#" class="btn btn-danger" role="button" style="text-decoration: line-through">
                        原价：&yen;3999</a> <a href="#" class="btn btn-success" role="button">折后价：&yen;1888</a></p>
                </div>
            </div>
        </div>
        <div class="col-md-3">
            <div class="thumbnail">
                <img src="img/05.jpg">
                <div class="caption">
                    <p>上海直飞三亚5天4晚自由行</p>
                    <p><a href="#" class="btn btn-danger" role="button" style="text-decoration: line-through">
                        原价：&yen;3999</a> <a href="#" class="btn btn-success" role="button">折后价：&yen;1888</a></p>
                </div>
            </div>
        </div>
        <div class="col-md-3">
            <div class="thumbnail">
                <img src="img/05.jpg">
                <div class="caption">
                    <p>上海直飞三亚5天4晚自由行</p>
                    <p><a href="#" class="btn btn-danger" role="button" style="text-decoration: line-through">
                        原价：&yen;3999</a> <a href="#" class="btn btn-success" role="button">折后价：&yen;1888</a></p>
                </div>
            </div>
        </div>
        <div class="col-md-3">
            <div class="thumbnail">
                <img src="img/05.jpg">
                <div class="caption">
                    <p>上海直飞三亚5天4晚自由行</p>
                    <p><a href="#" class="btn btn-danger" role="button" style="text-decoration: line-through">
                        原价：&yen;3999</a> <a href="#" class="btn btn-success" role="button">折后价：&yen;1888</a></p>
                </div>
            </div>
        </div>
    </div>
</main>
<!--脚部-->
<footer class="container-fluid">

</footer>

<!-- 2. 导入jQuery.js文件 (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
<script src="js/jquery-3.2.1.min.js"></script>
<!-- 3. 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
<script src="js/bootstrap.min.js"></script>
</body>
</html>
```

index.css

``` css
.jx_top {
    /*外上边距15px, 底部边框2px 实线 #ffc900做为下面的线，宽度100%，高度35px，行高35px，外下边距5px。*/
    margin-top: 15px;
    border-bottom: 2px solid #ffc900;
    width: 100%;
    height: 35px;
    line-height: 35px;
    margin-bottom: 5px;
}
```



## 案例：黑马旅游首页-国内游

### 目标

![1552209014652](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552209014652.png)

### 步骤

1. 每成2部分，左边4列，右边8列。
2. 左边使用图片guonei_1.jpg
3. 右边每个小格子占4列，一共6个缩略图。

### 代码

#### html

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <!-- 使用最新的内核来解析当前网页-->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!--
    设置视口参数：默认网页宽度等于设备的宽度，初始缩放比1:1
    视口概念：在浏览器中虚拟的一个网页容器，可以随着设备的不同进行缩放
    -->
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>黑马旅游-首页</title>

    <!-- 1. 导入Bootstrap的样式 -->
    <link href="css/bootstrap.css" rel="stylesheet">
    <link rel="stylesheet" href="css/index.css">
</head>
<body>
<!--上部-->
<header class="container-fluid">
    <!--第1行-->
    <div class="row">
        <!--img-responsive 响应式图片-->
        <img src="img/top_banner.jpg" width="100%" class="img-responsive">
    </div>

    <!-- 第2行-->
    <div class="row text-center">
        <div class="col-md-3">
            <img src="img/logo.jpg">
        </div>
        <div class="col-md-6">
            <img src="img/search.png" width="75%">
        </div>
        <div class="col-md-3">
            <img src="img/hotel_tel.png">
        </div>
    </div>

    <!--第3行：导航条-->
    <div class="row">
        <nav class="navbar navbar-default">
            <div class="container-fluid">
                <!-- 商标和折叠按钮 -->
                <div class="navbar-header">
                    <!--折叠按钮-->
                    <button type="button" class="navbar-toggle collapsed" data-toggle="collapse"
                            data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
                        <span class="glyphicon glyphicon-list"></span>
                    </button>
                    <a class="navbar-brand" href="#">
                        <span class="glyphicon glyphicon-piggy-bank"></span>
                    </a>
                </div>

                <!-- 链接 和 表单 -->
                <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">


                    <ul class="nav navbar-nav">
                        <li class="active"><a href="#">国内游</span></a></li>
                        <li><a href="#">出境游</a></li>
                        <li><a href="#">地沟游</a></li>
                        <li><a href="#">印度神游</a></li>

                        <!--下拉菜单-->
                        <li class="dropdown">
                            <!--<span class="caret"></span>表示向下箭头-->
                            <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button"
                               aria-haspopup="true" aria-expanded="false">港澳游<span class="caret"></span></a>
                            <ul class="dropdown-menu">
                                <li><a href="#">澳门威尼斯</a></li>
                                <li><a href="#">Another action</a></li>
                                <li><a href="#">Something else here</a></li>
                                <!--separator 水平分隔线-->
                                <li role="separator" class="divider"></li>
                                <li><a href="#">Separated link</a></li>
                                <li role="separator" class="divider"></li>
                                <li><a href="#">One more separated link</a></li>
                            </ul>
                        </li>
                    </ul>

                    <!--搜索表单-->
                    <form class="navbar-form navbar-right">
                        <div class="form-group">
                            <input type="text" class="form-control" placeholder="输入关键字">
                        </div>
                        <button type="submit" class="btn btn-default">
                            <span class="glyphicon glyphicon-search"></span>
                            搜索
                        </button>
                    </form>

                </div><!-- /.navbar-collapse -->
            </div><!-- /.container-fluid -->
        </nav>
    </div>

    <!--轮播图-->
    <div class="row">
        <div id="carousel-example-generic" class="carousel slide" data-ride="carousel" data-interval="3000">
            <!-- 1. 指示器：指的小圆点 -->
            <ol class="carousel-indicators">
                <!--active表示激活的-->
                <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li>
                <li data-target="#carousel-example-generic" data-slide-to="1"></li>
                <li data-target="#carousel-example-generic" data-slide-to="2"></li>
            </ol>

            <!-- 2. 图片部分 -->
            <div class="carousel-inner" role="listbox">
                <div class="item active">
                    <!--图片-->
                    <img src="img/banner_1.jpg">
                    <!--标题-->
                    <div class="carousel-caption">
                        花木兰
                    </div>
                </div>

                <div class="item">
                    <img src="img/banner_2.jpg">
                    <div class="carousel-caption">
                        毛玻璃
                    </div>
                </div>

                <div class="item">
                    <img src="img/banner_3.jpg">
                    <div class="carousel-caption">
                        10块钱3件
                    </div>
                </div>
            </div>

            <!-- 3. 左右按钮 -->
            <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
                <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
            </a>
            <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
                <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
            </a>
        </div>
    </div>
</header>
<!--主体-->
<main class="container">
    <div class="row">
            <div class="jx_top">
                <span class="glyphicon glyphicon-camera" style="font-size: 25px; color: green;"></span> 黑马精选
            </div>
    </div>

    <div class="row">
        <div class="col-md-3">
            <div class="thumbnail">
                <img src="img/05.jpg">
                <div class="caption">
                    <p>上海直飞三亚5天4晚自由行</p>
                    <p><a href="#" class="btn btn-danger" role="button" style="text-decoration: line-through">
                        原价：&yen;3999</a> <a href="#" class="btn btn-success" role="button">折后价：&yen;1888</a></p>
                </div>
            </div>
        </div>
        <div class="col-md-3">
            <div class="thumbnail">
                <img src="img/05.jpg">
                <div class="caption">
                    <p>上海直飞三亚5天4晚自由行</p>
                    <p><a href="#" class="btn btn-danger" role="button" style="text-decoration: line-through">
                        原价：&yen;3999</a> <a href="#" class="btn btn-success" role="button">折后价：&yen;1888</a></p>
                </div>
            </div>
        </div>
        <div class="col-md-3">
            <div class="thumbnail">
                <img src="img/05.jpg">
                <div class="caption">
                    <p>上海直飞三亚5天4晚自由行</p>
                    <p><a href="#" class="btn btn-danger" role="button" style="text-decoration: line-through">
                        原价：&yen;3999</a> <a href="#" class="btn btn-success" role="button">折后价：&yen;1888</a></p>
                </div>
            </div>
        </div>
        <div class="col-md-3">
            <div class="thumbnail">
                <img src="img/05.jpg">
                <div class="caption">
                    <p>上海直飞三亚5天4晚自由行</p>
                    <p><a href="#" class="btn btn-danger" role="button" style="text-decoration: line-through">
                        原价：&yen;3999</a> <a href="#" class="btn btn-success" role="button">折后价：&yen;1888</a></p>
                </div>
            </div>
        </div>
    </div>

    <div class="row">
        <div class="jx_top">
            <span class="glyphicon glyphicon-flag" style="font-size: 25px; color: blue;"></span> 国内游
        </div>
    </div>

    <div class="row">
        <div class="col-md-4">
            <img src="img/guonei_1.jpg">
        </div>
        <!--2行3列-->
        <div class="col-md-8">
            <div class="col-md-4">
                <div class="thumbnail">
                    <img src="img/07.jpg">
                    <div class="caption">
                        <p>上海直飞三亚5天4晚自由行</p>
                        <p><a href="#" class="btn btn-danger btn-xs" role="button" style="text-decoration: line-through">
                            原价：&yen;3999</a> <a href="#" class="btn btn-success btn-xs" role="button">折后价：&yen;1888</a></p>
                    </div>
                </div>
            </div>

            <div class="col-md-4">
                <div class="thumbnail">
                    <img src="img/07.jpg">
                    <div class="caption">
                        <p>上海直飞三亚5天4晚自由行</p>
                        <p><a href="#" class="btn btn-danger btn-xs" role="button" style="text-decoration: line-through">
                            原价：&yen;3999</a> <a href="#" class="btn btn-success btn-xs" role="button">折后价：&yen;1888</a></p>
                    </div>
                </div>
            </div>

            <div class="col-md-4">
                <div class="thumbnail">
                    <img src="img/07.jpg">
                    <div class="caption">
                        <p>上海直飞三亚5天4晚自由行</p>
                        <p><a href="#" class="btn btn-danger btn-xs" role="button" style="text-decoration: line-through">
                            原价：&yen;3999</a> <a href="#" class="btn btn-success btn-xs" role="button">折后价：&yen;1888</a></p>
                    </div>
                </div>
            </div>

            <div class="col-md-4">
                <div class="thumbnail">
                    <img src="img/07.jpg">
                    <div class="caption">
                        <p>上海直飞三亚5天4晚自由行</p>
                        <p><a href="#" class="btn btn-danger btn-xs" role="button" style="text-decoration: line-through">
                            原价：&yen;3999</a> <a href="#" class="btn btn-success btn-xs" role="button">折后价：&yen;1888</a></p>
                    </div>
                </div>
            </div>

            <div class="col-md-4">
                <div class="thumbnail">
                    <img src="img/07.jpg">
                    <div class="caption">
                        <p>上海直飞三亚5天4晚自由行</p>
                        <p><a href="#" class="btn btn-danger btn-xs" role="button" style="text-decoration: line-through">
                            原价：&yen;3999</a> <a href="#" class="btn btn-success btn-xs" role="button">折后价：&yen;1888</a></p>
                    </div>
                </div>
            </div>

            <div class="col-md-4">
                <div class="thumbnail">
                    <img src="img/07.jpg">
                    <div class="caption">
                        <p>上海直飞三亚5天4晚自由行</p>
                        <p><a href="#" class="btn btn-danger btn-xs" role="button" style="text-decoration: line-through">
                            原价：&yen;3999</a> <a href="#" class="btn btn-success btn-xs" role="button">折后价：&yen;1888</a></p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</main>
<!--脚部-->
<footer class="container-fluid">

</footer>

<!-- 2. 导入jQuery.js文件 (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
<script src="js/jquery-3.2.1.min.js"></script>
<!-- 3. 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
<script src="js/bootstrap.min.js"></script>
</body>
</html>
```



## 案例：黑马旅游首页-脚部

### 目标

最底下2行的效果

![1552209102788](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP正则&bootstrap/1552209102788.png)

### 步骤

1. 使用container-fluid容器

2. 第一行：使用一张图片footer_service.jpg，并设置类名 class="img-responsive"，响应式图片

3. 第二行：自定义样式，写一行文字。样式名company：字体大小12，背景色#ffc900，文字居中对齐，高度与行高为40px

### 代码

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <!-- 使用最新的内核来解析当前网页-->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!--
    设置视口参数：默认网页宽度等于设备的宽度，初始缩放比1:1
    视口概念：在浏览器中虚拟的一个网页容器，可以随着设备的不同进行缩放
    -->
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>黑马旅游-首页</title>

    <!-- 1. 导入Bootstrap的样式 -->
    <link href="css/bootstrap.css" rel="stylesheet">
    <link rel="stylesheet" href="css/index.css">
</head>
<body>
<!--上部-->
<header class="container-fluid">
    <!--第1行-->
    <div class="row">
        <!--img-responsive 响应式图片-->
        <img src="img/top_banner.jpg" width="100%" class="img-responsive">
    </div>

    <!-- 第2行-->
    <div class="row text-center">
        <div class="col-md-3">
            <img src="img/logo.jpg">
        </div>
        <div class="col-md-6">
            <img src="img/search.png" width="75%">
        </div>
        <div class="col-md-3">
            <img src="img/hotel_tel.png">
        </div>
    </div>

    <!--第3行：导航条-->
    <div class="row">
        <nav class="navbar navbar-default">
            <div class="container-fluid">
                <!-- 商标和折叠按钮 -->
                <div class="navbar-header">
                    <!--折叠按钮-->
                    <button type="button" class="navbar-toggle collapsed" data-toggle="collapse"
                            data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
                        <span class="glyphicon glyphicon-list"></span>
                    </button>
                    <a class="navbar-brand" href="#">
                        <span class="glyphicon glyphicon-piggy-bank"></span>
                    </a>
                </div>

                <!-- 链接 和 表单 -->
                <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">


                    <ul class="nav navbar-nav">
                        <li class="active"><a href="#">国内游</span></a></li>
                        <li><a href="#">出境游</a></li>
                        <li><a href="#">地沟游</a></li>
                        <li><a href="#">印度神游</a></li>

                        <!--下拉菜单-->
                        <li class="dropdown">
                            <!--<span class="caret"></span>表示向下箭头-->
                            <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button"
                               aria-haspopup="true" aria-expanded="false">港澳游<span class="caret"></span></a>
                            <ul class="dropdown-menu">
                                <li><a href="#">澳门威尼斯</a></li>
                                <li><a href="#">Another action</a></li>
                                <li><a href="#">Something else here</a></li>
                                <!--separator 水平分隔线-->
                                <li role="separator" class="divider"></li>
                                <li><a href="#">Separated link</a></li>
                                <li role="separator" class="divider"></li>
                                <li><a href="#">One more separated link</a></li>
                            </ul>
                        </li>
                    </ul>

                    <!--搜索表单-->
                    <form class="navbar-form navbar-right">
                        <div class="form-group">
                            <input type="text" class="form-control" placeholder="输入关键字">
                        </div>
                        <button type="submit" class="btn btn-default">
                            <span class="glyphicon glyphicon-search"></span>
                            搜索
                        </button>
                    </form>

                </div><!-- /.navbar-collapse -->
            </div><!-- /.container-fluid -->
        </nav>
    </div>

    <!--轮播图-->
    <div class="row">
        <div id="carousel-example-generic" class="carousel slide" data-ride="carousel" data-interval="3000">
            <!-- 1. 指示器：指的小圆点 -->
            <ol class="carousel-indicators">
                <!--active表示激活的-->
                <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li>
                <li data-target="#carousel-example-generic" data-slide-to="1"></li>
                <li data-target="#carousel-example-generic" data-slide-to="2"></li>
            </ol>

            <!-- 2. 图片部分 -->
            <div class="carousel-inner" role="listbox">
                <div class="item active">
                    <!--图片-->
                    <img src="img/banner_1.jpg">
                    <!--标题-->
                    <div class="carousel-caption">
                        花木兰
                    </div>
                </div>

                <div class="item">
                    <img src="img/banner_2.jpg">
                    <div class="carousel-caption">
                        毛玻璃
                    </div>
                </div>

                <div class="item">
                    <img src="img/banner_3.jpg">
                    <div class="carousel-caption">
                        10块钱3件
                    </div>
                </div>
            </div>

            <!-- 3. 左右按钮 -->
            <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
                <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
            </a>
            <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
                <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
            </a>
        </div>
    </div>
</header>
<!--主体-->
<main class="container">
    <div class="row">
            <div class="jx_top">
                <span class="glyphicon glyphicon-camera" style="font-size: 25px; color: green;"></span> 黑马精选
            </div>
    </div>

    <div class="row">
        <div class="col-md-3">
            <div class="thumbnail">
                <img src="img/05.jpg">
                <div class="caption">
                    <p>上海直飞三亚5天4晚自由行</p>
                    <p><a href="#" class="btn btn-danger" role="button" style="text-decoration: line-through">
                        原价：&yen;3999</a> <a href="#" class="btn btn-success" role="button">折后价：&yen;1888</a></p>
                </div>
            </div>
        </div>
        <div class="col-md-3">
            <div class="thumbnail">
                <img src="img/05.jpg">
                <div class="caption">
                    <p>上海直飞三亚5天4晚自由行</p>
                    <p><a href="#" class="btn btn-danger" role="button" style="text-decoration: line-through">
                        原价：&yen;3999</a> <a href="#" class="btn btn-success" role="button">折后价：&yen;1888</a></p>
                </div>
            </div>
        </div>
        <div class="col-md-3">
            <div class="thumbnail">
                <img src="img/05.jpg">
                <div class="caption">
                    <p>上海直飞三亚5天4晚自由行</p>
                    <p><a href="#" class="btn btn-danger" role="button" style="text-decoration: line-through">
                        原价：&yen;3999</a> <a href="#" class="btn btn-success" role="button">折后价：&yen;1888</a></p>
                </div>
            </div>
        </div>
        <div class="col-md-3">
            <div class="thumbnail">
                <img src="img/05.jpg">
                <div class="caption">
                    <p>上海直飞三亚5天4晚自由行</p>
                    <p><a href="#" class="btn btn-danger" role="button" style="text-decoration: line-through">
                        原价：&yen;3999</a> <a href="#" class="btn btn-success" role="button">折后价：&yen;1888</a></p>
                </div>
            </div>
        </div>
    </div>

    <div class="row">
        <div class="jx_top">
            <span class="glyphicon glyphicon-flag" style="font-size: 25px; color: blue;"></span> 国内游
        </div>
    </div>

    <div class="row">
        <div class="col-md-4 hidden-xs">
            <img src="img/guonei_1.jpg">
        </div>
        <!--2行3列-->
        <div class="col-md-8">
            <div class="col-md-4">
                <div class="thumbnail">
                    <img src="img/07.jpg">
                    <div class="caption">
                        <p>上海直飞三亚5天4晚自由行</p>
                        <p><a href="#" class="btn btn-danger btn-xs" role="button" style="text-decoration: line-through">
                            原价：&yen;3999</a> <a href="#" class="btn btn-success btn-xs" role="button">折后价：&yen;1888</a></p>
                    </div>
                </div>
            </div>

            <div class="col-md-4">
                <div class="thumbnail">
                    <img src="img/07.jpg">
                    <div class="caption">
                        <p>上海直飞三亚5天4晚自由行</p>
                        <p><a href="#" class="btn btn-danger btn-xs" role="button" style="text-decoration: line-through">
                            原价：&yen;3999</a> <a href="#" class="btn btn-success btn-xs" role="button">折后价：&yen;1888</a></p>
                    </div>
                </div>
            </div>

            <div class="col-md-4">
                <div class="thumbnail">
                    <img src="img/07.jpg">
                    <div class="caption">
                        <p>上海直飞三亚5天4晚自由行</p>
                        <p><a href="#" class="btn btn-danger btn-xs" role="button" style="text-decoration: line-through">
                            原价：&yen;3999</a> <a href="#" class="btn btn-success btn-xs" role="button">折后价：&yen;1888</a></p>
                    </div>
                </div>
            </div>

            <div class="col-md-4">
                <div class="thumbnail">
                    <img src="img/07.jpg">
                    <div class="caption">
                        <p>上海直飞三亚5天4晚自由行</p>
                        <p><a href="#" class="btn btn-danger btn-xs" role="button" style="text-decoration: line-through">
                            原价：&yen;3999</a> <a href="#" class="btn btn-success btn-xs" role="button">折后价：&yen;1888</a></p>
                    </div>
                </div>
            </div>

            <div class="col-md-4">
                <div class="thumbnail">
                    <img src="img/07.jpg">
                    <div class="caption">
                        <p>上海直飞三亚5天4晚自由行</p>
                        <p><a href="#" class="btn btn-danger btn-xs" role="button" style="text-decoration: line-through">
                            原价：&yen;3999</a> <a href="#" class="btn btn-success btn-xs" role="button">折后价：&yen;1888</a></p>
                    </div>
                </div>
            </div>

            <div class="col-md-4">
                <div class="thumbnail">
                    <img src="img/07.jpg">
                    <div class="caption">
                        <p>上海直飞三亚5天4晚自由行</p>
                        <p><a href="#" class="btn btn-danger btn-xs" role="button" style="text-decoration: line-through">
                            原价：&yen;3999</a> <a href="#" class="btn btn-success btn-xs" role="button">折后价：&yen;1888</a></p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</main>
<!--脚部-->
<footer class="container-fluid">
    <div class="row" style="margin-top: 5px">
        <img src="img/footer_service.png" width="100%" class="img-responsive">
    </div>

    <div class="row company">
        江苏传智播客教育科技股份有限公司 版权所有Copyright 2006-2018, All Rights Reserved 苏ICP备16007882
    </div>
</footer>

<!-- 2. 导入jQuery.js文件 (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
<script src="js/jquery-3.2.1.min.js"></script>
<!-- 3. 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
<script src="js/bootstrap.min.js"></script>
</body>
</html>
```



# 学习总结

1. 能够使用正则表达式进行表单的验证

   1. 创建正则表达式方法：

      ```
      new RegExp("正则表达式","匹配模式")
      /正则表达式/匹配模式
      ```

      

   2. 正则表达式的测试方法：

      ```
      boolean test("字符串") 匹配就返回true
      ```

      

   

2. 能够创建bootstrap的模板

   1. 复制三个目录css, js, fonts到项目中
   2. 复制jquery.js到项目js目录下
   3. 复制"起步->基本模板"到网页
   4. 修改网页中css和js和地址为本地文件

   

3. 能够使用bootstrap的两种布局容器

   | 两种容器的类样式名 | 说明                         |
   | ------------------ | ---------------------------- |
   | container          | 在不同的设备上显示不同的宽度 |
   | container-fluid    | 在所有的设备上显示100%的宽度 |

4. 能够理解bootstrap的响应式布局的特点

   ```
   同一个网页在不同的设备上自动适配分辩率或设备的宽度
   ```

   

5. 能够查询文档创建bootstrap的按钮、表格、表单等常用组件

   

6. 能够理解bootstrap的栅格系统

   | 类样式名             | 作用                      |
   | -------------------- | ------------------------- |
   | .container           | 容器 table                |
   | .container-fluid     | 容器 table                |
   | .row                 | 一行，一行最多12个格子 tr |
   | .col-xs-n            | 微型设备上1列占n格        |
   | .col-sm-n            | 小型                      |
   | .col-md-n            | 中型                      |
   | .col-lg-n            | 大型                      |
   | .hidden-lg/md/sm/xs  | 在指定的设备上隐藏        |
   | .visible-lg/md/sm/xs | 只在指定的设备上显示      |

7. 能够查询文档使用bootstrap的导航条

8. 能够查询文档使用bootstrap的轮播图

9. 能够利用Bootstrap完成黑马旅游的首页