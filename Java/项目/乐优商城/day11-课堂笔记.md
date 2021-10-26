顺利开展实战，同学们已经分了组，现在我们课程这样安排：

09：00-10：00 上午第一节课

10：10-11：00 上午第二节课

11：10-12：00 上午第三节课

14：00-15：00 下午第一节课

15：10-16：00 下午第二节课

16：10-17：00 下午第三节课

18：00-20：00 晚上练习辅导



一天下来是8节课，5-6节课讲课，2节课是辅导。



同学们可以先把问题汇总到组长哪里，组长收集完再发给我，我统一处理，第二天整好发给大家













## 01、课程目标

- 会使用阿里短信SDK发送短信
- 了解面向接口开发方式
- 实现数据校验功能
- 实现短信发送功能
- 实现注册功能
- 实现根据用户名和密码查询用户功能

















## 02、短信服务：开通阿里短信服务

注册页面上有短信发送的按钮，当用户点击发送短信，我们需要生成验证码，发送给用户。我们将使用阿里提供的阿里大于来实现短信发送。

参考课前资料的《阿里短信.md》学习demo入门

![1577257345500](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1577257345500.png) 



大家照着文档开通需要的服务，以及发短信的签名和模板就可以了。











## 03、短信服务：阿里在线短信调试

申请完签名和模板之后，我们可以借助阿里的在线调试短信功能来测试我们的短信签名和模板。

https://help.aliyun.com/document_detail/101414.html

点击链接打开页面后，点击这个按钮调式：

![1577259287700](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1577259287700.png) 

点击后，出现如下视图：

![1577259702775](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1577259702775.png) 

填入各类参数后，点击 `发起调用`按钮即可，然后该手机会受到这样一条短信：

![1577259801071](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1577259801071.png) 







## 04、短信服务：搭建短信微服务

因为系统中不止注册一个地方需要短信发送，因此我们将短信发送抽取为微服务：`ly-sms`，凡是需要的地方都可以使用。

另外，因为短信发送API调用时长的不确定性，为了提高程序的响应速度，短信发送我们都将采用异步发送方式，即：

- 短信服务监听MQ消息，收到消息后发送短信。
- 其它服务要发送短信时，通过MQ通知短信微服务。



### 1）创建module

![1577263325821](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1577263325821.png) 



### 2）依赖坐标

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>leyou</artifactId>
        <groupId>com.leyou</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>ly-sms</artifactId>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>com.aliyun</groupId>
            <artifactId>aliyun-java-sdk-core</artifactId>
            <version>4.1.1</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-amqp</artifactId>
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



### 3）编写启动类

```java
@SpringBootApplication
public class LySmsApplication {
    public static void main(String[] args) {
        SpringApplication.run(LySmsApplication.class, args);
    }
}
```



### 4）编写application.yml

```yml
server:
  port: 8086
spring:
  application:
    name: sms-service
  rabbitmq:
    host: 127.0.0.1
    username: leyou
    password: leyou
    virtual-host: /leyou
```











## 05、短信服务：封装短信工具

### 1）抽取属性

我们首先把一些常量抽取到application.yml中：

```yaml
ly:
  sms:
    accessKeyID: LTAIfmmL26haCK0b # 你自己的accessKeyId
    accessKeySecret: pX3RQns9ZwXs75M6Isae9sMgBLXDfY # 你自己的AccessKeySecret
    signName: 乐优商城 # 签名名称
    verifyCodeTemplate: SMS_133976814 # 模板名称
    domain: dysmsapi.aliyuncs.com # 域名
    action: SendSMS # API类型，发送短信
    version: 2017-05-25 # API版本，固定值
    regionID: cn-hangzhou # 区域id
```

然后注入到属性类中：

```java
package com.leyou.sms.config;

import lombok.Data;
import org.springframework.boot.context.properties.ConfigurationProperties;

@Data
@ConfigurationProperties(prefix = "ly.sms")
public class SmsProperties {
    /**
     * 账号
     */
    String accessKeyID;
    /**
     * 密钥
     */
    String accessKeySecret;
    /**
     * 短信签名
     */
    String signName;
    /**
     * 短信模板
     */
    String verifyCodeTemplate;
    /**
     * 发送短信请求的域名
     */
    String domain;
    /**
     * API版本
     */
    String version;
    /**
     * API类型
     */
    String action;
    /**
     * 区域
     */
    String regionID;
}
```



### 2）阿里客户端交给spring管理

首先，把发请求需要的客户端注册到Spring容器：

```java
package com.leyou.sms.config;

import com.aliyuncs.DefaultAcsClient;
import com.aliyuncs.IAcsClient;
import com.aliyuncs.profile.DefaultProfile;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * @author 黑马程序员
 */
@Configuration
@EnableConfigurationProperties(SmsProperties.class)
public class SmsConfiguration {

    @Bean
    public IAcsClient acsClient(SmsProperties prop){
        DefaultProfile profile = DefaultProfile.getProfile(
                prop.getRegionID(), prop.getAccessKeyID(), prop.getAccessKeySecret());
        return new DefaultAcsClient(profile);
    }
}
```



### 3）发送短信工具类

我们把阿里提供的demo进行简化和抽取，封装一个工具类：

