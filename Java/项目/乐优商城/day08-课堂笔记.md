## 01、课程目标

- 了解过滤功能的基本思路
- 独立实现分类和品牌展示
- 了解规格参数展示
- 实现过滤条件筛选































## 02、搜索过滤：分析过滤条件从何而来

### 1）功能模块

首先看下页面要实现的效果：

![1526725119663](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1526725119663.png)

整个过滤部分有3块：

- 顶部的导航，已经选择的过滤条件展示：
  - 商品分类面包屑，根据用户选择的商品分类变化
  - 其它已选择过滤参数
- 过滤条件展示，又包含3部分
  - 商品分类展示
  - 品牌展示
  - 其它规格参数
- 展开或收起的过滤条件的按钮



顶部导航要展示的内容跟用户选择的过滤条件有关。

- 比如用户选择了某个商品分类，则面包屑中才会展示具体的分类
- 比如用户选择了某个品牌，列表中才会有品牌信息。

所以，这部分需要依赖第二部分：过滤条件的展示和选择。因此我们先不着急去做。



展开或收起的按钮是否显示，取决于过滤条件现在有多少，如果有很多，那么就没必要展示。所以也是跟第二部分的过滤条件有关。

这样分析来看，我们必须先做第二部分：过滤条件展示。



### 2）问题分析

过滤条件包括：分类过滤、品牌过滤、规格过滤项等。我们必须弄清楚几个问题：

- 什么时候查询这些过滤项？
- 这些过滤项的数据从何而来？

我们先以分类和品牌来讨论一下：

> 问题1，什么时候查询这些过滤项？

现在，页面加载后就会调用`loadData`方法，向服务端发起请求，查询商品数据。我们有两种选择：

- 方式1：在查询商品数据的同时，顺便把分类和品牌的过滤数据一起查出来
  - 优点：只有一次请求，逻辑简单
  - 缺点：该请求处理业务较多，业务复杂，效率较差
- 方式2：在查询商品后，再发一个ajax请求，专门查询分类和品牌的过滤数据
  - 优点：每个请求做个业务，耦合度低，每次请求处理效率较高
  - 缺点：需要发多次请求

这里考虑使用方式2，让每次请求做自己的事情，减少业务耦合。

> 问题2，过滤项的数据从何而来？

在我们的数据库中已经有所有的分类和品牌信息。在这个位置，是不是把所有的分类和品牌信息都展示出来呢？

显然不是，用户搜索的条件会对商品进行过滤，而在搜索结果中，不一定包含所有的分类和品牌，直接展示出所有商品分类，让用户选择显然是不合适的。

比如，用户搜索：`小米手机`，结果中肯定只有手机，而且还得是小米的手机，就把分类和品牌限定死了，此时显示出其它品牌和分类过滤项显然是不合适的。

因此，只有**`在搜索过滤的结果中存在的分类和品牌才能作为过滤项让用户选择`**。

那么问题来了：我们怎么知道搜索结果中有哪些分类和品牌呢？

答案是：利用elasticsearch提供的聚合功能，**`在搜索条件基础上，对搜索结果聚合`**，就能知道结果中包含哪些分类和品牌了。当然，规格参数也是一样的。



















## 03、分类和品牌过滤：发送过滤参数请求

首先，我们先完成分类和品牌的过滤项的查询。

首先，定义一个函数，在函数内部发起ajax请求，查询各种过滤项：

==注意：url我们要参考api文档==

```js
loadFilterParams(){
    // 发起请求，查询过滤项
    ly.http.post("/search/filter", this.search)
      .then(res => {
          console.log(res)
      })
}
```

注意：请求的参数与搜索商品时的请求参数是一致的，因为我们需要**`在搜索条件基础上，对搜索结果聚合`**。



在created钩子函数中，在查询商品数据的之后，调用这个方法：

![1576465586185](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576465586185.png) 



编写完成之后我们打开浏览器，看一下请求是否已经发送到后台：

![1576466158053](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576466158053.png) 

请求参数和路径都没有问题了，而且特殊请求的预检OPTIONS请求也成功发送了：

![1576466249412](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576466249412.png) 

