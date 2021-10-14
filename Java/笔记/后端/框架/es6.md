## 课程目标

目标1: 能够按照文档安装Node.js 

目标2: 能够使用npm安装js库

目标3: 能够使用webpack打包js

目标4: 能够说出es6中let和const变量的区别

目标5: 使用解构表达式处理对象属性

目标6: 能够使用export导出模块文件

目标7: 能够使用import导入模块文件



## 01、nodejs介绍&安装

> 目标：了解Nodejs、并且在本地安装好Nodejs

### 1.1 Nodejs简介

![1572876304376](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572570713280.png)

+ 简单的说 Node.js 就是运行在服务端的 JavaScript。 
+ Node.js 是一个基于Chrome JavaScript 运行时建立的一个平台。 
+ Node.js是一个事件驱动I/O服务端JavaScript环境，基于Google的V8引擎，V8引擎执行Javascript的速度非常快，性能非常好。

### 1.2 Nodejs安装

+ 下载对应你系统的Node.js版本

  官网地址: https://nodejs.org 中文网站: http://nodejs.cn `安装包` 文件夹中已经提供。

  ![1572876304376](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572876304376.png) 

+ 安装步骤截图

  ![1572876349702](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572876349702.png)&nbsp;

  ![1572876386317](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572876386317.png)&nbsp;

  ![1572876453560](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572876453560.png)&nbsp;

  ![1572876567757](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572876567757.png)&nbsp;

  ![1572876604275](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572876604275.png)&nbsp;

  ![1572876642617](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572876642617.png)&nbsp;

  ![1572876665343](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572876665343.png)&nbsp;

+ 检查是否安装好,控制台查看版本号: node -v

  ![1572876974447](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572876974447.png)

## == nodejs快速入门 ==

### 02、控制台输出&函数&模块化编程

> 目标: 控制台输出字符串、使用函数、进行模块化编程

+ 创建测试模块

  ![1572878444556](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572878444556.png)

  ![1572878480639](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572878480639.png)

+ 控制台输出

  + 创建demo1.js

    ```javascript
    var a = 100;
    var b = 200;
    console.log(a + b);
    ```

  + 控制台输出

    ```javascript
    node demo1.js
    ```

    ![1572878934175](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572878934175.png)

+ 使用函数

  + 创建demo2.js

    ```javascript
    // 定义函数
    function add(a,b) {
        return a + b;
    }
    // 调用函数
    var c = add(200,200);
    // 控制台输出
    console.log(c);
    ```

  + 控制台输出

    ![1572879681343](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572879681343.png)

+ 模块化编程

  每个js文件就是一个模块，有自己的作用域。在一个文件里面定义的变量、函数都是私有的。

  + 创建demo3_1.js

    ```javascript
    // 模块化函数
    exports.add = function (a, b) {
        return a + b;
    }
    ```

    > 每个模块内部，module变量代表当前模块。这个变量是一个对象，它的exports属性（即module.exports）是对外的接口。加载某个模块，其实是加载该模块的module.exports属性。 

  + 创建demo3_2.js

    ```javascript
    // 引入模块
    var demo = require('./demo3_1'); 
    // 调用模块中的函数
    console.log(demo.add(500,300));
    ```

    ![1572916604283](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572916604283.png)&nbsp;



### 03、创建Web服务，输出字符串

#### 3.1 输出一行字符串

+ 创建demo4.js 

  参考在线中文api文档: <http://nodejs.cn/api/http.html> 

  ```javascript
  // 引入http模块 (http是内置模块)
  var http = require('http');
  // 创建web服务
  var server = http.createServer(function(request,response){
      // 写HTTP 头部
      // HTTP 状态值: 200 : OK
      // 内容类型: text/plain
      response.writeHead(200,{
          'Content-Type': 'text/plain;charset=UTF-8'
      });
      // 发送响应数据,关闭响应对象
      response.end('nodejs 我轻轻的来了！');
  });
  // 监听端口
  server.listen(8888);
  // 终端提示信息
  console.log("亲，web服务跑起来了.......");
  ```

  > 说明：http为node内置的web模块

