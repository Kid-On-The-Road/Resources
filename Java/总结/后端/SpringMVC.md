# SpringMVC

## 介绍

- SpringMVC是 **web应用** 的一个Java平台的 **开源框架**
- SpringMVC是Spring **基于MVC设计模式** 实现的 **轻量级** 框架

## 优点

| 优点                     | 详情                                                         |
| ------------------------ | :----------------------------------------------------------- |
| 清晰的角色划分           | 前端控制器（ DispatcherServlet） <br/>处理器映射器（ HandlerMapping） <br/>处理器适配器（ HandlerAdapter） <br/>视图解析器（ ViewResolver） <br/>处理器或页面控制器（ Controller） <br/>验证器（ Validator） <br/>命令对象（ Command 请求参数绑定到的对象就叫命令对象）<br/>表单对象（ Form Object 提供给表单展示和表单提交的对象） |
| 可扩展性好               | 可以很容易扩展，虽然几乎不需要                               |
| 与Spring 框架无缝集成    | 这是其它web框架不具备的                                      |
| 可适配性好               | 通过 HandlerAdapter 可以支持任意的类作为处理器               |
| 可定制性好               | 提供从最简单的URL映射，到复杂的、专用的定制策略              |
| 单元测试方便             | 利用 Spring 提供的 Mock 对象能够非常简单的进行 Web 层单元测试 |
| 本地化、主题的解析的支持 | 支持包括诸如数据绑定和主题(theme)之类的许多功能              |
| jsp标签库                | 强大的 JSP 标签库，使 JSP 编写更容易                         |

## 案例

- web.xml

```xml
<!-- 任何需要tomcat容器启动的框架都需要在这里配置 -->
	<!-- 1. 定义servlet映射路径 -->
	<servlet-mapping>
		<servlet-name>mvc</servlet-name>
		<url-pattern>*.do</url-pattern>
	</servlet-mapping>
	<!-- 2. 创建mvc前端控制器 -->
	<servlet>
		<servlet-name>mvc</servlet-name>
		<!-- 前端控制器: 处理所有的用户请求 -->
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:springMVC.xml</param-value>
		</init-param>
		<!-- 数字越小, 加载顺序越早 -->
		<load-on-startup>1</load-on-startup>
	</servlet>
```

- springMVC.xml

```xml
  <!-- 1. 添加Spring组件扫描配置 -->
    <context:component-scan base-package="com.itheima.demo"/>
    <!-- 2. 创建处理器映射器+处理器适配器 -->
    <mvc:annotation-driven/>
    <!-- <mvc:annotation-driven/> : 包含以下注册的bean -->
    <!--
    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/>
    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"/>
    -->
    <!-- 3. 创建视图解析器 -->
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/pages/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
```

- HelloController

```java
@Controller // 不能使用Spring的注解创建对象: 不会处理任何请求
public class HelloController {
    /**
     * 当用户发起请求, 该方法将会处理(执行)
     * http://localhost:8080/hello.do
     *
     * @return 是视图名称 (页面)
     */
    @RequestMapping("hello.do")
    public String hello(Model model){
        return "success";
    }
}
```

- pages/success.jsp

```html
<%--
  Created by IntelliJ IDEA.
  User: Jason
  Date: 2019/11/24
  Time: 9:53
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>OK</title>
</head>
<body>
    操作成功! ${msg}
</body>
</html>
```

### SpringMVC的工作流程

![1563517010381](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springMVC一/1563517010381.png)

## SpringMVC组件

### 基本组件

| 组件名     | 解释                                    | 作用                             |
| ---------- | --------------------------------------- | -------------------------------- |
| 前端控制器 | DispatcherServlet/前端控制器/核心控制器 | 用于接收并分发从浏览器发起的请求 |
| 后端控制器 | Controller/处理器/后端控制器            | 表现层处理用户请求的对象         |
| 视图       | View/视图                               | 描述响应内容的对象               |

###  三大组件

| 组件名       | 解释                        | 作用                   |
| ------------ | --------------------------- | ---------------------- |
| 处理器映射器 | HandlerMapping/处理器映射器 | 匹配处理器的执行方法   |
| 处理器适配器 | HandlerAdapter/处理器适配器 | 适配调用处理器的方法   |
| 视图解析器   | ViewResolver/视图解析器     | 根据名称解析出视图对象 |

## 源码分析

