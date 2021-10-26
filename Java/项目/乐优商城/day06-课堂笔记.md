## 01、课程目标

- 独立实现商品编辑后台
- 独立搭建前台系统页面
- 独立编写数据导入功能































## 02、商品修改：商品上下架操作

在商品详情页，每一个商品后面，都会有一个编辑按钮：

![1528874711108](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1528874711108.png) 

点击这个按钮，并没有打开商品编辑窗口，而是弹出了一个提示窗口：

![1552901853155](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1552901853155.png)

已经上架的商品用户可能正在购买，所以不能修改。必须要先下架才可以。



### 1）页面请求

此时打开控制台，可以看到请求已经发出了：

![1552902179096](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1552902179096.png)

请求方式：PUT

请求路径：/spu/saleable

参数有两个 ：

- id：应该是spu的id
- saleable：布尔值，代表上架或下架

返回结果：应该是无





### 2）后台实现：Controller

接下来我们在服务端接收请求，并且修改spu的saleable属性。

需要注意的是，我们在修改spu属性的同时，还需要修改sku的enable属性，因为spu下架，sku也要跟着进行下架。

```java
/**
 * 修改商品上下架
 * @param id 商品spu的id
 * @param saleable true：上架；false：下架
 * @return
 */
@PutMapping("/spu/saleable")
public ResponseEntity<Void> updateSaleable(@RequestParam("id") Long id,
                                           @RequestParam("saleable") Boolean saleable){
    goodsService.updateSaleable(id,saleable);
    return ResponseEntity.status(HttpStatus.NO_CONTENT).build();
}
```





### 3）后台实现：Service

```java
@Transactional
public void updateSaleable(Long spuId, Boolean saleable) {
    // 封装条件
    Spu spu = new Spu();
    spu.setId(spuId);
    spu.setSaleable(saleable);
    // 更新
    int count = spuMapper.updateByPrimaryKeySelective(spu);
    if(count!=1){
        throw new LyException(ExceptionEnum.UPDATE_OPERATION_FAIL);
    }
    // 根据spuId查询sku集合
    Sku record = new Sku();
    record.setSpuId(spuId);
    List<Sku> skuList = skuMapper.select(record);
    // 更新
    for (Sku sku : skuList) {
        // 是否可用： true就是可用；false不可用
        sku.setEnable(saleable);
        skuMapper.updateByPrimaryKeySelective(sku);
    }
}
```



打开页面测试即可。









## 03、商品修改：修改商品回显数据

### 1）编辑按钮点击事件

点击下架商品后，再次点击修改，发现这次没有弹出警告，但是编辑窗口也没有弹出，怎么回事呢？

打开控制台，发现发起了一次请求：

![1552914451933](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1552914451933.png)



编辑是需要数据回显的，而表格数据中只有spu信息，没有spuDetail和sku信息，需要去服务端查询。

因此，接下来我们就编写后台接口，提供查询spuDetail和sku接口。



### 2）根据Spu的id查询SpuDetail

接口文档：

![1575797061845](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575797061845.png) 

#### Controller

```java
@GetMapping("/spu/detail")
public ResponseEntity<SpuDetailDTO> findSpuDetailSpuId(@RequestParam("id") Long id) {
    SpuDetailDTO spuDetailDTO = goodsService.findSpuDetailSpuId(id);
    return ResponseEntity.ok(spuDetailDTO);
}
```



#### Service

```java
public SpuDetailDTO findSpuDetailSpuId(Long id) {
    SpuDetail spuDetail = spuDetailMapper.selectByPrimaryKey(id);
    if(spuDetail == null){
        throw new LyException(ExceptionEnum.GOODS_NOT_FOUND);
    }
    return BeanHelper.copyProperties(spuDetail, SpuDetailDTO.class);
}
```



 

### 3）根据Spu的id查询Sku集合

再次点击修改， 发现又发出了一次请求，这次应该是查询sku：

![1552915043070](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1552915043070.png)

接口文档：

![1575797093977](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575797093977.png) 

#### Controller

```java
@GetMapping("/sku/of/spu")
public ResponseEntity<List<SkuDTO>> findSkuListBySpuId(@RequestParam("id") Long id) {
    List<SkuDTO> list = goodsService.findSkuListBySpuId(id);
    return ResponseEntity.ok(list);
}
```



