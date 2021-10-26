## 01、课程目标

- 独立实现基本搜索
- 独立实现页面分页
- 独立实现结果排序



































## 02、商品搜索：构建Goods对象-基本思路

导入数据其实就是查询数据，然后把查询到的Spu转变为Goods来保存，因此我们先编写一个SearchService，然后在里面定义一个方法， 把Spu转为Goods。

只要能把Goods创建出来，那么我们就可以插入索引库了，在第六天的时候我们设计的Goods对象如下：

![1578836762826](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1578836762826.png) 

转成json结构之后，如下图：

![1578838033198](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1578838033198.png) 







在编写这个方法的时候，我们要一步一步来，看看那些数据已经有，那些数据需要调用接口去获取，这个我们要有思路：

```java
@Service
public class SearchService {

    @Autowired
    private ItemClient itemClient;

    /**
     * 把一个spu转为一个Goods对象
     * @param spu
     * @return
     */
    public Goods buildGoods(SpuDTO spu){

        //1、spu下所有的sku

        //2、sku的价格集合

        //3、搜索字段，包含名称、分类、品牌、等

        //4、规格参数，包含key和value

        //5、准备Goods对象
        Goods goods = new Goods();
        goods.setId(spu.getId()); //spuId
        goods.setBrandId(spu.getBrandId());//品牌id
        goods.setCategoryId(spu.getCid3());//分类id
        goods.setSubTitle(spu.getSubTitle());//副标题
        goods.setCreateTime(spu.getCreateTime().getTime());//创建时间
        goods.setSkus(null);//TODO spu下的所有sku
        goods.setSpecs(null);// TODO 规格参数，包含key和value
        goods.setPrice(null);//TODO sku价格的集合
        goods.setAll(null);//TODO 搜索字段，包含名称、分类、品牌等
        //返回Goods对象
        return goods;
    }
}
```



好了，架子我们搭好了，接下来，就一步一步把TODO标记需要的内容填充就好！













## 03、商品搜索：构建Goods对象-封装sku

在上一步中，我们标出了Goods的四个属性还没有值，接下来我们一个一个来完成，我们先处理sku

```java
@Service
public class SearchService {

    @Autowired
    private ItemClient itemClient;

    /**
     * 把一个spu转为一个Goods对象
     * @param spu
     * @return
     */
    public Goods buildGoods(SpuDTO spu){
        Long spuId = spu.getId();
        //1、spu下所有的sku
        List<SkuDTO> skuDTOList = itemClient.findSkuListBySpuId(spuId);
        List<Map<String, Object>> skuList = new ArrayList<>();
        skuDTOList.forEach(skuDTO -> {
            Map<String, Object> sku = new HashMap<>();
            sku.put("id", skuDTO.getId());
            sku.put("image", StringUtils.substringBefore(skuDTO.getImages(),","));
            sku.put("price", skuDTO.getPrice());
            sku.put("title", skuDTO.getTitle());
            skuList.add(sku);
        });
        //2、sku的价格集合

        //3、搜索字段，包含名称、分类、品牌、等

        //4、规格参数，包含key和value

        //5、准备Goods对象
        Goods goods = new Goods();
        goods.setId(spu.getId()); //spuId
        goods.setBrandId(spu.getBrandId());//品牌id
        goods.setCategoryId(spu.getCid3());//分类id
        goods.setSubTitle(spu.getSubTitle());//副标题
        goods.setCreateTime(spu.getCreateTime().getTime());//创建时间
        goods.setSkus(JsonUtils.toString(skuList));//spu下的所有sku
        goods.setSpecs(null);// TODO 规格参数，包含key和value
        goods.setPrice(null);//TODO sku价格的集合
        goods.setAll(null);//TODO 搜索字段，包含名称、分类、品牌等
        //返回Goods对象
        return goods;
    }
}
```







## 04、商品搜索：构建Goods对象-封装price和all字段

### 1）封装price

接下来处理价格集合，这个集合我们可以使用流来处理

```java
@Service
public class SearchService {

    @Autowired
    private ItemClient itemClient;

    public Goods buildGoods(SpuDTO spu){
       ......
        //2、sku的价格集合
        Set<Long> price = skuDTOList.stream().map(SkuDTO::getPrice).collect(Collectors.toSet());
        ......
        goods.setPrice(price);//sku价格的集合
        .....
    }
}
```



### 2）封装all字段

接下来封装all字段，这个字段封装的是后面要检索的全部内容，我们能搜索的内容都添加到这个字段中

