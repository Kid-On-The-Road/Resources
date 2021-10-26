## 01、课程目标

- 实现已选过滤项回显
- 实现取消选择过滤项
- 了解Thymeleaf的基本使用
- 实现商品详情页面
- 实现页面静态化功能

































## 02、搜索过滤筛选：后台拓展请求对象

既然请求已经发送到了后台，那接下来我们就在后台去添加这些条件，首先我们先来分析一下。

我们需要在请求类：`SearchRequest`中添加属性，接收过滤属性。过滤属性都是键值对格式，但是key不确定，所以用一个map来接收即可。

![1576577183196](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576577183196.png) 



添加完成过滤项后，我们就可以去过滤我们的查询了，过滤查询 需要我们传入多个条件，这里需要用到组合查询，接下来我们回顾一下组合查询。

















## 03、搜索过滤筛选：回顾组合条件查询

目前，我们的基本查询是这样的：

![1579080118797](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1579080118797.png) 



我们先做一个抽取：

```java
//构建查询条件
private QueryBuilder buildSearchKey(SearchRequest request) {
    return QueryBuilders.matchQuery("all",request.getKey()).operator(Operator.AND);
}
```

现在，我们要把页面传递的过滤条件也进入进去。

因此不能再使用普通的查询，而是要用到BooleanQuery，基本结构是这样的：

```json
GET /goods/_search
{
  "query": {
    "bool": {
      "must": {
        "match": {
          "all": "手机"
        }
      },
      "filter": {
        "term": {
          "brandId": "18374"
        }
      }
    }
  }
}
```

使用kibana我们完成了组合查询，接下来我们按照这个思路去编写后台代码

















## 04、搜索过滤筛选：组合条件查询后台代码

接下来我们修改组合条件查询方法，方法的返回值为`QueryBuilder`

```java
//构建查询条件
private QueryBuilder buildSearchKey(SearchRequest request) {
    //创建一个组合条件查询对象
    BoolQueryBuilder boolQueryBuilder = QueryBuilders.boolQuery();
    //向组合条件查询中封装搜索条件
    boolQueryBuilder.must(QueryBuilders.matchQuery("all",request.getKey()).operator(Operator.AND));
    //向组合条件查询中封装过滤参数
    Map<String, Object> filters = request.getFilters();
    filters.entrySet().forEach(entry -> {
        String field = entry.getKey();
        Object value = entry.getValue();
        //对域字段进行处理
        if("分类".equals(field)){
            field = "categoryId";
        }else if("品牌".equals(field)){
            field = "brandId";
        }else{
            field = "specs."+field;
        }
        boolQueryBuilder.filter(QueryBuilders.termsQuery(field,value));
    });
    //返回
    return boolQueryBuilder;
}
```

其他不变，然后我们重启微服务测试，打开页面，我们先不点击过滤条件，直接搜索手机：

![1576580663898](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576580663898.png) 

总共181条，接下来，我们点击一个过滤条件：

![1576580939931](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576580939931.png) 

得到结果：

![1576580974950](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576580974950.png) 

















## 05、搜索过滤筛选：页面过滤项超过一个才显示

现在页面基本完成，但是过滤项如果只剩下一个的时候，还可以点击，我们模仿京东，如果过滤项中只剩下一个，我们就不显示他了，这里用到vue的计算属性：

```js
computed: {
    newFilterParamMap(){
        //定义返回值
        const obj = {};
        //遍历map
        for (let key in this.filterParamMap) {
            if(this.filterParamMap[key].length > 1){
                obj[key] = this.filterParamMap[key]
            }
        }
        //一定要有return
        return obj;
    }
},
```

加入了计算属性之后，我们的遍历代码，也要修改：

![1576581884144](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576581884144.png) 

然后测试，ok了！



到此搜索部分我们就做完了，这些页面还有些内容没有完成，例如商品分类面包屑、取消过滤项的选择等等，大家可以课后参考`03、作业`目录下文档来完成。



















## 06、商品详情：实现方案分析

当用户搜索到商品，肯定会点击查看，就会进入商品详情页，接下来我们完成商品详情页的展示，商品详情页在leyou-portal中对应的页面是：item.html

但是不同的商品，到达item.html需要展示的内容不同，该怎么做呢？

- 思路1：统一跳转到item.html页面，然后异步加载商品数据，渲染页面
- 思路2：将请求交给tomcat处理，在服务端完成数据渲染，给不同商品生成不同页面后，返回给用户

