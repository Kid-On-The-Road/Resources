## 01、学习目标

- 实现品牌查询功能
- 独立实现品牌新增
- 实现服务端图片上传































## 02、从零开始制作页面：添加品牌菜单

商品分类完成以后，自然轮到了品牌功能了。

先看看我们要实现的效果：

![1575269537052](图片二/1575269537052-1578195702430.png) 

接下来，我们从0开始，实现下从前端到后端的完整开发。



为了方便看到效果，我们新建一个MyBrand.vue，从0开始搭建。

![1575269627152](图片二/1575269627152-1578195702430.png) 

内容初始化一下：

```vue
<template>
  <span>
    我的品牌
  </span>
</template>

<script>
    export default {
        name: "my-brand"
    }
</script>

<style>

</style>
```

改变router新的index.js，将路由地址指向MyBrand.vue

![1575269906245](图片二/1575269906245-1578195702508.png) 

在menu.js中添加菜单：

![1575270828528](图片二/1575270828528-1578195702509.png) 

打开页面点击“我的品牌”，如下：

![1575270993683](图片二/1575270993683-1578195702519.png) 



ok，页面出来了。











## 03、从零开始制作页面：品牌列表展示



### 1）data-tables组件

大家看到这个原型页面肯定能看出，其主体就是一个table。我们去Vuetify查看有关table的文档：

![1526023540226](图片二/1526023540226-1578195702509.png)

仔细阅读，发现`v-data-table`中有以下核心属性：

- dark：是否使用黑暗色彩主题，默认是false

- expand：表格的行是否可以展开，默认是false

- headers：定义表头的数组，数组的每个元素就是一个表头信息对象，结构：

  ```js
  {
    text: string, // 表头的显示文本
    value: string, // 表头对应的每行数据的key
    align: 'left' | 'center' | 'right', // 位置
    sortable: boolean, // 是否可排序
    class: string[] | string,// 样式
    width: string,// 宽度
  }
  ```

- items：表格的数据的数组，数组的每个元素是一行数据的对象，对象的key要与表头的value一致

- loading：是否显示加载数据的进度条，默认是false

- no-data-text：当没有查询到数据时显示的提示信息，string类型，无默认值

- pagination.sync：包含分页和排序信息的对象，将其与vue实例中的属性关联，表格的分页或排序按钮被触发时，会自动将最新的分页和排序信息更新。对象结构：

  ```js
  {
      page: 1, // 当前页
      rowsPerPage: 5, // 每页大小
      sortBy: '', // 排序字段
      descending:false, // 是否降序
  }
  ```

- total-items：分页的总条数信息，number类型，无默认值

- select-all ：是否显示每一行的复选框，Boolean类型，无默认值

- value：当表格可选的时候，返回选中的行



我们向下翻，找找有没有看起来牛逼的案例。

找到这样一条：

![1526023837773](图片二/1526023837773-1578195702519.png)

其它的案例都是由Vuetify帮我们对查询到的当前页数据进行排序和分页，这显然不是我们想要的。我们希望能在服务端完成对整体品牌数据的排序和分页，而这个案例恰好合适。

点击按钮，我们直接查看源码，然后直接复制到MyBrand.vue中

模板：

```vue
<template>
  <div>
    <v-data-table
      :headers="headers"
      :items="desserts"
      :pagination.sync="pagination"
      :total-items="totalDesserts"
      :loading="loading"
      class="elevation-1"
    >
      <template slot="items" slot-scope="props">
        <td>{{ props.item.name }}</td>
        <td class="text-xs-right">{{ props.item.calories }}</td>
        <td class="text-xs-right">{{ props.item.fat }}</td>
        <td class="text-xs-right">{{ props.item.carbs }}</td>
        <td class="text-xs-right">{{ props.item.protein }}</td>
        <td class="text-xs-right">{{ props.item.iron }}</td>
      </template>
    </v-data-table>
  </div>
</template>
```



### 2）表格数据分析

接下来，就分析一下案例中每一部分是什么意思，搞清楚了，我们也可以自己玩了。

先看模板中table上的一些属性：

```vue
<v-data-table
              :headers="headers"
              :items="desserts"
              :pagination.sync="pagination"
              :total-items="totalDesserts"
              :loading="loading"
              class="elevation-1"
              >
</v-data-table>
```

