# 01、Thymeleaf：模块引擎介绍

### 1.1 简介

官方网站：https://www.thymeleaf.org/index.html

![1572591157834](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/thymeleaf/1572591157834.png)

+ Thymeleaf是一个用于web和独立环境的现代的服务器端Java模板引擎。
+ Thymeleaf的主要目标是为您的开发工作流程带来优雅的*自然模板* - HTML。可以直接在浏览器中正确显示，并且可以作为静态原型，从而在开发团队中实现更强大的协作。
+ 借助Spring Framework的模块，可以根据自己的喜好进行自由选择，可插拔功能组件，Thymeleaf是现代HTML5 Web开发的理想选择。

> 说明：Spring官方支持的服务端渲染模板中，并不包含jsp。而是Thymeleaf和Freemarker等。

### 1.2 特点

- 动静结合：Thymeleaf 在有网络和无网络的环境下皆可运行，既可以让美工在浏览器查看页面的静态效果，也可以让程序员在服务器查看带数据的动态页面效果。这是由于它支持 html 原型，然后在 html 标签里增加额外的属性来达到模板+数据的展示方式。浏览器解释 html 时会忽略未定义的标签属性，所以 thymeleaf 的模板可以静态地运行；当有数据返回到页面时，Thymeleaf 标签会动态地替换掉静态内容，使页面动态显示。
- 开箱即用：它提供的标准 和 spring提供的标准(两种方式)，可以直接套用模板实现JSTL、OGNL表达式效果。同时开发人员也可以扩展和创建自定义的方言。
- 多方言支持：Thymeleaf 提供spring标准方言和一个与 SpringMVC 完美集成的可选模块，可以快速的实现表单绑定、属性编辑器、国际化等功能。
- 与SpringBoot完美整合，SpringBoot提供了Thymeleaf的默认配置，并且为Thymeleaf设置了视图解析器，我们可以像以前操作jsp一样来操作Thymeleaf。代码几乎没有任何区别，就是在模板语法上有区别。

# 02、Thymeleaf：入门示例

### 2.1 创建模块

+ 创建thymeleaf-demo模块

  ![1572592791397](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/thymeleaf/1572592791397.png)

  ![1572592877923](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/thymeleaf/1572592877923.png)

+ 修改pom.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <project xmlns="http://maven.apache.org/POM/4.0.0"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
           http://maven.apache.org/xsd/maven-4.0.0.xsd">
      <modelVersion>4.0.0</modelVersion>
      <groupId>cn.itcast</groupId>
      <artifactId>thymeleaf-demo</artifactId>
      <version>1.0-SNAPSHOT</version>
  
      <dependencies>
          <!-- 配置thymeleaf依赖 -->
          <dependency>
              <groupId>org.thymeleaf</groupId>
              <artifactId>thymeleaf</artifactId>
              <version>3.0.11.RELEASE</version>
          </dependency>
          <!-- 配置junit -->
          <dependency>
              <groupId>junit</groupId>
              <artifactId>junit</artifactId>
              <version>4.12</version>
              <scope>test</scope>
          </dependency>
      </dependencies>
  
      <build>
          <plugins>
              <plugin>
                  <groupId>org.apache.maven.plugins</groupId>
                  <artifactId>maven-compiler-plugin</artifactId>
                  <version>3.3</version>
                  <configuration>
                      <source>1.8</source>
                      <target>1.8</target>
                  </configuration>
              </plugin>
          </plugins>
      </build>
  </project>
  ```

### 2.2 模板文件

新建thymeleaf-demo\src\main\resources\templates\hello.html模块文件：

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Thymeleaf</title>
    <script type="text/javascript">
    </script>
</head>
<body>
    <div th:text="${message}"></div>
</body>
</html>
```

注意：

+ html标签上需要加thymeleaf模块**th**的命名空间
+ th:text 输出文本标签

![1572597193620](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/thymeleaf/1572597193620.png)

### 2.3 编写代码

#### 操作步骤

+ 第一步：创建类路径模板解析器
+ 第二步：创建模板引擎
+ 第三步：创建上下文对象(封装模板数据)
+ 第四步：模板引擎处理模板，输出文件

#### 代码

定义thymeleaf-demo\src\test\java\cn\itcast\ThymeleafTest.java：

