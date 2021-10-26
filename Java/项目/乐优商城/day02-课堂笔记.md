## 01、课程目标

- 会使用nginx进行反向代理
- 实现商品分类查询功能
- 了解跨域解决方案
- 掌握cors解决跨域































## 02、解决域名解析问题

### 1）统一环境

我们现在访问页面使用的是：http://localhost:8081

有没有什么问题？

实际开发中，会有不同的环境：

- 开发环境：自己的电脑
- 测试环境：提供给测试人员使用的环境
- 预发布环境：数据是和生成环境的数据一致，运行最新的项目代码进去测试
- 生产环境：项目最终发布上线的环境

如果不同环境使用不同的ip去访问，可能会出现一些问题。为了保证所有环境的一致，我们会在各种环境下都使用域名来访问。

我们将使用以下域名：

- 主域名是：www.leyou.com，
- 管理系统域名：manage.leyou.com
- 网关域名：api.leyou.com
- ...

但是最终，我们希望这些域名指向的还是我们本机的某个端口。

那么，当我们在浏览器输入一个域名时，浏览器是如何找到对应服务的ip和端口的呢？



域名要访问，先从==浏览器缓存==中解析，然后从==本地host文件==解析，最后去==域名根服务器==解析域名。



### 2）域名解析

一个域名一定会被解析为一个或多个ip。这一般会包含两步：

- 本地域名解析

  浏览器会首先在本机的hosts文件中查找域名映射的IP地址，如果查找到就返回IP ，没找到则进行域名服务器解析，一般本地解析都会失败，因为默认这个文件是空的。

  - Windows下的hosts文件地址：C:/Windows/System32/drivers/etc/hosts
  - Linux下的hosts文件所在路径： /etc/hosts 

- 域名服务器解析

  本地解析失败，才会进行域名服务器解析，域名服务器就是网络中的一台计算机，里面记录了所有注册备案的域名和ip映射关系，一般只要域名是正确的，并且备案通过，一定能找到。



### 3）解决域名解析问题

我们不可能去购买一个域名，因此我们可以伪造本地的hosts文件，实现对域名的解析。修改本地的host为：

```
127.0.0.1 www.leyou.com
127.0.0.1 api.leyou.com
127.0.0.1 manage.leyou.com
127.0.0.1 image.leyou.com
```

这样就实现了域名的关系映射了。

每次在C盘寻找hosts文件并修改是非常麻烦的，给大家推荐一个快捷修改host的工具，在课前资料中可以找到：两个版本，我们用新版的就好

![1574933694604](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1574933694604.png) 

效果：

![1574933827668](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1574933827668.png) 

我们添加了四个映射关系：

- 127.0.0.1 api.leyou.com           ：我们的网关Zuul
- 127.0.0.1 manage.leyou.com  ：我们的后台系统地址
- 127.0.0.1 www.leyou.com        : 这个是网站主页
- 127.0.0.1 image.leyou.com      : 这个是图片地址

现在，ping一下域名试试是否畅通：

![1574933988404](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1574933988404.png) 

OK了！ 









## 03、Nginx的简介

虽然域名解决了，但是现在如果我们要访问，还得自己加上端口：`http://manage.leyou.com:8081`。

这就不够优雅了。我们希望的是直接域名访问：`http://manage.leyou.com`。这种情况下端口默认是80，如何才能把请求转移到9001端口呢？

这里就要用到反向代理工具：Nginx

### 1）什么是Nginx

![1526187409033](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1526187409033.png) 



nginx可以作为web服务器，但更多的时候，我们把它作为网关，因为它具备网关必备的功能：

- 反向代理
- 负载均衡
- 动态路由
- 请求过滤



### 2）Nginx作为web服务器

Web服务器分2类：

- web应用服务器，如：
  - tomcat
  - resin
  - jetty
- web服务器，如：
  - Apache 服务器
  - Nginx
  - IIS

区分：web服务器不能解析jsp等页面，只能处理js、css、html等静态资源。
并发：web服务器的并发能力远高于web应用服务器。



### 3）Nginx作为反向代理

什么是反向代理？