```java
@Service
public class SearchService {

    @Autowired
    private ItemClient itemClient;

    /**
     * 把一个spu转为一个Goods对象
     * @param spu
     * @return
     */
    public Goods buildGoods(SpuDTO spu){
        Long spuId = spu.getId();
        //1、spu下所有的sku
        List<SkuDTO> skuDTOList = itemClient.findSkuListBySpuId(spuId);
        List<Map<String, Object>> skuList = new ArrayList<>();
        skuDTOList.forEach(skuDTO -> {
            Map<String, Object> sku = new HashMap<>();
            sku.put("id", skuDTO.getId());
            sku.put("image", StringUtils.substringBefore(skuDTO.getImages(),","));
            sku.put("price", skuDTO.getPrice());
            sku.put("title", skuDTO.getTitle());
            skuList.add(sku);
        });
        //2、sku的价格集合
        Set<Long> price = skuDTOList.stream().map(SkuDTO::getPrice).collect(Collectors.toSet());
        //3、搜索字段，包含名称、分类、品牌、等
        //3.1 查询分类名称
        List<CategoryDTO> categoryList = itemClient.queryByIds(spu.getCategoryIds());
        String categoryName = categoryList.stream().map(CategoryDTO::getName).collect(Collectors.joining());
        //3.2 查询品牌名称
        BrandDTO brandDTO = itemClient.queryById(spu.getBrandId());
        String all = spu.getName()+categoryName+brandDTO.getName();
        //4、规格参数，包含key和value

        //5、准备Goods对象
        Goods goods = new Goods();
        goods.setId(spu.getId()); //spuId
        goods.setBrandId(spu.getBrandId());//品牌id
        goods.setCategoryId(spu.getCid3());//分类id
        goods.setSubTitle(spu.getSubTitle());//副标题
        goods.setCreateTime(spu.getCreateTime().getTime());//创建时间
        goods.setSkus(JsonUtils.toString(skuList));//spu下的所有sku
        goods.setSpecs(null);// TODO 规格参数，包含key和value
        goods.setPrice(price);//sku价格的集合
        goods.setAll(all);//搜索字段，包含名称、分类、品牌等
        //返回Goods对象
        return goods;
    }
}
```







## 05、商品搜索：构建Goods对象-封装specs字段

接下来封装specs字段，这个是规格参数，Goods中的规格参数是一个map，map有key有value，key是规格参数的名称，value的来源有通用规格参数和特有规格参数，通用规格参数和特有规格参数都来自SpuDetail的两个字段，这两个字段中存储的都是JSON字符串，我们需要通过规格参数id去获取规格值，所以需要先把json字符串转成map，然后再去获取值即可。

```java
@Service
public class SearchService {

    @Autowired
    private ItemClient itemClient;

    /**
     * 把一个spu转为一个Goods对象
     * @param spu
     * @return
     */
    public Goods buildGoods(SpuDTO spu){
        Long spuId = spu.getId();
        //1、spu下所有的sku
        List<SkuDTO> skuDTOList = itemClient.findSkuListBySpuId(spuId);
        List<Map<String, Object>> skuList = new ArrayList<>();
        skuDTOList.forEach(skuDTO -> {
            Map<String, Object> sku = new HashMap<>();
            sku.put("id", skuDTO.getId());
            sku.put("image", StringUtils.substringBefore(skuDTO.getImages(),","));
            sku.put("price", skuDTO.getPrice());
            sku.put("title", skuDTO.getTitle());
            skuList.add(sku);
        });
        //2、sku的价格集合
        Set<Long> price = skuDTOList.stream().map(SkuDTO::getPrice).collect(Collectors.toSet());
        //3、搜索字段，包含名称、分类、品牌、等
        //3.1 查询分类名称
        List<CategoryDTO> categoryList = itemClient.queryByIds(spu.getCategoryIds());
        String categoryName = categoryList.stream().map(CategoryDTO::getName).collect(Collectors.joining());
        //3.2 查询品牌名称
        BrandDTO brandDTO = itemClient.queryById(spu.getBrandId());
        String all = spu.getName()+categoryName+brandDTO.getName();
        //4、规格参数，包含key和value
        Map<String, Object> specs = new HashMap<>();
        //4.1 查询规格参数的key：当前商品所属分类下的需要搜索的规格参数
        List<SpecParamDTO> params = itemClient.findSpecParam(null, spu.getCid3(), true);
        //4.2 查询规格参数的value
        SpuDetailDTO spuDetail = itemClient.findSpuDetailSpuId(spuId);
        //4.2.1 取出通用规格参数
        Map<Long, Object> genericSpec = JsonUtils.toMap(spuDetail.getGenericSpec(), Long.class, Object.class);
        //4.2.1 取出特有规格参数
        Map<Long, List<String>> specialSpec = JsonUtils.nativeRead(spuDetail.getSpecialSpec(), new TypeReference<Map<Long, List<String>>>() {});
        //4.3 存入specs
        for (SpecParamDTO param : params) {
            //从param中取出name作为规格参数的key
            String key = param.getName();
            //从spuDetail取出值作为value
            Object value = null;
            //判断是否通用规格还是特有规格
            if(param.getGeneric()){
                //通用
                value = genericSpec.get(param.getId());
            }else{
                //特有
                value = specialSpec.get(param.getId());
            }
            specs.put(key,value);
        }

        //5、准备Goods对象
        Goods goods = new Goods();
        goods.setId(spu.getId()); //spuId
        goods.setBrandId(spu.getBrandId());//品牌id
        goods.setCategoryId(spu.getCid3());//分类id
        goods.setSubTitle(spu.getSubTitle());//副标题
        goods.setCreateTime(spu.getCreateTime().getTime());//创建时间
        goods.setSkus(JsonUtils.toString(skuList));//spu下的所有sku
        goods.setSpecs(specs);//规格参数，包含key和value
        goods.setPrice(price);//sku价格的集合
        goods.setAll(all);//搜索字段，包含名称、分类、品牌等
        //返回Goods对象
        return goods;
    }
}
```