- headers：表头信息，是一个数组

- items：要在表格中展示的数据，数组结构，每一个元素是一行

- search：搜索过滤字段，用不到，直接删除

- pagination.sync：分页信息，包含了当前页，每页大小，排序字段，排序方式等。加上.sync代表服务端排序，当用户点击分页条时，该对象的值会跟着变化。监控这个值，并在这个值变化时去服务端查询，即可实现页面数据动态加载了。

- total-items：总条数

- loading：boolean类型，true：代表数据正在加载，会有进度条。false：数据加载完毕。

  ![1526029254159](图片二/1526029254159-1578195702520.png)



另外，在`v-data-tables`中，我们还看到另一段代码：

```vue
<!--
遍历数据列表
slot="items"         指定要遍历的数据
slot-scope="props"   给迭代数据起个别名
-->
<template slot="items" slot-scope="props">
        <td>{{ props.item.name }}</td>
        <td class="text-xs-right">{{ props.item.calories }}</td>
        <td class="text-xs-right">{{ props.item.fat }}</td>
        <td class="text-xs-right">{{ props.item.carbs }}</td>
        <td class="text-xs-right">{{ props.item.protein }}</td>
        <td class="text-xs-right">{{ props.item.iron }}</td>
</template>
```

这段就是在渲染每一行的数据。Vue会自动遍历上面传递的`items`属性，并把得到的对象传递给这段`template`中的`props.item`属性。我们从中得到数据，渲染在页面即可。



我们需要做的事情，主要有：

- 定义表头，编写headers
  - 根据我们的表数据结构，主要是4个字段：id、name、letter、image
- 获取表内容：items 
  - 这个需要去后台查询Brand列表，可以先弄点假数据
- 获取总条数：totalItems
  - 这个也需要去后台查，先写个假的
- 定义分页对象：pagination
  - 这个值会有vuetify传给我们，我们不用管
- 数据加载进度条：loading
  - 当我们加载数据时把这个值改成true，加载完毕改成false
- 完成页面数据渲染部分 slot="items" 的那个template标签
  - 基本上把Brand的四个字段在这里渲染出来就OK了，需要跟表头（headers）对应



### 3）修改表头信息

![1575273957260](图片二/1575273957260-1578195702520.png) 

### 4）修改显示数据

数据加载流程，详见课堂画图：

![1575276433829](图片二/1575276433829-1578195702521.png) 

我们先弄点假品牌数据，可以参考数据库表：

```json
 [
     {id: 2032, name: "OPPO", image: "1.jpg", letter: "O"},
     {id: 2033, name: "飞利浦", image: "2.jpg", letter: "F"},
     {id: 2034, name: "华为", image: "3.jpg", letter: "H"},
     {id: 2036, name: "酷派", image: "4.jpg", letter: "K"},
     {id: 2037, name: "魅族", image: "5.jpg", letter: "M"}
 ]
```

品牌中有id,name,image,letter字段。

刷新页面查看：

![1526029445561](图片二/1526029445561-1578195702521.png)

注意，**我们上面提供的假数据，因此大家的页面可能看不到图片信息！**







## 04、从零开始制作页面：添加按钮和搜索框

从vuetify文档中找到按钮和搜索框，样式如下图：

![1575278930253](图片二/1575278930253-1578195702521.png) 

在table上面加入如下代码：

```html
		<v-layout row wrap>
            <v-flex xs6>
                <v-btn color="info">添加品牌</v-btn>
            </v-flex>
            <v-flex xs6>
                <v-text-field
                        label="搜索"
                        prepend-icon="search"
                ></v-text-field>
            </v-flex>
        </v-layout>
```













## 05、从零开始制作页面：页面数据分析及优化

页面我们基本已经做好了，接下来我们需要分析一下页面数据该怎么渲染。

### 1）查询条件绑定

我们在data中定义一个searchKey属性，使用v-model绑定到搜索框中

![1575280607235](图片二/1575280607235.png) 



### 2）页面数据加载分析

页面的数据已经绑定好了，我们把页面的数据优化一下，删除一些没有必要的代码，然后重新编写代码：

