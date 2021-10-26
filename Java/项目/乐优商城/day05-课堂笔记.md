## 01、课程目标

- 了解SPU与SKU数据结构设计思路

- 独立实现商品查询

- 独立实现商品新增后台

  

  





























## 02、SPU与SKU：概念说明

SPU：Standard Product Unit （标准产品单位） ，一组具有共同属性的商品集

SKU：Stock Keeping Unit（库存量单位），SPU商品集因具体特性不同而细分的每个商品

以图为例来看： 

![1575542656523](图片二/1575542656523.png) 

- 本页的 华为Mate10 就是一个商品集（SPU）
- 因为颜色、内存等不同，而细分出不同的Mate10，如亮黑色128G版。（SKU）

可以看出：

- SPU是一个抽象的商品集概念，为了方便后台的管理。
- SKU才是具体要销售的商品，每一个SKU的价格、库存可能会不一样，用户购买的是SKU而不是SPU



**小结：**

SPU：==相当于一个Java类==

SKU：==相当于一个Java类下的实例==



public class  User{     类

}



new User()    实例1

new User()    实例2











## 03、SPU与SKU：数据库表结构分析

### 1）思考分析

弄清楚了SPU和SKU的概念区分，接下来我们一起思考一下该如何设计数据库表。

首先来看SPU，大家一起思考下SPU应该有哪些字段来描述？

```
id:主键
title：标题
description：描述
specification：规格
packaging_list：包装
after_service：售后服务
comment：评价
category_id：商品分类
brand_id：品牌
```

似乎并不复杂.

再看下SKU，大家觉得应该有什么字段？

```
id：主键
spu_id：关联的spu
price：价格
images：图片
stock：库存
颜色？
内存？
硬盘？
```

sku的特有属性也是变化的，不同商品，特有属性不一定相同，那么我们的表字段岂不是不确定？

sku的这个特有属性该如何设计呢？



### 2）SKU的特有属性

SPU中会有一些特殊属性，用来区分不同的SKU，我们称为SKU特有属性。如华为META10的颜色、内存属性。

不同种类的商品，一个手机，一个衣服，其SKU属性不相同。

同一种类的商品，比如都是衣服，SKU属性基本是一样的，都是颜色、尺码等。

这样说起来，似乎SKU的特有属性也是与分类相关的？事实上，仔细观察你会发现，**SKU的特有属性是商品规格参数的一部分**：

![1526088981953](图片二/1526088981953.png)



也就是说，我们没必要单独对SKU的特有属性进行设计，它可以看做是规格参数中的一部分。这样规格参数中的属性可以标记成两部分：

- spu下所有sku共享的规格属性（称为**通用属性**）
- spu下每个sku不同的规格属性（称为**特有属性**）

回忆一下之前我们设计的tb_spec_param表，是不是有一个字段，名为generic，标记通用和特有属性。就是为了这里使用。

这样以来，商品SKU表就只需要设计规格属性以外的其它字段了，规格属性由之前的规格参数表`tb_spec_param`来保存。



但是，规格属性的值依然是需要与商品相关联的。









## 04、SPU与SKU：spu与spu_detail表结构分析

### 1）数据库表切分说明

- 横向切分【水平拆分】

  > 一种情况是    数据量太大了【2000万左右】
  >
  > 一种情况是    历史数据使用的几率很低

- 纵向切分【垂直拆分】

  > 一种情况是   表的字段太多了【50个左右】
  >
  > 一种情况是   表中有一些大字段，增删改的效率低【long】，需要将这些大字段单独分离出去



### 2）表结构

#### spu表

```mysql
CREATE TABLE `tb_spu` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'spu id',
  `name` varchar(256) NOT NULL DEFAULT '' COMMENT '商品名称',
  `sub_title` varchar(256) DEFAULT '' COMMENT '副标题，一般是促销信息',
  `cid1` bigint(20) NOT NULL COMMENT '1级类目id',
  `cid2` bigint(20) NOT NULL COMMENT '2级类目id',
  `cid3` bigint(20) NOT NULL COMMENT '3级类目id',
  `brand_id` bigint(20) NOT NULL COMMENT '商品所属品牌id',
  `saleable` tinyint(1) NOT NULL DEFAULT '1' COMMENT '是否上架，0下架，1上架',
  `create_time` timestamp NULL DEFAULT CURRENT_TIMESTAMP COMMENT '添加时间',
  `update_time` timestamp NULL DEFAULT CURRENT_TIMESTAMP COMMENT '最后修改时间',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=183 DEFAULT CHARSET=utf8 COMMENT='spu表，该表描述的是一个抽象性的商品，比如 iphone8';
```



















#### spu_detail表

与我们前面分析的基本类似，但是似乎少了一些字段，比如商品描述。

我们做了表的垂直拆分，将SPU的详情放到了另一张表：tb_spu_detail