好了，请求已经成功发出了。















## 04、分类和品牌过滤：ES聚合查询

说明：搜索页面的列表数据是根据搜索条件查询出来的，搜索页面的过滤条件是由所有结果数据聚合而来的。



ElasticSearch中如何实现聚合查询？我们可以利用kibana来测试聚合查询：

执行的json数据：

```json
GET _search
{
  "query": {
    "match": {
      "all": "手机"
    }
  },
  "aggs": {
    "categoryAgg": {
      "terms": {
        "field": "categoryId",
        "size": 10
      }
    },
    "brandAgg": {
      "terms": {
        "field": "brandId",
        "size": 10
      }
    }
  },
  "size": 0,
  "_source": [""]
}
```

执行结果：

![1576469539622](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576469539622.png) 



















## 05、分类和品牌过滤：ES聚合查询业务代码

### 1）请求分析

上面的请求发出了，我们就知道了下面的信息：

- 请求方式：Post
- 请求路径：/search/filter
- 请求参数：与商品搜索一样，是SearchRequest对象

那么问题来了：以什么格式返回呢？

来看下页面的展示效果：

 ![1526742664217](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1526742664217.png)

虽然分类、品牌等过滤内容都不太一样，但是结构相似，都是key和value的结构。

- key是过滤参数名称，如：分类
- value是过滤的待选项，如：手机，儿童手表。

类似这样的键值对结构，是不是可以用一个Map来表示呢？

而这样的过滤条件很多，所以可以用一个数组表示，其基本结构是这样的：

```js
{
    "过滤字段名称1":['过滤项1','过滤项2',...],
    "过滤字段名称2":['过滤项1','过滤项2',...]
}
```

类似于java中的：`Map<String,List<?>>`

注意，这里的分类和品牌过滤项，不仅仅是分类和品牌的名称，还有品牌的图片，id等。所以待选项应该是一个分类和品牌的对象。

接下来就开始编写业务代码！



### 2）编写Controller

```java
@PostMapping("/filter")
public ResponseEntity<Map<String, List<?>>> queryFilterParams(@RequestBody SearchRequest request){
    Map<String, List<?>> result = searchService.queryFilterParams(request);
    return ResponseEntity.ok(result);
}
```



### 3）编写Service

这里我们得到的都是id集合，我们可以通过远程调用feign接口，去获取数据，在这里我们用到远程调用接口：

- ItemClient中的根据分类id集合获取分类集合
- ItemClient中的根据品牌id或者品牌，这里只是查询单个品牌，需要遍历收集，如果需要通过id集合批量查询，需要在`item-service`和`item-client`中新添加一个，我们这里就先不加了

```java
/**
 * 获取过滤参数
 * @param request
 * @return
 */
public Map<String, List<?>> queryFilterParams(SearchRequest request) {
    Map<String, List<?>> filterParam = new LinkedHashMap<>();
    //提供一个封装条件的对象
    NativeSearchQueryBuilder builder = new NativeSearchQueryBuilder();
    //优化查询部分：不要结果只要聚合部分
    builder.withSourceFilter(new FetchSourceFilter(new String[]{""}, null));
    //必须要一个分页
    builder.withPageable(PageRequest.of(0, 1));
    //添加查询条件
    builder.withQuery(buildSearchKey(request));
    //添加分类聚合条件
    String categoryAgg = "categoryAgg";
    builder.addAggregation(AggregationBuilders.terms(categoryAgg).field("categoryId"));
    //添加品牌聚合条件
    String brandAgg = "brandAgg";
    builder.addAggregation(AggregationBuilders.terms(brandAgg).field("brandId"));
    //索引库查询
    AggregatedPage<Goods> goodsAggregatedResult = esTemplate.queryForPage(builder.build(), Goods.class);
    //解析结果将分类结果放入filterParam中
    Terms categoryTerm = (Terms) goodsAggregatedResult.getAggregation(categoryAgg);
    handlerCatagoryParam(categoryTerm, filterParam);
    //解析结果将品牌结果放入filterParam中
    Terms brandTerm = goodsAggregatedResult.getAggregations().get(brandAgg);
    handlerBrandTerm(brandTerm, filterParam);
    return filterParam;
}

//获取搜索页面分类过滤参数
private void handlerCatagoryParam(Terms categoryTerm, Map<String, List<?>> filterParam) {
    List<Long> ids = categoryTerm.getBuckets().stream().map(Terms.Bucket::getKeyAsNumber).map(Number::longValue).collect(Collectors.toList());
    //通过所有分类的id集合查询所有分类对象的集合
    List<CategoryDTO> categoryDTOList = itemClient.queryByIds(ids);
    //放入map中
    filterParam.put("分类", categoryDTOList);
}

//获取搜索页面品牌过滤参数
private void handlerBrandTerm(Terms brandTerm, Map<String, List<?>> filterParam) {
    List<BrandDTO> brandDTOS = brandTerm.getBuckets().stream()
            .map(Terms.Bucket::getKeyAsNumber)
            .map(Number::longValue)
            .map(itemClient::queryById)
            .collect(Collectors.toList());
    //放入map中
    filterParam.put("品牌",brandDTOS);
}
```