```java
package cn.itcast;

import org.junit.Test;
import org.thymeleaf.TemplateEngine;
import org.thymeleaf.context.Context;
import org.thymeleaf.templatemode.TemplateMode;
import org.thymeleaf.templateresolver.ClassLoaderTemplateResolver;
import java.io.FileWriter;
import java.util.Calendar;
import java.util.Date;
import java.util.HashMap;
import java.util.Map;

public class ThymeleafTest {
    @Test
    public void test1() throws Exception{
        // 1. 创建类路径模板解析器
        ClassLoaderTemplateResolver templateResolver = new ClassLoaderTemplateResolver();
        // 1.1 设置模板前缀
        templateResolver.setPrefix("/templates/");
        // 1.2 设置模板后缀
        templateResolver.setSuffix(".html");
        // 1.3 设置模板类型
        templateResolver.setTemplateMode(TemplateMode.HTML);
        // 1.4 设置模板编码
        templateResolver.setCharacterEncoding("UTF-8");

        // 2. 创建模板引擎
        TemplateEngine templateEngine = new TemplateEngine();
        // 2.1 设置模板解析器
        templateEngine.setTemplateResolver(templateResolver);

        // 3. 创建上下文对象(封装数据)
        Context ctx = new Context();
        ctx.setVariable("message", "小华华，您真帅！");
        
        // 4. 模板引擎处理模板，输出文件
        templateEngine.process("hello", ctx,
                new FileWriter("C:\\Users\\Administrator\\Desktop\\html\\hello.html"));
    }
}
```

启动项目，访问页面：

![1572597783416](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/thymeleaf/1572597783416.png)

- 静态页面中，`th`指令不被识别，但是浏览器也不会报错，把它当做一个普通属性处理。

- 如果是在Thymeleaf环境下，`th`指令就会被识别和解析，而`th:text`的含义就是**替换所在标签中的文本内容**。

  > 标签的设计，正是Thymeleaf的高明之处，也是它优于其它模板引擎的原因。动静结合的设计，使得无论是前端开发人员还是后端开发人员可以完美契合。

#### 小结

+ ClassLoaderTemplateResolver: 类路径模板解析器

+ TemplateEngine: 模板引擎

+ Context: 上下文

  > 用到了上面三个核心API。

# 03、Thymeleaf：基本语法【了解】

### 3.1 表达式

+ 变量表达式: ${变量名}
+ 选择变量表达式: *{变量名}
+ 链接URL表达式: @{url路径}

### 3.2 变量类型

+ 文本变量: String字符串
+ 数值变量: 0,3.0
+ 布尔变量: true,false
+ 空变量: null

# 04、Thymeleaf：简单指令【掌握】

------

### 4.1 th:text

> th:text 显示普通文本字符串

+ 后台代码

  ```javascript
  // 设置普通字符串变量
  ctx.setVariable("message", "小华华，您真帅！");
  ```

+ 页面输出

  ```html
  <!-- 显示普通文本字符串 -->
  <div th:text="${message}"></div>
  ```

### 4.2 th:utext

> th:utext 显示html文本字符串

+ 后台代码

  ```java
  // 设置html字符串变量
  ctx.setVariable("html", "<h1>小华华，您真帅！</h1>");
  ```

+ 页面输出

  ```html
  <!-- 显示html文本字符串 -->
  <div th:utext="${html}"></div>
  ```

### 4.3 th:object

> th:object 显示对象

+ 后台代码

  ```java
  // 设置对象变量
  Map<String, Object> map = new HashMap<>();
  map.put("id", 1);
  map.put("name", "李小华");
  map.put("age", 18);
  ctx.setVariable("map", map);
  ```

+ 页面输出

  ```html
  <!-- 显示对象 -->
  <div th:object="${map}">
      编号：<span th:text="*{id}"></span><br/>
      姓名：<span th:text="*{name}"></span><br>
      年龄：<span th:text="*{age}"></span><br>
  </div>
  <div>
      编号：<span th:text="${map.id}"></span><br/>
      姓名：<span th:text="${map.name}"></span><br>
      年龄：<span th:text="${map.age}"></span><br>
  </div>
  ```

### 4.4 th:href

> th:href 设置超链接的href属性