```java
package com.leyou.sms.utils;

import com.aliyuncs.CommonRequest;
import com.aliyuncs.CommonResponse;
import com.aliyuncs.IAcsClient;
import com.aliyuncs.exceptions.ClientException;
import com.aliyuncs.exceptions.ServerException;
import com.aliyuncs.http.MethodType;
import com.leyou.common.enums.ExceptionEnum;
import com.leyou.common.exception.LyException;
import com.leyou.common.utils.JsonUtils;
import com.leyou.sms.config.SmsProperties;
import lombok.extern.slf4j.Slf4j;
import org.apache.commons.lang3.StringUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import java.util.Map;

import static com.leyou.sms.config.SmsConstants.*;

/**
 * 短信发送工具类
 */
@Slf4j
@Component
public class SmsUtils {

    @Autowired
    private IAcsClient acsClient;

    @Autowired
    private SmsProperties prop;

    public void sendMessage(String PhoneNumbers, String SignName, String template, String param){


        CommonRequest request = new CommonRequest();
        request.setMethod(MethodType.POST);
        request.setDomain(prop.getDomain());
        request.setVersion(prop.getVersion());
        request.setAction(prop.getAction());
        request.putQueryParameter(SMS_PARAM_REGION_ID, prop.getRegionID());
        request.putQueryParameter(SMS_PARAM_KEY_PHONE, PhoneNumbers);
        request.putQueryParameter(SMS_PARAM_KEY_SIGN_NAME, SignName);
        request.putQueryParameter(SMS_PARAM_KEY_TEMPLATE_CODE, template);
        request.putQueryParameter(SMS_PARAM_KEY_TEMPLATE_PARAM, param);
        try {
            CommonResponse response = acsClient.getCommonResponse(request);
            if(response.getHttpStatus() >= 300){
                log.error("【SMS服务】发送短信失败。响应信息：{}", response.getData());
            }
            // 获取响应体
            Map<String, String> resp = JsonUtils.toMap(response.getData(), String.class, String.class);
            // 判断是否是成功
            if(!StringUtils.equals(OK, resp.get(SMS_RESPONSE_KEY_CODE))){
                // 不成功，
                log.error("【SMS服务】发送短信失败，原因{}", resp.get(SMS_RESPONSE_KEY_MESSAGE));
                throw new LyException(ExceptionEnum.SEND_MESSAGE_ERROR);
            }
            log.info("【SMS服务】发送短信成功，手机号：{}, 响应：{}", PhoneNumbers, response.getData());
        } catch (ServerException e) {
            log.error("【短信服务】发送短信失败，阿里云服务端异常，{}",e.getMessage(),e);
            throw new LyException(ExceptionEnum.SEND_MESSAGE_ERROR);
        } catch (ClientException e) {
            log.error("【短信服务】发送短信失败，客户端异常，{}",e.getMessage(),e);
            throw new LyException(ExceptionEnum.SEND_MESSAGE_ERROR);
        }
    }
}
```



### 4）参数KEY的静态变量

```java
package com.leyou.sms.config;

/**
 * @author 黑马程序员
 */
public final class SmsConstants {

    /**
     * 请求参数
     */
    public static final String SMS_PARAM_REGION_ID = "RegionId";
    public static final String SMS_PARAM_KEY_PHONE = "PhoneNumbers";
    public static final String SMS_PARAM_KEY_SIGN_NAME = "SignName";
    public static final String SMS_PARAM_KEY_TEMPLATE_CODE = "TemplateCode";
    public static final String SMS_PARAM_KEY_TEMPLATE_PARAM= "TemplateParam";

    /**
     * 响应结果
     */
    public static final String SMS_RESPONSE_KEY_CODE = "Code";
    public static final String SMS_RESPONSE_KEY_MESSAGE = "Message";

    /**
     * 状态
     */
    public static final String OK = "OK";
}
```

如图：

![1577266847058](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1577266847058.png) 











## 06、短信服务：MQ监听器异步发送短信

接下来，编写消息监听器，当接收到消息后，我们发送短信。

### 1）编写监听器

```java
package com.leyou.sms.mq;

import com.leyou.common.utils.JsonUtils;
import com.leyou.common.utils.RegexUtils;
import com.leyou.sms.config.SmsProperties;
import com.leyou.sms.utils.SmsUtils;
import org.springframework.amqp.core.ExchangeTypes;
import org.springframework.amqp.rabbit.annotation.Exchange;
import org.springframework.amqp.rabbit.annotation.Queue;
import org.springframework.amqp.rabbit.annotation.QueueBinding;
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import org.springframework.util.CollectionUtils;

import java.util.Map;

import static com.leyou.common.constants.MQConstants.Exchange.SMS_EXCHANGE_NAME;
import static com.leyou.common.constants.MQConstants.Queue.SMS_VERIFY_CODE_QUEUE;
import static com.leyou.common.constants.MQConstants.RoutingKey.VERIFY_CODE_KEY;

@Component
public class MessageListener {

    @Autowired
    private SmsUtils smsUtils;

    @Autowired
    private SmsProperties prop;

    /**
     * 发送短信验证码监听器
     */
    @RabbitListener(bindings = @QueueBinding(
            value=@Queue(name = SMS_VERIFY_CODE_QUEUE, durable = "true"),
            exchange = @Exchange(name = SMS_EXCHANGE_NAME, type = ExchangeTypes.TOPIC),
            key = VERIFY_CODE_KEY
    ))
    public void listenVerifyCodeMsg(Map<String,String> msg){
        //判断消息是否为空
        if(CollectionUtils.isEmpty(msg)){
            return;
        }
        // 获取手机号
        String phone = msg.get("phone");
        if(!RegexUtils.isPhone(phone)){
            return;
        }
        // 获取验证码
        String code = msg.get("code");
        if(!RegexUtils.isVerifyCode(code)){
            return;
        }
        String param = "{\"code\":\""+code+"\"}";
        // 发送短信
        smsUtils.sendMessage(phone, prop.getSignName(), prop.getVerifyCodeTemplate(), param);
    }
}
```