- 代理：通过客户机的配置，实现让一台服务器代理客户机，客户的所有请求都交给代理服务器处理。
- 反向代理：用一台服务器，代理真实服务器，用户访问时，不再是访问真实服务器，而是代理服务器。

nginx可以当做反向代理服务器来使用：

- 我们需要提前在nginx中配置好反向代理的规则，不同的请求，交给不同的真实服务器处理
- 当请求到达nginx，nginx会根据已经定义的规则进行请求的转发，从而实现路由功能



利用反向代理，就可以解决我们前面所说的端口问题，如图

如果是安装在本机：

![1526016663674](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1526016663674.png) 





















## 04、Nginx的反向代理配置

先安装Nginx，安装非常简单，把课前资料提供的nginx直接解压即可，绿色免安装，舒服！

![img](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/xiaoyaya.gif) 



把软件解压到本地某个目录即可，目录结构：

![1574996389482](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1574996389482.png) 



> ### 使用

nginx可以通过命令行来启动，操作命令：

- 启动：`start nginx.exe`
- 停止：`nginx.exe -s stop`
- 重新加载：`nginx.exe -s reload`

我们需要让nginx反向代理我们的服务器，因此需要自定义nginx配置。

![1574996542375](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1574996542375.png) 

为了看起来清爽，我们先在nginx主配置文件`nginx.conf`中使用include指令引用我们的配置：

```
	include vhost/*.conf;
```

如图所示：

![1574996632209](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1574996632209.png) 

然后在nginx.conf所在目录新建文件夹vhost：

![1574996714707](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1574996714707.png) 

并在vhost中创建文件leyou.conf：

![1574997008925](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1574997008925.png) 

填写如下配置：

```nginx
upstream leyou-manage{
	server	127.0.0.1:9001;
}
upstream leyou-gateway{
	server	127.0.0.1:10010;
}
upstream leyou-portal{
	server	127.0.0.1:9002;
}

server {
	listen       80;
	server_name  manage.leyou.com;
	
	location / {
	    proxy_pass   http://leyou-manage;
		proxy_connect_timeout 600;
		proxy_read_timeout 5000;
	}
}
server {
	listen       80;
	server_name  www.leyou.com;
	
	location / {
	    proxy_pass   http://leyou-portal;
		proxy_connect_timeout 600;
		proxy_read_timeout 5000;
	}
}
server {
	listen       80;
	server_name  api.leyou.com;
	
	location / {
	    proxy_pass   http://leyou-gateway;
		proxy_connect_timeout 600;
		proxy_read_timeout 5000;
	}
}
```

解读：

- upstream：定义一个负载均衡集群，例如leyou-manage
  - server：集群中某个节点的ip和port信息，可以配置多个，实现负载均衡，默认轮询
- server：定义一个监听服务配置
  - listen：监听的断开
  - server_name：监听的域名
  - location：匹配当前域名下的哪个路径。例如：`/`，代表的是一切路径
    - proxy_pass：监听并匹配成功后，反向代理的目的地，可以指向某个ip和port，或者指向upstream定义的负载均衡集群，nginx反向代理时会轮询中服务列表中选择。





> ### 使用域名访问测试

重载nginx，然后用域名访问后台管理系统：

![1574997922919](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1574997922919.png) 

现在实现了域名访问网站了，中间的流程是怎样的呢？

![1575000188871](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575000188871.png) 

1. 浏览器准备发起请求，访问http://mamage.leyou.com，但需要进行域名解析

2. 优先进行本地域名解析，因为我们修改了hosts，所以解析成功，得到地址：127.0.0.1（本机）

3. 请求被发往解析得到的ip，并且默认使用80端口：http://127.0.0.1:80

   本机的nginx一直监听80端口，因此捕获这个请求

4. nginx中配置了反向代理规则，将manage.leyou.com代理到http://127.0.0.1:9001

5. 主机上的后台系统的webpack server监听的端口是9001，得到请求并处理，完成后将响应返回到nginx

6. nginx将得到的结果返回到浏览器











## 05、表关系分析：商品-分类-品牌

商城的核心自然是商品，而商品多了以后，肯定要进行分类，并且不同的商品会有不同的品牌信息，其关系如图所示：

![1525999005260](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1525999005260.png)