```mysql
CREATE TABLE `tb_spu_detail` (
  `spu_id` bigint(20) NOT NULL,
  `description` text COMMENT '商品描述信息',
  `generic_spec` varchar(2048) NOT NULL DEFAULT '' COMMENT '通用规格参数数据',
  `special_spec` varchar(1024) NOT NULL COMMENT '特有规格参数及可选值信息，json格式',
  `packing_list` varchar(1024) DEFAULT '' COMMENT '包装清单',
  `after_service` varchar(1024) DEFAULT '' COMMENT '售后服务',
  `create_time` timestamp NULL DEFAULT CURRENT_TIMESTAMP,
  `update_time` timestamp NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`spu_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

这张表中的数据都比较大，为了不影响主表的查询效率我们拆分出这张表。

需要注意的是这两个字段：generic_spec和special_spec。





### 3）spu中的规格参数：generic_spec字段

前面讲过规格参数与商品分类绑定，同一分类的商品，会有一套相同的规格参数key（规格参数模板），但是这个分类下每个商品的规格参数值都不相同，因此要满足下面几点：

- 我们有一个规格参数表，跟分类关联，保存的就是某分类下的规格参数模板。
- 我们还需要表，跟商品关联，保存某个商品，相关联的规格参数的值。
- 规格参数因为分成了通用规格参数和特有规格参数，因此规格参数值也需要分别于SPU和SKU关联：
  - 通用的规格参数值与SPU关联。
  - 特有规格参数值与SKU关联。

但是我们并没有增加新的表，来看下我们的 表如何存储这些信息：



**generic_spec字段**

如果要设计一张表，来表示spu中的通用规格属性的值，至少需要下面的字段：

```
spu_id：与哪个商品关联
param_id：是商品的哪个规格参数
value：具体的值
```

我们并没有这么设计。而是把与某个商品相关的规格属性值，直接保存到这个商品spu表中，因此这些规格属性关联的商品就一目了然，那么上述3个属性中的`spu_id`就无需保存了，而剩下的就是`param_id`和规格参数值了。两者刚好是一一对应关系，组成一个键值对。我们刚好可以用一个json结构来标示。

是也就是spuDetail表中的`generic_spec`，其中保存通用规格参数信息的值：

> 整体来看：

 ![1529554390912](图片二/1529554390912.png)

json结构，其中都是键值对：

- key：对应的规格参数的`spec_param`的id
- value：对应规格参数的值







### 4）spu中的规格参数：special_spec字段

我们说spu中只保存通用规格参数，那么为什么有多出了一个`special_spec`字段呢？



以手机为例，品牌、操作系统等肯定是通用规格属性，内存、颜色等肯定是特有属性。

当你确定了一个SPU，比如小米的：红米4X，因为颜色内存等不同，会形成多个sku。如果把每个sku的颜色、内存等信息都整理一下，会形成下面的结果：

```
颜色：[香槟金, 樱花粉, 磨砂黑]
内存：[2G, 3G]
机身存储：[16GB, 32GB]
```

也就是说这里把一个spu下的每个sku的特有规格属性值聚合在了一起！这个就是special_spec字段了。

来看数据格式：

 ![1529554916252](图片二/1529554916252.png)

也是json结构：

- key：规格参数id
- value：spu属性的数组



那么问题来：为什么要在spu中把所有sku的规格属性聚合起来保存呢？



因为我们有时候需要把所有规格参数都查询出来，而不是只查询1个sku的属性。比如，商品详情页展示可选的规格参数时：

   ![1526267828817](图片二/1526267828817.png)

刚好符号我们的结构，这样页面渲染就非常方便了。



综上所述，spu与商品规格参数模板的关系如图所示：

![1552792606155](图片二/1552792606155.png)











## 05、SPU与SKU：SKU表结构分析

### 1）表结构

```mysql
CREATE TABLE `tb_sku` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'sku id',
  `spu_id` bigint(20) NOT NULL COMMENT 'spu id',
  `title` varchar(256) NOT NULL COMMENT '商品标题',
  `images` varchar(1024) DEFAULT '' COMMENT '商品的图片，多个图片以‘,’分割',
  `stock` int(8) DEFAULT '9999' COMMENT '库存',
  `price` bigint(16) NOT NULL DEFAULT '0' COMMENT '销售价格，单位为分',
  `indexes` varchar(32) DEFAULT '' COMMENT '特有规格属性在spu属性模板中的对应下标组合',
  `own_spec` varchar(1024) DEFAULT '' COMMENT 'sku的特有规格参数键值对，json格式，反序列化时请使用linkedHashMap，保证有序',
  `enable` tinyint(1) NOT NULL DEFAULT '1' COMMENT '是否有效，0无效，1有效',
  `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '添加时间',
  `update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '最后修改时间',
  PRIMARY KEY (`id`),
  KEY `key_spu_id` (`spu_id`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=27359021554 DEFAULT CHARSET=utf8 COMMENT='sku表,该表表示具体的商品实体,如黑色的 64g的iphone 8';
```