+ 启动测试

  ![1572917800328](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572917800328.png)&nbsp;

  打开浏览器，输入网址: http://localhost:8888

  ![1572917838266](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572917838266.png)&nbsp;

  > 说明: 在命令行中按 **Ctrl+c** 终止运行。

#### 3.2 输出多行字符串

+ 创建demo5.js

  ```js
  // 引入http模块 (http是内置模块)
  var http = require('http');
  // 创建web服务
  var server = http.createServer(function(request,response){
      // 发送 HTTP 头部
      // HTTP 状态值: 200 : OK
      // 内容类型: text/plain
      response.writeHead(200,{
          'Content-Type': 'text/plain;charset=UTF-8'
      });
  
      // 循环往浏览器写多行字符串
      for (var i = 1; i <= 10; i++) {
          response.write("Hello World\n");
      }
  
      // 关闭响应对象
      response.end('');
  });
  // 监听端口
  server.listen(8888);
  // 终端提示信息
  console.log("亲，web服务跑起来了.......");
  ```

+ 启动测试

  ![1572918410103](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572918410103.png)&nbsp;

  ![1572918429637](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572918429637.png)&nbsp;

  > 右键“查看源代码”发现，并没有我们写的for循环语句，而是直接的10条Hello World ，这就说明这个循环是在服务端完成的，而非浏览器（客户端）来完成。这与JSP很是相似。



### 04、创建Web服务，接收请求参数

+ 创建demo6.js

  ```js
  // 引入http模块 (内置模块)
  var http = require('http');
  // 引入url模块 (内置模块)
  var url = require('url');
  
  // 创建web服务
  var server = http.createServer(function(request,response){
      // 获取请求地址
      var urlStr = request.url;
      // 解析参数
      // 参数1：请求地址；
      // 参数2：true时query解析参数为一个json对象，默认false
      var json = url.parse(urlStr,true).query;
  
      // 写HTTP 头部
      // HTTP 状态值: 200 : OK
      // 内容类型: text/plain
      response.writeHead(200,{
          'Content-Type': 'text/plain;charset=UTF-8'
      })
      // 发送响应数据,关闭响应对象
      response.end("name=" + json.name + ",age=" + json.age)
  
  });
  // 监听端口
  server.listen(8888);
  // 终端提示信息
  console.log("亲，web服务跑起来了.......")
  ```

+ 启动测试

  ![1572919499717](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572919499717.png)&nbsp;

  在浏览器访问 <http://localhost:8888/?name=admin&age=18> 

  ![1572919518083](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572919518083.png)

## 05、npm介绍&使用

### 5.1 npm介绍

+ **npm** 全称Node Package Manager，是nodejs的包管理器。我们可以把NPM理解为前端的Maven。通过 **npm **可以很方便地下载js库，构建与运行前端项目。

+ 现在的node.js已经集成了npm项目构建工具，在命令提示符输入 npm -v 可查看当前npm版本

  ![1572920177953](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572920177953.png)&nbsp;

### 5.2 初始化项目

+ **init** 命令是项目初始化命令。 

  新建一个空文件夹或者在上述的示例工程中，在命令提示符进入该文件夹  执行命令初始化

  ```cmd
  npm init
  ```

  ![1572921287010](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572921287010.png)

  ![1572921382641](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572921382641.png)

  + 按照提示输入相关信息，如果是用默认值则直接回车即可 
    + name: 项目名称
    + version: 项目版本号
    + description: 项目描述
    + keywords: {Array}关键词，便于用户搜索到我们的项目 

  > 说明: 最后会生成 package.json 文件，这个是包的配置文件，相当于maven的pom.xml之后也可以根据需要进行修改。

### 5.3 本地安装