我们注意到，消息体是一个Map，里面有两个属性：

- phone：电话号码
- code：短信验证码



### 2）改造MQ常量

不要忘了，几个队列和交换机的名称，定义到`ly-common`中：

```java
package com.leyou.common.constants;

/**
 * @author 黑马程序员
 */
public abstract class MQConstants {

    public static final class Exchange {
        // ... 略
        /**
         * 消息服务交换机名称
         */
        public static final String SMS_EXCHANGE_NAME = "ly.sms.exchange";
    }

    public static final class RoutingKey {
        // ... 略
        /**
         * 验证码的routing-key
         */
        public static final String VERIFY_CODE_KEY = "sms.verify.code";
    }


    public static final class Queue{
        // ... 略
        /**
         * 短信微服务服务，发送短信队列
         */
        public static final String SMS_VERIFY_CODE_QUEUE = "sms.verify.code.queue";
    }
}
```

### 3）改造RegexUtils

我们可以判断手机号码和验证码的完整性，在`RegexUtils`中加入方法：

```java
package com.leyou.common.utils;

import org.apache.commons.lang3.StringUtils;

/**
 * @author 黑马程序员
 */
public class RegexUtils {
    /**
     * 省略其他方法
     */

    private static boolean matches(String str, String regex){
        if (StringUtils.isBlank(str)) {
            return false;
        }
        return str.matches(regex);
    }

    /**
     * 判断验证码是否合法
     * @param code
     * @return true:合法，false：不合法
     */
    public static boolean isVerifyCode(String code){
        if(StringUtils.isBlank(code)){
            return false;
        }
        return code.matches(RegexPatterns.SMS_REGEX);
    }
}
```

### 4）RegexPatterns加入正则常量

另外我们在正则常量类中加入验证 验证码的常量：

```java
package com.leyou.common.utils;

/**
 * @author 黑马程序员
 */
public abstract class RegexPatterns {
    /**
     * 省略其他
     */
    /**
     * 短信验证码
     */
    public static final String SMS_REGEX = "^\\d{6}$";

}

```



### 5）单元测试

```java
package com.leyou.sms.mq;

import lombok.extern.slf4j.Slf4j;
import org.apache.commons.lang3.RandomStringUtils;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.amqp.core.AmqpTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

import java.util.HashMap;
import java.util.Map;

import static com.leyou.common.constants.MQConstants.Exchange.SMS_EXCHANGE_NAME;
import static com.leyou.common.constants.MQConstants.RoutingKey.VERIFY_CODE_KEY;

@Slf4j
@RunWith(SpringRunner.class)
@SpringBootTest
public class MessageListenerTest {

    @Autowired
    private AmqpTemplate amqpTemplate;

    @Test
    public void listenVerifyCodeMsg() {
        String code = RandomStringUtils.randomNumeric(6);
        log.warn("code: {}", code);
        Map<String, String> map = new HashMap<>();
        map.put("phone", "13800138000");
        map.put("code", code);
        amqpTemplate.convertAndSend(SMS_EXCHANGE_NAME, VERIFY_CODE_KEY, map );
    }
}
```



查看RabbitMQ控制台，发现交换机已经创建：

 ![1527239600218](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1527239600218.png) 









## 07、用户中心：接口文档介绍

Redis和短信我们都准备就绪了，接下来就是开搞用户注册了，开发用户注册，我们之前分析了注册功能，这时候开发这些功能我们要参照接口文档来开发，开发文档在课前资料中，我们打开API接口文档，找到下图部分：

![1577273341831](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1577273341831.png) 













## 08、用户中心：搭建用户中心

用户搜索到自己心仪的商品，接下来就要去购买，但是购买必须先登录。所以接下来我们编写用户中心，实现用户的登录和注册功能。

用户中心的提供的服务：

- 用户的注册
- 用户登录
- 用户个人信息管理
- 用户地址管理
- 用户收藏管理



这里我们暂时先实现基本的：`注册和登录`功能，其它功能大家可以自行补充完整。

因为用户中心的服务其它微服务也会调用，因此这里我们做聚合：

- ly-user：父工程，包含3个子工程：
  - ly-user-client：接口
  - ly-user-dto:实体
  - ly-user-service：业务和服务



### 1）创建父module

![1577328413615](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1577328413615.png) 

该项目打包方式记得改为pom ：

![1577328556607](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1577328556607.png) 



### 2）创建ly-user-dto

