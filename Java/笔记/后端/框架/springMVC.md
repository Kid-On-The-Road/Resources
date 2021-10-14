### 01复习

#### 目标

- 理解三层架构和MVC模式
- 理解MVC与3层架构的关系



#### 1. 三层架构和MVC模式

##### 1.1 三层架构

- 三层架构(3-tier architecture): **视图层**, **业务层**, **持久层**

![1563503473383](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springMVC一/1563503473383.png)



##### 1.2 MVC模式

- MVC(Model, View, Controller)是视图层的一种设计模式

- M: 数据模型: 封装数据的对象
- V: 视图: 页面样式
- C: 控制器: 控制数据以及页面的掉转



#### 2. MVC与3层架构的关系

![1563505799097](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springMVC一/1563505799097.png) 



#### 小结

- 什么是MVC ?
  - 数据模型+视图+控制器
- MVC与三层架构是什么关系?
  - 是视图层的一种设计模式 (视图层包含MVC)



### 02SpringMVC概诉【了解】

#### 目标

- 了解SpringMVC概念
- 了解SpringMVC优点



#### 1. SpringMVC概念

- SpringMVC是 **web应用** 的一个Java平台的 **开源框架**
- SpringMVC是Spring **基于MVC设计模式** 实现的 **轻量级** 框架

#### 2. SpringMVC优点

| 序号 | 优点                     | 详情                                                         |
| ---- | ------------------------ | :----------------------------------------------------------- |
| 1    | 清晰的角色划分           | 前端控制器（ DispatcherServlet） <br/>处理器映射器（ HandlerMapping） <br/>处理器适配器（ HandlerAdapter） <br/>视图解析器（ ViewResolver） <br/>处理器或页面控制器（ Controller） <br/>验证器（ Validator） <br/>命令对象（ Command 请求参数绑定到的对象就叫命令对象）<br/>表单对象（ Form Object 提供给表单展示和表单提交的对象） |
| 2    | 可扩展性好               | 可以很容易扩展，虽然几乎不需要                               |
| 3    | 与Spring 框架无缝集成    | 这是其它web框架不具备的                                      |
| 4    | 可适配性好               | 通过 HandlerAdapter 可以支持任意的类作为处理器               |
| 5    | 可定制性好               | 提供从最简单的URL映射，到复杂的、专用的定制策略              |
| 6    | 单元测试方便             | 利用 Spring 提供的 Mock 对象能够非常简单的进行 Web 层单元测试 |
| 7    | 本地化、主题的解析的支持 | 支持包括诸如数据绑定和主题(theme)之类的许多功能              |
| 8    | jsp标签库                | 强大的 JSP 标签库，使 JSP 编写更容易                         |



#### 小结

- SpringMVC是什么?
  - WEB应用的基于MVC设计模式的轻量级开源框架
- 至少说出SpringMVC的3个角色划分?
  - 处理器适配器
  - 处理器映射器
  - 控制器



### 03SpringMVC案例【掌握】

#### 目标

- 搭建SpringMVC工程环境
- 掌握SpringMVC入门案例



#### 1. SpringMVC环境搭建

- 工程名称: mvc01_demo_01
- WEB工程: 添加web支持或者使用插件转换
- 添加依赖: pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
  <modelVersion>4.0.0</modelVersion>  
  <groupId>com.itheima</groupId>  
  <artifactId>mvc01_demo_01</artifactId>  
  <version>1.0-SNAPSHOT</version>

  <!-- 打包类型是war -->
  <packaging>war</packaging>



  <dependencies>
    <!-- 1. SpringMVC框架依赖 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.0.2.RELEASE</version>
    </dependency>
  </dependencies>
</project>

```



#### 2. SpringMVC入门案例

- web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://java.sun.com/xml/ns/javaee"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
	http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
	version="2.5">

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

</web-app>
```

- springMVC.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd">


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


</beans>
```

- com.itheima.demo.HelloController

```java
package com.itheima.demo;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

/**
 * 控制器.
 *
 * @author : Jason.lee
 * @version : 1.0
 */
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

```jsp
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



#### 小结

- 如何创建SpringMVC的控制器?
  - @Controller修饰类
- hello方法的返回值代表什么?
  - 视图名称



### 04案例的执行流程【掌握】

#### 目标

- 掌握SpringMVC的工作流程



#### 1. SpringMVC的工作流程

![1563517010381](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springMVC一/1563517010381.png) 



#### 小结

- 案例中哪个是前端控制器?
  - DispatcherServlet
- 案例中哪个是处理器?
  - HelloController





### 05SpringMVC组件【理解】

#### 目标

- 理解SpringMVC的基本组件
- 掌握SpringMVC的三大组件



#### 1. 基本组件

##### 1.1 前端控制器

- DispatcherServlet/前端控制器/核心控制器

- 作用: 用于接收并分发从浏览器发起的请求



##### 1.2 后端控制器

- Controller/处理器/后端控制器
- 作用: 表现层处理用户请求的对象



##### 1.3 视图

- View/视图
- 作用: 描述响应内容的对象



#### 2. 三大组件

##### 2.1 处理器映射器

- HandlerMapping/处理器映射器

- 作用: 匹配处理器的执行方法



##### 2.2 处理器适配器

- HandlerAdapter/处理器适配器
- 作用: 适配调用处理器的方法



##### 2.3 视图解析器

- ViewResolver/视图解析器
- 作用: 根据名称解析出视图对象



#### 小结

- SpringMVC的三大组件有哪些?
  - 处理器映射器
  - 处理器适配器
  - 视图解析器
- 如何配置SpringMVC三大组件?
  - <mvc:annotation-driver: 包含了处理器映射器+处理器适配器
  - bean class=InternalResourceViewResolver



### 06源码分析【理解】

#### 目标

- 分析SpringMVC执行流程的源码



#### 1. 执行流程的源码

- SpringMVC源码版本: 5.1.7.RELEASE

##### 1.1 入口分析

###### 1.1.1 web.xml

![1563347588314](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springMVC一/1563347588314.png) 

###### 1.1.2 Servlet

```java
public interface Servlet {
    // 初始化方法,构造方法后执行
    void init(ServletConfig var1);
    
	// 每次访问servlet时候执行
    void service(ServletRequest var1, ServletResponse var2);
    