- 一个商品分类下有很多商品
- 一个商品分类下有很多品牌
- 而一个品牌，可能属于不同的分类
- 一个品牌下也会有很多商品



因此，我们需要依次去完成：商品分类、品牌、商品的开发。















## 06、商品分类：数据库表的结构分析

### 1）分析分类表

分类和分类之前是有自己的关系的，比如一个手机分类下面又有手机配件等分类，手机配件下面又有其他分类，怎么样快速找到他们的关系？   

让他们各自找自己的爹就好了。







### 2）导入数据

首先导入课前资料提供的sql：

![1575011957021](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575011957021.png) 

我们先看商品分类表：

![1575012157114](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575012157114.png) 

```sql
CREATE TABLE `tb_category` (
  `id` BIGINT(20) NOT NULL AUTO_INCREMENT COMMENT '类目id',
  `name` VARCHAR(20) NOT NULL COMMENT '类目名称',
  `parent_id` BIGINT(20) NOT NULL COMMENT '父类目id,顶级类目填0',
  `is_parent` TINYINT(1) NOT NULL COMMENT '是否为父节点，0为否，1为是',
  `sort` INT(4) NOT NULL COMMENT '排序指数，越小越靠前',
  `create_time` TIMESTAMP NULL COMMENT '数据创建时间',
  `update_time` TIMESTAMP NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '数据更新时间',
  PRIMARY KEY (`id`),
  KEY `key_parent_id` (`parent_id`) USING BTREE
) ENGINE=INNODB AUTO_INCREMENT=1424 DEFAULT CHARSET=utf8 COMMENT='商品类目表，类目和商品(spu)是一对多关系，类目与品牌是多对多关系';
```

因为商品分类会有层级关系，因此这里我们加入了`parent_id`字段，对本表中的其它分类进行自关联。











## 07、商品分类：页面分析

### 1）页面分析

首先我们看下要实现的效果：

![1575013720541](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575013720541.png) 

商品分类之间是会有层级关系的，采用树结构去展示是最直观的方式。

一起来看页面，对应的是/pages/item/Category.vue：

![1575013837176](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575013837176.png) 

页面模板：

```html
<v-card>
    <v-flex xs12 sm10>
        <v-tree url="/item/category/list"
                :isEdit="isEdit"
                @handleAdd="handleAdd"
                @handleEdit="handleEdit"
                @handleDelete="handleDelete"
                @handleClick="handleClick"
                />
    </v-flex>
</v-card>
```

- `v-card`：卡片，是vuetify中提供的组件，提供一个悬浮效果的面板，一般用来展示一组数据。

  ![1575014301065](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575014301065.png) 

- `v-flex`：布局容器，用来控制响应式布局。与BootStrap的栅格系统类似，整个屏幕被分为12格。我们可以控制所占的格数来控制宽度：

  ![1526001573140](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1526001573140.png) 

  本例中，我们用`sm10`控制在小屏幕及以上时，显示宽度为10格

- `v-tree`：树组件。Vuetify并没有提供树组件，这个是我们自己编写的自定义组件：

  ![1575013978201](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575013978201.png) 

  里面涉及一些vue的高级用法，大家暂时不要关注其源码，会用即可。



### 2）树组件的用法

也可参考课前资料中的：《自定义Vue组件的用法.md》



这里我贴出树组件的用法指南。

> 属性列表：

| 属性名称 | 说明                             | 数据类型 | 默认值 |
| :------- | :------------------------------- | :------- | :----- |
| url      | 用来加载数据的地址，即延迟加载   | String   | -      |
| isEdit   | 是否开启树的编辑功能             | boolean  | false  |
| treeData | 整颗树数据，这样就不用远程加载了 | Array    | -      |

这里推荐使用url进行延迟加载，**每当点击父节点时，就会发起请求，根据父节点id查询子节点信息**。

当有treeData属性时，就不会触发url加载

远程请求返回的结果格式：

```json
[
    { 
        "id": 74,
        "name": "手机",
        "parentId": 0,
        "isParent": true,
        "sort": 2
	},
     { 
        "id": 75,
        "name": "家用电器",
        "parentId": 0,
        "isParent": true,
        "sort": 3
	}
]
```



