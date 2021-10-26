

## 01、学习目标

- 了解vue-router使用
- 了解webpack使用
- 了解vue-cli搭建项目
- 了解项目基本信息
- 独立搭建后台管理系统
- 清楚项目基本结构



























## 02、乐优资料说明

![1575011000165](图片/1575011000165.png) 









## 03、了解电商：项目分类

主要从需求方、盈利模式、技术侧重点这三个方面来看它们的不同

### 1）传统项目

各种企业里面用的管理系统（ERP、HR、OA、CRM、物流管理系统、SAAS。）

- 需求方：公司、企业内部
- 盈利模式：项目本身卖钱
- 技术侧重点：业务功能

### 2）互联网项目

门户网站、电商网站：baidu.com、qq.com、taobao.com、jd.com  ...... 

- 需求方：广大用户群体
- 盈利模式：虚拟币、增值服务、广告收益......
- 技术侧重点：网站性能、业务功能



而我们今天要聊的就是互联网项目中的重要角色：电商。









## 04、了解电商：电商行业特点

### 1）谈谈双十一

双十一是电商行业的一个典型代表，从里面我们能看出电商的普遍特点：

==**2014年**==

![1574835989435](图片/1574835989435.png) 

2016双11开场30分钟，创造**每秒交易峰值17.5万笔**，**每秒**支付峰值**12万笔**的新纪录。菜鸟单日物流订单量超过**4.67亿**，创历史新高。



==**2019年**==

![1578046496538](图片/1578046496538.png) 



![1578046626009](图片/1578046626009.png) 

![1578046695438](图片/1578046695438.png) 

![1578046809613](图片/1578046809613.png) 