1. 用户发送请求至前端控制器DispatcherServlet 
2. DispatcherServlet收到请求调用HandlerMapping处理器映射器。 
3. HandlerMapping根据请求URI找到具体的处理器， 生成处理器对象及处理器拦
   截器(如果有则生成)一并返回给DispatcherServlet。 
4. DispatcherServlet通过HandlerAdapter处理器适配器调用处理器 
5. 执行处理器(Controller， 也叫后端控制器)。 
6. Controller执行完成返回ModelAndView 
7. HandlerAdapter将controller执行结果ModelAndView返回给
   DispatcherServlet 
8. DispatcherServlet将ModelAndView传给ViewReslover视图解析器 
9. ViewReslover解析后返回具体View
10. DispatcherServlet对View进行渲染视图（ 即将模型数据填充至视图中） 。
11. DispatcherServlet响应用户 (实际上是视图做的跳转(转发,重定向等..))

## @RequestMapping

### 作用

映射与限制请求

- 映射请求地址

```Java
    /**
     * @RequestMapping: 映射用户的请求路径
     *  处理相应的用户请求
     *      value: 映射的路径  .do可以省略
     *
     */
    @RequestMapping("list")
    public String list(){
        System.out.println("执行了list方法...");
        return "success";
    }
```

- 限制请求方法

```java
    /**
     * method: 限制GET之外的方式请求
     */
    @RequestMapping(value = "list2", method = {RequestMethod.GET})
    public String list2(){
        System.out.println("执行了list方法...");
        return "success";
    }
```

```html
    <h3>演示: 限制POST请求</h3>
    <form method="post" action="list2.do">
        <input type="text" name="id" value="1"/>
        <input type="submit">
    </form>
```

- 根路径映射限制

```Java
/**
 * 控制器.
 * @RequestMapping: 修饰类, 方法
 *  作用: 映射指定的路径, 或者限制匹配的方法
 *      如果写在类上, 表示类中所有的路径都应该加上此根路径 /hello/hello.do
 */
@Controller // 不能使用Spring的注解创建对象: 不会处理任何请求
@RequestMapping("hello")
public class HelloController {
    /**
     * 当用户发起请求, 该方法将会处理(执行)
     * http://localhost:8080/hello.do
     *
     * @return 是视图名称 (页面)
     */
    @RequestMapping("hello.do")
    public String hello(Model model){
        return "success";
    }
    /**
     * @RequestMapping: 映射用户的请求路径
     *  处理相应的用户请求
     *      value: 映射的路径  .do可以省略
     *
     */
    @RequestMapping("list")
    public String list(){
        System.out.println("执行了list方法...");
        return "success";
    }
    /**
     * method: 限制GET之外的方式请求
     */
    @RequestMapping(value = "list2", method = {RequestMethod.GET})
    public String list2(){
        System.out.println("执行了list方法...");
        return "success";
    }
}
```

## 参数绑定

###  解决参数的中文乱码

- web.xml

```xml
<!-- 1. 映射过滤的路径 -->
	<filter-mapping>
		<filter-name>encoding</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
	<!-- 2. 定义过滤器 -->
	<filter>
		<filter-name>encoding</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
	</filter>
```

### 基本类型

不建议使用int(基本类型)接收参数,因为基本数据类型没有默认值, 所以变成了必传参数, 可能会报错

```java
/**
     * 演示: 基本参数绑定
     * @param id 参数名称
     * @param name 参数名称
     * @return 视图
     */
    @RequestMapping("list3")
    public String list3(Integer id, String name){
        System.out.println(id);
        System.out.println(name);
        return "success";
    }
```

```html
<h3>演示: 限制POST请求</h3>
    <form method="post" action="list2.do">
        <input type="text" name="id" value="1"/>
        <input type="submit">
    </form>
    <h3>演示: 参数绑定乱码</h3>
    <form method="post" action="list3.do">
        <input type="text" name="id" value="1"/>
        <input type="text" name="name" value="丽晓"/>
        <input type="submit">
    </form>
```

### 对象类型

使用对象接收参数可以使参数列表更简洁

- 对象类型

```Java
   /**
     * 演示: 对象参数绑定
     * @param account 参数名称
     * @return 视图
     */
    @RequestMapping("list4")
    public String list4(Account account){
        System.out.println(account);
        return "success";
    }
```

- 嵌套类型