## 06、商品搜索：构建Goods对象-优化数据

因为过滤参数中有一类比较特殊，就是数值区间，这些区间的值，是商品列表结果中有的才会显示在这里，如果结果中没有个数值区间的，我们不显示。



我们怎么知道结果中有哪些段？先知道结果中有那些尺寸，我们可以用聚合查询加上当前的查询条件，查询这些段，但是我们还需要拿着这些段，去一个个判断，看看那些包含，那些不包含，包含就放入页面显示，不包含就不显示，这样的做法太麻烦，效率不高。



现在的问题是：
1）不知道显示那些段
2）用段来过滤，性能太差



怎么办？这里的设计非常巧妙，当我们把屏幕尺寸存入到索引库的时候，我们不存入值，我们先把它转成段，然后再存入索引库，这样搜索的时候，我们就可以使用term词条查询了，这样的性能就很高了。例如：   5.2英寸    ，我们就存入一个段：   ==5.0-5.5英寸==，   搜索的时候，把==5.0-5.5英寸==当成词条即可。

![1529717362585](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1529717362585.png)

所以我们在存入时要进行分段处理，这段逻辑比较复杂，我已经把它封装成方法，大家直接调用即可：

```java
private String chooseSegment(Object value, SpecParamDTO p) {
    if (value == null || StringUtils.isBlank(value.toString())) {
        return "其它";
    }
    double val = parseDouble(value.toString());
    String result = "其它";
    // 保存数值段
    for (String segment : p.getSegments().split(",")) {
        String[] segs = segment.split("-");
        // 获取数值范围
        double begin = parseDouble(segs[0]);
        double end = Double.MAX_VALUE;
        if (segs.length == 2) {
            end = parseDouble(segs[1]);
        }
        // 判断是否在范围内
        if (val >= begin && val < end) {
            if (segs.length == 1) {
                result = segs[0] + p.getUnit() + "以上";
            } else if (begin == 0) {
                result = segs[1] + p.getUnit() + "以下";
            } else {
                result = segment + p.getUnit();
            }
            break;
        }
    }
    return result;
}

private double parseDouble(String str) {
    try {
        return Double.parseDouble(str);
    } catch (Exception e) {
        return 0;
    }
}
```







优化之后的完整的==**SearchService**==类如下：