数据来自百度百科： [https://baike.baidu.com/item/2019%E5%8F%8C%E5%8D%81%E4%B8%80%E8%B4%AD%E7%89%A9%E7%8B%82%E6%AC%A2%E8%8A%82/23142156?fr=aladdin](https://baike.baidu.com/item/2019双十一购物狂欢节/23142156?fr=aladdin)





### 2）技术特点

从上面的数据我们不仅要看到钱，更要看到背后的技术实力。正是得益于电商行业的高强度并发压力，促使了BAT等巨头们的技术进步。电商行业有些什么特点呢？

- 技术范围广
- 技术新
- 要求双高：
  - 高并发（分布式、静态化技术、CDN服务、缓存技术、异步并发、池化、队列）
  - 高可用（集群、负载均衡、限流、降级、熔断）
- 数据量大
- 业务复杂





### 3）常见电商模式

电商行业的一些常见模式：

- B2C：商家对个人，如：亚马逊、当当等
- C2C平台：个人对个人，如：闲鱼、拍拍网、ebay
- B2B平台【B2B2B】：商家对商家，如：阿里巴巴、八方资源网等
- O2O：线上和线下结合，如：饿了么、电影票、团购等
- P2P：在线金融，贷款，如：网贷之家、人人聚财等。
- B2C平台【B2B2C】：天猫、京东、一号店等





















## 05、乐优商城介绍：系统架构



### 1）项目介绍

- 乐优商城是一个全品类的电商购物网站（B2C）。
- 用户可以在线购买商品、加入购物车、下单、秒杀商品
- 可以*评论已购买商品*
- 管理员可以在后台管理商品的上下架、*促销活动*
- 管理员可以*监控商品销售状况*
- 客服可以在后台处理*退款操作*
- 希望未来3到5年可以支持千万用户的使用







### 2）系统架构图

![1574834708916](图片/1574834708916.png) 



### 3）系统架构解读

> #### 前端页面

整个乐优商城从用户角度来看，可以分为两部分：后台管理、前台门户。

- **==后台管理==**：

  - 后台系统主要包含以下功能：
    - 商品管理，包括商品分类、品牌、商品规格等信息的管理
    - 销售管理，包括订单统计、订单退款处理、促销活动生成等
    - 用户管理，包括用户控制、冻结、解锁等
    - 权限管理，整个网站的权限控制，采用JWT鉴权方案，对用户及API进行权限控制
    - 统计，各种数据的统计分析展示
    - ...
  - 后台系统会采用前后端分离开发，而且整个后台管理系统会使用Vue.js框架搭建出单页应用（SPA）。
  - 预览图：

  ![1528098418861](图片/1528098418861.png)

- **==前台门户==**

  - 前台门户面向的是客户，包含与客户交互的一切功能。例如：
    - 搜索商品
    - 加入购物车
    - 下单
    - 评价商品等等
  - 前台系统我们会使用Nuxt(服务端渲染)结合Vue完成页面开发。出于SEO优化的考虑，我们将不采用单页应用。

  ![1525704277126](图片/1525704277126.png)



无论是前台门户、还是后台管理页面，都是前端页面，我们的系统采用前后端分离方式，因此前端会独立部署，不会在后端服务出现静态资源。



> #### 后端微服务

无论是前台还是后台系统，都共享相同的微服务集群，包括：

- 商品微服务：商品及商品分类、品牌、库存等的服务

- 搜索微服务：实现搜索功能

- 订单微服务：实现订单相关

- 购物车微服务：实现购物车相关功能

- 用户服务：用户的登录注册、用户信息管理等功能

- 短信服务：完成各种短信的发送任务

- 支付服务：对接各大支付平台

- 授权服务：完成对用户的授权、鉴权等功能

- Eureka注册中心

- Spring Cloud Gateway网关服务

- Spring Cloud Config配置中心

  









## 06、乐优商城介绍：微服务代码结构图

![1575017871560](图片/1575017871560.png) 

 



> **VO（View Object）**：视图对象，用于展示层，它的作用是把某个指定页面（或组件）的所有数据封装起来。
>
> ==**DTO（Data Transfer Object）**==：数据传输对象，这个概念来源于J2EE的设计模式，原来的目的是为了EJB的分布式应用提供粗粒度的数据实体，以减少分布式调用的次数，从而提高分布式调用的性能和降低网络负载，但在这里，我泛指用于展示层与服务层之间的数据传输对象。
>
> **DO（Domain Object）**：领域对象，就是从现实世界中抽象出来的有形或无形的业务实体。
>
> **PO（Persistent Object）**：持久化对象，它跟持久层（通常是关系型数据库）的数据结构形成一一对应的映射关系，如果持久层是关系型数据库，那么，数据表中的每个字段（或若干个）就对应PO的一个（或若干个）属性。
>
> **BO（Business Object）**：业务对象，一个BO可能包含一个或多个PO
>
> **DAO（Data Access Object）** ：数据访问对象，主要用来封装对DB的访问（CRUD操作）
>
> **POJO（Plain Ordinary Java Object ）** ： 无规则简单java对象，一个中间对象，可以转化为PO、DTO、VO









## 07、乐优商城介绍：开发文档说明

采用前后端绝对分离的方式，前端独立在node中运行，后台采用java。那么开发中如何保证前后端开发的东西是一致的？ 

没错就是我们的api文档了，具体参考资料中开发文档一项。



### 1）状态码约定

| HTTP状态码（HTTP   Status Code）                             |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1xx（临时响应）                                              |                                                              |
| 表示临时响应并需要请求者继续执行操作的状态代码。             |                                                              |
| 代码                                                         | 说明                                                         |
| 100                                                          | （继续） 请求者应当继续提出请求。 服务器返回此代码表示已收到请求的第一部分，正在等待其余部分。 |
| 101                                                          | （切换协议） 请求者已要求服务器切换协议，服务器已确认并准备切换。 |
| 2xx                                                          | （成功）                                                     |
| 表示成功处理了请求的状态代码。                               |                                                              |
| 代码 说明                                                    |                                                              |
| 200                                                          | （成功） 服务器已成功处理了请求。 通常，这表示服务器提供了请求的网页。 |
| 201                                                          | （已创建） 请求成功并且服务器创建了新的资源。                |
| 202                                                          | （已接受） 服务器已接受请求，但尚未处理。                    |
| 203                                                          | （非授权信息） 服务器已成功处理了请求，但返回的信息可能来自另一来源。 |
| 204                                                          | （无内容） 服务器成功处理了请求，但没有返回任何内容。        |
| 205                                                          | （重置内容） 服务器成功处理了请求，但没有返回任何内容。      |
| 206                                                          | （部分内容） 服务器成功处理了部分 GET 请求。                 |
| 3xx   （重定向）                                             |                                                              |
| 表示要完成请求，需要进一步操作。   通常，这些状态代码用来重定向。 |                                                              |
| 代码                                                         | 说明                                                         |
| 300                                                          | （多种选择） 针对请求，服务器可执行多种操作。 服务器可根据请求者 (user agent) 选择一项操作，或提供操作列表供请求者选择。 |
| 301                                                          | （永久移动） 请求的网页已永久移动到新位置。 服务器返回此响应（对 GET 或 HEAD 请求的响应）时，会自动将请求者转到新位置。 |
| 302                                                          | （临时移动） 服务器目前从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。 |
| 303                                                          | （查看其他位置） 请求者应当对不同的位置使用单独的 GET 请求来检索响应时，服务器返回此代码。 |
| 304                                                          | （未修改） 自从上次请求后，请求的网页未修改过。 服务器返回此响应时，不会返回网页内容。 |
| 305                                                          | （使用代理） 请求者只能使用代理访问请求的网页。 如果服务器返回此响应，还表示请求者应使用代理。 |
| 307                                                          | （临时重定向） 服务器目前从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。 |
| 4xx（请求错误）                                              |                                                              |
| 这些状态代码表示请求可能出错，妨碍了服务器的处理。           |                                                              |
| 代码                                                         | 说明                                                         |
| 400                                                          | （错误请求） 服务器不理解请求的语法。                        |
| 401                                                          | （未授权） 请求要求身份验证。 对于需要登录的网页，服务器可能返回此响应。 |
| 403                                                          | （禁止） 服务器拒绝请求。                                    |
| 404                                                          | （未找到） 服务器找不到请求的网页。                          |
| 405                                                          | （方法禁用） 禁用请求中指定的方法。                          |
| 406                                                          | （不接受） 无法使用请求的内容特性响应请求的网页。            |
| 407                                                          | （需要代理授权） 此状态代码与 401（未授权）类似，但指定请求者应当授权使用代理。 |
| 408                                                          | （请求超时） 服务器等候请求时发生超时。                      |
| 409                                                          | （冲突） 服务器在完成请求时发生冲突。 服务器必须在响应中包含有关冲突的信息。 |
| 410                                                          | （已删除） 如果请求的资源已永久删除，服务器就会返回此响应。  |
| 411                                                          | （需要有效长度） 服务器不接受不含有效内容长度标头字段的请求。 |
| 412                                                          | （未满足前提条件） 服务器未满足请求者在请求中设置的其中一个前提条件。 |
| 413                                                          | （请求实体过大） 服务器无法处理请求，因为请求实体过大，超出服务器的处理能力。 |
| 414                                                          | （请求的 URI 过长） 请求的 URI（通常为网址）过长，服务器无法处理。 |
| 415                                                          | （不支持的媒体类型） 请求的格式不受请求页面的支持。          |
| 416                                                          | （请求范围不符合要求） 如果页面无法提供请求的范围，则服务器会返回此状态代码。 |
| 417                                                          | （未满足期望值） 服务器未满足"期望"请求标头字段的要求。      |
| 5xx（服务器错误）                                            |                                                              |
| 这些状态代码表示服务器在尝试处理请求时发生内部错误。   这些错误可能是服务器本身的错误，而不是请求出错。 |                                                              |
| 代码                                                         | 说明                                                         |
| 500                                                          | （服务器内部错误） 服务器遇到错误，无法完成请求。            |
| 501                                                          | （尚未实施） 服务器不具备完成请求的功能。 例如，服务器无法识别请求方法时可能会返回此代码。 |
| 502                                                          | （错误网关） 服务器作为网关或代理，从上游服务器收到无效响应。 |
| 503                                                          | （服务不可用） 服务器目前无法使用（由于超载或停机维护）。 通常，这只是暂时状态。 |
| 504                                                          | （网关超时） 服务器作为网关或代理，但是没有及时从上游服务器收到请求。 |
| 505                                                          | （HTTP 版本不受支持） 服务器不支持请求中所用的 HTTP 协议版本。 |



### 2）乐优服务名称与端口

| 模块           | 服务ID         | 端口  | 路由规则            | 域名                                   |
| -------------- | -------------- | ----- | ------------------- | -------------------------------------- |
| 注册中心       | ly-registry    | 10086 |                     |                                        |
| 网关           | ly-gateway     | 10010 | api前缀：/api       | api.leyou.com                          |
| 商品微服务     | item-service   | 8081  | /item/**            |                                        |
| 图片上传服务   | upload-service | 8082  | /upload/**          |                                        |
| 搜索服务       | search-service | 8083  | /search/**          |                                        |
| 页面静态化服务 | page-service   | 8084  |                     |                                        |
| 用户中心服务   | user-service   | 8085  | /user/**            |                                        |
| 短信服务       | sms-service    | 8086  |                     |                                        |
| 授权服务       | auth-service   | 8087  | /auth/**            |                                        |
| 购物车服务     | cart-service   | 8088  | /cart/**            |                                        |
| 订单服务       | order-service  | 8090  | /order/** 、/pay/** |                                        |
|                |                |       |                     |                                        |
| 后台管理web    | nginx反向代理  | 9001  |                     | manage.leyou.com                       |
| 前端页面       | nginx反向代理  | 9002  |                     | [www.leyou.com](http://www.leyou.com/) |
| 图片           | nginx反向代理  |       |                     | image.leyou.com                        |
| 网关           | ningx反向代理  | 10010 |                     | api.leyou.com                          |



### 3）乐优商城SwaggerAPI

- api文档

​     ![1574837412731](图片/1574837412731.png) 

- 安装live-server

     ```
npm install live-server -g
     ```

- 运行api项目

```
进入api文档的项目路径下，执行如下命令：
live-server --port=9999     #端口随意
```



### 4）pdf格式的文档

![1574837618705](图片/1574837618705.png) 







## 08、乐优商城介绍：后台管理系统技术选型

### 1）介绍

我们的后台管理系统采用前后端分离开发，而且整个后台管理系统会使用Vue.js框架搭建出单页应用（SPA）。

前端技术：

- 基础的HTML、CSS、JavaScript（基于ES6标准）
- JQuery
- Vue.js 2.0
- 基于Vue的UI框架：Vuetify
- 前端构建工具：WebPack，项目编译、打包工具
- 前端安装包工具：NPM
- Vue脚手架：Vue-cli
- Vue路由：vue-router
- ajax框架：axios
- 基于Vue的富文本框架：quill-editor



### 2）什么是SPA

SPA，并不是去洗澡按摩，而是Single Page Application，即单页应用。整个后台管理系统只会出现一个HTML页面，剩余一切页面的内容都是通过Vue组件来实现。

我们的后台管理系统就是一个基于Vue的SPA的模式，其中的UI交互式通过一个名为Vuetify的框架来完成的。



### 3）Vuetify框架

Vuetify是一个基于Vue的UI框架，可以利用预定义的页面组件快速构建页面。有点类似BootStrap框架。

#### 3.1为什么要学习UI框架

Vue负责的是虽然会帮我们进行视图的渲染，但是样式是有我们自己来完成。这显然不是我们的强项，因此后端开发人员一般都喜欢使用一些现成的UI组件，拿来即用，常见的例如：

- BootStrap
- LayUI
- EasyUI
- ZUI

然而这些UI组件的基因天生与Vue不合，因为他们更多的是利用DOM操作，借助于jQuery实现，而不是MVVM的思想。

而目前与Vue吻合的UI框架也非常的多，国内比较知名的如：

- element-ui：饿了么出品
- i-view：某公司出品

然而我们都不用，我们今天推荐的是一款国外的框架：Vuetify

官方网站：https://vuetifyjs.com/zh-Hans/

![1574837969525](图片/1574837969525.png) 



#### 3.2为什么是Vuetify

有中国的为什么还要用外国的？原因如下：

- Vuetify几乎不需要任何CSS代码，而element-ui许多布局样式需要我们来编写
- Vuetify从底层构建起来的语义化组件。简单易学，容易记住。
- Vuetify基于Material Design（谷歌推出的多平台设计规范），更加美观，动画效果酷炫，且风格统一

这是官网的说明：

![1525959769978](图片/1525959769978.png) 

缺陷：

- 目前官网虽然有中文文档，但因为翻译问题，几乎不太能看。



#### 3.3怎么用？

基于官方网站的文档进行学习：

![1525960312939](图片/1525960312939.png)



我们重点关注`UI components`即可，里面有大量的UI组件，我们要用的时候再查看，不用现在学习，先看下有什么：

 ![1525961862771](图片/1525961862771.png)



 ![1525961875288](图片/1525961875288.png)

以后用到什么组件，就来查询即可。













## 09、乐优前端：创建vue工程



### 1）安装Vue.js插件

![1574838417376](图片/1574838417376.png) 



### 2）创建Vue工程

![1574840062926](图片/1574840062926.png) 

这个创建需要网络，保证网络顺畅即可。

然后下一步下一步即可









## 10、乐优前端：解读vue工程



**==解读顺序：==**

**index.html**    到    **src/main.js**   到   **App.vue**   到   **\<router-view/>**    到   **src/router/index.js**



**==代码结构如下图：==**

![1574840948208](图片/1574840948208.png) 







## 11、乐优前端：后台管理系统使用说明

我们的后台管理系统就是基于Vue，并使用Vuetify的UI组件来编写的。

### 1）导入已有资源

后台项目相对复杂，为了有利于教学，我们不再从0搭建项目，而是直接使用课前资料中给大家准备好的源码：

![1574841315399](图片/1574841315399.png) 

我们解压缩，放到工作目录中：

![1574841622287](图片/1574841622287.png) 

然后在IDE中导入新的工程：

![1574841726609](图片/1574841726609.png) 

选中我们的工程：

![1574841841060](图片/1574841841060.png) 

这正是一个用vue-cli构建的单页应用： 

![1574841943232](图片/1574841943232.png) 





### 2）运行项目

在控制台输入命令

```
npm run serve
```

![1574842213348](图片/1574842213348.png) 

发现默认的端口是9001。访问：http://localhost:9001

会自动进行跳转：

![1525958950616](图片/1525958950616.png)



### 3）项目结构

开始编码前，我们先了解下项目的结构：

#### 目录结构

首先是目录结构图：

![1574843089509](图片/1574843089509.png) 

可以看到，整个项目除了一个`index.html`外没有任何的静态页面。页面的切换都是靠一个个的vue组件的切换来完成的。这里的Vue组件称为单文件组件，就是后缀名为`.vue`的文件。



#### 单文件组件

`.vue`文件是vue组件的特殊形式，在以前我们定义一个Vue组件是这样来写的：

```js
const com = {
    template:`
		<div style="background-ground-color:red">
			<h1>hello ...</h1>
		</div>
	`,
    data(){
        return {
            
        }
    },
    methods:{
        
    }
}
Vue.component("com", com);
```

这种定义方式虽然可以实现，但是html、css、js代码混合在一起，而且在JS中编写html和css显然不够优雅。

而`.vue`文件是把三者做了分离：

![1574843396139](图片/1574843396139.png) 

#### 页面菜单项

页面的左侧是我们的导航菜单：

![1574843469816](图片/1574843469816.png) 

这个菜单的文字信息，在项目的src目录下，有一个menu.js文件，是页面的左侧菜单目录：

![1574843557365](图片/1574843557365.png) 

内容：

```js
const menus = [
  {
    action: "home",
    title: "首页",
    path:"/index",
    items: [{ title: "统计", path: "/dashboard" }]
  },
  {
    action: "apps",
    title: "商品管理",
    path:"/item",
    items: [
      { title: "分类管理", path: "/category" },
      { title: "品牌管理", path: "/brand" },
      { title: "商品列表", path: "/list" },
      { title: "规格参数", path: "/specification" }
    ]
  },
  {
    action: "people",
    title: "会员管理",
    path:"/user",
    items: [
      { title: "会员统计", path: "/statistics" },
      { title: "会员管理", path: "/list" }
    ]
  },
  {
    action: "attach_money",
    title: "销售管理",
    path:"/trade",
    items: [
      { title: "交易统计", path: "/statistics" },
      { title: "订单管理", path: "/order" },
      { title: "物流管理", path: "/logistics" },
      { title: "促销管理", path: "/promotion" }
    ]
  },
  {
    action: "settings",
    title: "权限管理",
    path:"/auth",
    items: [
      { title: "权限管理", path: "/list" },
      { title: "角色管理", path: "/role" },
      { title: "人员管理", path: "/member" },
      { title: "服务管理", path: "/services" }
    ]
  }
]

export default menus;
```



#### 菜单的组件

每一个菜单都对应一个组件，这些组件在项目的`src/views/`目录下：

![1574843669297](图片/1574843669297.png) 

那么，菜单是如何与页面组件对应的呢？src目录下有一个router.js，里面保存了页面路径与页面组件的对应关系：

![1574843898768](图片/1574843898768.png) 

核心部分代码：

![1574843969602](图片/1574843969602.png) 

当我们点击对应按钮，对应的组件就会展示在页面。









## 12、乐优微服务：创建后台项目父工程

先准备后台微服务集群的基本架构

### 1）技术选型

后端技术：

- 基础的SpringMVC、Spring 5.0和MyBatis3
- Spring Boot 2.1.4版本
- Spring Cloud 稳定版 Greenwich.SR1
- Redis-4.0
- RabbitMQ-3.4
- Elasticsearch-6.6.2
- nginx-1.10.2
- Thymeleaf
- JWT



### 2）开发环境

为了保证开发环境的统一，希望每个人都按照我的环境来配置：

- IDE：我们使用Idea 2019.1.3 版本
- JDK：统一使用JDK1.8.0_211
- 项目构建：maven3.3.x以上版本即可

idea大家可以在我的课前资料中找到。另外，使用帮助大家可以参考课前资料的《idea使用指南.md》





### 3）创建父工程

创建统一的父工程：leyou，用来管理依赖及其版本，注意是创建project，而不是moudle

![1574844626379](图片/1574844626379.png) 

填写工程信息：

 ![1574844686644](图片/1574844686644.png) 

保存的位置信息：

![1574844750124](图片/1574844750124.png) 



然后将pom文件修改成我这个样子：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.leyou</groupId>
    <artifactId>leyou</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>pom</packaging>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.4.RELEASE</version>
        <relativePath/>
    </parent>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
        <spring-cloud.version>Greenwich.SR1</spring-cloud.version>
        <mapper.starter.version>2.1.5</mapper.starter.version>
        <mysql.version>5.1.47</mysql.version>
        <pageHelper.starter.version>1.2.10</pageHelper.starter.version>
    </properties>

    <dependencyManagement>
        <dependencies>
            <!-- springCloud -->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!-- 通用Mapper启动器 -->
            <dependency>
                <groupId>tk.mybatis</groupId>
                <artifactId>mapper-spring-boot-starter</artifactId>
                <version>${mapper.starter.version}</version>
            </dependency>
            <!-- 分页助手启动器 -->
            <dependency>
                <groupId>com.github.pagehelper</groupId>
                <artifactId>pagehelper-spring-boot-starter</artifactId>
                <version>${pageHelper.starter.version}</version>
            </dependency>
            <!-- mysql驱动 -->
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>${mysql.version}</version>
            </dependency>
        </dependencies>
    </dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.4</version>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
    </dependencies>

</project>
```

可以发现，我们在父工程中引入了SpringCloud等很多以后需要用到的依赖，以后创建的子工程就不需要自己引入了。

如果是一个需要运行和启动的子工程，需要加上插件：

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```



















## 13、乐优微服务：搭建注册中心

### 1）创建工程

这个大家应该比较熟悉了。

我们的注册中心，起名为：ly-registry，直接创建maven项目，自然会继承父类的依赖：

选择新建module：

![1551239817387](图片/1551239817387.png)



选择maven安装，但是不要选择骨架：

![1551239800177](图片/1551239800177.png)

然后填写项目坐标，我们的项目名称为ly-registry:

![1551239836496](图片/1551239836496.png)

选择安装目录，因为是聚合项目，目录应该是在父工程leyou的下面：

![1551239848532](图片/1551239848532.png)



### 2）添加依赖

添加EurekaServer的依赖：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>leyou</artifactId>
        <groupId>com.leyou</groupId>
        <version>1.0.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>ly-registry</artifactId>

    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

### 3）编写启动类

创建一个包：com.leyou，然后新建一个启动类：

```java
/**
 * @author 黑马程序员
 */
@SpringBootApplication
@EnableEurekaServer
public class LyRegistry {
    public static void main(String[] args) {
        SpringApplication.run(LyRegistry.class, args);
    }
}
```

### 4）配置文件

```yaml
server:
  port: 10086
spring:
  application:
    name: ly-registry
eureka:
  client:
    fetch-registry: false
    register-with-eureka: false
    service-url:
      defaultZone: http://127.0.0.1:10086/eureka
```

### 5）项目的结构

目前，整个项目的结构如图：

![1551239918685](图片/1551239918685.png) 













## 14、乐优微服务：搭建网关

### 1）创建工程

与上面类似，选择maven方式创建Module，然后填写项目名称，我们命名为：ly-gateway

![1551239931675](图片/1551239931675.png)

填写保存的目录：

![1551239938112](图片/1551239938112.png)

### 2）添加依赖

这里我们需要添加Gateway和EurekaClient的依赖：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>leyou</artifactId>
        <groupId>com.leyou</groupId>
        <version>1.0.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>ly-gateway</artifactId>

    <dependencies>
        <!--网关-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-gateway</artifactId>
        </dependency>
        <!--注册中心-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <!--熔断器-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

### 3）编写启动类

创建一个包：com.leyou.gateway，然后新建一个启动类：

```java
/**
 * 网关启动类
 */
@SpringCloudApplication
public class LyGateway {
    public static void main(String[] args) {
        SpringApplication.run(LyGateway.class, args);
    }
}
```



### 4）配置文件

```yaml
server:
  port: 10010
spring:
  application:
    name: ly-gateway
eureka:
  client:
    service-url:
      defaultZone: http://localhost:10086/eureka
    registry-fetch-interval-seconds: 5
    instance-info-replication-interval-seconds: 5
    initial-instance-info-replication-interval-seconds: 5
hystrix:
  command:
    default:
      execution:
        isolation:
          thread:
            timeoutInMilliseconds: 6000 # 熔断超时时长：6000ms
ribbon:
  ConnectTimeout: 1000 # ribbon链接超时时长
  ReadTimeout: 2000 # ribbon读取超时时长
  MaxAutoRetries: 0  # 当前服务重试次数
  MaxAutoRetriesNextServer: 1 # 切换服务重试次数
  OkToRetryOnAllOperations: false # 是否对所有的请求方式都重试，只对get请求重试

```

### 5）项目结构

目前，leyou下有两个子模块：

- ly-registry：服务的注册中心（EurekaServer）
- ly-gateway：服务网关（gateway）

目前，服务的结构如图所示：

 	![1551240009768](图片/1551240009768.png)



截止到这里，我们已经把基础服务搭建完毕，为了便于开发，统一配置中心（ConfigServer）我们留待以后添加。











## 15、乐优微服务：搭建商品模块

既然是一个全品类的电商购物平台，那么核心自然就是商品。因此我们要搭建的第一个服务，就是商品微服务。其中会包含对于商品相关的一系列内容的管理，包括：

- 商品分类管理
- 品牌管理
- 商品规格参数管理
- 商品管理
- 库存管理

我们先完成项目的搭建：

### 1）微服务的结构

因为与商品的品类相关，我们的工程命名为`ly-item`.

需要注意的是，我们的ly-item是一个微服务，那么将来肯定会有其它系统需要来调用服务中提供的接口，因此肯定也会使用到接口中关联的实体类。

因此这里我们需要使用聚合工程，将要提供的接口及相关实体类放到独立子工程中，以后别人引用的时候，只需要知道坐标即可。

我们会在ly-item中创建两个子工程：

- ly-item-dto：主要是相关实体类
- ly-item-service：所有业务逻辑及内部使用接口

![1575017871560](图片/1575017871560.png) 



> **VO（View Object）**：视图对象，用于展示层，它的作用是把某个指定页面（或组件）的所有数据封装起来。
>
> ==**DTO（Data Transfer Object）**==：数据传输对象，这个概念来源于J2EE的设计模式，原来的目的是为了EJB的分布式应用提供粗粒度的数据实体，以减少分布式调用的次数，从而提高分布式调用的性能和降低网络负载，但在这里，我泛指用于展示层与服务层之间的数据传输对象。
>
> **DO（Domain Object）**：领域对象，就是从现实世界中抽象出来的有形或无形的业务实体。
>
> **PO（Persistent Object）**：持久化对象，它跟持久层（通常是关系型数据库）的数据结构形成一一对应的映射关系，如果持久层是关系型数据库，那么，数据表中的每个字段（或若干个）就对应PO的一个（或若干个）属性。
>
> **BO（Business Object）**：业务对象，一个BO可能包含一个或多个PO
>
> **DAO（Data Access Object）** ：数据访问对象，主要用来封装对DB的访问（CRUD操作）
>
> **POJO（Plain Ordinary Java Object ）** ： 无规则简单java对象，一个中间对象，可以转化为PO、DTO、VO











### 2）创建父工程ly-item

依然是使用maven构建：

![1551240063940](图片/1551240063940.png)

保存的位置：

![1551240086739](图片/1551240086739.png)

不需要任何依赖，我们可以把项目打包方式设置为pom

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>leyou</artifactId>
        <groupId>com.leyou</groupId>
        <version>1.0.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>
    <artifactId>ly-item</artifactId>
    <!-- 打包方式为pom -->
    <packaging>pom</packaging>
    
</project>
```



### 3）创建ly-item-dto

在ly-item工程上点击右键，选择new > module:

![1551240166926](图片/1551240166926.png)

依然是使用maven构建，注意父工程是ly-item：

**注意**：接下来填写的目录结构需要自己手动完成，保存到`ly-item`下的`ly-item-dto`目录中：

创建完成后，此时的项目结构：

![1575018619223](图片/1575018619223.png) 



### 4）创建ly-item-service

与`ly-item-interface`类似，我们选择在`ly-item`上右键，新建module，然后填写项目信息：

![1551240269328](图片/1551240269328.png)

填写存储位置，是在`/ly-item/ly-item-service`目录

![1551240283037](图片/1551240283037.png)

点击Finish完成。

### 5）整个微服务结构

如图所示：

![1544362113639](图片/1544362113639.png) 



我们打开ly-item的pom查看，会发现ly-item-dto和ly-item-service都已经称为module了：

![1575018952126](图片/1575018952126.png) 



### 6）添加依赖

接下来我们给`ly-item-service`中添加依赖：

思考一下我们需要什么？

- Eureka客户端
- web启动器
- 通用mapper启动器
- 分页助手启动器
- 连接池，我们用默认的Hykira，引入jdbc启动器
- mysql驱动
- 千万不能忘了，我们自己也需要`ly-item-dto`中的实体类

这些依赖，我们在顶级父工程：leyou中已经添加好了。所以直接引入即可：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>ly-item</artifactId>
        <groupId>com.leyou</groupId>
        <version>1.0.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>ly-item-service</artifactId>

    <dependencies>
        <!--web启动器-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--eureka客户端-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <!--通用mapper-->
        <dependency>
            <groupId>tk.mybatis</groupId>
            <artifactId>mapper-spring-boot-starter</artifactId>
        </dependency>
        <!--数据库驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <!--实体类-->
        <dependency>
            <groupId>com.leyou</groupId>
            <artifactId>ly-item-dto</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <!--单元测试-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
        </dependency>
        <!--分页助手-->
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper-spring-boot-starter</artifactId>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```



ly-item-dto中需要什么依赖我们暂时不清楚，所以先不管。



### 7）编写启动和配置

在整个`ly-item工程`中，只有`ly-item-service`是需要启动的。因此在其中编写启动类即可：

```java
/**
 * 黑马程序员
 */
@SpringBootApplication
@EnableDiscoveryClient
@MapperScan("com.leyou.item.mapper")
public class LyItemApplication {
    public static void main(String[] args) {
        SpringApplication.run(LyItemApplication.class, args);
    }
}
```



然后是全局属性文件：

```yaml
server:
  port: 8081
spring:
  application:
    name: item-service
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql:///leyou
    username: root
    password: root
mybatis:
  type-aliases-package: com.leyou.item.entity
  configuration:
    map-underscore-to-camel-case: true
eureka:
  client:
    service-url:
      defaultZone: http://127.0.0.1:10086/eureka
  instance:
    ip-address: 127.0.0.1
    prefer-ip-address: true
```



整个结构：

  ![1544362221352](图片/1544362221352.png)

### 8）添加商品微服务的路由规则

既然商品微服务已经创建，接下来肯定要添加路由规则到gateway中，我们不使用默认的路由规则。

```yaml
server:
  port: 10010
spring:
  application:
    name: ly-gateway
  cloud:
    gateway:
      routes:
        - id: item-service   # 路由id,可以随意写
          # 代理的服务地址；lb表示负载均衡(从eureka中获取具体服务)
          uri: lb://item-service
          # 路由断言，可以配置映射路径
          predicates:
            - Path=/api/item/**
          filters:
            # 表示过滤1个路径，2表示两个路径，以此类推
            - StripPrefix=1
```





### 9）启动测试

我们分别启动：ly-registry，ly-api-gateway，ly-item-service

 ![1551241034715](图片/1551241034715.png)

查看Eureka面板：

![1534081230701](图片/1534081230701.png)









## 16、乐优微服务：创建Common通用模块

有些工具或通用的约定内容，我们希望各个服务共享，因此需要创建一个工具模块：`ly-common`

### 1）创建common工程

使用maven来构建module：

![1551241061737](图片/1551241061737.png)

位置信息：

![1551241076840](图片/1551241076840.png)



结构：

 ![1551241113648](图片/1551241113648.png)

### 2）引入工具类

然后把课前资料提供的工具类引入：

![1574846576296](图片/1574846576296.png) 

 ![1529377719351](图片/1529377719351.png)

然后引入依赖：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>leyou</artifactId>
        <groupId>com.leyou</groupId>
        <version>1.0.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.leyou</groupId>
    <artifactId>ly-common</artifactId>

<dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <scope>compile</scope>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-logging</artifactId>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.9.8</version>
            <scope>compile</scope>
        </dependency>
    </dependencies>
</project>
```



每个工具类的作用：

- BeanHelper：实现Bean属性的拷贝，把一个Bean的属性拷贝到另一个Bean，前提是其属性名一致或部分一致
- CookieUtils：实现cookie的读和写
- IdWorker：生成Id
- JsonUtils：实现实体类与Json的转换
- RegexUtils：常用正则校验
- RegexPattern：常用正则的字符串



### 3）Json工具类

我们在项目中，经常有需求对对象进行JSON的序列化或者反序列化，所以我们定义了一个工具类，JsonUtils：

```java
public class JsonUtils {

    public static final ObjectMapper mapper = new ObjectMapper();

    private static final Logger logger = LoggerFactory.getLogger(JsonUtils.class);

    public static String toString(Object obj) {
        if (obj == null) {
            return null;
        }
        if (obj.getClass() == String.class) {
            return (String) obj;
        }
        try {
            return mapper.writeValueAsString(obj);
        } catch (JsonProcessingException e) {
            logger.error("json序列化出错：" + obj, e);
            return null;
        }
    }

    public static <T> T toBean(String json, Class<T> tClass) {
        try {
            return mapper.readValue(json, tClass);
        } catch (IOException e) {
            logger.error("json解析出错：" + json, e);
            return null;
        }
    }

    public static <E> List<E> toList(String json, Class<E> eClass) {
        try {
            return mapper.readValue(json, mapper.getTypeFactory().constructCollectionType(List.class, eClass));
        } catch (IOException e) {
            logger.error("json解析出错：" + json, e);
            return null;
        }
    }

    public static <K, V> Map<K, V> toMap(String json, Class<K> kClass, Class<V> vClass) {
        try {
            return mapper.readValue(json, mapper.getTypeFactory().constructMapType(Map.class, kClass, vClass));
        } catch (IOException e) {
            logger.error("json解析出错：" + json, e);
            return null;
        }
    }

    public static <T> T nativeRead(String json, TypeReference<T> type) {
        try {
            return mapper.readValue(json, type);
        } catch (IOException e) {
            logger.error("json解析出错：" + json, e);
            return null;
        }
    }
}

```



里面包含四个方法：

- toString：把一个对象序列化为String类型，包含1个参数：

  - `Object obj`：原始java对象

- toList：把一个json反序列化为List类型，需要指定集合中元素类型，包含两个参数：

  - `String json`：要反序列化的json字符串
  - `Class eClass`：集合中元素类型

- toMap：把一个json反序列化为Map类型，需要指定集合中key和value类型，包含三个参数：

  - `String json`：要反序列化的json字符串
  - `Class kClass`：集合中key的类型
  - `Class vClass`：集合中value的类型

- nativeRead：把json字符串反序列化，当反序列化的结果比较复杂时，通过这个方法转换，参数：

  - `String json`：要反序列化的json字符串

  - `TypeReference<T> type`：在传参时，需要传递TypeReference的匿名内部类，把要返回的类型写在TypeReference的泛型中，则返回的就是泛型中类型

  - 例如：

    ```java
    List<User> users = JsonUtils.nativeRead(json, new TypeReference<List<User>>() {});
    ```



测试代码：

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
static class User{
    String name;
    Integer age;
}
public static void main(String[] args) {
    User user = new User("Jack", 21);
    // toString
    //        String json = toString(user);
    //        System.out.println("json = " + json);

    // 反序列化
    //        User user1 = toBean(json, User.class);
    //        System.out.println("user1 = " + user1);

    // toList
    //        String json = "[20, -10, 5, 15]";
    //        List<Integer> list = toList(json, Integer.class);
    //        System.out.println("list = " + list);

    // toMap
    //        String json = "{\"name\":\"Jack\", \"age\": \"21\"}";
    //
    //        Map<String, String> map = toMap(json, String.class, String.class);
    //        System.out.println("map = " + map);

    String json = "[{\"name\":\"Jack\", \"age\": \"21\"}, {\"name\":\"Rose\", \"age\": \"18\"}]";

    List<Map<String, String>> maps = nativeRead(json, new TypeReference<List<Map<String, String>>>() {
    });

    for (Map<String, String> map : maps) {
        System.out.println("map = " + map);
    }
}
```





## 17、乐优微服务：通用异常场景

在项目中出现异常是在所难免的，但是出现异常后怎么处理，这就很有学问了。

### 1）场景

我们预设这样一个场景，假如我们做新增商品，需要接收下面的参数：

```
price：价格
name：名称
```

然后对数据做简单校验：

- 价格不能为空

新增时，自动形成ID，然后随商品对象一起返回

### 2）代码

在ly-item-service中编写实体类：

```java
@Data
public class Item {
    private Integer id;
    private String name;
    private Long price;
}
```

在ly-item-service中编写业务：

service：

```java
@Service
public class ItemService {
    
    public Item saveItem(Item item){
        // 如果名称为空，则抛出异常，返回400状态码，请求参数有误
        if(item.getName() == null){
            throw new RuntimeException("名称不能为空");
        }
        int id = new Random().nextInt(100);
        item.setId(id);
        return item;
    }
}
```

- 这里临时使用随机生成id，然后直接返回，没有做数据库操作

controller：

```java
@RestController
public class ItemController {

    @Autowired
    private ItemService itemService;

    @PostMapping("item")
    public ResponseEntity<Item> saveItem(Item item){
        Item result = itemService.saveItem(item);
        return ResponseEntity.status(HttpStatus.CREATED).body(result);
    }
}
```





### 3）存在问题

现在我们启动项目，做下测试：

通过RestClient去访问：

![1534202307259](图片/1534202307259.png) 

发现参数不正确时，返回了400。虽然上面返回结果正确，但是没有任何有效提示，这样是不是不太友好呢？

响应的错误信息前端根本无法识别，所以我们要和前端坐下来一起统一一下异常的规范，也就是我们微服务中的全局统一异常，异常信息提前约定好，主要包含两部分：==响应码和响应内容==











## 18、乐优微服务：统一异常处理

### 1）自定义异常

首先我们先在公共子模块中定义一个乐优异常类，这样每个模块都可以共用这个异常类。

我们在ly-common模块的 com.leyou.common.exception 包下创建如下异常类

```java
/**
 * 乐优公共异常
 */
@Getter
public class LyException extends RuntimeException{

    private Integer status;//状态码

    public LyException(Integer status, String message) {
        super(message);
        this.status = status;
    }
}
```



### 2）改造ly-item-service

我们把原来的`throw new RuntimeException`改为如下：

```java
throw new LyException(502,"名称不能为空");   
```

参数一已经包含了我们需要的状态码了



**==问题：==**

测试的时候发现，我们我们自己封装的异常信息没有对外抛出，框架底层还是把我们定义的异常当成了RuntimeException处理，返回的响应码不是我们定义的，所以我们需要进一步改造





### 3）全局异常拦截器

接下来，我们使用SpringMVC提供的统一异常拦截器，因为是统一处理，我们放到`ly-common`项目中：

新建一个类，名为：ExceptionHandlerController

所在的包：com.leyou.common.advice

```java
@ControllerAdvice
public class ExceptionHandlerController {

    @ExceptionHandler(LyException.class)//指定只处理 LyException 
    private ResponseEntity<LyException> handlerException(LyException e){
        //e中已经包含了我们定义的异常状态码
        return ResponseEntity.status(e.getStatus()).body(e);
    }
}
```

然后测试

![1574926256968](图片/1574926256968.png) 



但是，虽然我们要的信息都有了，但是返回的异常信息太多太多，很多都是框架的信息，前端根本用不到这些信息，如果我们将这些信息返回，那么响应的数据多了，一定程度上响应了数据的响应速度，所以我们需要进一步优化。





### 4）自定义异常结果类

为了让异常结果更友好，我们不在ExceptionHandlerController返回LyException，而是自定义一个响应的类，这个类只包含异常响应的结果。

所在包：com.leyou.common.exception

```java
/**
 * 异常响应结果类
 */
@Getter
public class ExceptionResult {
    private int status;//响应码
    private String message;//响应信息
    private String timestamp;//响应时间搓

    public ExceptionResult(LyException e) {
        this.status = e.getStatus();
        this.message = e.getMessage();
        this.timestamp = DateTime.now().toString("yyyy-MM-dd HH:mm:ss");
    }
}
```

定义好类后，我们改造全局异常类：

```java
@ControllerAdvice
public class ExceptionHandlerController {

    @ExceptionHandler(LyException.class)
    private ResponseEntity<ExceptionResult> handlerException(LyException e){
        return ResponseEntity.status(e.getStatus()).body(new ExceptionResult(e));
    }
}
```



重启微服务，然后测试

![1574927126678](图片/1574927126678.png) 

到这一步，我们的全局异常问题已经解决了，要的响应码和响应内容都是我们自定义的了。



但是还有一个问题，在实际开发中，我们的异常是很多的，而且这些异常是和前端约定好的，但现在异常响应码和异常响应内容都是写死在代码中，如果某一个响应码或者响应内容需要修改，那么所有用到这个响应码和响应内容的地方都要跟着改，很不方便，会造成很多重复的工作，所以还需要进一步优化。





### 5）自定义异常枚举

因为现在的异常响应信息包含多个内容，所以只用字符串就很难满足我们的需求，所以我们需要定义一个类来装这些信息，但是异常信息其实都是提前约定好的，所以用枚举比较符合我们需求。



枚举：把一件事情的所有可能性列举出来。在计算机中，枚举也可以叫多例，单例是多例的一种情况 。

单例：一个类只能有一个实例。

多例：一个类只能有有限个数的实例。



单例的实现：

- 私有化构造函数
- 在成员变量中初始化本类对象
- 对外提供静态方法，访问这个对象



我们定义一个枚举，用于封装异常状态码和异常信息：

![1574927879074](图片/1574927879074.png) 

枚举代码：

```java
package com.leyou.common.enums;

import lombok.Getter;

/**
 * 黑马程序员
 */
@Getter
public enum ExceptionEnum {
    INVALID_FILE_TYPE(400, "无效的文件类型！"),
    INVALID_PARAM_ERROR(400, "无效的请求参数！"),
    INVALID_PHONE_NUMBER(400, "无效的手机号码"),
    INVALID_VERIFY_CODE(400, "验证码错误！"),
    INVALID_USERNAME_PASSWORD(400, "无效的用户名和密码！"),
    INVALID_SERVER_ID_SECRET(400, "无效的服务id和密钥！"),
    INVALID_NOTIFY_PARAM(400, "回调参数有误！"),
    INVALID_NOTIFY_SIGN(400, "回调签名有误！"),

    CATEGORY_NOT_FOUND(404, "商品分类不存在！"),
    BRAND_NOT_FOUND(404, "品牌不存在！"),
    SPEC_NOT_FOUND(404, "规格不存在！"),
    GOODS_NOT_FOUND(404, "商品不存在！"),
    CARTS_NOT_FOUND(404, "购物车不存在！"),
    APPLICATION_NOT_FOUND(404, "应用不存在！"),
    ORDER_NOT_FOUND(404, "订单不存在！"),
    ORDER_DETAIL_NOT_FOUND(404, "订单数据不存在！"),

    DATA_TRANSFER_ERROR(500, "数据转换异常！"),
    INSERT_OPERATION_FAIL(500, "新增操作失败！"),
    UPDATE_OPERATION_FAIL(500, "更新操作失败！"),
    DELETE_OPERATION_FAIL(500, "删除操作失败！"),
    FILE_UPLOAD_ERROR(500, "文件上传失败！"),
    DIRECTORY_WRITER_ERROR(500, "目录写入失败！"),
    FILE_WRITER_ERROR(500, "文件写入失败！"),
    SEND_MESSAGE_ERROR(500, "短信发送失败！"),
    INVALID_ORDER_STATUS(500, "订单状态不正确！"),
    STOCK_NOT_ENOUGH_ERROR(500, "库存不足！"),

    UNAUTHORIZED(401, "登录失效或未登录！");

    private int status;
    private String message;

    ExceptionEnum(int status, String message) {
        this.status = status;
        this.message = message;
    }
}
```



**改造LyException类**

![1574928285200](图片/1574928285200.png) 



**改造ly-item-service**

![1574928351796](图片/1574928351796.png) 







### 6）全局异常梳理

打开课堂画图，查看大图：

![1574928130563](图片/1574928130563.png) 























## 19、课程总结

![1578114631008](图片/1578114631008.png) 





