```Java
public class Account {
  private Integer id;
  private Integer uid;
  private Double money;
  private User user;
```

### 默认支持

```Java
/**
     * 演示: 绑定Servlet原生的API (servlet-api: jar)
     * @param req 请求对象
     * @param res 响应对象
     * @param session 会话对象
     * @return 视图
     */
    @RequestMapping("list5")
    public String list5(HttpServletRequest req, HttpServletResponse res, HttpSession session){
        req.setAttribute("msg", "你好啊~");
        return "success";
    }
    /**
     * 演示: 默认支持的参数类型
     * @param model 请求与参数集合
     * @param modelMap 请求与参数集合 (同上)
     * @return 视图
     */
    @RequestMapping("list6")
    public String list6(Model model, ModelMap modelMap){
        // 往请求域中添加参数
        modelMap.addAttribute("msg", "你好呀~");
        return "success";
    }
```

### 自定义参数转换器

- 定义转换器: DateConverter

```Java
public class DateConverter implements Converter<String, Date> {
    /**
     * 参数转换器
     * @param s 请求参数
     * @return 转换结果
     */
    @Override
    public Date convert(String s) {
        System.out.println(s);
        try {
            return new SimpleDateFormat("yyyy-MM-dd").parse(s);
        } catch (ParseException e) {
            return null;
        }
    }
}
```

- 配置转换器: springMVC.xml

```xml
<!-- 2. 创建处理器映射器+处理器适配器 -->
<mvc:annotation-driven conversion-service="conversionServiceFactoryBean"/>
<!-- 定义转换服务工厂对象 -->
<bean id="conversionServiceFactoryBean" class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
    <property name="converters">
        <!-- 时间转换器 -->
        <bean class="com.itheima.demo.DateConverter"/>
        <!-- 货币转换器 -->
    </property>
</bean>
```

## 高级参数绑定

### 数组

```java
@Controller
public class Param2Controller {
    /**
     * 演示: 数组绑定
     * @param ids 数组名称 (参数名称)
     */
    @RequestMapping("list7")
    public String list7(Integer[] ids, Model model){
        System.out.println(ids);
        return "success";
    }
}
```

```jsp
 <h3>演示: 数组参数绑定</h3>
    <form method="post" action="list7.do">
        <input type="text" name="ids" value="1"/>
        <input type="text" name="ids" value="2"/>
        <input type="text" name="ids" value="3"/>
        <input type="submit">
    </form>
```

### 集合

#### 直接绑定的集合

```java
/**
     * 演示: 集合绑定 (官方不支持直接绑定)
     * @param ids 集合名称 (参数名称)
     *            @RequestParam: 修饰参数列表
     *              required:默认ｔｒｕｅ：表示参数必须传递
     *                  false: 可以不传
     */
@RequestMapping("list8")
public String list8(@RequestParam(required = false) List<Integer> ids, Model model){
    System.out.println(ids);
    return "success";
}
```

```jsp
<h3>演示: 集合参数绑定</h3>
<form method="post" action="list8.do">
    <input type="text" name="ids" value="2"/>
    <input type="text" name="ids" value="3"/>
    <input type="submit">
</form>
```

#### 嵌套绑定的集合

```java
@RequestMapping("list9")
public String list9(Account account, Model model){
    System.out.println(account);
    model.addAttribute("msg", account);
    return "success";
}  
```

```jsp
<h3>演示: 集合参数绑定</h3>
    <form method="post" action="list8.do">
        <input type="text" name="ids" value="2"/>
        <input type="text" name="ids" value="3"/>
        <input type="submit">
    </form>

    <h3>演示: 嵌套集合绑定</h3>
    <form method="post" action="list9.do">
        <input type="text" name="id" value="9"/>
        <input type="text" name="users[0].id" value="9"/>
        <input type="text" name="users[0].username" value="Jason9"/>
        <input type="text" name="users[1].username" value="Jason10"/>
        <input type="text" name="users[1].id" value="10"/>
        <input type="submit">
    </form>
```

## 返回值

### void

#### 意义: 

SpringMVC不响应任何内容(因为已经调用原生的API响应了)

#### 用法

- 转发

```Java
    /**
     * void: 表示SpringMVC框架不响应任何内容
     *
     * @param req 原生的请求对象  (需要通过API来响应)
     * @param res
     */
    @RequestMapping("void1")
    public void void1(HttpServletRequest req, HttpServletResponse res) throws Exception {
        System.out.println("访问了void1方法..");
        req.setAttribute("msg", "你好哦");
        RequestDispatcher rd = req.getRequestDispatcher("/pages/success.jsp");
        rd.forward(req, res);
    }
```