+ **install** 命令用于安装某个js的库，npm会从中央仓库下载js的文件，输出命令如下:

  ```cmd
  npm install [js文件名称]
  ```

  ![1572922540516](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572922540516.png)

  + 出现警告信息，可以忽略，请放心，你已经成功执行了该命令。 

  + 在该目录下已经出现了一个node_modules文件夹 和package-lock.json。

  + node_modules文件夹用于存放下载的js库（相当于maven的本地仓库）。

  + package-lock.json是当 node_modules 或 package.json 发生变化时自动生成的文件。这个文件主要功能是确定当前安装包的依赖，以便后续重新安装的时候生成相同的依赖，而忽略项目开发过程中有些依赖已经发生的更新（可能存在切换了不同的镜像源后，同一个大版本号下可能出现兼容问题，package-lock可以保证即使换了源，下载的文件和原来的可以保持一致）。

  + 我们再打开package.json文件，发现刚才下载的jquery已经添加到依赖列表中了。

    ![1572922682388](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572922682388.png)

  + 关于版本号定义(了解)

    ![1572922682388](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572521616033.png)

### 5.4 全局安装

+ 本地安装: 会将js库安装在当前项目的node_modules目录。

+ 全局安装: 会将js库安装到你的全局目录下。(**以后的本地安装，就会来自全局目录**)

  如果你不知道你的全局目录在哪里，执行命令查看全局目录路径

  ```shell
  npm root -g  
  ```

  ![1572924022675](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572924022675.png)

  默认全局目录在: C:\Users\Administrator\AppData\Roaming\npm\node_modules

  ![1572924084771](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572924084771.png)

+ 比如全局安装jquery，输入以下命令

  语法: npm install [js文件名称] -g

  ```shell
  npm install jquery -g
  ```

  ![1572924395338](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572924395338.png)

  ![1572924439185](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572924439185.png)

### 5.5 批量下载

从网上下载某些代码，发现只有package.json，没有node_modules文件夹，这时需要通过命令重新下载这些js库. 进入目录（package.json所在的目录）输入命令

```shell
npm install
```

![1572924779290](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572924779290.png)

> 说明: npm会自动下载package.json中依赖的js库。

### 5.6 切换NPM镜像

有时我们使用npm下载资源会很慢，所以可以切换下载的镜像源（如：淘宝镜像）；或者安装一个cnpm(指定淘宝镜像)来加快下载速度。

+ 如果使用切换镜像源的方式，可以使用一个工具：nrm 

  ![](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1565186863237.png)

  + 安装nrm，这里 -g 代表全局安装 

    ```shell
    # 管理员身份 打开cmd执行如下命令 
    npm install nrm -g
    ```

    ![1572925438429](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572925438429.png)

    ![1572925492706](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572925492706.png)

  + 通过 nrm ls 命令查看npm的仓库列表,带*的就是当前选中的镜像仓库

    ![1572925629407](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572925629407.png)

  + 通过 nrm use taobao 来指定要使用的镜像源

    ![1572925716772](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572925716772.png)

+ 如果使用cnpm的方式，则先安装cnpm，输入如下命令

  ```shell
  # 如果不使用nrm 切换，可以在安装cnpm的时候指定镜像仓库 
  npm install -g cnpm --registry=https://registry.npm.taobao.org
  ```

  ![1572927176676](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572927176676.png)安装后，我们可以使用以下命令来查看cnpm的版本: cnpm -v

  ![1572927255951](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572927255951.png)

  使用cnpm安装js库

  ```shell
  cnpm install jquery[需要下载的js库]
  ```

  ![1572927330601](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572927330601.png)

### 5.7 运行工程【了解】

+ 如果我们想运行某个工程，则使用run命令 如果package.json中定义的脚本中有:

  + dev 是开发阶段测试运行 
  + build 是构建编译工程 
  + lint 是运行js代码检测

  运行时命令格式:

  ```shell
  npm run dev或者build或者lint
  ```

  安装live-server模块:

  ```shell
  npm install live-server -g
  ```

  ![1574791996281](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1574791996281.png) 

  修改package.json文件:

  ![1574792063855](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1574792063855.png) 

  运行项目: npm run dev