```vue
<template>
    <div>
        <v-layout row wrap>
            <v-flex xs6>
                <v-btn color="info">添加品牌</v-btn>
            </v-flex>
            <v-flex xs6>
                <v-text-field
                        label="搜索"
                        prepend-icon="search"
                        v-model="searchKey"
                ></v-text-field>
            </v-flex>
        </v-layout>

        <v-data-table
                :headers="headers"
                :items="brands"
                :pagination.sync="pagination"
                :total-items="totalBrands"
                :loading="loading"
                class="elevation-1"
        >
            <!--
            遍历数据列表
            slot="items"         指定要遍历的数据
            slot-scope="props"   给迭代数据起个别名
            -->
            <template slot="items" slot-scope="props">
                <td class="text-xs-center">{{ props.item.id }}</td>
                <td class="text-xs-center">{{ props.item.name }}</td>
                <td class="text-xs-center">{{ props.item.image }}</td>
                <td class="text-xs-center">{{ props.item.letter }}</td>
            </template>
        </v-data-table>
    </div>
</template>

<script>
    export default {
        data () {
            return {
                totalBrands: 0,
                brands: [],
                loading: true,
                pagination: {},
                headers: [
                    { text: '品牌编号', align: 'center', value: 'id'},
                    { text: '品牌名称', align: 'center', value: 'name' },
                    { text: '品牌logo', align: 'center', sortable:false, value: 'image' },
                    { text: '品牌字母', align: 'center', value: 'letter' }
                ],
                searchKey: ""
            }
        },
        mounted () {
            //钩子函数
            this.loadBrandsData();
        },
        methods: {
            loadBrandsData () {
                //进度条
                this.loading = true
                // 模拟延迟一段时间，随后进行赋值
                setTimeout(() => {
                    // 然后赋值给brands
                    this.brands = [
                        {id: 2032, name: "OPPO", image: "1.jpg", letter: "O"},
                        {id: 2033, name: "飞利浦", image: "2.jpg", letter: "F"},
                        {id: 2034, name: "华为", image: "3.jpg", letter: "H"},
                        {id: 2036, name: "酷派", image: "4.jpg", letter: "K"},
                        {id: 2037, name: "魅族", image: "5.jpg", letter: "M"}
                    ];
                    // 总条数暂时写死
                    this.totalBrands = 20;
                    // 完成赋值后，把加载状态赋值为false
                    this.loading = false;
                },400)
            }
        }
    }
</script>

<style>

</style>
```



### 3）分页条件分析

我们把源代码都已经删除了很多，在data中的pagination是空对象，但是我们用vue的chrome插件查看页面的数据发现，里面都已经包含了所有的分页条件，如下：

![1575283525398](图片二/1575283525398.png) 

条件如下：

```json
{"descending":false,"page":1,"rowsPerPage":5,"sortBy":"id","totalItems":0}
```

我们在代码中并没有给pagination赋值，但是发现打开页面它就自己有值了，所以是框架给我们赋的值，也就是说这些数据我们都不用关心了，我们只需要把后台的数据正确返回页面就能显示了。







## 06、从零开始制作页面：获取后台品牌数据请求

### 1）查看接口文档

![1575283960577](图片二/1575283960577.png) 

品牌列表接口

![1575284045719](图片二/1575284045719.png) 



### 2）使用axios调用接口

```js
//调用后台接口获取品牌数据
this.$http.get("/item/brand/page",{
    params: {
        key: this.searchKey,
        page: this.pagination.page,
        rows: this.pagination.rowsPerPage,
        sortBy: this.pagination.sortBy,
        desc: this.pagination.descending
    }
}).then(res=>{
    //先打印
    console.log(res)
})
```



### 3）查看浏览器控制台请求是否已经发出

![1575284918441](图片二/1575284918441.png) 

从上图可知，报了404的错，说明我们的请求已经发出了，路径和参数都有了，只是后台还没有接口而已。







## 07、从零开始制作页面：监听分页参数与查询条件

添加watch的监听事件，由于分页参数是一个对象，所以我们需要使用深度监听。

在Vue中添加watch事件

