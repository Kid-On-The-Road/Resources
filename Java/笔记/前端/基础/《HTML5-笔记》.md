# 《HTML5-笔记》

# 回顾

前面6天时间学习数据库

1. mysql学了2天
2. JDBC学了1天
3. mybatis学习了3天

# 前端学习的内容

学习4天(仅仅只是开始，后期还会进一步学习)

1. HTML(在代码中去记忆)
2. CSS+JavaScript基础
3. JavaScript高级(难点，建议提前预习)
4. Bootstrap(前端框架，简化HTML和CSS开发)



# 学习目标

注：重点用红色标出，复习的时候多花时间，其它的听懂就可以了。

1. 能够使用idea创建html文档
2. 文本标签
   1. 能够使用h1~h6、hr、p、br 等与文本有关的标签
   2. 能够使用有序列表ol-li和无序列表ul-li显示列表内容
   3. 能够使用图片img标签把图片显示在页面中
   4. 能够使用超链接a标签跳转到一个新页面
   5. 能够使用table、tr、td标签定义表格
3. 能够制作黑马旅游网的首页
4. 表单容器
   1. 能够使用表单form标签创建表单容器
   2. 能够使用表单中常用的input标签创建输入项
   3. 能够使用表单select标签定义下拉选择输入项
   4. 能够使用表单textarea标签定义文本域



# 学习内容

## HTML的概述

### 目标

​	HTML的概念是什么，它是用来做什么的?

### 概念

​	HTML(Hyper Text Markup Language) 超文本标记语言，俗称：网页。今天学习制作网页

1. 超文本：不同于普通的文本，比普通文本功能更强。可以设置字体，颜色，点文字可以跳转到其它的页面，还有可以图片，声音，视频。

2. 标记语言：整个网页由标签(标记)组成，不同于编程的语言，前后没有逻辑。

   ![1571448327538](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1571448327538.png) 

3. 纠错：如果网页上有错误，浏览器有纠错的功能，就算有错误也不会有提示。

### 网页的运行方式图解

![1571448570658](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1571448570658.png)



### 小结

1. 什么是HTML？

   超文本标记语言

2. 网页运行在哪里？

   运行在浏览器客户端



## HTML5的介绍

### 目标

​	什么是HTML5，它有什么特点？

### HTML5的作用

#### 什么是HTML5

就是HTML的第5个版本，它的出现主要是为了移动设备使用。如：手机，平板等。以前的HTML只适合在电脑上运行。HTML5一共用了8年的时间才确定下来。

但一些早期的浏览器并不支持HTML5，比如：IE9以下版本

#### 支持的浏览器 

​	目前支持Html5的浏览器包括Firefox（火狐浏览器），IE9及其更高版本，Chrome（谷歌浏览器），Safari，Opera等。不同的浏览器之间是有差异的，同一个网页在不同的浏览器上运行结果可能不同。

![1551669532678](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551669532678.png)

### HTML5的作品

![1551669555295](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551669555295.png)

### 小结

​	什么是HTML5，它有什么特点？

```
HTML的第5个版本，用了8年时间研制
为了移动设备使用网页
```





## 网页的基本结构、使用idea创建网页

### 目标

​	如何创建HTML？

### 三层架构

1. 表示层：HTML 直接面向客户，对前端的要求没有那么高，一般公司有专门的前端制作人员。
2. 业务层
3. 持久层：mysql mybatis

### 常见的HTML编辑器

1. HBuilder

   ![1551669763877](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551669763877.png)

2. Adobe Dreamweaver CS

   ![1551669807940](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551669807940.png)

3. SublimeText

   ![1551669828420](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551669828420.png)

4. Visual Studio Code 

   微软公司第一次向开发者们提供了一款真正的跨平台编辑器。

   ![1551769940609](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551769940609.png)

5. NotePad

   ![1551669847102](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551669847102.png)

### 使用记事本创建

```html
<!-- 根元素 -->
<html>
	<!-- 头部 -->
	<head>
		<title>网页标题</title>
	</head>
	<!-- 主体：网页内容 -->
	<body>
		<h1 style="color:red">Hello World</h1>
	</body>
</html>
```



### 使用IntelliJ IDEA创建html

![1571449881007](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1571449881007.png) 

1. 创建静态Web工程

   ![1551670035481](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551670035481.png)

2. 指定工程名和保存位置

   ![1551670125978](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551670125978.png)

3. 创建HTML文件，选择html5的版本

   ![1551670139700](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551670139700.png)

4. 创建HTML文件

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <!--  网页的编码，建议使用UTF-8-->
       <meta charset="UTF-8">
       <title>第一个网页</title>
   </head>
   <body bgcolor="green">
       这是我的第一个网页！
   </body>
   </html>
   ```

5. 点右上角一排浏览器按钮运行，idea会使用内置的服务器在指定的浏览器上运行。

   ![1551670171151](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551670171151.png)

6. 在浏览器上运行的结果

   ![1551670186424](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551670186424.png)

   

### 访问地址

![1571450128452](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1571450128452.png) 

### 模块目录结构

![1571450196178](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1571450196178.png) 

### 小结

| 标签名 | 作用                                      |
| ------ | ----------------------------------------- |
| html   | 网页的根元素                              |
| head   | 作用：网页的头部，子标签title：网页的标题 |
| body   | 网页的主体内容<br />bgcolor属性：背景色   |
| 注释   | 格式： \<!-- -- ><br />嵌套：不能嵌套     |



## 文本标签的学习

### 目标

1.  标签的分类
2.  文本标签的学习

### HTML标签的分类

| 有否有主体   | 格式                                          |
| ------------ | --------------------------------------------- |
| 有主体标签   | 标签体内包含了其它的内容：子标签，文本等      |
| 没有主体标签 | 标签只有一行，没有主体内容，如：\<br/> \<hr/> |

| 是否换行 | 特点                                           |
| -------- | ---------------------------------------------- |
| 块标签   | 每个标签在网页上单独占一行，本身有换行的功能。 |
| 内联标签 | 标签没有换行的功能                             |

### 文本标签介绍

![1551670393595](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551670393595.png)

![1551670419113](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551670419113.png)

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <!--  网页的编码，建议使用UTF-8-->
    <meta charset="UTF-8">
    <title>第一个网页</title>
</head>
<body>
<!--
自带换行的功能：块级标签
h1~h6：六级标题，1级标题最大，6级标题最小，标题自带换行和加粗
    属性：align 对齐方式 center left right
-->
<h1 align="center">这是我的第一个网页！</h1>
<h2 align="right">这是我的第一个网页！</h2>
<h3>这是我的第一个网页！</h3>
<h4>这是我的第一个网页！</h4>
<h5>这是我的第一个网页！</h5>
<h6>这是我的第一个网页！</h6>


<!--hr 水平线：块级标签，没有主体的，默认是居中
    color: 颜色
    size: 粗细，单位是像素
    width: 宽度
 -->
<hr color="red" size="5" width="500"/>

<!--
没有换行功能：内联标签，在H5中已经淘汰，今天还要使用
face：设置字体
size: 1-7 字号，字号越大，字越大
color: 颜色

b标签和strong标签的功能一样：都可以加粗
i标签：斜体
br 换行
-->
<font face="楷体" size="7" color="blue">这是不换行的</font>
<font face="楷体" size="7" color="blue"><b>这是不换行的</b></font> <br/>
<font face="楷体" size="7" color="blue"><strong>这是不换行的</strong></font>
<font face="楷体" size="7" color="blue"><b><i>这是不换行的</i></b></font>

<!--
p标签：分成几个段落。效果：段落之间有间隔，首行没有缩进
1. 选中文字
2. 按ctrl+alt+t 在文字前后添加标记
3. 如果要使用空格，使用实体字符 &nbsp; 半角空格。以后使用CSS还有更好的方法

title属性：当鼠标移上去的时候，显示提示的文字信息
-->
<p title="你那里下雪了吗">&nbsp;&nbsp;&nbsp;&nbsp;同学，还在浏览网页吗？
    同学，还在浏览网页吗？同学，还在浏览网页吗？同学，还在浏览网页吗？
    我们老师可以根据你的个人情况和要求推荐一个适合你的课程，你可以先了解下，考虑好再决定是否学习也可以！</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;同学，还在浏览网页吗？我们老师可以根据你的个人情况和要求推荐一个适合你的课程，你可以先了解下，考虑好再决定是否学习也可以！</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;同学，还在浏览网页吗？我们老师可以根据你的个人情况和要求推荐一个适合你的课程，你可以先了解下，考虑好再决定是否学习也可以！</p>
</body>
</html>
```