	// 停止服务器时候执行
    void destroy();
}
```

###### 1.1.3 DispatcherServlet

![1563347766390](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springMVC一/1563347766390.png) 

###### 1.1.4 FrameworkServlet

![1563348062495](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springMVC一/1563348062495.png) 

##### 1.2 调度源码

###### 1.2.1 FrameworkServlet

```java
protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    HttpMethod httpMethod = HttpMethod.resolve(request.getMethod());
    if (httpMethod != HttpMethod.PATCH && httpMethod != null) {
        super.service(request, response);
    } else {
        // 最终都会执行处理请求的方法
        this.processRequest(request, response);
    }
}
```

```java
protected final void processRequest(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    long startTime = System.currentTimeMillis();
    Throwable failureCause = null;
    LocaleContext previousLocaleContext = LocaleContextHolder.getLocaleContext();
    LocaleContext localeContext = this.buildLocaleContext(request);
    RequestAttributes previousAttributes = RequestContextHolder.getRequestAttributes();
    ServletRequestAttributes requestAttributes = this.buildRequestAttributes(request, response, previousAttributes);
    WebAsyncManager asyncManager = WebAsyncUtils.getAsyncManager(request);
    asyncManager.registerCallableInterceptor(FrameworkServlet.class.getName(), new FrameworkServlet.RequestBindingInterceptor());
    this.initContextHolders(request, localeContext, requestAttributes);

    try {
        // 最终会调用doService方法; 该方法被DispatcherServlet重写了
        this.doService(request, response);
    } catch (IOException | ServletException var16) {
        failureCause = var16;
        throw var16;
    } catch (Throwable var17) {
        failureCause = var17;
        throw new NestedServletException("Request processing failed", var17);
    } finally {
        this.resetContextHolders(request, previousLocaleContext, previousAttributes);
        if (requestAttributes != null) {
            requestAttributes.requestCompleted();
        }

        this.logResult(request, response, (Throwable)failureCause, asyncManager);
        this.publishRequestHandledEvent(request, response, startTime, (Throwable)failureCause);
    }

}
```

###### 1.2.3 DispatcherServlet

```java
protected void doService(HttpServletRequest request, HttpServletResponse response) throws Exception {
    this.logRequest(request);
    Map<String, Object> attributesSnapshot = null;
    if (WebUtils.isIncludeRequest(request)) {
        attributesSnapshot = new HashMap();
        Enumeration attrNames = request.getAttributeNames();

        label95:
        while(true) {
            String attrName;
            do {
                if (!attrNames.hasMoreElements()) {
                    break label95;
                }

                attrName = (String)attrNames.nextElement();
            } while(!this.cleanupAfterInclude && !attrName.startsWith("org.springframework.web.servlet"));

            attributesSnapshot.put(attrName, request.getAttribute(attrName));
        }
    }

    request.setAttribute(WEB_APPLICATION_CONTEXT_ATTRIBUTE, this.getWebApplicationContext());
    request.setAttribute(LOCALE_RESOLVER_ATTRIBUTE, this.localeResolver);
    request.setAttribute(THEME_RESOLVER_ATTRIBUTE, this.themeResolver);
    request.setAttribute(THEME_SOURCE_ATTRIBUTE, this.getThemeSource());
    if (this.flashMapManager != null) {
        FlashMap inputFlashMap = this.flashMapManager.retrieveAndUpdate(request, response);
        if (inputFlashMap != null) {
            request.setAttribute(INPUT_FLASH_MAP_ATTRIBUTE, Collections.unmodifiableMap(inputFlashMap));
        }

        request.setAttribute(OUTPUT_FLASH_MAP_ATTRIBUTE, new FlashMap());
        request.setAttribute(FLASH_MAP_MANAGER_ATTRIBUTE, this.flashMapManager);
    }

    try {
        // 最终都会执行doDispatch方法
        this.doDispatch(request, response);
    } finally {
        if (!WebAsyncUtils.getAsyncManager(request).isConcurrentHandlingStarted() && attributesSnapshot != null) {
            this.restoreAttributesAfterInclude(request, attributesSnapshot);
        }

    }

}
```

```java
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
    HttpServletRequest processedRequest = request;
    HandlerExecutionChain mappedHandler = null;
    boolean multipartRequestParsed = false;
    WebAsyncManager asyncManager = WebAsyncUtils.getAsyncManager(request);

    try {
        try {
            ModelAndView mv = null;
            Object dispatchException = null;

            try {
                processedRequest = this.checkMultipart(request);
                multipartRequestParsed = processedRequest != request;
                // 根据URI获取到具体的处理器方法 (三大组件之一: 处理器映射器)
                mappedHandler = this.getHandler(processedRequest);
                if (mappedHandler == null) {
                    this.noHandlerFound(processedRequest, response);
                    return;
                }
				// 获取到处理器适配器 (三大组件之一: 处理器适配器)
                HandlerAdapter ha = this.getHandlerAdapter(mappedHandler.getHandler());
                String method = request.getMethod();
                boolean isGet = "GET".equals(method);
                if (isGet || "HEAD".equals(method)) {
                    long lastModified = ha.getLastModified(request, mappedHandler.getHandler());
                    if ((new ServletWebRequest(request, response)).checkNotModified(lastModified) && isGet) {
                        return;
                    }
                }

                if (!mappedHandler.applyPreHandle(processedRequest, response)) {
                    return;
                }
				// 适配器通过反射调用处理器方法
                // 并返回视图和模型
                mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
                if (asyncManager.isConcurrentHandlingStarted()) {
                    return;
                }
				// 如果视图是空采用默认视图
                this.applyDefaultViewName(processedRequest, mv);
                // 拦截器处理
                mappedHandler.applyPostHandle(processedRequest, response, mv);
            } catch (Exception var20) {
                dispatchException = var20;
            } catch (Throwable var21) {
                dispatchException = new NestedServletException("Handler dispatch failed", var21);
            }
			// 调用视图解析器处理结果 (三大组件之一: 视图解析器)
            this.processDispatchResult(processedRequest, response, mappedHandler, mv, (Exception)dispatchException);
        } catch (Exception var22) {
            this.triggerAfterCompletion(processedRequest, response, mappedHandler, var22);
        } catch (Throwable var23) {
            this.triggerAfterCompletion(processedRequest, response, mappedHandler, new NestedServletException("Handler processing failed", var23));
        }

    } finally {
        if (asyncManager.isConcurrentHandlingStarted()) {
            if (mappedHandler != null) {
                mappedHandler.applyAfterConcurrentHandlingStarted(processedRequest, response);
            }
        } else if (multipartRequestParsed) {
            this.cleanupMultipart(processedRequest);
        }

    }
}
```

##### 1.3 组件源码

###### 1.3.1 处理器映射器

```java
@Nullable
protected HandlerExecutionChain getHandler(HttpServletRequest request) throws Exception {
    if (this.handlerMappings != null) {
        // handlerMappings: 处理器映射器集合
        Iterator var2 = this.handlerMappings.iterator();

        while(var2.hasNext()) {
            HandlerMapping mapping = (HandlerMapping)var2.next();
            HandlerExecutionChain handler = mapping.getHandler(request);
            if (handler != null) {
                return handler;
            }
        }
    }

    return null;
}
```

###### 1.3.2 处理器适配器

```java
protected HandlerAdapter getHandlerAdapter(Object handler) throws ServletException {
    if (this.handlerAdapters != null) {
        // handlerAdapters: 处理器适配器集合
        Iterator var2 = this.handlerAdapters.iterator();

        while(var2.hasNext()) {
            HandlerAdapter adapter = (HandlerAdapter)var2.next();
            if (adapter.supports(handler)) {
                return adapter;
            }
        }
    }

    throw new ServletException("No adapter for handler [" + handler + "]: The DispatcherServlet configuration needs to include a HandlerAdapter that supports this handler");
}
```

###### 1.3.3 视图解析器

```java
@Nullable
protected View resolveViewName(String viewName, @Nullable Map<String, Object> model, Locale locale, HttpServletRequest request) throws Exception {
    if (this.viewResolvers != null) {
        Iterator var5 = this.viewResolvers.iterator();

        while(var5.hasNext()) {
            ViewResolver viewResolver = (ViewResolver)var5.next();
            View view = viewResolver.resolveViewName(viewName, locale);
            if (view != null) {
                return view;
            }
        }
    }

    return null;
}
```

##### 1.4 视图源码

###### 1.4.1 视图处理: DispatcherServlet

```java
private void processDispatchResult(HttpServletRequest request, HttpServletResponse response, @Nullable HandlerExecutionChain mappedHandler, @Nullable ModelAndView mv, @Nullable Exception exception) throws Exception {
    boolean errorView = false;
    if (exception != null) {
        if (exception instanceof ModelAndViewDefiningException) {
            this.logger.debug("ModelAndViewDefiningException encountered", exception);
            mv = ((ModelAndViewDefiningException)exception).getModelAndView();
        } else {
            Object handler = mappedHandler != null ? mappedHandler.getHandler() : null;
            mv = this.processHandlerException(request, response, handler, exception);
            errorView = mv != null;
        }
    }

    if (mv != null && !mv.wasCleared()) {
        // 渲染视图
        this.render(mv, request, response);
        if (errorView) {
            WebUtils.clearErrorRequestAttributes(request);
        }
    } else if (this.logger.isTraceEnabled()) {
        this.logger.trace("No view rendering, null ModelAndView returned.");
    }

    if (!WebAsyncUtils.getAsyncManager(request).isConcurrentHandlingStarted()) {
        if (mappedHandler != null) {
            mappedHandler.triggerAfterCompletion(request, response, (Exception)null);
        }

    }
}
```

###### 1.4.2 视图渲染: DispatcherServlet

```java
protected void render(ModelAndView mv, HttpServletRequest request, HttpServletResponse response) throws Exception {
    Locale locale = this.localeResolver != null ? this.localeResolver.resolveLocale(request) : request.getLocale();
    response.setLocale(locale);
    String viewName = mv.getViewName();
    View view;
    if (viewName != null) {
        // 根据视图解析器创建视图对象
        view = this.resolveViewName(viewName, mv.getModelInternal(), locale, request);
        if (view == null) {
            throw new ServletException("Could not resolve view with name '" + mv.getViewName() + "' in servlet with name '" + this.getServletName() + "'");
        }
    } else {
        view = mv.getView();
        if (view == null) {
            throw new ServletException("ModelAndView [" + mv + "] neither contains a view name nor a View object in servlet with name '" + this.getServletName() + "'");
        }
    }

    if (this.logger.isTraceEnabled()) {
        this.logger.trace("Rendering view [" + view + "] ");
    }

    try {
        if (mv.getStatus() != null) {
            response.setStatus(mv.getStatus().value());
        }
		// 视图渲染: AbstractView
        view.render(mv.getModelInternal(), request, response);
    } catch (Exception var8) {
        if (this.logger.isDebugEnabled()) {
            this.logger.debug("Error rendering view [" + view + "]", var8);
        }

        throw var8;
    }
}
```

###### 1.4.3 合并视图: AbstractView

```java
public void render(@Nullable Map<String, ?> model, HttpServletRequest request, HttpServletResponse response) throws Exception {
    if (this.logger.isDebugEnabled()) {
        this.logger.debug("View " + this.formatViewName() + ", model " + (model != null ? model : Collections.emptyMap()) + (this.staticAttributes.isEmpty() ? "" : ", static attributes " + this.staticAttributes));
    }

    Map<String, Object> mergedModel = this.createMergedOutputModel(model, request, response);
    this.prepareResponse(request, response);
    // 合并视图与数据模型: InternalResourceView
    this.renderMergedOutputModel(mergedModel, this.getRequestToExpose(request), response);
}
```

##### 1.5 响应源码

###### 1.5.1 InternalResourceView

```java
protected void renderMergedOutputModel(Map<String, Object> model, HttpServletRequest request, HttpServletResponse response) throws Exception {
    this.exposeModelAsRequestAttributes(model, request);
    this.exposeHelpers(request);
    String dispatcherPath = this.prepareForRendering(request, response);
    RequestDispatcher rd = this.getRequestDispatcher(request, dispatcherPath);
    if (rd == null) {
        throw new ServletException("Could not get RequestDispatcher for [" + this.getUrl() + "]: Check that the corresponding file exists within your web application archive!");
    } else {
        if (this.useInclude(request, response)) {
            response.setContentType(this.getContentType());
            if (this.logger.isDebugEnabled()) {
                this.logger.debug("Including [" + this.getUrl() + "]");
            }

            rd.include(request, response);
        } else {
            if (this.logger.isDebugEnabled()) {
                this.logger.debug("Forwarding to [" + this.getUrl() + "]");
            }
			// 转发页面
            rd.forward(request, response);
        }

    }
}
```

###### 1.5.2 RedirectView

```java
protected void renderMergedOutputModel(Map<String, Object> model, HttpServletRequest request, HttpServletResponse response) throws IOException {
    String targetUrl = this.createTargetUrl(model, request);
    targetUrl = this.updateTargetUrl(targetUrl, model, request, response);
    RequestContextUtils.saveOutputFlashMap(targetUrl, request, response);
    // 重定向地址
    this.sendRedirect(request, response, targetUrl, this.http10Compatible);
}
```

```java
protected void sendRedirect(HttpServletRequest request, HttpServletResponse response, String targetUrl, boolean http10Compatible) throws IOException {
    String encodedURL = this.isRemoteHost(targetUrl) ? targetUrl : response.encodeRedirectURL(targetUrl);
    HttpStatus attributeStatusCode;
    if (http10Compatible) {
        attributeStatusCode = (HttpStatus)request.getAttribute(View.RESPONSE_STATUS_ATTRIBUTE);
        if (this.statusCode != null) {
            response.setStatus(this.statusCode.value());
            response.setHeader("Location", encodedURL);
        } else if (attributeStatusCode != null) {
            response.setStatus(attributeStatusCode.value());
            response.setHeader("Location", encodedURL);
        } else {
            // 重定向地址
            response.sendRedirect(encodedURL);
        }
    } else {
        attributeStatusCode = this.getHttp11StatusCode(request, response, targetUrl);
        response.setStatus(attributeStatusCode.value());
        response.setHeader("Location", encodedURL);
    }

}
```

##### 1.6 源码说明

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





#### 小结

- 项目中最早被加载的配置文件?
  - web.xml
- SpringMVC的入口类是哪个?
  - DispatcherServlet

- 最终的页面跳转由什么组件完成?
  - View



### 07@RequestMapping【理解】

#### 目标

- 理解@RequestMapping的作用



#### 1. 注解的作用

- 映射与限制请求

##### 1.1 映射请求地址

1. 控制器: com.itheima.demo.RequestMappingController

   ```java
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