```js
watch: {
    //监听分页参数
    pagination: {
        deep: true, //深度监听，监听的是对象中属性的变化
        handler(){
            this.loadBrandsData() //属性变化就重新发送分页查询请求
        }
    },
    //监听查询条件
    searchKey: {
        handler() {
            this.loadBrandsData() //属性变化就重新发送分页查询请求
        }
    }
},
```



到此页面需要的所有东西都开发完成了，现在就差后台的数据过来了，真正的前后端开发，前端开发完成不用等待后台的，前端会自己根据api接口模拟一些数据来渲染页面，这里我们就不关注模拟数据了，我们来开发真的后台接口。



课堂代码：

![1578363914515](图片二/1578363914515.png) 

**课堂代码：**

```vue
<template>
    <div>
        <v-layout row wrap>
            <v-flex xs6>
                <!--按钮-->
                <v-btn color="success">新增品牌</v-btn>
            </v-flex>
            <v-flex xs6>
                <!--搜索框-->
                <v-text-field
                        label="搜索关键字"
                        append-icon="search"
                        v-model="searchKey"
                ></v-text-field>
            </v-flex>
        </v-layout>



        <!-- 数据列表-->
        <v-data-table
                :headers="headers"
                :items="brandItems"
                :pagination.sync="pagination"
                :total-items="totalNum"
                :loading="loading"
                class="elevation-1"
        >
            <template slot="items" slot-scope="props">
                <td class="text-xs-left">{{ props.item.id }}</td>
                <td class="text-xs-left">{{ props.item.name }}</td>
                <td class="text-xs-left">{{ props.item.image }}</td>
                <td class="text-xs-left">{{ props.item.letter }}</td>
            </template>
        </v-data-table>
    </div>
</template>

<script>
    export default {
        name: "MyBrand",
        data () {
            return {
                totalNum: 0,
                brandItems: [],
                loading: true,
                pagination: {},
                headers: [
                    {text: '品牌ID',align: 'left',sortable: true,value: 'id'},
                    {text: '品牌名称',align: 'left',sortable: true,value: 'name'},
                    {text: '品牌LOGO',align: 'left',sortable: false,value: 'image'},
                    {text: '品牌首字母',align: 'left',sortable: true,value: 'letter'}
                ],
                searchKey: ""
            }
        },
        watch: {
            // 监听查询条件
            searchKey: {
                // vue自己的属性不要使用箭头函数
                handler:  function (val, oldVal) {
                    this.getBrandItemData()
                }
            },
            // 深度监听
            pagination: {
                deep: true,
                // vue自己的属性不要使用箭头函数
                handler: function() {
                    this.getBrandItemData()
                }
            }
        },
        mounted () {
            // 给列表赋值
            this.getBrandItemData();
        },
        methods: {
            getBrandItemData(){
                setTimeout(()=>{

                    //调用后台接口去查询列表数据
                    this.$http.get("/item/brand/page",{
                        params: {
                            key: this.searchKey,
                            page: this.pagination.page,
                            rows: this.pagination.rowsPerPage,
                            sortBy: this.pagination.sortBy,
                            desc: this.pagination.descending
                        }
                    }).then(reps => {
                        // 成功后的回调
                        console.log(reps)
                        // 给列表赋值
                        // 给总记录数赋值
                    })


                    this.brandItems = [
                        {id: 2032, name: "OPPO", image: "1.jpg", letter: "O"},
                        {id: 2033, name: "飞利浦", image: "2.jpg", letter: "F"},
                        {id: 2034, name: "华为", image: "3.jpg", letter: "H"},
                        {id: 2036, name: "酷派", image: "4.jpg", letter: "K"},
                        {id: 2037, name: "魅族", image: "5.jpg", letter: "M"}
                    ]
                    // 总记录数
                    this.totalNum = 10

                    // 关闭进度条
                    this.loading = false
                },400)
            }
        }
    }
</script>

<style>

</style>
```





![1578363982509](图片二/1578363982509.png) 











## 08、编写品牌后台接口：品牌模块的准备工作1

### 1）开发步骤

```
1、创建品牌的pojo对象
2、创建品牌的dto对象
3、创建一个通用的分页对象
4、创建mapper
5、创建service
6、创建controller
```



### 2）品牌POJO对象

![1575341037866](图片二/1575341037866.png) 