我们该选哪一种思路？

思路1：

- 优点：页面加载快，异步处理，用户体验好
- 缺点：会向服务端发起多次数据请求，增加服务端压力

思路2：

- 优点：服务端处理页面后返回，用户拿到是最终页面，不会再次向服务端发起数据请求。
- 缺点：在服务端处理页面，服务端压力过大，tomcat并发能力差



对于大型电商网站而言，必须要考虑的就是服务的高并发问题，因此要尽可能减少服务端压力，提高服务响应速度，所以这里我们两个方案都不会用，我们采用方案3：

方案3：页面静态化

页面静态化：顾名思义，就是把本来需要动态渲染的页面提前渲染完成，生成静态的HTML，当用户访问时直接读取静态HTML，提高响应速度，减轻服务端压力。

这种方式与方案2类似，都是要为每个商品生成独立页面，不同之处在于方案2需要每次都重新渲染，而静态化不需要这么频繁。不过，其中还有很多细节问题需要解决。

我们可以先实现方式2的方式，再来对比两者区别，改造为页面静态化方案。



后两种方案都是在服务端完成页面渲染，以前服务端渲染我们都使用的JSP，不过在SpringBoot中已经不推荐使用Jsp了，因此我们会使用另外的模板引擎技术：Thymeleaf。

与其类似的模板渲染技术有：Freemarker、Velocity等都可以。





























## 07、商品详情：Thymeleaf快速入门

在商品详情页中，我们会使用到Thymeleaf来渲染页面，如果需要先了解Thymeleaf的语法。详见课前资料中《Thymeleaf语法入门.md》

![1576657593637](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576657593637.png) 



接下来我们就来快速入门搞起Thymeleaf



### 1）创建module

![1576658038948](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576658038948.png) 



### 2）导入pom依赖

由于我们现在只是Thymeleaf入门，导入我们只需要导入两个依赖即可：

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

    <artifactId>ly-page</artifactId>

    <dependencies>
        <!--web-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--thymeleaf-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
    </dependencies>
</project>

```



### 3）编写启动类

```java
/**
 * 启动类
 */
@SpringBootApplication
public class LyPageApplication {
    public static void main(String[] args) {
        SpringApplication.run(LyPageApplication.class, args);
    }
}

```



### 4）Controller

```java
@Controller
public class PageController {

    /**
     *  回顾：在springmvc中需要配置一个内部资源视图解析器：InternalResourceViewResolver
     *      prefix : 前缀     /WEB-INF/views
     *      suffix ：后缀     .jsp
     *      寻找JSP路径： prefix + viewName + suffix
     * @return  视图名称
     */
    @GetMapping("/hello")
    public String hello(Model model){
        model.addAttribute("msg","Thymeleaf快速入门");
        return "hello";
    }
}

```





### 5）页面模板

在上一步中我们新建了处理器，并且返回了一个hello字符串，现在我们要开发一个页面，让它跳转到这个视图模板中。

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1 th:text="${msg}">欢迎光临</h1>
</body>
</html>

```

不需要做任何配置，启动器已经帮我们把Thymeleaf的视图器配置完成：

 ![1526435647041](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1526435647041.png)

而且，还配置了模板文件（html）的位置，与jsp类似的前缀+ 视图名 + 后缀风格：

 ![1526435706301](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1526435706301.png)

- 默认前缀：`classpath:/templates/`
- 默认后缀：`.html`

所以如果我们返回视图：`hello`，会指向到 `classpath:/templates/hello.html`













## 08、商品详情：Thymeleaf其他语法

### 1）页面缓存

Thymeleaf默认会开启页面缓存，提高页面并发能力。但会导致我们修改页面不会立即被展现，因此我们关闭缓存：

```properties
# 关闭Thymeleaf的缓存
spring.thymeleaf.cache=false

```

另外，修改完毕页面，需要使用快捷键：`Ctrl + Shift + F9`来刷新工程。



### 2）其他语法

详细的我们跟着《Thymeleaf语法入门.md》文档走即可。





















## 09、商品详情：完成商品详情页面的跳转

Thymeleaf我们已经入门完了，接下来，我就来开始做商品详情页了。



### 1）静态页面

在我们资料包下有个`item.html`，我们直接拿来用即可：

![1576665605642](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576665605642.png) 

把我们把这个文件拷贝到`ly-page项目下`