##### 1.2 限制请求方法

1. 控制器: com.itheima.demo.RequestMappingController

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

2. 页面演示: index.jsp

   ```jsp
       <h3>演示: 限制POST请求</h3>
       <form method="post" action="list2.do">
           <input type="text" name="id" value="1"/>
           <input type="submit">
       </form>
   ```



##### 1.3 根路径映射限制

1. 控制器: com.itheima.demo.RequestMappingController

   ```java
   package com.itheima.demo;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.ui.Model;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RequestMethod;
   
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



#### 小结

- @RequestMapping的作用是什么?
  - 映射和限制请求



### 08参数绑定-基本类型【理解】

#### 目标

- 理解基本类型的参数绑定
- 解决参数的中文乱码问题



#### 1. 基本类型的参数绑定

1. 接收基本类型: com.itheima.demo.ParamController

   ```java
   package com.itheima.demo;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   
   /**
    * ParamController.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   @Controller
   public class ParamController {
   
   
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
   }
   
   ```

2. 演示提交中文: index.jsp

   ```jsp
   <%--
     Created by IntelliJ IDEA.
     User: Jason
     Date: 2019/11/24
     Time: 11:36
     To change this template use File | Settings | File Templates.
   --%>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>欢迎光临</title>
   </head>
   <body>
   
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
   </body>
   </html>
   
   ```



#### 2. 解决参数的中文乱码

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

  

#### 小结

- 为什么不建议使用int(基本类型)接收参数?
  - 因为基本数据类型没有默认值, 所以变成了必传参数, 可能会报错
- SpringMVC是如何解决参数乱码的问题?
  - 配置Spring乱码过滤器





### 09参数绑定-对象类型【掌握】

#### 目标

- 掌握对象类型的参数绑定
- 理解嵌套类型的参数绑定



#### 1. 对象类型的参数绑定

- 创建实体: com.itheima.demo.Account

```java
package com.itheima.demo;

/**
 * Account.
 *
 * @author : Jason.lee
 * @version : 1.0
 */
public class Account {

    private Integer id;
    private Integer uid;
    private Double money;


    public void setId(Integer id) {
        this.id = id;
    }

    public void setUid(Integer uid) {
        this.uid = uid;
    }

    public void setMoney(Double money) {
        this.money = money;
    }

    @Override
    public String toString() {
        return "Account{" +
                "id=" + id +
                ", uid=" + uid +
                ", money=" + money +
                '}';
    }
}

```

- 接收对象: com.itheima.demo.ParamController

```java
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



#### 2. 嵌套类型的参数绑定

- 创建实体: com.itheima.demo.domain.User

```java
package com.itheima.demo.domain;

import java.util.Date;

/**
 * User.
 *
 * @author : Jason.lee
 * @version : 1.0
 */
public class User {


    private Integer id;
    private String username;
    private Date birthday;
    private String sex;
    private String address;


    public void setId(Integer id) {
        this.id = id;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", birthday=" + birthday +
                ", sex='" + sex + '\'' +
                ", address='" + address + '\'' +
                '}';
    }
}

```

- 嵌套实体: com.itheima.demo.domain.Account