```java
/**
 * 黑马程序员
 */
@Data
@Table(name = "tb_brand")
public class Brand {
    @Id
    @KeySql(useGeneratedKeys = true)
    private Long id;
    private String name;
    private String image;
    private Character letter;
    private Date createTime;
    private Date updateTime;
}
```



### 3）品牌DTO对象

```java
package com.leyou.item.dto;

/**
 * @author 黑马程序员
 */
@Data
public class BrandDTO {
    private Long id;
    private String name;
    private String image;
    private Character letter;
}
```



### 4）通用的分页对象

我们把这个对象放到 ly-common 模块的vo包中。

什么是VO？VO 全称是 View Object，视图对象，主要用于页面展示，不用来传参，只用来展示。

```java
@Data
public class PageResult<T> {
    private Long total;// 总条数
    private Integer totalPage;// 总页数
    private List<T> items;// 当前页数据

    public PageResult() {
    }

    public PageResult(Long total, List<T> items) {
        this.total = total;
        this.items = items;
    }

    public PageResult(Long total, Integer totalPage, List<T> items) {
        this.total = total;
        this.totalPage = totalPage;
        this.items = items;
    }
}
```

### 5）创建Mapper

```java
package com.leyou.item.mapper;

import com.leyou.item.entity.Brand;
import tk.mybatis.mapper.common.Mapper;

/**
 * 通用mapper简化开发
 */
public interface BrandMapper extends Mapper<Brand> {
}
```





## 09、编写品牌后台接口：品牌模块的准备工作2

由于分页查询的条件较多，我们可以从Controller开始写起，然后再写service

### 6）创建Controller

编写controller先思考四个问题，这次没有前端代码，需要我们自己来设定

- 请求方式：查询，肯定是Get
- 请求路径：分页查询，/brand/page
- 请求参数：根据我们刚才编写的页面，有分页功能，有排序功能，有搜索过滤功能，因此至少要有5个参数
- 响应结果：分页结果一般至少需要两个数据



我们编写的Controller如下：

```java
@RestController
@RequestMapping("/brand")
public class BrandController {

    @Autowired
    private BrandService brandService;

    /**
     * 分页条件查询
     */
    @GetMapping("/page")
    public ResponseEntity<PageResult<BrandDTO>> brandPageQuery(
            @RequestParam(value="key",required = false) String key,
            @RequestParam(value="page", defaultValue = "1") Integer page,
            @RequestParam(value="rows", defaultValue = "5") String rows,
            @RequestParam(value="sortBy",defaultValue = "id") String sortBy,
            @RequestParam(value="desc", defaultValue = "false") String desc
    ){
        //调用业务层方法
        PageResult<BrandDTO> result = brandService.brandPageQuery(key, page, rows, sortBy, desc);
        //返回页面
        return ResponseEntity.ok(result);
    }
}
```



### 7）创建Service

```java
/**
 * 业务层代码
 */
@Service
@Transactional
public class BrandService {

    @Autowired
    private BrandMapper brandMapper;


    public PageResult<BrandDTO> brandPageQuery(String key, Integer page, Integer rows, String sortBy, Boolean desc) {
        //设置分页条件
        PageHelper.startPage(page, rows);
        //设置封装总对象
        Example example = new Example(Brand.class);
        //封装条件对象
        if(StringUtils.isNotBlank(key)){
            Example.Criteria criteria = example.createCriteria();
            criteria.orLike("name","%"+key+"%");
            criteria.orEqualTo("letter", key.toUpperCase());
        }
        //封装排序，使用的是原始的sql语句order by id desc
        example.setOrderByClause(sortBy+" "+(desc ? "DESC" : "ASC"));
        //分页查询
        List<Brand> brands = brandMapper.selectByExample(example);
        //判空
        if(CollectionUtils.isEmpty(brands)){
            throw new LyException(ExceptionEnum.BRAND_NOT_FOUND);
        }
        //得到PageHelper的分页对象
        PageInfo<Brand> pageInfo = new PageInfo<>(brands);
        //得到总记录数
        long total = pageInfo.getTotal();
        //得到BrandDTO的集合
        List<BrandDTO> brandDTOS = BeanHelper.copyWithCollection(pageInfo.getList(), BrandDTO.class);
        //创建自己的分页对象
        PageResult<BrandDTO> pageResult = new PageResult<>(total, brandDTOS);
        //返回
        return pageResult;
    }
}
```