```java
package com.leyou.search.service;

import com.fasterxml.jackson.core.type.TypeReference;
import com.leyou.common.utils.JsonUtils;
import com.leyou.item.client.ItemClient;
import com.leyou.item.dto.*;
import com.leyou.search.entity.Goods;
import org.apache.commons.lang.StringUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.*;
import java.util.stream.Collectors;

@Service
public class SearchService {

    @Autowired
    private ItemClient itemClient;

    /**
     * 把一个spu转为一个Goods对象
     * @param spu
     * @return
     */
    public Goods buildGoods(SpuDTO spu){
        Long spuId = spu.getId();
        //1、spu下所有的sku
        List<SkuDTO> skuDTOList = itemClient.findSkuListBySpuId(spuId);
        List<Map<String, Object>> skuList = new ArrayList<>();
        skuDTOList.forEach(skuDTO -> {
            Map<String, Object> sku = new HashMap<>();
            sku.put("id", skuDTO.getId());
            sku.put("image", StringUtils.substringBefore(skuDTO.getImages(),","));
            sku.put("price", skuDTO.getPrice());
            sku.put("title", skuDTO.getTitle());
            skuList.add(sku);
        });
        //2、sku的价格集合
        Set<Long> price = skuDTOList.stream().map(SkuDTO::getPrice).collect(Collectors.toSet());
        //3、搜索字段，包含名称、分类、品牌、等
        //3.1 查询分类名称
        List<CategoryDTO> categoryList = itemClient.queryByIds(spu.getCategoryIds());
        String categoryName = categoryList.stream().map(CategoryDTO::getName).collect(Collectors.joining());
        //3.2 查询品牌名称
        BrandDTO brandDTO = itemClient.queryById(spu.getBrandId());
        String all = spu.getName()+categoryName+brandDTO.getName();
        //4、规格参数，包含key和value
        Map<String, Object> specs = new HashMap<>();
        //4.1 查询规格参数的key：当前商品所属分类下的需要搜索的规格参数
        List<SpecParamDTO> params = itemClient.findSpecParam(null, spu.getCid3(), true);
        //4.2 查询规格参数的value
        SpuDetailDTO spuDetail = itemClient.findSpuDetailSpuId(spuId);
        //4.2.1 取出通用规格参数
        Map<Long, Object> genericSpec = JsonUtils.toMap(spuDetail.getGenericSpec(), Long.class, Object.class);
        //4.2.1 取出特有规格参数
        Map<Long, List<String>> specialSpec = JsonUtils.nativeRead(spuDetail.getSpecialSpec(), new TypeReference<Map<Long, List<String>>>() {});
        //4.3 存入specs
        for (SpecParamDTO param : params) {
            //从param中取出name作为规格参数的key
            String key = param.getName();
            //从spuDetail取出值作为value
            Object value = null;
            //判断是否通用规格还是特有规格
            if(param.getGeneric()){
                //通用
                value = genericSpec.get(param.getId());
            }else{
                //特有
                value = specialSpec.get(param.getId());
            }
            //判断是否是数值类型，需要判断是那个区间，存区间到索引库
            if(param.getNumeric()){
                //数值类型，需要处理区间
                value = chooseSegment(value, param);
            }
            specs.put(key,value);
        }

        //5、准备Goods对象
        Goods goods = new Goods();
        goods.setId(spu.getId()); //spuId
        goods.setBrandId(spu.getBrandId());//品牌id
        goods.setCategoryId(spu.getCid3());//分类id
        goods.setSubTitle(spu.getSubTitle());//副标题
        goods.setCreateTime(spu.getCreateTime().getTime());//创建时间
        goods.setSkus(JsonUtils.toString(skuList));//spu下的所有sku
        goods.setSpecs(specs);//规格参数，包含key和value
        goods.setPrice(price);//sku价格的集合
        goods.setAll(all);//搜索字段，包含名称、分类、品牌等
        //返回Goods对象
        return goods;
    }

    private String chooseSegment(Object value, SpecParamDTO p) {
        if (value == null || StringUtils.isBlank(value.toString())) {
            return "其它";
        }
        double val = parseDouble(value.toString());
        String result = "其它";
        // 保存数值段
        for (String segment : p.getSegments().split(",")) {
            String[] segs = segment.split("-");
            // 获取数值范围
            double begin = parseDouble(segs[0]);
            double end = Double.MAX_VALUE;
            if (segs.length == 2) {
                end = parseDouble(segs[1]);
            }
            // 判断是否在范围内
            if (val >= begin && val < end) {
                if (segs.length == 1) {
                    result = segs[0] + p.getUnit() + "以上";
                } else if (begin == 0) {
                    result = segs[1] + p.getUnit() + "以下";
                } else {
                    result = segment + p.getUnit();
                }
                break;
            }
        }
        return result;
    }

    private double parseDouble(String str) {
        try {
            return Double.parseDouble(str);
        } catch (Exception e) {
            return 0;
        }
    }
}
```





## 07、商品搜索：创建GoodsRepository并测试数据导入

现在， 我们就可以调用商品微服务接口，完成数据的导入了，基本步骤如下：

- 查询商品数据
- 根据spu等信息，构建goods对象
- 把goods存入索引库



### 1）创建GoodsRepository

```java
package com.leyou.search.repository;

import com.leyou.search.pojo.Goods;
import org.springframework.data.elasticsearch.repository.ElasticsearchRepository;

/**
 * @author 黑马程序员
 */
public interface GoodsRepository extends ElasticsearchRepository<Goods, Long> {
}
```





### 2）测试数据导入

