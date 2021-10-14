# 《jQuery框架-上课笔记》

# 回顾

## 过滤器的生命周期方法

| Filter接口中的方法                                           | 作用和执行次数            |
| ------------------------------------------------------------ | ------------------------- |
| void init(FilterConfig filterConfig)                         | 服务器开启就执行，执行1次 |
| void  doFilter(ServletRequest request, <br />ServletResponse response, FilterChain chain) | 每次请求都执行            |
| public void destroy()                                        | 服务器关闭就执行1次       |



## 过滤器的拦截类型

| 过滤类型 | 作用           |
| -------- | -------------- |
| REQUEST  | 对请求进行拦截 |
| INCLUDE  | 对包含进行拦截 |
| FORWARD  | 对转发进行拦截 |



## FilterChain接口中的方法 

| FilterChain接口中的方法                                    | 参数的作用                                                   |
| ---------------------------------------------------------- | ------------------------------------------------------------ |
| doFilter(ServletRequest request, ServletResponse response) | 作用：执行它就放行，不执行就拦截   <br />ServletRequest： 请求   <br />ServletResponse: 响应 |



## 过滤器链的执行顺序

1. web.xml配置的方式：按配置前后顺序
2. @WebFilter注解的方式： 按类名的字符串的大小



## ServletContextListener监听器

| 接口中的方法                                       | 功能                     |
| -------------------------------------------------- | ------------------------ |
| void contextDestroyed(ServletContextEvent   sce)   | 上下文域销毁，服务器关闭 |
| void contextInitialized(ServletContextEvent   sce) | 上下文域创建，服务器启动 |



## Web高级阶段

1. jQuery框架：前端内容(方法比较多)
2. jQuery和AJAX：异步访问服务器
3. Redis：非关系型数据库
4. Linux：学习2天(命令)
5. Maven基础：对jar包的管理



# 学习目标

1.	<font color="red">能够使用jQuery的基本选择器</font>
2.	能够使用jQuery的层级选择器
3.	<font color="red">能够使用jQuery的DOM操作的方法</font>
4.	能够使用jQuery的绑定与解绑方法
5.	能够使用jQuery对象的遍历方法
6.	能够使用jQuery全局的遍历方法
7.	能够完成隔行换色
8.	能够完成案例：抽奖程序



# 学习内容

## 什么是jQuery

### 目标

1. 什么是jQuery

2. 它有什么作用

### 什么是jQuery

jQuery是一个JavaScript框架，可以简化JavaScript的开发，提高开发效率，降低开发成本，降低开发难度。

jQuery这个框架就是使用JavaScript编写的，本质上还是JavaScript。

### 学习了哪些框架？

- mybatis 持久层框架，简化访问数据库。
- bootstrap 简化CSS的开发

### 为什么要使用jQuery框架

![1572745114365](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JQuery/1572745114365.png)



### jQuery框架特点

1. 轻量级：框架很小，占用资源少。免费开源。
2. 兼容性：几乎兼容所有的主流浏览器
3. 插件：本身功能强大之外，还支持插件扩展机制。很多插件的功能也非常强大。
4. 宗旨：write less do more

### 小结

jQuery开发有什么好处？

```
1. 开发一套代码就可以运行在所有的浏览器				
2. 所有浏览器运行的效果是一样的				
3. 降低开发难度和工作量，提高开发效率，降低开发成本	
```



## jQuery的导入与使用

### 目标

在页面上使用jQuery框架

### 官网

http://www.jquery.com

![1553152806464](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JQuery/1553152806464.png)

版本的选择：

![1572745609284](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JQuery/1572745609284.png) 

### 文档网站

![1572745710760](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JQuery/1572745710760.png) 



### 案例：导入jQuery并测试是否成功

#### 步骤

1. 复制jquery.js文件到js目录
2. 使用\<script>标签导入jquery.js文件

#### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>jQuery的导入</title>
</head>
<body>
<script type="text/javascript" src="js/jquery-3.3.1.js"></script>

<script type="text/javascript">
    /*
    $ = jQuery
    这个函数相当于 window.onload = function() ，表示页面加载完毕以后自动执行
     */
    $(function () {   //这就是它的入口函数
        alert("你好，jQuery 1");
    });

    $(function () {  
        alert("你好，jQuery 2");
    });

    /*
    第二个会覆盖第一个，两个方法只会执行1次
    window.onload = function () {
        alert("你好, JavaScript 1");
    }

    window.onload = function () {
        alert("你好, JavaScript 2");
    }
     */
</script>
</body>
</html>
```

#### 导入失败的情况

![1572746160502](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JQuery/1572746160502.png) 

### 小结

$(function())的作用是什么？

```
jQuery的入口函数，相当于window.onload
```



与window.onload的区别

```
只会执行1次，而jquery每个入口都会执行
```



## jQuery对象与js对象之间的转换

### JS对象与jQuery对象的区别

- JS对象：以前通过document.getElementById()这个方法得到的对象，称为JS对象。JS对象只能调用原生的JS中方法和属性。

- jQuery对象：通过jQuery选择器选中的元素，称为jQuery对象，它可以调用jQuery框架中方法，功能更加强大。

### 为什么要转换

如果JS对象需要调用JQ对象中的方法，需要将JS对象转成JQ对象。反之亦然。

### 转换语法

| 操作                                            | 方法                       |
| ----------------------------------------------- | -------------------------- |
| 将JS对象-->jQuery对象                           | $(JS对象)                  |
| 将jQuery对象--> JS对象 (JQ对象本质是一个JS数组) | JQ对象[0] 或 JQ对象.get(0) |

### JS与JQ转换的演示案例：

#### 需求

页面上有一个文本框，文本框中有值：传智播客。分别使用JS和jQuery的方法得到文件框中的值。

#### 效果

![1553153641477](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JQuery/1553153641477.png)

#### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JS对象与JQ对象转换</title>
    <script type="text/javascript" src="js/jquery-3.3.1.js"></script>
</head>
<body>
<h2>JS对象与JQ对象的转换</h2>
<input type="text" id="company" value="传智播客">
<input type="button" id="b1" value="JS得到值">
<input type="button" id="b2" value="JQ得到值">
<script type="text/javascript">
    /*
    使用JS对象的方式
     */
    document.getElementById("b1").onclick = function () {
        //得到文本框的JS对象
        var jsObject = document.getElementById("company");
        //调用JS对象的属性
        //var val = jsObject.value;
        
        // JS --> JQ对象
        var jqObject = $(jsObject);
        //调用JQ方法
        var val = jqObject.val();
        //输出
        alert(val);
    };

    /**
     * 使用JQ对象的方式
     */
    $("#b2").click(function () {   //事件处理函数
        //得到文本框JQ对象
        var jqObject = $("#company");
        //调用JQ对象的方法
        //var val = jqObject.val();

        //将JQ对象转成JS对象
        var jsObject = jqObject.get(0);   //或jqObject[0]
        //调用JS的属性
        var val = jsObject.value;

        //输出
        alert(val);
    });
</script>
</body>
</html>
```

### 小结

| 操作                   | 方法                       |
| ---------------------- | -------------------------- |
| 将JS对象-->jQuery对象  | $(JS对象)                  |
| 将jQuery对象--> JS对象 | JQ对象[0] 或 JQ对象.get(0) |



## 选择器：基本选择器

### 目标

基本选择器的使用

### 选择器的作用

如果要操作网页中各种元素，首先要选中这些元素。jQuery中的选择器功能强大，选择方式灵活多样。

### jQuery常用的选择器有如下：

1. 基本选择器
2. 层级选择器
3. 属性选择器
4. 基本过滤选择器
5. 属性过滤选择器

### 选择器的语法

所有的选择器外面都要写上$("选择器")，如果选中的是多个元素。jQuery中大部分方法都可以直接操作一个集合

| 基本选择器 | 语法   |
| ---------- | ------ |
| ID选择器   | \#ID   |
| 类选择器   | .类名  |
| 标签选择器 | 标签名 |