我们在ly-user下创建module：ly-user-dto

![1577328629109](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1577328629109.png) 

 

### 3）创建ly-user-client

我们在ly-user下创建module：ly-user-client

![1577328682036](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1577328682036.png) 

 

### 4）创建ly-user-service

![1577328836318](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1577328836318.png) 



主要的业务逻辑在这个module中，所以我们现在在这个module中添加依赖和启动类等。



> pom文件加入依赖坐标

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>ly-user</artifactId>
        <groupId>com.leyou</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>ly-user-service</artifactId>

    <dependencies>
        <!--web启动器-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--eureka客户端-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <!-- 通用Mapper启动器 -->
        <dependency>
            <groupId>tk.mybatis</groupId>
            <artifactId>mapper-spring-boot-starter</artifactId>
        </dependency>
        <!-- mysql驱动 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <dependency>
            <groupId>com.leyou</groupId>
            <artifactId>ly-user-dto</artifactId>
            <version>1.0-SNAPSHOT</version>
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



> 启动类

```java
@SpringBootApplication
@EnableDiscoveryClient
@MapperScan("com.leyou.user.mapper")
public class LyUserApplication {
    public static void main(String[] args) {
        SpringApplication.run(LyUserApplication.class,args);
    }
}
```



> 配置文件：application.yml

```yml
server:
  port: 8085
spring:
  application:
    name: user-service
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://127.0.0.1:3306/leyou
    username: root
    password: passw0rd
eureka:
  client:
    service-url:
      defaultZone: http://127.0.0.1:10086/eureka
  instance:
    ip-address: 127.0.0.1
    prefer-ip-address: true
mybatis:
  type-aliases-package: com.leyou.user.entity
  configuration:
    map-underscore-to-camel-case: true
logging:
  level:
    com.leyou: debug
```



> 父工程 ly-user的pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>leyou</artifactId>
        <groupId>com.leyou</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>
    
    <artifactId>ly-user</artifactId>
    <packaging>pom</packaging>
    <modules>
        <module>ly-user-dto</module>
        <module>ly-user-client</module>
        <module>ly-user-service</module>
    </modules>
</project>
```







### 5）添加路由网关

我们修改`ly-gateway`配置，添加路由规则，对`ly-user-service`进行路由:

![1577329581013](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1577329581013.png) 













## 09、用户中心：后台功能准备

### 1）数据库表结构

```sql
CREATE TABLE `tb_user` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `username` varchar(32) NOT NULL COMMENT '用户名',
  `password` varchar(60) NOT NULL COMMENT '密码，加密存储',
  `phone` varchar(11) DEFAULT NULL COMMENT '注册手机号',
  `create_time` timestamp NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `update_time` timestamp NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `username` (`username`) USING BTREE,
  UNIQUE KEY `phone` (`phone`)
) ENGINE=InnoDB AUTO_INCREMENT=30 DEFAULT CHARSET=utf8 COMMENT='用户表';
```

数据结构比较简单，因为根据用户名查询的频率较高，所以我们给用户名创建了索引

看下数据结构算法：

https://www.cs.usfca.edu/~galles/visualization/Algorithms.html





### 2）实体类

```java
package com.leyou.user.entity;

import lombok.Data;
import tk.mybatis.mapper.annotation.KeySql;

import javax.persistence.Id;
import javax.persistence.Table;
import java.util.Date;

@Table(name = "tb_user")
@Data
public class User {
    @Id
    @KeySql(useGeneratedKeys = true)
    private Long id;
    private String username;
    private String password;
    private String phone;
    private Date createTime;
    private Date updateTime;
}
```



### 3）mapper

```java
package com.leyou.user.mapper;

import com.leyou.user.entity.User;
import tk.mybatis.mapper.common.Mapper;

public interface UserMapper extends Mapper<User> {
}
```



### 4）Service

```java
package com.leyou.user.service;

import com.leyou.user.mapper.UserMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    @Autowired
    private UserMapper userMapper;

}
```



### 5）Controller

```java
package com.leyou.user.web;

import com.leyou.user.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class UserController {

    @Autowired
    private UserService userService;

}
```







## 10、用户中心：校验数据是否唯一

### 1）接口说明

#### 接口路径

```
GET /check/{data}/{type}
```

#### 参数说明

| 参数 | 说明                                 | 是否必须 | 数据类型 | 默认值 |
| ---- | ------------------------------------ | -------- | -------- | ------ |
| data | 要校验的数据                         | 是       | String   | 无     |
| type | 要校验的数据类型：1，用户名；2，手机 | 是       | Integer  | 无     |

#### 返回结果

返回布尔类型结果：

- true：可用
- false：不可用

状态码：

- 200：校验成功
- 400：参数有误
- 500：服务器内部异常







### 2）Controller

因为有了接口，我们可以不关心页面，所有需要的东西都一清二楚：

- 请求方式：GET
- 请求路径：/check/{param}/{type}
- 请求参数：param,type
- 返回结果：true或false

```java
package com.leyou.user.web;