## 10、品牌查询：前后端测试及前端完整代码

重启微服务，然后在页面测试，就可以看到效果了：

![1575362674988](图片二/1575362674988.png) 

注意，图片显示的地方需要修改一下，否则图片显示不了

```html
<img :src="props.item.image ">
```



完整前端页面代码：

```vue
<template>
    <div>
        <v-layout row wrap>
            <v-flex xs6>
                <v-btn color="info">添加品牌</v-btn>
            </v-flex>
            <v-flex xs6>
                <v-text-field
                        label="搜索"
                        prepend-icon="search"
                        v-model="searchKey"
                ></v-text-field>
            </v-flex>
        </v-layout>

        <v-data-table
                :headers="headers"
                :items="brands"
                :pagination.sync="pagination"
                :total-items="totalBrands"
                :loading="loading"
                class="elevation-1"
        >
            <!--
            遍历数据列表
            slot="items"         指定要遍历的数据
            slot-scope="props"   给迭代数据起个别名
            -->
            <template slot="items" slot-scope="props">
                <td class="text-xs-center">{{ props.item.id }}</td>
                <td class="text-xs-center">{{ props.item.name }}</td>
                <td class="text-xs-center"> <img :src="props.item.image "></td>
                <td class="text-xs-center">{{ props.item.letter }}</td>
            </template>
        </v-data-table>
    </div>
</template>

<script>
    export default {
        data () {
            return {
                totalBrands: 0,
                brands: [],
                loading: true,
                pagination: {},
                headers: [
                    { text: '品牌编号', align: 'center', value: 'id'},
                    { text: '品牌名称', align: 'center', value: 'name' },
                    { text: '品牌logo', align: 'center',sortable: false, value: 'image' },
                    { text: '品牌字母', align: 'center', value: 'letter' }
                ],
                searchKey: "",
                dialog: false, //弹窗
                msg: "" //提示信息
            }
        },
        mounted () {
            //钩子函数
            this.loadBrandsData();
        },
        watch: {
            //监听分页参数
            pagination: {
                deep: true, //深度监听，监听的是对象中属性的变化
                handler(){
                    this.loadBrandsData() //属性变化就重新发送分页查询请求
                }
            },
            //监听查询条件
            searchKey: {
                handler() {
                    this.loadBrandsData() //属性变化就重新发送分页查询请求
                }
            }
        },
        methods: {
            loadBrandsData () {
                //进度条
                this.loading = true
                // 模拟延迟一段时间，随后进行赋值
                setTimeout(() => {
                    //调用后台接口获取品牌数据
                    this.$http.get("/item/brand/page",{
                        params: {
                            key: this.searchKey,
                            page: this.pagination.page,
                            rows: this.pagination.rowsPerPage,
                            sortBy: this.pagination.sortBy,
                            desc: this.pagination.descending
                        }
                    }).then(res=>{
                        console.log(res.data)
                        //给列表赋值
                        this.brands = res.data.items
                        //给总记录数赋值
                        this.totalBrands = res.data.total
                    })
                    // 完成赋值后，把加载状态赋值为false
                    this.loading = false;
                },400)
            }
        }
    }
</script>

<style>

</style>
```

品牌分页查询到此做完了！











## 11、新增品牌：思路分析

品牌查询已经做好了，那么新增品牌我们直接在Brand.vue文件中开发即可。

![1575365293099](图片二/1575365293099.png) 

品牌数据如何添加？ 除了自己的数据，还有无其他数据？

之前分析过品牌表和分类表是多对多，分类的数据，一般变动的比较少，所以中间表我们一般让变动比较多的品牌表来维护，也就是说我们添加完品牌数据，还需要在中间表添加数据。

![1575365502869](图片二/1575365502869.png) 



```
外键：
	逻辑外键：通过代码维护关系，
		优点：便于迁移
		缺点：出现脏数据
	物理外键：数据库维护关系，
		优点：数据完整
		缺点：不方便删除
		
```









除了中间表的数据之外，还有一个特殊的数据，就是图片了：