接下来我们就可以编写测试类，然后把数据库的数据导入索引库了，来，开干了！

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class LoadDataTest {

    @Autowired
    private SearchService searchService;

    @Autowired
    private GoodsRepository repository;

    @Autowired
    private ItemClient itemClient;

    @Test
    public void test(){
        int page = 1, rows = 100;
        do {
            try {
                //查询spu
                PageResult<SpuDTO> result = itemClient.querySpuByPage(null,page, rows, true);
                List<SpuDTO> spuList = result.getItems();
                //构建goods
                List<Goods> goodsList = spuList.stream().map(searchService::buildGoods).collect(Collectors.toList());
                //存储到索引库
                repository.saveAll(goodsList);
                //分页
                page++;
            } catch (Exception e) {
                e.printStackTrace();
                break;
            }
        }while (true);
    }
}
```

执行测试代码，然后查看索引库：

![1576059450814](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576059450814.png) 























## 08、基本搜索：搜索页面分析

在首页的顶部，有一个输入框：

![1576118211900](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576118211900.png) 

当我们输入任何文本，点击搜索，就会跳转到搜索页`search.html`了：

并且将搜索关键字以请求参数携带过来：

![1576118400717](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576118400717.png) 

首页是`index.html`中有这么一段代码：

![1576118772328](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576118772328.png) 

接下来我们看看js下的pages目录的top.js文件：

![1576119088489](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576119088489.png) 

当我们点击了搜索按钮，那么就会触发search方法，所以接下来我们看下top.js的search方法：

![1576119247538](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576119247538.png) 

可以看到，这个方法会自动把搜索关键字  拼接到search.html页面的参数部分，因此跳转到search.html页面的时候，这个页面就自动有了参数，那么页面有了参数，在哪里去执行查询操作？

自动触发搜索，那肯定是钩子函数了，我们可以打开search.html，在这里编写相应的方法即可。

在search.html这个Vue实例中，通过import导入的方式，加载了另外一个js：top.js并作为一个局部组件。top其实是页面顶部导航组件，我们暂时不管，我们主要关系**search.html**就可以了。













## 09、基本搜索：发送搜索请求与跨域问题

接下来我们编写search.html页面，在这个页面我们来发起搜索请求，在哪里发起？

就是 我们上一步分析的钩子函数，我们在search.html页面开发即可。

接下来，我们就在这个页面开发发起搜索的请求，

要想在页面加载后，就展示出搜索结果。我们应该在页面加载时，获取地址栏请求参数，并发起异步请求，查询后台数据，然后在页面渲染。

我们在data中定义一个对象，记录请求的参数：

```js
data: {
    search:{
        key:"", // 搜索页面的关键字
        page: 0, // 记录分页参数
    }
}
```



我们通过钩子函数created，在页面加载时获取请求参数，并记录下来。

```js
created() {
    // 获取url路径中的参数，目前可以想到的有搜索参数和分页参数，分页参数需要我们初始化
    const key = ly.getUrlParam("key");
    const page = ly.getUrlParam("page");
    // 判断是否有请求参数
    if (!key) {
        return;
    }
    // 保存key
    this.search.key = key;
    this.search.page = page ? parseInt(page) : 1;
    // 发起请求，根据条件搜索
    this.loadData();
}
```

然后发起请求，搜索数据。

```js
methods: {
    loadData() {
        // 发起异步请求
        ly.http.post("/search/page", this.search)
            .then(resp => {
            console.log(resp.data);
        })
    }
}
```

- 我们这里使用`ly`是common.js中定义的工具对象。
- 这里使用的是post请求，这样可以携带更多参数，并且以json格式发送



通过浏览器查看我们的参数和接口是否正确：

发送请求时发现，有跨域问题，原因是我们之前的配置的跨域，是manage.leyou.com，现在我们是在www.leyou.com发送请求到api.leyou.com的，因此需要在网关配置允许www.leyou.com的跨域：

![1576134693861](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576134693861.png) 

打开`ly-gateway`的application.yml配置允许跨域：

![1576134846932](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576134846932.png) 

修改完成后，重启网关微服务，再次访问，发现跨域问题已经解决：

![1576134970618](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576134970618.png) 

接下来我们看看请求参数等是否正确：

![1576135079535](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576135079535.png) 

好，页面请求和页面参数都处理好了，接下来我们就开始写后台代码







## 10、基本搜索：后台Controller编写

首先分析几个问题：

- 请求方式：Post

- 请求路径：/search/page，不过前面的/search应该是网关的映射路径，因此真实映射路径page，代表分页查询

- 请求参数：json格式，目前只有一个属性：key,搜索关键字，但是搜索结果页一定是带有分页查询的，所以将来肯定会有page属性，因此我们可以用一个对象来接收请求的json数据：

  ```java
  package com.leyou.search.dto;
  
  public class SearchRequest {
      private String key;// 搜索条件
  
      private Integer page;// 当前页
  
      private static final Integer DEFAULT_SIZE = 20;// 每页大小，不从页面接收，而是固定大小
      private static final Integer DEFAULT_PAGE = 1;// 默认页
  
      public String getKey() {
          return key;
      }
  
      public void setKey(String key) {
          this.key = key;
      }
  
      public Integer getPage() {
          if(page == null){
              return DEFAULT_PAGE;
          }
          // 获取页码时做一些校验，不能小于1
          return Math.max(DEFAULT_PAGE, page);
      }
  
      public void setPage(Integer page) {
          this.page = page;
      }
  
      public Integer getSize() {
          return DEFAULT_SIZE;
      }
  }
  ```

- 返回结果：作为分页结果，一般都两个属性：当前页数据、总条数信息，我们可以使用之前定义的PageResult类，但是，PageResult中要使用DTO返回：

  ```java
  package com.leyou.search.dto;
  
  import lombok.Data;
  
  /**
   * @author 黑马程序员
   */
  @Data
  public class GoodsDTO {
      private Long id; // spuId
      private String subTitle;// 卖点
      private String skus;// sku信息的json结构
  }
  ```

- 结构：

  ![1576139823476](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576139823476.png) 

 

- 处理器代码

  ```java
  @RestController
  public class SearchController {
  
      @Autowired
      private SearchService searchService;
  
      @PostMapping("/page")
      public ResponseEntity<PageResult<GoodsDTO>> search(@RequestBody SearchRequest request){
          PageResult<GoodsDTO> result = searchService.search(request);
          return ResponseEntity.ok(result);
      }
  }
  ```

  







## 11、基本搜索：后台Service编写

我们在SearchService中添加搜索的方法：

```java
@Service
public class SearchService {