```java
package com.itheima.demo;

import com.itheima.demo.domain.User;

/**
 * Account.
 *
 * @author : Jason.lee
 * @version : 1.0
 */
public class Account {

    private Integer id;
    private Integer uid;
    private Double money;


    private User user;


    public void setUser(User user) {
        this.user = user;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public void setUid(Integer uid) {
        this.uid = uid;
    }

    public void setMoney(Double money) {
        this.money = money;
    }

    @Override
    public String toString() {
        return "Account{" +
                "id=" + id +
                ", uid=" + uid +
                ", money=" + money +
                ", user=" + user +
                '}';
    }
}

```



#### 小结

- 使用对象接收参数的好处?
  - 使参数列表更简洁



### 10参数绑定-默认支持【了解】

#### 目标

- 了解默认支持的参数类型



#### 1. 默认支持的参数类型

- com.itheima.demo.ParamController

  ```java
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



#### 小结

- 支持哪些常用的ServletAPI?
  - Request, Response, Session
- Model参数的作用是什么?
  - 往请求与中存储参数



### 11自定义参数转换器【理解】

#### 目标

- 理解SpringMVC的参数转换器



#### 1. 自定义参数转换器

1. 定义转换器: com.itheima.demo.DateConverter

   ```java
   package com.itheima.demo;
   
   import org.springframework.core.convert.converter.Converter;
   
   import java.text.ParseException;
   import java.text.SimpleDateFormat;
   import java.util.Date;
   import java.util.logging.SimpleFormatter;
   
   /**
    * 时间转换器.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
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

2. 配置转换器: springMVC.xml

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



#### 小结

- 自定义的转换器需要实现什么接口?
  - Converter
- 自定义的转换器需要做什么配置?
  - 定义并引用格式化转换服务工厂





### 12高级绑定-数组【了解】

#### 目标

- 了解数组类型参数的绑定



#### 1. 数组类型参数的绑定

1. 接收数组: com.itheima.demo.Param2Controller

  ```java
  package com.itheima.demo;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

/**
 * 演示高级参数的绑定.
 *
 * @author : Jason.lee
 * @version : 1.0
 */
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

2. 提交数组: index.jsp

  ```jsp
  

    <h3>演示: 数组参数绑定</h3>
    <form method="post" action="list7.do">
        <input type="text" name="ids" value="1"/>
        <input type="text" name="ids" value="2"/>
        <input type="text" name="ids" value="3"/>
        <input type="submit">
    </form>
  ```



#### 小结

- 在页面中如何表现为数组?
  - 提交多个相同的参数名称的值



### 13高级绑定-集合【了解】

#### 目标

- 了解直接绑定的集合
- 了解嵌套绑定的集合



#### 1. 直接绑定的集合

1. 接收集合: com.itheima.demo.Param2Controller

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

2. 提交集合: index.jsp

  ```jsp
<h3>演示: 集合参数绑定</h3>
<form method="post" action="list8.do">
    <input type="text" name="ids" value="2"/>
    <input type="text" name="ids" value="3"/>
    <input type="submit">
</form>
  ```



#### 2. 嵌套绑定的集合

1. 嵌套对象: com.itheima.demo.domain.Account

  ```java
package com.itheima.demo;

import com.itheima.demo.domain.User;

import java.util.List;

/**
 * Account.
 *
 * @author : Jason.lee
 * @version : 1.0
 */
public class Account {

    private Integer id;
    private Integer uid;
    private Double money;


    private List<Integer> ids;

    private List<User> users;

    public List<User> getUsers() {
        return users;
    }

    public void setUsers(List<User> users) {
        this.users = users;
    }

    public List<Integer> getIds() {
        return ids;
    }

    public void setIds(List<Integer> ids) {
        this.ids = ids;
    }

    private User user;

    public Integer getId() {
        return id;
    }

    public Integer getUid() {
        return uid;
    }

    public Double getMoney() {
        return money;
    }

    public User getUser() {
        return user;
    }

    public void setUser(User user) {
        this.user = user;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public void setUid(Integer uid) {
        this.uid = uid;
    }

    public void setMoney(Double money) {
        this.money = money;
    }

    @Override
    public String toString() {
        return "Account{" +
                "id=" + id +
                ", uid=" + uid +
                ", money=" + money +
                ", ids=" + ids +
                ", users=" + users +
                ", user=" + user +
                '}';
    }
}

  ```

2. 嵌套参数: com.itheima.demo.Param2Controller

  ```java
@RequestMapping("list9")
public String list9(Account account, Model model){
    System.out.println(account);
    model.addAttribute("msg", account);
    return "success";
}  
  ```

3. 提交参数: index.jsp

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

  

#### 小结

- 直接绑定集合参数需要做什么?
  - 使用@RequestParam注解修饰参数列表
- 嵌套集合如何传参给指定属性?
  - 使用下标指定对象, 再使用.属性名称指定属性值 (users[0].username)
- 可否将接收到的参数返回给前端？
  - 可以



### 14返回值-void【理解】

#### 目标

- 理解返回值void的意义
- 理解返回值void的用法



#### 1. 返回值void的意义

- SpringMVC不响应任何内容



#### 2. 返回值void的用法

##### 1.1 转发

- com.itheima.demo.ReturnController

  ```java
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

##### 1.2 重定向

- com.itheima.demo.ReturnController

  ```java
  @RequestMapping("void2")
  public void void2(HttpServletRequest req, HttpServletResponse res) throws Exception {
      System.out.println("访问了void1方法..");
      res.sendRedirect("/pages/success.jsp");
  }
  ```

##### 1.3 响应数据

- com.itheima.demo.ReturnController

  ```java
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



#### 小结

- 为什么可以不返回视图呢?
  - 因为已经调用原生的API响应了



### 15返回值-string【掌握】

#### 目标

- 理解返回值string的用法



#### 1. 返回值string的用法

##### 1.1 转发

- com.itheima.demo.ReturnController

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

##### 1.2 重定向

- com.itheima.demo.ReturnController

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

##### 1.3 响应数据

- com.itheima.demo.ReturnController

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



#### 小结

- 使用forward:的作用?
  - 转发指定路径的资源



### 16返回值-MV【理解】

#### 目标

- 理解返回值ModelAndView的用法



#### 1. 返回值ModelAndView的用法

- com.itheima.demo.ReturnController

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



#### 小结

- 返回值mv与string的区别?
  - mv包含了String(视图名称)和数据模型



### 17总结

1. 什么是SpringMVC?
   - WEB应用基于MVC设计模式的轻量级开源框架
2. 入门案例中的处理器是哪个类?
   - HelloController
3. SpringMVC的三大组件是哪些?
   - 处理器映射器
   - 处理器适配器
   - 视图解析器

4. `<mvc:annotation-driven/>`的作用?

- 可以注册处理器映射器+处理器适配器

5. SpringMVC的入口类是哪个?

   - DispatcherServlet (doDispatcher())

6. @RequestMapping的作用?

   - 限制和映射请求

7. 至少说出3个默认支持的参数类型?

   - Model

   - HttpServletRequest

   - HttpSession

8. SpringMVC支持哪些参数绑定?

   - 基本数据类型
   - 对象数据类型
   - 嵌套数据类型

9. Controller返回值有哪些类型?

   - void
   - string
   - ModelAndView



### 01复习

#### 目标

- 搭建SpringMVC案例环境



#### 1. 搭建案例环境

##### 1.1 环境搭建

1. 工程名称: mvc02_json_01

2. 添加WEB: 转换为WEB工程

