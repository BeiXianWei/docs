# PTUNS API DOC


## 会员模块

### 1、登录接口

Url：{{BaseUrl}}/portal/login
RequestMethod:Post
Content-Type:application/json
请求参数：
| :---- | :---- | :---- | :---- | :---- |
|参数名|必填|数据类型|说明|备注|
| mobile | 是 | String | 手机号码 | -- |
| smsCode | 是 | String | 短信验证码 | -- |


## 商品模块




## 订单模块





## 售后模块





## 通用模块











[官网API文档](  https://docs.spring.io/spring-boot/docs/2.2.7.RELEASE/api/ )

[功能详细介绍](https://docs.spring.io/spring-boot/docs/2.2.7.RELEASE/reference/html/spring-boot-features.html#boot-features)

## spring boot 是什么，能做什么，能解决什么问题

1. Spring Boot 是基于 spring framework 5.0 开发的全新框架 。其设计是为了让开发者尽可能快的跑起来 Spring 应用程序并且尽可能减少你的配置文件。 

2. Spring Boot 最大的特点是遵循“约定大于配置” ，零配置，解决传统Spring项目的繁琐冗长配置问题；且集成了大量常用的第三方库，让各个组件开箱即用；让开发者快速构建企业级应用。

3. Spring Boot 是 Spring Cloud 微服务架构的基础。

![image-20200512201653895](image\image-20200512201653895.png)



## spring boot 怎么用

### 要求

1.  依赖管理器版本要求
   - Maven 需要 3.3 以上版本
   - Gradle 需要 5.0 以上版本

2.  Servlet容器 版本要求（需要 支持Servlet 3.1+）
   - Tomcat 9.0  |  Servlet 4.0
   - Jetty 9.4 | Servlet 3.1
   - Undertow 2.0  |  Servlet 4.0

### 编码

1. 创建spring boot项目，步骤如下：

   - 通过Spring Initializr，选择 SDK 及 start 地址，点击下一步

   ![image-20200515182300171](image\image-20200515182300171.png)

   - 输入对应的项目名等信息，点击下一步

     ![image-20200515183158704](image\image-20200515183158704.png)

   - 选择Spring Boot 版本 并勾选 需要使用到的 组件及依赖，点击下一步

     ![image-20200515183354870](image\image-20200515183354870.png)

   - 输入项目名并 选择 项目存放路径 ，点击完成按钮即可

2. 项目启动入口：DemoApplication.java

   ```java
   package com.example.demo;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   
   //标识为spring boot项目，其实是 @ComponentScan、@EnableAutoConfiguration 的复合注解
   @SpringBootApplication
   public class DemoApplication {
   
   	public static void main(String[] args) {
   		SpringApplication.run(DemoApplication.class, args);
   	}
   
   }
   ```

3. 写一个接口

   ```java
   	@RequestMapping(value = "/xm", method = RequestMethod.GET)
   	@ApiOperation("say hello to xm")
   	public ResponseData helloXM() {
   		return ResponseUtil.success("say hello to xm!");
   	}
   ```

4. @RestController 类注解

   是 @Controller + @ResponseBody 的复合注解

5. @SpringBootApplication

   - @Configuration

     标识类是 配置类，并可以被Spring Boot扫描并添加到Spring容器

   - @EnableAutoConfiguration

     开启Spring Boot自动配置

   - @ComponentScan

     自动包扫描并添加到Spring容器

6. yml 配置文件（相比 properties 配置文件 更简洁）**冒号后面必须要有空格**

   - spring.profiles.active=dev

   - spring.datasource

     ```yml
     spring:
       profiles:
         active: dev
     
     mybatis:
       mapper-locations: classpath:mapper/*Mapper.xml
       type-aliases-package: com.example.demo.entity
     spring:
       datasource:
         url: jdbc:mysql://localhost:3306/springboot?useUnicode=true&characterEncoding=utf8&useSSL=false&serverTimezone=GMT%2B8
         driver-class-name: com.mysql.cj.jdbc.Driver
         username: root
         password: beilife
     ```

7. @ConfingurationProperties

   - 配置文件配置对应的对象或属性

     ```yml
     pet:
       type: dog
       name: wangcai
       weight: 5kg
       height: 30cm
       favorite: bone
     ```

   - 新建PetProperties.java文件，添加类注解：@Component 和 @ConfigurationProperties

     ```java
     package com.example.demo.properties;
     
     import lombok.Data;
     import org.springframework.boot.context.properties.ConfigurationProperties;
     import org.springframework.stereotype.Component;
     
     // lombok 插件，会对类的属性自动编译生成对应getter和setter,toString等方法
     @Data
     // spring会自动扫描并添加到SpringApplicationContext中
     @Component
     // prefix 为配置 文件中的前缀
     @ConfigurationProperties(prefix = "pet")
     public class PetProperties {
     
     	private String type;
     	private String name;
     	private String weight;
     	private String height;
     	private String favorite;
     
     }
     
     ```

   - @Autowired 自动注入并使用

     ![image-20200515183949493](image\image-20200515183949493.png)

8. 自动配置案例

   - Spring Boot 集成Mybatis

     - 使用Mysql，数据库表如下：

       ```sql
       CREATE TABLE `user` (
         `id` INT(32) NOT NULL AUTO_INCREMENT,
         `userName` VARCHAR(32) NOT NULL,
         `password` VARCHAR(50) NOT NULL,
         `age` INT(32) DEFAULT NULL,
         PRIMARY KEY (`id`)
       )
       ```

     - 引入Maven依赖

       ```xml
       <!-- mybatis -->
       		<dependency>
                   <groupId>org.mybatis.spring.boot</groupId>
                   <artifactId>mybatis-spring-boot-starter</artifactId>
                   <version>2.1.2</version>
               </dependency>
       <!-- mysql -->
               <dependency>
                   <groupId>mysql</groupId>
                   <artifactId>mysql-connector-java</artifactId>
                   <scope>runtime</scope>
               </dependency>
       ```

     - 在配置文件中配置mybatis.mapper-location  :  mapper.xml的位置

       ```yml
  mybatis:
         mapper-locations: classpath:mapper/*Mapper.xml # 对应下图的路径位置
       ```
     
       ![image-20200515184335366](image\image-20200515184335366.png)

     - 在启动类中添加 ： @MapperScan("[basePackage]")

       ```java
  package com.example.demo;
       
       import org.mybatis.spring.annotation.MapperScan;
       import org.springframework.boot.SpringApplication;
       import org.springframework.boot.autoconfigure.SpringBootApplication;
       
       @SpringBootApplication
       //指定Mybatis Mapper对应的包，所有Mybatis的Mapper接口类必须放在 指定的 包中
       @MapperScan("com.example.demo.mapper")
       public class DemoApplication {
       
       	public static void main(String[] args) {
       		SpringApplication.run(DemoApplication.class, args);
       	}
       
       }
       ```
     
     - 添加User实体类

       ```java
  package com.example.demo.entity;
       
       import lombok.Data;
       
       @Data
       public class User {
       	private int id;
       	private String userName;
       	private String password;
       	private int age;
       }
       ```
     
     - 添加UserMapper.xml 及 UserMapper.java

       ```xml
  <!-- UserMapper.xml -->
       <?xml version="1.0" encoding="UTF-8"?>
       <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
       <mapper namespace="com.example.demo.mapper.UserMapper">
       
           <select id="selectAll" resultType="com.example.demo.entity.User">
               SELECT * FROM USER
           </select>
       </mapper>
       ```
     
       ```java
  //UserMapper.java
       package com.example.demo.mapper;
       
       import com.example.demo.entity.User;
       import java.util.List;
       
       public interface UserMapper {
       
       	List<User> selectAll();
           
       }
       ```
     
     - 添加UserService.java 和 UserServiceImpl.java

       ```java
  //UserService.java
       package com.example.demo.service;
       
       import com.example.demo.entity.User;
       
       import java.util.List;
       
       public interface UserService {
       
       	List<User> selectAll();
           
       }
       ```
     
       ```java
  //UserServiceImpl.java
       package com.example.demo.service.impl;
       
       import com.example.demo.entity.User;
       import com.example.demo.mapper.UserMapper;
       import com.example.demo.service.UserService;
       import org.springframework.beans.factory.annotation.Autowired;
       import org.springframework.stereotype.Service;
       
       import java.util.List;
       
       @Service
       public class UserServiceImpl implements UserService {
       
       	@Autowired
       	private UserMapper userMapper;
       
       	@Override
       	public List<User> selectAll() {
       
       		return userMapper.selectAll();
       	}
           
       }
       
       ```
     
     - 在controller中添加一个API接口

       ```java
  package com.example.demo.controller;
       
       import com.example.demo.ResponseResult.ResponseData;
       import com.example.demo.ResponseResult.ResponseUtil;
       import com.example.demo.service.UserService;
       import org.springframework.beans.factory.annotation.Autowired;
       import org.springframework.web.bind.annotation.*;
       
       @RestController
       @RequestMapping(value = "/hello")
       public class HelloController {
       
       	@Autowired
       	private UserService userService;
       
       	@GetMapping("/queryAllUser")
       	@ApiOperation("查询所有User")
       	public ResponseData queryAllUser(){
       		return ResponseUtil.success(userService.selectAll());
       	}
       }
       ```
     
     - 启动项目并查看接口

       ![image-20200515185418621](image\image-20200515185418621.png)

       

   - Spring Boot 集成swagger

     - 引入Maven依赖

       ```xml
    <!-- swagger2 -->
               <dependency>
                   <groupId>io.springfox</groupId>
                   <artifactId>springfox-swagger2</artifactId>
                   <version>2.9.2</version>
               </dependency>
               <dependency>
                   <groupId>io.springfox</groupId>
                   <artifactId>springfox-swagger-ui</artifactId>
                   <version>2.9.2</version>
               </dependency>
               <dependency>
                   <groupId>com.google.guava</groupId>
                   <artifactId>guava</artifactId>
                   <version>21.0</version>
               </dependency>
       ```
   
     - 添加SwaggerConfig.java

       ```java
    package com.example.demo.config;
       
       import io.swagger.annotations.ApiOperation;
       import org.springframework.context.annotation.Bean;
       import org.springframework.context.annotation.Configuration;
       import springfox.documentation.builders.ApiInfoBuilder;
       import springfox.documentation.builders.PathSelectors;
       import springfox.documentation.builders.RequestHandlerSelectors;
       import springfox.documentation.service.ApiInfo;
       import springfox.documentation.service.Contact;
       import springfox.documentation.spi.DocumentationType;
       import springfox.documentation.spring.web.plugins.Docket;
       import springfox.documentation.swagger2.annotations.EnableSwagger2;
       
       @EnableSwagger2
       @Configuration
       public class SwaggerConfig {
       
       	@Bean
       	public Docket createRestApi() {
       		return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo()).select()
       				.apis(RequestHandlerSelectors.basePackage("com.example.demo.controller"))
       				//扫描所有有注解的api，用这种方式更灵活
       				.apis(RequestHandlerSelectors.withMethodAnnotation(ApiOperation.class))
       				.paths(PathSelectors.any()).build();//.globalOperationParameters(pars);
       	}
       
       	@Bean
       	public ApiInfo apiInfo() {
       		return new ApiInfoBuilder().description("Interface Portal Restful Api").title("Interface Portal")
       				.version("0.1.0-SNAPSHOT").contact(new Contact("Beilife", "", "beixianwei@126.com"))
       				.termsOfServiceUrl("").build();
       	}
       }
       
       ```
   
     - 在Controller中添加对应的注解

       ```java
    package com.example.demo.controller;
       
       import com.example.demo.ResponseResult.ResponseData;
       import com.example.demo.ResponseResult.ResponseUtil;
       import com.example.demo.properties.PersonProperties;
       import com.example.demo.properties.PetProperties;
       import com.example.demo.service.UserService;
       import io.swagger.annotations.Api;
       import io.swagger.annotations.ApiImplicitParam;
       import io.swagger.annotations.ApiOperation;
       import org.springframework.beans.factory.annotation.Autowired;
       import org.springframework.web.bind.annotation.*;
       
       @Api(tags = "HelloController")
       @RestController
       @RequestMapping(value = "/hello")
       public class HelloController {
       
       	@Autowired
       	private UserService userService;
       
       	@GetMapping("/queryAllUser")
       	@ApiOperation("查询所有User")
       	public ResponseData queryAllUser(){
       		return ResponseUtil.success(userService.selectAll());
       	}
       }
       ```
   
       

     - 重启项目并打开浏览器，输入：http://localhost:8080/swagger-ui.html

       ![image-20200515185735235](image\image-20200515185735235.png)

9. 打包运行

- mvn clean package（maven 打包会生成两个包，一个是fat jar，一个是源码包）

  ![image-20200515180149776](image\image-20200515180149776.png)

- java -jar jar包 --spring.profiles.active=dev

  打开CMD，cd到jar包的路径

  ![image-20200515180604261](image\image-20200515180604261.png)

  打开浏览器输入：http://localhost:8080





### Demo 在code文件夹中