#### Service

```java
public List<SkuDTO> findSkuListBySpuId(Long id) {
    Sku record = new Sku();
    record.setSpuId(id);
    List<Sku> skus = skuMapper.select(record);
    if(CollectionUtils.isEmpty(skus)){
        throw new LyException(ExceptionEnum.GOODS_NOT_FOUND);
    }
    return BeanHelper.copyWithCollection(skus, SkuDTO.class);
}
```





### 4）测试

编写完成后，随便点击一个编辑按钮，发现数据回显完成：

![1528875639346](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1528875639346.png) 







## 04、商品修改：修改商品数据分析

这里的保存按钮与新增其实是同一个，因此提交的逻辑也是一样的，这里不再赘述。

随便修改点数据，然后点击保存，可以看到浏览器已经发出请求：

![1552916650110](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1552916650110.png)

区别在于，这次的请求中带上了id信息，因为需要根据id来修改数据

包括spuDetail也是一样：

![1552916682788](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1552916682788.png)

但是再来看sku：

![1552916705801](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1552916705801.png)

发现sku中并没有带上sku的id信息，为什么呢？如果没有id我们又该怎样修改呢？

这是因为sku的规格参数修改或删减，可能会导致新增很多sku或者以前的sku直接消失，无法去做修改操作。因此建议的业务逻辑是先对sku进行删除，然后再进行新增。















## 05、商品修改：修改商品后台代码

### 1）Controller

- 请求方式：PUT
- 请求路径：/goods
- 请求参数：SpuDTO对象
- 返回结果：无

```java
/**
 * 修改商品
 * @param spuDTO
 * @return
 */
@PutMapping("/goods")
public ResponseEntity<Void> updateGoods(@RequestBody SpuDTO spuDTO) {
    goodsService.updateGoods(spuDTO);
    return ResponseEntity.status(HttpStatus.NO_CONTENT).build();
}
```



### 2）Service

spu数据可以修改，但是SKU数据无法修改，因为有可能之前存在的SKU现在已经不存在了，或者以前的sku属性都不存在了。比如以前内存有4G，现在没了。

因此这里直接删除以前的SKU，然后新增即可。

代码：

```java
public void updateGoods(SpuDTO spuDTO) {
    //修改spu
    Spu spu = BeanHelper.copyProperties(spuDTO, Spu.class);
    spuMapper.updateByPrimaryKeySelective(spu);
    //修改spuDetail
    SpuDetailDTO spuDetailDTO = spuDTO.getSpuDetail();
    SpuDetail spuDetail = BeanHelper.copyProperties(spuDetailDTO, SpuDetail.class);
    spuDetailMapper.updateByPrimaryKeySelective(spuDetail);
    //删除spu下所有的sku
    Sku record = new Sku();
    record.setSpuId(spu.getId());
    skuMapper.delete(record);
    //保存sku
    List<SkuDTO> skuDTOS = spuDTO.getSkus();
    List<Sku> skus = BeanHelper.copyWithCollection(skuDTOS, Sku.class);
    Date now = new Date();
    skus.forEach(sku -> {
        sku.setSpuId(spu.getId());
        sku.setCreateTime(now);//创建时间
        sku.setUpdateTime(now);//更新时间
    });
    skuMapper.insertList(skus);
}
```



### 3）测试

打开编辑页面，然后修改测试，成功，到此商品的操作我们就做完了！



















## 06、门户网站：搭建门户网站

后台系统的内容暂时告一段落，有了商品，接下来我们就要在页面展示商品，给用户提供浏览和购买的入口，那就是我们的门户系统。

门户系统面向的是用户，安全性很重要，而且搜索引擎对于单页应用并不友好。因此我们的门户系统不再采用与后台系统类似的SPA（单页应用）。

依然是前后端分离，不过前端的页面会使用独立的html，在每个页面中使用vue来做页面渲染。



### 1）静态资源

webpack打包多页应用配置比较繁琐，项目结构也相对复杂。这里为了简化开发（毕竟我们不是专业的前端人员），我们不在使用webpack，而是直接编写原生的静态HTML。

将课前资料中的leyou-portal解压，并把结果赋值到工作空间的目录

![1575856639891](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575856639891.png) 