### 小结

| 说说下面文本标签的作用 | 功能                       |
| ---------------------- | -------------------------- |
| h1                     | 标题标签，6级标题，1级最大 |
| font                   | 设置字体，大小，颜色       |
| br                     | 换行                       |
| p                      | 分段                       |
| hr                     | 水平线                     |
| b                      | 加粗                       |
| i                      | 斜体                       |



## 案例：制作黑马公司介绍

### 目标

​	使用已经学习的文本标签，制作如下的公司介绍的页面

### 效果

![1551670516854](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551670516854.png)

### 步骤

1. “公司介绍”，需要使用标题标签完成 。例如：\<h3>
2. 两条橙色的线使用水平线完成
3. “中关村黑马程序员训练营” 需要使用字体标签完成 
4. “传智播客” 需要斜体\<i> 和 粗体\<b> 组合完成
5. 这个文档被划分成4个段落，每一个段落之间有定义的间隔，需要使用段落标签\<p>完成，如果要缩进目前可以使用两个全角空格。后面还有其它实现方式。
6. 第2行与第3行是一个普通的换行，需要使用\<br/>完成
7. 最下面的页脚使用2号字体，灰色，居中。

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>公司简介</title>
</head>
<body>
<h2>公司简介</h2>
<hr color="orange" size="3"/>
<p>&nbsp;&nbsp;&nbsp;&nbsp;<font color="red">"中关村黑马程序员训练营"</font>是由<b><i>传智播客</i></b>联合中关村软件园、CSDN， 并委托传智播客进行教学实施的软件开发高端培训机构，致力于服务各大软件企业，
    解决当前软件开发技术飞速发展， 而企业招不到优秀人才的困扰。<br/>
    目前，“中关村黑马程序员训练营”已成长为行业“学员质量好、课程内容深、企业满意”的移动开发高端训练基地， 并被评为中关村软件园重点扶持人才企业。</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;黑马程序员的学员多为大学毕业后，有理想、有梦想，想从事IT行业，而没有环境和机遇改变自己命运的年轻人。 黑马程序员的学员筛选制度，远比现在90%以上的企业招聘流程更为严格。任何一名学员想成功入学“黑马程序员”，
    必须经历长达2个月的面试流程，这些流程中不仅包括严格的技术测试、自学能力测试，还包括性格测试、压力测试、 品德测试等等测试。毫不夸张地说，黑马程序员训练营所有学员都是精挑细选出来的。百里挑一的残酷筛选制度确 保学员质量，并降低企业的用人风险。</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;中关村黑马程序员训练营不仅着重培养学员的基础理论知识，更注重培养项目实施管理能力，并密切关注技术革新， 不断引入先进的技术，研发更新技术课程，确保学员进入企业后不仅能独立从事开发工作，更能给企业带来新的技术体系和理念。</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;一直以来，黑马程序员以技术视角关注IT产业发展，以深度分享推进产业技术成长，致力于弘扬技术创新，倡导分享、 开放和协作，努力打造高质量的IT人才服务平台。</p>
<hr color="orange" size="3"/>
<!--center用于居中的标签-->
<center>
<font size="2" color="grey">
江苏传智播客教育科技股份有限公司<br/>
版权所有Copyright © 2006-2018, All Rights Reserved 苏ICP备16007882
</font>
</center>
</body>
</html>
```



### 小结

案例中我们用到了哪些标签？

```
h2,p,center,font,hr, &nbsp; b i 
```





## 列表标签：有序列表和无序列表

### 目标

1. 有序列表
2. 无序列表

### 案例需求

​	制作如图所示的菜单列表，左边列是有序列表，右边列是无序列表

### 效果

![1551670700487](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551670700487.png)

### 代码

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>有序列表和无序列表</title>
</head>
<body>
<h2>有序列表</h2>
<!--
有序列表：每一项都有编号
ol：表示有序列表的容器   Order List
    type属性：
        a A 字母的编号
        i I 罗马数字的编号
        1 默认是数字编号
li：表示列表中每一项  list
alt 键选中多行，再输入，有惊喜
-->
<ol type="1">
    <li>红烧肉</li>
    <li>地沟油炒蛋</li>
    <li>牛欢喜</li>
    <li>番茄炒西红柿</li>
</ol>

<ol type="a">
    <li>红烧肉</li>
    <li>地沟油炒蛋</li>
    <li>牛欢喜</li>
    <li>番茄炒西红柿</li>
</ol>

<ol type="i">
    <li>红烧肉</li>
    <li>地沟油炒蛋</li>
    <li>牛欢喜</li>
    <li>番茄炒西红柿</li>
</ol>

<hr/>
<h2>无序列表</h2>
<!--
ul 无序列表的容器
    type属性：
        1. 默认是disc 表示实心圆
        2. circle 空心圆
        3. square 正方形
li 其中每一项
-->
<ul type="disc">
    <li>红烧肉</li>
    <li>地沟油炒蛋</li>
    <li>牛欢喜</li>
    <li>番茄炒西红柿</li>
</ul>

<ul type="circle">
    <li>红烧肉</li>
    <li>地沟油炒蛋</li>
    <li>牛欢喜</li>
    <li>番茄炒西红柿</li>
</ul>

<ul type="square">
    <li>红烧肉</li>
    <li>地沟油炒蛋</li>
    <li>牛欢喜</li>
    <li>番茄炒西红柿</li>
</ul>
</body>
</html>
```