+ 后台代码

  ```java
  // 设置id变量
  ctx.setVariable("id", 10);
  ```

+ 页面输出

  ```html
  <!-- 设置超链接的href属性 -->
  <a th:href="@{http://www.baidu.com(id=1)}">百度</a>
  <a th:href="@{http://www.baidu.com(id=${id})}">百度</a>
  ```

### 4.5 th:action

> th:action 设置form表单的action属性

```html
<!-- 设置form表单的action属性 -->
<form method="get" th:action="@{https://www.baidu.com(id=${id})}">
    <input type="submit" value="提交"/>
</form>
```

### 4.6 th:fragment

> th:fragment 定义一个片段

新建common.html页面:

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>片段</title>
</head>
<body>
    <!-- 定义footer片段 -->
    <div th:fragment="footer">
        <h1>我是底部</h1>
    </div>
</body>
</html>
```

### 4.7 th:include

> th:include 包含 th:fragment定义的片段。

在hello.html页面包含footer片段:

```html
<!-- 包含th:fragment定义的片段。 -->
<div th:include="common::footer"></div>
```

# 05、Thymeleaf：高级指令【掌握】

### 5.1 th:block|th:with

> th:block | th:with 定义变量

```html
<!-- 定义变量 -->
<th:block th:with="num=666">
    <div th:text="${num}"></div>
</th:block>
```

### 5.2 th:if|th:unless

> th:if 如果条件成立，就输出 (相当于if)
>
> th:unless 如果条件不成立,就输出 (相当于else)

```html
<!-- 条件判断
   th:if 如果条件成立，就输出
   th:unless 如果条件不成立,就输出
-->
<div th:if="${id > 10}">
    大于10
</div>
<div th:unless="${id} > 10">
    不太于10
</div>
```

说明: Thymeleaf中使用 th:if 或者 th:unless，两者的意思恰好相反。

### 5.3 th:switch|th:case

> th:switch|th:case 分支控制

+ 后台代码

  ```java
  // 设置角色
  ctx.setVariable("role", "admin");
  ```

+ 页面输出

  ```html
  <!-- 分支控制 -->
  <div th:switch="${role}">
      <p th:case="'admin'">超级管理员</p>
      <p th:case="'guest'">游客用户</p>
      <p th:case="*">普通用户</p>
  </div>
  ```

  注意: 一旦有一个th:case成立，其它的则不再判断。与java中的switch是一样的。

  ​     另外 th:case="*" 表示默认，放最后。

### 5.4 th:each

> th:each 数据迭代

#### 5.4.1 迭代List集合

+ 后台代码

  + 定义User.java实体类

  ```java
  package cn.itcast.pojo;
  
  public class User {
      private String name;
      private String sex;
      private int age;
      public User(){}
      public User(String name, String sex, int age) {
          this.name = name;
          this.sex = sex;
          this.age = age;
      }
      public String getName() {
          return name;
      }
      public void setName(String name) {
          this.name = name;
      }
      public String getSex() {
          return sex;
      }
      public void setSex(String sex) {
          this.sex = sex;
      }
      public int getAge() {
          return age;
      }
      public void setAge(int age) {
          this.age = age;
      }
  }
  ```

  + 在Context中添加变量

  ```java
  // 设置用户集合
  List<User> users = new ArrayList<>();
  users.add(new User("李大华", "男", 30));
  users.add(new User("李中华", "女", 25));
  users.add(new User("李小华", "男", 20));
  ctx.setVariable("users", users);
  ```

+ 页面输出

  ```html
  <!-- 迭代List集合 -->
  <table border="1">
      <tr>
          <th>编号</th>
          <th>姓名</th>
          <th>性别</th>
          <th>年龄</th>
      </tr>
      <tr th:each="u,stat : ${users}">
          <td th:text="${stat.index + 1}"></td>
          <td th:text="${u.name}"></td>
          <td th:text="${u.sex}"></td>
          <td th:text="${u.age}"></td>
      </tr>
  </table>
  ```

  stat对象包含以下属性：

  - index: 从0开始的下标
  - count: 元素的个数，从1开始
  - size: 总元素个数
  - current: 当前遍历到的元素
  - even/odd: 返回是否为奇偶，boolean值
  - first/last: 返回是否为第一或最后，boolean值

#### 5.4.2 迭代Map集合

+ 页面输出

  ```html
  <!-- 迭代Map集合 -->
  <div th:each="item : ${map}">
      <span th:text="${item.key}"></span>
      <===>
      <span th:text="${item.value}"></span>
  </div>
  ```

#### 5.4.3 迭代数组

+ 后端代码

  ```java
  // 设置数组
  ctx.setVariable("strArr", new String[]{"Spring Boot",
                   	"Spring Cloud", "Elastic Search"});
  ```

+ 页面输出

  ```html
   <!-- 迭代数组 -->
  <ul>
      <li th:each="str : ${strArr}"
          th:text="${str}"></li>
  </ul>
  ```


# 06、Thymeleaf：字符串操作符

### 6.1 字符串拼接

语法："'' + ${变量名} + ''"

```html
<!-- 字符串拼接 -->
<span th:text="'欢迎您:' + ${map['name']}"></span>
```

### 6.2 字符串替换

语法：|name is ${变量名}|

```html
<!-- 字符串替换 -->
<span th:text="|欢迎您:${users[0]['name']}|"></span>
```

# 07、Thymeleaf：运算符

注意：`${}`内部是通过OGNL表达式解析，外部的是通过Thymeleaf解析，因此运算符尽量放在`${}`外进行。

### 7.1 算术运算符

语法: +、-、*、/、%

```html
<!-- 算术运算符 -->
<div th:text="100 + 100"></div>
<div th:text="500 + 100 - 50 * 20 / 10"></div>
<div th:text="${id} / 2"></div>
```

### 7.2 布尔运算符

语法: and、or、!、not

```html
<!-- 布尔运算符 -->
<div th:if="!(100 == 100 and 50 > 50)"
     th:text="${message}"></div>