- 重定向

```Java
@RequestMapping("void2")
public void void2(HttpServletRequest req, HttpServletResponse res) throws Exception {
    System.out.println("访问了void1方法..");
    res.sendRedirect("/pages/success.jsp");
}
```

- 响应数据

```Java
    /**
     * void: 表示SpringMVC框架不响应任何内容
     *
     * @param req 原生的请求对象  (需要通过API来响应)
     * @param res
     */
    @RequestMapping("void1")
    public void void1(HttpServletRequest req, HttpServletResponse res) throws Exception {
        System.out.println("访问了void1方法..");
        req.setAttribute("msg", "你好哦");
        RequestDispatcher rd = req.getRequestDispatcher("/pages/success.jsp");
        rd.forward(req, res);
    }
```

### string

- 转发

```java
/**
     * 演示: str的返回值用法
     * @return forward: 表示转发指定的路径资源
     */
@RequestMapping("str")
public String str() {
    System.out.println("已经访问了str方法..");
    return "forward:/static/list.jsp";
}
```

- 重定向

```java
    /**
     *
     * 演示: str的返回值用法
     * @return redirect: 表示重定向指定的路径资源
     */
    @RequestMapping("str2")
    public String str2() {
        System.out.println("已经访问了str2方法..");
        return "redirect:/static/list.jsp";
    }
```

- 响应数据

```java
    /**
     * 演示: str的返回值用法
     * @return forward: 表示转发指定的路径资源
     */
    @RequestMapping("str")
    public String str(Model model) {
        model.addAttribute("msg","说点什么呢");
        System.out.println("已经访问了str方法..");
//        return "forward:/static/list.jsp";
        return "forward:/pages/success.jsp";
    }
```

### MV

```java
/**
     * 演示: ModelAndView mv的返回值用法
     * @return
     */
@RequestMapping("mv")
public ModelAndView mv() {
    ModelAndView mv = new ModelAndView();
    mv.addObject("msg", "嘿嘿");
    mv.setViewName("success");
    return mv;
}
```

## 数据交互

### json

```xml
  <!-- 2.  添加jackson依赖 -->
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <!-- 版本: 练习时必须一致 -->
      <version>2.9.9</version>
    </dependency>
```

```java
  /**
     * 演示: json交互
     * @RequestBody: 修饰参数列表
     *  作用: 将请求体中的json字符串转换成java对象
     *      required: 默认是true: 表示请求体中必须含有json字符串数据
     *          false: 表示, 可以请求空参的内容
     * @ResponseBody: 修饰方法
     *  作用: 将方法的返回值(对象)转换成json字符串
     */
    @RequestMapping("json")
    @ResponseBody
    public Object json(@RequestBody(required = true) Account account, Model model){
        System.out.println("执行了json方法..");
        model.addAttribute("msg", account);
//        return "success";
        return account;
    }
```

```jsp
<h3>演示: json数据交互</h3>
<form id="jsonForm">
    <input type="text" name="id" value="1" />
    <input type="text" name="uid" value="2" />
    <button type="button" onclick="jsonCommit()">提交</button>
    <div id="jsonRes"></div>
</form>
<script>
	function jsonCommit() {
        // 1. 创建异步请求对象
        var req = new XMLHttpRequest();
        // 2. 打开请求
        req.open("POST", "json", true);
        // 3. 设置请求的数据类型(必须)
        req.setRequestHeader("Content-Type", "application/json;charset=UTF-8");
        // 4. 异步读取响应
        req.onreadystatechange = function () {
            // 2.1 判断响应状态
            if (req.readyState == 4 || req.status == 200) {
                var data = req.responseText;
                document.getElementById("jsonRes").innerHTML = data;
            }
        };
        // 5. 封装数据
        var es =
            document.getElementById("jsonForm").getElementsByTagName("input");
        var json = {};
        for (var i = 0; i <= es.length; i++) {
            if (es[i] != undefined) {
                var name = es[i].name;
                var value = es[i].value;
                json[name] = value;
            }
        }
        // 6. 发送数据
        var body = JSON.stringify(json);
        req.send(body);
    }
</script>
```

### RESTful风格

