# 《CSS样式与JavaScript基础-笔记》

# 回顾

## 说出以下文本标记的作用

| 标签名 | 作用               |
| ------ | ------------------ |
| h1~h6  | 标题，1级最大      |
| hr     | 水平线             |
| font   | 字体，颜色，大小   |
| br     | 换行               |
| p      | 段落               |
| ol-li  | 有序列表           |
| ul-li  | 无序列表           |
| span   | 小的布局，内联元素 |
| div    | 大的布局，块级元素 |

## 超链接标记

```html
<a>文字或图片</a>
```

| 链接属性 | 作用                                                         |
| -------- | ------------------------------------------------------------ |
| href     | 指定跳转的地址                                               |
| target   | 打开网页的方式<br />\_self  默认，在当前窗口中打开 <br />_blank 在新的窗口中打开 |



## img标签常用属性

| 属性名 | 作用                   |
| ------ | ---------------------- |
| src    | 图片的URL(地址)        |
| width  | 宽度                   |
| height | 高度                   |
| alt    | 图片失效，显示的文字   |
| title  | 鼠标移上去后显示的信息 |

##  表格有关的标签

| 标签名 | 作用                           |
| ------ | ------------------------------ |
| table  | 表格容器                       |
| tr     | 一行                           |
| th     | 列标题：加粗，居中             |
| td     | 单元格                         |
| tbody  | 表格的主体，不写浏览器也会生成 |

## 使用表单form标签

提交数据到服务器

| 常用属性 | 作用                  |
| -------- | --------------------- |
| action   | 提交给服务器的地址    |
| method   | 提交的方式：GET或POST |

## input标签输入项

| 表单项 type属性            | 代码     |
| -------------------------- | -------- |
| 文本框                     | text     |
| 密码框                     | password |
| 单选框   checked="checked" | radio    |
| 复选框  checked="checked"  | checkbox |
| 提交按钮                   | submit   |

## select标签

| 容器            | multiple：多选                                          |
| --------------- | ------------------------------------------------------- |
| 其中一项 option | value:  指定这一项的值  <br />selected:  默认选中这一项 |



# 学习目标

1. 选择器
   1. 能够使用CSS的基本选择器选择元素
   2. 能够使用CSS的扩展选择器选择元素
2. CSS样式
   1. 能够使用常见的CSS属性
   2. 能够说出盒子模型的属性
3. JavaScript基础
   1. 能够说出JS中五种数据类型
   2. 能够使用JS中常用的运算符
   3. 能够使用JS中的流程控制语句



# 学习内容

## 什么是CSS

### 目标

1. 什么是CSS
2. CSS有什么用

### 概念

- HTML：主要用于网页的结构制作(建房子，毛坯房)
- CSS：美化网页(装修)

CSS：Cascading Style Sheet  层叠样式表，用于网页美化，用于分工协作。



![1562743701250](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1562743701250.png)



### 案例：CSS的作用

​	将一个表格中所有的单元格居中，如果使用以前的方式，每个td或tr都要设置align属性为center，而使用css则方便得多。

### 效果

![1551771191107](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1551771191107.png)

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>CSS样式的好处</title>
 <!--
 1. 所有的td居中
 2. 文字颜色设置为蓝色
 -->
    <style type="text/css">
        td {
            text-align: center;
            color: blue;
        }

        h2 {
            color: red;
        }
    </style>
</head>
<body>
<h2>成绩表</h2>
<table border="1" align="center" width="500">
    <tr>
        <td>1111</td>
        <td>1111</td>
        <td>1111</td>
    </tr>
    <tr>
        <td>1111</td>
        <td>1111</td>
        <td>1111</td>
    </tr>
    <tr>
        <td>1111</td>
        <td>1111</td>
        <td>1111</td>
    </tr>
</table>
</body>
</html>
```

### 小结

| CSS的好处 | 描述                                               |
| --------- | -------------------------------------------------- |
| 功能上    | 比HTML功能更加强大，可以实现HTML不能实现的美化功能 |
| 耦合性    | 降低了代码的耦合度，美化的代码和结构的代码分离了。 |



## CSS在网页中的3种位置

### 目标

1. CSS的规则
2. CSS出现的三种位置

### CSS规则

```css
<style type="text/css">
    td {
        /*文本居中*/
        text-align: center;
        color: blue;
    }

    h2 {
        color: red;
    }
</style>
```



| 规范     | 说明                                               |
| -------- | -------------------------------------------------- |
| 大括号   | 所有的样式写在2个大括号之间                        |
| 样式名   | 都是小写，多个单词使用-分隔                        |
| 样式值   | 每个样式名有对应固定的样式值，两者之间使用冒号分隔 |
| 样式结尾 | 每条样式以分号结尾                                 |
| 注释     | /* */ 与Java中多行注释一样                         |

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>CSS出现的三种位置</title>
</head>
<body>
<h2>行内样式</h2>
<!-- 只出现在一个标签中，只对这一个标签起作用，以style属性存在 -->

<span style="color: red;">行内样式</span><br/>
<span>行内样式</span><br/>
<span>行内样式</span><br/>

<h2>内部样式</h2>
<!--
在网页的内部，以style标签的方式存在，对整个网页起作用
-->
<!-- 内部样式-->
<style type="text/css">
    span {
        color: green;
        /*字体的大小*/
        font-size: 35px;
    }
</style>

<h2>外部样式</h2>
<!--
以文件的方式存在HTML之外，文件扩展名叫.css，使用的时候必须要导入在HTML文件中
可以给多个不同的HTML文件使用
link标签作用：导入外部的CSS文件
    属性：
        href：必须的属性，指定外部的CSS文件
        rel:  指定HTML与外部文件的关系为样式表的关系，必须的。
        type：可选，指定为CSS样式
-->
<link href="css/out.css" rel="stylesheet" type="text/css">

<!--同名的样式会被覆盖，不同名的还是起作用的，所以称为层叠样式表 -->
</body>
</html>
```