### 5.8 编译工程【了解】

+ 编译后的代码会放在dist文件夹中，进入命令提示符输入命令

  ```shell
  npm run build
  ```

  + 生成后会发现只有一个静态页面,js与css两个文件夹。
  + 这种工程我们称之为单页Web应用（single page web application，SPA），就是只有一张Web页面的应用，是加载单个HTML 页面并在用户与应用程序交互时动态更新该页面的Web应用程序。 
  + 这里其实是调用了webpack来实现打包的，关于webpack下面的章节将进行介绍。

## 06、webpack介绍&安装

### 6.1 webpack介绍

+ Webpack是一个前端资源加载/打包工具。它将根据模块的依赖关系进行静态分析，然后将这些模块按照指定的规则生成对应的静态资源。

+ 官网地址: <https://www.webpackjs.com/> 

  ![1572928203228](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572928203228.png)

  打包所有的资源:

  + 打包所有的图片(img)
  + 打包所有的样式(css)
  + 打包所有的脚本(js)

  > 说明: 从图中我们可以看出，Webpack 可以将多种静态资源 js、css等转换成一个静态文件，减少了页面的请求。

### 6.2 webpack安装

+ 全局安装

  ```shell
  npm install webpack -g
  npm install webpack-cli -g
  ```

  ![1572929291633](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572929291633.png)

  ![1572929365820](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572929365820.png)&nbsp;

  > 如果安装失败；则将全局目录下的webpack的相关文件夹删除再执行上述命令。

+ 安装后查看版本号

  ```shell
  webpack -v
  ```

  ![1572929404056](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572929404056.png)&nbsp;



## 07、webpack打包js

#### 操作步骤

+ 创建src文件夹，创建bar.js

  ```js
  // 定义模块
  exports.info = function(str){
      document.write(str);
  }
  ```

+ src下创建logic.js

  ```js
  // 定义模块
  exports.add = function(a, b){
      return a + b;
  }
  ```

+ src下创建main.js

  ```js
  // 引入模块
  var bar = require('./bar');
  var logic = require('./logic');
  
  // 调用函数
  bar.info('100 + 200 = '+ logic.add(100, 200));
  ```

+ 创建配置文件webpack.config.js，该文件与src处于同级目录

  ```js
  // 引入path模块(内置模块)
  var path = require("path");
  
  // exports 就是 module.exports，
  // 但是这里直接是赋值，所以不能直接使用exports，
  // 否则exports就不是module.exports了
  module.exports = {
      // 入口文件
      entry: "./src/main.js",
      // 输出路径与文件名
      output:{
          // __dirname 是node的一个全局变量，获得当前文件所在目录的完整目录名
          path: path.resolve(__dirname, "./dist"),
          filename: "bundle.js"
      }
  };
  ```

  以上代码的意思是: 读取当前目录下src文件夹中的main.js（入口文件）内容，把对应的js文件打包，打包后的文件放入当前目录的dist文件夹下，打包后的js文件名为bundle.js

  ![1572935627670](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572935627670.png)

+ 执行编译命令

  ```shell
  webpack
  ```

  执行后查看bundle.js,会发现里面包含了上面两个js文件的内容

  ![1572935719554](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572935719554.png)

+ 创建index.html,引用bundle.js

  ```html
  <!DOCTYPE html>
  <html>
  <head>
      <meta charset="UTF-8">
      <title>webpack</title>
      <script src="dist/bundle.js"></script>
  </head>
  <body>
  </body>
  </html>
  ```

  浏览器上访问index.html文件，会发现有内容输出:

  ![1572935895585](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572935895585.png)

## 08、webpack打包css

#### 操作步骤