3. 添加依赖: pom.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
     <modelVersion>4.0.0</modelVersion>  
     <groupId>com.itheima</groupId>  
     <artifactId>mvc02_json_01</artifactId>  
     <version>1.0-SNAPSHOT</version>
   
     <packaging>war</packaging>
   
     <dependencies>
       <!-- SpringMVC依赖 -->
       <!--<dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-web</artifactId>
         <version>5.0.2.RELEASE</version>
       </dependency>-->
   
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-webmvc</artifactId>
         <version>5.0.2.RELEASE</version>
       </dependency>
     </dependencies>
   </project>
   
   ```



##### 1.2 配置组件

1. 前端控制器: web.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   	xmlns="http://java.sun.com/xml/ns/javaee"
   	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
   	http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
   	version="2.5">
   
   
   	<!--1. 映射处理器资源路径-->
   	<servlet-mapping>
   		<servlet-name>mvc</servlet-name>
   		<!-- *.do: 只拦截.do结尾的请求 -->
   		<!-- /*: 拦截所有资源(正常情况下, servlet不会使用) -->
   		<!-- /: 拦截所有没有被拦截的资源(例如: jsp) -->
   		<url-pattern>/</url-pattern>
   	</servlet-mapping>
   	<!--2. 定义前端控制器 -->
   	<servlet>
   		<servlet-name>mvc</servlet-name>
   		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
   		<init-param>
   			<param-name>contextConfigLocation</param-name>
   			<param-value>classpath:springMVC.xml</param-value>
   		</init-param>
   		<load-on-startup>1</load-on-startup>
   	</servlet>
   
   </web-app>
   ```

2. 三大组件: springMVC.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd">
   
   
       <!-- 1. 添加Spring组件扫描 -->
       <context:component-scan base-package="com.itheima.json"/>
       <!-- 2. 注册三大组件 -->
       <mvc:annotation-driven />
       <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
           <property name="prefix" value="/pages/"/>
           <property name="suffix" value=".jsp"/>
       </bean>
   </beans>
   ```

3. 后端控制器: com.itheima.json.JsonController

   ```java
   package com.itheima.json;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.ui.Model;
   import org.springframework.web.bind.annotation.RequestMapping;
   
   /**
    * 演示Json数据交互的控制器.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   @Controller
   public class JsonController {
   
   
   
       @RequestMapping("list")
       public String list(Model model){
           model.addAttribute("msg", "案例环境搭建完成..");
           return "success";
       }
   }
   
   ```



##### 1.3 单元测试

1. pages/success.jsp

   ```jsp
   <%--
     Created by IntelliJ IDEA.
     User: Jason
     Date: 2019/11/26
     Time: 9:24
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

2. index.jsp

   ```jsp
   <%--
     Created by IntelliJ IDEA.
     User: Jason
     Date: 2019/11/26
     Time: 9:24
     To change this template use File | Settings | File Templates.
   --%>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>欢迎光临</title>
   </head>
   <body>
   
   </body>
   </html>
   
   ```



#### 小结

- SpringMVC的三大组件是?
  - 处理器映射器
  - 处理器适配器
  - 视图解析器
- SpringMVC的工作流程是?

  - 1. 找到入口文件: web.xml

    2. 找到入口类: DispatcherServlet
    3. 找到入口方法: service()
    4. 找到最终的入口方法: doDispacher(req, res)
    5. 解析请求: 获取URI
    6. 获取控制器执行方法: 遍历处理器映射器匹配URI处理的方法
    7. 获取处理器适配器: 遍历出可以处理执行方法的适配器
    8. 处理器适配器执行控制器方法: 返回mv
    9. 调用视图解析器解析视图: View
    10. 渲染视图: 合并Model+View
    11. 返回资源: 跳转/转发/重定向
- 控制器支持哪些参数绑定?

  - 基本数据类型
  - 对象数据类型
  - 嵌套类型



### 02数据交互-json【掌握】

#### 目标

- 掌握json格式的数据交互



#### 1. json格式的数据交互

1. 添加依赖: pom.xml

   ```xml
   
       <!-- 2.  添加jackson依赖 -->
       <dependency>
         <groupId>com.fasterxml.jackson.core</groupId>
         <artifactId>jackson-databind</artifactId>
         <!-- 版本: 练习时必须一致 -->
         <version>2.9.9</version>
       </dependency>
   ```

2. 创建实体: com.itheima.json.domain.Account

   ```java
   package com.itheima.json.domain;
   
   /**
    * 账户类.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class Account {
   
       private Integer id;
       private Integer uid;
       private Double money;
   
       public Integer getId() {
           return id;
       }
   
       public void setId(Integer id) {
           this.id = id;
       }
   
       public Integer getUid() {
           return uid;
       }
   
       public void setUid(Integer uid) {
           this.uid = uid;
       }
   
       public Double getMoney() {
           return money;
       }
   
       public void setMoney(Double money) {
           this.money = money;
       }
   
       @Override
       public String toString() {
           return "Account{" +
                   "id=" + id +
                   ", uid=" + uid +
                   ", money=" + money +
                   '}';
       }
   }
   
   ```

3. 使用注解: com.itheima.json.JsonController

   ```java
   package com.itheima.json;
   
   import com.itheima.json.domain.Account;
   import org.springframework.stereotype.Controller;
   import org.springframework.ui.Model;
   import org.springframework.web.bind.annotation.RequestBody;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.ResponseBody;
   
   /**
    * 演示Json数据交互的控制器.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   @Controller
   public class JsonController {
   
   
   
       @RequestMapping("list")
       public String list(Model model){
           model.addAttribute("msg", "案例环境搭建完成..");
           return "success";
       }
   
   
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
   }
   
   ```

4. 页面请求: index.jsp

   ```html
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



#### 小结

- 如何使用对象接收json数据?
  - 使用@RequestBody修饰参数
- 响应对象如何转换成json数据?
  - @ResponseBody修饰方法



### 03RESTful风格【了解】

#### 目标

- 了解RESTful风格
- RESTful参数绑定



#### 1. RESTful介绍

##### 1.1 基本概诉

- REST全称是Representational State Transfer，REST本身并没有创造新的技术、组件或服务。
- REST指的是约束条件和原则，如果一个架构符合REST的约束条件和原则，就称它为RESTful架构。

##### 1.2 风格对比

|             | 增        | 删                | 查             | 改                |
| ----------- | --------- | ----------------- | -------------- | ----------------- |
| 传统        | /user/add | /user/delete?id=1 | /user/get?id=1 | /user/update?id=1 |
| **RESTful** | /user     | /user/1           | /user/1        | /user/1           |

#### 2. RESTful参数

- com.itheima.json.RestController

  ```java
  package com.itheima.json;
  
  import org.springframework.stereotype.Controller;
  import org.springframework.ui.Model;
  import org.springframework.web.bind.annotation.PathVariable;
  import org.springframework.web.bind.annotation.RequestMapping;
  
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



#### 小结

- RESTful是什么?
  - 是一种URL的编码风格
- @PathVariable的作用?
  - 获取URI中的参数



### 04RESTful支持【了解】

#### 目标

- 了解RESTful的方法支持



#### 1. RESTful的方法支持

1. web.xml

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

2. com.itheima.json.RestController

   ```java
   package com.itheima.json;
   
   import com.itheima.json.domain.Account;
   import org.springframework.stereotype.Controller;
   import org.springframework.ui.Model;
   import org.springframework.web.bind.annotation.RequestBody;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.ResponseBody;
   
   /**
    * 演示Json数据交互的控制器.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   @Controller
   public class JsonController {
   
   
   
       @RequestMapping("list")
       public String list(Model model){
           model.addAttribute("msg", "案例环境搭建完成..");
           return "success";
       }
   
   
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
   }
   
   ```

3. index.jsp

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



#### 小结

- HiddenHttpMethodFilter的作用?
  - 将POST请求转换成相应隐藏域中的请求方式



### 05文件上传-传统【了解】

#### 目标

- 回顾传统的文件上传



#### 1. 传统的文件上传

1. 添加依赖: pom.xml

   ```xml
   <!-- 基于commons-fileupload组件完成文件解析 -->
   
   ```

2. 前端页面: index.jsp

   ```jsp
   <h3>演示: 传统文件上传</h3>
   <%--
       文件上传的form表单的要求
           1. 请求方法必须是post
           2. 表单类型必须是:multipart/form-data
           3. 表单中需要携带file类型的内容
   
   --%>
   <form action="upload" method="post" enctype="multipart/form-data">
       <input type="file" name="file"/>
       <input type="submit"/>
   </form>
   ```

3. com.itheima.json.UploadController

   ```java
       /**
        * 传统的文件上传
        * @param req
        * @param res
        * @return
        */
       @RequestMapping("upload")
       public String list(HttpServletRequest req, HttpServletResponse res, Model model) throws Exception {
           // 1. 获取文件上传路径
           String root = req.getSession().getServletContext().getRealPath("upload");
           File rootFile = new File(root);
           if(!rootFile.exists()){
               rootFile.mkdir();
           }
           // 2. 创建文件上传的工具
           DiskFileItemFactory factory = new DiskFileItemFactory();
           ServletFileUpload sfu = new ServletFileUpload(factory);
           // 3. 使用工具解析请求 (得到所有字段)
           List<FileItem> items = sfu.parseRequest(req);
           // 4. 遍历字段  (判断是否是文件)
           for (FileItem item : items){
               // 判断是不是普通的表单字段
               if(item.isFormField()){
                   // 打印普通字段的内容
                   System.out.println(item.getString());
               } else {
                   // 否则做文件上传
                   // 1. 拼接全文件名 (root/UUID.filename)
                   String filename = root + "/" + UUID.randomUUID().toString() + "." + item.getName();
                   model.addAttribute("msg", filename);
                   // 2. 创建文件并写出文件内容
                   File f = new File(filename);
                   item.write(f);
               }
           }
   
           // 5. 响应结果
           return "success";
       }
   ```



#### 小结

- 传统方式需要解析文件吗?
  - 需要手动解析文件已经其他普通字段



### 05SpringMVC上传【了解】

#### 目标

- 使用SpringMVC上传文件



#### 1. SpringMVC上传文件

1. springMVC.xml

   ```xml
   <!-- 3. 注册文件解析器 -->
   <!--
           id: 必须取名: multipartResolver
       -->
   <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
       <property name="maxUploadSize" value="10485760‬"/>
   </bean>
   ```

2. com.itheima.json.UploadController

   ```java
       /**
        * SpringMVC的文件上传
        *  提示: 一但使用了解析器, 那么传统的文件都将被解析成了MultipartFile对象
        *
        * @param req   the req
        * @param file  参数名称必须与前端的字段名称一致
        */
       @RequestMapping("upload")
       public String list(HttpServletRequest req, MultipartFile file, Model model) throws Exception {
           // 1. 获取文件上传路径
           String root = req.getSession().getServletContext().getRealPath("upload");
           File rootFile = new File(root);
           if(!rootFile.exists()){
               rootFile.mkdir();
           }
   
           // 2. 拼接全文件名 (root/UUID.filename)
           // 乱码问题: 也可以使用乱码过滤器解决
           String name = new String(file.getOriginalFilename().getBytes("ISO-8859-1"), "UTF-8");
           String filename = root + "/" + UUID.randomUUID().toString() + "." + name;
           model.addAttribute("msg", filename);
           File f = new File(filename);
           file.transferTo(f);
   
           // 3. 响应结果
           return "success";
       }
   ```



#### 小结

- multipartResolver的作用是什么?
  - 解析请求对象中的文件内容封装multipartFile对象



### 07异常处理方案【理解】

#### 目标

- 理解3层架构的代码调用关系
- 分析3层架构的异常处理方案



#### 1. 代码调用关系

![1567403737180](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springMVC二/1567403737180.png)

#### 2. 异常处理方案

1. 模拟视图层异常: com.itheima.json.ExceptionController

   ```java
   package com.itheima.json;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.ui.Model;
   import org.springframework.web.bind.annotation.RequestMapping;
   
   /**
    * 异常控制器.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   @Controller
   public class ExceptionController {
   
   
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
   }
   
   ```

2. 提供成功页面: pages/success.jsp

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

3. 提供错误页面: pages/error.jsp

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

   

##### 2.1 代码处理

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

##### 2.2 过滤器处理

1. 创建过滤器: com.itheima.json.filter.ExceptionFilter

  ```java
package com.itheima.json.filter;

import javax.servlet.*;
import java.io.IOException;

/**
 * 异常处理过滤器.
 * 异常处理: 第二种解决方案
 *
 * @author : Jason.lee
 * @version : 1.0
 */
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