### 小结

![1551670658102](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551670658102.png)



## 块标签与内联标签

### 目标

​	学习span标签和div标签的作用和区别

### 作用

| 标签 | 作用                                                         | 特点                 |
| ---- | ------------------------------------------------------------ | -------------------- |
| span | span和div标签的功能是相同的，用于网页的布局                  | 内联标签，不换行     |
| div  | div通常用于网页的大布局, span用于行中小的分割。<br />都可以做为其它标签的容器 | 块级标签，换行的功能 |

### 区别

#### div网页代码

\<div> 元素是块级元素，浏览器会在其前后显示折行。它是可用于组合其他 HTML 元素的容器。 \<div> 元素是英文division，起到分割的意思。如果与 CSS 一同使用，\<div> 元素可用于对大的内容块设置样式属性。\<div> 元素的一个常见的用途是文档布局。

#### span网页代码

\<span>元素是内联元素，可用作文本的容器。当与 CSS 一同使用时，\<span> 元素可用于为部分文本设置样式属性。

### 案例

​	通过代码认识div和span的功能和特点

### 效果

![1551670962184](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551670962184.png)

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>span和div标签</title>
</head>
<body>
<!--都是将网页分割成不同的部分-->
<h2>span标签</h2>
万维网联盟，又称<span style="background-color: yellow">W3C理事会</span>。1994年10月在麻省理工学院计算机科学实验室成立。
<hr/>
<h2>div标签</h2>
万维网联盟，又称<div  style="background-color: yellow">W3C理事会</div>。1994年10月在麻省理工学院计算机科学实验室成立。
</body>
</html>
```



### 小结

div标签和span标签的主要区别是什么？

```
div换行
span不换行
作用：用于网页布局，div大的布局，span用于小的分割
```



## dl-dt-dd列表标签

### 目标

学习dl-dt-dd标签的使用

### 介绍

dl: 容器，用来包含下面的2个标签

dt: 列表标题

dd: 列表项目

以后主要用于小的布局

### 案例

![1562745005540](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1562745005540.png) 

```html 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>列表标签</title>
</head>
<body>
<!--
dl: 容器，用来包含下面的2个标签
dt: 列表标题
dd: 列表项目
以后主要用于小的布局
-->
<dl>
    <dt>欢迎来到传智播客</dt>
    <dd>HTML教程</dd>
    <dd>CSS教程</dd>
    <dd>JavaScript教程</dd>
</dl>
<dl>
    <dt>游戏列表</dt>
    <dd>王者荣耀</dd>
    <dd>帝国时代</dd>
    <dd>红色警戒</dd>
</dl>
</body>
</html>
```



### 小结

dl: 列表容器

dt: 列表标题

dd: 列表每一项





## 实体字符

### 目标

​	当页面上需要使用一些特殊符号的时候怎么办呢？

![1551683446920](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551683446920.png)

### 案例代码

格式：&开头 ; 结尾

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>实体字符</title>
</head>
<body>
<h1>
    小于 &lt;
    大于 &gt;
    日元：&yen;
    与符号： &amp;
    双引号： &quot;
    版权 &copy;
</h1>
<font size="6" color="red">红心  &hearts;</font>
</body>
</html>
```

### 常用的实体字符表

![1551683521415](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551683521415.png)

### 小结

​	所有的实体字符都以什么符号开头，以什么符号结尾？

```
&开头
;结尾
```



## <font color="red">图像标签</font>

### 目标

​	如何在页面上显示图片

### 效果

![1571456888260](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1571456888260.png)

### 基本语法

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h2>图片标签</h2>
<!--
显示本服务器图片：服务器上要有图片
img：显示图片的标签 image，也是内联标签
    属性： src 必须的，指定图片的地址
               width 宽度指定图片的宽度，如果只指定宽度，高度是等比例缩放
               height 高度
               alt 如果图片丢失，显示的替代文字
               title 鼠标移上去显示的文字
-->
<img src="img/football.jpg">
<img src="img/football.jpg" alt="球不见了" title="我是你的球迷">
<hr/>
<!--也可以显示其它服务器图片(盗链)-->
<img src="https://wx3.sinaimg.cn/crop.0.0.1498.842.1000/005NLzplly1fveykabrsgj31an0nen60.jpg">
</body>
</html>
```



### 小结

| 属性名 | 作用                                     |
| ------ | ---------------------------------------- |
| src    | 指定图片地址                             |
| width  | 指定宽度，如果只指定一个，另一个等比缩放 |
| height | 指定高度                                 |
| alt    | 图片丢失，显示的替代文字                 |
| title  | 鼠标移上，显示的文字信息                 |





## 案例：家用电器排行榜

### 目标

制作家用电器排行榜案例

![1551683693058](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551683693058.png) 

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>家用电器排行榜</title>
</head>
<body>
<h2>家用电器排行榜</h2>
<hr color="orange" size="3"/>
<img src="img/tv01.jpg"> 创维425OWER&nbsp;&nbsp;55英寸&nbsp;&nbsp;&yen;3000.00

<hr color="orange" size="3"/>
<img src="img/tv02.jpg"> 创维425OWER&nbsp;&nbsp;55英寸&nbsp;&nbsp;&yen;3000.00

<hr color="orange" size="3"/>
<img src="img/tv03.jpg"> 创维425OWER&nbsp;&nbsp;55英寸&nbsp;&nbsp;&yen;3000.00
</body>
</html>
```

### 小结

在这个案例中我们用到了哪些标签？

```
hr, img, &nbsp; &yen;
```



## <font color="red">链接标签：基本使用</font>

### 目标

1. 链接标签的基本语法

2. 点链接调用发邮件的客户端

### 语法

作用：用于文字或图片上，点击它就可以跳转到其它的页面。有主体，内联标签。

```html
<a href="跳转的地址">文字或图片</a>
```



### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>珍爱网</title>
</head>
<body>
<h1>珍爱网</h1>
<hr/>
<h3>我是女生</h3>
<!--
a标签属性：
    href：要跳转的地址， 可以是同一个服务器的地址，也可以是其它服务器的
    title：鼠标移上去显示的提示文字
    target 以什么方式打开链接
        _blank 在新的窗口中打开
        _self 在当前窗口中打开(默认)
-->

<a href="demo07_img.html" title="喜欢我吗?" target="_blank">
    <img src="img/girl1.jpg">