解压缩：

![1575856696888](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575856696888.png) 

然后通过idea打开，可以看到项目结构：

![1575856847088](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575856847088.png) 



### 2）live-server

#### 简介

没有webpack，我们就无法使用webpack-dev-server运行这个项目，实现热部署。

所以，这里我们使用另外一种热部署方式：live-server

地址；https://www.npmjs.com/package/live-server

![1575857010863](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575857010863.png) 

这是一款带有热加载功能的小型开发服务器。用它来展示你的HTML / JavaScript / CSS，但不能用于部署最终的网站。 



#### 安装和运行参数

安装，使用npm命令即可，这里建议全局安装，以后任意位置可用

```
npm install -g live-server
```



运行时，直接输入命令：

```
live-server
```

另外，你可以在运行命令后，跟上一些参数以配置：

- `--port=NUMBER` - 选择要使用的端口，默认值：PORT env var或8080
- `--host=ADDRESS` - 选择要绑定的主机地址，默认值：IP env var或0.0.0.0（“任意地址”）
- `--no-browser` - 禁止自动Web浏览器启动
- `--browser=BROWSER` - 指定使用浏览器而不是系统默认值
- `--quiet | -q` - 禁止记录
- `--verbose | -V` - 更多日志记录（记录所有请求，显示所有侦听的IPv4接口等）
- `--open=PATH` - 启动浏览器到PATH而不是服务器root
- `--watch=PATH` - 用逗号分隔的路径来专门监视变化（默认值：观看所有内容）
- `--ignore=PATH`- 要忽略的逗号分隔的路径字符串（[anymatch](https://github.com/es128/anymatch) -compatible definition）
- `--ignorePattern=RGXP`-文件的正则表达式忽略（即`.*\.jade`）（**不推荐使用**赞成`--ignore`）
- `--middleware=PATH` - 导出要添加的中间件功能的.js文件的路径; 可以是没有路径的名称，也可以是引用`middleware`文件夹中捆绑的中间件的扩展名
- `--entry-file=PATH` - 提供此文件（服务器根目录）代替丢失的文件（对单页应用程序有用）
- `--mount=ROUTE:PATH` - 在定义的路线下提供路径内容（可能有多个定义）
- `--spa` - 将请求从/ abc转换为/＃/ abc（方便单页应用）
- `--wait=MILLISECONDS` - （默认100ms）等待所有更改，然后重新加载
- `--htpasswd=PATH` - 启用期待位于PATH的htpasswd文件的http-auth
- `--cors` - 为任何来源启用CORS（反映请求源，支持凭证的请求）
- `--https=PATH` - 到HTTPS配置模块的路径
- `--proxy=ROUTE:URL` - 代理ROUTE到URL的所有请求
- `--help | -h` - 显示简洁的使用提示并退出
- `--version | -v` - 显示版本并退出



#### 测试

我们进入leyou-portal目录，输入命令：

```
live-server --port=9002
```

![1575857312796](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575857312796.png) 



### 3）在package.json中配置启动命令

初始化npm

```
npm init -y
```

安装vue

```
npm install vue --save
```

配置启动脚本：

进入package.json文件，在script中添加启动脚本：

```json
{
  "name": "leyou-portal",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "server": "live-server --port=9002"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "vue": "^2.6.10"
  }
}
```

以后可以用 npm run server启动



### 4）域名访问

现在我们访问只能通过：http://127.0.0.1:9002

我们希望用域名访问：http://www.leyou.com

在之前配置后台访问的域名的时候，我们把门户的域名也配置了，所以这里就不用配置了，主要步骤：

第一步，修改hosts文件，添加一行配置：

![1575857694357](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575857694357.png) 



第二步，修改nginx配置，将www.leyou.com反向代理到127.0.0.1:9002

![1575857827272](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575857827272.png) 

如果之前没有配置，现在需要按照上面步骤配置，配置完成后需要重新加载nginx配置：

`nginx.exe -s reload`

![1575857973549](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575857973549.png) 













## 07、门户网站：common.js工具使用

为了方便后续的开发，我们在前台系统中定义了一些工具，放在了common.js中：

![1575858309929](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575858309929.png) 



- 首先对axios进行了一些全局配置，请求超时时间，请求的基础路径，是否允许跨域操作cookie等

  ![1575858773458](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575858773458.png) 

- 定义了对象 ly ，也叫leyou，包含了下面的属性：

  - getUrlParam(key)：获取url路径中的参数

    ![1575858701582](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575858701582.png) 

  - http：axios对象的别名。以后发起ajax请求，可以用ly.http.get()

  - store：localstorage便捷操作，后面用到再详细说明

  - formatPrice：格式化价格，如果传入的是字符串，则扩大100被并转为数字，如果传入是数字，则缩小100倍并转为字符串

    ![1575858879276](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575858879276.png) 

  - formatDate(val, pattern)：对日期对象val按照指定的pattern模板进行格式化

    ![1575858925601](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575858925601.png) 

  - stringify：将对象转为参数字符串

  - parse：将参数字符串变为js对象

    ![1575859566989](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575859566989.png) 



  - 解构表达式

    ![1575860041644](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575860041644.png) 



















## 08、商品搜索：搜索页面结构说明

图片展示：

![1575862471312](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575862471312.png)  

- 搜索栏部分一定要分词，也就是输入的内容要先进行分词，然后再去搜索
- 分类和品牌是固定展示的过滤字段，凡是过滤字段都是词条查询【term查询】，是搜索最小单位
- 规格参数： 规格参数展示动态过滤字段，这些过滤字段会根据分类的选择动态展示
- 搜索后展示的数据：搜索到商品决定了过滤条件的值，也就是能搜索到商品才能对应相应的规格参数，否则规格参数不展示，也就是有搜索结果才有搜索过滤条件。



**==注意：==**

页面展示的图片的地址，都在sku表中存储的，我们在==第五天==的时候，已经导入了sku的图片，并使用图片服务器进行了展示，详情请看第五天的课堂笔记。













## 09、商品搜索：创建搜索微服务

之前我们学习了Elasticsearch的基本应用。今天就学以致用，搭建搜索微服务，利用elasticsearch实现搜索功能。

### 创建module：

![1575873023690](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575873023690.png) 



![1575873060134](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575873060134.png)

### Pom文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>leyou</artifactId>
        <groupId>com.leyou</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>ly-search</artifactId>

    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <!--调用其他微服务-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
        <dependency>
            <groupId>com.leyou</groupId>
            <artifactId>ly-item-client</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
        </dependency>
        <dependency>
            <groupId>com.leyou</groupId>
            <artifactId>ly-common</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
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

这里要导入`openfeign`依赖，因为要从商品微服务中查询商品信息，并导入到索引库。



### application.yml

```yaml
server:
  port: 8083
spring:
  application:
    name: search-service
  data:
    elasticsearch:
      cluster-name: elasticsearch
      cluster-nodes: 192.168.13.111:9300
eureka:
  client:
    service-url:
      defaultZone: http://127.0.0.1:10086/eureka
    registry-fetch-interval-seconds: 5
  instance:
    prefer-ip-address: true
    ip-address: 127.0.0.1
```



### 启动类

```java
@SpringBootApplication
@EnableDiscoveryClient
@EnableFeignClients
public class LySearchApplication {

    public static void main(String[] args) {
        SpringApplication.run(LySearchApplication.class, args);
    }
}
```



### 添加网关路由

在网关ly-gateway的==spring.cloud.gateway.routes== 配置下添加路由：

```yaml
- id: search-service
  uri: lb://search-service
  predicates:
    - Path=/api/search/**
  filters:
    - StripPrefix=2
```





## 10、商品搜索：ElasticSearch环境准备

### 1）安装ElasticSearch

![1575973786265](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575973786265.png) 

把 elasticsearch-6.6.2.zip 解压到没有中文的路径下，然后再把ik插件解压缩，拷贝到es的plugins目录下，启动前，可以修改es的config目录下的jvm.options文件，修改内存大小：默认值是1g，我们改小点，128m或者256m都可以，如果你的电脑内存够，不改也行。

![1575876595669](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575876595669.png) 

### 2）安装head插件

![1575974029838](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575974029838.png) 

head插件有安装版、tomcat版和chrome的插件版，我们采用插件版即可。

### 3）安装kibana

![1575974208240](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575974208240.png) 

启动kibana的前提是，需要安装好nodejs的环境。

解压缩就可以使用了，双击解压后的bin目录的kibana.bat文件即可。













## 11、商品搜索：搜索相关概念回顾

### 1）结构回顾

索引、类型、文档、域

![1575878066949](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575878066949.png) 



### 2）搜索时需要注意的三个点

#### 是否索引

```
索引的字段才能被搜索到，不索引，就意味着不被搜索匹配。
日期，数量，点击数不索引。
```

#### 是否分词

```
分词就意味着要先分词，再拿分词后的词条匹配搜索结果。
如果不分词，就直接把当前搜索的内容当作一个词条。
人名，地名都不分词。
```



#### 是否存储

```
存储就意味着要显示，不存储不显示。
但是如果使用了elasticSearch，默认是所有字段全部存储。
被标记为store=true的是存储在索引库【lucene定义的】中。
如果没有store=true标记，存储在elasticSearch自己的库中。
```

 

这些三部分内容我们都可以通过mapping来定义：

Mapping是用来定义Document中每个Field的特征的（数据类型，是否存储，是否索引，是否分词等）

```
类型名称：就是前面将的type的概念，类似于数据库中的表
字段名：任意填写，下面指定许多属性，例如：

- type：类型，可以是text、long、short、date、integer、object等
- index：是否索引，默认为true
- store：是否存储，默认为false
- analyzer：分词器，这里的ik_max_word即使用ik分词器
```

==注意：store属性，不管设置为什么都会存储，如果有些域不想存，可以通过下面的方法去排除。==



==如果只想存储某几个字段的原始值到Elasticsearch，可以通过incudes参数来设置:==

```json
{
    "yourtype":{
        "_source":{
            "includes":["field1","field2"]
        },
        "properties": {
            ... 
        }
    }
}
```

==同样，可以通过excludes参数排除某些字段：==

```json
{
    "yourtype":{
        "_source":{
            "excludes":["field1","field2"]
        },
        "properties": {
            ... 
        }
    }
}
```









## 12、商品搜索：索引库中Goods对象设计分析

接下来，我们需要商品数据导入索引库，便于用户搜索。

那么问题来了，我们有SPU和SKU，到底把什么数据保存到索引库？

### 1）以结果为导向

大家来看下搜索结果页：

![1526607357743](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1526607357743.png)

可以看到，每一个搜索结果都有至少1个商品，当我们选择大图下方的小图，商品会跟着变化。

因此，**搜索的结果是SPU，即多个SKU的集合**。

既然搜索的结果是SPU，那么我们索引库中存储的应该也是SPU，但是却需要包含SKU的信息。



### 2）需要什么数据

存入索引库的数据分成两种：

- 一种是用来页面渲染的数据，展示给用户看。
- 一种是用来搜索过滤的数据，方便用户搜索。

先来看看页面中直观看到有什么数据，这些数据就是用来渲染的数据：

 ![1526607712207](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1526607712207.png) 



直观能看到的：图片、价格、标题 属于SKU数据（用来展示的数据），另外还有副标题信息。

暗藏的数据：spu的id，sku的id

索引库数据的json示例：

```json
{
    "id": 1,# spu的id
    skus:[
    	{"id":2, "title":"xx", "price":299900, "image":""},
    	{"id":3, "title":"xxx", "price":399900, "image":""},
    ],
	"subTitle":""
}
```



再来看看页面中的过滤条件：

![1526608095471](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1526608095471.png)

这些过滤条件也都需要存储到索引库中，包括：商品分类、品牌、可用来搜索的规格参数

另外还有需要排序的字段：

![1552989729067](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1552989729067.png) 

包括：商品创建时间、商品价格等

综上所述，我们需要的数据格式有：

spu的Id、spu下的所有sku（包括SkuId、图片、价格、标题）、商品分类id、品牌id、商品的创建时间、可搜索的规格参数：

```json
{
    "id": 1,# spu的id
    "skus":[
    	{"id":2, "title":"xx", "price":299900, "image":""},
    	{"id":3, "title":"xxx", "price":399900, "image":""},
    ],
	"subTitle":"",
	"categoryId":11,
	"brandId":122,
	"createTime": 12121,
	"price": [299900,399900],
	"specs":{}
}
```

### 3）最终的数据结构

我们创建一个类，封装要保存到索引库的数据，并设置映射属性：

```java
package com.leyou.search.entity;

import lombok.Data;
import org.springframework.data.annotation.Id;
import org.springframework.data.elasticsearch.annotations.Document;
import org.springframework.data.elasticsearch.annotations.Field;
import org.springframework.data.elasticsearch.annotations.FieldType;

import java.util.Map;
import java.util.Set;

/**
 * 一个SPU对应一个Goods
 */
@Data
@Document(indexName = "goods", type = "docs", shards = 1, replicas = 1)
public class Goods {
    @Id
    @Field(type = FieldType.Keyword)
    private Long id; // spuId
    @Field(type = FieldType.Keyword, index = false)
    private String subTitle;// 卖点
    @Field(type = FieldType.Keyword, index = false)
    private String skus;// sku信息的json结构

    @Field(type = FieldType.Text, analyzer = "ik_max_word")
    private String all; // 所有需要被搜索的信息，包含标题，分类，甚至品牌
    private Long brandId;// 品牌id
    private Long categoryId;// 商品3级分类id
    private Long createTime;// spu创建时间
    private Set<Long> price;// 价格
    private Map<String, Object> specs;// 可搜索的规格参数，key是参数名，值是参数值
}
```

一些特殊字段解释：

- all：用来进行全文检索的字段，里面包含标题、商品分类、品牌、规格等信息

- price：价格数组，是所有sku的价格集合。方便根据价格进行筛选过滤

- skus：用于页面展示的sku信息，因为不参与搜索，所以转为json存储。然后设置不索引，不搜索。包含skuId、image、price、title字段

- specs：所有规格参数的集合。key是参数名，值是参数值。

  例如：我们在specs中存储 内存：4G,6G，颜色为红色，转为json就是：

  ```json
  {
      "specs":{
          "内存":[4G,6G],
          "颜色":"红色"
      }
  }
  ```

  当存储到索引库时，elasticsearch会处理为两个字段：

  - specs.内存 ： [4G,6G]
  - specs.颜色：红色







## 13、商品搜索：创建商品索引库



我们通过kibana来创建索引库及映射：

```json
PUT /goods
{
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 1
  },
  "mappings": {
    "docs":{
      "properties": {
        "id":{
          "type": "keyword"
        },
        "subTitle":{
          "type": "keyword",
          "index": false
        },
        "skus":{
          "type": "keyword",
          "index": false
        },
        "all":{
          "type": "text",
          "analyzer": "ik_max_word"
        }
      },
      "dynamic_templates": [
        {
          "strings": {
            "match_mapping_type": "string",
            "mapping": {
              "type": "keyword"
            }
          }
        }
      ]
    }
  }
}
```















## 14、商品搜索：提供微服务接口

索引库中的数据来自于数据库，我们不能直接去查询商品的数据库，因为真实开发中，每个微服务都是相互独立的，包括数据库也是一样。所以我们只能调用商品微服务提供的接口服务。

先思考我们需要的数据：

- SPU信息
- SKU信息
- SPU的详情
- 商品分类（拼接all字段）
- 规格参数key
- 品牌

再思考我们需要哪些服务：

- 第一：分批查询spu的服务，已经写过。
- 第二：根据spuId查询sku的服务，已经写过
- 第三：根据spuId查询SpuDetail的服务，已经写过
- 第四：根据商品分类id，查询商品分类，没写过。需要一个根据多级分类id查询分类的接口
- 第五：查询分类下可以用来搜索的规格参数：写过
- 第六：根据id查询品牌，没写过



因此我们需要额外提供一个查询商品分类名称的接口以及品牌查询接口。

### 1）商品分类接口

controller：

```java
/**
     * 根据id的集合查询商品分类
     * @param idList 商品分类的id集合
     * @return 分类集合
     */
@GetMapping("list")
public ResponseEntity<List<CategoryDTO>> queryByIds(@RequestParam("ids") List<Long> idList){
    return ResponseEntity.ok(categoryService.queryCategoryByIds(idList));
}
```

service我们就不用添加了，之前编写处理分类和品牌名称的时候已经编写过：

![1576032495824](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576032495824.png) 

测试：

![1576033154083](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576033154083.png) 





### 2）品牌查询接口：

```java
/**
 * 根据id查询品牌
 * @param id
 * @return
 */
@GetMapping("{id}")
public ResponseEntity<BrandDTO> queryById(@PathVariable("id") Long id){
    return ResponseEntity.ok(brandService.queryById(id));
}
```

测试：

![1576033238112](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576033238112.png) 















## 15、商品搜索：FeignClient最佳实战

FeignClient代码遵循SpringMVC的风格，因此与商品微服务的Controller完全一致。这样就存在一定的问题：

- 代码冗余。尽管不用写实现，只是写接口，但服务调用方要写与服务controller一致的代码，有几个消费者就要写几次。
- 增加开发成本。调用方还得清楚知道接口的路径，才能编写正确的FeignClient。





Feign的一种比较友好的实践是这样的：

- 我们的服务提供方不仅提供实体类，还要提供api接口声明
- 调用方不用自己编写接口方法声明，直接调用提供方给的Api接口即可



 

### 第一步：在ly-item-client中引用依赖

![1576026808540](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576026808540.png) 

```xml
<dependencies>
    <dependency>
        <groupId>com.leyou</groupId>
        <artifactId>ly-item-dto</artifactId>
        <version>1.0-SNAPSHOT</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-openfeign-core</artifactId>
    </dependency>
    <dependency>
        <groupId>com.leyou</groupId>
        <artifactId>ly-common</artifactId>
        <version>1.0-SNAPSHOT</version>
    </dependency>
</dependencies>
```



### 第二步：提供feign接口

在服务提供方中提供Feign接口，并编写接口声明

然后把所有需要对外提供的接口写到一个client中：ItemClient

分别到：GoodsController、CategoryController、SpecController、BrandController拷贝所需的方法

```java
package com.leyou.item.client;


import com.leyou.common.vo.PageResult;
import com.leyou.item.dto.*;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestParam;

import java.util.List;

/**
 * @author 黑马程序员
 */
@FeignClient("item-service")
public interface ItemClient {

    /**
     * 分页查询spu
     * @param key       查询条件
     * @param page      当前页
     * @param rows      页大小
     * @param saleable  上下架
     * @return
     */
    @GetMapping("/spu/page")
    public PageResult<SpuDTO> querySpuByPage(
            @RequestParam(value="key",required = false) String key,
            @RequestParam(value="page", defaultValue = "1") Integer page,
            @RequestParam(value="rows", defaultValue = "5") Integer rows,
            @RequestParam(value="saleable", required = false) Boolean saleable
    );

    /**
     * 根据spuId查询sku服务
     * @param id
     * @return
     */
    @GetMapping("/sku/of/spu")
    public List<SkuDTO> findSkuListBySpuId(@RequestParam("id") Long id);


    /**
     * 根据spuId查询spuDetail
     * @param id
     * @return
     */
    @GetMapping("/spu/detail")
    public SpuDetailDTO findSpuDetailSpuId(@RequestParam("id") Long id);

    /**
     * 根据id集合查询分类列表
     * @param idList
     * @return
     */
    @GetMapping("/category/list")
    public List<CategoryDTO> queryByIds(@RequestParam("ids") List<Long> idList);

    /**
     * 根据分类查询规格参数
     * @param gid
     * @param cid
     * @param searching
     * @return
     */
    @GetMapping("/spec/params")
    public List<SpecParamDTO> findSpecParam(
            @RequestParam(value="gid",required = false) Long gid,
            @RequestParam(value="cid",required = false) Long cid,
            @RequestParam(value="searching",required = false) Boolean searching);

    /**
     * 根据id查询品牌
     * @param id
     * @return
     */
    @GetMapping("/brand/{id}")
    public BrandDTO queryById(@PathVariable("id") Long id);
}
```



然后，在ly-search中就无需编写任何Feign代码了，直接就可以调用Client了。

最终微服务结构：

![1576035892780](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576035892780.png) 



### 第三步：测试

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class TestFeign {

    @Autowired
    private ItemClient itemClient;

    @Test
    public void test(){
        PageResult<SpuDTO> spuDTOPageResult = itemClient.querySpuByPage(null, 1, 3, false);
        spuDTOPageResult.getItems().forEach(rs->{
            System.out.println(rs);
        });
    }
}
```

![1576036391645](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576036391645.png) 













