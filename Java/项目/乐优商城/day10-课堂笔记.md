## 01、课程目标

- 了解RabbitMQ的基本使用
- 能够发送MQ消息
- 能够编写监听器，接收MQ消息
- 会使用SpringDataRedis































## 02、页面静态化：实现页面静态化

本次课程我们采用Thymeleaf来实现页面静态化。

### 1）概念

先说下Thymeleaf中的几个概念：

- Context：运行上下文
- TemplateResolver：模板解析器
- TemplateEngine：模板引擎

> Context

上下文： 用来保存模型数据，当模板引擎渲染时，可以从Context上下文中获取数据用于渲染。

当与SpringBoot结合使用时，我们放入Model的数据就会被处理到Context，作为模板渲染的数据使用。

> TemplateResolver

模板解析器：用来读取模板相关的配置，例如：模板存放的位置信息，模板文件名称，模板文件的类型等等。

当与SpringBoot结合时，TemplateResolver已经由其创建完成，并且各种配置也都有默认值，比如模板存放位置，其默认值就是：templates。比如模板文件类型，其默认值就是html。

> TemplateEngine

模板引擎：用来解析模板的引擎，需要使用到上下文、模板解析器。分别从两者中获取模板中需要的数据，模板文件。然后利用内置的语法规则解析，从而输出解析后的文件。来看下模板引起进行处理的函数：

```java
templateEngine.process("模板名", context, writer);
```

三个参数：

- 模板名称
- 上下文：里面包含模型数据
- writer：输出目的地的流

在输出时，我们可以指定输出的目的地，如果目的地是Response的流，那就是网络响应。如果目的地是本地文件，那就实现静态化了。

而在SpringBoot中已经自动配置了模板引擎，因此我们不需要关心这个。现在我们做静态化，就是把输出的目的地改成本地文件即可！







### 2）具体实现

我们在PageService中，编写方法，实现静态化功能：

```java
/**
 * @author 黑马程序员
 */
@Slf4j
@Service
public class PageService {

    @Autowired
    private ItemClient itemClient;

    @Autowired
    private SpringTemplateEngine templateEngine;

    @Value("${ly.static.itemDir}")
    private String itemDir;
    @Value("${ly.static.itemTemplate}")
    private String itemTemplate;

    public Map<String, Object> loadItemData(Long id) {
        // 查询spu
        SpuDTO spu = itemClient.querySpuById(id);
        // 查询分类集合
        List<CategoryDTO> categories = itemClient.queryCategoryByIds(spu.getCategoryIds());
        // 查询品牌
        BrandDTO brand = itemClient.queryBrandById(spu.getBrandId());
        // 查询规格
        List<SpecGroupDTO> specs = itemClient.querySpecsByCid(spu.getCid3());
        // 封装数据
        Map<String, Object> data = new HashMap<>();
        data.put("categories", categories);
        data.put("brand", brand);
        data.put("spuName", spu.getName());
        data.put("subTitle", spu.getSubTitle());
        data.put("skus", spu.getSkus());
        data.put("detail", spu.getSpuDetail());
        data.put("specs", specs);
        return data;
    }

    public void createItemHtml(Long id) {
        // 上下文，准备模型数据
        Context context = new Context();
        // 调用之前写好的加载数据方法
        context.setVariables(loadItemData(id));
        // 准备文件路径
        File dir = new File(itemDir);
        if (!dir.exists()) {
            if (!dir.mkdirs()) {
                // 创建失败，抛出异常
                log.error("【静态页服务】创建静态页目录失败，目录地址：{}", dir.getAbsolutePath());
                throw new LyException(ExceptionEnum.DIRECTORY_WRITER_ERROR);
            }
        }
        File filePath = new File(dir, id + ".html");
        // 准备输出流
        try (PrintWriter writer = new PrintWriter(filePath, "UTF-8")) {
            templateEngine.process(itemTemplate, context, writer);
        } catch (IOException e) {
            log.error("【静态页服务】静态页生成失败，商品id：{}", id, e);
            throw new LyException(ExceptionEnum.FILE_WRITER_ERROR);
        }
    }
}
```

在application.yml中配置生成静态文件的目录：

```yaml
ly:
  static:
    itemDir: D:\\leyou\\software\\nginx-1.16.0\\html\\item
    itemTemplate: item
```



### 3）单元测试

我们编写一个单元测试：

```java
package com.leyou.page.service;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

import static org.junit.Assert.*;

@RunWith(SpringRunner.class)
@SpringBootTest
public class PageServiceTest {

    @Autowired
    private PageService pageService;

    @Test
    public void createItemHtml() throws InterruptedException {
        Long[] arr = {96L, 114L, 124L, 125L, 141L};
        for (Long id : arr) {
            pageService.createItemHtml(id);
            Thread.sleep(500);
        }
    }
}
```

然后到nginx下查看：

![1576895203444](图片二/1576895203444.png) 



### 4）nginx代理静态页面

接下来，我们修改nginx，让它对商品请求进行监听，指向本地静态页面，如果本地没找到，才进行反向代理：

```nginx
server {
	listen       80;
	server_name  www.leyou.com;
	location /item {
		# 先找本地
        root html;
        if (!-f $request_filename) { #请求的文件不存在，就反向代理
            proxy_pass http://127.0.0.1:8084;
            break;
        }
	}
	location / {
		proxy_pass   http://leyou-portal;
	}
}
```

重启测试：

发现请求速度得到了极大提升：

![1576895534563](图片二/1576895534563.png) 













## 03、页面静态化：缓存击穿问题



**`注意！！！`**，这里为了演示方便我们采用了if判断文件是否存在，不存在则反向代理到服务端，服务端生成页面返回。这种做法有`缓存穿透`的风险，建议不要加入if判断，如果nginx中不存在商品的静态页，直接返回404即可。

即：

```nginx
server {
	listen       80;
	server_name  www.leyou.com;
	location /item {
        root html;
	}
	location / {
		proxy_pass   http://leyou-portal;
	}
}
```





`缓存穿透`：缓存穿透是当缓存没有走数据库，黑客使用数据库和缓存都没有的id，例如0，这样请求就会落到数据库，批量请求落到数据库导致数据库宕机。

`缓存雪崩`：设置缓存定时的时候，大家都到期了，导致所有的请求都落到数据库，解决方案失效时间分散一点，不要都是同一时间段



错误页面设置：

```nginx
server {
	listen       80;
	server_name  www.leyou.com;
	
	error_page   404 500 502 503 504  50x.html;
	
	location /item {
        root html;
	}
	
	location / {
	    proxy_pass   http://leyou-portal;
		proxy_connect_timeout 600;
		proxy_read_timeout 5000;
	}
}
```















## 04、页面静态化：数据同步问题

我们思考这样几个问题：

- 商品详情页应该在什么时候生成呢？不能每次都用单元测试生成吧？
- 如果商品数据修改以后，静态页的内容与商品实际内容不符，该如何完成同步？
- 商品下架后，不应该再展示商品页面，静态页如何处理？



思考上面问题的同时，我们会想起一件事情，其实商品数据如果发生了增、删、改，不仅仅静态页面需要处理，我们的索引库数据也需要同步！！这又该如何解决？



因为商品新增后需要上架用户才能看到，商品修改需要先下架，然后修改，再上架。因此上述问题可以统一的设计成这样的逻辑处理：

- 商品上架：
  - 生成静态页
  - 新增索引库数据
- 商品下架：
  - 删除静态页
  - 删除索引库数据

这样既可保证数据库商品与索引库、静态页三者之间的数据同步。

那么，如何实现上述逻辑呢？

就是使用我们之前学习的RabbitMQ来完成了，解决方案架构图：

![1576899268757](图片二/1576899268757.png) 

MQ：两大功能，业务解耦合，流量削峰。

上一节中，我们要实现这样的效果：商品上架或下架时，静态页及索引库要做出相应的处理。我们思考一下，是否存在问题？

- 商品数据的上架或下架都在商品微服务（`item-service`）中完成。
- 索引库数据的增删改是在搜索微服务（`search-service`）中完成。
- 商品详情做了页面静态化，静态页面数据的操作是在静态页服务（`page-service`）中完成。

搜索服务和静态页服务，并不知晓商品微服务对商品数据的！！如何解决？



这里有两种解决方案：