+ 安装style-loader和css-loader

  + Webpack 本身只能处理 JavaScript 模块，如果要处理其他类型的文件，就需要使用 loader 进行转换。
  + Loader 可以理解为是模块和资源的转换器，它本身是一个函数，接受源文件作为参数，返回转换的结果。这样，我们就可以通过 require 来加载任何类型的模块或文件。首先我们需要安装相关Loader插件,css-loader是将 css 装载到 javascript；style-loader 是让 javascript 认识css

  ```shell
  npm install css-loader style-loader --save-dev
  ```

  ![1572936232894](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572936232894.png)

  + --save 的意思是将模块安装到项目目录下，并在package文件的**dependencies**节点写入依赖。运行npm install。
  + --save-dev 的意思是将模块安装到项目目录下，并在package文件的**devDependencies**节点写入依赖。

+ 修改webpack.config.js

  ```js
  // 引入path模块(内置模块)
  var path = require("path");
  
  // exports 就是 module.exports，
  // 但是这里直接是赋值，所以不能直接使用exports，
  // 否则exports就不是module.exports了
  module.exports = {
      // 入口文件
      entry: "./src/main.js",
      // 输出路径与文件名
      output:{
          // __dirname 是node的一个全局变量，获得当前文件所在目录的完整目录名
          path: path.resolve(__dirname, "./dist"),
          filename: "bundle.js"
      },
      // 模块
      module : {
          // 规则
          rules : [
              {
                  test: /\.css$/,
                  use: ["style-loader","css-loader"]
              }
          ]
      }
  };
  ```

+ 在src文件夹创建css文件夹,css文件夹下创建css1.css 

  ```css
  body{
      background-color: red;
  }
  ```

  ![1572936582973](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572936582973.png)

+ 修改main.js ，引入css1.css

  ```js
  // 引入样式
  require('./css/css1.css');
  ```

  ![1572936716789](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572936716789.png)

+ 重新运行webpack

  ![1572936775248](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572936775248.png)

+ 运行index.html看看背景是不是变成红色?

  ![1572936809874](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572936809874.png)



## 09、ES6的概述

![1572521734832](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572521734832.png)  

**ECMAScript的快速发展：**

![1572521734832](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572535384596.png) 

+ 编程语言JavaScript是ECMAScript的实现和扩展 。ECMAScript是由ECMA（一个类似W3C的标准组织）参与进行标准化的语法规范。ECMAScript定义了：

  + [语言语法] – 语法解析规则、关键字、语句、声明、运算符等。

  + [类型]– 布尔型、数字、字符串、对象等。

  + [原型和继承]

  + 内建对象和函数的[标准库] – [JSON]、[Math]、[数组方法]、[对象自省方法]等。

    

+ ECMAScript标准不定义HTML或CSS的相关功能，也不定义类似DOM（文档对象模型）的[Web API]，这些都在独立的标准中进行定义。ECMAScript涵盖了各种环境中JS的使用场景，无论是浏览器环境还是类似[node.js]的非浏览器环境。

  

+ ECMAScript标准的历史版本分别是1、2、3、5。

  + 那么为什么没有第4版？其实，在过去确实曾计划发布提出巨量新特性的第4版，但最终却因想法太过激进而惨遭废除（这一版标准中曾经有一个极其复杂的支持泛型和类型推断的内建静态类型系统）。

    ES4饱受争议，当标准委员会最终停止开发ES4时，其成员同意发布一个相对谦和的ES5版本，随后继续制定一些更具实质性的新特性。这一明确的协商协议最终命名为“Harmony”，因此，ES5规范中包含这样两句话ECMAScript是一门充满活力的语言，并在不断进化中。未来版本的规范中将持续进行重要的技术改进。

  + 2009年发布的改进版本ES5，引入了[Object.create()]、[Object.defineProperty()]、[getters]和[setters]、[严格模式]以及[JSON]对象。

  + ES6: 是JavaScript语言的下一代标准，2015年6月正式发布。它的目标，是使得JavaScript语言可以用来编写复杂的大型应用程序，成为企业级开发语言。

**小结**

> ECMAScript是前端js的语法规范；可以应用在各种js环境中。如：浏览器或者node.js环境。
>
> 它有很多版本：es1/2/3/5/6，很多新特性，可以在js环境中使用这些新特性。