</a>
<br/>
<a href="http://www.itcast.cn"> 点我有惊喜 </a>
</body>
</html>
```

### 调用发邮件客户端语法

```html
<!--mailto: 表示调用本地客户端发邮件-->
<a href="mailto:newboy@itcast.cn">发邮件</a>
```

### 小结

| 属性名 | 作用                                                         |
| ------ | ------------------------------------------------------------ |
| href   | 要跳转地址，可以是本服务器，也可以是其它服务器               |
| title  | 鼠标移上去显示文字                                           |
| target | 打开窗口的方式<br />\_self 在同一个窗口中打开<br />\_blank 在新的窗口中打开 |



## 回顾上午的知识点

### 文本标签

```
h1~h6 标题标签，6级标题，1级最大的。
hr 水平线
br 换行
font 设置字体，大小，颜色
b strong 加粗
i 斜体
p 段落
```



### 列表标签

```
1. 有序列表 ol-li
	type=1 数字
	type=a A 字母
	type=i I 罗马数字
2. 无序列表 ul-li
	type="disc" 实心圆
	type="circle" 空心圆
	type="square" 正方形
3. dl-dt-dd
	dl 容器
	dt 项目的标题
	dd 每一项
```



### 块级标签和内联标签

```
div span 作用是类似的，div用于大的网页布局,span小于布局
div 块级元素：自带换行的功能
span 内联元素：本身没有换行的功能
```



### 实体字符

```
&gt; 大于   greater than
&lt; 小于   less than
&amp; &符号
&quto;  双引号
&nbsp;  空格
&copy;  版权
&yen; 日元
```



### 图像标签

```
<img src="图片地址" width="宽度" height="高度" alt="替换文字" title="鼠标移上去显示的文字"/>
```



### 链接

```
<a href="跳转的地址" target="_blank" /> 新开的窗口中打开页面
<a href="mailto:邮箱地址"/>  打开发邮件客户端，给这个邮件发邮件
```



## 链接标签：设置锚点

### 目标

如何使用锚点定位到同一个网页中不同的位置？

### 锚点的步骤

如果一个网页内容比较多，有时需要跳转到同一个网页的不同位置。需要在这些位置上设置锚点。

1. 在网页的不同位置创建锚点，给锚点起名字，使用a标签 name属性
2. 在上面的链接部分使用a标签的href属性跳转到锚点位置

![1571467925030](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1571467925030.png) 

不但可以在同一个网页中跳转，也可以在不同的网页之间跳转

### 案例代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>锚点</title>
</head>
<body>
<h1>卡莉·克劳斯</h1>
<hr/>
<!--跳转到指定的锚点-->
<a href="#1">早年经历</a>
<a href="#2">演艺经历</a>
<a href="#3">个人生活</a>

<hr/>
<!-- 在不同的位置设置锚点, 使用a标签的name属性 -->
<h3><a name="1">早年经历</a></h3>
<img src="img/mimi.png"><br/>
<img src="img/mimi.png"><br/>
<hr/>
<h3><a name="2">演艺经历</a></h3>
<img src="img/mimi.png"><br/>
<img src="img/mimi.png"><br/>
<hr/>

<h3><a name="3">个人生活</a></h3>
<img src="img/mimi.png"><br/>
<img src="img/mimi.png"><br/>
<hr/>

</body>
</html>
```



### 小结

1. 如何定义锚点？

   ```
   <a name="名字">
   ```

   

2. 如何跳转到锚点？

   ```
   <a href="#名字">
   ```

   



## <font color="red">表格标签：基本使用</font>

### 目标

​	表格的结构是怎样的

### 表格的作用

1. 用来服务器或数据库中数据
2. 可以用于网页布局(很少使用了，目前主要使用div布局)

### 案例：表格的表头

![1551684059532](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551684059532.png)

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>联系人列表</title>
</head>
<body>
<!--
在浏览器上按：f12进入调试模式
table标签代表一个表格，表格的容器
属性：
    width：表格的宽度
    border：指定外边框的粗细，默认是0
    align: 指定整个表格的对齐方式

子元素：
    tr 表示一行
        align属性：这一行居中
    th 表格的列标题，加粗，居中
    td 放在tr中，表示一个普通的单元格。默认是左对齐的
        align属性：表示这一格内容居中
-->
<table width="500" border="4" align="center">
    <caption>联系人列表</caption>
    <!-- 第1行-->
    <tr bgcolor="gray">
        <th>姓名</th>
        <th>电话</th>
        <th>地址</th>
    </tr>
    <!-- 第2行-->
    <tr align="center">
        <td>Bill Gates</td>
        <td>234234234</td>
        <td>西雅图</td>
    </tr>
    <!-- 第3行-->
    <tr>
        <td>Steven Jobs</td>
        <td align="center">98734574</td>
        <td>硅谷</td>
    </tr>
</table>
</body>
</html>
```

### 表格的结构标签

![1551684133580](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551684133580.png)

![1551684144361](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551684144361.png)

### 表格属性

![1551684166720](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551684166720.png)

### 小结

| 表格结构 | 标签  |
| -------- | ----- |
| 容器     | table |
| 行       | tr    |
| 列标题   | th    |
| 单元格   | td    |



## <font color="red">案例：学生成绩表</font>

### 目标

![1551684229193](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551684229193.png)

### 步骤

1. 表格有标题，使用caption标签
2. 使用thead、tbody、tfoot对表格进行分块
3. 第一行有表格列标题，使用th标签
4. 表格的每一行都居中，使用tr标签的align属性居中对齐
5. 成绩和总成绩分别有跨行和跨列的要求
6. 使用属性要设置表格之间的间隔
7. 使用属性设置文本与表格边框之间的距离

![1571469446787](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1571469446787.png) 

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>学生成绩表</title>
</head>
<body>
<!--
table按ctrl+j
table标签属性：
        cellspacing：设置单元格之间的间隔
        cellpadding：单元格的内边距，内容与边框之间间隔
        
使用以下三个标签可以将一个表格在逻辑上分成不同的部分：
thead: 表格的头部
tbody：表格的主体部分，注：无论代码中是写tbody，浏览器解析的时候都会添加tbody元素，
            tr的父元素是不是table，而是tbody
tfoot：表格的脚部

td的属性：
    rowspan 跨几行
    colspan 跨几列
-->
<table width="500" border="2" align="center" cellspacing="0" cellpadding="4">
    <caption>学生成绩表</caption>
    <thead>
    <tr>
        <th>编号</th>
        <th>姓名</th>
        <th>性别</th>
        <th>成绩</th>
    </tr>
    </thead>
    <tbody align="center">
    <tr>
        <td>100</td>
        <td>潘金莲</td>
        <td>女</td>
        <td>80</td>
    </tr>
    <tr>
        <td>200</td>
        <td>白骨精</td>
        <td>女</td>
        <td rowspan="2">90</td>
    </tr>
    <tr>
        <td>300</td>
        <td>孙悟空</td>
        <td>男</td>
    </tr>
    </tbody>
    <tfoot>
    <tr align="center">
        <td>总成绩</td>
        <td colspan="3">900</td>
    </tr>
    </tfoot>
</table>
</body>
</html>
```