- 方案1：在商品微服务的上下架业务后，加入修改索引库数据及静态页面的代码
- 方案2：搜索服务和静态页服务对外提供操作索引库和静态页接口，商品微服务在商品上下架后，调用接口。



以上两种方式都有同一个严重问题：就是代码耦合，后台服务中需要嵌入搜索和商品页面服务，违背了微服务的`独立`原则，而且严重违背了开闭原则。

所以，我们会通过另外一种方式来解决这个问题：消息队列











## 05、数据同步：RabbitMQ回顾

### 1）什么是消息队列

消息队列，即MQ，Message Queue。

![1527063872737](图片二/1527063872737.png)



消息队列是典型的：生产者、消费者模型。生产者不断向消息队列中生产消息，消费者不断的从队列中获取消息。因为消息的生产和消费都是异步的，而且只关心消息的发送和接收，没有业务逻辑的侵入，这样就实现了生产者和消费者的解耦。

结合前面所说的问题：

- 商品服务对商品上下架以后，无需去操作索引库或静态页面，只是发送一条消息，也不关心消息被谁接收。
- 搜索服务和静态页面服务接收消息，分别去处理索引库和静态页面。

如果以后有其它系统也依赖商品服务的数据，同样监听消息即可，商品服务无需任何代码修改。



### 2）AMQP和JMS

MQ是消息通信的模型，并发具体实现。现在实现MQ的有两种主流方式：AMQP、JMS。

![1527064480681](图片二/1527064480681.png)

![1527064487042](图片二/1527064487042.png)



两者间的区别和联系：

- JMS是定义了统一的接口，来对消息操作进行统一；AMQP是通过规定协议来统一数据交互的格式
- JMS限定了必须使用Java语言；AMQP只是协议，不规定实现方式，因此是跨语言的。
- JMS规定了两种消息模型；而AMQP的消息模型更加丰富



### 3）常见MQ产品

![1527064606029](图片二/1527064606029.png)



- ActiveMQ：基于JMS， Apache
- RabbitMQ：基于AMQP协议，erlang语言开发，稳定性好
- RocketMQ：基于JMS，阿里巴巴产品，目前交由Apache基金会
- Kafka：分布式消息系统，高吞吐量

### 4）RabbitMQ

RabbitMQ是基于AMQP的一款消息管理系统

官网： http://www.rabbitmq.com/

官方教程：http://www.rabbitmq.com/getstarted.html

![1576899571915](图片二/1576899571915.png) 

 ![1527064762982](图片二/1527064762982.png)

RabbitMQ基于Erlang语言开发：

![1527065024587](图片二/1527065024587.png)









## 06、数据同步：RabbitMQ安装与五种模型

### 1）下载

官网下载地址：http://www.rabbitmq.com/download.html

课前资料提供了安装包：

![1576900890697](图片二/1576900890697.png) 

### 2）安装

安装，之前的课中已经安装过了，我们这里就忽略了。



### 3）五种模型

RabbitMQ提供了6种消息模型，但是第6种其实是RPC，并不是MQ，因此不予学习。那么也就剩下5种。

但是其实3、4、5这三种都属于订阅模型，只不过进行路由的方式不同。

![1527068544487](图片二/1527068544487.png)



### 4）基本消息模型

#### 说明

官方文档说明：

> RabbitMQ是一个消息的代理者（Message Broker）：它接收消息并且传递消息。
>
> 你可以认为它是一个邮局：当你投递邮件到一个邮箱，你很肯定邮递员会终究会将邮件递交给你的收件人。与此类似，RabbitMQ 可以是一个邮箱、邮局、同时还有邮递员。
>
> 不同之处在于：RabbitMQ不是传递纸质邮件，而是二进制的数据

基本消息模型图：

 ![1527070619131](图片二/1527070619131.png)

在上图的模型中，有以下概念：

- P：生产者，也就是要发送消息的程序
- C：消费者：消息的接受者，会一直等待消息到来。
- queue：消息队列，图中红色部分。类似一个邮箱，可以缓存消息；生产者向其中投递消息，消费者从其中取出消息。

#### 生产者

连接工具类：

```java
public class ConnectionUtil {
    /**
     * 建立与RabbitMQ的连接
     * @return
     * @throws Exception
     */
    public static Connection getConnection() throws Exception {
        //定义连接工厂
        ConnectionFactory factory = new ConnectionFactory();
        //设置服务地址
        factory.setHost("192.168.56.101");
        //端口
        factory.setPort(5672);
        //设置账号信息，用户名、密码、vhost
        factory.setVirtualHost("/leyou");
        factory.setUsername("leyou");
        factory.setPassword("leyou");
        // 通过工程获取连接
        Connection connection = factory.newConnection();
        return connection;
    }
}

```



生产者发送消息：

```java
public class Send {

    private final static String QUEUE_NAME = "simple_queue";

    public static void main(String[] argv) throws Exception {
        // 获取到连接
        Connection connection = ConnectionUtil.getConnection();
        // 从连接中创建通道，使用通道才能完成消息相关的操作
        Channel channel = connection.createChannel();
        // 声明（创建）队列
        channel.queueDeclare(QUEUE_NAME, false, false, false, null);
        // 消息内容
        String message = "Hello World!";
        // 向指定的队列中发送消息
        channel.basicPublish("", QUEUE_NAME, null, message.getBytes());
        
        System.out.println(" [x] Sent '" + message + "'");

        //关闭通道和连接
        channel.close();
        connection.close();
    }
}

```

控制台：

 ![1527072505530](图片二/1527072505530.png)

#### web控制台查看消息

进入队列页面，可以看到新建了一个队列：simple_queue

![1527072699034](图片二/1527072699034.png)

点击队列名称，进入详情页，可以查看消息：

 ![1527072746634](图片二/1527072746634.png)

在控制台查看消息并不会将消息消费，所以消息还在。



#### 消费者获取消息

```java
public class Recv {
    private final static String QUEUE_NAME = "simple_queue";

    public static void main(String[] argv) throws Exception {
        // 获取到连接
        Connection connection = ConnectionUtil.getConnection();
        // 创建通道
        Channel channel = connection.createChannel();
        // 声明队列
        channel.queueDeclare(QUEUE_NAME, false, false, false, null);
        // 定义队列的消费者
        DefaultConsumer consumer = new DefaultConsumer(channel) {
            // 获取消息，并且处理，这个方法类似事件监听，如果有消息的时候，会被自动调用
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, BasicProperties properties,
                    byte[] body) throws IOException {
                // body 即消息体
                String msg = new String(body);
                System.out.println(" [x] received : " + msg + "!");
            }
        };
        // 监听队列，第二个参数：是否自动进行消息确认。
        channel.basicConsume(QUEUE_NAME, true, consumer);
    }
}

```

控制台：

 ![1527072874080](图片二/1527072874080.png)

这个时候，队列中的消息就没了：

![1527072947108](图片二/1527072947108.png)



#### 消费者的消息确认机制

通过刚才的案例可以看出，消息一旦被消费者接收，队列中的消息就会被删除。

那么问题来了：RabbitMQ怎么知道消息被接收了呢？

这就要通过消息确认机制（Acknowlege）来实现了。当消费者获取消息后，会向RabbitMQ发送回执ACK，告知消息已经被接收。不过这种回执ACK分两种情况：

- 自动ACK：消息一旦被接收，消费者自动发送ACK
- 手动ACK：消息接收后，不会发送ACK，需要手动调用

大家觉得哪种更好呢？

这需要看消息的重要性：

- 如果消息不太重要，丢失也没有影响，那么自动ACK会比较方便
- 如果消息非常重要，不容丢失。那么最好在消费完成后手动ACK，否则接收消息后就自动ACK，RabbitMQ就会把消息从队列中删除。如果此时消费者宕机，那么消息就丢失了。

我们之前的测试都是自动ACK的，如果要手动ACK，需要改动我们的代码：

```java
public class Recv2 {
    private final static String QUEUE_NAME = "simple_queue";

    public static void main(String[] argv) throws Exception {
        // 获取到连接
        Connection connection = ConnectionUtil.getConnection();
        // 创建通道
        final Channel channel = connection.createChannel();
        // 声明队列
        channel.queueDeclare(QUEUE_NAME, false, false, false, null);
        // 定义队列的消费者
        DefaultConsumer consumer = new DefaultConsumer(channel) {
            // 获取消息，并且处理，这个方法类似事件监听，如果有消息的时候，会被自动调用
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, BasicProperties properties,
                    byte[] body) throws IOException {
                // body 即消息体
                String msg = new String(body);
                System.out.println(" [x] received : " + msg + "!");
                // 手动进行ACK
                channel.basicAck(envelope.getDeliveryTag(), false);
            }
        };
        // 监听队列，第二个参数false，手动进行ACK
        channel.basicConsume(QUEUE_NAME, false, consumer);
    }
}

```