### 示例：基本选择器的使用

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
<head>
    <title>基本选择器</title>
    <meta http-equiv="content-type" content="text/html; charset=UTF-8">
    <script type="text/javascript" src="js/jquery-3.3.1.js"></script>
    <style type="text/css">
        div, span {
            width: 180px;
            height: 180px;
            margin: 20px;
            background: #9999CC;
            border: #000 1px solid;
            float: left;
            font-size: 17px;
            font-family: Roman;
        }

        div .mini {
            width: 50px;
            height: 50px;
            background: #CC66FF;
            border: #000 1px solid;
            font-size: 12px;
            font-family: Roman;
        }

        div .mini01 {
            width: 50px;
            height: 50px;
            background: #CC66FF;
            border: #000 1px solid;
            font-size: 12px;
            font-family: Roman;
        }
    </style>
</head>
<body>
<input type="button" value="改变 id 为 one 的元素的背景色为 红色" id="b1"/>
<input type="button" value=" 改变元素名为 <div> 的所有元素的背景色为 红色" id="b2"/>
<input type="button" value=" 改变 class 为 mini 的所有元素的背景色为 红色" id="b3"/>

<h1>有一种奇迹叫坚持</h1>
<div id="one">
    id为one
</div>

<div id="two" class="mini">
    id为two class是 mini
    <div class="mini">class是 mini</div>
</div>

<div class="one">
    class是 one
    <div class="mini">class是 mini</div>
    <div class="mini">class是 mini</div>
</div>
<div class="one">
    class是 one
    <div class="mini01">class是 mini01</div>
    <div class="mini">class是 mini</div>
</div>

<div id="mover">
    div 动画
</div>

<span class="spanone">class为spanone的span元素</span>
<span class="mini">class为mini的span元素</span>
</body>

<script type="text/javascript">
    // 1) 改变 id 为one的元素的背景色为红色
    $("#b1").click(function () {
        //css("样式名","样式值") 相当于:   对象.style.样式名=样式值
        //通过id选择器选中元素
        $("#one").css("background-color", "red");
    });

    // 2) 改变元素名为 <div> 的所有元素的背景色为 红色。
    $("#b2").click(function () {
        //通过标签选择器选中元素, 选择器选中一个集合，方法本身就可以操作多个元素。
        $("div").css("backgroundColor", "red");
    });

    // 3) 改变 class 为 mini 的所有元素的背景色为 红色
    $("#b3").click(function () {
        $(".mini").css("background", "red");
    });
</script>
</html>

```

### 小结

| 基本选择器 | 语法   |
| ---------- | ------ |
| ID选择器   | \#ID   |
| 类选择器   | .类名  |
| 标签选择器 | 标签名 |



## 选择器：层级选择器

### 目标

层级选择器的使用

### 语法

| 层级选择器                                                   | 语法 |
| ------------------------------------------------------------ | ---- |
| 获取A元素内部所有B元素 (B是A的子孙元素)                      | A B  |
| 获得A元素下面的所有B子元素 (B一定是A的子元素)                | A>B  |
| 获得A元素同级，下一个B元素 (A与B是兄弟的关系，B是A的后一个元素) | A+B  |
| 获取A元素后面所有同级B元素 (B是A后面所有的兄弟，多个元素)    | A~B  |

### 演示案例

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
<head>
    <title>层次选择器</title>
    <meta http-equiv="content-type" content="text/html; charset=UTF-8">
    <script type="text/javascript" src="js/jquery-3.3.1.js"></script>
    <style type="text/css">
        div, span {
            width: 180px;
            height: 180px;
            margin: 20px;
            background: #9999CC;
            border: #000 1px solid;
            float: left;
            font-size: 17px;
            font-family: Roman;
        }

        div .mini {
            width: 50px;
            height: 50px;
            background: #CC66FF;
            border: #000 1px solid;
            font-size: 12px;
            font-family: Roman;
        }

        div .mini01 {
            width: 50px;
            height: 50px;
            background: #CC66FF;
            border: #000 1px solid;
            font-size: 12px;
            font-family: Roman;
        }
    </style>
</head>

<body>
<input type="button" value="改变 <body> 内所有 <div> 的背景色为红色" id="b1"/>
<input type="button" value="改变 <body> 内子 <div> 的背景色为 红色" id="b2"/>
<input type="button" value="改变 id为two的下一个div兄弟元素的背景色为红色" id="b3"/>
<input type="button" value="改变id为two的后面同级兄弟div元素的背景色为红色" id="b4"/>
<h1>有一种奇迹叫坚持</h1>
<div id="one">
    id为one
</div>

<div id="two" class="mini">
    id为two class是 mini
    <div class="mini">class是 mini</div>
</div>

<div class="one">
    class是 one
    <div class="mini">class是 mini</div>
    <div class="mini">class是 mini</div>
</div>
<div class="one">
    class是 one
    <div class="mini01">class是 mini01</div>
    <div class="mini">class是 mini</div>
</div>

<span class="spanone">span</span>
</body>

<script type="text/javascript">
    //1) 改变 <body> 内所有 <div> 的背景色为红色
    $("#b1").click(function () {
        $("body div").css("backgroundColor", "yellow");
    });

    //2) 改变 <body> 内子 <div> 的背景色为 红色
    $("#b2").click(function () {
        $("body>div").css("backgroundColor", "yellow");
    });

    //3) 改变 id为two的下一个div兄弟元素的背景色为红色
    $("#b3").click(function () {
        $("#two+div").css("backgroundColor", "yellow");
    });
	
	//4) 改变id为two的后面同级兄弟div元素的背景色为红色
	 $("#b4").click(function () {
         $("#two~div").css("backgroundColor", "yellow");
     });
</script>
</html>
```

### 小结

| 层级选择器                 | 语法 |
| -------------------------- | ---- |
| 获取A元素内部所有B元素     | A B  |
| 获得A元素下面的所有B子元素 | A>B  |
| 获得A元素同级，下一个B元素 | A+B  |
| 获取A元素后面所有同级B元素 | A~B  |



## 选择器：属性选择器

### 目标

属性选择器的使用

### 语法

| 属性选择器                | 语法         |
| ------------------------- | ------------ |
| 获得有属性名的元素        | [属性名]     |
| 获得属性名 等于   值 元素 | [属性名=值]  |
| 获取属性名不等于值的元素  | [属性名!=值] |
| 获取属性名包含值的元素    | [属性名*=值] |

### 案例演示

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
<head>
    <title>属性过滤选择器</title>
    <meta http-equiv="content-type" content="text/html; charset=UTF-8">
    <script type="text/javascript" src="js/jquery-3.3.1.js"></script>
    <style type="text/css">
        div, span {
            width: 180px;
            height: 180px;
            margin: 20px;
            background: #9999CC;
            border: #000 1px solid;
            float: left;
            font-size: 17px;
            font-family: Roman;
        }

        div .mini {
            width: 50px;
            height: 50px;
            background: #CC66FF;
            border: #000 1px solid;
            font-size: 12px;
            font-family: Roman;
        }

        div .mini01 {
            width: 50px;
            height: 50px;
            background: #CC66FF;
            border: #000 1px solid;
            font-size: 12px;
            font-family: Roman;
        }

        div.visible {
            display: none;
        }
    </style>
</head>

<body>

<input type="button" value="含有属性title 的div元素背景色为红色" id="b1"/>
<input type="button" value="属性title值等于test的div元素背景色为红色" id="b2"/>
<input type="button" value="属性title值不等于test的div元素(没有属性title的也将被选中)背景色为红色" id="b3"/>
<input type="button" value="属性title值包含est的div元素背景色为红色" id="b4"/>
<hr/>

<div id="one">
    id为one div
</div>

<div id="two" class="mini" title="test">
    id为two class是 mini div title="test"
    <div class="mini">class是 mini</div>
</div>

<div class="visible">
    class是 one
    <div class="mini">class是 mini</div>
    <div class="mini">class是 mini</div>
