# 介绍

![1526187409033](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Nginx/1526187409033.png)

nginx可以作为web服务器，但更多的时候，我们把它作为网关，因为它具备网关必备的功能：

- 反向代理
- 负载均衡
- 动态路由
- 请求过滤

## Nginx作为web服务器

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



## Nginx作为反向代理

什么是反向代理？

- 代理：通过客户机的配置，实现让一台服务器代理客户机，客户的所有请求都交给代理服务器处理。
- 反向代理：用一台服务器，代理真实服务器，用户访问时，不再是访问真实服务器，而是代理服务器。

nginx可以当做反向代理服务器来使用：

- 我们需要提前在nginx中配置好反向代理的规则，不同的请求，交给不同的真实服务器处理
- 当请求到达nginx，nginx会根据已经定义的规则进行请求的转发，从而实现路由功能



利用反向代理，就可以解决我们前面所说的端口问题，如图

如果是安装在本机：

![1526016663674](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/项目二/跨域问题/1526016663674.png) 



## Nginx的反向代理配置

先安装Nginx，安装非常简单，把课前资料提供的nginx直接解压即可，绿色免安装，舒服！

![img](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/项目二/跨域问题/xiaoyaya.gif) 



把软件解压到本地某个目录即可，目录结构：

![1574996389482](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/项目二/跨域问题/1574996389482.png) 



> ### 使用

nginx可以通过命令行来启动，操作命令：

- 启动：`start nginx.exe`
- 停止：`nginx.exe -s stop`
- 重新加载：`nginx.exe -s reload`

我们需要让nginx反向代理我们的服务器，因此需要自定义nginx配置。

![1574996542375](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/项目二/跨域问题/1574996542375.png) 

为了看起来清爽，我们先在nginx主配置文件`nginx.conf`中使用include指令引用我们的配置：

```
	include vhost/*.conf;
```

如图所示：

![1574996632209](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/项目二/跨域问题/1574996632209.png) 

然后在nginx.conf所在目录新建文件夹vhost：

![1574996714707](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/项目二/跨域问题/1574996714707.png) 

并在vhost中创建文件leyou.conf：

![1574997008925](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/项目二/跨域问题/1574997008925.png) 

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

![1574997922919](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/项目二/跨域问题/1574997922919.png) 

现在实现了域名访问网站了，中间的流程是怎样的呢？

![1575000188871](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/项目二/跨域问题/1575000188871.png) 

1. 浏览器准备发起请求，访问http://mamage.leyou.com，但需要进行域名解析

2. 优先进行本地域名解析，因为我们修改了hosts，所以解析成功，得到地址：127.0.0.1（本机）

3. 请求被发往解析得到的ip，并且默认使用80端口：http://127.0.0.1:80

   本机的nginx一直监听80端口，因此捕获这个请求

4. nginx中配置了反向代理规则，将manage.leyou.com代理到http://127.0.0.1:9001

5. 主机上的后台系统的webpack server监听的端口是9001，得到请求并处理，完成后将响应返回到nginx

6. nginx将得到的结果返回到浏览器

## 配置详解

语法规则： `location [=|~|~*|^~] /uri/ { … }`

- `=` 开头表示精确匹配
- `^~` 开头表示uri以某个常规字符串开头，理解为匹配 url路径即可。nginx不对url做编码，因此请求为/static/20%/aa，可以被规则^~ /static/ /aa匹配到（注意是空格）。以xx开头
- `~` 开头表示区分大小写的正则匹配           以xx结尾
- `~*` 开头表示不区分大小写的正则匹配        以xx结尾
- `!~`和`!~*`分别为区分大小写不匹配及不区分大小写不匹配 的正则
- `/` 通用匹配，任何请求都会匹配到。

多个location配置的情况下匹配顺序为（参考资料而来，还未实际验证，试试就知道了，不必拘泥，仅供参考）：

![img](https://img-blog.csdn.net/20180219195126400?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM4NjI2NDQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

首先精确匹配 =-》其次以xx开头匹配^~-》然后是按文件中顺序的正则匹配-》最后是交给 / 通用匹配。

当有匹配成功时候，停止匹配，按当前匹配规则处理请求。

例子，有如下匹配规则：

```php
# http://localhost/ 
location = / {
   #规则A
}
# http://localhost/login 
location = /login {
   #规则B
}
# http://localhost/static/a.html 
# http://localhost/static/c.png
location ^~ /static/ {
   #规则C
}
# http://localhost/a.gif
location ~ \.(gif|jpg|png|js|css)$ {
   #规则D，注意：是根据括号内的大小写进行匹配。括号内全是小写，只匹配小写
}
# http://localhost/b.jpg
# http://localhost/a.PNG 
location ~* \.png$ {
   #规则E
}
location !~ \.xhtml$ {
   #规则F
}
location !~* \.xhtml$ {
   #规则G
}
# http://localhost/register 
# http://localhost/category/id/1111
location / {
   #规则H
}
```

那么产生的效果如下：

访问根目录/， 比如http://localhost/ 将匹配规则A

访问 http://localhost/login 将匹配规则B，http://localhost/register 则匹配规则H

访问 http://localhost/static/a.html 将匹配规则C

访问 http://localhost/a.gif, http://localhost/b.jpg 将匹配规则D和规则E，但是规则D顺序优先，规则E不起作用， 而 http://localhost/static/c.png 则优先匹配到 规则C

访问 http://localhost/a.PNG 则匹配规则E， 而不会匹配规则D，因为规则E不区分大小写。

访问 http://localhost/a.xhtml 不会匹配规则F和规则G，

http://localhost/a.XHTML不会匹配规则G，（因为!）。规则F，规则G属于排除法，符合匹配规则也不会匹配到，所以想想看实际应用中哪里会用到。

访问 http://localhost/category/id/1111 则最终匹配到规则H，因为以上规则都不匹配，这个时候nginx转发请求给后端应用服务器，比如FastCGI（php），tomcat（jsp），nginx作为方向代理服务器存在。

所以实际使用中，个人觉得至少有三个匹配规则定义，如下：

```html
#直接匹配网站根，通过域名访问网站首页比较频繁，使用这个会加速处理，官网如是说。
#这里是直接转发给后端应用服务器了，也可以是一个静态首页
# 第一个必选规则
location = / {
    proxy_pass http://tomcat:8080/index
}
# 第二个必选规则是处理静态文件请求，这是nginx作为http服务器的强项
# 有两种配置模式，目录匹配或后缀匹配,任选其一或搭配使用
location ^~ /static/ {                              //以xx开头
    root /webroot/static/;
}
location ~* \.(gif|jpg|jpeg|png|css|js|ico)$ {     //以xx结尾
    root /webroot/res/;
}
#第三个规则就是通用规则，用来转发动态请求到后端应用服务器
#非静态文件请求就默认是动态请求，自己根据实际把握
location / {
    proxy_pass http://tomcat:8080/
}
```