注意到最后一行代码：

```java
channel.basicConsume(QUEUE_NAME, false, consumer);

```

如果第二个参数为true，则会自动进行ACK；如果为false，则需要手动ACK。方法的声明：

![1527073500352](图片二/1527073500352.png)



### 5）work消息模型

#### 说明

在刚才的基本模型中，一个生产者，一个消费者，生产的消息直接被消费者消费。比较简单。

Work queues，也被称为（Task queues），任务模型。

当消息处理比较耗时的时候，可能生产消息的速度会远远大于消息的消费速度。长此以往，消息就会堆积越来越多，无法及时处理。此时就可以使用work 模型：**让多个消费者绑定到一个队列，共同消费队列中的消息**。队列中的消息一旦消费，就会消失，因此任务是不会被重复执行的。

 ![1527078437166](图片二/1527078437166.png)

角色：

- P：生产者：任务的发布者
- C1：消费者，领取任务并且完成任务，假设完成速度较慢
- C2：消费者2：领取任务并完成任务，假设完成速度快



#### 生产者

生产者与案例1中的几乎一样：

```java
public class Send {
    private final static String QUEUE_NAME = "test_work_queue";

    public static void main(String[] argv) throws Exception {
        // 获取到连接
        Connection connection = ConnectionUtil.getConnection();
        // 获取通道
        Channel channel = connection.createChannel();
        // 声明队列
        channel.queueDeclare(QUEUE_NAME, false, false, false, null);
        // 循环发布任务
        for (int i = 0; i < 50; i++) {
            // 消息内容
            String message = "task .. " + i;
            channel.basicPublish("", QUEUE_NAME, null, message.getBytes());
            System.out.println(" [x] Sent '" + message + "'");

            Thread.sleep(i * 2);
        }
        // 关闭通道和连接
        channel.close();
        connection.close();
    }
}

```

不过这里我们是循环发送50条消息。

#### 消费者1

![1527085386747](图片二/1527085386747.png)

#### 消费者2

![1527085448377](图片二/1527085448377.png)

与消费者1基本类似，就是没有设置消费耗时时间。

这里是模拟有些消费者快，有些比较慢。



接下来，两个消费者一同启动，然后发送50条消息：

![1527085826462](图片二/1527085826462.png)

可以发现，两个消费者各自消费了25条消息，而且各不相同，这就实现了任务的分发。



#### 能者多劳

刚才的实现有问题吗？

- 消费者1比消费者2的效率要低，一次任务的耗时较长
- 然而两人最终消费的消息数量是一样的
- 消费者2大量时间处于空闲状态，消费者1一直忙碌

现在的状态属于是把任务平均分配，正确的做法应该是消费越快的人，消费的越多。

怎么实现呢？

我们可以修改设置，让消费者同一时间只接收一条消息，这样处理完成之前，就不会接收更多消息，就可以让处理快的人，接收更多消息 ：

![1527086103576](图片二/1527086103576.png)

再次测试：

![1527086159534](图片二/1527086159534.png)



### 6）订阅模型分类

订阅模型示意图：

 ![1527086284940](图片二/1527086284940.png)

前面2个案例中，只有3个角色：

- P：生产者，也就是要发送消息的程序
- C：消费者：消息的接受者，会一直等待消息到来。
- queue：消息队列，图中红色部分。类似一个邮箱，可以缓存消息；生产者向其中投递消息，消费者从其中取出消息。

而在订阅模型中，多了一个exchange角色，而且过程略有变化：

- P：生产者，也就是要发送消息的程序，但是不再发送到队列中，而是发给X（交换机）
- C：消费者，消息的接受者，会一直等待消息到来。
- Queue：消息队列，接收消息、缓存消息。
- Exchange：交换机，图中的X。一方面，接收生产者发送的消息。另一方面，知道如何处理消息，例如递交给某个特别队列、递交给所有队列、或是将消息丢弃。到底如何操作，取决于Exchange的类型。Exchange有以下3种类型：
  - Fanout：广播，将消息交给所有绑定到交换机的队列
  - Direct：定向，把消息交给符合指定routing key 的队列
  - Topic：通配符，把消息交给符合routing pattern（路由模式） 的队列

**Exchange（交换机）只负责转发消息，不具备存储消息的能力**，因此如果没有任何队列与Exchange绑定，或者没有符合路由规则的队列，那么消息会丢失！



### 7）订阅模型-Fanout

Fanout，也称为广播。

#### 流程说明

流程图：

 ![1527086564505](图片二/1527086564505.png)

在广播模式下，消息发送流程是这样的：

- 1）  可以有多个消费者
- 2）  每个**消费者有自己的queue**（队列）
- 3）  每个**队列都要绑定到Exchange**（交换机）
- 4）  **生产者发送的消息，只能发送到交换机**，交换机来决定要发给哪个队列，生产者无法决定。
- 5）  交换机把消息发送给绑定过的所有队列
- 6）  队列的消费者都能拿到消息。实现一条消息被多个消费者消费



#### 生产者

两个变化：

- 1）  声明Exchange，不再声明Queue
- 2）  发送消息到Exchange，不再发送到Queue

```java
public class Send {

    private final static String EXCHANGE_NAME = "fanout_exchange_test";

    public static void main(String[] argv) throws Exception {
        // 获取到连接
        Connection connection = ConnectionUtil.getConnection();
        // 获取通道
        Channel channel = connection.createChannel();
        
        // 声明exchange，指定类型为fanout
        channel.exchangeDeclare(EXCHANGE_NAME, "fanout");
        
        // 消息内容
        String message = "Hello everyone";
        // 发布消息到Exchange
        channel.basicPublish(EXCHANGE_NAME, "", null, message.getBytes());
        System.out.println(" [生产者] Sent '" + message + "'");

        channel.close();
        connection.close();
    }
}

```

#### 消费者1

```java
public class Recv {
    private final static String QUEUE_NAME = "fanout_exchange_queue_1";

    private final static String EXCHANGE_NAME = "fanout_exchange_test";

    public static void main(String[] argv) throws Exception {
        // 获取到连接
        Connection connection = ConnectionUtil.getConnection();
        // 获取通道
        Channel channel = connection.createChannel();
        // 声明队列
        channel.queueDeclare(QUEUE_NAME, false, false, false, null);

        // 绑定队列到交换机
        channel.queueBind(QUEUE_NAME, EXCHANGE_NAME, "");

        // 定义队列的消费者
        DefaultConsumer consumer = new DefaultConsumer(channel) {
            // 获取消息，并且处理，这个方法类似事件监听，如果有消息的时候，会被自动调用
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, BasicProperties properties,
                    byte[] body) throws IOException {
                // body 即消息体
                String msg = new String(body);
                System.out.println(" [消费者1] received : " + msg + "!");
            }
        };
        // 监听队列，自动返回完成
        channel.basicConsume(QUEUE_NAME, true, consumer);
    }
}

```

要注意代码中：**队列需要和交换机绑定**

#### 消费者2

```java
public class Recv2 {
    private final static String QUEUE_NAME = "fanout_exchange_queue_2";

    private final static String EXCHANGE_NAME = "fanout_exchange_test";

    public static void main(String[] argv) throws Exception {
        // 获取到连接
        Connection connection = ConnectionUtil.getConnection();
        // 获取通道
        Channel channel = connection.createChannel();
        // 声明队列
        channel.queueDeclare(QUEUE_NAME, false, false, false, null);

        // 绑定队列到交换机
        channel.queueBind(QUEUE_NAME, EXCHANGE_NAME, "");
        
        // 定义队列的消费者
        DefaultConsumer consumer = new DefaultConsumer(channel) {
            // 获取消息，并且处理，这个方法类似事件监听，如果有消息的时候，会被自动调用
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, BasicProperties properties,
                    byte[] body) throws IOException {
                // body 即消息体
                String msg = new String(body);
                System.out.println(" [消费者2] received : " + msg + "!");
            }
        };
        // 监听队列，手动返回完成
        channel.basicConsume(QUEUE_NAME, true, consumer);
    }
}

```