#### 介绍

- REST全称是Representational State Transfer，REST本身并没有创造新的技术、组件或服务。
- REST指的是约束条件和原则，如果一个架构符合REST的约束条件和原则，就称它为RESTful架构。

#### 风格对比

|             | 增        | 删                | 查             | 改                |
| ----------- | --------- | ----------------- | -------------- | ----------------- |
| 传统        | /user/add | /user/delete?id=1 | /user/get?id=1 | /user/update?id=1 |
| **RESTful** | /user     | /user/1           | /user/1        | /user/1           |

#### RESTful参数

```java
/**
 * 演示: RESTful风格.
 *  1. 可以从URI当中传递参数或则获取参数 (使用@PathVariable获取URI中的参数)
 *  2. 
 *
 * @author : Jason.lee
 * @version : 1.0
 */
@Controller
public class RestController {
    /**
     * 根据ID查询用户
     * @param id 用户编号
     * @param model 请求域集合
     * @return 视图
     */
    @RequestMapping("user/{id}")
    public String list(@PathVariable Integer id, Model model){
        System.out.println(id);
        model.addAttribute("msg", id);
        return "success";
    }
}
```

#### RESTful支持

- web.xml

```xml
<!-- 1. 映射过滤器路径资源 -->
<filter-mapping>
    <filter-name>rest</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
<!-- 2. 定义隐藏域转换RESTful风格方法的过滤器 -->
<filter>
    <filter-name>rest</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
```

- RestController

```java
/**
     * @param id
     * @param account
     * @param model
     * @Description: 修改方法
     * @Author: guo shu tao
     * @Date: 2021/9/29  8:39
     * @Return java.lang.String
     */
    @RequestMapping(value = "/{id}", method = RequestMethod.PUT)
    public String put(@PathVariable Integer id, Account account, Model model) {
        System.out.println("修改方法执行");
        System.out.println(id);
        account.setId(id);
        model.addAttribute("msg", account);
        return "success";
    }
    /**
     * @param id
     * @param account
     * @param model
     * @Description: 删除方法执行
     * @Author: guo shu tao
     * @Date: 2021/9/29  8:41
     * @Return java.lang.String
     */
    @RequestMapping(value = "/{id}", method = RequestMethod.DELETE)
    public String delete(@PathVariable Integer id, Account account, Model model) {
        System.out.println("删除方法执行");
        System.out.println(id);
        account.setId(id);
        model.addAttribute("msg", account);
        return "success";
    }
```

- index.jsp

```jsp
<h3>演示: REST新增方法</h3>
<form action="user" method="post">
    <input type="text" name="id" value="1" />
    <input type="text" name="uid" value="2" />
    <input type="text" name="money" value="2" />
    <input type="submit" />
</form>
<h3>演示: REST修改方法*</h3>
<form action="user/1" method="post">
    <input type="hidden" name="_method" value="PUT">
    <input type="text" name="id" value="1" />
    <input type="text" name="uid" value="2" />
    <input type="text" name="money" value="2" />
    <input type="submit" />
</form>
<h3>演示: REST删除方法*</h3>
<form action="user/1" method="post">
    <input type="hidden" name="_method" value="DELETE">
    <input type="text" name="id" value="1" />
    <input type="text" name="uid" value="2" />
    <input type="text" name="money" value="2" />
    <input type="submit" />
</form>
<h3>演示: REST查询方法</h3>
<form action="user/1" method="get">
    <input type="submit" />
</form>
```

## 异常处理

- 代码调用关系

![1567403737180](E:\SyncData\WeiyunSync\Program\Study\Note\Java\笔记图片/springMVC二/1567403737180.png)

- 模拟视图层异常: ExceptionController

```java
 /**
     * 异常处理: 第一种方案
     */
    @RequestMapping("ex")
    public String list(Model model){
        try {
            // 1. 参数处理

            // 2. 调用业务层代码
            // try {
            //      service.save()
            // }

            int i = 1/0;
            return "success";
        } catch (Exception e) {
            e.printStackTrace();
            model.addAttribute("msg", e.getMessage());
            return "error";
        }
    }
```

- 成功页面: pages/success.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>OK</title>
</head>
<body>
    操作成功 !
</body>
</html>
```

- 错误页面: pages/error.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Fail</title>
</head>
<body>
    操作失败 !${msg}
</body>
</html>
```

#### 代码处理