import com.leyou.user.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class UserController {

    @Autowired
    private UserService userService;

    /**
     * 数据校验
     * @param data 要校验的数据
     * @param type 要校验的数据类型：1，用户名；2，手机
     * @return
     */
    @GetMapping("/check/{data}/{type}")
    public ResponseEntity<Boolean> checkData(@PathVariable("data") String data,
                                             @PathVariable("type") Integer type){
        return ResponseEntity.ok(userService.checkData(data, type));
    }
}
```







### 3）Service

```java
package com.leyou.user.service;

import com.leyou.common.enums.ExceptionEnum;
import com.leyou.common.exception.LyException;
import com.leyou.user.entity.User;
import com.leyou.user.mapper.UserMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    @Autowired
    private UserMapper userMapper;

    public Boolean checkData(String data, Integer type) {
        User record = new User();
        // 条件判断
        switch (type){
            case 1:
                record.setUsername(data);
                break;
            case 2:
                record.setPhone(data);
                break;
            default:
                throw new LyException(ExceptionEnum.INVALID_PARAM_ERROR);
        }
        return userMapper.selectCount(record) == 0;
    }
}
```



### 4）测试

我们查看数据库中的数据：

![1577330768054](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1577330768054.png) 

然后启动微服务，在浏览器调用接口，测试用户名：

![1577330914146](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1577330914146.png) 

![1577330871961](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1577330871961.png) 



测试手机号码：

![1577330973722](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1577330973722.png) 

![1577331011749](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1577331011749.png) 









## 11、用户中心：发送短信验证码

短信微服务已经准备好，我们就可以继续编写用户中心接口了。

### 1）思路分析

这里的业务逻辑是这样的：

- 1）我们接收页面发送来的手机号码
- 2）生成一个随机验证码
- 3）将验证码保存在服务端
- 4）发送短信，将验证码发送到用户手机



那么问题来了：验证码保存在哪里呢？

验证码有一定有效期，一般是5分钟，我们可以利用Redis的过期机制来保存。







### 2）接口文档

#### 功能说明

用户输入手机号，点击发送验证码，前端会把手机号发送到服务端。服务端生成随机验证码，长度为6位，纯数字。并且调用短信服务，发送验证码到用户手机。

步骤：

- 接收用户请求，手机号码
- 验证手机号格式
- 生成验证码
- 保存验证码到redis
- 发送RabbitMQ消息到ly-sms

#### 接口路径

```
POST /code
```

#### 参数说明

| 参数  | 说明           | 是否必须 | 数据类型 | 默认值 |
| ----- | -------------- | -------- | -------- | ------ |
| phone | 用户的手机号码 | 是       | String   | 无     |

#### 返回结果

无

状态码：

- 204：请求已接收
- 400：参数有误
- 500：服务器内部异常









### 3）导入依赖及配置

这里的逻辑会稍微复杂：

- 生成随机验证码
- 将验证码保存到Redis中，用来在注册的时候验证
- 发送验证码到`ly-sms-service`服务，发送短信

因此，我们需要引入Redis和AMQP：

> pom.xml中导入redis和amqp的依赖

```xml
<!--redis-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<!--amqp-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```



> 在application.yml中添加RabbitMQ和Redis配置

```yml
spring:
  redis:
    host: 127.0.0.1
  rabbitmq:
    host: 127.0.0.1
    username: leyou
    password: leyou
    virtual-host: /leyou
```









### 4）Controller

```java
/**
 * 发送短信验证码
 * @param phone 手机号
 * @return
 */
@GetMapping("/code")
public ResponseEntity<Void> sendCode(@RequestParam("phone") String phone){
    userService.sendCode(phone);
    return ResponseEntity.status(HttpStatus.NO_CONTENT).build();
}
```









### 5）Service

```java
@Autowired
private StringRedisTemplate redisTemplate;

@Autowired
private AmqpTemplate amqpTemplate;

private static final String KEY_PREFIX = "user:verify:phone:";

public void sendCode(String phone) {
    // 服务端接收，并验证手机号
    if(!RegexUtils.isPhone(phone)){
        throw new LyException(ExceptionEnum.INVALID_PHONE_NUMBER);
    }
    // 生成随机验证码
    String code = RandomStringUtils.randomNumeric(6);
    // 存储验证码，存入redis中
    redisTemplate.opsForValue().set(KEY_PREFIX + phone, code, 5, TimeUnit.MINUTES);
    // 发给 ly-sms
    Map<String, String> map = new HashMap<>();
    map.put("phone", phone);
    map.put("code", code);
    amqpTemplate.convertAndSend(SMS_EXCHANGE_NAME, VERIFY_CODE_KEY, map );
}
```

注意：

- 手机号校验使用了ly-common中定义的正则工具类
- 要设置短信验证码在Redis的缓存有效时间















## 12、用户中心：用户注册

### 1）接口说明

#### 功能说明

用户页面填写数据，发送表单到服务端，服务端实现用户注册功能。需要对用户密码进行加密存储，使用MD5加密，加密过程中使用随机码作为salt加盐。另外还需要对用户输入的短信验证码进行校验。

- 验证短信验证码
- 生成盐
- 对密码加密
- 写入数据库

#### 接口路径

```
POST /register
```

#### 参数说明

form表单格式

| 参数     | 说明                                     | 是否必须 | 数据类型 | 默认值 |
| -------- | ---------------------------------------- | -------- | -------- | ------ |
| username | 用户名，格式为4~30位字母、数字、下划线   | 是       | String   | 无     |
| password | 用户密码，格式为4~30位字母、数字、下划线 | 是       | String   | 无     |
| phone    | 手机号码                                 | 是       | String   | 无     |
| code     | 短信验证码                               | 是       | String   | 无     |

#### 返回结果

无返回值。

状态码：

- 201：注册成功
- 400：参数有误，注册失败
- 500：服务器内部异常，注册失败







### 2）Controller

```java
/**
 * 注册用户
 * @param user
 * @param code
 * @return
 */