![1575365648975](图片二/1575365648975.png) 

填写基本信息，此时，点击提交按钮，可以看到页面已经发出请求：

![1552139565844](图片二/1552139565844.png) 





## 12、新增品牌：完成品牌的保存

### 1）Controller

还是一样，先分析四个内容：

- 请求方式：刚才看到了是POST
- 请求路径：/brand
- 请求参数：brand对象的三个属性，可以用BrandDTO接收，外加商品分类的id数组cids
- 返回值：无

代码：

```java
/**
 * 新增品牌
 * @param brand
 * @return
 */
@PostMapping
public ResponseEntity<Void> saveBrand(BrandDTO brand, @RequestParam("cids") List<Long> ids) {
    brandService.saveBrand(brand, ids);
    return ResponseEntity.status(HttpStatus.CREATED).build();
}
```



### 2）Service

这里要注意，我们不仅要新增品牌，还要维护品牌和商品分类的中间表。

```java
@Transactional
public void saveBrand(BrandDTO brandDTO, List<Long> ids) {
    // 新增品牌
    Brand brand = BeanHelper.copyProperties(brandDTO, Brand.class);
    brand.setId(null);
    int count = brandMapper.insertSelective(brand);
    if(count != 1){
        // 新增失败，抛出异常
        throw new LyException(ExceptionEnum.INSERT_OPERATION_FAIL);
    }
    // 新增品牌和分类中间表
    count = brandMapper.insertCategoryBrand(brand.getId(), ids);
    if(count != ids.size()){
        // 新增失败，抛出异常
        throw new LyException(ExceptionEnum.INSERT_OPERATION_FAIL);
    }
}
```

这里调用了brandMapper中的一个自定义方法，来实现中间表的数据新增



### 3）Mapper

通用Mapper只能处理单表，也就是Brand的数据，因此我们手动编写一个方法及sql，实现中间表的新增：

```java
package com.leyou.item.mapper;

import com.leyou.item.entity.Brand;
import org.apache.ibatis.annotations.Param;
import tk.mybatis.mapper.common.Mapper;

import java.util.List;

/**
 * @author 黑马程序员
 */
public interface BrandMapper extends Mapper<Brand> {
    /**
     * 新增商品分类和品牌中间表数据
     * @param ids 商品分类id集合
     * @param bid 品牌id
     * @return 新增的条数
     */
    int insertCategoryBrand(@Param("bid") Long bid, @Param("ids") List<Long> ids);
}

```

在resource下新建一个目录：`mappers`，并在下面新建文件：`BrandMapper.xml`

 ![1552143745932](图片二/1552143745932.png) 

然后在`application.yml`文件中配置mapper文件的地址：

```yaml
mybatis:
  type-aliases-package: com.leyou.item.entity
  configuration:
    map-underscore-to-camel-case: true
  mapper-locations: mappers/*.xml
```