特别需要注意的是sku表中的`indexes`字段和`own_spec`字段。sku中应该保存特有规格参数的值，就在这两个字段中。



### 2）sku中的特有规格参数：indexes字段

在SPU表中，已经对特有规格参数及可选项进行了保存，结构如下：



```json
{
    "4": [
        "香槟金",
        "樱花粉",
        "磨砂黑"
    ],
    "12": [
        "2GB",
        "3GB"
    ],
    "13": [
        "16GB",
        "32GB"
    ]
}
```

这些特有属性如果排列组合，会产生12个不同的SKU，而不同的SKU，其属性就是上面备选项中的一个。

比如：

- 红米4X，香槟金，2GB内存，16GB存储
- 红米4X，磨砂黑，2GB内存，32GB存储

你会发现，每一个属性值，对应于SPUoptions数组的一个选项，如果我们记录下角标，就是这样：

- 红米4X，0,0,0
- 红米4X，2,0,1

既然如此，我们是不是可以将不同角标串联起来，作为SPU下不同SKU的标示。这就是我们的indexes字段。

 ![1526266901335](图片二/1526266901335.png)

这个设计在商品详情页会特别有用：

 ![1526267180997](图片二/1526267180997.png)

当用户点击选中一个特有属性，你就能根据 角标快速定位到sku。





### 3）sku中的特有规格参数：own_spec字段

看结构：

```json
{"4":"香槟金","12":"2GB","13":"16GB"}
```

保存的是特有属性的键值对。

SPU中保存的是可选项，但不确定具体的值，而SKU中的保存的就是具体的值。



















## 06、SPU与SKU：导入图片信息

现在商品表中虽然有数据，但是所有的图片信息都是无法访问的，看看数据库中给出的图片信息：

![1575543538260](图片二/1575543538260.png) 

现在这些图片的地址指向的是`http://image.leyou.com`这个域名。而之前我们在nginx中做了反向代理，把图片地址指向了阿里巴巴的OSS服务。

我们可以再课前资料中找到这些商品的图片：

![1575543648045](图片二/1575543648045.png) 

并且把这些图片放到nginx目录，然后由nginx对其加载即可。

首先，我们需要把图片放到nginx/html目录，并且解压缩：

![1575543769978](图片二/1575543769978.png) 

修改Nginx中，有关`image.leyou.com`的监听配置，使nginx来加载图片地址：

```nginx
server {
	listen       80;
	server_name  image.leyou.com;
	location /images { # 监听以/images打头的路径，
		root	html;
	}
	location / {
		proxy_pass   https://ly-images.oss-cn-shanghai.aliyuncs.com;
	}
}
```

再次测试：

![1575543999864](图片二/1575543999864.png) 



ok，商品图片都搞定了！



















## 07、商品查询：商品对象与表的关系

商品对象所需表：商品对象对应数据库中多张表

![1575601382376](图片二/1575601382376.png)



商品对象比较复杂，需要多张表组合起来，开发中，复杂的业务是会这样的，大家一定要捋清晰这些表的关系。

商品对象的创建需要六张表的数据。















## 08、商品查询：商品列表数据来源分析

接下来，我们实现商品管理的页面，先看下我们要实现的效果：

![1575602076461](图片二/1575602076461.png) 

可以看出整体是一个table，然后有新增按钮。是不是跟之前写品牌管理很像？

点击页面菜单中的商品列表页面：

![1575603090588](图片二/1575603090588.png) 

页面右侧是一个空白的数据表格：

![1552814807340](图片二/1552814807340.png)

看到浏览器发起已经发起了查询商品数据的请求：

 ![1528082212618](图片二/1528082212618.png)

因此接下来，我们编写接口即可。



那么这些列表的数据分别来自那些表？

![1575602926575](图片二/1575602926575.png) 



列表中我们只能先展示这几个数据，商品表我们之前分析了来源于六张表，那在哪里展示呢？

我们可以在商品详情页去展示。











## 09、商品查询：商品模块准备工作

### 1）SPU的POJO

```java
@Table(name = "tb_spu")
@Data
public class Spu {
    @Id
    @KeySql(useGeneratedKeys = true)
    private Long id;
    private Long brandId;
    private Long cid1;// 1级类目
    private Long cid2;// 2级类目
    private Long cid3;// 3级类目
    private String name;// 商品名称
    private String subTitle;// 子标题
    private Boolean saleable;// 是否上架
    private Date createTime;// 创建时间
    private Date updateTime;// 最后修改时间
}
```



### 2）SPU详情的POJO

```java
@Data
@Table(name="tb_spu_detail")
public class SpuDetail {
    @Id
    private Long spuId;// 对应的SPU的id
    private String description;// 商品描述
    private String specialSpec;// 商品特殊规格的名称及可选值模板
    private String genericSpec;// 商品的全局规格属性
    private String packingList;// 包装清单
    private String afterService;// 售后服务
    private Date createTime;// 创建时间
    private Date updateTime;// 最后修改时间
}
```