#### 测试

我们运行两个消费者，然后发送1条消息：

 ![1527087071693](图片二/1527087071693.png)

### 8）订阅模型-Direct

#### 说明

在Fanout模式中，一条消息，会被所有订阅的队列都消费。但是，在某些场景下，我们希望不同的消息被不同的队列消费。这时就要用到Direct类型的Exchange。

 在Direct模型下：

- 队列与交换机的绑定，不能是任意绑定了，而是要指定一个`RoutingKey`（路由key）
- 消息的发送方在 向 Exchange发送消息时，也必须指定消息的 `RoutingKey`。
- Exchange不再把消息交给每一个绑定的队列，而是根据消息的`Routing Key`进行判断，只有队列的`Routingkey`与消息的 `Routing key`完全一致，才会接收到消息



流程图：

 ![1527087677192](图片二/1527087677192.png)

图解：

- P：生产者，向Exchange发送消息，发送消息时，会指定一个routing key。
- X：Exchange（交换机），接收生产者的消息，然后把消息递交给 与routing key完全匹配的队列
- C1：消费者，其所在队列指定了需要routing key 为 error 的消息
- C2：消费者，其所在队列指定了需要routing key 为 info、error、warning 的消息

#### 生产者

此处我们模拟商品的增删改，发送消息的RoutingKey分别是：insert、update、delete

```java
public class Send {
    private final static String EXCHANGE_NAME = "direct_exchange_test";

    public static void main(String[] argv) throws Exception {
        // 获取到连接
        Connection connection = ConnectionUtil.getConnection();
        // 获取通道
        Channel channel = connection.createChannel();
        // 声明exchange，指定类型为direct
        channel.exchangeDeclare(EXCHANGE_NAME, "direct");
        // 消息内容
        String message = "商品新增了， id = 1001";
        // 发送消息，并且指定routing key 为：insert ,代表新增商品
        channel.basicPublish(EXCHANGE_NAME, "insert", null, message.getBytes());
        System.out.println(" [商品服务：] Sent '" + message + "'");

        channel.close();
        connection.close();
    }
}

```

#### 消费者1

我们此处假设消费者1只接收两种类型的消息：更新商品和删除商品。

```java
public class Recv {
    private final static String QUEUE_NAME = "direct_exchange_queue_1";
    private final static String EXCHANGE_NAME = "direct_exchange_test";

    public static void main(String[] argv) throws Exception {
        // 获取到连接
        Connection connection = ConnectionUtil.getConnection();
        // 获取通道
        Channel channel = connection.createChannel();
        // 声明队列
        channel.queueDeclare(QUEUE_NAME, false, false, false, null);
        
        // 绑定队列到交换机，同时指定需要订阅的routing key。假设此处需要update和delete消息
        channel.queueBind(QUEUE_NAME, EXCHANGE_NAME, "update");
        channel.queueBind(QUEUE_NAME, EXCHANGE_NAME, "delete");

        // 定义队列的消费者
        DefaultConsumer consumer = new DefaultConsumer(channel) {
            // 获取消息，并且处理，这个方法类似事件监听，如果有消息的时候，会被自动调用
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, BasicProperties properties,
                    byte[] body) throws IOException {
                // body 即消息体
                String msg = new String(body);
                System.out.println(" [消费者1] received : " + msg + "!");
            }
        };
        // 监听队列，自动ACK
        channel.basicConsume(QUEUE_NAME, true, consumer);
    }
}

```



#### 消费者2

我们此处假设消费者2接收所有类型的消息：新增商品，更新商品和删除商品。

```java
public class Recv2 {
    private final static String QUEUE_NAME = "direct_exchange_queue_2";
    private final static String EXCHANGE_NAME = "direct_exchange_test";

    public static void main(String[] argv) throws Exception {
        // 获取到连接
        Connection connection = ConnectionUtil.getConnection();
        // 获取通道
        Channel channel = connection.createChannel();
        // 声明队列
        channel.queueDeclare(QUEUE_NAME, false, false, false, null);
        
        // 绑定队列到交换机，同时指定需要订阅的routing key。订阅 insert、update、delete
        channel.queueBind(QUEUE_NAME, EXCHANGE_NAME, "insert");
        channel.queueBind(QUEUE_NAME, EXCHANGE_NAME, "update");
        channel.queueBind(QUEUE_NAME, EXCHANGE_NAME, "delete");

        // 定义队列的消费者
        DefaultConsumer consumer = new DefaultConsumer(channel) {
            // 获取消息，并且处理，这个方法类似事件监听，如果有消息的时候，会被自动调用
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, BasicProperties properties,
                    byte[] body) throws IOException {
                // body 即消息体
                String msg = new String(body);
                System.out.println(" [消费者2] received : " + msg + "!");
            }
        };
        // 监听队列，自动ACK
        channel.basicConsume(QUEUE_NAME, true, consumer);
    }
}

```



#### 测试

我们分别发送增、删、改的RoutingKey，发现结果：

 ![1527088296131](图片二/1527088296131.png)



### 9）订阅模型-Topic

#### 说明

`Topic`类型的`Exchange`与`Direct`相比，都是可以根据`RoutingKey`把消息路由到不同的队列。只不过`Topic`类型`Exchange`可以让队列在绑定`Routing key` 的时候使用通配符！



`Routingkey` 一般都是有一个或多个单词组成，多个单词之间以”.”分割，例如： `item.insert`

 通配符规则：

`#`：匹配一个或多个词

`*`：匹配不多不少恰好1个词



举例：

`item.#`：能够匹配`item.spu.insert` 或者 `item.spu`

`item.*`：只能匹配`item.spu`

​     

图示：

 ![1527088518574](图片二/1527088518574.png)

解释：

- 红色Queue：绑定的是`usa.#` ，因此凡是以 `usa.`开头的`routing key` 都会被匹配到
- 黄色Queue：绑定的是`#.news` ，因此凡是以 `.news`结尾的 `routing key` 都会被匹配

#### 生产者

使用topic类型的Exchange，发送消息的routing key有3种： `item.isnert`、`item.update`、`item.delete`：

```java
public class Send {
    private final static String EXCHANGE_NAME = "topic_exchange_test";

    public static void main(String[] argv) throws Exception {
        // 获取到连接
        Connection connection = ConnectionUtil.getConnection();
        // 获取通道
        Channel channel = connection.createChannel();
        // 声明exchange，指定类型为topic
        channel.exchangeDeclare(EXCHANGE_NAME, "topic");
        // 消息内容
        String message = "新增商品 : id = 1001";
        // 发送消息，并且指定routing key 为：insert ,代表新增商品
        channel.basicPublish(EXCHANGE_NAME, "item.insert", null, message.getBytes());
        System.out.println(" [商品服务：] Sent '" + message + "'");

        channel.close();
        connection.close();
    }
}

```

#### 消费者1

我们此处假设消费者1只接收两种类型的消息：更新商品和删除商品

```java
public class Recv {
    private final static String QUEUE_NAME = "topic_exchange_queue_1";
    private final static String EXCHANGE_NAME = "topic_exchange_test";

    public static void main(String[] argv) throws Exception {
        // 获取到连接
        Connection connection = ConnectionUtil.getConnection();
        // 获取通道
        Channel channel = connection.createChannel();
        // 声明队列
        channel.queueDeclare(QUEUE_NAME, false, false, false, null);
        
        // 绑定队列到交换机，同时指定需要订阅的routing key。需要 update、delete
        channel.queueBind(QUEUE_NAME, EXCHANGE_NAME, "item.update");
        channel.queueBind(QUEUE_NAME, EXCHANGE_NAME, "item.delete");

        // 定义队列的消费者
        DefaultConsumer consumer = new DefaultConsumer(channel) {
            // 获取消息，并且处理，这个方法类似事件监听，如果有消息的时候，会被自动调用
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, BasicProperties properties,
                    byte[] body) throws IOException {
                // body 即消息体
                String msg = new String(body);
                System.out.println(" [消费者1] received : " + msg + "!");
            }
        };
        // 监听队列，自动ACK
        channel.basicConsume(QUEUE_NAME, true, consumer);
    }
}

```



#### 消费者2

我们此处假设消费者2接收所有类型的消息：新增商品，更新商品和删除商品。