### 4）测试

![1576482078958](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576482078958.png) 







## 06、分类和品牌过滤：页面处理过滤结果

### 1）页面接收数据

在data中定义接收数据的对象，然后在请求回调中给它赋值，赋值完成后就可以渲染了。

![1576482796120](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576482796120.png) 



### 2）页面展示数据

上一步已经保存了返回的数据，现在我们需要把分类和品牌的数据在搜索页面中进行遍历渲染。

这一步我们就开始渲染分类的品牌：

```html
<div class="clearfix selector">
    <div class="type-wrap" v-for="(value, key, index) in filterParamMap" :key="index" v-if="key !== '品牌'">
        <div class="fl key">{{key}}</div>
        <div class="fl value">
            <ul class="type-list">
                <li v-for="o in value" :key="o.id">
                    <a>{{o.name}}</a>
                </li>
            </ul>
        </div>
        <div class="fl ext"></div>
    </div>
    <div class="type-wrap logo" v-else>
        <div class="fl key brand">{{key}}</div>
        <div class="value logos">
            <ul class="logo-list">
                <li v-for="b in value" :key="b.id" v-if="b.image">
                    <img :src="b.image" />
                </li>
                <li style="text-align: center" v-else>
                <a style="line-height: 30px; font-size: 12px" href="#">{{b.name}}</a>
                </li>
            </ul>
        </div>
        <div class="fl ext">
            <a href="javascript:void(0);" class="sui-btn">多选</a>
        </div>
    </div>
    <div class="type-wrap" style="text-align: center">
        <v-btn small flat>
            更多<v-icon>arrow_drop_down</v-icon>
        </v-btn>
        <v-btn small="" flat>
            收起<v-icon>arrow_drop_up</v-icon>
        </v-btn>
    </div>
</div>
```

为了好看，我们把css样式的某段注释代码放开，如下图的样式代码：

![1576485079624](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576485079624.png) 

然后查看页面，看看效果即可：

![1576485181311](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576485181311.png) 



















## 07、规格参数过滤：分析规格参数的来源

有3个问题需要先思考清楚：

- 什么时候显示规格参数过滤？
- 如何知道哪些规格需要过滤？
- 要过滤的参数，其可选值是如何获取的？



> 什么情况下显示有关规格参数的过滤？

可能有同学会想，这还要思考吗？查询商品分类和品牌过滤项的同时，把规格参数过滤项一起返回啊！



但是，如果用户尚未选择商品分类，或者聚合得到的分类数大于1，那么就没必要进行规格参数的聚合。因为不同分类的商品，其规格是不同的，我们无法确定到底有多少规格需要聚合，代码无法进行。

因此，我们在后台**需要对聚合得到的商品分类数量进行判断，如果等于1，我们才继续进行规格参数的聚合**。

此时，只需要聚合当前分类下的规格参数即可，数量可以确定。

> 如何知道哪些规格需要过滤？

那么，我们是不是把数据库中的所有规格参数都拿来过滤呢？

显然不是！

因为并不是所有的规格参数都可以用来过滤。