```java 
@RequestMapping("list")
public String list(Model model) {
    try {
        int i = 1 / 0;
        // service.method();
    } catch (Exception e) {
        model.addAttribute("msg", e.getMessage());
        return "error";
    }
    return "success";
}
```

#### 过滤器处理

```java
public class ExceptionFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("初始化完成..");
    }
    /**
     * 放行: 将接受到的请求通过, 交给控制器执行
     */
    @Override
    public void doFilter(ServletRequest req, ServletResponse res,
                         FilterChain chain) throws IOException, ServletException {
        try {
            // 会执行处理器方法
            chain.doFilter(req, res);
        } catch (Exception e) {
            e.printStackTrace();
            req.setAttribute("msg", e.getMessage());
            RequestDispatcher rd = req.getRequestDispatcher("/pages/error.jsp");
            rd.forward(req, res);
        }
    }
    @Override
    public void destroy() {
        System.out.println("即将销毁..");
    }
}
```

- web.xml

```xml
<!-- 1. 映射过滤器路径资源 -->
<filter-mapping>
    <filter-name>ex</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
<!-- 2. 定义异常处理过滤器 -->
<filter>
    <filter-name>ex</filter-name>
    <filter-class>com.itheima.json.filter.ExceptionFilter</filter-class>
</filter>
```

#### 统一异常处理

```java
/**
 * 异常处理器.
 *  1. 实现HandlerExceptionResolver接口和方法
 *  2. 添加到容器中: @Component
 *
 * @author : Jason.lee
 * @version : 1.0
 */
@Component
public class ExceptionHandler implements HandlerExceptionResolver {
    /**
     * DispatcherServlet内部调用该方法处理异常
     * @param req 请求对象
     * @param res 响应对象
     * @param o 控制器对象
     * @param e 异常对象
     * @return 视图+模型
     */
    @Override
    public ModelAndView resolveException(HttpServletRequest req,
                                         HttpServletResponse res,
                                         Object o, Exception e) {
        // 1. 创建MV
        ModelAndView mv = new ModelAndView();
        // 2. 封装视图+模型
        mv.setViewName("error");
        mv.addObject("msg", e.getMessage());
        return mv;
    }
}
```

## 自定义拦截器

```java
/**
 * 自定义处理器拦截器.
 *
 * @author : Jason.lee
 * @version : 1.0
 */
public class CustomInterceptor implements HandlerInterceptor {
    /**
     * 预处理: 处理器方法执行前执行的方法 (前置通知)
     * @return boolean : true表示放行, false: 表示不再执行控制器方法
     */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("预处理方法执行..");
        return true;
    }
    /**
     * 后处理: 处理器方法执行之后执行的方法 (后置通知)
     *  处理器方法没有执行, 不会执行该方法
     */
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("后处理方法执行..");
    }
    /**
     * 完成后处理: 处理器方法执行成功后执行的方法 (最终通知)
     *  处理器方法没有执行, 不会执行该方法
     */
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("完成后处理方法执行..");
    }
}
```

- SpringMVC.xml

```xml
    <!-- 4. 定义拦截器 -->
    <mvc:interceptors>
        <!-- 定义单个拦截器 -->
        <mvc:interceptor>
            <!-- 映射拦截的资源路径 -->
            <mvc:mapping path="/**"/>
            <!-- 定义拦截器所在的位置 -->
            <bean class="com.itheima.json.interceptor.CustomInterceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>
```

## 拦截器与过滤器