### 小结

哪个元素浏览器会自动创建？

```
tbody
```

td中哪个属性跨列的？

```
colspan
```

cellspacing属性作用？

```
指定单元格之间间隔
```



## <font color="red">表单标签:介绍和基本属性</font>

### 目标

1. 表单标签的作用
2. 表单标签有哪些属性

### 作用

![1571470749255](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1571470749255.png)

### 常用属性

| form常用属性 | 作用                                        |
| ------------ | ------------------------------------------- |
| action       | 提交给服务器的地址                          |
| method       | 提交的方式，有2个取值：get或post，默认是get |

### GET与POST的区别

| 提交方法 | 特点                              |
| -------- | --------------------------------- |
| GET      | 提交的参数会显示在地址栏后面(URL) |
| POST     | 不会显示在地址栏上，相对更加安全  |

![1571471339938](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1571471339938.png) 

### 案例：表单登录

![1551770885387](/assets/1551770885387.png) 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>表单</title>
</head>
<body>
<h2>用户登录</h2>
<!--
form标签的属性：
    action: 提交给服务器的地址，如果地址不存在，返回404错误
    method：表单提交的方式

input标签默认是文本框
    name属性：提交给服务器的参数名
    value：输入项默认的值，在按钮上显示为它的文字
    type属性：指定表单控件的类型
        text: 文本框，默认值
        submit： 提交表单的按钮
        password：密码框，输入的字符不可见
-->
<form action="server" method="post">
    用户名： <input name="username" type="text"/><br/>
    密码：<input name="password" type="password"/> <br/>
    <input type="submit" value="登录"/>
</form>
</body>
</html>
```

### 小结

1. 表单标签的作用? 将浏览器端的数据提交给服务器
2. 表单标签有哪些属性?  
   1. action 提交的地址
   2. method 提交的方式



## <font color="red">表单控件：文本框、密码框、单选框、复选框</font>

### 目标

​	结合表格布局，制作如图所示的注册页面

### 效果

![1551770654462](/assets/1551770654462.png) 

### 步骤

1. 整个表单由8行2列组成，第1列显示文本，可以在td中使用label标签
2. 用户名、密码、性别、爱好、照片使用input标签，设置不同的type属性
3. 学历使用select，个人简介使用textarea
4. 最后1行跨2列，注册、清空、按钮的type分别是submit、reset、button

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>用户注册</title>
</head>
<body>
<h1>用户注册</h1>
<hr/>
<!--
input元素
    name属性：给服务器使用的参数名字
    type属性：
        text: 文本框
        password: 密码框
        radio：单选框
        checkbox：复选框
  -->

<form action="server" method="get">
    <table>
        <tr>
            <td>用户名：</td>
            <td>
                <input type="text" name="username">
            </td>
        </tr>

        <tr>
            <td>密码：</td>
            <td>
                <input type="password" name="password">
            </td>
        </tr>

        <tr>
            <td>性别：</td>
            <td>
                <!--
                同名的单选框是一组，一组只能选中一个
                如果没有给单选框值，默认是on，必须指定value属性
                checked="checked" 表示默认选中这一项
                -->
                <input type="radio" name="gender" value="男" checked="checked">男
                <input type="radio" name="gender" value="女">女
            </td>
        </tr>

        <tr>
            <td>爱好：</td>
            <td>
                <!--
                要加上value属性才有值
                checked="checked" 表示默认选中这一项
                -->
                <input type="checkbox" name="hobby" value="上网"> 上网
                <input type="checkbox" name="hobby" value="上车" checked="checked"> 上车
                <input type="checkbox" name="hobby" value="上吊"> 上吊
                <input type="checkbox" name="hobby" value="上传"> 上传
            </td>
        </tr>
        
        <tr>
            <td colspan="2" align="center">
                <input type="submit" value="注册">
            </td>
        </tr>
    </table>
</form>
</body>
</html>
```

### 小结

1. 上面学的所有的标签都是：input
2. 只是type不同：
   1. 文本框：text
   2. 密码框：password
   3. 单选框：radio
   4. 复选框：checkbox



## 表单: 其它控件的学习

### 目标

​	表单中其它控件的作用和常用属性

### 表单控件

![1551770781384](/assets/1551770781384.png)

![1551770803741](/assets/1551770803741.png)

![1551770820907](/assets/1551770820907.png)

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>用户注册</title>
</head>
<body>
<h1>用户注册</h1>
<hr/>
<!--
input元素
    name属性：给服务器使用的参数名字
    type属性：
        text: 文本框
        password: 密码框
        radio：单选框
        checkbox：复选框
        file: 文件域，用来选择一个文件
        button： 普通按钮
        reset：将表单还原到最初的状态
        hidden：在表单上不可见，可以将数据提交给服务器
  -->

<form action="server" method="get">
    <input type="hidden" name="id" value="23094234324324"/>
    <table>
        <tr>
            <td>用户名：</td>
            <td>
                <!--
                placeholder：提示的信息
                value：表示默认值
                readonly：只能读取，不能修改
                disabled：不可用，不能将值提交给服务器
                -->
                <input type="text" name="username" disabled="disabled" placeholder="请输入用户名" value="牛魔王" readonly="readonly">
            </td>
        </tr>

        <tr>
            <td>密码：</td>
            <td>
                <input type="password" name="password" placeholder="请输入密码">
            </td>
        </tr>

        <tr>
            <td>性别：</td>
            <td>
                <!--
                同名的单选框是一组，一组只能选中一个
                如果没有给单选框值，默认是on，必须指定value属性
                checked="checked" 表示默认选中这一项
                -->
                <input type="radio" name="gender" value="男" checked="checked">男
                <input type="radio" name="gender" value="女">女
            </td>
        </tr>

        <tr>
            <td>爱好：</td>
            <td>
                <!--
                要加上value属性才有值
                checked="checked" 表示默认选中这一项
                -->
                <input type="checkbox" name="hobby" value="上网"> 上网
                <input type="checkbox" name="hobby" value="上车" checked="checked"> 上车
                <input type="checkbox" name="hobby" value="上吊"> 上吊
                <input type="checkbox" name="hobby" value="上传"> 上传
            </td>
        </tr>

        <tr>
            <td>
                学历:
            </td>
            <td>
                <!--
                下拉列表
                select是一个容器，包含以下标签
                    name给服务器使用的参数名
                    multiple 设置为多选，按ctrl可以选中多个
                    size 设置显示多少项

                option 代表其中一项
                    默认它的值就是中间的文本，如果值不同，就必须指定
                    value 指定这一项的值
                    disabled="disabled" 这一项不可用
                    selected="selected" 默认选中这一项
                -->
                <select name="edu">
                    <option disabled="disabled">- 请选择 -</option>
                    <option value="1">本科</option>
                    <option value="2">硕士</option>
                    <option value="3" selected="selected">烈士</option>
                    <option value="4">圣斗士</option>
                </select>
            </td>
        </tr>

        <tr>
            <td>照片：</td>
            <td>
                <!--accept指定显示文件的类型：大类型/小类型-->
                <input type="file" name="photo" accept="image/jpeg">
            </td>
        </tr>

        <tr>
            <td>简介：</td>
            <td>
                <!--
                多行文本域，它没有value属性。它的值就是主体内容
                rows：行数
                cols：列数
                -->
                <textarea name="introduce" rows="5" cols="50"></textarea>
            </td>
        </tr>
        <tr>
            <td colspan="2" align="center">
                <input type="submit" value="注册">
                <input type="button" value="按钮">
                <input type="reset" value="重置">
                <!--image图片按钮，功能与submit一样，指定src属性：指定图片地址-->
                <input type="image" src="img/girl.jpg" width="100">
            </td>
        </tr>
    </table>