</div>
<div class="one" title="test02">
    class是 one title="test02"
    <div class="mini01">class是 mini01</div>
    <div class="mini" style="margin-top:0px;">class是 mini</div>
</div>

<div class="one">
</div>

<div id="mover" title="best">
    id="mover" title="best"
</div>
</body>

<script type="text/javascript">
    //1) 含有属性title 的div元素背景色为红色
    $("#b1").click(function () {
        $("div[title]").css("backgroundColor","red");
    });

    //2) 属性title值等于test的div元素背景色为红色
    $("#b2").click(function () {
        $("div[title=test]").css("backgroundColor","red");
    });

    //3) 属性title值不等于test的div元素(没有title属性的也将被选中)背景色为红色
    $("#b3").click(function () {
        $("div[title!=test]").css("backgroundColor","red");
    });

	//4) 属性title值包含tes的div元素背景色为红色
    $("#b4").click(function () {
        $("div[title*=tes]").css("backgroundColor","red");
    });
</script>
</html>

```

### 小结

| 属性选择器                | 语法         |
| ------------------------- | ------------ |
| 获得有属性名的元素        | [属性名]     |
| 获得属性名 等于   值 元素 | [属性名=值]  |
| 获取属性名不等于值的元素  | [属性名!=值] |
| 获取属性名包含值的元素    | [属性名*=值] |



## 选择器：基本过滤选择器

### 目标

基本过滤选择器的使用

### 什么是过滤选择器

在已经选中的元素中再次进行筛选，得到的元素

### 语法

| 基本过滤选择器                 | 语法   |
| ------------------------------ | ------ |
| 获得选择的元素中的第一个元素   | :first |
| 获得选择的元素中的最后一个元素 | :last  |
| 不包括指定内容的元素           | :not() |
| 偶数，从 0 开始计数            | :even  |
| 奇数，从 0 开始计数            | :odd   |
| 等于第几个                     | :eq()  |
| 大于第n个，不含第index个       | :gt()  |
| 小于第n个，不含第index个       | :lt()  |



### 演示案例

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <title>基本过滤选择器</title>
    <meta http-equiv="content-type" content="text/html; charset=UTF-8">
	<script type="text/javascript" src="js/jquery-3.3.1.js"></script>
	<style type="text/css">
			table {
				margin: auto;
				border-collapse: collapse;
				width: 525px;
			}
			
			tr {
				height: 30px;
				text-align: center;
			}
			
			.girl {
				width: 260px;
				height: 260px;
				border: 1px solid gray;
				float: left;
			}
	 </style>
	</head>
  <body>
		 <input type="button" value="第一行的背景色为浅灰色"  id="b1"/>
		 <input type="button" value="最后一行的背景色为浅绿色"  id="b2"/>
		 <input type="button" value="除第1行和最后1行的其它行背景色为浅黄色"  id="b3"/>
		 <input type="button" value="索引值为偶数的行背景色为浅粉色"  id="b4"/>
		 <input type="button" value="索引值为奇数的行背景色为aquamarine色"  id="b5"/>
		 <input type="button" value="索引值大于 3 的tr元素的背景色为oldlace色"  id="b6"/>
		 <input type="button" value="索引值等于 3 的tr元素的背景色为antiquewhite"  id="b7"/>
		 <input type="button" value="索引值为小于 3 的tr元素背景色为yellowgreen"  id="b8"/>
		<hr />
		<div style="width: 525px; margin: auto;">
		<table border="1" align="center">
				<caption><h3>学生信息列表</h3></caption>
				<tr>
					<th>学号</th>
					<th>姓名</th>
					<th>性别</th>
					<th>年龄</th>
					<th>家庭住址</th>
					<th>成绩</th>
				</tr>
				<tr>
					<td>1001</td>
					<td>孙悟空</td>
					<td>男</td>
					<td>72</td>
					<td>花果山</td>
					<td>90</td>
				</tr>
				<tr>
					<td>1002</td>
					<td>猪八戒</td>
					<td>男</td>
					<td>36</td>
					<td>高老庄</td>
					<td>78</td>
				</tr>
				<tr>
					<td>2002</td>
					<td>沙僧</td>
					<td>男</td>
					<td>30</td>
					<td>流沙河</td>
					<td>67</td>
				</tr>
				<tr>
					<td>3000</td>
					<td>唐三藏</td>
					<td>男</td>
					<td>26</td>
					<td>东土</td>
					<td>87</td>
				</tr>
				<tr>
					<td>4000</td>
					<td>白骨精</td>
					<td>女</td>
					<td>18</td>
					<td>白骨洞</td>
					<td>96</td>
				</tr>
				<tr>
					<td>5000</td>
					<td>蜘蛛精</td>
					<td>女</td>
					<td>17</td>
					<td>盘丝洞</td>
					<td>95</td>
				</tr>
				<tr>
					<td>总成绩</td>
					<td colspan="5">3045</td>
				</tr>
		</table>
		</div>
		<br />
	</body>
	
	<script type="text/javascript">
	//1) 改变第一行的背景色为浅灰色
    $("#b1").click(function () {
        $("tr:first").css("background", "lightgreen");
    });

	//2) 改变最后一行的背景色为浅绿色
    $("#b2").click(function () {
        $("tr:last").css("background", "lightgreen");
    });

	//3) 改变除第1行和最后1行的其它行背景色为浅黄色
    $("#b3").click(function () {
        $("tr:not(:first):not(:last)").css("background", "lightgreen");
    });

	//4) 改变索引值为偶数的行背景色为浅粉色，下标从0开始
    $("#b4").click(function () {
        $("tr:even").css("background", "lightgreen");
    });

	//5) 改变索引值为奇数的行背景色为aquamarine色
    $("#b5").click(function () {
        $("tr:odd").css("background", "lightgreen");
    });

	//6) 改变索引值大于 3 的tr元素的背景色为oldlace色
    $("#b6").click(function () {
        $("tr:gt(3)").css("background", "lightgreen");
    });

	//7) 改变索引值等于 3 的tr元素的背景色为antiquewhite
    $("#b7").click(function () {
        $("tr:eq(3)").css("background", "lightgreen");
    });

	//8) 改变索引值为小于 3 的tr元素背景色为yellowgreen
    $("#b8").click(function () {
        $("tr:lt(3)").css("background", "lightgreen");
    });
	</script>
</html>

```

### 小结

| 基本过滤选择器                 | 语法   |
| ------------------------------ | ------ |
| 获得选择的元素中的第一个元素   | :first |
| 获得选择的元素中的最后一个元素 | :last  |
| 不包括指定内容的元素           | :not() |
| 偶数，从 0 开始计数            | :even  |
| 奇数，从 0 开始计数            | :odd   |
| 等于第几个                     | :eq()  |
| 大于第n个，不含第index个       | :gt()  |
| 小于第n个，不含第index个       | :lt()  |



## 选择器：属性过滤选择器

### 目标

属性过滤选择器

### 语法

- 注：属性过滤选择器用于表单中元素

| 表单属性选择器                         | 语法      |
| -------------------------------------- | --------- |
| 可用                                   | :enabled  |
| 不可用                                 | :disabled |
| 选中（单选radio ，多选checkbox）       | :checked  |
| 选择（下列列表\<select>中的\<option>） | :selected |