```java
/**
 * 消费者2
 */
public class Recv2 {
    private final static String QUEUE_NAME = "topic_exchange_queue_2";
    private final static String EXCHANGE_NAME = "topic_exchange_test";

    public static void main(String[] argv) throws Exception {
        // 获取到连接
        Connection connection = ConnectionUtil.getConnection();
        // 获取通道
        Channel channel = connection.createChannel();
        // 声明队列
        channel.queueDeclare(QUEUE_NAME, false, false, false, null);
        
        // 绑定队列到交换机，同时指定需要订阅的routing key。订阅 insert、update、delete
        channel.queueBind(QUEUE_NAME, EXCHANGE_NAME, "item.*");

        // 定义队列的消费者
        DefaultConsumer consumer = new DefaultConsumer(channel) {
            // 获取消息，并且处理，这个方法类似事件监听，如果有消息的时候，会被自动调用
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, BasicProperties properties,
                    byte[] body) throws IOException {
                // body 即消息体
                String msg = new String(body);
                System.out.println(" [消费者2] received : " + msg + "!");
            }
        };
        // 监听队列，自动ACK
        channel.basicConsume(QUEUE_NAME, true, consumer);
    }
}

```

### 10）持久化

如何避免消息丢失？

1）  消费者的ACK机制。可以防止消费者丢失消息。

2）  但是，如果在消费者消费之前，MQ就宕机了，消息就没了。



所以我们需要将消息持久化到硬盘，以防服务宕机。

 要将消息持久化，前提是：队列、Exchange都持久化

交换机持久化

 ![1527088933255](图片二/1527088933255.png)

队列持久化

 ![1527088960059](图片二/1527088960059.png)

消息持久化

![1527088984029](图片二/1527088984029.png)









## 07、数据同步：RabbitMQ面试题



### 面试题1：如何解决消息丢失？

- ack（消费者确认）
- 持久化
- 生产者确认（publisher confirm）：生产者发送消息后，等待mq的ACK，如果没有收到或者收到失败信息，则重试。如果收到成功消息则业务结束。
- 可靠消息服务（可选）：对于部分不支持生产者确认的消息队列，可以发送消息前，将消息持久化到数据库，并记录消息状态，后续消息发送、消费等过程都依赖于数据库中消息状态的判断和修改。





### 面试题2：如何避免消息堆积？

- 通过同一个队列多消费者监听，实现消息的争抢，加快消息消费速度。





### 面试题3：如何保证消息的有序性？

答：大部分业务对消息的有序性要求不高，如果遇到对时序要求较高的业务，分两种情况来处理：

- 业务同时对并发要求不高：
  - 保证消息发送时有序同步发送
  - 保证消息发送被同一个队列接收
  - 保证一个队列只有一个消费者，可以有从机（待机状态），实现高可用。
  - 实现主从（Zookeeper集群选主）
- 业务同时对并发要求较高：
  - 满足上述第一个场景的条件
  - 可以有多个队列
  - 有时序要求的一组消息，通过hash方式分派到一个固定队列





### 面试题4：如何避免消息重复消费？

- 保证接口幂等即可，那么如何保证接口幂等呢？
  - 某些接口天生幂等，例如查询请求
  - 某些接口天生不幂等，比如新增，还有某些接口的修改功能
    - 能根据具体的业务或状态来确定的，在消费端通过业务判断是否执行过













## 08、数据同步：SpringAMQP的使用

### 1）简介

Sprin有很多不同的项目，其中就有对AMQP的支持：

![1576917538170](图片二/1576917538170.png) 

Spring AMQP的页面：<http://projects.spring.io/spring-amqp/> 

![1576917684996](图片二/1576917684996.png) 

注意这里一段描述：

```
     Spring-amqp是对AMQP协议的抽象实现，而spring-rabbit 是对协议的具体实现，也是目前的唯一实现。底层使用的就是RabbitMQ。

```



### 2）依赖和配置

添加AMQP的启动器：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>

```

在`application.yml`中添加RabbitMQ地址：

```yaml
spring:
  rabbitmq:
    host: 192.168.13.111
    username: leyou
    password: leyou
    virtual-host: /leyou

```



### 3）监听者

在SpringAmqp中，对消息的消费者进行了封装和抽象，一个普通的JavaBean中的普通方法，只要通过简单的注解，就可以成为一个消费者。

```java
@Component
public class Listener {

    @RabbitListener(bindings = @QueueBinding(
            value = @Queue(value = "spring.test.queue", durable = "true"),
            exchange = @Exchange(
                    value = "spring.test.exchange",
                    ignoreDeclarationExceptions = "true",
                    type = ExchangeTypes.TOPIC
            ),
            key = {"#.#"}))
    public void listen(String msg){
        System.out.println("接收到消息：" + msg);
    }
}

```

- `@Componet`：类上的注解，注册到Spring容器
- `@RabbitListener`：方法上的注解，声明这个方法是一个消费者方法，需要指定下面的属性：
  - `bindings`：指定绑定关系，可以有多个。值是`@QueueBinding`的数组。`@QueueBinding`包含下面属性：
    - `value`：这个消费者关联的队列。值是`@Queue`，代表一个队列
    - `exchange`：队列所绑定的交换机，值是`@Exchange`类型
    - `key`：队列和交换机绑定的`RoutingKey`

类似listen这样的方法在一个类中可以写多个，就代表多个消费者。



### 4）消息发送：AmqpTemplate

Spring最擅长的事情就是封装，把他人的框架进行封装和整合。

Spring为AMQP提供了统一的消息处理模板：AmqpTemplate，非常方便的发送消息，其发送方法：

![1577072783807](图片二/1577072783807.png) 

红框圈起来的是比较常用的3个方法，分别是：

- 指定交换机、RoutingKey和消息体
- 指定消息
- 指定RoutingKey和消息，会向默认的交换机发送消息



### 5）测试代码

```java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = Application.class)
public class MqDemo {

    @Autowired
    private AmqpTemplate amqpTemplate;

    @Test
    public void testSend() throws InterruptedException {
        String msg = "hello, Spring boot amqp";
        this.amqpTemplate.convertAndSend("spring.test.exchange","a.b", msg);
        // 等待10秒后再结束
        Thread.sleep(10000);
    }
}

```

运行后查看日志：

![1527090414110](图片二/1527090414110.png)













## 09、数据同步：环境准备及思路分析

我们已经完成了对MQ的基本学习和认识。接下来，我们就改造项目，实现搜索服务、商品静态页的数据同步。

### 1）思路分析

> 发送方：商品微服务

- 什么时候发？

  当商品服务对商品进行新增和上下架的时候，需要发送一条消息，通知其它服务。

- 发送什么内容？

  对商品的增删改时其它服务可能需要新的商品数据，但是如果消息内容中包含全部商品信息，数据量太大，而且并不是每个服务都需要全部的信息。因此我们**只发送商品id**，其它服务可以根据id查询自己需要的信息。

> 接收方：搜索微服务、静态页微服务

- 接收消息后如何处理？
  - 搜索微服务：
    - 上架：添加新的数据到索引库
    - 下架：删除索引库数据
  - 静态页微服务：
    - 上架：创建新的静态页
    - 下架：删除原来的静态页



### 2）常量准备

在`ly-common`中编写一个常量类，记录将来会用到的Exchange名称、Queue名称、routing_key名称

```java
package com.leyou.common.constants;

/**
 * @author 黑马程序员
 */
public abstract class MQConstants {

    public static final class Exchange {
        /**
         * 商品服务交换机名称
         */
        public static final String ITEM_EXCHANGE_NAME = "ly.item.exchange";
    }

    public static final class RoutingKey {
        /**
         * 商品上架的routing-key
         */
        public static final String ITEM_UP_KEY = "item.up";
        /**
         * 商品下架的routing-key
         */
        public static final String ITEM_DOWN_KEY = "item.down";
    }

    public static final class Queue{
        /**
         * 搜索服务，商品上架的队列
         */
        public static final String SEARCH_ITEM_UP = "search.item.up.queue";
        /**
         * 搜索服务，商品下架的队列
         */
        public static final String SEARCH_ITEM_DOWN = "search.item.down.queue";

        /**
         * 搜索服务，商品上架的队列
         */
        public static final String PAGE_ITEM_UP = "page.item.up.queue";
        /**
         * 搜索服务，商品下架的队列
         */
        public static final String PAGE_ITEM_DOWN = "page.item.down.queue";
    }
}