2. 配置过滤器: web.xml

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

##### 1.3 框架处理

- 欲知框架如何, 请看下回分解 !



#### 小结

- 3层架构中的代码调用关系？
  - **controller**->service->dao
- 代码捕捉方式有什么缺点?
  - 代码重复性高



### 08统一异常处理【掌握】

#### 目标

- SpringMVC的异常源码分析
- SpringMVC的统一异常处理



#### 1. 框架处理分析

- 【源码】org.springframework.web.servlet.DispatcherServlet

  ```java
  private void processDispatchResult(HttpServletRequest request, HttpServletResponse response, @Nullable HandlerExecutionChain mappedHandler, @Nullable ModelAndView mv, @Nullable Exception exception) throws Exception {
      boolean errorView = false;
      if (exception != null) {
          if (exception instanceof ModelAndViewDefiningException) {
              this.logger.debug("ModelAndViewDefiningException encountered", exception);
              mv = ((ModelAndViewDefiningException)exception).getModelAndView();
          } else {
              Object handler = mappedHandler != null ? mappedHandler.getHandler() : null;
              // 异常处理方法【执行出现异常将获取异常解析器处理异常】
              mv = this.processHandlerException(request, response, handler, exception);
              errorView = mv != null;
          }
      }
  
      if (mv != null && !mv.wasCleared()) {
          this.render(mv, request, response);
          if (errorView) {
              WebUtils.clearErrorRequestAttributes(request);
          }
      } else if (this.logger.isTraceEnabled()) {
          this.logger.trace("No view rendering, null ModelAndView returned.");
      }
  
      if (!WebAsyncUtils.getAsyncManager(request).isConcurrentHandlingStarted()) {
          if (mappedHandler != null) {
              mappedHandler.triggerAfterCompletion(request, response, (Exception)null);
          }
  
      }
  }
  ```

- 【源码】org.springframework.web.servlet.HandlerExceptionResolver

  ```java
  @Nullable
  ModelAndView resolveException(HttpServletRequest var1, HttpServletResponse var2, @Nullable Object var3, Exception var4);
  ```

#### 2. 统一异常处理

- 创建异常处理器: com.itheima.json.ex.ExceptionHandler

  ```java
  package com.itheima.json.ex;
  
  import org.springframework.stereotype.Component;
  import org.springframework.web.servlet.HandlerExceptionResolver;
  import org.springframework.web.servlet.ModelAndView;
  
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  
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



#### 小结

- 定义多个异常处理类都会生效吗?
  - 不会
- SpringMVC异常处理的底层原理?
  - try/catch



### 09自定义拦截器【理解】

#### 目标

- 自定义SpringMVC拦截器
- 拦截器与过滤器的区别



#### 1.  自定义拦截器

1. 创建拦截器: com.itheima.json.interceptor.CustomInterceptor

   ```java
   package com.itheima.json.interceptor;
   
   import org.springframework.web.servlet.HandlerInterceptor;
   import org.springframework.web.servlet.ModelAndView;
   
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   
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

2. 配置拦截器: springMVC.xml

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



#### 2. 拦截器与过滤器