## == ES6的语法 == 

### 10、let和const关键字

##### 10.1 测试模块

![1573116701996](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1573116701996.png)

![1573116748770](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1573116748770.png)

![1573118753146](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1573118753146.png)

**注意：如果要在IDEA中运行es6的语法，需要在IDEA中配置运行es6**

![1572593164249](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572593164249.png) 

##### 10.2 定义变量

以前: js定义变量只有一个关键字 var

现在: 可用var、let定义变量

> var: 所声明的变量，都是全局变量。
>
> let: 所声明的变量，全局变量或局部变量。

**核心代码**

```js
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>ES6测试</title>
    <script type="text/javascript">
        // 以前用var定义的变量：相当于全局变量
        for (var i = 0; i < 2; i++){
            console.log("内循环输出i: " + i);
        }
        // 正常输出
        console.log("外循环输出i：" + i);

        console.log("==============")

        // 现在用let定义的变量：相当于局部变量(作用于当前代码块)
        for (let j = 0; j < 2; j++){
            console.log("内循环输出j: " + j);
        }
        // 此处会报错
        console.log("外循环输出j: " + j);
    </script>
</head>
<body>
</body>
</html>
```

**运行效果**

![1573142319278](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1573142319278.png)

##### 10.3 定义常量

> const: 声明的变量是常量，变量值不能被修改。

**核心代码**

```js
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>ES6测试</title>
    <script type="text/javascript">
        // 定义常量
        const name1 = "小一";
        console.log(name1 + ",好年轻呀！");

        // 常量不能改值
        name1 = "小二";
        console.log(name1);
    </script>
</head>
<body>
</body>
</html>
```

![1573176197204](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1573176197204.png)

##### 10.4 小结

+ var: 全局变量
+ let : 全局变量或局部变量
+ const: 常量，值不能修改。

### 11、模板字符串

 【反引号】: `` : 定义模板字符串

![1572530348687](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1572530348687.png)   

用途一：**字符串替换**。将变量嵌入到模板字符串中进行替换。用${}来表示。

```js
// 作用一: 字符串替换
// 定义变量
let name = "itcast";
// 定义模板字符串
let str = `hello：${name}`;
// 控制台输出
console.log(str); // hello: itcast
```

用途二：**字符串拼接**。ES6反引号(``)直接搞定。

```js
// 字符串拼接
let str2 = "<div style='color: red'>" +
    "<span>黑马</span>" +
    "</div>";
document.write(str2);

// 作用二: 字符串拼接
// 字符串拼接: 定义模板字符串
let str3 = `<div style='color: blue'>
			   <span>传智</span>
		    </div>`;
document.write(str3);
```

### 12、函数默认参数&箭头函数

+ 函数默认参数

  作用: 在函数的参数后面加上一个默认值

  语法: var 函数名 = function(x='默认值',y='默认值'){}

  **核心代码**

  ```javascript
  // 函数默认参数
  var add = function (a=10,b=20) {
      return a + b;
  }
  console.log(add());
  ```

+ 箭头函数

  作用: 箭头函数简化函数的定义。

  三个特点:

  + 不需要function关键字来创建函数
  + 没有大括号时,可省略return关键字
  + 继承当前上下文的 this 关键字

  定义语法:

  + var 函数名 = (形参列表) => {函数体} 【标准】
  + var 函数名 = (形参列表) => 函数体
  + var 函数名 = () => {函数体}
  + var 函数名 = 一个形参列表 => {函数体}
  + var 函数名 = 一个形参列表 => 函数体

  **核心代码**

  ```javascript
   // 1. 箭头函数(第一种方式)
  // var 函数名 = (形参列表) => {函数体}
  var add2 = (a, b) => {
      return a + b;
  }
  console.log(add2(100,200));
  
  // 2. 箭头函数(第二种方式)
  // 如果函数体只有一行语句可以再次简化，并默认return
  var add3 = (a, b) => a + b;
  console.log(add3(150,250));
  
  // 3. 箭头函数(第三种方式)
  // 如果没有形参列表
  var add4 = () => {
      return 10;
  }
  console.log(add4());
  
  // 4. 箭头函数(第四种方式)
  // 如果只有一个形参列表
  var add5 = num => {
      return 10 + num;
  }
  console.log(add5(12));
  
  // 5. 箭头函数(第五种方式)
  // 如果只有一个形参列表 (再次简化)
  var add6 = num =>  10 + num;
  console.log(add6(13));
  ```