### 示例

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
	<head>
		<title>表单属性过滤选择器</title>
		<meta http-equiv="content-type" content="text/html; charset=UTF-8">
		<script type="text/javascript" src="js/jquery-3.3.1.js"></script>
		<style type="text/css">
			div,
			span {
				width: 180px;
				height: 180px;
				margin: 20px;
				background: #9999CC;
				border: #000 1px solid;
				float: left;
				font-size: 17px;
				font-family: Roman;
			}
			
			div .mini {
				width: 50px;
				height: 50px;
				background: #CC66FF;
				border: #000 1px solid;
				font-size: 12px;
				font-family: Roman;
			}
			
			div .mini01 {
				width: 50px;
				height: 50px;
				background: #CC66FF;
				border: #000 1px solid;
				font-size: 12px;
				font-family: Roman;
			}
		</style>
	</head>

	<body>
		<input type="button" value="val() 方法改变表单内可用文本框元素的值" id="b1" />
		<input type="button" value="val() 方法改变表单内不可用 <input> 元素的值" id="b2" />
		<input type="button" value="length 属性获取(单选和多选)选框选中的个数" id="b3" />
		<input type="button" value="length 属性获取下拉列表选中的个数" id="b4" />
		<br><br>

		<input type="text" value="不可用值1" disabled="disabled">
		<input type="text" value="可用值1">
		<input type="text" value="不可用值2" disabled="disabled">
		<input type="text" value="可用值2">

		<br><br>
		<input type="checkbox" name="items" value="美容">美容
		<input type="checkbox" name="items" value="IT">IT
		<input type="checkbox" name="items" value="金融">金融
		<input type="checkbox" name="items" value="管理">管理

		<br><br>

		<input type="radio" name="sex" value="男">男
		<input type="radio" name="sex" value="女">女

		<br><br>
        <!--multiple表示多选-->
		<select name="job" id="job" multiple="multiple" size=4>
			<option>程序员</option>
            <option>中级程序员</option>
            <option>高级程序员</option>
            <option>系统分析师</option>
		</select>
		<select name="edu" id="edu">
			<option>本科</option>
			<option>博士</option>
			<option>硕士</option>
			<option>大专</option>
		</select>

	</body>

	<script type="text/javascript">
        //1) val() 方法改变表单内可用文本框 <input> 元素的值
        $("#b1").click(function () {
            /*
            value修改属性值
            val() 方法既可设置值set，也可以获取值get
            带参数就是设置值，没有参数获取值
             */
            $("input[type=text]:enabled").val("传智播客");
        });

        //2) val() 方法改变表单内不可用 <input> 元素的值
        $("#b2").click(function () {
            $("input[type=text]:disabled").val("传智播客");
        });

        //3) length 属性获取选框选checked中的个数
        $("#b3").click(function () {
            //得到数组个数
            alert($("input:checked").length);
        });

        //4) length 属性获取下拉列表选中的个数
        $("#b4").click(function () {
            alert($("option:selected").length);
        });
	</script>
</html>
```

### 小结

| 表单属性选择器                          | 语法      |
| --------------------------------------- | --------- |
| 选中（单选radio ，多选checkbox）        | :checked  |
| 选择（下列列表 \<select>中的\<option>） | :selected |



## DOM操作的方法：html()，text()，val()

### 目标

在JQ中如何操作innerHTML、innerText、value属性(input元素)

### jQuery的DOM操作方法

操作方法的特点：如果是一个参数，表示设置值，如果没有参数表示获取值。

既可以设置值，又可以获取值。

### 语法

```
html()：相当于innerHTML
text(): 相当于innerText
val()：相当于value
```

### 演示案例

#### 需求

获取"张三"，获得"标题标签"的文本，获得mydiv的标签体内容

#### 效果

![1553159246973](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JQuery/1553159246973.png)

#### 代码

```html
<!DOCTYPE html>
<html>
<head>
    <title>html和text和val</title>
    <meta http-equiv="content-type" content="text/html; charset=UTF-8">
    <script type="text/javascript" src="js/jquery-3.3.1.js"></script>
</head>
<body>
<input type="button" id="b1" value="设置值value"/>
<input type="button" id="b2" value="获取值value"/>
<input type="button" id="b3" value="设置html"/>
<input type="button" id="b4" value="获取值html"/>
<input type="button" id="b5" value="设置text"/>
<input type="button" id="b6" value="获取text"/>
<hr/>

<input id="myinput" type="text" name="username" value="张三"/><br/>
<div id="mydiv"><p><a href="#">标题标签</a></p></div>
</body>
<script type="text/javascript">
    $("#b1").click(function () {
        $("#myinput").val("李四");
    });
    
    $("#b2").click(function () {
        alert($("#myinput").val());
    });

    //设置html
    $("#b3").click(function () {
        $("#mydiv").html("<h2 style='color:red'>这是标题</h2>");
    });

    //获取值html
    $("#b4").click(function () {
        alert( $("#mydiv").html());
    });

    //设置text
    $("#b5").click(function () {
        $("#mydiv").text("<h2 style='color:red'>这是标题</h2>");
    });

    //获取text
    $("#b6").click(function () {
        alert( $("#mydiv").text());
    });
</script>
</html>
```

### 小结

| 操作方法 | 功能            |
| -------- | --------------- |
| html()   | 相当于innerHTML |
| text()   | 相当于innerText |
| val()    | 相当于value     |



## DOM操作的方法：与属性有关的方法

### 目标

学习attr()、removeAttr()、prop()、removeProp()方法的使用

### 语法

```
attr() 如果是2个参数就是设置属性，1个参数就是获取属性 attribute
prop() 功能同上，区别是它通常用于属性值是布尔类型的情况，即属性值为true或false  property
removeAttr(),removeProp() 删除属性名和值
```

​	

![1553159503659](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JQuery/1553159503659.png)

### 代码

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
<head>
    <title>获取属性</title>
    <meta http-equiv="content-type" content="text/html; charset=UTF-8">
    <script type="text/javascript" src="js/jquery-3.3.1.js"></script>
</head>

<body>
<input type="button" id="b1" value="获取北京节点的name属性值"/>
<input type="button" id="b2" value="设置北京节点的name属性的值为dabeijing"/>
<input type="button" id="b3" value="新增北京节点的discription属性 属性值是didu"/>
<input type="button" id="b4" value="删除北京节点的name属性"/>
<input type="button" id="b5" value="获得hobby的选中状态"/>

<ul>
    <li id="bj" name="beijing">北京</li>
    <li id="tj" name="tianjin">天津</li>
</ul>
爱好：<input type="checkbox" id="hobby" value="游泳"/>游泳

</body>

<script type="text/javascript">
    //获取北京节点的name属性值
    $("#b1").click(function () {
        var attr = $("#bj").attr("name");
        alert(attr);
    });

    //设置北京节点的name属性的值为"雾都"
    $("#b2").click(function () {
        $("#bj").attr("name", "雾都");   //如果存在就是修改
    });

    //新增北京节点的discription属性 属性值是didu
    $("#b3").click(function () {
        $("#bj").attr("discription","帝都");
    });

    //删除北京节点的name属性
    $("#b4").click(function () {
        $("#bj").removeAttr("name");
    });

    //获得hobby的选中状态
    $("#b5").click(function () {
        alert($("#hobby").prop("checked"));  //得到checked属性
    });
</script>

</html>
```

### 小结

| 方法名                     | 功能描述                   |
| -------------------------- | -------------------------- |
| attr()                     | 设置或获取属性的值         |
| prop()                     | 设置或获取布尔类型的属性值 |
| removeAttr() /removeProp() | 删除属性                   |



## DOM操作的方法：与样式有关的方法

### 目标

学习与样式和类有关的方法

### 回顾

在JS中操作CSS的方法：

```javascript
对象.style.样式名=样式值   (修改行内样式，一条代码修改一个样式)
```

```javascript
对象.className=类名  (修改类样式，一条代码可以修改多个样式)
```

### 与类名有关的方法

```
addClass() 添加一个类名，因为一个class="btn btn-default"
removeClass() 删除一个类名，只是删除类名，不会删除class属性名
toggleClass()  切换，如果有类名就删除，没有这个类名就添加
```

### 与样式有关的方法

![1553159755613](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JQuery/1553159755613.png) 

### 示例