```

这些常量我们用一个图来展示用在什么地方：

![1577084809261](图片二/1577084809261.png)













## 10、数据同步：商品微服务发送消息

我们先在商品微服务`ly-item-service`中实现发送消息。

### 1）引入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>

```

### 2）配置文件

我们在application.yml中添加一些有关RabbitMQ的配置：

```yaml
spring:
  rabbitmq:
    host: 127.0.0.1
    username: leyou
    password: leyou
    virtual-host: /leyou
    template:
      retry:
        enabled: true
        initial-interval: 10000ms
        max-interval: 80000ms
        multiplier: 2
    publisher-confirms: true

```

- template：有关`AmqpTemplate`的配置
  - retry：失败重试
    - enabled：开启失败重试
    - initial-interval：第一次重试的间隔时长
    - max-interval：最长重试间隔，超过这个间隔将不再重试
    - multiplier：下次重试间隔的倍数，此处是2即下次重试间隔是上次的2倍
  - exchange：缺省的交换机名称，此处配置后，发送消息如果不指定交换机就会使用这个
- publisher-confirms：生产者确认机制，确保消息会正确发送，如果发送失败会有错误回执，从而触发重试

### 3）Json消息转换器

需要注意的是，默认情况下，AMQP会使用JDK的序列化方式进行处理，传输数据比较大，效率太低。我们可以自定义消息转换器，使用JSON来处理：

```java
/**
 * @author 黑马程序员
 */
@Configuration
public class RabbitConfig {

    @Bean
    public Jackson2JsonMessageConverter messageConverter(){
        return new Jackson2JsonMessageConverter();
    }
}

```

位置：

![1553261106957](图片二/1553261106957.png) 



### 4）改造GoodsService

改造GoodsService中的商品上下架功能，发送消息，注意用静态导入方式，导入在ly-common中定义的常量：

```java
import static com.leyou.common.constants.MQConstants.RoutingKey.*;
import static com.leyou.common.constants.MQConstants.Exchange.*;

@Autowired
private AmqpTemplate amqpTemplate;


/**
 * 商品上下架
 * @param id
 * @param saleable
 */
@Transactional
public void updateSaleable(Long id, Boolean saleable) {
    Spu record = new Spu();
    record.setId(id);
    record.setSaleable(saleable);
    int count = spuMapper.updateByPrimaryKeySelective(record);
    if (count != 1) {
        throw new LyException(ExceptionEnum.UPDATE_OPERATION_FAIL);
    }
    //发消息
    String routingKey = saleable ? ITEM_UP_KEY : ITEM_DOWN_KEY;
    amqpTemplate.convertAndSend(ITEM_EXCHANGE_NAME, routingKey, id);
}

```















## 11、数据同步：搜索微服务接收消息

搜索服务接收到消息后要做的事情：

- 上架：添加新的数据到索引库
- 下架：删除索引库数据

我们需要两个不同队列，监听不同类型消息。

### 1）引入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```



### 2）添加配置

```yaml
spring:
  rabbitmq:
    host: 192.168.13.111
    username: leyou
    password: leyou
    virtual-host: /leyou

```

这里只是接收消息而不发送，所以不用配置template相关内容。

不过，不要忘了消息转换器：

![1553262961034](图片二/1553262961034.png) 

```java
/**
 * @author 黑马程序员
 */
@Configuration
public class RabbitConfig {

    @Bean
    public Jackson2JsonMessageConverter messageConverter(){
        return new Jackson2JsonMessageConverter();
    }
}

```



### 3）编写监听器

 ![1553263050354](图片二/1553263050354.png) 

代码：

```java
package com.leyou.search.mq;

import com.leyou.search.service.SearchService;
import org.springframework.amqp.core.ExchangeTypes;
import org.springframework.amqp.rabbit.annotation.Exchange;
import org.springframework.amqp.rabbit.annotation.Queue;
import org.springframework.amqp.rabbit.annotation.QueueBinding;
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import static com.leyou.common.constants.MQConstants.Exchange.*;
import static com.leyou.common.constants.MQConstants.Queue.*;
import static com.leyou.common.constants.MQConstants.RoutingKey.*;

@Component
public class ItemListener {

    @Autowired
    private SearchService searchService;
    

    @RabbitListener(bindings = @QueueBinding(
            value=@Queue(value= SEARCH_ITEM_UP, durable = "true"),
            exchange = @Exchange(value= ITEM_EXCHANGE_NAME, type = ExchangeTypes.TOPIC),
            key = ITEM_UP_KEY
    ))
    public void listenItemUp(Long spuId){
        if (spuId != null){
            //创建索引
            searchService.saveGoods(spuId);
        }
    }


    @RabbitListener(bindings = @QueueBinding(
            value=@Queue(value= SEARCH_ITEM_DOWN, durable = "true"),
            exchange = @Exchange(value= ITEM_EXCHANGE_NAME, type = ExchangeTypes.TOPIC),
            key = ITEM_DOWN_KEY
    ))
    public void listenItemDown(Long spuId){
        if (spuId != null){
            //删除索引
            searchService.deleteGoods(spuId);
        }
    }
}

```



### 4）编写创建和删除索引方法

这里因为要创建和删除索引，我们需要在SearchService中拓展两个方法，创建和删除索引：

```java
//创建索引
public void saveGoods(Long spuId) {
    //查询spu
    SpuDTO spu = itemClient.querySpuById(spuId);
    //创建Goods
    Goods goods = buildGoods(spu);
    //创建索引
    repository.save(goods);
}

//删除索引
public void deleteGoods(Long spuId) {
    repository.deleteById(spuId);
}

```

创建索引的方法可以从之前导入数据的测试类中拷贝和改造。



















## 12、数据同步：静态页服务接收消息

商品静态页服务接收到消息后的处理：

- 上架：创建新的静态页
- 下架：删除原来的静态页

与前面搜索服务类似，也需要两个队列来处理。

### 1）引入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```

### 2）添加配置

```yaml
spring:
  rabbitmq:
    host: 192.168.13.111
    username: leyou
    password: leyou
    virtual-host: /leyou
```

这里只是接收消息而不发送，所以不用配置template相关内容。

不过，不要忘了消息转换器：

![1577093458724](图片二/1577093458724.png)  



```java
/**
 * @author 黑马程序员
 */
@Configuration
public class RabbitConfig {

    @Bean
    public Jackson2JsonMessageConverter messageConverter(){
        return new Jackson2JsonMessageConverter();
    }
}

```



### 3）编写监听器

 ![1577093412587](图片二/1577093412587.png) 

代码：

```java
package com.leyou.page.mq;

import com.leyou.page.service.PageService;
import org.springframework.amqp.core.ExchangeTypes;
import org.springframework.amqp.rabbit.annotation.Exchange;
import org.springframework.amqp.rabbit.annotation.Queue;
import org.springframework.amqp.rabbit.annotation.QueueBinding;
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import static com.leyou.common.constants.MQConstants.Exchange.ITEM_EXCHANGE_NAME;
import static com.leyou.common.constants.MQConstants.Queue.PAGE_ITEM_DOWN;
import static com.leyou.common.constants.MQConstants.Queue.PAGE_ITEM_UP;
import static com.leyou.common.constants.MQConstants.RoutingKey.ITEM_DOWN_KEY;
import static com.leyou.common.constants.MQConstants.RoutingKey.ITEM_UP_KEY;

@Component
public class ItemListener {

    @Autowired
    private PageService pageService;

    @RabbitListener(bindings = @QueueBinding(
            value=@Queue(value= PAGE_ITEM_UP, durable = "true"),
            exchange = @Exchange(value= ITEM_EXCHANGE_NAME, type = ExchangeTypes.TOPIC),
            key = ITEM_UP_KEY
    ))
    public void listenItemUp(Long spuId){
        if (spuId != null){
            //创建静态页面
            pageService.createItemHtml(spuId);
        }
    }


    @RabbitListener(bindings = @QueueBinding(
            value=@Queue(value= PAGE_ITEM_DOWN, durable = "true"),
            exchange = @Exchange(value= ITEM_EXCHANGE_NAME, type = ExchangeTypes.TOPIC),
            key = ITEM_DOWN_KEY
    ))
    public void listenItemDown(Long spuId){
        if (spuId != null){
            //删除静态页面
            pageService.deleteItemHtml(spuId);
        }
    }
}


```