### 13、对象初始化简写

> ES5对于对象都是以键值对的形式书写，是有可能出现键值对重名的情况，所以ES6做了简化。 

**核心代码**

```javascript
// ES6之前的写法
var user = function(name, age)  {
    return {name : name , age : age};
};
console.log(user('小华华', 18));

// ES6 对象初始化写法
// 约定: 形参名 与 对象的key一致可以简写
var user2 = function (name, age){
    return {name, age};
}
console.log(user2('大华华', 20));

// ES6 配合箭头函数，对象初始化
// 不能直接用{}号，因为ES6会认为是函数体
var user3 = (name, age) => ({name, age});
console.log(user3('中华华', 19));
```

### 14、对象解构

+ 解构数组: 方便我们获取数组中的元素。

  ```java
  // 解构数组: 方便我们获取数组元素
  var arr = [10,20,30];
  // 数组中的元素 放到新的变量中
  let [x,y,z] = arr;
  console.log(x, y, z);
  ```

+ 解构对象: 方便我们获取对象中的属性。

  ```javascript
  // 解构对象: 方便我们获取对象属性
  var user = {name : "admin", age : 20};
  // 对象中的属性 放到新的变量中(变量名与属性名必须一致)
  var {name, age} = user;
  console.log(name + "==" + age);
  ```

### 15、传播操作符

作用: 把一个对象的属性传播到另外一个对象中。

语法: ... 数组名|对象名

**核心代码**

```js
// 定义数组
var arr = [10,20,30];
// 传播操作符: 把一个数组的元素传播到另一个数组
var arr1 = [... arr, 40];
console.log(arr1);

// 定义对象
var user = {name : "admin", age : 20};
// 传播操作符: 把一个对象的属性传播到另一个对象
var person = {... user, sex :'男' };
console.log(person);
```

### 16、操作数组

##### 16.1 数组迭代

 数组迭代的四种方式:

+ for 循环

+ for in

+ for of

+ forEach

  ```javascript
   // 定义数组
  var arr = [10,20,40,50];
  // 1. 数组迭代：第一种方式 i: 索引号
  for (let i = 0; i < arr.length; i++){
      console.log(arr[i]);
  }
  console.log("============")
  // 2. 数组迭代：第二种方式 i: 索引号
  for (let i in arr){
      console.log(arr[i]);
  }
  console.log("============")
  // 3. 数组迭代：第三种方式 i: 数组元素
  for (let i of arr){
      console.log(i);
  }
  console.log("============")
  // 4. 数组迭代：第四种方式 i: 数组元素
  arr.forEach( i => {
      console.log(i);
  });
  ```

##### 16.2 map方法

作用: 将一个数组转化成一个新的数组。

语法: var 新的数组变量名 = 数组变量名.map(function(item, index){});

+ item: 数组中的元素
+ index: 索引号

```js
 // 定义数组
var arr1 = ["90","100","110","120"];
console.log(arr1);

// 将arr1数组转化成新的数组
var newArr = arr1.map(item => parseInt(item));
console.log(newArr);
```

##### 16.3 reduce方法

作用: 对一个数组中的元素做计算,并返回计算的结果。

语法: 数组变量名.reduce(function(a,b){}, [初始值]);

+ 第一个参数: 函数
  + a: 上一次reduce计算的结果(如果[初始值]指定了，第一次a就是初始值)
  + b: 数组中要处理的下一个元素
+ 第二个参数: 初始值(可选)