    @Autowired
    private ItemClient itemClient;

    @Autowired
    private GoodsRepository repository;

    @Autowired
    private ElasticsearchTemplate esTemplate;

    /**
     * 商品搜索
     * @param request
     * @return
     */
    public PageResult<GoodsDTO> search(SearchRequest request) {
        // 0.健壮性判断
        String key = request.getKey();
        if (StringUtils.isBlank(key)) {
            throw new LyException(ExceptionEnum.INVALID_PARAM_ERROR);
        }
        // 1.创建原生搜索构建器
        NativeSearchQueryBuilder queryBuilder = new NativeSearchQueryBuilder();
        // 2.组织条件
        // 2.0.source过滤，控制字段数量
        queryBuilder.withSourceFilter(new FetchSourceFilter(new String[]{"id", "subTitle", "skus"}, null));
        // 2.1.搜索条件
        queryBuilder.withQuery(QueryBuilders.matchQuery("all", key).operator(Operator.AND));
        // 2.2.分页条件
        int page = request.getPage() - 1;
        int size = request.getSize();
        queryBuilder.withPageable(PageRequest.of(page, size));
        // 3.搜索结果
        AggregatedPage<Goods> result = esTemplate.queryForPage(queryBuilder.build(), Goods.class);
        // 4.解析结果
        // 4.1.解析分页数据
        long total = result.getTotalElements();
        int totalPage = result.getTotalPages();
        List<Goods> list = result.getContent();
        // 4.2.转换DTO
        List<GoodsDTO> dtoList = BeanHelper.copyWithCollection(list, GoodsDTO.class);
        // 5.封装并返回
        return new PageResult<>(total, totalPage, dtoList);
    }

    //构建查询条件
    private MatchQueryBuilder buildSearchKey(SearchRequest request) {
        return QueryBuilders.matchQuery("all",request.getKey()).operator(Operator.AND);
    }
    
    。。。。。。。
    //这里省略之前编写的其他方法
    。。。。。。。
    
}
```

注意点：我们要设置SourceFilter，来选择要返回的结果，否则返回一堆没用的数据，影响查询效率。



编写完成后，我们重启搜索微服务，然后测试：

![1576144902502](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576144902502.png) 





















## 12、基本搜索：页面保存后台返回的列表数据

页面已经拿到了结果，接下来就要渲染样式了，通过上一步的操作中，数据是已经全部返回了，但有些数据我们为了操作更加方便，我们需要把一些字符串的数据转成JSON对象来操作。

在search.html的js代码中加入渲染样式：

```js
<script type="text/javascript">
    var vm = new Vue({
        el: "#searchApp",
        data: {
            search: {
                key: "",//搜索页面关键字
                page: 1,//记录分页参数
            },
            total: 0,
            totalPage: 0,
            items: []
        },
        created(){
            const key = ly.getUrlParam("key");
            if(!key){
                alert("请添加查询条件")
            }
            //赋值
            this.search.key = key
            this.search.page = ly.getUrlParam("page") || 1;
            //向服务器发送查询请求
            this.loadData()
        },
        methods: {
          loadData(){
              //向后台发送请求
              ly.http.post("/search/page",this.search)
                  .then(res=>{
                      console.log(res)
                      this.total = res.data.total
                      this.totalPage = res.data.totalPage

                      res.data.items.forEach(item => {
                          //把sku字符串集合转成对象格式
                          item.skus = JSON.parse(item.skus)
                          //给当前对象添加一个属性【选中的sku对象】
                          item.selectSku = item.skus[0]
                      })
                      this.items = res.data.items
                  });
          }
        },
        components:{
            lyTop: () => import("./js/pages/top.js")
        }
    });