``` html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <title>样式操作</title>
    <meta http-equiv="content-type" content="text/html; charset=UTF-8">
	<script type="text/javascript" src="js/jquery-3.3.1.js"></script>
	<style type="text/css">
		   .one{
			    width: 200px;
			    height: 140px;
			    margin: 20px;
			    background: red;
			    border: #000 1px solid;
				float:left;
			    font-size: 17px;
			    font-family:Roman;
			}
		
		 	div{
			    width: 140px;
			    height: 140px;
			    margin: 20px;
			    background: #9999CC;
			    border: #000 1px solid;
				float:left;
			    font-size: 17px;
			    font-family:Roman;
			}
			
			/*待用的样式*/
			.second{
				width: 222px;
			    height: 220px;
			    margin: 20px;
			    background: yellow;
			    border: pink 3px dotted;
				float:left;
			    font-size: 22px;
			    font-family:Roman;
			}
	 </style>
	</head>
	 
	<body>
		 <input type="button" value="采用属性增加样式(改变id=one的样式)"  id="b1"/>
		 <input type="button" value=" addClass"  id="b2"/>
		 <input type="button" value="removeClass"  id="b3"/>
		 <input type="button" value=" 切换样式"  id="b4"/>
		 <input type="button" value=" 通过css()获得id为one背景颜色"  id="b5"/>
 		 <input type="button" value=" 通过css()设置id为one背景颜色为绿色"  id="b6"/>
		 <hr/>
		
		 <h1>有一种奇迹叫坚持</h1>
		 <h2>自信源于努力</h2>
		 
	     <div id="one">
	    	 id为one 
		 </div>
		
	</body>
	
	<script type="text/javascript">
		// 采用属性增加样式(改变id=one的样式)，样式名为second
        $("#b1").click(function () {
            $("#one").attr("class", "second");
        });

		// addClass
        $("#b2").click(function () {
            $("#one").addClass("second");
        });

		// removeClass
        $("#b3").click(function () {
            $("#one").removeClass("second");
        });
        
		// 切换样式
        $("#b4").click(function () {
            $("#one").toggleClass("second");
        });
        
		// 通过css()获得id为one背景颜色
        $("#b5").click(function () {
            alert($("#one").css("background-color"));
        });
        
 		// 通过css()设置id为one背景颜色为绿色
        $("#b6").click(function () {
            $("#one").css("background-color", "red");
        });
	</script>
</html>
```

### 小结

| 方法名             | 功能         |
| ------------------ | ------------ |
| addClass(类样式名) | 添加类样式   |
| css(样式名)        | 添加行内样式 |



## 上午的回顾

### JQ对象与JS对象转换

```
JS -> JQ对象：$(JS对象)
JQ -> JS对象：JQ对象[0] 或 JQ对象.get(0)
```



### 基本选择器

```
#ID
.类名
标签名
```



### 层级选择器

```
A B
A>B
A+B
A~B
```



### 属性选择器

```
[属性名]
[属性名=值]
[属性名!=值]
[属性名*=值]
```



### 基本过滤选择器

```
:first
:last
:not()
:even
:odd
:eq()
:gt()
:lt()
```



### 属性过滤选择器

```
:enabled
:disabled
:checked
:selected
```



### DOM操作方法

```
html() 
text()
val() 
```



### 属性有关的方法

```
attr() 
prop() 布尔类型属性
removeAttr()或removeProp() 删除属性
```



### 样式有关的方法

```
addClass()
removeClass()
toggleClass()
css()
```



## DOM操作的方法：元素的创建和添加

### 目标

使用JQ对象的方法创建和插入元素

### 回顾

JS中创建元素：document.createElement("元素名")

JS添加到DOM树：父元素.appendChild(子元素)  添加成最后一个子元素

### 语法

```
$("标签内容")  创建一个元素，一个完整的标签包含所有的属性和子元素
父元素.append(子元素) 添加成最后一个子元素(父子)
父元素.prepend(子元素) 添加成第一个子元素(父子)
旧元素.before(新元素) 将新元素添加到当前旧元素前面，两者是兄弟关系
旧元素.after(新元素) 将新元素添加到当前元素后面，两者是兄弟关系
```

### 演示代码

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
<head>
    <title>内部插入脚本</title>
    <meta http-equiv="content-type" content="text/html; charset=UTF-8">
    <script type="text/javascript" src="js/jquery-3.3.1.js"></script>
</head>

<body>
<input type="button" id="b1" value="将反恐放置到city的后面"/>
<input type="button" id="b2" value="将反恐放置到city的最前面"/>
<input type="button" id="b3" value="将反恐放在天津前面"/>
<input type="button" id="b4" value="将反恐放在天津后面"/>
<input type="button" id="b5" value="创建一个广州的节点"/>
<hr/>

<ol id="city">
    <li id="bj" name="beijing">北京</li>
    <li id="tj" name="tianjin">天津</li>
    <li id="cq" name="chongqing">重庆</li>
</ol>

<ul id="game">
    <li id="fk" name="fankong">反恐</li>
    <li id="xj" name="xingji">星际</li>
</ul>
</body>

<script type="text/javascript">
    //将反恐放置到city的后面
    $("#b1").click(function () {
        //append有剪切的功能
        //$("#city").append($("#fk"));
        //将子元素添加到父元素的最后一个
        $("#fk").appendTo($("#city"));
    });

    //将反恐放置到city的最前面
    $("#b2").click(function () {
        //$("#city").prepend($("#fk"));
        //剪切
        //$("#fk").prependTo($("#city"));
        //复制
        $("#fk").clone().prependTo($("#city"));
    });

    //将反恐放在天津前面
    $("#b3").click(function () {
        $("#tj").before($("#fk"));
    });

    //将反恐放在天津后面
    $("#b4").click(function () {
        $("#tj").after($("#fk"));
    });

    //创建一个广州的节点，加到城市中：<li id="gz" name="guangzhou">广州</li>
    $("#b5").click(function () {
        //var li = $("<li id=\"gz\" name=\"guangzhou\">广州</li>");
        //$("#city").append(li);
        //直接写字符串也可以创建
        $("#city").append("<li id=\"gz\" name=\"guangzhou\">广州</li>");
    });
</script>
</html>
```



### 小结

| 方法名                | 描述                 |
| --------------------- | -------------------- |
| $(标签的全部内容)     | 创建一个元素         |
| 父元素.append(子元素) | 添加成最后一个子元素 |
| 元素.before(元素)     | 添加到当前元素的前面 |



## DOM操作方法：删除元素

### 目标

与删除元素有关的方法

### 语法

```
元素.remove()  删除当前元素(自杀)
父元素.empty() 清空当前元素下所有的子元素，自己还在(他杀)
```



### 示例

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <title>删除节点</title>
    <meta http-equiv="content-type" content="text/html; charset=UTF-8">
	<script type="text/javascript" src="js/jquery-3.3.1.js"></script>
	</head>
	 
	<body>
	   <input type="button" id="b1" value="从city中删除北京" />
       <input type="button" id="b2" value="删除city中所有的子节点" />
	   <hr/>
		 <ol id="city">
		 	 <li id="bj" name="beijing">北京</li>
			 <li id="tj" name="tianjin">天津</li>
			 <li id="cq" name="chongqing">重庆</li>
		 </ol>
	</body>
	
	<script type="text/javascript">
	   //从city中删除<li id="bj" name="beijing">北京</li>节点
       $("#b1").click(function () {
           $("#bj").remove();
       });

	   //删除city中所有的子节点，观察city本身有没有删除
       $("#b2").click(function () {
           $("#city").empty();
       });
	</script>
   
</html>
```



### 小结

| 方法名         | 功能             |
| -------------- | ---------------- |
| 元素.remove()  | 删除自己         |
| 父元素.empty() | 清空所有的子元素 |



## jQuery的事件使用

### 目标

1. 一个元素同时使用多个事件

2. 多个事件的链式写法

### 事件处理函数的命名

jQuery中事件的特点：所有的事件都是一个方法，没有on开头。如：元素.click()

既可以做事件处理函数，也可以主动调用，激活事件。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>jQuery事件的特点</title>
    <script src="js/jquery-3.3.1.js"></script>
</head>
<body>
<input type="button" value="按钮1" id="b1" >
<input type="button" value="按钮2" id="b2" >