<div th:if="${id > 5} or ${id > 10}"
     th:text="${message}"></div>
```

### 7.3 比较运算符

语法: , < , >= , <= ( gt , lt , ge , le )，==，!=(eq，ne)

```html
<!-- 比较运算符 -->
<div th:text="100 > 100">100大于100</div>
<div th:text="100 gt 100">100大于100</div>

<div th:text="100 < 100">100小于100</div>
<div th:text="100 lt 100">100小于100</div>

<div th:text="100 == 100">100等于100</div>
<div th:text="100 eq 100">100等于100</div>
```

### 7.4 三目运算符

语法:

- if-then-else: (if) ? (then) : (else)

- Default: (value) ?: (defaultvalue)

  ```html
  <!-- 三目运算符 -->
  <div th:text="100 ge 100 ? '大于等于100' : '小于100'"></div>
  <div th:text="${map['age']} > 18 ? '成年人' : '未成年'"></div>
  
  <!-- 当前面的值为null时，就会使用后面的默认值 -->
  <div th:text="${map['name']} ?: '张三'"></div>
  ```

  > 注意：`?:` 之间没有空格。


# 08、Thymeleaf：内置对象【了解】

Thymeleaf中提供了一些内置对象，并且在这些对象中提供了一些方法，方便我们来调用。获取这些对象，需要使用`#对象名`来引用。

|       对象        | 作用                                          |
| :---------------: | :-------------------------------------------- |
|      `#ctx`       | 获取Thymeleaf自己的Context对象                |
|    `#requset`     | 如果是web程序，可以获取HttpServletRequest对象 |
|    `#response`    | 如果是web程序，可以获取HttpServletReponse对象 |
|    `#session`     | 如果是web程序，可以获取HttpSession对象        |
| `#servletContext` | 如果是web程序，可以获取HttpServletContext对象 |

获取Context对象中的变量:

```html
<!-- 内置对象 -->
<div th:text="${#ctx.message}"></div>
<div th:utext="${#ctx.html}"></div>
```

# 09、Thymeleaf：全局对象

|     对象     | 作用                             |
| :----------: | :------------------------------- |
|   `#dates`   | 处理java.util.date的工具对象     |
| `#calendars` | 处理java.util.calendar的工具对象 |
|  `#numbers`  | 用来对数字格式化的方法           |
|  `#strings`  | 用来处理字符串的方法             |
|   `#bools`   | 用来判断布尔值的方法             |
|  `#arrays`   | 用来处理数组的方法               |
|   `#lists`   | 用来处理List集合的方法           |
|   `#sets`    | 用来处理Set集合的方法            |
|   `#maps`    | 用来处理Map集合的方法            |