![1576665544677](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576665544677.png) 



### 2）Controller跳转到这个页面

我们修改PageController，让他能跳转到item.html

```java
@Controller
public class PageController {

    @GetMapping("/item/{id}.html")
    public String toItemHtml(@PathVariable("id") Long id){
        return "item";
    }
}

```



另外，我们的商品详情微服务端口要修改下，打开开发文档可知，端口为8084：

![1576811682097](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576811682097-1579044307142.png) 

打开该文档可知，微服务端口为：8084，所以我们打开application.yml把默认端口改为8084：

```yaml
server:
  port: 8084
```



重启`ly-page`测试：

![1576812416099](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576812416099.png) 

虽然报错了，但是说明已经跳转过来了。





### 3）Nginx配置反向代理跳转

页面能跳转了，但是我们不会直接访问这个页面，而是在`www.leyou.com`中跳转过来的，所以我需要在nginx哪里配置一下反向代理。



配置之前，我们先修改`search.html`我们修改这个页面的跳转链接：

![1576667018192](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576667018192.png) 

改完这个页面后，我们点击商品图片可以跳转到相应的链接了：

![1576667119695](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576667119695.png) 

但是报没有这个页面，为什么呢？原因是我们的`www.leyou.com`访问的是`leyou-portal`项目，而这个项目根本没有这个`57.html` ，所以报错了，这个是正常的，接下来我们就通过nginx来配置反向代理到`ly-page`

我们打开nginx的配置文件，然后修改如下：

```nginx
server {
	listen       80;
	server_name  www.leyou.com;
	
	location /item {
		proxy_pass   http://localhost:8084;
		proxy_connect_timeout 600;
		proxy_read_timeout 5000;
	}
	
	location / {
	    proxy_pass   http://leyou-portal;
		proxy_connect_timeout 600;
		proxy_read_timeout 5000;
	}
}

```

修改配置完成后，重启nginx，然后访问页面如下：

![1576667380905](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576667380905.png) 

说明反向代理成功了，接下来我们开发这个详情页面！

![1579143643576](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1579143643576.png) 





























## 10、商品详情：分析item页面需要什么数据

昨天我们已经学习了Thymeleaf，并且商品详情页面我们已经可以正常跳转了，但是页面报错，原因是页面还没有数据，报错了，原因是我们没有给这个页面提供相应的数据，接下来我们就先分析一下这个详情页面需要的一些数据。



我们打开`item.html`然后搜索`${`符号，因为昨天我们知道，页面获取从后台返回的变量都是在==spel==表达式里去获取的，所以我们搜索有哪些变量，然后我们在`PageController`中去添加这些变量即可。



### 1）改造PageController

```java
@Controller
public class PageController {

    @Autowired
    private PageService pageService;

    /**
     * 跳转到商品详情页
     */
    @GetMapping("/item/{id}.html")
    public String toItemHtml(Model model, @PathVariable("id") Long id){
        //查询模型数据
        Map<String,Object> itemData = pageService.loadItemData(id);
        //存入模型数据，因为数据较多，直接存入一个map
        model.addAllAttributes(itemData);
        return "item";
    }
}
```

注意我们把数据的查询和加载放到了一个PageService中去完成，数据较多，因此这里的PageService方法返回的是一个Map。

### 2）新增PageService

我们创建一个PageService，在里面来封装数据模型。

下面个map中的每个key，我们都是从`item.html`页面拷贝的：

```java
@Service
public class PageService {

    public Map<String, Object> loadItemData(Long id) {
        Map<String, Object> data = new HashMap<>();
        data.put("categories", null); // TODO：三级分类的对象集合
        data.put("brand", null);      // TODO：品牌对象
        data.put("spuName", null);    // TODO: 来自于SpuDTO
        data.put("subTitle", null);   // TODO：来自于SpuDTO
        data.put("skus", null);       // TODO：sku集合
        data.put("detail", null);     // TODO：SpuDetail对象
        data.put("specs", null);      // TODO：规格组集合，每个规格组中有个规格参数的集合
        return data;
    }
}
```

我们把需要的数据都用`TODO`来标记，然后按照需要的对象，一个一个去查询。

### 3）item页面需要什么？

> categories ： 分类集合
>
> brand：     品牌对象
>
> spuName： spu的name
>
> subTitle：副标题
>
> detail：  商品详情
>
> skus： sku的集合
>
> specs： 规格参数的集合