</form>
</body>
</html>
```



### 小结

1. 下拉列表是什么标签？每个选项是什么标签？选中的属性是什么？

   ```
   select option selected="selected"
   ```

2. 多行文本域是什么标签？它有没有value属性？

   ```
   textarea
   没有，value就是主体内容
   ```

   



## HTML5中新增的type类型[了解]

### 目标

HTML5中新增的几个type作用

### 效果

![1571389301648](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1571389301648.png) 

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>HTML5中新增的类型</title>
</head>
<body>
<form action="server">
    生日： <input type="date" name="birthday"> <br/>
    <!--email可以验证邮箱地址是否正确-->
    邮箱： <input type="email"> <br/>
    颜色：<input type="color" name="color"/> <br/>\
    <!--不能输入字母，除了小写的e-->
    QQ： <input type="number" name="qq"> <br/>
    <input type="submit">
</form>
</body>
</html>
```

### 小结

| type属性值 | 作用       |
| ---------- | ---------- |
| date       | 日历选择框 |
| email      | 验证邮箱   |
| color      | 颜色的值   |
| number     | 数字       |



## 案例：黑马旅游页面的布局

### 目标

![/](/assets/1551684229194.png)

### 步骤

1. 创建最外面的表格，边框、单元格间隔、单元格与内容的间隔都设置成0，宽度100%，居中对齐
2. 上面的4行，设置为thead
3. 中间部分设置为tbody
4. 最后2行，设置为tfoot
5. 创建7行tr和td

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>黑马旅游的首页</title>
</head>
<body>
<!--最外面的表格，边框、单元格间隔、单元格与内容的间隔都设置成0，宽度100%，居中对齐-->
<table width="100%" cellspacing="0" cellpadding="0" align="center">
    <thead>
    <!--第1行-->
    <tr>
        <td></td>
    </tr>

    <!--第2行-->
    <tr>
        <td></td>
    </tr>

    <!--第3行-->
    <tr>
        <td></td>
    </tr>

    <!--第4行-->
    <tr>
        <td></td>
    </tr>
    </thead>

    <tbody>
    <!--第5行-->
    <tr>
        <td></td>
    </tr>
    </tbody>

    <tfoot>
    <!--第6行-->
    <tr>
        <td></td>
    </tr>

    <!--第7行-->
    <tr>
        <td></td>
    </tr>
    </tfoot>
</table>
</body>
</html>
```



## 案例：黑马旅游页面的头部

### 目标

![1551684497512](/assets/1551684497512.png)

### 步骤

1. 第1行：直接在里面插入一张图片top_banner.jpg即可，图片的宽度使用100%
2. 第2行：
   1. 使用表格嵌套，插入一个1行3列的表格。边框、单元格间隔、单元格与内容的间隔都设置成0，宽度100%。
   2. 表格只有1行，全部居中
   3. 第1列的图片：logo.jpg，第2列的图片search.png，宽度设置为500，第3列图片hotel_tel.png
3. 第3行：
   1. 插入一个1行10列的表格
   2. 边框、单元格间隔、单元格与内容的间隔都设置成0，宽度100%，背景色设置#ffc900。
   3. 在每个单元格中输入一个菜单项，每个菜单项上可以加上a标签，单元格的高度设置为45
   4. tr对齐居中，则表格中的文字居中
   5. 上一级的td对齐居中，则表格整个位置居中。
4. 这里只要插入一张图片banner_3.jpg即可，宽度为100%

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>黑马旅游的首页</title>
</head>
<body>
<!--最外面的表格，边框、单元格间隔、单元格与内容的间隔都设置成0，宽度100%，居中对齐-->
<table width="100%" cellspacing="0" cellpadding="0" align="center">
    <thead>
    <!--第1行-->
    <tr>
        <td>
            <img src="img/top_banner.jpg" width="100%">
        </td>
    </tr>

    <!--
    第2行：1行3列表格嵌套
    -->
    <tr>
        <td>
            <table width="100%" cellspacing="0" cellpadding="0" align="center">
                <!--1行3列-->
                <tr align="center">
                    <td>
                        <img src="img/logo.jpg">
                    </td>
                    <td>
                        <input type="text" size="50" placeholder="请输入线路名称"><input type="submit" value="  搜  索   ">
                    </td>
                    <td>
                        <img src="img/hotel_tel.png">
                    </td>
                </tr>
            </table>
        </td>
    </tr>

    <!--第3行：导航条，1行10列表格-->
    <tr>
        <td>
            <table width="100%" cellspacing="0" cellpadding="0" align="center">
                <tr align="center" bgcolor="orange">
                    <td height="45"><a href="#">国内游</a></td>
                    <td><a href="#">地沟游</a></td>
                    <td>地沟游</td>
                    <td>地沟游</td>
                    <td>地沟游</td>
                    <td>地沟游</td>
                    <td>地沟游</td>
                    <td>地沟游</td>
                    <td>地沟游</td>
                    <td>地沟游</td>
                </tr>
            </table>
        </td>
    </tr>

    <!--第4行-->
    <tr>
        <td>
            <img src="img/banner_3.jpg" width="100%">
        </td>
    </tr>
    </thead>

    <tbody>
    <!--第5行-->
    <tr>
        <td></td>
    </tr>
    </tbody>

    <tfoot>
    <!--第6行-->
    <tr>
        <td></td>
    </tr>

    <!--第7行-->
    <tr>
        <td></td>
    </tr>
    </tfoot>
</table>
</body>
</html>
```



## 案例：黑马旅游页面的主体

### 目标

![](/assets/1551684497513.png)

### 步骤