<script type="text/javascript">
    //1. 事件处理函数
    $("#b1").click(function () {
        alert("这是按钮1");
    });

    //2. 激活按钮1的点击事件
    $("#b2").click(function () {
        $("#b1").click();  //直接调用方法
    });
</script>
</body>
</html>
```



### jQuery中常用的事件

| 事件方法    | 功能                                      |
| ----------- | ----------------------------------------- |
| blur()      | 失去焦点                                  |
| change()    | 改变事件                                  |
| click()     | 单击                                      |
| dblclick()  | 双击                                      |
| focus()     | 得到焦点                                  |
| keydown()   | 按下键                                    |
| keyup()     | 松开键                                    |
| mouseover() | 鼠标移上                                  |
| mouseout()  | 鼠标移出                                  |
| submit()    | 提交                                      |
| hover()     | 鼠标悬浮，相当mouseover和mouseout两个事件 |



### 事件方法使用示例

#### 需求

1. 文本框得到焦点和失去焦点分别显示不同的背景色
2. 松开按键，将字母转成大写，再显示在文本框中
3. 使用链式写法实现相同的功能

#### 效果

![1553157150803](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JQuery/1553157150803.png) 

#### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>多个事件的写法</title>
    <script type="text/javascript" src="js/jquery-3.3.1.js"></script>
</head>
<body>
用户名：
<input type="text" id="t1" value="" />
</body>
<script type="text/javascript">
    /*
    //得到焦点
    $("#t1").focus(function () {
        //this是JS对象，必须转成JQ对象
        $(this).css("background-color", "yellow");
    });

    //失去焦点
    $("#t1").blur(function () {
        $(this).css("background-color", "green");
    });
    
    //松开键盘
    $("#t1").keyup(function (event) {
        //得到文本框的值，转成大写赋值给自己
        $(this).val($(this).val().toUpperCase());
        //键盘上每个按钮都有一个键盘码，如：a和A是65 (键盘上每个按钮都有一个编码)
        //alert(event.keyCode);
    });
     */

    //支持链式写法：如果一个元素有多个事件可以写在一起
    $("#t1").focus(function () {
        $(this).css("background-color", "blue");
    }).blur(function () {
        $(this).css("background-color", "red");
    }).keyup(function (event) {
        $(this).val($(this).val().toUpperCase());
    });
</script>
</html>
```

### 小结

| 事件方法 | 功能     |
| -------- | -------- |
| blur()   | 失去焦点 |
| focus()  | 得到焦点 |
| keyup()  | 松开键盘 |



## jQuery中鼠标悬停事件

### 目标

学习jQuery中鼠标悬停事件

### 语法

![1553157391112](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JQuery/1553157391112.png)

### 演示案例

#### 需求

1. 创建一个DIV，指定宽和高为500px，边框2px,灰色，实线。
2. 当鼠标移入div背景图片发生变化，移出DIV背景图片变成另一张。
3. 使用hover，同时支持两个事件

#### 效果

![1553157494335](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JQuery/1553157494335.png) 

#### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style type="text/css">
        #girl {
            width: 550px;
            height: 550px;
            border: 2px solid gray;
        }
    </style>
    <script type="text/javascript" src="js/jquery-3.3.1.js"></script>
</head>
<body>
<div id="girl">
<h2>我是女生</h2>
</div>

<script type="text/javascript">
    //鼠标移上，鼠标移出事件
    $("#girl").hover(function () {
        $(this).css("background-image","url(img/g1.jpg)");
    }, function () {
        $(this).css("background-image","url(img/g3.jpg)");
    })
</script>
</body>
</html>
```

### 小结

鼠标悬停使用哪个事件？两个参数的作用是什么？

```
hover(鼠标移上，鼠标移出)
```



## 动态绑定事件：绑定(on)与解绑(off)

### 目标

学习事件的动态绑定与解绑

### 什么是事件绑定

 某个元素一开始没有事件处理函数，在程序执行过程，动态的给元素添加事件处理函数，也可以去掉事件处理的功能。

### 绑定与解绑语法

| 事件绑定语法                                  | 说明                               |
| --------------------------------------------- | ---------------------------------- |
| JQ对象.on("事件名", 事件处理函数);            | 在执行过程中对元素动态绑定事件     |
| JQ对象.on("事件名","子选择器", 事件处理函数); | 可以对后面新创建的元素动态绑定事件 |

| 事件解绑语法                  | 说明               |
| ----------------------------- | ------------------ |
| JQ对象.off("事件名1 事件名2") | 解绑一个或多个事件 |
| JQ对象.off()                  | 解绑全部事件       |

 

### 示例：绑定事件

#### 需求

有四个按钮，b1按钮使用以前的事件写法，点击弹出信息。b2按钮没有事件。b3按钮点击给b2按钮绑定点击事件。然后点击b2，查看输出的信息。b4按钮点击解除b2按钮的点击事件。

#### 效果

![1563197414736](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JQuery/1563197414736.png)                                                  

#### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>事件绑定和解绑</title>
    <script src="js/jquery-3.3.1.js"></script>
</head>
<body>
<input type="button" value="按钮1" id="b1">
<input type="button" value="按钮2" id="b2">
<hr/>
<input type="button" value="绑定" id="b3">
<input type="button" value="解绑" id="b4">
<script type="text/javascript">
     //按钮1是以前的方式编写事件处理函数
    $("#b1").click(function () {
        alert("按钮1被点击了");
    });

    //按钮2在程序执行过程中绑定或解绑事件
    $("#b3").click(function () {
        //给b2添加事件，参数：事件，事件处理函数
        $("#b2").on("click", function () {
            alert("按钮2被点击了");
        });
    });

    $("#b4").click(function () {
        //给b2解绑事件
        $("#b2").off("click");   //不带参数解绑所有的
    });
</script>
</body>
</html>
```

### 小结

```
on("事件名",function())
off("事件名 事件名")  off()
```



## 案例：新创建的元素绑定事件

### 目标

如何给新创建的元素绑定事件

### 案例需求

1. 有一个城市的有序列表，每个li有点击事件，点击以后弹出城市的名字
2. 在下面的文本框中输入新的城市，点按钮添加到li列表中，会发现新加的城市没有点击事件。
3. 使用上面的动态绑定的方法，可以让新加的城市支持点击事件。

### 案例效果

![1563197537646](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JQuery/1563197537646.png)                                                          

### 案例代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>添加城市列表</title>
    <script type="text/javascript" src="js/jquery-3.3.1.js"></script>
    <style>
        #city li:hover {
            background-color: lawngreen;
        }
    </style>
</head>
<body>
<ol id="city">
    <li>广州</li>
    <li>上海</li>
    <li>北京</li>
</ol>
城市名：<input type="text" id="t1" placeholder="请输入城市名">
<input type="button" id="b1" value="添加城市列表">

<script type="text/javascript">
    //参数：事件名 子选择器 事件处理函数
    $("#city").on("click","li", function () {
        //this表示点击的那个li
       alert($(this).text());
    });
    
    //按钮的点击事件
    $("#b1").click(function () {
        var val = $("#t1").val();  //得到文本框的值
        $("#city").append("<li>" + val +"</li>");  //添加
        //清空文本框的值
        $("#t1").val("");
    });