### 4）需要什么接口？

>根据id查询spu															 还没有：
>
>根据分类id查询商品分类											 有了
>
>根据id查询品牌															有了
>
>根据spuId查询SpuDetail										    有了：
>
>根据Spu查询sku的集合											   有了：
>
>根据分类id查询specs： 包含规格组及组内的规格参数			  还没有



也就是上述接口中，我需要编写两个接口即可。





















## 11、商品详情：提供根据spuid查询spu对象

### 1）在GoodsController中添加方法

```java
/**
 * 根据spu的id查询spu
 * @param id  spu的id
 * @return   spu对象
 */
@GetMapping("/spu/{id}")
public ResponseEntity<SpuDTO> querySpuById(@PathVariable("id") Long id){
    return ResponseEntity.ok(goodsService.querySpuById(id));
}
```





### 2）在GoodsService中添加方法

我们添加了方法，去查询spu。但这里我们优化一下，因为我们需要的数据比较多，也就是调用的接口比较多，这样会比较占用带宽，我们可以把`根据spuId查询SpuDetail`和 `根据Spu查询sku的集合`这两个接口的数据都在这个方法中返回，这样比较节省带宽。

SpuDTO中已经了SpuDetail和Sku集合，所以我们直接封装到这个方法的返回值即可：

![1576823756364](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576823756364.png) 

方法如下：

```java
public SpuDTO querySpuById(Long id) {
    //查询spu
    Spu spu = spuMapper.selectByPrimaryKey(id);
    if(spu == null){
        throw new LyException(ExceptionEnum.GOODS_NOT_FOUND);
    }
    //转换
    SpuDTO spuDTO = BeanHelper.copyProperties(spu, SpuDTO.class);
    //查询detail
    spuDTO.setSpuDetail(findSpuDetailSpuId(id));
    //查询skus
    spuDTO.setSkus(findSkuListBySpuId(id));
    return spuDTO;
}
```













## 12、商品详情：查询规格组及组内参数

编写完第一个接口，接下来我们编写下一组接口：

我们在页面展示规格时，需要按组展示：

 ![1535423451465](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1535423451465.png)

组内有多个参数，为了方便展示。我们提供一个接口，查询规格组，同时在规格组中持有组内的所有参数。

> 拓展`SpecGroupDTO`类：

我们在`SpecGroupDTO`中添加一个`SpecParamDTO`的集合，保存改组下所有规格参数

```java
@Data
public class SpecGroupDTO {
    private Long id;

    private Long cid;

    private String name;
    
    private List<SpecParamDTO> params;
}
```

> SpecController

```java
/**
 * 查询规格参数组，及组内参数
 * @param id 商品分类id
 * @return 规格组及组内参数
 */
@GetMapping("/of/category")
public ResponseEntity<List<SpecGroupDTO>> querySpecsByCid(@RequestParam("id") Long id){
    return ResponseEntity.ok(specService.querySpecsByCid(id));
}
```

> SpecService

```java
public List<SpecGroupDTO> querySpecs(Long cid) {
    //查询规格组
    List<SpecGroupDTO> groupDTOS = findSpecGroupByCid(cid);
    //查询规格参数，当前分类下所有的组
    List<SpecParamDTO> params = findSpecParam(null, cid, null);
    //把规格参数按照groupId分组，groupId一致的放到一个集合中
    //key是groupid，值是group一致的param集合
    Map<Long, List<SpecParamDTO>> map = params.stream()
        .collect(Collectors.groupingBy(SpecParamDTO::getGroupId));
    //遍历
    for (SpecGroupDTO groupDTO : groupDTOS) {
        groupDTO.setParams(map.get(groupDTO.getId()));
    }
    return groupDTOS;
}
```

在service中，我们调用之前编写过的方法，查询规格组，和规格参数，然后封装返回。









## 13、商品详情：查询数据渲染商品详情页

编写完成后，接下来我们重启微服务，然后测试我们编写完的两个接口：

### 1）根据分类id查询规格组及组内参数

![1576826734154](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576826734154.png) 

### 2）根据id查询spu

![1576827038218](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576827038218.png) 



### 3）测试成功后，把这些接口提取到item-client中

在ItemClient中加入两个方法：