@PostMapping("/register")
public ResponseEntity<Void> register(User user, @RequestParam("code") String code){
    userService.register(user, code);
    return ResponseEntity.status(HttpStatus.CREATED).build();
}
```



### 3）Service

基本逻辑：

- 1）校验短信验证码
- 2）对密码加密
- 3）写入数据库

密码加密：

密码加密使用传统的MD5加密并不安全，这里我们使用的是Spring提供的BCryptPasswordEncoder加密算法，分成加密和验证两个过程：

- 加密：算法会对明文密码随机生成一个salt，使用salt结合密码来加密，得到最终的密文。
- 验证密码：需要先拿到加密后的密码和要验证的密码，根据已加密的密码来推测出salt，然后利用相同的算法和salt对要验证码的密码加密，与已加密的密码对比即可。

为了防止有人能根据密文推测出salt，我们需要在使用BCryptPasswordEncoder时配置随即密钥：

```java
package com.leyou.user.config;

import lombok.Data;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;

import java.security.SecureRandom;

/**
 * @author 黑马程序员
 */
@Data
@Configuration
@ConfigurationProperties(prefix = "ly.encoder.crypt")
public class PasswordConfig {

    private int strength;
    private String secret;

    @Bean
    public BCryptPasswordEncoder passwordEncoder(){
        // 利用密钥生成随机安全码
        SecureRandom secureRandom = new SecureRandom(secret.getBytes());
        // 初始化BCryptPasswordEncoder
        return new BCryptPasswordEncoder(strength, secureRandom);
    }
}
```

在配置文件中配置属性：

```yaml
ly:
  encoder:
    crypt:
      secret: ${random.uuid} # 随机的密钥，使用uuid
      strength: 10 # 加密强度4~31，决定了密码和盐加密时的运算次数，超过10以后加密耗时会显著增加
```



代码：

```java
@Autowired
private BCryptPasswordEncoder passwordEncoder;

public void register(User user, String code) {
    // 1 校验验证码
    // 1.1 取出redis中的验证码
    String cacheCode = redisTemplate.opsForValue().get(KEY_PREFIX + user.getPhone());
    // 1.2 比较验证码
    if (!StringUtils.equals(code, cacheCode)) {
        throw new LyException(ExceptionEnum.INVALID_VERIFY_CODE);
    }
    // 2 对密码加密
    user.setPassword(passwordEncoder.encode(user.getPassword()));
    // 3 写入数据库
    int count = userMapper.insertSelective(user);
    if (count != 1) {
        throw new LyException(ExceptionEnum.INSERT_OPERATION_FAIL);
    }
}
```











## 13、用户中心：服务端数据校验

刚才虽然实现了注册，但是服务端并没有进行数据校验，而前端的校验是很容易被有心人绕过的。所以我们必须在后台添加数据校验功能：

我们这里会使用Hibernate-Validator框架完成数据校验：

而SpringBoot的web启动器中已经集成了相关依赖：

![1577336953660](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1577336953660.png) 



### 1）什么是Hibernate Validator

Hibernate Validator是Hibernate提供的一个开源框架，使用注解方式非常方便的实现服务端的数据校验。

官网：http://hibernate.org/validator/

![1577337059910](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1577337059910.png) 



**hibernate Validator** 是 Bean Validation 的参考实现 。

Hibernate Validator 提供了 JSR 303 规范中所有内置 constraint（约束） 的实现，除此之外还有一些附加的 constraint。

在日常开发中，Hibernate Validator经常用来验证bean的字段，基于注解，方便快捷高效。







### 2）Bean校验的注解

常用注解如下：

| **Constraint**                                     | **详细信息**                                                 |
| -------------------------------------------------- | ------------------------------------------------------------ |
| **@Valid**                                         | 被注释的元素是一个对象，需要检查此对象的所有字段值           |
| **@Null**                                          | 被注释的元素必须为 null                                      |
| **@NotNull**                                       | 被注释的元素必须不为 null                                    |
| **@AssertTrue**                                    | 被注释的元素必须为 true                                      |
| **@AssertFalse**                                   | 被注释的元素必须为 false                                     |
| **@Min(value)**                                    | 被注释的元素必须是一个数字，其值必须大于等于指定的最小值     |
| **@Max(value)**                                    | 被注释的元素必须是一个数字，其值必须小于等于指定的最大值     |
| **@DecimalMin(value)**                             | 被注释的元素必须是一个数字，其值必须大于等于指定的最小值     |
| **@DecimalMax(value)**                             | 被注释的元素必须是一个数字，其值必须小于等于指定的最大值     |
| **@Size(max,   min)**                              | 被注释的元素的大小必须在指定的范围内                         |
| **@Digits   (integer, fraction)**                  | 被注释的元素必须是一个数字，其值必须在可接受的范围内         |
| **@Past**                                          | 被注释的元素必须是一个过去的日期                             |
| **@Future**                                        | 被注释的元素必须是一个将来的日期                             |
| **@Pattern(value)**                                | 被注释的元素必须符合指定的正则表达式                         |
| **@Email**                                         | 被注释的元素必须是电子邮箱地址                               |
| **@Length**                                        | 被注释的字符串的大小必须在指定的范围内                       |
| **@NotEmpty**                                      | 被注释的字符串的必须非空                                     |
| **@Range**                                         | 被注释的元素必须在合适的范围内                               |
| **@NotBlank**                                      | 被注释的字符串的必须非空                                     |
| **@URL(protocol=,host=,   port=,regexp=, flags=)** | 被注释的字符串必须是一个有效的url                            |
| **@CreditCardNumber**                              | 被注释的字符串必须通过Luhn校验算法，银行卡，信用卡等号码一般都用Luhn计算合法性 |







### 3）给User添加校验

我们在User对象的部分属性上添加注解：

```java
@Table(name = "tb_user")
@Data
public class User {
    @Id
    @KeySql(useGeneratedKeys = true)
    private Long id;
    @Pattern(regexp = RegexPatterns.USERNAME_REGEX, message = "用户名格式不正确")
    private String username;
    @Length(min = 4, max = 30, message = "密码格式不正确")
    private String password;
    @Pattern(regexp = RegexPatterns.PHONE_REGEX, message = "手机号格式不正确")
    private String phone;
    private Date createTime;
    private Date updateTime;
}
```







### 4）在controller上进行控制

在controller中只需要给User添加 @Valid注解即可。

![1577345073689](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1577345073689.png) 





### 5）测试

我们故意填错，然后SpringMVC会自动返回错误信息：

![1577345163508](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1577345163508.png) 



如果需要自定义返回结果，可以这么写：

```java
/**
 * 注册用户
 * @param user
 * @param code
 * @return
 */