### 3）SPU的DTO

先分析：

- 请求方式：GET

- 请求路径：/spu/page

- 请求参数：

  - page：当前页
  - rows：每页大小
  - key：过滤条件
  - saleable：上架或下架

- 返回结果：通过页面能看出来，查询要展示的数据都是SPU数据，而且因为是分页查询，我们可以返回与之前品牌查询一样的PageResult。

  - 要注意，页面展示的中需要是商品分类和品牌名称，而SPU表中中保存的是id，我们需要在DTO中处理这些字段。

    我们可以对Spu拓展categoryName和brandName属性：

```java
/**
 * @author 黑马程序员
 */
@Data
public class SpuDTO {
    private Long id;
    private Long brandId;
    private Long cid1;// 1级类目
    private Long cid2;// 2级类目
    private Long cid3;// 3级类目
    private String name;// 名称
    private String subTitle;// 子标题
    private Boolean saleable;// 是否上架
    private Date createTime;// 创建时间
    private String categoryName; // 商品分类名称拼接
    private String brandName;// 品牌名称

    /**
     * 方便同时获取3级分类
     * @return
     */
    @JsonIgnore
    public List<Long> getCategoryIds(){
        return Arrays.asList(cid1, cid2, cid3);
    }
}
```

需要注意的是，这里通过`@JsonIgnore`注解来忽略这个`getCategoryIds`方法，避免将其序列化到JSON结果

因此这里需要在`ly-item-dto`中引入Jackson依赖：

```xml
<dependencies>
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-annotations</artifactId>
        <version>2.9.0</version>
        <scope>compile</scope>
    </dependency>
</dependencies>
```



### 5）SPU的Mapper

```java
public interface SpuMapper extends Mapper<Spu> {
}
```



### 6）SpuDetail的Mapper

```java
public interface SpuDetailMapper extends Mapper<SpuDetail> {
}
```



### 7）Goods商品的Service

```java
@Service
@Transactional
public class GoodsService {

    @Autowired
    private CategoryService categoryService;

    @Autowired
    private BrandService brandService;

    @Autowired
    private SpuMapper spuMapper;

    @Autowired
    private SpuDetailMapper spuDetailMapper;


}
```



### 8）Goods商品的Controller

```java
@RestController
@RequestMapping("/spu")
public class GoodsController {

    @Autowired
    private GoodsService goodsService;

    
}
```







## 10、商品查询：商品查询的基本逻辑

### 1）GoodsController添加方法

先分析：

- 请求方式：GET
- 请求路径：/spu/page
- 请求参数：
  - page：当前页
  - rows：每页大小
  - key：过滤条件
  - saleable：上架或下架
- 返回结果：PageResult

```java
/**
 * 分页查询spu
 * @param key       查询条件
 * @param page      当前页
 * @param rows      页大小
 * @param saleable  上下架
 * @return
 */
@GetMapping("/page")
public ResponseEntity<PageResult<SpuDTO>> querySpuByPage(
        @RequestParam(value="key",required = false) String key,
        @RequestParam(value="page", defaultValue = "1") Integer page,
        @RequestParam(value="rows", defaultValue = "5") Integer rows,
        @RequestParam(value="saleable", required = false) Boolean saleable
){
    return ResponseEntity.ok(goodsService.querySpuByPage(key,page,rows,saleable));
}
```



### 2）GoodsService添加方法

```java
public PageResult<SpuDTO> querySpuByPage(String key, Integer page, Integer rows, Boolean saleable) {
    //1、分页
    PageHelper.startPage(page, rows);
    //2、过滤
    Example example = new Example(Spu.class);
    Example.Criteria criteria = example.createCriteria();
    //2.1 关键字过滤
    if(StringUtils.isNotBlank(key)){
        criteria.andLike("name", "%" + key + "%");
    }
    //2.2 上下架判断
    if(saleable != null){
        criteria.andEqualTo("saleable", saleable);
    }
    //3、排序
    example.setOrderByClause("update_time DESC");
    //4、搜索结果
    List<Spu> list = spuMapper.selectByExample(example);
    if(CollectionUtils.isEmpty(list)){
        throw new LyException(ExceptionEnum.GOODS_NOT_FOUND);
    }
    //5、封装结果
    //5.1 分页结果
    PageInfo<Spu> info = new PageInfo<>(list);
    //5.2 转换DTO
    List<SpuDTO> spuDTOS = BeanHelper.copyWithCollection(list, SpuDTO.class);
    //返回结果
    return new PageResult<>(info.getTotal(), spuDTOS);
}
```











## 11、商品查询：处理品牌名称和分类名称

### 1）处理品牌名称

页面列表显示的是品牌的名称，但是我们返回的spu只有品牌的id，所以我们还需要对返回的数据进一步处理。