#### 疑问：以后三种样式哪种会用得比较多？

```
外部样式，耦合度比较度。注：为了让大家上课代码看得更加清楚，我使用内部样式比较多。
```



### 小结

1. 行内样式有什么特点？

   ```
   范围：只对一个标签起作用
   ```

   

2. 内部样式有什么特点？

   ```
   范围：对整个HTML文件中元素起作用
   ```

   

3. 外部样式有什么特点？

   ```
   范围：可以用于多个不同的HTML文件
   ```

   

4. 三种样式出现同名的样式会怎么样？

   ```
   后面出现的同名样式会覆盖前面，不同名还是起作用
   ```

   

## <font color="red">CSS基本选择器</font>

### 目标

学习CSS的四种基本选择器

### 选择器作用

在操作对网页中的元素设置CSS样式之前，必须先选中要操作的元素。这就是选择器的使用。

### 案例演示

#### 需求

通过代码演示上面CSS的导入，各种基本选择器的使用

#### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>基本选择器</title>
</head>
<body>
<h2>ID选择器</h2>
<!--
1. 通过ID属性来选择一个唯一的元素(在同一个网页中不能出现2个同名的ID)
语法：#ID
-->
<style type="text/css">
    #sun {
        font-size: 25px;
    }
</style>

<h2>标签选择器</h2>
<!--
2. 通过标签名字来选中一个或多个元素
语法：标签名
-->
<style>
    p {
        color: green;
    }
</style>

<h2>类选择器</h2>
<!--3. 类选择器通过class值来选择一个或多个元素
    先使用class属性，给标签进行分类。
    语法：.类名
    -->
<style>
    .child {
        color: red;
    }
</style>

<h2>通用选择器</h2>
<!--
4. 选中整个网页中所有的元素
语法：  *
-->
<style>
    * {
        font-family: 隶书;
    }
</style>
<p id="sun">孙悟空</p>

<p>牛魔王</p>

<p class="child">红孩子</p>
<span class="child">铁扇公主</span>

</body>
</html>
```

#### 选择器之间优先级

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>优先级</title>
    <style>
        /*通用选择器*/
        * {
            color: blue;
        }

        /*标签选择器*/
        p {
            color: red;
        }

        /*类选择器*/
        .one {
            color: green;
        }

        /*ID选择器*/
        #two {
            color: pink;
        }
    </style>
</head>
<body>
<h2>标题</h2>
<p>段落</p>
<p class="one" id="two">段落</p>
<div>块级元素</div>
</body>
</html>
```



```
通用选择器 < 标签选择器 < 类选择器 < ID选择器
```



### 小结

![1551771636816](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1551771636816.png)



## <font color="red">扩展选择器：层级、属性选择器</font>

### 目标

1. 层级选择器
2. 属性选择器

### 什么是扩展选择器

有时我们希望有更加灵活的选择方式，如果只通过4个基本选择器来选择的话，不够灵活。

在基本选择器的基础上进行的一些组合的选择。

### 扩展选择器案例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>层级，属性选择器</title>
</head>
<body>
<h2>层级选择器</h2>
<!--
1. 层级选择器：从父元素开始一级级通过层级去选择元素，得到子孙元素
语法：  父元素 子孙元素
-->
<style>
    div span {
        color: red;
    }
</style>

<div>
    <a>
        <span>
            我是女生
        </span>
    </a>
</div>

<span title="one">
    我是男生
</span>


<h2>属性选择器</h2>
<!--
2. 通过属性的名字去选择元素
语法：
    [属性名] 只要包含这个属性的元素都会选中
    标签名[属性名] 同时指定标签名和属性名
    标签名[属性名=值] 同时指定标签名，属性名和属性值
-->
<style>
    p[title="three"] {
            font-family: 隶书;
    }
</style>

<p title="two">
    明天更美好
</p>

<p title="three">
    后天更加美好
</p>
</body>
</html>
```

### 小结

![1551944648449](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1551944648449.png)



## CSS扩展选择器：伪类、并集、交集选择器

### 目标

1. 伪类选择器
2. 并集选择器
3. 交集选择器

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>伪类、并集、交集选择器</title>
</head>
<body>
<h2>伪类选择器</h2>
<!--
某个元素在不同的操作状态下显示不同的样式，设置选中它不同状态。
语法： 选择器:状态
-->
<style>
    /*正常状态*/
    a:link {
        /*去掉下划线*/
        text-decoration: none;
    }

    /*访问过的链接*/
    a:visited {
        color: grey;
    }

    /*正在点击状态*/
    a:active {
        color: yellow;
    }

    /*鼠标悬停状态*/
    a:hover {
        color: red;
        /*显示下划线*/
        text-decoration: underline;
    }

    /*文本框得到焦点，背景色发生变化*/
    #user:focus {
        background-color: yellow;
    }
</style>
<a href="#1">点我试试</a><br/>
<a href="#2">点我试试</a><br/>
<a href="#3">点我试试</a><br/>
<a href="#4">点我试试</a><br/>
<hr/>
<!--
获得焦点：获得了一个可以操作的状态，正在操作这个元素
focus 焦点
-->
用户名：<input type="text" id="user">

<h2>并集选择器</h2>
<!--
同时多个选择器起作用，选中的元素是所有选择器的和
语法： 选择器1,选择器2,选择器3
-->
<style>
    p,div {
        color: orange;
    }
</style>
<p>好好学习</p>
<div>天天向上</div>

<h2>交集选择器</h2>
<!--
几个选择器的交集，共同的部分被选中
语法：注不能有空格
1. 标签名#ID  既是这个标签，又有一个ID
2. 标签名.类名 既是这个标签，又有一个类名
-->
<style>
    p#one {
        color: green;
    }

    p.two {
        color: blue;
    }
</style>
<p id="one">刘备</p>
<p class="two">关羽</p>

<!--<div id="one">张飞</div>-->
<div class="two">周瑜</div>
</body>
</html>
```