@PostMapping("/register")
public ResponseEntity<Void> register(@Valid User user, BindingResult result, @RequestParam("code") String code){
    // 校验用户数据
    if(result.hasErrors()){
        List<ObjectError> allErrors = result.getAllErrors();
        String msg = allErrors.stream()
                .map(ObjectError::getDefaultMessage)
                .collect(Collectors.joining("|"));
        throw new LyException(400, msg);
    }
    userService.register(user, code);
    return ResponseEntity.status(HttpStatus.CREATED).build();
}
```

我们在User参数后面跟一个BindingResult参数，不管校验是否通过，都会进入方法内部。如何判断校验是否通过呢？

BindingResult中会封装错误结果，我们通过result.hashErrors来判断是否有错误，然后荣光result.getFieldErrors来获取错误信息。

再次测试：

![1577345745350](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1577345745350.png) 























































## 14、用户查询：根据用户名和密码查询用户

### 1）接口说明

#### 功能说明

查询功能，根据参数中的用户名和密码查询指定用户

#### 接口路径

```
GET /query
```

#### 参数说明

form表单格式

| 参数     | 说明                                     | 是否必须 | 数据类型 | 默认值 |
| -------- | ---------------------------------------- | -------- | -------- | ------ |
| username | 用户名，格式为4~30位字母、数字、下划线   | 是       | String   | 无     |
| password | 用户密码，格式为4~30位字母、数字、下划线 | 是       | String   | 无     |

#### 返回结果

用户的json格式数据

```json
{
    "id": 6572312,
    "username":"test",
    "phone":"13688886666"
}
```



状态码：

- 200：查询成功
- 400：用户名或密码错误
- 500：服务器内部异常，查询失败





### 2）创建DTO

这里要返回的结果与数据库字段不一致，需要在`ly-user-pojo`中定义一个dto：

```java
package com.leyou.user.dto;

import lombok.Data;

/**
 * @author 黑马程序员
 */
@Data
public class UserDTO {
    private Long id;
    private String username;
    private String phone;
}
```





### 3）Controller

```java
/**
     * 根据用户名和密码查询用户
     * @param username 用户名
     * @param password 密码
     * @return 用户信息
     */