庆幸的是，我们在设计规格参数时，已经标记了某些规格可搜索，某些不可搜索，还记得SpecParam中的searching字段吗？

因此，一旦商品分类确定，我们就可以根据商品分类查询到其对应的规格参数，并过滤哪些searching值为true的规格参数，然后对这些参数聚合即可。



> 要过滤的参数，其可选值是如何获取的？

虽然数据库中有所有的规格参数的可能值，但是不能把一切数据都用来供用户选择。

与商品分类和品牌一样，应该是结果中有哪些规格参数值，就显示哪些。

即：**`在搜索条件基础上，对搜索结果聚合`**，得到规格参数的待选项。

比如：用户搜索了OPPO 手机，那么过滤项中只应该有OPPO可能存在的屏幕尺寸，比如5.5以上的，不会存在5.5以下的尺寸让你选择。



















## 08、规格参数过滤：查询需要过滤的规格参数

接下来，我们就用代码实现刚才的思路。

总结一下，应该是以下几步：

- 1）用户搜索得到商品，并聚合出商品分类（已完成）
- 2）判断分类数量是否等于1，如果是则进行规格参数聚合
- 3）先根据分类，查找可以用来搜索的规格
- 4）在用户搜索结果的基础上，对规格参数进行聚合
- 5）将规格参数聚合结果整理后返回



### 1）返回聚合得到的分类id

```java
//获取搜索页面分类过滤参数
private List<Long> handlerCatagoryParam(Terms categoryTerm, Map<String, List<?>> filterParam) {
    List<Long> ids = categoryTerm.getBuckets().stream().map(Terms.Bucket::getKeyAsNumber).map(Number::longValue).collect(Collectors.toList());
    //通过所有分类的id集合查询所有分类对象的集合
    List<CategoryDTO> categoryDTOList = itemClient.queryByIds(ids);
    //放入map中
    filterParam.put("分类", categoryDTOList);
    return ids;
}
```

注意原来的返回值是：void，现在只是返回id的集合。



### 2）在过滤方法中提供规格参数过滤条件的查询

在SearchService#queryFilterParams方法中添加规格参数过滤条件。

```java
public Map<String, List<?>> queryFilterParams(SearchRequest request) {
    Map<String, List<?>> filterParam = new LinkedHashMap<>();
    ......
    ......
    //将规格参数过滤条件加入到filterParam中
    addSpecParamFilter(catagoryIds, buildSearchKey(request) ,filterParam);
    return filterParam;
}
```

然后我们在SearchService中重新添加一个方法：addSpecParamFilter

```java
//过滤规格参数
private void addSpecParamFilter(List<Long> catagoryIds, MatchQueryBuilder buildSearchKey, Map<String, List<?>> filterParam) {
    catagoryIds.forEach(categoryId -> {
        //获取当前分类下的可以被做为查询条件的规格参数
        List<SpecParamDTO> specParamDTOS = itemClient.findSpecParam(null, categoryId, true);
        //提供一个封装条件的对象
        NativeSearchQueryBuilder searchQueryBuilder = new NativeSearchQueryBuilder();
        //优化查询部分：不要结果只要聚合部分
        searchQueryBuilder.withSourceFilter(new FetchSourceFilter(new String[]{""}, null));
        //必须要一个分页
        searchQueryBuilder.withPageable(PageRequest.of(0, 1));
        //添加查询条件
        searchQueryBuilder.withQuery(buildSearchKey);
        //循环添加组合查询条件
        specParamDTOS.forEach(specParamDTO -> {
            //得到组合的名称
            String aggName = specParamDTO.getName();
            //得到组合所需的field字段
            String aggField = "specs."+aggName;
            //添加组合条件
            searchQueryBuilder.addAggregation(AggregationBuilders.terms(aggName).field(aggField));
        });
        //索引库查询
        AggregatedPage<Goods> result = esTemplate.queryForPage(searchQueryBuilder.build(), Goods.class);
        //得到所有的组合结果
        Aggregations aggregations = result.getAggregations();
        //遍历解析出数据和结果
        specParamDTOS.forEach(specParamDTO -> {
            //通过组合名称取出桶
            String aggName = specParamDTO.getName();
            //根据聚合后的桶的名称得到桶中数据的集合
            Terms terms = aggregations.get(aggName);
            //解析桶中数据的集合
            List<String> specList = terms.getBuckets().stream().map(Terms.Bucket::getKeyAsString).collect(Collectors.toList());
            //得到的规格参数放入map中
            filterParam.put(aggName, specList);
        });
    });
}
```