> 事件：

| 事件名称     | 说明                                       | 回调参数                                         |
| :----------- | :----------------------------------------- | :----------------------------------------------- |
| handleAdd    | 新增节点时触发，isEdit为true时有效         | 新增节点node对象，包含属性：name、parentId和sort |
| handleEdit   | 当某个节点被编辑后触发，isEdit为true时有效 | 被编辑节点的id和name                             |
| handleDelete | 当删除节点时触发，isEdit为true时有效       | 被删除节点的id                                   |
| handleClick  | 点击某节点时触发                           | 被点击节点的node对象,包含全部信息                |

> 完整node的信息

回调函数中返回完整的node节点会包含以下数据：

```json
{
    "id": 76, // 节点id
    "name": "手机", // 节点名称
    "parentId": 75, // 父节点id
    "isParent": false, // 是否是父节点
    "sort": 1, // 顺序
    "path": ["手机", "手机通讯", "手机"] // 所有父节点的名称数组
}
```





### 3）树组件与后台交互

给大家的页面中已经配置了url，我们刷新页面看看会发生什么：

```html
<v-tree url="/item/category/of/parent"
        :isEdit="isEdit"
        @handleAdd="handleAdd"
        @handleEdit="handleEdit"
        @handleDelete="handleDelete"
        @handleClick="handleClick"
        />
```

刷新页面，可以看到：

 ![1552992441349](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1552992441349.png)

页面发起了一条请求：http://api.leyou.com/api/item/category/of/parent?pid=0 



大家可能会觉得很奇怪，我们明明是使用的相对路径，讲道理发起的请求地址应该是：

http://manage.leyou.com/item/category/of/parent

但实际却是：

http://api.leyou.com/api/item/category/of/parent?pid=0 

这是因为，我们有一个全局的配置文件，对所有的请求路径进行了约定：

![1575015552439](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575015552439.png) 

里面已经定义了api的前缀：

![1575015635649](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575015635649.png) 

路径是http://api.leyou.com/api，因此页面发起的一切请求都会以这个路径为前缀。

而我们的Nginx又完成了反向代理，将这个地址代理到了http://127.0.0.1:10010，也就是我们的Gateway网关，最终再被Gateway转到微服务，由微服务来完成请求处理并返回结果。









## 08、商品分类：使用Axios与微服务交互



### 1）引入Axios

package.json引入依赖：

![1575266598731](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575266598731.png) 

在js文件中引入：

![1575266690896](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575266690896.png) 



### 2）get请求

```js
//第一种：可以把参数直接写到url的?后面
axios.get('/user?ID=12345')
  .then(function (response) {
    // 成功执行
    console.log(response);
  })
  .catch(function (error) {
    // 失败执行
    console.log(error);
  })
  .finally(function () {
    // 总是执行
  });




// 第二种：get请求传参也可以使用下面这种方式
axios.get('/user', {
    params: {
      ID: 12345
    }
  })
  .then(function (response) {
    //成功执行
    console.log(response);
  })
  .catch(function (error) {
    //失败执行
    console.log(error);
  })
  .finally(function () {
    //总是执行
  });  
```





### 3）post请求

```js
//post请求
axios.post('/user', {
    firstName: 'Fred',
    lastName: 'Flintstone'
  })
  .then(function (response) {
    //成功执行
    console.log(response);
  })
  .catch(function (error) {
    //失败执行
    console.log(error);
  });
```





### 4）设置baseURL地址

baseURL就是一个基本url地址，所有请求都会先拼接上baseURL。

![1575267208993](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575267208993.png) 



### 5）把Axios加入Vue的全局属性中

以后我们会有很多个vue页面，每个页面如果都引入axios，那很麻烦，我们想要的是一次引入多次使用，怎么实现？



我们可以在Vue中加入一个我们自定义的属性，然后调用即可，怎么在一个js对象中加入一个自定义属性呢？靠的是js的prototype属性。

![1575267492933](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575267492933.png) 



我们在http.js中直接添加一个http属性，以后直接调用即可：

![1575267582669](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575267582669.png) 

然后在vue页面中就可以直接调用了：

![1575267683266](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575267683266.png) 













## 09、商品分类：微服务接口实现