</script>
</body>
</html>
```

### 小结

```
对后面新增的元素绑定事件使用：
JQ对象.on("事件名","子选择器",事件处理函数)
```

 

## jQuery循环遍历的几种方式

### 目标

1. jQuery对象遍历的2种方式
2. JS中遍历集合的循环

### 遍历的三种方式

- 前2种是jQuery框架自带的方式，必须要在jQuery框架中才可以使用，要导入jQuery.js文件才可以使用
- 第3种是JS中功能，可以在任何地方使用

###  遍历的示例

#### 需求

对一个select中的所有option元素进行遍历，并且输出它的HTML内容

#### 效果

![1569586922787](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JQuery/1569586922787.png)

#### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>遍历元素</title>
    <script type="text/javascript" src="js/jquery-3.3.1.js"></script>
</head>
<body>
<select name="city" id="city">
    <option>广州</option>
    <option>深圳</option>
    <option>东莞</option>
</select>
<input type="button" id="b1" value="jq对象的方法遍历" />
<input type="button" id="b2" value="jq对象的全局方法" />
<input type="button" id="b3" value="for-of方法遍历" />
<script type="text/javascript">
    //得到所有的option元素，这是一个集合，无论使用哪种遍历方式，每个元素都是JS对象。
    var options = $("#city>option");   //这是一个jQuery对象
    /*
    jQuery的对象.each(function(索引, 元素))
    索引：这是第几个遍历的元素，从0开始
    元素：元素本身，这是一个JS对象。
     */
    $("#b1").click(function () {
        options.each(function (index, element) {
            //alert("索引：" + index + "，元素：" + element.innerText);   //这是JS对象
            alert("索引：" + index + "，元素：" + $(element).text());  //转成JQ对象，调用它的方法
        });
    });

    /**
     * JQ的全局方法：参数1是要遍历的集合，第2个参数同上(索引，元素)
     */
    $("#b2").click(function () {
        $.each(options, function (index, element) {
            alert("索引：" + index + "，元素：" + element.innerText);   //这是JS对象
        });
    });

    /**
     * 与jQuery无关
     * 语法：for(var 变量名 of 集合或数组)
     */
    $("#b3").click(function () {
        for(var element of options) {
            alert("元素：" + element.innerText);   //这是JS对象
        }
    });
</script>
</body>
</html>
```

### 小结

| jQuery遍历的三种方式                         |
| -------------------------------------------- |
| JQ对象.each(function(index,element))         |
| $.each(集合或数组, function(index, element)) |
| for(var 变量 of 集合或数组)                  |
| 无论使用上面哪种方式，每个元素都是JS对象     |



## 案例：表格隔行换色与全选全不选

### 目标

1. 实现隔行变色的效果
2. 实现全选全不选的效果

### 效果

![1553156460136](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JQuery/1553156460136.png)

### 步骤

#### 隔行变色

1. 页面加载完毕，得到所有的tr。
2. 使用基本过滤选择器，除了第0行，设置偶数行背景色为lightgreen
3. 使用基本过滤选择器，除了第0行，设置奇数行背景色为lightyellow

#### 全选全不选

1. 给最上面的id=all的复选框添加点击事件
2. 使用属性选择器选中所有item=name的复选框，设置它的checked属性与id=all的复选框的checked属性相同。

### 代码

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>隔行换色/全选全不选</title>
    <style type="text/css">
        table {
            border-collapse: collapse;
        }

        tr {
            height: 35px;
            text-align: center;
        }

        a:link {
            text-decoration: none;
        }
    </style>
    <script type="text/javascript" src="js/jquery-3.3.1.js"></script>
    <script type="text/javascript">
        //页面加载完毕以后执行
        $(function () {
            //大于0的偶数行
            $("tr:gt(0):even").css("background-color", "lightgreen");
            //大于0的奇数行
            $("tr:gt(0):odd").css("background-color", "lightpink");
            
            //最上面复选框的点击事件
            $("#all").click(function () {
                //得到最上面选中状态
                //得到下面所有的复选框，checked=true
                $("input[name=item]").prop("checked", $("#all").prop("checked"));
            });
        });
    </script>
</head>
<body>
<table id="tab1" border="1" width="700" align="center">
    <tr style="background-color: #999999;">
        <td><input type="checkbox" id="all"></td>
        <td>分类ID</td>
        <td>分类名称</td>
        <td>分类描述</td>
        <td>操作</td>
    </tr>
    <tr>
        <td><input type="checkbox" name="item"></td>
        <td>1</td>
        <td>手机数码</td>
        <td>手机数码类商品</td>
        <td><a href="">修改</a>|<a href="">删除</a></td>
    </tr>
    <tr>
        <td><input type="checkbox" name="item"></td>
        <td>2</td>
        <td>电脑办公</td>
        <td>电脑办公类商品</td>
        <td><a href="">修改</a>|<a href="">删除</a></td>
    </tr>
    <tr>
        <td><input type="checkbox" name="item"></td>
        <td>3</td>
        <td>鞋靴箱包</td>
        <td>鞋靴箱包类商品</td>
        <td><a href="">修改</a>|<a href="">删除</a></td>
    </tr>
    <tr>
        <td><input type="checkbox" name="item"></td>
        <td>4</td>
        <td>家居饰品</td>
        <td>家居饰品类商品</td>
        <td><a href="">修改</a>|<a href="">删除</a></td>
    </tr>
</table>
</body>
</html>
```

### 小结

1. 选中大于0的tr的偶数行的选择器怎么写？

   ```
   $("tr:gt(0):even")
   ```

   

2. 设置某个复选框选中的方法是什么？

   ```
   JQ对象.prop("checked", true)
   ```

   



## 案例：添加QQ表情

### 目标

使用上面学习的方法，实现QQ表情的案例

### 效果

![1563198014652](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JQuery/1563198014652.png)                                                  

###  分析

1. 每张表情都是一张gif图片，"请发言"放在一个p标签中
2. 通过层级选择器选中所有的img对象，添加点击事件
3. 使用this得到当前图片对象，把当前的小表情克隆，再追加到p元素中

### 代码

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8"/>
    <title>QQ表情选择</title>
    <style type="text/css">
        * {
            margin: 0;
            padding: 0;
            list-style: none;
        }

        .emoji {
            margin: 50px;
        }

        ul {
            overflow: hidden;
        }

        li {
            float: left;
            width: 48px;
            height: 48px;
            cursor: pointer;
        }

        .emoji img {
            cursor: pointer;
        }
    </style>
    <script src="js/jquery-3.3.1.js"></script>
    <script>
        $(function () {
            //给.emoji ul img 元素添加点击事件
            $(".emoji ul img").click(function () {
                //对自己克隆，添加成p的最后一个子元素
                $(this).clone().appendTo(".word");
            });
        });
    </script>
</head>
<body>
<div class="emoji">
    <ul>
        <li><img src="img/01.gif"/></li>
        <li><img src="img/02.gif"/></li>
        <li><img src="img/03.gif"/></li>
        <li><img src="img/04.gif"/></li>
        <li><img src="img/05.gif"/></li>
        <li><img src="img/06.gif"/></li>
        <li><img src="img/07.gif"/></li>
        <li><img src="img/08.gif"/></li>
        <li><img src="img/09.gif"/></li>
        <li><img src="img/10.gif"/></li>
        <li><img src="img/11.gif"/></li>
        <li><img src="img/12.gif"/></li>
    </ul>
    <p class="word">
        <strong>请发言：</strong>
        <img src="img/12.gif"/>
    </p>
</div>
</body>
</html>
```

### 小结

```
$(this).clone().appendTo(".word");  //子元素.appendTo(父元素)
```



## 案例：实现购物车

### 目标

使用今天学习的内容制作一个购物车案例，实现全选全不选的功能

### 需求

1. 实现添加商品，如果商品名为空，提示：商品名不能为空。如果不为空，则添加成功一行，清空文本框的内容，图片使用固定一张。
2. 实现删除一行商品的功能，删除前弹出确认对话框。

### 效果

![1563197758057](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JQuery/1563197758057.png)                                                          