+ 后台代码:

  ```java
  // 设置变量
  ctx.setVariable("today", new Date());
  ctx.setVariable("date", Calendar.getInstance());
  ctx.setVariable("money", 9999.83d);
  ctx.setVariable("hobby", "我爱java开发");
  ```

+ 页面输出

  ```html
   <!-- #### 全局对象 ####### -->
  <!-- #dates: Date类型格式化 -->
  <div th:text="'dates：' + ${#dates.format(today,'yyyy-MM-dd')}"></div>
  <!-- #calendars: Calendar类型格式化 -->
  <div th:text="'calendars: ' + ${#calendars.format(date,'yyyy年MM月dd日')}"></div>
  
  <!-- 数值类型格式化 -->
  <!-- #numbers.formatCurrency: 格式化人民币显示的字衔串 -->
  <div th:text="'formatCurrency：' + ${#numbers.formatCurrency(money)}"></div>
  <!-- #numbers.formatDecimal: 格式化数字，整数至少保留1位，小数最大保留1位 -->
  <div th:text="'formatDecimal：' + ${#numbers.formatDecimal(money, 1,1)}"></div>
  
  <!-- #strings 操作字符串(字符串截取) -->
  <div th:text="'substring：' + ${#strings.substring(hobby, 0, 6)}"></div>
  
  <!-- #arrays 操作数组(数组长度) -->
  <div th:text="'数组长度：' + ${#arrays.length(strArr)}"></div>
  
  <!-- #lists 操作List集合(集合大小) -->
  <div th:text="'List集合大小：' + ${#lists.size(users)}"></div>
  
  <!-- #maps: 操作Map集合(集合大小) -->
  <div th:text="'Map集合大小：' + ${#maps.size(map)}"></div>
  ```

  ![1572752569113](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/thymeleaf/1572752569113.png)

# 10、Thymeleaf：js模板【掌握】

模板引擎不仅可以渲染html,也可以对JS进行模板化预处理。而且为了在纯静态环境下可以运行,其Thymeleaf代码可以被注释起来:

   ```javascript
<script th:inline="javascript">
    var map = /*[[${map}]]*/ {};
	var user = /*[[${users[0]}]]*/ {};
	var age = /*[[${users[0].age}]]*/ 20;
	console.log(map);
	console.log(user);
	console.log(age);
</script>
   ```

#### 使用步骤

- `<script>`标签中通过`th:inline="javascript"`来声明js模板

  ![1572754241195](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/thymeleaf/1572754241195.png)

- 表达式语法格式

  ```js
  var user = /*[[Thymeleaf表达式]]*/ "静态环境下的默认值";
  ```

  + 因为Thymeleaf被注释起来，因此即便是静态环境下,js代码也不会报错，而是采用表达式后面跟着的默认值。

- 运行测试

  + 查看页面源码:

    ![1572754381941](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/thymeleaf/1572754381941.png)

  + 控制台输出

    ![1572754413535](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/thymeleaf/1572754413535.png)

    > 说明：我们的User与Map对象被直接处理为json对象了，非常方便。

# 11、Thymeleaf：整合SpringMVC

### 11.1 创建模块

+ 创建thymeleaf-springmvc

  ![1572756659933](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/thymeleaf/1572756659933.png)

  ![1572756712343](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/thymeleaf/1572756712343.png)

  > 注意：需要转化成web项目

### 11.2 配置文件