1. "黑马精选"、"国内游"、"境外游"三个部分嵌套一个6行1列的表格，边框、内边距、单元格间距都为0，表格宽度为95%，表格设置居中。
2. 其中1，3，5行在td中添加图标icon_5.jpg、icon_6.jpg、icon_7.jpg和文字，下面使用hr添加水平线，粗细2，颜色为：#ffc900，td单元格的高度为80
3. 第2行嵌套一个1行4列的表格，每个单元格插入1张图片jiangxuan_1.jpg和文字。文字使用p。价格使用font标签设置为红色。
4. 第4行嵌套一个1行2列的表格，左边1列单元格插入1张图片guonei_1.jpg，宽度30%，右边1列嵌套一个2行3列的表格，每个单元格插入1张图片和文字。
5. 第6行与第4行的制作方法相同，左边1列的图片名为jiangwai_1.jpg，修改文字和图片即可

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>黑马旅游的首页</title>
</head>
<body>
<!--最外面的表格，边框、单元格间隔、单元格与内容的间隔都设置成0，宽度100%，居中对齐-->
<table width="100%" cellspacing="0" cellpadding="0" align="center">
    <thead>
    <!--第1行-->
    <tr>
        <td>
            <img src="img/top_banner.jpg" width="100%">
        </td>
    </tr>

    <!--
    第2行：1行3列表格嵌套
    -->
    <tr>
        <td>
            <table width="100%" cellspacing="0" cellpadding="0" align="center">
                <!--1行3列-->
                <tr align="center">
                    <td>
                        <img src="img/logo.jpg">
                    </td>
                    <td>
                        <input type="text" size="50" placeholder="请输入线路名称"><input type="submit" value="  搜  索   ">
                    </td>
                    <td>
                        <img src="img/hotel_tel.png">
                    </td>
                </tr>
            </table>
        </td>
    </tr>

    <!--第3行：导航条，1行10列表格-->
    <tr>
        <td>
            <table width="100%" cellspacing="0" cellpadding="0" align="center">
                <tr align="center" bgcolor="orange">
                    <td height="45"><a href="#">国内游</a></td>
                    <td><a href="#">地沟游</a></td>
                    <td>地沟游</td>
                    <td>地沟游</td>
                    <td>地沟游</td>
                    <td>地沟游</td>
                    <td>地沟游</td>
                    <td>地沟游</td>
                    <td>地沟游</td>
                    <td>地沟游</td>
                </tr>
            </table>
        </td>
    </tr>

    <!--第4行-->
    <tr>
        <td>
            <img src="img/banner_3.jpg" width="100%">
        </td>
    </tr>
    </thead>

    <tbody>
    <!--第5行-->
    <tr>
        <td>
            <!--没有占满，4行1列-->
            <table width="95%" cellspacing="0" cellpadding="0" align="center">
                <!--1行-->
                <tr>
                    <td height="70">
                        <img src="img/icon_5.jpg">黑马精选
                        <hr color="orange" size="3"/>
                    </td>
                </tr>

                <!--2行-->
                <tr>
                    <td>
                        <table width="100%" cellspacing="0" cellpadding="0" align="center">
                            <!--1行4列-->
                            <tr>
                                <td>
                                    <img src="img/jiangxuan_4.jpg">
                                    <p>上海直飞三亚5天4晚自由行</p>
                                    <font color="red">&yen; 899</font>
                                </td>
                                <td>
                                    <img src="img/jiangxuan_4.jpg">
                                    <p>上海直飞三亚5天4晚自由行</p>
                                    <font color="red">&yen; 899</font>
                                </td>
                                <td>
                                    <img src="img/jiangxuan_4.jpg">
                                    <p>上海直飞三亚5天4晚自由行</p>
                                    <font color="red">&yen; 899</font>
                                </td>
                                <td>
                                    <img src="img/jiangxuan_4.jpg">
                                    <p>上海直飞三亚5天4晚自由行</p>
                                    <font color="red">&yen; 899</font>
                                </td>
                            </tr>
                        </table>
                    </td>
                </tr>

                <!--3行-->
                <tr>
                    <td height="80">
                        <img src="img/icon_6.jpg">国内游
                        <hr color="orange" size="3"/>
                    </td>
                </tr>

                <!--4行-->
                <tr>
                    <td>
                        <!--1行2列-->
                        <table width="100%" cellspacing="0" cellpadding="0" align="center">
                            <tr>
                                <td>
                                    <img src="img/guonei_1.jpg">
                                </td>
                                <td>
                                    <table width="100%" cellspacing="0" cellpadding="0" align="center">
                                        <!--2行3列-->
                                        <tr>
                                            <td>
                                                <img src="img/jiangxuan_5.jpg">
                                                <p>上海直飞三亚5天4晚自由行</p>
                                                <font color="red">&yen; 899</font>
                                            </td>
                                            <td>
                                                <img src="img/jiangxuan_5.jpg">
                                                <p>上海直飞三亚5天4晚自由行</p>
                                                <font color="red">&yen; 899</font>
                                            </td>
                                            <td>
                                                <img src="img/jiangxuan_5.jpg">
                                                <p>上海直飞三亚5天4晚自由行</p>
                                                <font color="red">&yen; 899</font>
                                            </td>
                                        </tr>

                                        <tr>
                                            <td>
                                                <img src="img/jiangxuan_5.jpg">
                                                <p>上海直飞三亚5天4晚自由行</p>
                                                <font color="red">&yen; 899</font>
                                            </td>
                                            <td>
                                                <img src="img/jiangxuan_5.jpg">
                                                <p>上海直飞三亚5天4晚自由行</p>
                                                <font color="red">&yen; 899</font>
                                            </td>
                                            <td>
                                                <img src="img/jiangxuan_5.jpg">
                                                <p>上海直飞三亚5天4晚自由行</p>
                                                <font color="red">&yen; 899</font>
                                            </td>
                                        </tr>
                                    </table>
                                </td>
                            </tr>
                        </table>
                    </td>
                </tr>
            </table>

        </td>
    </tr>
    </tbody>

    <tfoot>
    <!--第6行-->
    <tr>
        <td></td>
    </tr>

    <!--第7行-->
    <tr>
        <td></td>
    </tr>
    </tfoot>