@GetMapping("query")
public ResponseEntity<UserDTO> queryUserByUsernameAndPassword(
    @RequestParam("username") String username, @RequestParam("password") String password){
    return ResponseEntity.ok(userService.queryUserByUsernameAndPassword(username, password));
}
```





### 4）service

```java
public UserDTO queryUserByUsernameAndPassword(String username, String password) {
    // 1根据用户名查询
    User u = new User();
    u.setUsername(username);
    User user = userMapper.selectOne(u);
    // 2判断是否存在
    if (user == null) {
        // 用户名错误
        throw new LyException(ExceptionEnum.INVALID_USERNAME_PASSWORD);
    }

    // 3校验密码
    if(!passwordEncoder.matches(password, user.getPassword())){
        // 密码错误
        throw new LyException(ExceptionEnum.INVALID_USERNAME_PASSWORD);
    }
    return BeanHelper.copyProperties(user, UserDTO.class);
}
```

要注意，查询时也要对密码进行加密后判断是否一致。





### 5）测试

我们通过RestClient测试：

 ![1554548239554](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1554548239554.png)









## 15、API文档：Swagger的介绍

完成了用户中心接口的开发，接下来我们就要测试自己的接口了，而且为了方便前端调用和参考，我们最好提供一份更直观的api文档，这里我们介绍一个工具，叫做swagger-ui

什么是swagger呢？swagger是对Open-API的一种实现。那么，什么是OpenAPI呢？

### 1）什么是OpenAPI

随着互联网技术的发展，现在的网站架构基本都由原来的后端渲染，变成了：前端渲染、前后端分离的形态，而且前端技术和后端技术在各自的道路上越走越远。  前端和后端的唯一联系，变成了API接口；API文档变成了前后端开发人员联系的纽带，变得越来越重要。

没有API文档工具之前，大家都是手写API文档的，在什么地方书写的都有，而且API文档没有统一规范和格式，每个公司都不一样。这无疑给开发带来了灾难。

OpenAPI规范（OpenAPI Specification 简称OAS）是Linux基金会的一个项目，试图通过定义一种用来描述API格式或API定义的语言，来规范RESTful服务开发过程。目前V3.0版本的OpenAPI规范已经发布并开源在github上 。

官网：https://github.com/OAI/OpenAPI-Specification





### 2）什么是swagger？

OpenAPI是一个编写API文档的规范，然而如果手动去编写OpenAPI规范的文档，是非常麻烦的。而Swagger就是一个实现了OpenAPI规范的工具集。

官网：https://swagger.io/

看官方的说明：



Swagger包含的工具集：

- **Swagger编辑器：** Swagger Editor允许您在浏览器中编辑YAML中的OpenAPI规范并实时预览文档。
- **Swagger UI：** Swagger UI是HTML，Javascript和CSS资产的集合，可以从符合OAS标准的API动态生成漂亮的文档。
- **Swagger Codegen：**允许根据OpenAPI规范自动生成API客户端库（SDK生成），服务器存根和文档。
- **Swagger Parser：**用于解析来自Java的OpenAPI定义的独立库
- **Swagger Core：**与Java相关的库，用于创建，使用和使用OpenAPI定义
- **Swagger Inspector（免费）：** API测试工具，可让您验证您的API并从现有API生成OpenAPI定义
- **SwaggerHub（免费和商业）：** API设计和文档，为使用OpenAPI的团队构建。











## 16、API文档：Swagger的使用



### 1）快速入门

SpringBoot已经集成了Swagger，使用简单注解即可生成swagger的API文档。

#### 引入依赖

```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.8.0</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.8.0</version>
</dependency>
```

#### 编写配置

```java
package com.leyou.user.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@Configuration
@EnableSwagger2
public class SwaggerConfig {
    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
                .host("localhost:8085")
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.leyou.user.web"))
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("乐优商城用户中心")
                .description("乐优商城用户中心接口文档")
                .version("1.0")
                .build();
    }
}
```

#### 启动测试

重启服务，访问：http://localhost:8085/swagger-ui.html

就能看到swagger-ui为我们提供的API页面了：

![1554549137117](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1554549137117.png)

可以看到我们编写的4个接口，任意点击一个，即可看到接口的详细信息：

![1554554238765](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1554554238765.png)

可以看到详细的接口声明，包括：

- 请求方式：
- 请求路径
- 请求参数
- 响应等信息

点击右上角的`try it out!`还可以测试接口：

![1554554464170](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1554554464170.png)

填写参数信息，点击execute，可以发起请求并测试：

![1554554533108](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1554554533108.png)







### 2）自定义接口说明

刚才的文档说明中，是swagge-ui根据接口自动生成，不够详细。如果有需要，可以通过注解自定义接口声明。常用的注解包括：

```java
/**
 @Api：修饰整个类，描述Controller的作用
 @ApiOperation：描述一个类的一个方法，或者说一个接口
 @ApiParam：单个参数描述
 @ApiModel：用对象来接收参数
 @ApiProperty：用对象接收参数时，描述对象的一个字段
 @ApiResponse：HTTP响应其中1个描述
 @ApiResponses：HTTP响应整体描述
 @ApiIgnore：使用该注解忽略这个API
 @ApiError ：发生错误返回的信息
 @ApiImplicitParam：一个请求参数
 @ApiImplicitParams：多个请求参数
 */
```

示例：

```java
/**
     * 校验数据是否可用
     * @param data
     * @param type
     * @return
     */
@GetMapping("/check/{data}/{type}")
@ApiOperation(value = "校验用户名数据是否可用，如果不存在则可用")
@ApiResponses({
    @ApiResponse(code = 200, message = "校验结果有效，true或false代表可用或不可用"),
    @ApiResponse(code = 400, message = "请求参数有误，比如type不是指定值")
})
public ResponseEntity<Boolean> checkUserData(
    @ApiParam(value = "要校验的数据", example = "lisi") @PathVariable("data") String data,
    @ApiParam(value = "数据类型，1：用户名，2：手机号", example = "1") @PathVariable(value = "type") Integer type) {
    return ResponseEntity.ok(userService.checkData(data, type));
}
```

查看文档：

![1554555057087](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/乐优商城/图片/1554555057087.png)