![1575623567305](图片二/1575623567305.png) 

我们在GoodsService中添加一个私有方法，对品牌名称和分类名称进行处理：

在querySpuByPage方法中去引用

![1575623688080](图片二/1575623688080.png)

处理的方法：

```java
//处理分类和品牌名称
private void handleCategoryAndBrandName(List<SpuDTO> spuDTOS) {
    for (SpuDTO spuDTO : spuDTOS) {
        //查询分类名称
        spuDTO.setCategoryName("");
        //查询品牌名称
        Long brandId = spuDTO.getBrandId();
        BrandDTO brandDTO = brandService.queryById(brandId);
        spuDTO.setBrandName(brandDTO.getName());
    }
}
```

这个方法调用了品牌业务层代码，所以我们需要在业务层添加一个方法：

```java
@Service
@Transactional
public class BrandService {
    @Autowired
    private BrandMapper brandMapper;

    public void saveBrand(BrandDTO brandDTO, List<Long> ids) {
        .......
    }
    public PageResult<BrandDTO> brandPageQuery(String key, Integer page, Integer rows, String sortBy, Boolean desc) {
        .......
    }
    //根据id查询品牌
    public BrandDTO queryById(Long id){
        Brand brand = brandMapper.selectByPrimaryKey(id);
        if(brand == null){
            throw new LyException(ExceptionEnum.BRAND_NOT_FOUND);
        }
        return BeanHelper.copyProperties(brand, BrandDTO.class);
    }
}
```







### 2）处理分类名称

好了，品牌的名称处理完了，分类的名称我们也要处理一下，页面的效果我们之前看过，效果如下：

![1575623957274](图片二/1575623957274.png) 

在我们的dto中只有这几个分类的id集合，但是页面需要的是名称的集合，所以我们需要根据id集合去查询到分类的名称，那么我们的mapper需要根据id集合去查询分类名称，但是我们的mapper没有根据id集合去查询对象集合的方法，所以还需要扩展我们的mapper。



我们先改造CategoryMapper接口，我们让这个接口继承多一个mapper接口，如下：

```java
public interface CategoryMapper extends Mapper<Category>, IdListMapper<Category,Long> {
}
```



扩展完成mapper之后，我们在CategoryService中需要添加一个方法去批量查询分类：

```java
public List<CategoryDTO> queryByIds(List<Long> categoryIds) {
    //查询分类
    List<Category> list = categoryMapper.selectByIdList(categoryIds);
    //判断结果
    if (CollectionUtils.isEmpty(list)) {
        throw new LyException(ExceptionEnum.CATEGORY_NOT_FOUND);
    }
    //使用工具类把Category集合转为dto集合
    List<CategoryDTO> categoryDTOS = BeanHelper.copyWithCollection(list, CategoryDTO.class);
    return categoryDTOS;
}
```



然后继续改造**GoodsService**的==handleCategoryAndBrandName==方法

```java
//处理分类和品牌名称
private void handleCategoryAndBrandName(List<SpuDTO> spuDTOS) {
    for (SpuDTO spuDTO : spuDTOS) {
        //查询分类名称
        List<Long> categoryIds = spuDTO.getCategoryIds();
        String name = categoryService.queryByIds(categoryIds)
                .stream().map(CategoryDTO::getName)
                .collect(Collectors.joining("/"));
        spuDTO.setCategoryName(name);
        //查询品牌名称
        Long brandId = spuDTO.getBrandId();
        BrandDTO brandDTO = brandService.queryById(brandId);
        spuDTO.setBrandName(brandDTO.getName());
    }
}
```



编写完成后，重启微服务，然后打开页面测试：

![1575624468429](图片二/1575624468429.png) 







## 12、商品查询：拓展通用Mapper

需要注意的是，上面的categoryMapper调用了一个`selectByIdList`的方法，而这个方法在我们学过的通用Mapper中是没有的。我们的方法都是继承自`Mapper<T>`

事实上，除了`Mapper<T>`接口以为，通用mapper还提供了很多其它XxxMapper接口供我们使用：

![1575626360816](图片二/1575626360816.png) 

所以， 只要我们的CategoryMapper继承了这些接口，那么就具备了他们的功能。

不过呢，不仅仅是CategoryMapper有这样的需求，以后的其它Mapper也有这样的需求，我们可以自定义一个BaseMapper接口，在里面把需要的其它接口都继承过来。以后再编写Mapper只要继承BaseMapper即可。



我们在ly-common中定义一个这样的Mapper：

```java
package com.leyou.common.mapper;

import tk.mybatis.mapper.additional.idlist.IdListMapper;
import tk.mybatis.mapper.annotation.RegisterMapper;
import tk.mybatis.mapper.common.IdsMapper;
import tk.mybatis.mapper.common.Mapper;
import tk.mybatis.mapper.common.special.InsertListMapper;

/**
 * @author 黑马程序员
 */
@RegisterMapper
public interface BaseMapper<T> extends Mapper<T>, IdListMapper<T, Long>, IdsMapper<T>, InsertListMapper<T> {
}
```