### 3）测试

![1576550667269](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576550667269.png) 



### 4）整个SearchService的完整代码

```java
package com.leyou.search.service;

import com.fasterxml.jackson.core.type.TypeReference;
import com.leyou.common.enums.ExceptionEnum;
import com.leyou.common.exception.LyException;
import com.leyou.common.utils.BeanHelper;
import com.leyou.common.utils.JsonUtils;
import com.leyou.common.vo.PageResult;
import com.leyou.item.client.ItemClient;
import com.leyou.item.dto.*;
import com.leyou.search.dto.GoodsDTO;
import com.leyou.search.dto.SearchRequest;
import com.leyou.search.entity.Goods;
import com.leyou.search.repository.GoodsRepository;
import org.apache.commons.lang.StringUtils;
import org.elasticsearch.index.query.MatchQueryBuilder;
import org.elasticsearch.index.query.Operator;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.search.aggregations.AggregationBuilders;
import org.elasticsearch.search.aggregations.Aggregations;
import org.elasticsearch.search.aggregations.bucket.terms.Terms;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.elasticsearch.core.ElasticsearchTemplate;
import org.springframework.data.elasticsearch.core.aggregation.AggregatedPage;
import org.springframework.data.elasticsearch.core.query.FetchSourceFilter;
import org.springframework.data.elasticsearch.core.query.NativeSearchQueryBuilder;
import org.springframework.stereotype.Service;

import java.util.*;
import java.util.stream.Collectors;

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
        queryBuilder.withQuery(buildSearchKey(request));
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

    /**
     * 获取过滤参数
     * @param request
     * @return
     */
    public Map<String, List<?>> queryFilterParams(SearchRequest request) {
        Map<String, List<?>> filterParam = new LinkedHashMap<>();

        //提供一个封装条件的对象
        NativeSearchQueryBuilder builder = new NativeSearchQueryBuilder();
        //优化查询部分：不要结果只要聚合部分
        builder.withSourceFilter(new FetchSourceFilter(new String[]{""}, null));
        //必须要一个分页
        builder.withPageable(PageRequest.of(0, 1));
        //添加查询条件
        builder.withQuery(buildSearchKey(request));

        //添加分类聚合条件
        String categoryAgg = "categoryAgg";
        builder.addAggregation(AggregationBuilders.terms(categoryAgg).field("categoryId"));

        //添加品牌聚合条件
        String brandAgg = "brandAgg";
        builder.addAggregation(AggregationBuilders.terms(brandAgg).field("brandId"));

        //索引库查询
        AggregatedPage<Goods> goodsAggregatedResult = esTemplate.queryForPage(builder.build(), Goods.class);

        //解析结果将分类结果放入filterParam中
        Terms categoryTerm = (Terms) goodsAggregatedResult.getAggregation(categoryAgg);
        List<Long> catagoryIds = handlerCatagoryParam(categoryTerm, filterParam);

        //解析结果将品牌结果放入filterParam中
        Terms brandTerm = goodsAggregatedResult.getAggregations().get(brandAgg);
        handlerBrandTerm(brandTerm, filterParam);

        //将规格参数过滤条件加入到filterParam中
        addSpecParamFilter(catagoryIds, buildSearchKey(request) ,filterParam);

        return filterParam;
    }

    //过滤规格参数
    private void addSpecParamFilter(List<Long> catagoryIds, MatchQueryBuilder buildSearchKey, Map<String, List<?>> filterParam) {
        catagoryIds.forEach(categoryId -> {
            //获取当前分类下的可以被做为查询条件的规格参数
            List<SpecParamDTO> specParamDTOS = itemClient.findSpecParam(null, categoryId, true);

            //提供一个封装条件的对象
            NativeSearchQueryBuilder searchQueryBuilder = new NativeSearchQueryBuilder();
            //优化查询部分：不要结果只要聚合部分
            searchQueryBuilder.withSourceFilter(new FetchSourceFilter(new String[]{""}, null));
            //必须要一个分页
            searchQueryBuilder.withPageable(PageRequest.of(0, 1));
            //添加查询条件
            searchQueryBuilder.withQuery(buildSearchKey);
            //循环添加组合查询条件
            specParamDTOS.forEach(specParamDTO -> {
                //得到组合的名称
                String aggName = specParamDTO.getName();
                //得到组合所需的field字段
                String aggField = "specs."+aggName;
                //添加组合条件
                searchQueryBuilder.addAggregation(AggregationBuilders.terms(aggName).field(aggField));
            });
            //索引库查询
            AggregatedPage<Goods> result = esTemplate.queryForPage(searchQueryBuilder.build(), Goods.class);

            //得到所有的组合结果
            Aggregations aggregations = result.getAggregations();

            //遍历解析出数据和结果
            specParamDTOS.forEach(specParamDTO -> {
                //通过组合名称取出桶
                String aggName = specParamDTO.getName();
                //根据聚合后的桶的名称得到桶中数据的集合
                Terms terms = aggregations.get(aggName);
                //解析桶中数据的集合
                List<String> specList = terms.getBuckets().stream()
                        .map(Terms.Bucket::getKeyAsString)
                        .filter(StringUtils::isNotBlank)
                        .collect(Collectors.toList());
                //得到的规格参数放入map中
                filterParam.put(aggName, specList);
            });
        });
    }

    //获取搜索页面分类过滤参数
    private List<Long> handlerCatagoryParam(Terms categoryTerm, Map<String, List<?>> filterParam) {
        List<Long> ids = categoryTerm.getBuckets().stream().map(Terms.Bucket::getKeyAsNumber).map(Number::longValue).collect(Collectors.toList());
        //通过所有分类的id集合查询所有分类对象的集合
        List<CategoryDTO> categoryDTOList = itemClient.queryByIds(ids);
        //放入map中
        filterParam.put("分类", categoryDTOList);
        return ids;
    }

    //获取搜索页面品牌过滤参数
    private void handlerBrandTerm(Terms brandTerm, Map<String, List<?>> filterParam) {
        List<BrandDTO> brandDTOS = brandTerm.getBuckets().stream()
                .map(Terms.Bucket::getKeyAsNumber)
                .map(Number::longValue)
                .map(itemClient::queryById)
                .collect(Collectors.toList());
        //放入map中
        filterParam.put("品牌",brandDTOS);
    }



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







## 09、规格参数过滤：页面渲染规格参数过滤条件

刷新页面，出事了：

![1576551172897](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576551172897.png) 

除了分类和品牌外，其它的规格过滤项没有正常显示出数据，为什么呢？

原因是待选项的格式不同：

![1576551525699](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576551525699.png) 

我们需要略做处理：

![1576551635876](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576551635876.png) 

修改完成之后，刷新页面：

![1576551690320](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576551690320.png) 

完美！















## 10、规格参数过滤：更多和收起按钮操作

### 1）在data中定义一个变量用于控制按钮

![1576551896824](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576551896824.png) 

### 2）给按钮绑定事件

![1576552101226](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576552101226.png) 

### 3）控制显示的数量

![1576552763576](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576552763576.png) 

### 4）测试

![3](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/3.gif) 



ok，完美搞定了！











## 11、搜索过滤筛选：将过滤条件添加到map中

当我们点击页面的过滤项，要做哪些事情？

- 把过滤条件保存在search对象中
- 监控search属性变化，如果有新的值，则发起请求，重新查询商品及过滤项
- 在页面顶部展示已选择的过滤项



### 1）保存过滤项

我们把已选择的过滤项保存在search中，因为不确定用户会选中几个，会选中什么，所以我们用一个对象（Map）来保存可能被选中的键值对：

![1576553838311](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576553838311.png) 

要注意，在created构造函数中会对search进行初始化，可能会覆盖filters的值，所以我们在created函数中把浏览器url的search部分的内容重新放入data的search参数中：

```js
created(){
    const key = ly.getUrlParam("key");
    if(!key){
        alert("请添加查询条件")
    }
    //把url参数重新转为对象
    const loSearch = ly.parse(location.search.substring(1));
    loSearch.key = key
    loSearch.page = loSearch.page || 1
    //判断过滤参数对象是否有值
    loSearch.filters = loSearch.filters || {} 
    //重新把值赋给data的查询部分
    this.search = loSearch
    //替换url的内容
    ly.replaceUrlPageParam(this.search)
    //向服务器发送查询请求
    this.loadData()
    this.loadFilterParams()
},
```





### 2）绑定事件

给所有的过滤项绑定点击事件：

![1576564554582](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576564554582.png) 

要注意，点击事件传2个参数：

- k：过滤项的名称
- option或option.id：当前过滤项的值或者id（因为分类和品牌要拿id去索引库过滤）

在点击事件中，保存过滤项到`selectedFilter`，我们加入事件：

```js
selectFilter(k, v){
    // 复制原来的search
    const {...obj} = this.search.filters
    // 添加新的属性
    obj[k] = v
    // 重新把对象给data中的filters赋值
    this.search.filters = obj
    //把参数放入url中
    ly.replaceUrlPageParam(this.search)
}
```

我们刷新页面，点击后通过浏览器功能查看`search.filter`的属性变化：

![1576576327470](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576576327470.png) 

并且，此时浏览器地址也发生了变化：

```
http://www.leyou.com/search.html?key=%E6%89%8B%E6%9C%BA&page=2&filters.%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F=Android&filters.CPU%E5%93%81%E7%89%8C=%E6%B5%B7%E6%80%9D%EF%BC%88Hisilicon%EF%BC%89&filters.CPU%E6%A0%B8%E6%95%B0=%E5%85%AB%E6%A0%B8
```









## 12、搜索过滤筛选：绑定过滤条件点击事件

条件参数都有了，接下来我们通过watch监控`search.filter`的变化，在这里向后台发送请求：

```js
watch: {
  "search.page":{
      .......................
  },
  "search.filters":{
    //如果监听的是map或者list，必须是对象的地址变了才能触发
    handler(){
        //把参数放入url中
        ly.replaceUrlPageParam(this.search)
        //当page改变的时候，向后台发送请求
        this.loadData()
        //查询过滤条件
        this.loadFilterParams()
    }
  },
},
```

我们添加完watch事件后，点击过滤参数，请求正常发出了：

![1576576684685](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576576684685.png) 







































13、搜索过滤筛选：后台拓展请求对象

既然请求已经发送到了后台，那接下来我们就在后台去添加这些条件，首先我们先来分析一下。

我们需要在请求类：`SearchRequest`中添加属性，接收过滤属性。过滤属性都是键值对格式，但是key不确定，所以用一个map来接收即可。

![1576577183196](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576577183196-1579044366829.png) 



添加完成过滤项后，我们就可以去过滤我们的查询了，过滤查询 需要我们传入多个条件，这里需要用到组合查询，接下来我们回顾一下组合查询。























14、搜索过滤筛选：回顾组合条件查询

目前，我们的基本查询是这样的：

```java
//构建查询条件
private MatchQueryBuilder buildSearchKey(SearchRequest request) {
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





















15、搜索过滤筛选：组合条件查询后台代码

接下来我们修改组合条件查询方法，方法的返回值要改为`QueryBuilder`

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

![1576580663898](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576580663898-1579044366831.png) 

总共181条，接下来，我们点击一个过滤条件：

![1576580939931](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576580939931-1579044366831.png) 

得到结果：

![1576580974950](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576580974950-1579044366831.png) 



















16、搜索过滤筛选：页面过滤项超过一个才显示

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

![1576581884144](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1576581884144-1579044366832.png) 

然后测试，ok了！



到此搜索部分我们就做完了，这些页面还有些内容没有完成，例如商品分类面包屑、取消过滤项的选择等等，大家可以课后参考讲义完成。