</script>
```

在Chrome中通过vue插件去查看，这些数据是否已经变成我们要的数据：

![1576148514401](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576148514401.png) 

通过这一步，我们要渲染的数据全部准备好了，我们动态加入的数据如下图：

![1576201525605](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576201525605.png) 

接下来，我们就把这些数据渲染到页面中。









## 13、基本搜索：循环展示商品列表数据

### 1）循环展示商品

在search.html的中部，有一个`div`，用来展示所有搜索到的商品：

![1576202284568](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576202284568.png) 

可以看到，`div`中有一个无序列表`ul`，内部的每一个`li`就是一个商品spu了。

我们删除多余的，只保留一个`li`，然后利用vue的循环来展示搜索到的结果：

![1576202625582](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576202625582.png) 



### 2）多SKU展示

#### 分析

接下来展示具体的商品信息，来看图：

 ![1526607712207](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1526607712207.png)

这里我们可以发现，一个商品位置，是多个sku的信息集合。**当用户鼠标选择某个sku，对应的图片、价格、标题会随之改变！**

我们先来实现sku的选择，才能去展示不同sku的数据。

 ![1526654252710](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1526654252710.png)

可以看到，在列表中默认第一个是被选中的，而且渲染的图片价格也是第一个sku的，而我们返回的数据中，多个sku是json格式：

![1553684825713](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1553684825713.png)

那我们就需要做4件事情：

- 把skus这个json字符串，变成JS对象
- 记录当前被选中的是哪一个sku，记录在哪里比较合适呢？显然是遍历到的goods对象自己内部，因为每一个goods都会有自己的sku信息。
- 默认把第一个sku作为被选中的，记录下来
- 以后在根据用户行为，选中不同的sku



#### 初始化sku

我们在查询成功的回调函数中，对goods进行遍历，然后添加一个selected属性，保存被选中的sku：

![1576209632363](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576209632363.png) 

看到结果中，默认选中的是数组中的第一个：

![1576215458903](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576215458903.png)  



#### 多sku图片列表

接下来，我们看看多个sku的图片列表位置：下图中ul部分

![1576209766993](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576209766993.png) 

看到又是一个无序列表，这里我们也一样删掉多余的，保留一个`li`，需要注意选中的项有一个样式类：selected

我们的代码：

```vue
<!--多sku图片列表-->
<ul class="skus">
    <li :class="{selected: sku.id===goods.selectSku.id}"
        v-for="sku in goods.skus"
        :key="sku.id"
        @mouseover="goods.selectSku=sku">
        <img :src="sku.image">
    </li>