```java
/**
 * 根据分类id，查询分类下所有规格组和组内参数
 * @param cid  分类id
 * @return     规格组及参数
 */
@GetMapping("/spec/of/category")
public List<SpecGroupDTO> querySpecs(@RequestParam("id") Long cid);

/**
 * 根据spu的id查询spu
 * @param id  spu的id
 * @return   spu对象
 */
@GetMapping("/spu/{id}")
public SpuDTO querySpuById(@PathVariable("id") Long id);
```

在item-client加入方法后，接下来我们要远程调用了，需要远程调用那么就要进行Feign的配置



### 4）远程调用OpenFeign的配置

我们需要在`ly-page`的`PageController`中远程调用其他接口，所以我们需要在这个微服务中进行如下的配置：

> 添加feign、eureka等的依赖

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

    <artifactId>ly-page</artifactId>

    <dependencies>
        <!--web-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--thymeleaf-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <!--eureka-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <!--feign-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
        <!-- 商品服务接口 -->
        <dependency>
            <groupId>com.leyou</groupId>
            <artifactId>ly-item-client</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <!--单元测试-->
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

### 5）修改启动类

```java
/**
 * 启动类
 */
@SpringBootApplication
@EnableFeignClients //远程调用
@EnableDiscoveryClient//注册中心
public class LyPageApplication {
    public static void main(String[] args) {
        SpringApplication.run(LyPageApplication.class, args);
    }
}
```



### 6）修改配置文件

```yml
server:
  port: 8084
spring:
  application:
    name: page-service
  thymeleaf:
    cache: false
eureka:
  client:
    service-url:
      defaultZone: http://127.0.0.1:10086/eureka
```





### 7）在PageService中注入ItemClient

```java
package com.leyou.page.service;

import com.leyou.item.client.ItemClient;
import com.leyou.item.dto.BrandDTO;
import com.leyou.item.dto.CategoryDTO;
import com.leyou.item.dto.SpecGroupDTO;
import com.leyou.item.dto.SpuDTO;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

@Service
public class PageService {

    @Autowired
    private ItemClient itemClient;

    public Map<String, Object> loadItemData(Long id) {
        //查询spu
        SpuDTO spu = itemClient.querySpuById(id);
        //查询分类
        List<CategoryDTO> categorys = itemClient.queryByIds(spu.getCategoryIds());
        // 查询品牌
        BrandDTO brand = itemClient.queryById(spu.getBrandId());
        //查询规格
        List<SpecGroupDTO> specs = itemClient.querySpecs(spu.getCid3());

        Map<String, Object> data = new HashMap<>();
        data.put("categories", categorys); // 三级分类的对象集合
        data.put("brand", brand);      // 品牌对象
        data.put("spuName", spu.getName());    // 来自于SpuDTO
        data.put("subTitle", spu.getSubTitle());   // 来自于SpuDTO
        data.put("skus", spu.getSkus());       // sku集合
        data.put("detail", spu.getSpuDetail());     // SpuDetail对象
        data.put("specs", specs);      // 规格组集合，每个规格组中有个规格参数的集合
        return data;
    }
}

```



最后重启微服务测试：

![3](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/3.gif)















## 14、页面静态化：页面静态化思路

### 1）问题分析

现在，我们的页面是通过Thymeleaf模板引擎渲染后返回到客户端。在后台需要大量的数据查询，而后渲染得到HTML页面。会对数据库造成压力，并且请求的响应时间过长，并发能力不高。

大家能想到什么办法来解决这个问题？

- 缓存，后端可以对要查询的数据进行缓存，提高查询效率，减小数据库压力。
  - 优点：以后无论是页面查询数据、移动的查询数据，都走缓存，更通用
  - 缺点：请求依然会到达服务端，造成服务端压力。
- 静态化，把页面渲染后写入HTML文件，以后不再查询服务端数据
  - 优点：不访问服务端，效率更高
  - 缺点：适应于H5，APP中需要另外的静态化方案







### 2）什么是静态化

静态化是指把动态生成的HTML页面变为静态内容保存，以后用户的请求到来，直接访问静态页面，不再经过服务的渲染。

而静态的HTML页面可以部署在nginx中，从而大大提高并发能力，减小tomcat压力。





### 3）如何实现静态化

目前，静态化页面都是通过模板引擎来生成，而后保存到nginx服务器来部署。常用的模板引擎比如：

- Freemarker
- Velocity
- Thymeleaf

我们之前就使用的Thymeleaf，来渲染html返回给用户。Thymeleaf除了可以把渲染结果写入Response，也可以写到本地文件，从而实现静态化。



