### 小结

![1562745616606](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1562745616606.png)



## 常见样式：背景样式

### 目标

学习背景样式有哪些属性

### 样式名和值

![1551772698804](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1551772698804.png)

### 效果

![1551772735047](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1551772735047.png) 

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>背景有关的样式</title>
    <style>
        body {
            /*背景色*/
            /*background-color: yellow;
            !*背景图片*!
            background-image: url("img/girl1.jpg");
            !*平铺的方式*!
            background-repeat: repeat;
            !*平铺的起始位置*!
            background-position: 50px 50px;*/
            
            /*设置背景的大小*/
            background-size: 200px;

            /*缩写*/
            background: url("img/girl2.jpg") 50px 50px yellow no-repeat;
        }
    </style>
</head>
<body>
<h2>骑车的女生</h2>
</body>
</html>
```

### 小结

| 属性名              | 属性取值       |
| ------------------- | -------------- |
| background-color    | 背景颜色       |
| background-image    | 背景图片       |
| background-repeat   | 平铺的方式     |
| background-position | 起始平铺位置   |
| background-size     | 背景图片的大小 |



## <font color="red">常用样式：文本样式和字体样式</font>

### 目标

1. 文本样式
2. 字体样式
3. 制作案例：诗歌

### 文本样式

![1552181469390](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1552181469390.png)

### 字体样式

![1551774415181](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1551774415181.png)

### 案例：诗歌

![1551774444336](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1551774444336.png)

### 步骤

1.	诗放在div的层中，宽400px，全文字体大小14px;
2.	标题放在h1中。每段文字放在p中
3.	标题文字大小18px，颜色#06F，居中对齐
4.	每段文字缩进2em
5.	段中的行高是22px
6.	"胸怀中满溢着幸福，只因你就在我眼前",加粗，倾斜，蓝色
7.	最后一段，颜色#390; 下划线，鼠标移上去指针变化。
8.	美字加粗，颜色color:#F36，大小18px;
9.	文席慕容，三个字，12px，颜色#999，不加粗

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>初相遇</title>
    <style>
        /*1. 诗放在div的层中，宽400px，全文字体大小14px;*/
        div {
            width: 400px;
            font-size: 14px;
        }

        /*2. 标题放在h1中。每段文字放在p中*/
        /*3. 标题文字大小18px，颜色#06F，居中对齐*/
        h1 {
            font-size: 18px;
            color: #06f;
            text-align: center;
        }

        p {
            /*4. 每段文字缩进2em*/
            text-indent: 2em;
            /*5. 段中的行高是22px*/
            line-height: 22px;
        }

        /*6. "胸怀中满溢着幸福，只因你就在我眼前",加粗，倾斜，蓝色*/
        .xiong {
            font-weight: bolder;
            font-style: italic;
            color: blue;
        }

        /*7. 最后一段，颜色#390; 下划线，鼠标移上去指针变化。*/
        p:last-child {
            color: #390;
            text-decoration: underline;
            cursor: pointer;
        }

        /*8. 美字加粗，颜色color:#F36，大小18px;*/
        #mei {
            font-weight: bolder;
            color: #f36;
            font-size: 18px;
        }

        /*9. 文席慕容，三个字，12px，颜色#999，不加粗*/
        h1 span {
            font-size: 12px;
            color: #999;
            font-weight: normal;
        }
    </style>
</head>
<body>
<div>
    <h1>初相遇 <span>文/席慕容</span></h1>
    <p><span id="mei">美</span>丽的梦和美丽的诗一样，都是可遇而不可求的，常常在最没能料到的时刻里出现。</p>
    <p>我喜欢那样的梦，在梦里，一切都可以重新开始，一切都可以慢慢解释，心里甚至还能感觉到，所有被浪费的时光竟然都能重回时的狂喜与感激。
        <span class="xiong">胸怀中满溢着幸福，只因你就在我眼前，</span>对我微笑，一如当年。</p>
    <p>我喜欢那样的梦，明明知道你已为我跋涉千里，却又觉得芳草鲜美，落落英缤纷，好像你我才初相遇。</p>
</div>
</body>
</html>
```



### 小结

| 属性名          | 作用     |
| --------------- | -------- |
| color           | 颜色     |
| line-height     | 行高     |
| text-decoration | 下划线   |
| text-indent     | 缩进     |
| text-align      | 对齐方式 |

| 属性名      | 作用 |
| ----------- | ---- |
| font-family | 字体 |
| font-size   | 大小 |
| font-style  | 斜体 |
| font-weight | 加粗 |



## 上午的回顾

### CSS三种位置

1. 行内样式：只在一行中起作用

2. 内部样式：在一个HTML文件中起作用

3. 外部样式：可以用在多个HTML中，使用的时候必须导入

   ```
   <link href="外部CSS文件" rel="stylesheet" type="text/css"/>
   ```

   

### 4种基本选择器

1. \#ID
2. .类
3. 标签名
4. 通用 *



### 5种扩展选择器

1. 层级： 父选择器 子孙选择器

2. 属性： 标签名[属性名="值"]

3. 伪类：  标签: 状态

   ```
   a:link  a:active a:visited a:hover
   元素:focus
   元素:last-child
   ```

   

4. 并集： 选择器1,选择器2

5. 交集：同时符合2个条件

   ```
   标签名#ID
   标签名.类名
   ```

   