### 1）实体类

我们在**ly-item-service**中添加Category实体类，放到pojo包下，代表与数据库表映射的对象：

```java
/**
 * 黑马程序员
 */
@Table(name="tb_category")
@Data
public class Category {
	@Id
	@KeySql(useGeneratedKeys=true)
	private Long id;
	private String name;
	private Long parentId;
	private Boolean isParent;
	private Integer sort;
	private Date createTime;
	private Date updateTime;
}
```



我们在**ly-item-dto**中添加CategoryDTO实体类，放到dto包下，代表数据传输对象，用于隐藏数据库字段：

页面需要的数据：

![1575024071551](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575024071551.png) 

这里返回结果中并不包含createTime和updateTime字段，所以为了符合页面需求，我们需要给页面量身打造一个用来传递的数据转移对象（Data Transfer Object），简称为DTO。

```java
/**
 * 黑马程序员
 */
@Data
public class CategoryDTO {
	private Long id;
	private String name;
	private Long parentId;
	private Boolean isParent;
	private Integer sort;
}
```



### 2）Mapper

我们使用通用mapper来简化开发：

```java
public interface CategoryMapper extends Mapper<Category> {
}
```

要注意，我们并没有在mapper接口上声明@Mapper注解，那么mybatis如何才能找到接口呢？

我们在启动类上添加一个扫描包功能：

```java
@SpringBootApplication
@EnableDiscoveryClient
@MapperScan("com.leyou.item.mapper") // 扫描mapper包
public class LyItemApplication {
    public static void main(String[] args) {
        SpringApplication.run(LyItemApplication.class, args);
    }
}
```



### 3）Service

一般service层我们会定义接口和实现类，不过这里我们就偷懒一下，直接写实现类了：

```java
/**
 * @author 黑马程序员
 */
@Service
public class CategoryService {

    @Autowired
    private CategoryMapper categoryMapper;

    public List<CategoryDTO> queryListByParent(Long pid) {
        // 查询条件，mapper会把对象中的非空属性作为查询条件
        Category t = new Category();
        t.setParentId(pid);
        List<Category> list = categoryMapper.select(t);
        // 判断结果
        if(CollectionUtils.isEmpty(list)){
            throw new LyException(ExceptionEnum.CATEGORY_NOT_FOND);
        }
        // 使用自定义工具类，把Category集合转为DTO的集合
        return BeanHelper.copyWithCollection(list, CategoryDTO.class);
    }
}
```



### 4）Controller

编写一个controller一般需要知道四个内容：

- 请求方式：决定我们用GetMapping还是PostMapping
- 请求路径：决定映射路径
- 请求参数：决定方法的参数
- 返回值结果：决定方法的返回值

```java
/**
 * 黑马程序员
 */
@RestController
@RequestMapping("category")
public class CategoryController {

    @Autowired
    private CategoryService categoryService;
    /**
     * 根据父节点查询商品类目
     *
     * @param pid
     * @return
     */
    @GetMapping("/of/parent")
    public ResponseEntity<List<CategoryDTO>> queryByParentId(
            @RequestParam(value = "pid", defaultValue = "0") Long pid) {
        return ResponseEntity.ok(this.categoryService.queryListByParent(pid));
    }
}
```





### 5）启动并测试

我们不经过网关，直接访问：

![1575024871135](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575024871135.png) 

然后试试网关是否畅通：

如果网关不通，记得修改下ly-gateway中的application.yml中的routes

![1575024967459](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575024967459.png) 

一切OK！

然后刷新页面查看：

![1526017362418](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1526017362418.png)

发现报错了！

浏览器直接访问没事，但是这里却报错，什么原因？









## 10、跨域问题：什么是跨域及解决方案

### 1）跨域简介

跨域是指跨域名的访问，以下情况都属于跨域：

| 跨域原因说明       | 示例                                   |
| ------------------ | -------------------------------------- |
| 域名不同           | `www.jd.com` 与 `www.taobao.com`       |
| 域名相同，端口不同 | `www.jd.com:8080` 与 `www.jd.com:8081` |
| 二级域名不同       | `item.jd.com` 与 `miaosha.jd.com`      |