| 组件名称 | 组件来源       | 应用范围              |
| -------- | -------------- | --------------------- |
| 拦截器   | SpringMVC      | Controller方法        |
| 过滤器   | Servlet2.3规范 | 所有WEB资源 ( **/** ) |

# 框架整合

## 依赖

```xml
<dependencies>
        <!-- Spring 相关 -->
        <!-- 1. Spring IOC和AOP 依赖 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
        <!-- 2. Aspectj 依赖 -->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.8.13</version>
        </dependency>
        <!-- 3. Spring TX 依赖 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
        <!-- 4. Spring JDBC 依赖 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
        <!-- 5. Mysql驱动 依赖 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.17</version>
        </dependency>
        <!-- 6. Druid驱动 依赖 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.12</version>
        </dependency>
        <!-- SpringMVC 相关 -->
        <!-- 1. Spring-WEB -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
        <!-- 2. Spring-WEBMVC -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
        <!-- Mybatis 相关 -->
        <!-- 1. mybatis-Spring依赖,提供会话对象创建工具和代理对象创建工具的包-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.2</version>
        </dependency>
        <!-- 2. mybatis依赖 -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.2</version>
        </dependency>
        <!-- Junit 相关 -->
        <!-- 1. SpringTest依赖 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
        <!-- 2. Junit 依赖 -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
        <!-- Log4j 相关 -->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.7</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>1.7.26</version>
        </dependency>
        <!-- Servlet 相关 -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.0</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>jstl</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
    </dependencies>
```

## SSM - Spring

- applicationContext.xml

```xml
 <!-- 1. 添加Spring注解扫描 -->
    <context:component-scan base-package="com.itheima.ssm.service"/>
    <!-- 2. 开启AOP自动代理(支持) -->
    <aop:aspectj-autoproxy proxy-target-class="true"/>
    <!-- 3. 添加事务注解扫描(支持) -->
    <tx:annotation-driven />
    <!-- 4. 创建三层架构的对象(使用了注解代替) -->
    <!--<bean id="accountService" class="com.itheima.ssm.service.AccountService"/>-->
    <!-- 5. 创建事务管理器 -->
    <context:property-placeholder location="classpath:db.properties"/>
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${db.driver}"/>
        <property name="url" value="${db.url}"/>
        <property name="username" value="${db.username}"/>
        <property name="password" value="${db.password}"/>
    </bean>
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
     </bean>
```

## SSM - SpringMVC

- web.xml

```xml
<!--　1. 映射前端控制器的资源路径 -->
	<servlet-mapping>
		<servlet-name>ssm</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
	<!--　2. 配置前端控制器 -->
	<servlet>
		<servlet-name>ssm</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:springMVC.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
```

- springMVC.xml

```xml
<!--　1. 添加Spring组件扫描　-->
    <context:component-scan base-package="com.itheima.ssm.controller"/>
    <!--　2. 注册三大组件　-->
    <mvc:annotation-driven />
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/pages/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
```

## SSM - 整合SS

![1567420138244](E:\SyncData\WeiyunSync\Program\Study\Note\Java\笔记图片/springMVC三/1567420138244.png)

- 启动 **IOC** 容器: web.xml

```xml
<!-- 1. 提供监听器: 监听tomcat启动的时间, 当tomcat启动时, 会创建SpringIOC容器, 并且将MVC容器添加到IOC容器中 -->
	<!-- 父容器: SpringIOC容器 -->
	<!-- 子容器: MVC容器 -->
         <!--先启动父容器, 再启动子容器-->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
	<!-- 2. 配置默认加载的参数: 添加主配置文件 -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:applicationContext.xml</param-value>
	</context-param>
```

## SSM - Mybatis

- sqlMapConfig.xml

```xml
 <!-- 1. 加载外部配置文件 -->
    <properties resource="db.properties"/>
    <!-- 2. 配置环境信息 -->
    <environments default="dev">
        <environment id="dev">
            <transactionManager type="JDBC"></transactionManager>
            <dataSource type="POOLED">
                <property name="driver" value="${db.driver}"/>
                <property name="url" value="${db.url}"/>
                <property name="username" value="${db.username}"/>
                <property name="password" value="${db.password}"/>
            </dataSource>
        </environment>
    </environments>
    <!-- 3. 扫描代理接口包 -->
    <mappers>
        <package name="com.itheima.ssm.dao"/>
    </mappers>
```

## SSM - 整合SM

Mybatis由Spring启动

- applicationContext.xml

```xml
<!-- ======================Mybatis相关配置=========================== -->
<!-- 1. 创建会话对象:SqlSessionFactory.. -->
<bean class="org.mybatis.spring.SqlSessionFactoryBean">
    <!-- 注入数据源: 连接池 -->
    <property name="dataSource" ref="dataSource"/>
    <!-- 注入配置文件路径 -->
    <property name="configLocation" value="classpath:sqlMapConfig.xml"/>
    <!-- 如果是映射文件的方式: 注入sql文件路径 -->
    <!--<property name="mapperLocations" value="classpath:accountMapper.xml"/>-->
</bean>
<!-- 2. 创建代理对象:MapperScan... -->
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <!-- 注入代理接口所在的包路径 -->
    <property name="basePackage" value="com.itheima.ssm.dao"/>
</bean>
```