| 组件名称 | 组件来源       | 应用范围              |
| -------- | -------------- | --------------------- |
| 拦截器   | SpringMVC      | Controller方法        |
| 过滤器   | Servlet2.3规范 | 所有WEB资源 ( **/** ) |



#### 小结

- 拦截器与过滤器的区别?
  - 作用范围不同, 过滤器可以拦截所有资源, 拦截器只能拦截控制器的资源



### 10总结

1. 使用Json交互需要什么依赖?
   - jackson-databind
2. @RequestBody的作用?
   - 将json数据转换成java对象
3. @ResponseBody的作用?
   - 将方法返回值对象转换成json数据
4. @PathVariable的作用?
   - 修饰参数列表: 获取uri当中的参数值
5. 为什么配置multipartResolver?
   - 需要做文件上传功能: 作用是解析请求中的文件内容
6. 如何对SSM项目做统一异常处理?
   - 使用SpringMVC框架自带的处理器异常解析器 (实现处理器异常解析器接口并创建对象放置IOC容器)
7. 过滤器与拦截器的区别是什么?
   - 作用范围不同, 过滤器可以拦截所有资源, 拦截器只能拦截控制器的资源



### 01复习

#### 目标

- 了解常见的SSM项目



#### 1. 了解SSM项目

1. SSM项目指的是项目在技术的选择上采取了 SpringMVC+Spring+Mybatis 等框架
2. 在三层架构中, SSM架构比较主流, 且各框架分工明确:
   - SpringMVC: 处理视图层的用户请求
   - Spring: 处理业务层的事务管理
   - Mybatis: 处理持久层的数据操作



#### 小结

- 什么是SSM项目?

  - 在技术选型上采取了SpringMVC+Spring+Mybatis结构的项目



### 02SSM - 框架整合【掌握】

#### 目标

- 了解整合目的
- 整合步骤分析



#### 1. 了解整合目的

- 为什么要整合?

  >  在3层架构项目中, 各层都需要相应的解决方案, 所以SSM需要配合使用。



#### 2. 整合步骤分析

- 在3层架构中, 哪些对象需要创建?
  - Controller, Service, Dao, JdbcTemlate, DataSource, SqlSession...
- 在3层架构中, 哪层需要Spring框架?
  - 都需要

1. 使Spring环境能够独立运行
2. 使SpringMVC能够独立运行
3. 使Spring+SpringMVC共同运行
4. 使Mybatis能够独立运行
5. 使SSM共同运行
6. 整体测试





#### 小结

- 三层架构中哪层需要用到Spring?
  - 都需要



### 03SSM - 环境准备【理解】

#### 目标

- 准备数据环境
- 准备项目环境



#### 1. 准备数据环境

1. 数据库: mybatisdb

2. 表名称: account

   ```sql
   DROP TABLE IF EXISTS `account`;
   CREATE TABLE `account` (
     `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '主键',
     `uid` int(11) DEFAULT '1' COMMENT '用户编号',
     `money` decimal(10,2) DEFAULT '0.00' COMMENT '余额',
     PRIMARY KEY (`id`)
   ) ENGINE=InnoDB AUTO_INCREMENT=111 DEFAULT CHARSET=utf8;
   
   -- ----------------------------
   -- Records of account
   -- ----------------------------
   INSERT INTO `account` VALUES ('1', '1', '10.00');
   INSERT INTO `account` VALUES ('2', '10', '0.00');
   INSERT INTO `account` VALUES ('3', '24', '99.00');
   ```

#### 2. 准备项目环境

1. 工程名称: mvc03_ssm_02

2. 添加依赖: pom.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
   
       <groupId>com.itheima</groupId>
       <artifactId>mvc03_ssm_02</artifactId>
       <version>1.0-SNAPSHOT</version>
   
   
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
           <!-- 1. mybatis-Spring依赖 -->
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
   
   
   
   </project>
   ```

3. 实体类: com.itheima.ssm.domain.Account

   ```java
   package com.itheima.ssm.domain;
   
   /**
    * 账户类.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class Account {
   
       private Integer id;
       private Integer uid;
       private Double money;
   
       public Integer getId() {
           return id;
       }
   
       public void setId(Integer id) {
           this.id = id;
       }
   
       public Integer getUid() {
           return uid;
       }
   
       public void setUid(Integer uid) {
           this.uid = uid;
       }
   
       public Double getMoney() {
           return money;
       }
   
       public void setMoney(Double money) {
           this.money = money;
       }
   
       @Override
       public String toString() {
           return "Account{" +
                   "id=" + id +
                   ", uid=" + uid +
                   ", money=" + money +
                   '}';
       }
   }
   
   ```

4. 数据库配置: db.properties

5. 日志配置: log4j.properties



#### 小结

- 非必需的依赖有哪些?
  - servlet相关, log4j相关



### 04SSM - Spring【理解】

#### 目标

- 搭建Spring独立环境



#### 1. 搭建Spring独立环境

1. 业务类: com.itheima.ssm.service.AccountService

   ```java
   package com.itheima.ssm.service;
   
   import com.itheima.ssm.domain.Account;
   import org.springframework.stereotype.Service;
   import org.springframework.transaction.annotation.Transactional;
   
   import java.util.ArrayList;
   import java.util.List;
   
   /**
    * 账户业务类.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   @Service
   public class AccountService {
   
   
       /**
        * 保存账户
        */
       @Transactional
       public void save(Account account){
           System.out.println("已经保存: "+account);
       }
       /**
        * 查询所有账户
        * @return 查询结果
        */
       public List<Account> findAll(){
           return new ArrayList<>();
       }
   
   }
   
   ```

2. applicationContext.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xmlns:tx="http://www.springframework.org/schema/tx"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context
           http://www.springframework.org/schema/context/spring-context.xsd
           http://www.springframework.org/schema/aop
           http://www.springframework.org/schema/aop/spring-aop.xsd
           http://www.springframework.org/schema/tx
           http://www.springframework.org/schema/tx/spring-tx.xsd">
   
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
   
   </beans>
   ```

3. 单元测试: SsmTests

   ```java
   import com.itheima.ssm.domain.Account;
   import com.itheima.ssm.service.AccountService;
   import org.junit.Test;
   import org.junit.runner.RunWith;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.test.context.ContextConfiguration;
   import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
   
   /**
    * 框架整合的代码测试.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   @RunWith(SpringJUnit4ClassRunner.class)
   @ContextConfiguration(locations = "classpath:applicationContext.xml")
   public class SsmTests {
   
       @Autowired
       AccountService accountService;
   
   
       @Test
       public void testSpring (){
           System.out.println("=================="+accountService.getClass());
           Account account = new Account();
           account.setId(10);
           account.setUid(10);
           account.setMoney(10D);
           accountService.save(account);
       }
   }
   
   ```

   

#### 小结

- @RunWith的作用是什么?
  - 启动IOC容器



### 05SSM - SpringMVC【理解】

#### 目标

- 搭建SpringMVC独立环境



#### 1. 搭建SpringMVC环境

1. 升级工程: war

2. 配置前端控制器: web.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   	xmlns="http://java.sun.com/xml/ns/javaee"
   	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
   	http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
   	version="2.5">
   
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
   
   </web-app>
   ```

3. 添加配置: springMVC.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          http://www.springframework.org/schema/context/spring-context.xsd
          http://www.springframework.org/schema/mvc
          http://www.springframework.org/schema/mvc/spring-mvc.xsd">
   
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

4. 控制器: com.itheima.ssm.controller.AccountController

   ```java
   package com.itheima.ssm.controller;
   
   import com.itheima.ssm.domain.Account;
   import org.springframework.stereotype.Controller;
   import org.springframework.ui.Model;
   import org.springframework.web.bind.annotation.RequestMapping;
   
   import java.util.ArrayList;
   
   /**
    * 账户控制器.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   @Controller
   public class AccountController {
   
   
       /**
        * 保存账户
        * @param account
        * @param model
        * @return
        */
       @RequestMapping("save")
       public String list(Account account, Model model){
           return "redirect:/list";
       }
   
   
       /**
        * 查询账户
        * @param model
        * @return
        */
       @RequestMapping("list")
       public String list(Model model){
           model.addAttribute("list", new ArrayList<>());
           return "account/list";
       }
   }
   
   ```

5. 添加页面

   - pages/account/list.jsp

     ```jsp
     <%@ page contentType="text/html;charset=UTF-8" language="java" %>
     <html>
     <head>
         <title>账户列表</title>
     </head>
     <body>
         <h3>账户列表</h3>
         ${list}
     </body>
     </html>
     ```

   - pages/account/add.jsp

     ```jsp
     <%@ page contentType="text/html;charset=UTF-8" language="java" %>
     <html>
     <head>
         <title>账户注册</title>
     </head>
     <body>
         <h3>账户注册</h3>
         ...
     </body>
     </html>
     ```

6. 单元测试: http://localhost:8080/list



#### 小结

- MVC容器是谁启动的?
  - Tomcat容器启动的



### 06SSM - 整合SS【理解】

#### 目标

- 整合Spring+SpringMVC框架



#### 1. 整合SS框架

​	![1567420138244](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springMVC三/1567420138244.png) 

1. 启动 **IOC** 容器: web.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   	xmlns="http://java.sun.com/xml/ns/javaee"
   	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
   	http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
   	version="2.5">
   
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
   
   
   
   
   	<!-- 1. 提供监听器: 监听tomcat启动的时间, 当tomcat启动时, 会创建SpringIOC容器, 并且将MVC容器添加到IOC容器中 -->
   	<!-- 父容器: SpringIOC容器 -->
   	<!-- 子容器: MVC容器 -->
   	<listener>
   		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
   	</listener>
   
   	<!-- 2. 配置默认加载的参数: 添加主配置文件 -->
   	<context-param>
   		<param-name>contextConfigLocation</param-name>
   		<param-value>classpath:applicationContext.xml</param-value>
   	</context-param>
   	
   	
   </web-app>
   ```