### 4）添加删除页面方法

创建商品详情静态页面的方法我们之前已经添加过了，只需要添加删除静态页的方法即可：

```java
/**
 * 删除静态页
 * @param spuId
 */
public void deleteItemHtml(Long spuId) {
    //准备目标文件
    File file = new File(itemDir, spuId + ".html");
    //删除文件
    if(file.exists()){
        boolean delete = file.delete();
        if(!delete){
            log.error("【静态页服务】静态页删除失败，商品id：{}", spuId);
            throw new LyException(ExceptionEnum.DELETE_OPERATION_FAIL);
        }
    }
}

```



















## 13、测试数据同步

### 查看RabbitMQ控制台

重新启动项目，并且登录RabbitMQ管理界面：http://192.168.13.111:15672

可以看到，交换机已经创建出来了：

![1577093964631](图片二/1577093964631.png) 

队列也已经创建完毕：

![1577094058227](图片二/1577094058227.png) 

并且队列都已经绑定到交换机：

 ![1577094169361](图片二/1577094169361.png) 

### 查看数据

我们搜索下手机：

![1527165091338](图片二/1527165091338.png) 

商品详情页：

![1527165112725](图片二/1527165112725.png)



### 修改商品

然后在管理后台修改商品：

我们修改以下内容：

标题改成6.1

 ![1527165345197](图片二/1527165345197.png)



商品详情加点文字：

 ![1527165323776](图片二/1527165323776.png)



价格改为3999

![1527165286619](图片二/1527165286619.png)b

### 再次查看数据

搜索页：

![1527166160735](图片二/1527166160735.png) 



详情页：

![1527166183985](图片二/1527166183985.png)



详情内容：

![1527166202112](图片二/1527166202112.png) 

完美！



























## 14、用户注册：注册功能分析

完成了商品的详情展示，下一步自然是购物了。不过购物之前要完成用户的注册和登录等业务，我们打开注册页面分析下需求：

![1577098522193](图片二/1577098522193.png) 



> 1）用户名和手机号需要验证唯一性
>
> 2）密码我们需要加密存储
>
> 3）获取短信验证码：需要用到Redis和MQ























## 15、Redis：Redis的安装和介绍



### 1）NoSQL

Redis是目前非常流行的一款NoSql数据库。

> 什么是NoSql？

![1535363817109](图片二/1535363817109.png)



常见的NoSql产品：

![1535363864689](图片二/1535363864689.png)





### 2）Redis简介

> Redis的网址：