+ 修改pom.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <project xmlns="http://maven.apache.org/POM/4.0.0"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                               http://maven.apache.org/xsd/maven-4.0.0.xsd">
  	<modelVersion>4.0.0</modelVersion>
  	<groupId>cn.itcast</groupId>
  	<artifactId>thymeleaf-springmvc</artifactId>
  	<version>1.0-SNAPSHOT</version>
  	<packaging>war</packaging>
  
  	<dependencies>
          <!-- servlet-api -->
          <dependency>
              <groupId>org.apache.tomcat.embed</groupId>
              <artifactId>tomcat-embed-core</artifactId>
              <version>8.5.16</version>
              <scope>provided</scope>
          </dependency>
          <!-- springmvc -->
          <dependency>
              <groupId>org.springframework</groupId>
              <artifactId>spring-webmvc</artifactId>
              <version>5.1.3.RELEASE</version>
          </dependency>
          <!-- thymeleaf-spring5 -->
          <dependency>
              <groupId>org.thymeleaf</groupId>
              <artifactId>thymeleaf-spring5</artifactId>
              <version>3.0.11.RELEASE</version>
          </dependency>
  	</dependencies>
  	<build>
          <plugins>
              <!-- 编译插件 -->
              <plugin>
                  <groupId>org.apache.maven.plugins</groupId>
                  <artifactId>maven-compiler-plugin</artifactId>
                  <version>3.3</version>
                  <configuration>
                      <source>1.8</source>
                      <target>1.8</target>
                      <encoding>UTF-8</encoding>
                  </configuration>
              </plugin>
              <!-- tomcat7插件 -->
              <plugin>
                  <groupId>org.apache.tomcat.maven</groupId>
                  <artifactId>tomcat7-maven-plugin</artifactId>
                  <version>2.2</version>
                  <configuration>
                      <path>/</path>
                      <port>8080</port>
                  </configuration>
              </plugin>
          </plugins>
      </build>
  </project>
  ```

+ 修改web.xml整合springmvc

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns="http://java.sun.com/xml/ns/javaee"
           xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
  	     http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
           version="2.5">
  
      <servlet>
          <servlet-name>springmvc</servlet-name>
          <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
          <init-param>
              <param-name>contextConfigLocation</param-name>
              <param-value>classpath:springmvc.xml</param-value>
          </init-param>
          <load-on-startup>1</load-on-startup>
      </servlet>
      <servlet-mapping>
          <servlet-name>springmvc</servlet-name>
          <url-pattern>/</url-pattern>
      </servlet-mapping>
  
  </web-app>
  ```

+ 新建springmvc.xml整合thymeleaf

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:mvc="http://www.springframework.org/schema/mvc"
         xmlns:context="http://www.springframework.org/schema/context"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd
         http://www.springframework.org/schema/mvc
         http://www.springframework.org/schema/mvc/spring-mvc.xsd
         http://www.springframework.org/schema/context
         http://www.springframework.org/schema/context/spring-context.xsd">
  
      <!-- 开启组件扫描 -->
      <context:component-scan base-package="cn.itcast.thymeleaf.controller"/>
      <!-- 开启MVC注解驱动 -->
      <mvc:annotation-driven/>
      <!-- 配置静态资源用WEB容器默认的servlet来处理 -->
      <mvc:default-servlet-handler/>
  
      <!-- ### 配置整合thymeleaf模板引擎 ### -->
      <!-- 配置模板解析器 -->
      <bean id="templateResolver" class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
          <!-- 设置模板文件存储路径前缀 -->
          <property name="prefix" value="/WEB-INF/templates/"/>
          <!-- 设置模板文件后缀名 -->
          <property name="suffix" value=".html"/>
          <!-- 设置模板文件编码 -->
          <property name="characterEncoding" value="UTF-8"/>
          <!-- 设置模板文件不开启缓存 -->
          <property name="cacheable" value="false"/>
      </bean>
      <!-- 配置模板引擎 -->
      <bean id="templateEngine" class="org.thymeleaf.spring5.SpringTemplateEngine">
          <!-- 设置模板解析器 -->
          <property name="templateResolver" ref="templateResolver"/>
      </bean>
      <!-- 配置视图解析器 -->
      <bean class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
          <!-- 设置模板引擎 -->
          <property name="templateEngine" ref="templateEngine"/>
          <!-- 设置模板文件输出的html页面的编码 -->
          <property name="characterEncoding" value="UTF-8"/>
      </bean>
  </beans>
  ```

### 11.3 模板文件

+ 新建WEB-INF/templates/user.html

  ```html
  <!DOCTYPE html>
  <html xmlns:th="http://www.thymeleaf.org">
  <head>
      <meta charset="UTF-8">
      <title>thymeleaf整合springmvc</title>
      <script type="text/javascript">
      </script>
  </head>
  <body>
      <h1 th:text="${message}"></h1>
  </body>
  </html>
  ```

### 11.4 编写代码

+ 新建cn.itcast.thymeleaf.controller.UserController.java

  ```java
  package cn.itcast.thymeleaf.controller;
  
  import org.springframework.stereotype.Controller;
  import org.springframework.ui.Model;
  import org.springframework.web.bind.annotation.GetMapping;
  
  @Controller
  public class UserController {
      
      @GetMapping("/show")
      public String show(Model model){
          model.addAttribute("message", "Thymeleaf整合SpringMVC成功！");
          return "user";
      }
  }
  ```

### 11.5 运行测试

+ 请求地址: <http://localhost:8080/show>

  ![1572759078721](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/thymeleaf/1572759078721.png)

# 12、Thymeleaf：整合SpringBoot

### 12.1 创建模块

+ 创建thymeleaf-springboot

  ![1572772546166](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/thymeleaf/1572772546166.png)

  ![1572772578327](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/thymeleaf/1572772578327.png)

### 12.2 配置文件

+ 修改pom.xml,引入依赖

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <project xmlns="http://maven.apache.org/POM/4.0.0"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
           http://maven.apache.org/xsd/maven-4.0.0.xsd">
      <modelVersion>4.0.0</modelVersion>
      <parent>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-parent</artifactId>
          <version>2.1.6.RELEASE</version>
      </parent>
      <groupId>cn.itcast</groupId>
      <artifactId>thymeleaf-springboot</artifactId>
      <version>1.0-SNAPSHOT</version>
      <properties>
          <java.version>1.8</java.version>
      </properties>
      <dependencies>
          <!-- 配置web启动器 -->
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-web</artifactId>
          </dependency>
          <!-- 配置Thymeleaf启动器 -->
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-thymeleaf</artifactId>
          </dependency>
      </dependencies>
  </project>
  ```