2. 注入业务对象: com.itheima.ssm.controller.AccountController

   ```java
   package com.itheima.ssm.controller;
   import com.itheima.ssm.domain.Account;
      import com.itheima.ssm.service.AccountService;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.stereotype.Controller;
      import org.springframework.ui.Model;
      import org.springframework.web.bind.annotation.RequestMapping;
   
      import java.util.ArrayList;
   
      /**
       * 账户控制器.
       *
       * @author : Jason.lee
       * @version : 1.0
       */
      @Controller
      public class AccountController {
             @Autowired
      AccountService accountService;
      /**
       * 保存账户
       * @param account
       * @param model
       * @return
       */
      @RequestMapping("save")
      public String list(Account account, Model model){
          accountService.save(account);
          return "redirect:/list";
      }
      /**
       * 查询账户
       * @param model
       * @return
       */
      @RequestMapping("list")
      public String list(Model model){
          model.addAttribute("list", accountService.findAll());
          return "account/list";
      }
   }
   ```

#### 小结

- ContextLoaderListener的作用?
  - 监听tomcat启动时间, 在tomcat启动时创建IOC容器

- 父子容器启动的顺序是怎样的?
  - 先启动父容器, 再启动子容器



### 07SSM - Mybatis【理解】

#### 目标

- 搭建Mybatis独立环境



#### 1. 搭建Mybatis环境

1. 持久类: com.itheima.ssm.dao.AccountMapper

   ```Java
   package com.itheima.ssm.dao;
   
   import com.itheima.ssm.domain.Account;
   import org.apache.ibatis.annotations.Insert;
   import org.apache.ibatis.annotations.Select;
   
   import java.util.List;
   
   /**
    * 账户持久层接口 (账户操作类).
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public interface AccountMapper {
   
   
       /**
        * 查询所有账户.
        *
        * @return 返回结果
        */
       @Select("select * from account")
       List<Account> findAll();
   
       /**
        * 保存账户.
        *
        * @param account the account
        */
       @Insert("insert into account values(#{id},#{uid},#{money})")
       void save(Account account);
   }
   
   ```

2. sqlMapConfig.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
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
   
   </configuration>
   ```

3. 单元测试: SsmTests

   ```java
   @Test
   public void testMybatis () throws Exception {
       // 1. 加载主配置文件
       InputStream in = Resources.getResourceAsStream("sqlMapConfig.xml");
       // 2. 创建SqlSessionFactory对象
       SqlSessionFactory sessionFactory = new SqlSessionFactoryBuilder().build(in);
       // 3. 创建SqlSession对象
       SqlSession sqlSession = sessionFactory.openSession();
       // 4. 创建Mapper对象
       AccountMapper accountMapper = sqlSession.getMapper(AccountMapper.class);
       // 5. 操作数据库
       List<Account> all = accountMapper.findAll();
       all.forEach(x-> System.out.println(x));
   }
   ```



#### 小结

- 启动Mybatis框架需要做什么?
  - 创建会话对象
  - 创建代理对象



### 08SSM - 整合SM【理解】

#### 目标

- 整合Spring+Mybatis框架



#### 1. 整合SM框架

> ​	启动Mybatis框架只需要做两件事
>
> 1. 创建会话工厂 
> 2. 创建代理对象

1. 创建会话工厂: applicationContext.xml

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
   ```

2. 创建代理对象: applicationContext.xml

   ```xml
   <!-- 2. 创建代理对象:MapperScan... -->
   <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
       <!-- 注入代理接口所在的包路径 -->
       <property name="basePackage" value="com.itheima.ssm.dao"/>
   </bean>
   ```

3. 注入持久对象: com.itheima.ssm.service.AccountService

   ```java
   package com.itheima.ssm.service;
   
   import com.itheima.ssm.dao.AccountMapper;
   import com.itheima.ssm.domain.Account;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.stereotype.Service;
   import org.springframework.transaction.annotation.Transactional;
   
   import java.util.ArrayList;
   import java.util.List;
   
   /**
    * 账户业务类.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   @Service
   public class AccountService {
   
       @Autowired
       AccountMapper accountMapper;
   
       /**
        * 保存账户
        */
       @Transactional
       public void save(Account account){
           accountMapper.save(account);
       }
       /**
        * 查询所有账户
        * @return 查询结果
        */
       public List<Account> findAll(){
           return accountMapper.findAll();
       }
   
   }
   
   ```



#### 小结

- Mybatis是谁启动的?
  - Spring 启动的



### 09SSM - 整体测试【了解】

#### 目标

- 改造前后端代码进行整体测试



#### 1. 项目改造

1. pages/account/list.jsp

   ```jsp
   <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
   <%--
     Created by IntelliJ IDEA.
     User: Jason
     Date: 2019/11/27
     Time: 10:05
     To change this template use File | Settings | File Templates.
   --%>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>账户列表</title>
   </head>
   <body>
       <h3>账户列表</h3>
       <table border="1">
           <tr>
               <th>序号</th>
               <th>编号</th>
               <th>用户编号</th>
               <th>账户余额</th>
           </tr>
           <c:forEach items="${list}" var="acc" varStatus="i">
               <tr>
                   <td>${i.count}</td>
                   <td>${acc.id}</td>
                   <td>${acc.uid}</td>
                   <td>${acc.money}</td>
               </tr>
           </c:forEach>
       </table>
   </body>
   </html>
   
   ```

2. pages/account/add.jsp

   ```jsp
   <%--
     Created by IntelliJ IDEA.
     User: Jason
     Date: 2019/11/27
     Time: 10:06
     To change this template use File | Settings | File Templates.
   --%>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>注册账户</title>
   </head>
   <body>
       <form method="post" action="add">
           <input type="text" name="id" value="10"/>
           <input type="text" name="uid" value="10"/>
           <input type="text" name="money" value="10"/>
           <input type="submit"/>
       </form>
   </body>
   </html>
   
   ```



#### 小结

- mybatis-spring依赖的作用?

  - 提供会话对象创建工具和代理对象创建工具的包



### 10总结

1. 什么是SSM ?

   - SpringMVC+Spring+Mybatis

2. 整合Spring+SpringMVC需要额外添加依赖吗?

   - 不需要, 无缝集成

3. 整合Spring+Mybatis需要额外添加依赖吗?

   - 需要mybatis-spring

4. SpringMVC是谁启动的?

   - tomcat

5. Spring是谁启动的?

   - tomcat

6. Mybatis是谁启动的?

   - spring

7. SpringMVC的三大组件有哪些?

   - 处理器映射器

   - 处理器适配器
   - 视图解析器

8. SpringMVC的工作原理?

   - 用户发起请求: hello.do
   - 前端控制器处理请求: hello.do
   - 获取处理器映射器并且匹配url相应的方法: method
   - 获取处理器适配器: ha
   - 调用处理器适配器执行控制器方法(method): ha.handler(..
   - 获得ModelAndView, 并且处理结果
   - 调用视图解析器解析视图: view
   - 渲染视图: 合并/填充模型数据到视图中
   - 转发/重定向资源给Tomcat
   - 响应客户