</ul>
```

注意：

- class样式通过 goods.selectedSku的id是否与当前sku的id一致来判断
- 绑定了鼠标事件，鼠标进入后把当前sku赋值到goods.selectedSku



### 3）展示SKU其他属性

#### 价格显示的是分

首先价格显示就不正确，我们数据库中存放的是以分为单位，所以这里要格式化。

好在我们之前common.js中定义了工具类，可以帮我们转换。

改造：

 ![1576217473014](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576217473014.png) 

结果报错：

 ![1526656512105](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1526656512105.png)

为啥？

因为在Vue范围内使用任何变量，都会默认去Vue实例中寻找，我们使用ly，但是Vue实例中没有这个变量。所以解决办法就是把ly记录到Vue实例：

![1576217388844](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576217388844.png) 

然后刷新页面：

 ![1526656689574](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1526656689574.png)



#### 标题过长

标题内容太长了，已经无法完全显示，怎么办？

可以通过css样式隐藏多余数据：

![1576217552123](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576217552123.png) 



#### 最终结果

 ![](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/skus.gif)









## 14、基本搜索：展示商品列表的分页信息

数据展示部分我们已经开发完成了，接下来我们来开发分页部分的内容，在这个页面中，我们的分页有两部分，列表下方的分页部分，和列表上面的分页部分：

列表下方的分页：

![1576220429830](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576220429830.png) 

列表上方的分页：

![1576220481597](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576220481597.png) 

列表下方的分页，项目一已经做过了，这块纯前端的js就可以搞定，我们就不浪费时间在这里了，另外，讲义里有详细的实现代码，大家有兴趣，可以按照讲义去好好实现以下即可。

我们的这个分页，就只做列表上方的部分即可。

我们打开search.html页面，找到列表上面分页部分的代码，然后展示从后台返回的分页数据，如下：

![1576221314000](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576221314000.png) 

分页数据在我们的请求中已经返回了，现在来展示它们：

展示代码：

![1576222300529](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576222300529.png) 

在methods中添加两个方法，分别对应上一页和下一页的事件：

![1576222436041](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576222436041.png) 

页面的效果如下：

![page](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/page.gif) 





## 15、基本搜索：提供page改变时的监听事件

在上一步中，我们已经在上一页和下一页中实现了当前页的改变，接下来我们要实现把这个当前页往后台传递，实现分页查询。

现在要做分页，也就是说当查询条件的page改变的时候，向后台发送请求即可，在之前我们学过vue的监听事件：watch，那么现在我们就用监听事件来完成这个分页的动作。

![1576224935074](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/1576224935074.png) 

修改完代码，我们查看页面是否真的分页查询了：

![1576224935075](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片二/pageNo.gif)

ok了，点击下一页或者上一页，请求都已经发送到后台并且正确返回我们要的数据。















## 16、基本搜索：刷新操作的page值传递

在上一步我们已经完成 当`page`发生变化，马上重新查询数据并且页面也跟着发生变化。

不过，如果我们直接发起ajax请求，那么浏览器的地址栏中是不会有变化的，没有记录下分页信息。如果用户刷新页面，那么就会回到第一页。

这样不太友好，我们应该把**搜索条件记录在地址栏的查询参数中**。

因此，我们监听search中的page的变化，然后把search的过滤字段拼接在url路径后：

```js
watch:{
    "search.page":{
        handler(val){
            // 把search对象变成请求参数，拼接在url路径
            window.location.search = "?" + ly.stringify(this.search);
        }
    }
},
```

刷新页面测试，然后就出现重大bug：页面无限刷新！为什么？



因为Vue实例初始化的钩子函数中，我们读取请求参数，赋值给search的时候，也触发了watch监视！也就是说，每次页面创建完成，都会触发watch，然后就会去修改window.location路径，然后页面被刷新，再次触发created钩子，又触发watch，周而复始，无限循环。

思考：如果watch中我们不修改路径地址，则不会引起页面自动刷新，但是又需要把请求参数记录在URL路径中，怎么办？



我们可以通过浏览器的history来实现：

详细代码如下：

```js
watch: {
  "search.page":{
      handler(){
          //这里处理url中的page参数
          var thisURL = document.location.href
          if (thisURL.indexOf('?') > 0) {
              thisURL = thisURL.split("?")[0] + "?" + ly.stringify(this.search);
          } else {
              thisURL = thisURL + "?" + ly.stringify(this.search);
          }
          var state = {
              title: document.title,
              url: document.location.href,
              otherkey: null
          };
          //替换url中的参数
          history.replaceState(state, document.title, thisURL);
          //当page改变的时候，向后台发送请求
          this.loadData()
      }
  }
},
```

好了，通过上面的代码我们就可以实现点击下一页的时候，页码也能放到浏览器的地址栏了。

以后有可能很多地方都需要这样的配置，所以我们将这段代码抽取放入到common.js中，供后续的使用，优化如下：

common.js：

```js
/**
 * 把请求参数放入url中，并且浏览器不刷新
 * @param params
 */
replaceUrlPageParam(params){
    //这里处理url中的page参数
    var thisURL = document.location.href
    if (thisURL.indexOf('?') > 0) {
        //判断如果url中有？,则去除？后面的参数，加入我们自己的参数
        thisURL = thisURL.split("?")[0] + "?" + ly.stringify(params);
    } else {
        //url没有？则直接把我们的参数拼接到url中
        thisURL = thisURL + "?" + ly.stringify(params);
    }
    //判断如果最后一个字符是&  去除它
    if(thisURL.endsWith("&")){
        thisURL = thisURL.substring(0,thisURL.length-1);
    }
    //定义状态
    var state = {
        title: document.title,
        url: document.location.href,
        otherkey: null
    };
    //替换url中的参数
    history.replaceState(state, document.title, thisURL);
}
```

修改search.html：

```js
watch: {
  "search.page":{
      handler(){
          //把参数放入url中
          ly.replaceUrlPageParam(this.search)
          //当page改变的时候，向后台发送请求
          this.loadData()
      }
  }
},
```





