### 代码

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>购物车的实现</title>
    <style type="text/css">
        table {
            width: 400px;
            border-collapse: collapse;
            margin: auto;
        }

        td, th {
            text-align: center;
            height: 30px;
        }

        .container {
            width: 400px;
            margin: auto;
            text-align: right;
        }

        img {
            width: 100px;
            height: 100px;
        }

        th {
            background-color: lightgray;
        }

        tr:hover {
            background-color: lightyellow;
        }
    </style>
    <script type="text/javascript" src="js/jquery-3.3.1.js"></script>
    <script type="text/javascript">
        //添加1行
        function addRow() {
            //1.得到文本框的值
            var pname = $("#pname").val().trim();   //去掉前后的空格
            if (pname == "") {
                alert("请输入商品名字");
                //得到焦点点
                $("#pname").focus();
                return;
            }
            //2. 创建一个tr
            var tr = "<tr><td><img src=\"img/girl.jpg\" /></td><td>" + pname + "</td>" +
                "<td><input type=\"button\" value=\"删除\" onclick=\"deleteRow(this)\"></td></tr>";
            //3. 添加到tbody中
            $("#cart").append(tr);
            //4. 清空文本框
            $("#pname").val("");
        }

        //删除一行
        function deleteRow(btn) {
            //按钮对象是一个JS对象，调用jQuery的方法转成JQ对象
            if (confirm("真的要删除这一行吗?")) {
                // 按钮 -> td -> tr -> 删除
                $(btn).parent().parent().remove();
            }
        }
    </script>
</head>

<body>
<div class="container">
    <table border="1">
        <tbody id="cart">
        <tr>
            <th>
                图片
            </th>
            <th>
                商品名
            </th>
            <th>
                操作
            </th>
        </tr>
        <tr>
            <td><img src="img/sx.jpg"/></td>
            <td>
                三星Note7
            </td>
            <td>
                <!--this表示当前按钮-->
                <input type="button" value="删除" onclick="deleteRow(this)">
            </td>
        </tr>
        <tbody>
    </table>
    <br/>
    商品名：
    <input type="text" id="pname" value=""/>&nbsp;
    <input type="button" value="添加到购物车" onclick="addRow()"/>
</div>
</body>
</html>
```

### 小结

```
添加：创建一个tr，再使用append()添加1行
删除：parent()得到父元素， remove()删除自己
```



## 案例：抽奖程序

### 目标

1. 当用户点击开始按钮时，小像框中的像片快速切换。
2. 当用户点击停止按钮时，小像框中的像片停止切换，大像框中淡入显示与小像框相同的像片。

### 效果

![1553160427040](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JQuery/1553160427040.png)

### 实现思路

1. 将所有的图片文件名保存在一个数组中，定义三个全局变量：总计数num，数组长度total，计时器timer

2. 一开始让停止按钮不可用

3. 点开始按钮，点击后让开始按钮不可用，停止按钮可用。

4. 每过50毫秒调用一次切换函数，不停的替换小图片中的src地址为数组中的下一张图片。

5. 在重复调用的方法体中，每替换一次src属性，图片计数总数加1，再计算数组下标。

6. 点结束按钮，让结束按钮不可用，让开始按钮可用。

7. 清除计时器，将小图片中的地址赋值给大图片的地址，先隐藏，再淡入fadeIn()显示图片。

### 代码实现

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>抽奖程序</title>
    <style type="text/css">
        #small {
            border: 1px solid blueviolet;
            width: 75px;
            height: 75px;
            margin-bottom: 20px;
        }

        #smallPhoto {
            width: 75px;
            height: 75px;
        }

        #big {
            border: 2px solid orangered;
            width: 500px;
            height: 500px;
            position: absolute;
            left: 300px;
            top: 10px
        }

        #bigPhoto {
            width: 500px;
            height: 500px;
        }

        #btnStart {
            width: 100px;
            height: 100px;
            font-size: 22px;
        }

        #btnStop {
            width: 100px;
            height: 100px;
            font-size: 22px;
        }
    </style>
    <script type="text/javascript" src="js/jquery-3.3.1.js"></script>

</head>

<body>

<!-- 小像框 -->
<div id="small">
    <img id="smallPhoto" src="img/man00.jpg"/>
</div>

<!-- 大像框 -->
<div id="big">
    <img id="bigPhoto" src="img/man00.jpg"/>
</div>

<!-- 开始按钮 -->
<input id="btnStart" type="button" value="点击开始">

<!-- 停止按钮 -->
<input id="btnStop" type="button" value="点击停止" disabled="disabled">


<script type="text/javascript">
    //准备一个数组，用户的图片的路径
    var imgs = [
        "img/man00.jpg",
        "img/man01.jpg",
        "img/man02.jpg",
        "img/man03.jpg",
        "img/man04.jpg",
        "img/man05.jpg",
        "img/man06.jpg"
    ];

    var index = 0;  //计数到哪一张了
    var timer;  //计时器

    //开始按钮的点击事件
    $("#btnStart").click(function () {
        //开始按钮不可用，停止按钮可用
        $("#btnStart").prop("disabled", true);
        $("#btnStop").prop("disabled", false);
        //每过50毫秒换一张图片，修改小图片的src属性
        timer = setInterval(function() {
            $("#smallPhoto").attr("src", imgs[index++]);
            //如果index等于6，index设置为0
            if(index == 7) {
                index = 0;
            }
        }, 50);
    });

    //停止按钮
    $("#btnStop").click(function () {
        //开始按钮可用，停止按钮不可用
        $("#btnStart").prop("disabled", false);
        $("#btnStop").prop("disabled", true);
        //清除计时器
        clearInterval(timer);
        //将大图片的src设置为小图片的src
        $("#bigPhoto").attr("src", $("#smallPhoto").attr("src"));
        //设置效果
        $("#bigPhoto").hide();  //隐藏元素
        $("#bigPhoto").fadeIn(3000);    //动画
    });
</script>
</body>
</html>
```

### 小结

| 案例中用到的方法 | 作用                           |
| ---------------- | ------------------------------ |
| setInterval()    | 每隔一段时间调用一次指定的函数 |
| clearInterval()  | 清除计时器                     |
| attr()/prop()    | 设置属性                       |
| hide()           | 隐藏                           |
| fadeIn()         | 淡入                           |



# 学习总结

1. 能够使用jQuery的基本选择器

   | 基本选择器 | 语法   |
   | ---------- | ------ |
   | ID选择器   | \#ID   |
   | 类选择器   | .类名  |
   | 标签选择器 | 标签名 |

2. 能够使用jQuery的层级选择器

   | 层级选择器                 | 语法 |
   | -------------------------- | ---- |
   | 获得A元素内部的所有的B元素 | A B  |
   | 获得A元素下面的所有B子元素 | A>B  |
   | 获得A元素同级，下一个B元素 | A+B  |

3. 能够使用jQuery的DOM操作的方法

   | jQuery中的方法  | 作用                 |
   | --------------- | -------------------- |
   | html()          | innerHTML            |
   | text()          | innerText            |
   | val()           | value                |
   | attr()          | 操作属性             |
   | prop()          | boolean属性          |
   | addClass()      | 添加类样式           |
   | removeClass()   | 删除类样式           |
   | $(标签全部内容) | 创建一个新的元素     |
   | append()        | 添加成最后一个子元素 |
   | before()        | 添加到当前元素的前面 |
   | after()         | 添加到当前元素的后面 |
   | remove()        | 删除自己             |

   

4. 能够使用jQuery的绑定与解绑方法

   | 事件绑定                           | 语法         |
   | ---------------------------------- | ------------ |
   | JQ对象.on("事件名", function() {}) | 动态绑定事件 |

   | 事件解绑                      | 语法         |
   | ----------------------------- | ------------ |
   | JQ对象.off("事件名1 事件名2") | 解绑多个事件 |
   | JQ对象.off()                  | 解绑所有的   |

   

5. 能够使用jQuery对象的遍历方法

   | JQ对象.each(function(index,element))                         |
   | ------------------------------------------------------------ |
   | JQ对象：要遍历的集合<br />index: 索引<br />element: 每个元素是JS对象 |

6. 能够使用jQuery全局的遍历方法

   | $.each(数组或集合,   function(index,element))                |
   | ------------------------------------------------------------ |
   | 数组或集合： 要遍历的集合<br />index: 索引<br />element: 每个元素 |

7. 能够完成隔行换色

   ```
   :even和:odd
   ```

   

8. 能够完成案例：抽奖程序