### 背景样式

```
background-color
background-image
background-repeat
background-postion
background-size
```



### 文本样式

```
color
line-height
text-decoration
text-indent
text-align
```



### 字体样式

```
font-family
font-size
font-style
font-weight
```



## 两种盒子模型

### 目标

​	两种盒子模型的区别

### 概述

![1571553397625](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1571553397625.png)

​	任何一个网页元素包含由这些属性组成：内容(content)、内边距(padding)、边框(border)、外边距(margin)， 这些属性我们可以用日常生活中的常见事物——盒子作一个比喻来理解，所以叫它盒子模型。

- 内容（content）就是盒子里装的东西
- 内边距(padding)就是怕盒子里装的东西（贵重的）损坏而添加的泡沫或者其它抗震的辅料；
- 边框(border)就是盒子本身了；
- 外边界(margin)则说明盒子摆放的时候的不能全部堆在一起，要留一定空隙保持通风，同时也为了方便取出。

### 盒子模型的属性

![1551774655949](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1551774655949.png)

### 如何计算盒子的尺寸

​	在盒子模型中，最重要的还是如何理解元素的实际尺寸。
 	盒子模型分为两种，分别是：标准盒模型 和 怪异盒模型。绝大多数元素的尺寸默认是按照标准盒模型计算的。下面是元素的尺寸分别在两种盒模型下计算规则：

| box-sizing 样式名 | 说明                                                         |
| ----------------- | ------------------------------------------------------------ |
| content-box       | 标准盒子模型：尺寸会被内边距，边框的大小撑大                 |
| border-box        | 怪异盒子模型：尺寸一旦设置好，固定的。内容会被内边距，边框缩小 |

### 标准盒子模型

![1551774810940](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1551774810940.png)

### 怪异盒子模型

![1551774834485](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1551774834485.png)

### 计算方式尺寸

```
标准盒子：
实际宽度=width+左右内边距+左右边框
实际高度=height+上下内边距+上下的边框
```

```
怪异盒子：
实际宽度=width
实际高度=height
```

### 案例：盒子模型

#### 效果

![1551774996564](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1551774996564.png) 

#### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>2种盒子模型</title>
</head>
<body>
<h2>标准盒子</h2>
<style>
    .test1 {
        width: 200px;
        height: 200px;
        /*边框参数：线型 粗细 颜色*/
        border: solid 15px blue;
        /*内边距：4个方向距离都是一样的 */
        padding: 10px;
        /*标准盒子，默认*/
        box-sizing: content-box;
    }
</style>

<div class="test1">
</div>

<h2>怪异盒子</h2>
<style>
    .test2 {
        width: 200px;
        height: 200px;
        /*边框参数：线型 粗细 颜色*/
        border: solid 15px green;
        /*内边距：4个方向距离都是一样的 */
        padding: 10px;
        /*怪异盒子*/
        box-sizing: border-box;
    }
</style>

<div class="test2">

</div>
</body>
</html>
```



### 小结

| 属性    | 作用                                |
| ------- | ----------------------------------- |
| width   | 宽度                                |
| height  | 高度                                |
| margin  | 外边距                              |
| padding | 内边距                              |
| border  | 边框：线型 粗细 颜色 (三个顺序随意) |

| 盒子模型的样式名 | 样式值            |
| ---------------- | ----------------- |
| box-sizing       | 标准：content-box |
| box-sizing       | 怪异：border-box  |



## 常用样式：边框有关

### 目标

边框有哪些属性和取值

### 边框的样式

![1551775156839](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1551775156839.png)

### 内边距和外边距的写法

| 内边距的写法            | 含义     |
| ----------------------- | -------- |
| padding-top:10px;       | 内上边距 |
| padding-left:10px;      | 内左边距 |
| padding-bottom:   10px; | 内下边距 |
| padding-right:10px;     | 内右边距 |

| 外边距的写法           | 含义     |
| ---------------------- | -------- |
| margin-top:10px;       | 外上边距 |
| margin-left:10px;      | 外左边距 |
| margin-bottom:   10px; | 外下边距 |
| margin-right:10px;     | 外右边距 |

### 案例：边框的使用

#### 效果

![1551775249504](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1551775249504.png)

### 步骤

1. 盒子模型宽和高分别是200px，边框使用double线形，红色，1px粗细
2. 上内边距10px，右内边距20px，下内边距30px，左内边距40px
3. 使用缩写的1句话的方式实现
4. 对div整个居中

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>边框样式</title>
</head>
<body>
<style>
    #box {
        width: 200px;
        height: 200px;
        /*线型 粗细 颜色*/
        /*border: dotted 5px red;*/
        border-style: double;
        border-width: 5px;
        border-color: red;
        /*边框圆角*/
        border-radius: 10px;

        /*内边距*/
        /*4个方向相同*/
        padding: 10px;
        /*上下  左右*/
        /*padding: 10px 20px;*/
        /*上  左右  下*/
        /*padding: 10px 20px 30px;*/
        /*上 右 下 左*/
        /*padding: 10px 20px 30px 40px;*/

        /*padding-top: 10px;
        padding-right: 20px;
        padding-bottom: 30px;
        padding-left: 40px;*/

        /*外边距自动*/
        margin: 0px auto;
    }

    body {
        padding: 0px;
        margin: 0px;
    }
</style>
<div id="box">
    我喜欢那样的梦，在梦里，一切都可以重新开始，一切都可以慢慢解释，心里甚至还能感觉到，
    所有被浪费的时光竟然都能重回时的狂喜与感激。 胸怀中满溢着幸福，只因你就在我眼前，对我微笑，一如当年。
</div>
</body>
</html>
```

### 关于块级元素居中