如果**域名和端口都相同，但是请求路径不同**，不属于跨域，如：

`www.jd.com/item` 

`www.jd.com/goods`



而我们刚才是从`manage.leyou.com`去访问`api.leyou.com`，这属于二级域名不同，跨域了。





### 2）为什么有跨域

跨域不一定会有跨域问题。

因为跨域问题是浏览器对于ajax请求的一种安全限制：**一个页面发起的ajax请求，只能是于当前页同域名的路径**，这能有效的阻止跨站攻击。

因此：**跨域问题 是针对ajax的一种限制**。

但是这却给我们的开发带来了不便，而且在实际生产环境中，肯定会有很多台服务器之间交互，地址和端口都可能不同，怎么办？





### 3）解决跨域问题方案

目前比较常用的跨域解决方案有3种：

- Jsonp

  最早的解决方案，利用script标签可以跨域的原理实现。

  限制：

  - 需要服务的支持
  - 只能发起GET请求

- nginx反向代理

  思路是：利用nginx反向代理把跨域为不跨域，支持各种请求方式

  缺点：需要在nginx进行额外配置，语义不清晰

- CORS

  规范化的跨域请求解决方案，安全可靠。

  优势：

  - 在服务端进行控制是否允许跨域，可自定义规则
  - 支持各种请求方式

  缺点：

  - 会产生额外的请求

我们这里会采用cors的跨域方案。

















## 11、跨域问题：CORS解决跨域原理

### 1）什么是CORS

CORS是一个W3C标准，全称是"跨域资源共享"（Cross-origin resource sharing）。

它允许浏览器向跨源服务器，发出[`XMLHttpRequest`](http://www.ruanyifeng.com/blog/2012/09/xmlhttprequest_level_2.html)请求，从而克服了AJAX只能[同源](http://www.ruanyifeng.com/blog/2016/04/same-origin-policy.html)使用的限制。

CORS需要浏览器和服务器同时支持。目前，所有浏览器都支持该功能，IE浏览器不能低于IE10。

- 浏览器端：

  目前，所有浏览器都支持该功能（IE10以下不行）。整个CORS通信过程，都是浏览器自动完成，不需要用户参与。

- 服务端：

  CORS通信与AJAX没有任何差别，因此你不需要改变以前的业务逻辑。只不过，浏览器会在请求中携带一些头信息，我们需要以此判断是否允许其跨域，然后在响应头中加入一些信息即可。这一般通过过滤器完成即可。



### 2）实现原理

浏览器会将ajax请求分为两类，其处理方案略有差异：简单请求、特殊请求。

#### 简单请求

只要同时满足以下两大条件，就属于简单请求。：

（1) 请求方法是以下三种方法之一：

- HEAD
- GET
- POST

（2）HTTP的头信息不超出以下几种字段：

- Accept
- Accept-Language
- Content-Language
- Last-Event-ID
- Content-Type：只限于三个值`application/x-www-form-urlencoded`、`multipart/form-data`、`text/plain`



当浏览器发现发现的ajax请求是简单请求时，会在请求头中携带一个字段：`Origin`.

 ![1526019242125](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1526019242125.png)

Origin中会指出当前请求属于哪个域（协议+域名+端口）。服务会根据这个值决定是否允许其跨域。

如果服务器允许跨域，需要在返回的响应头中携带下面信息：

```http
Access-Control-Allow-Origin: http://manage.leyou.com
Access-Control-Allow-Credentials: true
```

- Access-Control-Allow-Origin：可接受的域，是一个具体域名或者*，代表任意
- Access-Control-Allow-Credentials：是否允许携带cookie，默认情况下，cors不会携带cookie，除非这个值是true

注意：

如果跨域请求要想操作cookie，需要满足3个条件：

- 服务的响应头中需要携带Access-Control-Allow-Credentials并且为true。
- 浏览器发起ajax需要指定withCredentials 为true
- 响应头中的Access-Control-Allow-Origin一定不能为*，必须是指定的域名

#### 特殊请求

不符合简单请求的条件，会被浏览器判定为特殊请求,，例如请求方式为PUT。

> 预检请求

特殊请求会在正式通信之前，增加一次HTTP查询请求，称为"预检"请求（preflight）。