需要额外引入通用mapper依赖：

```xml
<dependency>
    <groupId>tk.mybatis</groupId>
    <artifactId>mapper-base</artifactId>
    <version>${mapper.version}</version>
</dependency>
<dependency>
    <groupId>tk.mybatis</groupId>
    <artifactId>mapper-core</artifactId>
    <version>${mapper.version}</version>
</dependency>
<dependency>
    <groupId>tk.mybatis</groupId>
    <artifactId>mapper-extra</artifactId>
    <version>${mapper.version}</version>
</dependency>
```

可以再leyou父工程中定义mapper的版本信息：

```xml
<properties>
    <mapper.version>1.1.5</mapper.version>
</properties>
```

然后让CategoryMapper继承我们的BaseMapper即可：

```java
package com.leyou.item.mapper;

import com.leyou.common.mapper.BaseMapper;
import com.leyou.item.entity.Category;
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.annotations.Select;

import java.util.List;

/**
 * @author 黑马程序员
 */
public interface CategoryMapper extends BaseMapper<Category> {
    // ... 略
}
```



以后我们的Mapper就直接继承BaseMapper就好了。











## 13、商品保存：业务分析

### 1）页面预览

当我们点击新增商品按钮：

![1528083727447](图片二/1528083727447.png) 

就会出现一个弹窗：

![1528086595597](图片二/1528086595597.png) 

里面把商品的数据分为了4部分来填写：

- 基本信息：主要是一些简单的文本数据，包含了SPU和SpuDetail的部分数据，如
  - 商品分类：是SPU中的`cid1`，`cid2`，`cid3`属性
  - 品牌：是spu中的`brandId`属性
  - 标题：是spu中的`title`属性
  - 子标题：是spu中的`subTitle`属性
  - 售后服务：是SpuDetail中的`afterService`属性
  - 包装列表：是SpuDetail中的`packingList`属性
- 商品描述：是SpuDetail中的`description`属性，数据较多，所以单独放一个页面
- 规格参数：商品规格信息，对应SpuDetail中的`genericSpec`属性
- SKU属性：spu下的所有Sku信息

也就是说这个页面包含了商品相关的四张表中的数据：

- tb_spu
- tb_spu_detail
- tb_sku



### 2）商品分类

商品分类的级联选框我们之前在品牌查询已经做过，是要根据分类的pid查询分类，所以这里的级联选框已经实现完成：

刷新页面，可以看到请求已经发出：

 ![1528101483023](图片二/1528101483023.png)

 ![1528101622952](图片二/1528101622952.png)



 ![1528101649596](图片二/1528101649596.png)

需要注意的是，这里选中以后会显示3级分类，因为数据库中保存的就是商品的1~3级类目。

### 3）品牌选择

品牌也是一个下拉选框，不过其选项是不确定的，只有当用户选择了商品分类，才会把这个分类下的所有品牌展示出来。

所以页面编写了watch函数，监控商品分类的变化，每当商品分类值有变化，就会发起请求，查询品牌列表。刷新页面，当选中一个分类时，可以看到请求发起：

![1552822964914](图片二/1552822964914.png)

接下来，我们只要编写后台接口，根据商品分类id，查询对应品牌即可。





## 14、商品保存：基本信息及品牌查询

页面需要去后台查询品牌信息，我们自然需要提供：

### 1）controller

```java
/**
 * 根据分类查询品牌
 * @param categoryId 商品分类id
 * @return 该分类下的品牌的集合
 */
@GetMapping("/of/category")
public ResponseEntity<List<BrandDTO>> queryBrandByCategoryId(@RequestParam("id") Long categoryId) {
    return ResponseEntity.ok(this.brandService.queryBrandByCategoryId(categoryId));
}
```

### 2）service

```java
public List<BrandDTO> queryBrandByCategoryId(Long categoryId) {
    List<Brand> list = brandMapper.queryByCategoryId(categoryId);
    // 判断是否为空
    if(CollectionUtils.isEmpty(list)){
        throw new LyException(ExceptionEnum.BRAND_NOT_FOUND);
    }
    return BeanHelper.copyWithCollection(list, BrandDTO.class);
}
```



### 3）mapper

根据分类查询品牌有中间表，需要自己编写Sql：

```java
@Select("SELECT b.id, b.name, b.letter, b.image FROM tb_category_brand cb INNER JOIN tb_brand b ON b.id = cb.brand_id WHERE cb.category_id = #{cid}")
List<Brand> queryByCategoryId(@Param("cid") Long cid);
```



效果：

 ![1528102194745](图片二/1528102194745.png)









## 15、商品保存：商品描述及富文本编辑器

### 1）富文本编辑器介绍