在`BrandMapper.xml`中定义Sql的statement：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.leyou.item.mapper.BrandMapper">

    <insert id="insertCategoryBrand">
        INSERT INTO tb_category_brand (category_id, brand_id) VALUES 
        <foreach collection="ids" item="id" separator=",">
            (#{id}, #{bid})
        </foreach>
    </insert>
</mapper>

```



编写完成代码后，重启微服务，测试查看品牌和中间表的数据是否已经插入即可！















## 13、图片上传：上传文件插件说明

**回顾页面上传图片三要素：**

1）必须是post请求

2）必须是多部件类型form表单

3）必须有一个type=“file”文件上传项











这三要素指的是form表单，我们的vue中没有这个表单，所以我们换一种方式，也就是做一个Vue的文件上传插件来实现文件上传，但是如果我们自己去开发这个插件，那会浪费很多时间，所以我们找一个现成的，直接放到项目中就可以用了，我们只需要看懂即可，关于这个文件上传插件的开发大家可以参考这个文件：

![1575424402339](图片二/1575424402339.png) 

看这个文章的这一项：

![1575424714898](图片二/1575424714898.png) 



有兴趣的同学可以看看，但我们的重点在微服务中。











## 14、图片上传：图片服务器与上传微服务搭建



### 1）图片服务器

图片服务器我们可以选择tomcat，也可以选择nginx，如何选择他们？

那就看谁处理静态资源更加好了，从这方面来看，nginx肯定是首选了，所以我们去nginx下的html目录下创建一个brand-img的文件夹，然后把图片上传到该目录即可，接下来我们就开始来创建图片上传的微服务。



刚才的新增实现中，我们并没有上传图片，接下来我们一起完成图片上传逻辑。

文件的上传并不只是在品牌管理中有需求，以后的其它服务也可能需要，因此我们创建一个独立的微服务，专门处理各种上传。







### 2）创建module

![1552144636424](图片二/1552144636424.png)



### 3）添加依赖

我们需要EurekaClient和web依赖：

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

    <artifactId>ly-upload</artifactId>

    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>com.leyou</groupId>
            <artifactId>ly-common</artifactId>
            <version>1.0-SNAPSHOT</version>
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



### 4）编写配置

```yaml
server:
  port: 8082
spring:
  application:
    name: upload-service
  servlet:
    multipart:
      max-file-size: 5MB # 限制文件上传的大小
# Eureka
eureka:
  client:
    service-url:
      defaultZone: http://127.0.0.1:10086/eureka
  instance:
    prefer-ip-address: true
```

需要注意的是，我们应该添加了限制文件大小的配置





### 5）启动类

```java
@SpringBootApplication
@EnableDiscoveryClient
public class LyUploadApplication {
    public static void main(String[] args) {
        SpringApplication.run(LyUploadApplication.class, args);
    }
}

```

结构：

![1552144792255](图片二/1552144792255.png) 











## 15、图片上传：上传功能初步实现

### 1）Controller

点击新增品牌页面的上传图片按钮，即可看到上传图片请求：

![1552144440922](图片二/1552144440922.png)



从图中很容易看出编写controller需要知道4个内容：

- 请求方式：上传肯定是POST
- 请求路径：/upload/image
- 请求参数：文件，参数名是file，SpringMVC会封装为一个接口：MultipleFile
- 返回结果：这里上传与表单分离，文件不是跟随表单一起提交，而是单独上传并得到结果，随后把结果跟随表单提交。因此上传成功后需要返回一个可以访问的文件的url路径

代码如下：

```java
@RestController
public class UploadController {

    @Autowired
    private UploadService uploadService;

    /**
     * 上传图片功能
     * @param file
     * @return
     */
    @PostMapping("/image")
    public ResponseEntity<String> uploadImage(@RequestParam("file") MultipartFile file) {
        // 返回200，并且携带url路径
        return ResponseEntity.ok(uploadService.upload(file));
    }
}
```



### 2）Service

完成文件上传：

```java
@Service
public class UploadService {

    //图片上传的位置
    private static final String IMAGE_PATH = "D:\\leyou\\software\\nginx-1.16.0\\html\\brand-img\\";
    //返回的图片路径中拼接上的前缀
    private static final String IMAGE_URL = "http://localhost/brand-img/";
    
    /**
     * 文件上传
     */
    public String upload(MultipartFile file) {
        //得到文件上传的文件对象：路径
        File imagePathFile = new File(IMAGE_PATH);
        //指定上传文件的名称
        String imageName = UUID.randomUUID() + file.getOriginalFilename();
        //本地上传文件
        //参数一：上传到那个路径
        //参数二：图片名称是什么
        try {
            file.transferTo(new File(imagePathFile, imageName));
        } catch (IOException e) {
            throw new LyException(ExceptionEnum.FILE_UPLOAD_ERROR);
        }
        //返回图片路径
        return IMAGE_URL+imageName;
    }
}
```



### 3）修改网关路由配置

在spring.cloud.gateway.routes下添加一个路由映射

```yml
        - id: upload-service
          uri: lb://upload-service
          predicates:
            - Path=/api/upload/**
          filters:
            - StripPrefix=2
```



### 4）测试上传

启动上传文件微服务和网关微服务，然后打开页面测试：

![1575428085329](图片二/1575428085329.png) 

图片回显路径：

![1575428303757](图片二/1575428303757.png) 

由此可见我们的文件上传功能基本实现了。







## 16、课程总结

![1578373354087](图片二/1578373354087.png) 



