+ 新建application.yml

  ```properties
  server:
    port: 8080
  
  spring:
    thymeleaf:
      prefix: classpath:/templates/ # 前缀(默认)
      suffix: .html # 后缀(默认)
      mode: HTML # 模式(默认)
      encoding: UTF-8 # 编码(默认)
      cache: false # 不开启缓存(开发阶段)
  ```

### 12.3 模板文件

+ 新建thymeleaf-springboot\src\main\resources\templates\user.html

  ```html
  <!DOCTYPE html>
  <html xmlns:th="http://www.thymeleaf.org">
  <head>
      <meta charset="UTF-8">
      <title>Thymeleaf整合SpringBoot</title>
      <script type="text/javascript">
      </script>
  </head>
  <body>
      <h1 th:text="${message}"></h1>
  </body>
  </html>
  ```

### 12.4 编写代码

- 新建cn.itcast.ThymeleafApplication.java启动类

  ```java
  package cn.itcast;
  
  import org.springframework.boot.SpringApplication;
  import org.springframework.boot.autoconfigure.SpringBootApplication
  
  @SpringBootApplication
  public class ThymeleafApplication {
      public static void main(String[] args){
          SpringApplication.run(ThymeleafApplication.class ,args);
      }
  }
  ```

- 新建cn.itcast.controller.UserController.java控制器

  ```java
  package cn.itcast.controller;
  
  import org.springframework.stereotype.Controller;
  import org.springframework.ui.Model;
  import org.springframework.web.bind.annotation.GetMapping;
  
  @Controller
  public class UserController {
      @GetMapping("/show")
      public String show(Model model){
          model.addAttribute("message", "Thymeleaf整合SpringBoot成功！");
          return "user";
      }
  
  ```

### 12.5 运行测试

+ 访问地址: <http://localhost:8080/show>

  ![1572775537315](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/thymeleaf/1572775537315.png)

# 13、Thymeleaf：课程总结

+ 简单指令: th:text、th:utext、th:object、th:href
+ 高级指令: th:if|th:unless、th:switch|th:case、th:each
+ 运算符: +、-、*、/、%、> < >= <= == != and or !
+ 全局对象: #dates、#numbers、#strings、#lists、#maps
+ js模板: th:inline  var 变量名 =  /`*`[[Thymeleaf表达式]]`*`/ "静态环境下的默认值";
+ 整合SpringBoot
  + spring-boot-starter-thymeleaf 启动器