![1551775342231](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1551775342231.png) 

### 小结

| 属性          | 边框样式 |
| ------------- | -------- |
| border-style  | 设置线型 |
| border-width  | 粗细     |
| border-color  | 颜色     |
| border-radius | 圆角     |



## 案例：用户的注册

### 目标

使用CSS样式排版出如图所示的效果

### 效果

![1551775397597](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1551775397597.png) 

### 步骤

1. 表格有四行，第一行和第四行跨2列，第一列占30%的宽度，第一行是标题th，第四行是放按钮。使用图片按钮
2. table居中,宽300px，高180px; 边框solid 1px gray
3. td的文字对齐居中，字体大小14px
4. table添加背景，不平铺，图片背景的宽度和高度与table的宽和高一样。
5. 用户名和密码文本框使用类样式，也可以使用其它选择器。宽150px，高32px，边框用实线，圆角5px，1个宽度，黑色
6. 使用伪类样式，当鼠标移动到文本框上的时候，变成虚线橙色边框。得到焦点，背景色变成浅黄色
7. 文本框前面有头像，密码框前面有键盘，左内边距35

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>黑马旅游注册</title>
    <style>
        /*table居中,宽300px，高180px; 边框solid 1px gray*/
        table {
            width: 300px;
            height: 180px;
            border: solid 1px gray;
            /*居中*/
            margin: auto;
            /*圆角*/
            border-radius: 10px;
            /*设置背景图片*/
            background-image: url("img/bg1.jpg");
            background-size: 300px 180px;
        }

        td {
            text-align: center;
        }

        /*宽150px，高32px，边框用实线，圆角5px，1个宽度，黑色*/
        #user,#password {
            width: 150px;
            height: 32px;
            border: solid 1px black;
            border-radius: 5px;
        }

        #user {
            background-image: url("img/head.png");
            background-repeat: no-repeat;
            /*设置左内边距35px*/
            padding-left: 35px;
        }

        #password {
            background-image: url("img/keyboard.png");
            background-repeat: no-repeat;
            /*设置左内边距35px*/
            padding-left: 35px;
        }

        /*得到焦点，背景色发生变化*/
        #user:focus,#password:focus {
            background-color: yellow;
        }

        /*鼠标移上，边框发生变化*/
        #user:hover,#password:hover {
            border: 1px orange dashed;
        }
    </style>
</head>
<body>
<table>
    <tr>
        <th colspan="2">黑马旅游注册</th>
    </tr>

    <tr>
        <td>用户名：</td>
        <td>
            <input type="text" id="user" placeholder="请输入用户名"/>
        </td>
    </tr>

    <tr>
        <td>密码：</td>
        <td>
            <input type="password" id="password" placeholder="请输入密码"/>
        </td>
    </tr>

    <tr>
        <td colspan="2">
            <input type="image" src="img/regbtn.jpg">
        </td>
    </tr>

</table>
</body>
</html>
```



## JavaScript概述和体验

### 目标

1. JavaScript的作用
2. 编写第1个JavaScript代码

### JavaScript的基本概述

​	在1995年时，由Netscape公司在网景导航者浏览器上首次设计实现而成。因为Netscape与Sun合作，Netscape管理层希望它外观看起来像Java，因此取名为JavaScript。
​	JavaScript是一种属于网络的脚本语言,已经被广泛用于Web应用开发,常用来为网页添加各式各样的动态功能,为用户提供更流畅美观的浏览效果。

运行在浏览器端的脚本编程语言，让网页与用户之间有交互的功能，让网页活起来。



### 为什么要JavaScript

![1551945857805](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1551945857805.png)

### 网页中各技术的作用

| 技术       | 作用                     |
| ---------- | ------------------------ |
| HTML       | 用于网页结构制作         |
| CSS        | 用于网页美化             |
| JavaScript | 让网页与用户有交互的功能 |



### JS体验案例

#### 需求

​	使用JS在网页上输出5个Hello World

#### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>javascript初相遇</title>
</head>
<body>
<!--所有的JS代码必须放在script标签中才可以执行-->
<script type="text/javascript">
    for (var i = 0; i < 5; i++) {
        document.write("<h1>Hello World</h1>");
    }
</script>
</body>
</html>
```

### JavaScript与Java的区别

| 特点     | Java                 | JavaScript                                 |
| -------- | -------------------- | ------------------------------------------ |
| 面向对象 | 面向对象编程         | 基于对象，不完全面向对象                   |
| 运行方式 | 编译型，生成中间文件 | 解释型，直接解析源代码                     |
| 跨平台   | 运行在虚拟机中       | 运行在浏览器，只要系统有浏览器就可以执行   |
| 数据类型 | 强类型语言           | 弱类型，同一个变量可以赋值为不同的数据类型 |
| 大小写   | 区分                 | 区分                                       |

### 小结

1. 为什么要JS？

   ```
   让网页活起来，提高用户操作体验
   ```

   

2. 说说网页上各种技术的作用：HTML，CSS，JS。

   ```
   HTML：网页结构
   CSS：美化
   JS：网页的脚本语言
   ```

   



## script标签的使用说明

### 目标

1. JS语言的三个组成部分
2. \<script>标签的说明

### JavaScript语言组成

| 组成部分    | 作用                                                         |
| ----------- | ------------------------------------------------------------ |
| ECMA Script | 所有网页脚本语言的标准，构成了JS语言的核心基础               |
| BOM         | Browser Object Model 浏览器对象模型，用来操作浏览器中对象    |
| DOM         | Document Object Model 文档对象模型，用来操作网页中各种标签和元素 |

### script标签的说明