```js
// 定义数组
let arr2 = [100,200,300]
// 计算数组元素的总和: 100 + 200 + 300
let a = arr2.reduce((a, b) => a + b);
// 计算数组元素的总和:  1 + 100 + 200 + 300
let b = arr2.reduce((a, b) => a + b, 1);

// 计算数组元素的乘积: 100 * 200 * 300
let c = arr2.reduce((a, b) => a * b);
// 计算数组元素的乘积: 0 * 100 * 200 * 300
let d = arr2.reduce((a, b) => a * b, 0);
console.log(a,b,c,d);
```

### 17、promise函数

+ 所谓Promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。

+ 语法格式

  ```js
  const promise = new Promise(function(resolve, reject) {  // ... 执行异步操作
      if (/* 异步操作成功 */){    
      	resolve(value);// 调用resolve，代表Promise将返回成功的结果 
      } else {    
     		reject(error);// 调用reject，代表Promise会返回失败结果
      } 
   });
  ```

+ promise是一个对象，保存着预期事件执行的结果；可以应用在异步操作时候，指定异步操作的成功与失败的结果。

  ```js
  // 定义promise对象,封装处理成功 或 处理失败的业务
  const promise = new Promise(function (resolve, reject) {
      let num = Math.random();
      if(num < 0.5){
          // resolve() 会触发 then()函数的调用
          resolve("操作成功！num=" + num);
      } else {
          // reject() 会触发 catch()函数的调用
          reject("操作失败！num=" + num);
      }
  });
  ```

+ promise绑定方法

  ```js
  // then: 处理成功
  promise.then(function (msg) {
      console.log("then:" + msg);
  }).catch(function (msg) { // catch: 处理失败
      console.log("catch:" + msg);
  });
  ```

### 18、导入&导出模块

  ES6 在语言标准的层面上，实现了模块功能。ES6 模块不是对象，而是通过export命令显式指定输出的代码，再通过import命令输入。遗憾的是export和import命令不能在浏览器直接使用，不过可以通过安装babel编译器把es6转换为es5在Nodejs中可以运行。

##### 18.1 安装babel

+ babel是JavaScript语法的编译器。 

  + babel转换配置,项目根目录添加 .babelrc 文件

    ```js
    {
      "presets" : ["es2015"]
    }
    ```

    ![1573208830634](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1573208830634.png)

  + 安装es6转换模块

    ```shell
    npm install babel-preset-es2015 --save-dev
    ```

    ![1573208933548](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1573208933548.png)

  + 全局安装命令行工具

    ```shell
    cnpm install babel-cli -g
    ```

    ![1573209050955](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1573209050955.png)

  + 命令使用语法

    ```shell
    babel-node js文件名
    ```

##### 18.2 命名导出

+ 导出文件(export.js)

  ```js
  // 导出一个变量
  export let name = "小华华";
  
  let age = 20;
  let sex = "男";
  let info = function (str) {
      console.log(str);
  };
  // 批量导出变量
  export {age, sex, info};
  ```

+ 导入文件(import.js)

  ```js
  import {name, age, sex, info} from './export';
  console.log(name, age, sex);
  info("我叫小花花...");
  ```

  ![1573210967639](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1573210967639.png)&nbsp;

##### 18.3 默认导出

- 导出文件(export2.js)

  ```js
  // 默认导出一个函数
  // export default function (str) {
  //     console.log(str);
  // }
  
  // 默认批量导出函数
  export default {
      add : function(a, b){
          return a + b;
      },
      min : function (a,b) {
          return a - b;
      }
  }
  ```

  > 注意：只能定义一个默认导出函数

- 导入文件(import2.js)

  ```js
  import itcast from './export2';
  
  console.log("add: " + itcast.add(100,200));
  console.log("min: " + itcast.min(400,100));
  ```

  ![1573211125849](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/nodejs&ES6/1573211125849.png)

## 19、课程总结

+ nodejs安装与快速入门
+ ES6新的语法