浏览器先询问服务器，当前网页所在的域名是否在服务器的许可名单之中，以及可以使用哪些HTTP动词和头信息字段。只有得到肯定答复，浏览器才会发出正式的`XMLHttpRequest`请求，否则就报错。

一个“预检”请求的样板：

```http
OPTIONS /cors HTTP/1.1
Origin: http://manage.leyou.com
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: X-Custom-Header
Host: api.leyou.com
Accept-Language: en-US
Connection: keep-alive
User-Agent: Mozilla/5.0...
```

与简单请求相比，除了Origin以外，多了两个头：

- Access-Control-Request-Method：接下来会用到的请求方式，比如PUT
- Access-Control-Request-Headers：会额外用到的头信息

> 预检请求的响应

服务的收到预检请求，如果许可跨域，会发出响应：

```http
HTTP/1.1 200 OK
Date: Mon, 01 Dec 2008 01:15:39 GMT
Server: Apache/2.0.61 (Unix)
Access-Control-Allow-Origin: http://manage.leyou.com
Access-Control-Allow-Credentials: true
Access-Control-Allow-Methods: GET, POST, PUT
Access-Control-Allow-Headers: X-Custom-Header
Access-Control-Max-Age: 1728000
Content-Type: text/html; charset=utf-8
Content-Encoding: gzip
Content-Length: 0
Keep-Alive: timeout=2, max=100
Connection: Keep-Alive
Content-Type: text/plain
```

除了`Access-Control-Allow-Origin`和`Access-Control-Allow-Credentials`以外，这里又额外多出3个头：

- Access-Control-Allow-Methods：允许访问的方式
- Access-Control-Allow-Headers：允许携带的头
- Access-Control-Max-Age：本次许可的有效时长，单位是秒，**过期之前的ajax请求就无需再次进行预检了**



如果浏览器得到上述响应，则认定为可以跨域，后续就跟简单请求的处理是一样的了。











## 12、跨域问题：解决方式一

第一种解决方案最简单，只需要在controller类上添加注解@CrossOrigin 即可！这个注解其实是CORS的实现。

![1575254948453](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575254948453.png) 

源码解释：

![1575255168139](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575255168139.png) 



Controller代码：

```java
package com.leyou.item.web;

import com.leyou.item.dto.CategoryDTO;
import com.leyou.item.service.CategoryService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/category")
@CrossOrigin
public class CategoryController {

    @Autowired
    private CategoryService categoryService;

    @GetMapping("/of/parent")
    public ResponseEntity<List<CategoryDTO>> queryByParentId(@RequestParam(value="pid",defaultValue = "0") Long pid){
        List<CategoryDTO> categoryDTOS = categoryService.queryListByParent(pid);
        return ResponseEntity.ok(categoryDTOS);
    }

}
```



这种方式需要在每个控制器上加上注解，不推荐这种方式。



















## 13、跨域问题：解决方式二

SpringCloudGateway是Spring官方出品，他已经把各方面想到了，关于这个跨域，官方文档有详细解释：

https://cloud.spring.io/spring-cloud-gateway/reference/html/#cors-configuration

截图如下：

![1575361390402](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1575361390402.png)

我们只需要在application.yml文件中配置一下就可以解决跨域问题：

```yml
spring:
  application:
    name: ly-gateway
  cloud:
    gateway:
      globalcors:
        add-to-simple-url-handler-mapping: true
        corsConfigurations:
          '[/**]':
            allowedOrigins:
              - "http://manage.leyou.com"
            allowedHeaders:
              - "*"
            allowCredentials: true
            maxAge: 360000
            allowedMethods:
              - GET
              - POST
              - DELETE
              - PUT
              - OPTIONS
              - HEAD
      routes:
        # 路由id,可以随意写
        - id: item-service
          # 代理的服务地址；lb表示负载均衡(从eureka中获取具体服务)
          uri: lb://item-service
          # 路由断言，可以配置映射路径
          predicates:
            - Path=/api/item/**
          filters:
            # 表示过滤1个路径，2表示两个路径，以此类推
            - StripPrefix=2
```





























## 14、课程总结



![1578200562600](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1578200562600.png) 

