1. 标签个数：可以出现多个，每个标签都会依次执行。
2. 位置：可以出现在网页的任何位置，并且正常的执行。
3. 语句后分号：如果一行只有一条代码就可以省略

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>javascript初相遇</title>
</head>
<body>
<script type="text/javascript" src="js/out.js"></script>

<!--所有的JS代码必须放在script标签中才可以执行-->
<script type="text/javascript">
    for (var i = 0; i < 5; i++) {
        document.write("<h1>Hello World</h1>");
        if (i == 2) {
            document.write("Hello JavaScript");
        }
    }
</script>

<!--导入外部脚本：script标签必须是一个有主体的标签 -->
<script type="text/javascript" src="js/out.js"></script>
</body>
</html>
```

```javascript
document.write("外部脚本" + "<br/>");
```

### JavaScript的注释

![1551946466936](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1551946466936.png) 

### 小结

1. JS由哪三个部分组成？

   ```
   ECMA Script
   DOM
   BOM
   ```

   

2. script标签有哪两个属性？

   ```
   type="text/javascript"
   src="指定外部脚本文件"
   ```

   



## 变量的定义

### 目标

1. 变量的定义的语法
2. var关键字的使用

### 定义语法

| 数据类型 | Java中定义变量                    |
| -------- | --------------------------------- |
| 整数     | int i = 5;                        |
| 浮点数   | float f = 3.14; 或 double d=3.14; |
| 布尔     | boolean b = true;                 |
| 字符     | char c = 'a';                     |
| 字符串   | String str =   "abc";             |

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>变量的定义</title>
</head>
<body>
<script type="text/javascript">
    var i = 5;  //整数
    document.write(i + "<br/>");

    var f = 3.14;  //浮点数
    document.write(f  + "<br/>");

    var b = true; //布尔
    document.write(b + "<br/>");

    //在JS中只有字符串类型，没有字符类型，字符串既可以使用单引号，也可以使用双引号。
    var c = 'a';    //字符串
    document.write(c  + "<br/>");

    var str = "abc";  //字符串
    document.write(str + "<br/>");
</script>
</body>
</html>
```



### 注意事项

1. 关于弱类型？

   ```
   同一个变量可以赋不同类型的值
   ```

2. 在JS中的字符和字符串引号？

   ```
   在JS中只有字符串类型，没有字符类型，字符串既可以使用单引号，也可以使用双引号。
   ```

3. var定义变量的特点

   1. var关键字不是必须的，如果函数内的变量不写var，默认是全局变量。
   2. 变量可以重复定义
   3. 函数之外的大括号不能限制变量的范围

### 案例

#### 需求

​	输出不同类型的数据

#### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>变量的定义</title>
</head>
<body>
<script type="text/javascript">
    var i = 5;  //整数
    document.write(i + "<br/>");

    var i = false; //布尔
    document.write(i + "<br/>");

    var f = 3.14;  //浮点数
    document.write(f  + "<br/>");
    {
        var b = true; //布尔
    }
    document.write(b + "<br/>");

    //在JS中只有字符串类型，没有字符类型，字符串既可以使用单引号，也可以使用双引号。
    c = 'a';    //字符串
    document.write(c  + "<br/>");

    var str = "abc";  //字符串
    document.write(str + "<br/>");
</script>
</body>
</html>
```

### 小结

1. 在JS中定义变量使用哪个关键字？ var
2. JS是弱类型还是强类型？  弱
3. 有没有字符和字符串的区别？ 没有



## 五种数据类型

### 目标

JS中有哪五种数据类型

### 五种数据类型

| 关键字    | 说明                                               |
| --------- | -------------------------------------------------- |
| number    | 数值型(整数和浮点数)                               |
| boolean   | true/false                                         |
| string    | 包含字符和字符串，既可以使用双引号也可以使用单引号 |
| object    | 对象类型                                           |
| undefined | 未定义的类型，未知的类型                           |

### typeof操作符

作用：判断一个变量的数据类型

写法：typeof(变量名)  或  typeof 变量名

### 案例：数据类型的演示

#### 需求

分别输出整数、浮点数、字符串(单引号和双引号)、布尔、未定义、对象、null的数据类型

#### 效果

![1571560013560](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1571560013560.png) 

#### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>判断变量的类型</title>
</head>
<body>
<script type="text/javascript">
    var i = 5;  //整数 number
    document.write(typeof(i) + "<br/>");

    var f = 3.14;  //浮点数 number
    document.write(typeof(f) + "<br/>");

    var b = true; //布尔 boolean
    document.write(typeof b+ "<br/>");

    var c = 'a';    //字符串 string
    document.write(typeof(c) + "<br/>");

    var str = "abc";  //字符串  string
    document.write(typeof str+ "<br/>");

    var date = new Date();   //JS内置对象，代表日期对象  object
    document.write(date + "<br/>");
    document.write(typeof date+ "<br/>");

    var n = null;  //object
    document.write(typeof n+ "<br/>");

    var u;    //undefined
    document.write(typeof u+ "<br/>");
</script>
</body>
</html>
```

### 小结

| null与undefined的区别 | 说明                                     |
| --------------------- | ---------------------------------------- |
| null                  | 表示这是一个Object类型，但是这个对象为空 |
| undefined             | 不知道它是什么类型，未知的类型           |



## 数据类型的转换函数

### 目标

学习JS中类型转换函数

### 全局函数

概念：一个函数可以在JS任何的地方调用

### 案例：字符串转数字

#### 需求：

1. 创建两个字符串的变量，保存2个数字
2. 对两个数字进行加的运算，输出它们的计算结果
3. 使用两个小数再进行计算
4. 判断一个字符串能否转换成数字，看看输出结果

#### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>转换函数</title>
</head>
<body>
<script type="text/javascript">
    var num1 = "6.2";
    var num2 = "3.1";
    /*
    parseInt(变量名)：转成整数类型
    1. 对字符串进行转换，能转的就直接转换，不能转换的，转换前面一部分。
    2. 如果不能转换的，得到NaN = Not a Number
     */
    //document.write(parseInt(num1) + parseInt(num2)+ "<br/>");

    /*
    parseFloat(变量名)：转成小数，功能与上面的是一样的
     */
    document.write(parseFloat(num1) + parseFloat(num2)+ "<br/>");

    /*
         is Not a Number
     *  isNaN(变量名)：判断变量名是否为非数字，如果非数字，返回true，否则返回false
     *  如："abc" 返回true  "123"返回false
     */
    document.write(isNaN("abc") + "<br/>");   //true
    document.write(isNaN("123") + "<br/>");   //false
    document.write(isNaN("123a") + "<br/>");   //true
</script>
</body>
</html>
```

### 小结

| 转换函数     | 作用                                                         |
| ------------ | ------------------------------------------------------------ |
| parseInt()   | 转成整数类型，如果字符串前面的部分可以转换就转换前面部分，<br />遇到第一个不能转换的字符就停止 |
| parseFloat() | 转成浮点数，转换功能同上                                     |
| isNaN()      | 判断是否为非数字，非数字返回true                             |



## 在浏览器中调试代码

### 目标

如何在浏览器中调试JS代码

### 设置断点

注：设置断点以后要重新刷新页面才会在断点停下来

![1551953129121](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1551953129121.png)

### 语法错误

![1551947495069](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1551947495069.png) 



## 常用的运算符

### 目标

学习JS中的各种运算符

### 算术运算符

算术运算符用于执行两个变量或值的算术运算

![1551947559080](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1551947559080.png) 

### 赋值运算符

赋值运算符用于给JavaScript 变量赋值

![1551947658451](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1551947658451.png) 

### 比较运算符

比较运算符用于逻辑语句的判断，从而确定给定的两个值或变量是否相等。

![1551947697070](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1551947697070.png) 

数字可以与字符串进行比较，字符串可以与字符串进行比较。字符串与数字进行比较的时候会先把字符串转换成数字然后再进行比较

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>运算符</title>
</head>
<body>
<script type="text/javascript">
    var n1 = 5;
    var n2 = "5";
    var n3 = 4;
    //在比较运算符中字符串会转换成整数类型进行比较
    document.write((n1 == n2) + "<br/>");  //true
    document.write((n3 < n2) + "<br/>");  //true
    //恒等于：既比较值，又比较类型
    document.write((n1 === n2) + "<br/>");  //false
    document.write((n2 === "5") + "<br/>");  //true
</script>
</body>
</html>
```

### 逻辑运算符

逻辑运算符用来确定变量或值之间的逻辑关系，支持短路运算

![1551947786567](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1551947786567.png) 

逻辑运算符不存在单与&、单或|

### 三元运算符

![1551947843411](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1551947843411.png) 

### 小结

- 运算符 === 有什么作用？

  ```
  恒等于，既比较值，又比较类型
  ```

  



## 流程控制：if 语句

### 目标

JS中if语句使用非boolean类型的条件

### if 语句：

在一个指定的条件成立时执行代码。 

```
if(条件表达式) {
     //代码块;
}
```



### if...else 语句

在指定的条件成立时执行代码，当条件不成立时执行另外的代码。

```
if(条件表达式) {
     //代码块;
 }
 else {
     //代码块;
 }
```



### if...else if....else 语句

使用这个语句可以选择执行若干块代码中的一个。

```
if (条件表达式) {
     //代码块;
 }
 else if(条件表达式) {
     //代码块;
 }
 else {
     //代码块;
 }
```

### 非布尔类型的表达式

| 数据类型            | 为真   | 为假 |
| ------------------- | ------ | ---- |
| number              | 非0    | 0    |
| string              | 非空串 | 空串 |
| undefined           |        | 假   |
| NaN(Not a   Number) |        | 假   |
| object              | 有对象 | null |

### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>if语句</title>
</head>
<body>
<script type="text/javascript">
    /*if (0) {
        document.write("真" + "<br/>");
    }
    else {
        document.write("假" + "<br/>");
    }*/

    /*
    if ("abc") {
        document.write("真" + "<br/>");
    }
    else {
        document.write("假" + "<br/>");
    }
    */

    /*
    var u;
    if (u) {
        document.write("真" + "<br/>");
    }
    else {
        document.write("假" + "<br/>");
    }*/

    /*
    var num = parseInt("abc");  //NaN
    if (num) {
        document.write("真" + "<br/>");
    }
    else {
        document.write("假" + "<br/>");
    }
    */

    var date = null;
    if (date) {
        document.write("真" + "<br/>");
    }
    else {
        document.write("假" + "<br/>");
    }
</script>
</body>
</html>
```

### 小结

1. string中空串表示真还是假？假
2. 数字哪个是真，哪个是假? 0假，非0为真
3. object哪个是真，哪个是假？ 非null真，null假



## 流程控制：switch语句

### 目标

1. switch语句与Java的区别
2. window对象的方法

### 语法一：case后使用常量，与Java相同

```
switch(变量名) {
   case 常量值:
      break;
   case 常量值：
      break;
   default:
       break;
 }
```



### 语法二：case后使用表达式

```
switch(true) {  //这里的变量名写成true
   case 表达式:     //如：n > 5
     break;
   case 表达式:
     break;
   default:
     break;
 }
```

### 相关的方法

| window对象的方法名                  | 作用                                   |
| ----------------------------------- | -------------------------------------- |
| string  prompt("提示信息","默认值”) | 输入框<br />1. 提示信息<br />2. 默认值 |
| alert("提示信息")                   | 信息提示框                             |

#### 输入框

![1562746273460](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1562746273460.png) 

#### 信息提示框

![1562746197150](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1562746197150.png) 

注：window对象

#### 代码

```html