[官网](http://redis.io/)：速度很慢，几乎进去不去啊。

[中文网站](http://www.redis.cn/)：有部分翻译的官方文档，英文差的同学的福音



> 历史：

![1535364502694](图片二/1535364502694.png)

> 特性：

 ![1535364521915](图片二/1535364521915.png)





### 3）Redis安装

课前资料中已经准备了安装软件：

![1577100527718](图片二/1577100527718.png) 

解压完成后，双击`redis-server.exe`即可，然后使用`redis-destop-manager`链接即可。





### 4）Redis与Memcache

Redis和Memcache是目前非常流行的两种NoSql数据库，读可以用于服务端缓存。两者有怎样的差异呢？

- 从实现来看：
  - redis：单线程
  - Memcache：多线程
- 从存储方式来看：
  - redis：支持数据持久化和主从备份，数据更安全
  - Memcache：数据存于内存，没有持久化功能 
- 从功能来看：
  - redis：除了基本的k-v 结构外，支持多种其它复杂结构、事务等高级功能
  - Memcache：只支持基本k-v 结构
- 从可用性看：
  - redis：支持主从备份、数据分片、哨兵监控
  - memcache：没有分片功能，需要从客户端支持

可以看出，Redis相比Memcache功能更加强大，支持的数据结构也比较丰富，已经不仅仅是一个缓存服务。而Memcache的功能相对单一。

一些面试问题：Redis缓存击穿问题、缓存雪崩问题。









## 16、Redis：Redis的命令回顾

通过`help`命令可以让我们查看到Redis的指令帮助信息：

在`help`后面跟上`空格`，然后按`tab`键，会看到Redis对命令分组的组名：

 ![](图片二/test.gif)

主要包含：

- @generic：通用指令
- @string：字符串类型指令
- @list：队列结构指令
- @set：set结构指令
- @sorted_set：可排序的set结构指令
- @hash：hash结构指令

其中除了@generic以为的，对应了Redis中常用的5种数据类型：

- String：等同于java中的，`Map<String,String>`
- list：等同于java中的`Map<String,List<String>>`
- set：等同于java中的`Map<String,Set<String>>`
- sort_set：可排序的set
- hash：等同于java中的：`Map<String,Map<String,String>>`

可见，Redis中存储数据结构都是类似java的map类型。Redis不同数据类型，只是`'map'`的值的类型不同。



### 1）通用指令

> keys

获取符合规则的键名列表。

- 语法：keys pattern

  示例：keys *	(查询所有的键)

  ![img](图片二/wpsD8A4.tmp.jpg)

这里的pattern其实是正则表达式，所以语法基本是类似的

> exists

判断一个键是否存在，如果存在返回整数1，否则返回0

- 语法：EXISTS key

- 示例：

  ![img](图片二/wps979F.tmp.jpg)

> del

DEL：删除key，可以删除一个或多个key，返回值是删除的key的个数。

- 语法：DEL key [key … ]

- 示例：

  ![img](图片二/wpsF22.tmp.jpg)

> expire

- 语法：

  ```
  EXPIRE key seconds
  ```

- 作用：设置key的过期时间，超过时间后，将会自动删除该key。

- 返回值：

  - 如果成功设置过期时间，返回1。
  - 如果key不存在或者不能设置过期时间，返回0

> TTL

TTL：查看一个key的过期时间

- 语法：`TTL key`
- 返回值：
  - 返回剩余的过期时间
  - -1：永不过期
  - -2：已过期或不存在
- 示例：

![img](图片二/wpsFAFA.tmp.jpg) 

> persist

- 语法：

  ```
  persist key
  ```

- 作用：

  移除给定key的生存时间，将这个 key 从带生存时间 key 转换成一个不带生存时间、永不过期的 key 。

- 返回值：

  - 当生存时间移除成功时，返回 1 .
  - 如果 key 不存在或 key 没有设置生存时间，返回 0 .

- 示例：

![img](图片二/wpsCE15.tmp.jpg) 

### 2）字符串指令

字符串结构，其实是Redis中最基础的K-V结构。其键和值都是字符串。类似Java的Map<String,String>

![img](图片二/wps9794.tmp.jpg)

常用指令：

| 语法                                                         | 说明                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [SET key value](http://www.runoob.com/redis/strings-set.html) | 设置指定 key 的值                                            |
| [GET key](http://www.runoob.com/redis/strings-get.html)      | 获取指定 key 的值。                                          |
| [GETRANGE key start end](http://www.runoob.com/redis/strings-getrange.html) | 返回 key 中字符串值的子字符                                  |
| [INCR key](http://www.runoob.com/redis/strings-incr.html)    | 将 key 中储存的数字值增一。                                  |
| [INCRBY key increment](http://www.runoob.com/redis/strings-incrby.html) | 将 key 所储存的值加上给定的增量值（increment） 。            |
| [DECR key](http://www.runoob.com/redis/strings-decr.html)    | 将 key 中储存的数字值减一。                                  |
| [DECRBY key decrement](http://www.runoob.com/redis/strings-decrby.html) | key 所储存的值减去给定的减量值（decrement） 。               |
| [APPEND key value](http://www.runoob.com/redis/strings-append.html) | 如果 key 已经存在并且是一个字符串， APPEND 命令将 value 追加到 key 原来的值的末尾。 |
| [STRLEN key](http://www.runoob.com/redis/strings-strlen.html) | 返回 key 所储存的字符串值的长度。                            |
| [MGET key1  key2 ...](http://www.runoob.com/redis/strings-mget.html) | 获取所有(一个或多个)给定 key 的值。                          |
| [MSET key value key value ...](http://www.runoob.com/redis/strings-mset.html) | 同时设置一个或多个 key-value 对。                            |

### 3）hash结构命令

Redis的Hash结构类似于Java中的Map<String,Map<String,Stgring>>，键是字符串，值是另一个映射。结构如图：

![11](图片二/wps47DA.tmp.jpg)

这里我们称键为key，字段名为  hKey， 字段值为 hValue



 常用指令：

> **HSET、HSETNX和HGET（添加、获取）**

HSET

- 介绍：
  - ![img](图片二/wps55A2.tmp.jpg) 
  - Redis Hset 命令用于为哈希表中的字段赋值 。
  - 如果哈希表不存在，一个新的哈希表被创建并进行 HSET 操作。
  - 如果字段已经存在于哈希表中，旧值将被覆盖。
- 返回值：
  - 如果字段是哈希表中的一个新建字段，并且值设置成功，返回 1 。
  - 如果哈希表中域字段已经存在且旧值已被新值覆盖，返回 0
- 示例：

![img](图片二/wps55B3.tmp.jpg) 

 

> HGET

- 介绍：

![img](图片二/wps55D5.tmp.jpg) 

```
Hget 命令用于返回哈希表中指定字段的值。 
```

- 返回值：返回给定字段的值。如果给定的字段或 key 不存在时，返回 nil
- 示例：

![img](图片二/wps55E5.tmp.jpg) 

> HGETALL

- 介绍：

![img](图片二/wps562A.tmp.jpg) 

- 返回值：

指定key 的所有字段的名及值。返回值里，紧跟每个字段名(field name)之后是字段的值(value)，所以返回值的长度是哈希表大小的两倍

- 示例：

  ![img](图片二/wps562B.tmp.jpg)

> HKEYS

- 介绍

![img](图片二/wps563C.tmp.jpg) 

- 示例：

  ![img](图片二/wps563D.tmp.jpg) 



> HVALS

![img](图片二/wps564D.tmp.jpg) 

- 注意：这个命令不是HVALUES，而是HVALS，是value 的缩写：val

- 示例：

  ![img](图片二/wps565E.tmp.jpg) 

> **HDEL（删除）**

Hdel 命令用于删除哈希表 key 中的一个或多个指定字段，不存在的字段将被忽略。

![img](图片二/wps565F.tmp.jpg) 

- 语法：

  HDEL key field1 [field2 ... ]

- 返回值：

  被成功删除字段的数量，不包括被忽略的字段

- 示例：

  ![img](图片二/wps566F.tmp.jpg) 





### 4）Redis的持久化：RDB

Redis有两种持久化方案：RDB和AOF



> 触发条件

RDB是Redis的默认持久化方案，当满足一定的条件时，Redis会自动将内存中的数据全部持久化到硬盘。

条件在redis.conf文件中配置，格式如下：

```
save (time) (count)
```

当满足在time（单位是秒）时间内，至少进行了count次修改后，触发条件，进行RDB快照。

例如，默认的配置如下：

![1535375586633](图片二/1535375586633.png)

> 基本原理

RDB的流程是这样的：

- Redis使用fork函数来复制一份当前进程（父进程）的副本（子进程）
- 父进程继续接收并处理请求，子进程开始把内存中的数据写入硬盘中的临时文件
- 子进程写完后，会使用临时文件代替旧的RDB文件



### 5）Redis的持久化：AOF

> 基本原理

AOF方式默认是关闭的，需要修改配置来开启：

```
appendonly yes # 把默认的no改成yes
```

AOF持久化的策略是，把每一条服务端接收到的写命令都记录下来，每隔一定时间后，写入硬盘的AOF文件中，当服务器重启后，重新执行这些命令，即可恢复数据。



AOF文件写入的频率是可以配置的：

![1535376414724](图片二/1535376414724.png)



> AOF文件重写

当记录命令过多，必然会出现对同一个key的多次写操作，此时只需要记录最后一条即可，前面的记录都毫无意义了。因此，当满足一定条件时，Redis会对AOF文件进行重写，移除对同一个key的多次操作命令，保留最后一条。默认的触发条件：

![1535376351400](图片二/1535376351400.png)

主从





















## 17、Redis：SpringDataRedis的使用

之前，我们使用Redis都是采用的Jedis客户端，不过既然我们使用了SpringBoot，为什么不使用Spring对Redis封装的套件呢？

### 1）Spring Data Redis

官网：<http://projects.spring.io/spring-data-redis/>

![1527250056698](图片二/1527250056698.png)                                    

Spring Data Redis，是Spring Data 家族的一部分。 对Jedis客户端进行了封装，与spring进行了整合。可以非常方便的来实现redis的配置和操作。 

### 2）RedisTemplate基本操作

与以往学习的套件类似，Spring Data 为 Redis 提供了一个工具类：RedisTemplate。里面封装了对于Redis的五种数据结构的各种操作，包括：

- redisTemplate.opsForValue() ：操作字符串
- redisTemplate.opsForHash() ：操作hash
- redisTemplate.opsForList()：操作list
- redisTemplate.opsForSet()：操作set
- redisTemplate.opsForZSet()：操作zset

例如我们对字符串操作比较熟悉的有：get、set等命令，这些方法都在 opsForValue()返回的对象中有：

![1535376791994](图片二/1535376791994.png)



其它一些通用命令，如del，可以通过redisTemplate.xx()来直接调用。

 ![1535376896536](图片二/1535376896536.png)



### 3）StringRedisTemplate

RedisTemplate在创建时，可以指定其泛型类型：

- K：代表key 的数据类型
- V: 代表value的数据类型

注意：这里的类型不是Redis中存储的数据类型，而是Java中的数据类型，RedisTemplate会自动将Java类型转为Redis支持的数据类型：字符串、字节、二二进制等等。

![1527250218215](图片二/1527250218215.png)

不过RedisTemplate默认会采用JDK自带的序列化（Serialize）来对对象进行转换。生成的数据十分庞大，因此一般我们都会指定key和value为String类型，这样就由我们自己把对象序列化为json字符串来存储即可。



因为大部分情况下，我们都会使用key和value都为String的RedisTemplate，因此Spring就默认提供了这样一个实现：

 ![1527256139407](图片二/1527256139407.png)

### 4）测试

我们新建一个测试项目，然后在项目中引入Redis启动器：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

然后在配置文件中指定Redis地址：

```yaml
spring:
  redis:
    host: 127.0.0.1
```

然后就可以直接注入`StringRedisTemplate`对象了：

```java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = LyUserService.class)
public class RedisTest {

    @Autowired
    private StringRedisTemplate redisTemplate;

    @Test
    public void testRedis() {
        // 存储数据
        this.redisTemplate.opsForValue().set("key1", "value1");
        // 获取数据
        String val = this.redisTemplate.opsForValue().get("key1");
        System.out.println("val = " + val);
    }

    @Test
    public void testRedis2() {
        // 存储数据，并指定剩余生命时间,5小时
        this.redisTemplate.opsForValue().set("key2", "value2",
                5, TimeUnit.HOURS);
    }

    @Test
    public void testHash(){
        BoundHashOperations<String, Object, Object> hashOps =
                this.redisTemplate.boundHashOps("user");
        // 操作hash数据
        hashOps.put("name", "jack");
        hashOps.put("age", "21");

        // 获取单个数据
        Object name = hashOps.get("name");
        System.out.println("name = " + name);

        // 获取所有数据
        Map<Object, Object> map = hashOps.entries();
        for (Map.Entry<Object, Object> me : map.entrySet()) {
            System.out.println(me.getKey() + " : " + me.getValue());
        }
    }
}
```

![1577159922463](图片二/1577159922463.png) 















































06、短信服务：开通阿里短信服务

注册页面上有短信发送的按钮，当用户点击发送短信，我们需要生成验证码，发送给用户。我们将使用阿里提供的阿里大于来实现短信发送。

参考课前资料的《阿里短信.md》学习demo入门

![1577257345500](图片二/1577257345500-1579162961200.png) 



大家照着文档开通需要的服务，以及发短信的签名和模板就可以了。











07、短信服务：阿里在线短信调试

申请完签名和模板之后，我们可以借助阿里的在线调试短信功能来测试我们的短信签名和模板。

https://help.aliyun.com/document_detail/101414.html

点击链接打开页面后，点击这个按钮调式：

![1577259287700](图片二/1577259287700-1579162961201.png) 

点击后，出现如下视图：

![1577259702775](图片二/1577259702775-1579162961201.png) 

填入各类参数后，点击 `发起调用`按钮即可，然后该手机会受到这样一条短信：

![1577259801071](图片二/1577259801071-1579162961201.png) 