</table>
</body>
</html>
```



## 案例：黑马旅游页面的底部

### 目标

![1551684792614](/assets/1551684792614.png)

### 步骤

1. 第1行插入图片footer_service.png，宽度100%
2. 第2行输入文字，背景色\#ffc900，内容居中，大小2，颜色用灰色

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>黑马旅游的首页</title>
</head>
<body>
<!--最外面的表格，边框、单元格间隔、单元格与内容的间隔都设置成0，宽度100%，居中对齐-->
<table width="100%" cellspacing="0" cellpadding="0" align="center">
    <thead>
    <!--第1行-->
    <tr>
        <td>
            <img src="img/top_banner.jpg" width="100%">
        </td>
    </tr>

    <!--
    第2行：1行3列表格嵌套
    -->
    <tr>
        <td>
            <table width="100%" cellspacing="0" cellpadding="0" align="center">
                <!--1行3列-->
                <tr align="center">
                    <td>
                        <img src="img/logo.jpg">
                    </td>
                    <td>
                        <input type="text" size="50" placeholder="请输入线路名称"><input type="submit" value="  搜  索   ">
                    </td>
                    <td>
                        <img src="img/hotel_tel.png">
                    </td>
                </tr>
            </table>
        </td>
    </tr>

    <!--第3行：导航条，1行10列表格-->
    <tr>
        <td>
            <table width="100%" cellspacing="0" cellpadding="0" align="center">
                <tr align="center" bgcolor="orange">
                    <td height="45"><a href="#">国内游</a></td>
                    <td><a href="#">地沟游</a></td>
                    <td>地沟游</td>
                    <td>地沟游</td>
                    <td>地沟游</td>
                    <td>地沟游</td>
                    <td>地沟游</td>
                    <td>地沟游</td>
                    <td>地沟游</td>
                    <td>地沟游</td>
                </tr>
            </table>
        </td>
    </tr>

    <!--第4行-->
    <tr>
        <td>
            <img src="img/banner_3.jpg" width="100%">
        </td>
    </tr>
    </thead>

    <tbody>
    <!--第5行-->
    <tr>
        <td>
            <!--没有占满，4行1列-->
            <table width="95%" cellspacing="0" cellpadding="0" align="center">
                <!--1行-->
                <tr>
                    <td height="70">
                        <img src="img/icon_5.jpg">黑马精选
                        <hr color="orange" size="3"/>
                    </td>
                </tr>

                <!--2行-->
                <tr>
                    <td>
                        <table width="100%" cellspacing="0" cellpadding="0" align="center">
                            <!--1行4列-->
                            <tr>
                                <td>
                                    <img src="img/jiangxuan_4.jpg">
                                    <p>上海直飞三亚5天4晚自由行</p>
                                    <font color="red">&yen; 899</font>
                                </td>
                                <td>
                                    <img src="img/jiangxuan_4.jpg">
                                    <p>上海直飞三亚5天4晚自由行</p>
                                    <font color="red">&yen; 899</font>
                                </td>
                                <td>
                                    <img src="img/jiangxuan_4.jpg">
                                    <p>上海直飞三亚5天4晚自由行</p>
                                    <font color="red">&yen; 899</font>
                                </td>
                                <td>
                                    <img src="img/jiangxuan_4.jpg">
                                    <p>上海直飞三亚5天4晚自由行</p>
                                    <font color="red">&yen; 899</font>
                                </td>
                            </tr>
                        </table>
                    </td>
                </tr>

                <!--3行-->
                <tr>
                    <td height="80">
                        <img src="img/icon_6.jpg">国内游
                        <hr color="orange" size="3"/>
                    </td>
                </tr>

                <!--4行-->
                <tr>
                    <td>
                        <!--1行2列-->
                        <table width="100%" cellspacing="0" cellpadding="0" align="center">
                            <tr>
                                <td>
                                    <img src="img/guonei_1.jpg">
                                </td>
                                <td>
                                    <table width="100%" cellspacing="0" cellpadding="0" align="center">
                                        <!--2行3列-->
                                        <tr>
                                            <td>
                                                <img src="img/jiangxuan_5.jpg">
                                                <p>上海直飞三亚5天4晚自由行</p>
                                                <font color="red">&yen; 899</font>
                                            </td>
                                            <td>
                                                <img src="img/jiangxuan_5.jpg">
                                                <p>上海直飞三亚5天4晚自由行</p>
                                                <font color="red">&yen; 899</font>
                                            </td>
                                            <td>
                                                <img src="img/jiangxuan_5.jpg">
                                                <p>上海直飞三亚5天4晚自由行</p>
                                                <font color="red">&yen; 899</font>
                                            </td>
                                        </tr>

                                        <tr>
                                            <td>
                                                <img src="img/jiangxuan_5.jpg">
                                                <p>上海直飞三亚5天4晚自由行</p>
                                                <font color="red">&yen; 899</font>
                                            </td>
                                            <td>
                                                <img src="img/jiangxuan_5.jpg">
                                                <p>上海直飞三亚5天4晚自由行</p>
                                                <font color="red">&yen; 899</font>
                                            </td>
                                            <td>
                                                <img src="img/jiangxuan_5.jpg">
                                                <p>上海直飞三亚5天4晚自由行</p>
                                                <font color="red">&yen; 899</font>
                                            </td>
                                        </tr>
                                    </table>
                                </td>
                            </tr>
                        </table>
                    </td>
                </tr>
            </table>

        </td>
    </tr>
    </tbody>

    <tfoot>
    <!--第6行-->
    <tr>
        <td>
            <img src="img/footer_service.png" width="100%">
        </td>
    </tr>

    <!--第7行-->
    <tr>
        <td bgcolor="orange" height="60" align="center">
            <font color="red" size="2">江苏传智播客教育科技股份有限公司 版权所有Copyright 2006-2018, All Rights Reserved 苏ICP备16007882</font>
        </td>
    </tr>
    </tfoot>
</table>
</body>
</html>
```



# 学习总结

1. 能够使用idea创建html文档

![1551670139700](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551670139700.png)

2. 能够使用h1~h6、hr、p、br 等与文本有关的标签

| 说说下面文本标签的作用 | 功能 |
| ---------------------- | ---- |
| h1                     |      |
| font                   |      |
| br                     |      |
| p                      |      |
| hr                     |      |
| b                      |      |
| i                      |      |

3. 能够使用有序列表ol-li和无序列表ul-li显示列表内容 ![1551670658102](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551670658102.png)

4. 能够使用块标签div与内联标签span![1551767026042](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/HTML/1551767026042.png)

 

5. 能够使用图片img标签把图片显示在页面中

| 属性名 | 作用 |
| ------ | ---- |
| src    |      |
| width  |      |
| height |      |
| alt    |      |
| title  |      |

 

6. 能够使用超链接a标签跳转到一个新页面

| 属性名 | 作用               |
| ------ | ------------------ |
| href   |                    |
| title  |                    |
| target | \_self<br />_blank |

7. 能够使用table、tr、td标签定义表格

| 标签名  | 作用 |
| ------- | ---- |
| table   |      |
| tr      |      |
| th      |      |
| td      |      |
| caption |      |

8. 能够制作黑马旅游网的首页



9. 能够使用表单form标签创建表单容器

| 常用属性 | 作用 |
| -------- | ---- |
| action   |      |
| method   |      |

10. 能够使用表单中常用的input标签创建输入项

| 表单项   | 代码 |
| -------- | ---- |
| 文本框   |      |
| 密码框   |      |
| 单选框   |      |
| 复选框   |      |
| 提交按钮 |      |

11. 能够使用表单select标签定义下拉选择输入项

| 容器     | multiple：   <br />size:  |
| -------- | ------------------------- |
| 其中一项 | value:    <br />selected: |

12. 能够使用表单textarea标签定义文本域

rows:

cols:

是否有value属性