标题、子标题等都是普通文本框，我们直接填写即可，没有需要特别注意的，商品描述信息比较复杂，而且图文并茂，甚至包括视频。

这样的内容，一般都会使用富文本编辑器。

**==什么是富文本编辑器？==**

百度百科：

![1526290914491](图片二/1526290914491.png)

通俗来说：富文本，就是比较丰富的文本编辑器。普通的框只能输入文字，而富文本还能给文字加颜色样式等。

富文本编辑器有很多，例如：KindEditor、Ueditor。但并不原生支持vue

但是我们今天要说的，是一款支持Vue的富文本编辑器：`vue-quill-editor`



### 2）Vue-Quill-Editor介绍

GitHub的主页：https://github.com/surmon-china/vue-quill-editor

Vue-Quill-Editor是一个基于Quill的富文本编辑器：[Quill的官网](https://quilljs.com/)

![1526291232678](图片二/1526291232678.png)



### 3）Vue-Quill-Editor使用指南

使用非常简单：

第一步：安装，使用npm命令：

```
npm install vue-quill-editor --save
```

第二步：加载，在js中引入：

全局使用：

```js
import Vue from 'vue'
import VueQuillEditor from 'vue-quill-editor'

const options = {}; /* { default global options } */

Vue.use(VueQuillEditor, options); // options可选
```



局部使用：

```js
import 'quill/dist/quill.core.css'
import 'quill/dist/quill.snow.css'
import 'quill/dist/quill.bubble.css'

import {quillEditor} from 'vue-quill-editor'

var vm = new Vue({
    components:{
        quillEditor
    }
})
```



第三步：页面引用：

```html
<quill-editor v-model="goods.spuDetail.description" :options="editorOption"/>
```

### 4）自定义富文本编辑器

不过这个组件有个小问题，就是图片上传的无法直接上传到后台，因此我们对其进行了封装，支持了图片的上传。

 ![1526296083605.png](图片二/1526296083605.png)

使用也非常简单：

```html
<v-stepper-content step="2">
    <v-editor v-model="goods.spuDetail.description" url="/upload/signature" needSignature/>
</v-stepper-content>
```

- url：是图片上传的路径或者上传阿里OSS时的签名路径，这里输入的是签名路径
- v-model：双向绑定，将富文本编辑器的内容绑定到goods.spuDetail.description

### 5）效果

![1526297276667](图片二/1526297276667.png) 







## 16、商品保存：规格参数与SKU属性

### 1）规格参数

规格参数的查询我们之前也已经编写过接口，因为商品规格参数也是与商品分类绑定，所以需要在商品分类变化后去查询，我们也是通过watch监控来实现：

 页面如下：

 ![1529631887407](图片二/1529631887407.png) 



### 2）SKU属性

Sku属性是SPU下的每个商品的不同特征，如图：

![1529656674978](图片二/1529656674978.png) 

当我们填写一些属性后，会在页面下方生成一个sku表格，大家可以计算下会生成多少个不同属性的Sku呢？

当你选择了上图中的这些选项时：

- 颜色共2种：夜空黑，绚丽红
- 内存共2种：4GB，6GB
- 机身存储1种：64GB

此时会产生多少种SKU呢？ 应该是 2 * 2 * 1 = 4种，这其实就是在求笛卡尔积。

我们会在页面下方生成一个sku的表格：

![1528856353718](图片二/1528856353718.png)

这个表格中就包含了以上颜色内存的所有可能组合，剩下的价格等信息就需要用户自己来完成了。

注意，页面中的sku的图片上传，默认是上传到本地，后面我们改为上传到阿里云。

如果是上传到本地，则需要在nginx中配置图片地址的判断：

```nginx
server {
	listen       80;
	server_name  image.leyou.com;
	location /images {
		root	html;
	}
	location / {
		root 	html;
		if (!-f $request_filename) {
            proxy_pass http://ly-upload-server.oss-cn-shenzhen.aliyuncs.com;
            break;
        }
	}
}
```

- `if (!-f $request_filename)`：判断是否在本地能找到图片，找不到才反向代理到阿里去找图片



**如果不想修改nginx**，也可以修改成上传到阿里，这样nginx就无需修改了：

![1557134693791](图片二/1557134693791.png)

大概是在119行。











## 17、商品保存：保存商品准备工作

### 1）页面请求

在sku列表的下方，有一个提交按钮：

![1575789664636](图片二/1575789664636.png) 

点击提交，查看控制台提交的数据格式：

![1575789550106](图片二/1575789550106.png) 

整体是一个json格式数据，包含Spu表所有数据：

- brandId：品牌id

- cid1、cid2、cid3：商品分类id

- subTitle：副标题

- name：商品名称

- spuDetail：是一个json对象，代表商品详情表数据

  ![1552827536439](图片二/1552827536439.png)

  - afterService：售后服务
  - description：商品描述
  - packingList：包装列表
  - specialSpec：sku规格属性模板
  - genericSpec：通用规格参数

- skus：spu下的所有sku数组，元素是每个sku对象：

  ![1552827560584](图片二/1552827560584.png)

  - title：标题
  - images：图片
  - price：价格
  - stock：库存
  - ownSpec：特有规格参数
  - indexes：特有规格参数的下标



### 2）Sku的POJO

```java
package com.leyou.item.entity;

import lombok.Data;
import tk.mybatis.mapper.annotation.KeySql;

import javax.persistence.Id;
import javax.persistence.Table;
import java.util.Date;

@Data
@Table(name = "tb_sku")
public class Sku {
    @Id
    @KeySql(useGeneratedKeys=true)
    private Long id;
    private Long spuId;
    private String title;
    private String images;
    private Long price;
    private Integer stock;
    private String ownSpec;// 商品特殊规格的键值对
    private String indexes;// 商品特殊规格的下标
    private Boolean enable;// 是否有效，逻辑删除用
    private Date createTime;// 创建时间
    private Date updateTime;// 最后修改时间
}
```



### 3）Sku的DTO

```java
package com.leyou.item.dto;

import lombok.Data;

@Data
public class SkuDTO {
    private Long id;
    private Long spuId;
    private String title;
    private String images;
    private Long price;
    private String ownSpec;// 商品特殊规格的键值对
    private String indexes;// 商品特殊规格的下标
    private Boolean enable;// 是否有效，逻辑删除用
    private Integer stock; // 库存
}
```



### 4）SpuDetail的DTO

```java
/**
 * @author 黑马程序员
 */
@Data
public class SpuDetailDTO {
    private Long spuId;// 对应的SPU的id
    private String description;// 商品描述
    private String specialSpec;// 商品特殊规格的名称及可选值模板
    private String genericSpec;// 商品的全局规格属性
    private String packingList;// 包装清单
    private String afterService;// 售后服务
}
```



### 5）修改SpuDTO对象

修改SpuDTO，在里面包含SpuDetail和Sku的集合

```java
package com.leyou.item.dto;

import lombok.Data;

import java.util.Arrays;
import java.util.Date;
import java.util.List;

/**
 * @author 黑马程序员
 */
@Data
public class SpuDTO {
	// 之前属性略 ...
    /**
     * 商品详情
     */
    private SpuDetailDTO spuDetail;
    /**
     * spu下的sku的集合
     */
    private List<SkuDTO> skus;
}
```



### 6）Sku的Mapper

```java
public interface SkuMapper extends BaseMapper<Sku> {
}
```



### 7）在GoodsService注入Sku的mapper

![1575790550140](图片二/1575790550140.png) 



准备工作我们就做到这里，接下来就开始写业务代码了！











## 18、商品保存：商品保存操作

### 1）Controller

完整的url是：`http://api.leyou.com/api/item/goods` 我们在GoodsController下添加如下处理器：

```java
@PostMapping("/goods")
public ResponseEntity<Void> saveGoods(@RequestBody SpuDTO spuDTO){
    goodsService.saveGoods(spuDTO);
    return ResponseEntity.status(HttpStatus.CREATED).build();
}
```



### 2）Service

```java
/**
 * 保存商品信息
 * @param spuDTO
 */
public void saveGoods(SpuDTO spuDTO) {
    //保存Spu
    //将dto转成pojo
    Spu spu = BeanHelper.copyProperties(spuDTO, Spu.class);
    spu.setSaleable(false);//新保存的商品都是下架操作
    int count = spuMapper.insertSelective(spu);
    if (count != 1) {
        throw new LyException(ExceptionEnum.INSERT_OPERATION_FAIL);
    }
    //保存SpuDetail
    SpuDetailDTO spuDetailDTO = spuDTO.getSpuDetail();
    SpuDetail spuDetail = BeanHelper.copyProperties(spuDetailDTO, SpuDetail.class);
    spuDetail.setSpuId(spu.getId());//设置外键
    count = spuDetailMapper.insertSelective(spuDetail);
    if (count != 1) {
        throw new LyException(ExceptionEnum.INSERT_OPERATION_FAIL);
    }
    //保存Sku集合
    List<SkuDTO> skuDTOS = spuDTO.getSkus();
    List<Sku> skus = BeanHelper.copyWithCollection(skuDTOS, Sku.class);
    Date now = new Date();
    skus.forEach(sku -> {
        sku.setSpuId(spu.getId());
        sku.setCreateTime(now);//创建时间
        sku.setUpdateTime(now);//更新时间
    });
    count = skuMapper.insertList(skus);
    if (count != skus.size()) {
        throw new LyException(ExceptionEnum.INSERT_OPERATION_FAIL);
    }
}
```



### 3）测试

点击保存，顶部弹窗保存成功，再查看列表就有我们新增的数据了！

![1575793649105](图片二/1575793649105.png) 







