```



### 案例：判断一个学生的等级

​	通过prompt输入的分数，如果90~100之间，输出优秀。80~90之间输出良好。60~80输出及格。60以下输出不及格。其它分数输出:分数有误。

#### 效果

![1551948291900](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1551948291900.png) 

#### 分析

1. 使用prompt得到输入的分数
2. 使用switch对分数进行判断
3. 如果在90到100之间，则输出优秀，其它依次类推。

#### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script type="text/javascript">
    // 如果90~100之间，输出优秀。80~90之间输出良好。60~80输出及格。60以下输出不及格。其它分数输出:分数有误。
    var score = window.prompt("请输入您的分数：", "60");

    if (isNaN(score)) {  //判断是否非数字
        alert("输入有误");
    }
    else {
        document.write("分数：" + score + "<br/>");
        switch (true) {
            case score >=90 && score <=100:
                document.write("优秀" + "<br/>");
                break;
            case score >=80 && score <90:
                document.write("良好" + "<br/>");
                break;
            case score >=60 && score <80:
                document.write("及格" + "<br/>");
                break;
            case score >=0 && score <60:
                document.write("不及格" + "<br/>");
                break;
            default:
                document.write("输入有误" + "<br/>");
                break;
        }
    }

</script>
</body>
</html>
```

### 小结

​	如果switch的case后面要使用表达式，switch括号中写成什么？ true



## 流程控制：循环语句

### 目标

使用循环实现9x9乘法表

### while语句：

当指定的条件为 true 时循环执行代码

```
while (条件表达式) {
     需要执行的代码;
 }
```



### do-while语句：

最少执行1次循环

```
do {
     需要执行的代码;
 }
 while (条件表达式)
```



### for 语句

循环指定次数

```
for (var i=0; i<10; i++) {
     需要执行的代码;
 }
```



### break和continue

```
break: 结束整个循环
continue：跳过本次循环，执行下一次循环
```



### 案例：乘法表

#### 需求

以表格的方式输出乘法表，其中行数通过用户输入

#### 效果

![1551950186004](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/CSS&JSP/1551950186004.png) 

#### 分析

1. 先制作一个没有表格，无需用户输入的9x9乘法表
2. 由用户输入乘法表的行数
3. 使用循环嵌套的方式，每个外循环是一行tr，每个内循环是一个td
4. 输出每个单元格中的计算公式
5. 给表格添加样式，设置内间距

#### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>乘法表</title>
    <style>
        table {
            border-collapse: collapse;
        }

        td {
            padding: 4px;
        }
    </style>
</head>
<body>
<script type="text/javascript">
    //只要是window对象的方法和属性，window都可以省略
    var num = prompt("请输入乘法表的行数：", "9");

    document.write("<table border='1'>");
    document.write("<caption>" + num + "&times;" + num+"乘法表</caption>");
    //num是字符串类型，会自动转成数字类型
    for (var i = 1; i <= num; i++) {   //外循环每次是一行
        document.write("<tr>");
        for (var j = 1; j <= i; j++) {
            //每个是一个td
            document.write("<td>");
            document.write(j + "&times;" + i + "=" + (i * j));
            document.write("</td>");
        }
        document.write("</tr>");
    }
    document.write("</table>");
</script>
</body>
</html>
```





# 学习总结

1. 能够使用CSS的基本选择器选择元素

| 选择器分类 | 语法   |
| ---------- | ------ |
| 标签选择器 | 标签名 |
| 类选择器   | .类名  |
| ID选择器   | \#ID   |
| 通用选择器 | *      |

2. 能够使用CSS的扩展选择器选择元素

| 扩展选择器 | 语法符号                                           |
| ---------- | -------------------------------------------------- |
| 层级选择器 | 父选择器 子孙选择器                                |
| 属性选择器 | 标签名[属性名="值"]                                |
| 伪类选择器 | 选择器:状态   focus,  link, hover, active, vistied |
| 并集选择器 | 选择器,选择器                                      |
| 交集选择器 | 标签名.类名   标签名#ID                            |

3. 能够使用常见的CSS属性

| 属性名              | 属性取值       |
| ------------------- | -------------- |
| background-color    | 背景色         |
| background-image    | 背景图片       |
| background-repeat   | 平铺方式       |
| background-position | 起始平铺的位置 |
| background-size     | 背景大小       |

| 属性名          | 属性取值 |
| --------------- | -------- |
| color           | 设置颜色 |
| line-height     | 行高     |
| text-decoration | 下划线   |
| text-indent     | 缩进     |
| text-align      | 对齐方式 |

| 属性名      | 作用 |
| ----------- | ---- |
| font-family | 字体 |
| font-size   | 大小 |
| font-style  | 斜体 |
| font-weight | 加粗 |

4. 能够说出盒子模型的属性

| 属性    | 作用                 |
| ------- | -------------------- |
| width   | 宽度                 |
| height  | 高度                 |
| margin  | 外边距               |
| padding | 内边距               |
| border  | 边框：线形 颜色 粗细 |

5. 能够说出JS中五种数据类型

| 关键字    | 说明       |
| --------- | ---------- |
| number    | 数值       |
| boolean   | 布尔       |
| string    | 字符串     |
| object    | 对象       |
| undefined | 未初始化的 |

6. 能够使用JS中常用的运算符

   1. 算术运算符
   2. 赋值运算符
   3. 比较运算符  ===
   4. 逻辑运算符
   5. 三元运算符

   

7. 能够使用JS中的流程控制语句

   1. if  可以使用非布尔类型的表达式
   2. switch  (true)  case后可以使用表达式
   3. for
   4. while

   

