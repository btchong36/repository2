# SpringBoot

Spring和SpingMVC的整合

目的是消除配置文件

starter是核心

要选择使用GA版本

https://start.spring.io/  下载案例

普通创建项目

配置pom文件

配置父工程

父工程配置了大量的版本号

记录了配置文件的路径
![image-20200228104256422](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200228104256422.png)

配置启动器
spring有各种启动器，它帮我们定义了各种第三方框架（非常方便）

requestmapping

### 注解

配置主配置类

@SpringBootApplication 注解--加在Controller，service等注解配节

---@SpringBootConfiguration

---------@configuration注解（没听懂）

用于定义配置类，替换xml文件，相当于< beans>，里面包含多个含有@Bean的注解。

​	![image-20200228111236002](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200228111236002.png)

----@EnableAutoConfiguration 开启自动配置

-----------@AutoconfigurationPackage 自动扫描包，扫描主配置类所在目录里所有的包
-----------@Import   导入加载自动配置



将项目打包成jar用于测试，里面内置tomcat服务器



SpringBoot配置文件

默认在resources里，名称application.properties/application.yml/application.yaml



属性:【空格】值

默认不加引号，加双引号识别转义字符，加单引号原样输出

```
server:
 port: 8080
 servlet:
  context-path: /my   --项目根路径
```

实体类的注解

@ConfigurationProperties(prefix="yml文件里的属性名，表示将yml的值插入到对象中")

测试类的注解

@SpringBootTest

当yml和properties文件里有相同属性时，以properties文件内容为准。

springboot会依次读取两种配置文件，只要找到一个即可。



### 属性赋值

![image-20200228145512571](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200228145512571.png)





自定义对象增加提示效果



@value赋值方式

${yml中属性名}

spel表达式 #{}

直接赋值

@ConfigurationProperties(prefix="")  加在class上面。可以给对象赋值，不能使用spel表达式和直接赋值。



@PropertySource()  放在类上，指定配置文件

@ImportSource(value=("classpath:beans.xml"))   ---导入spring配置文件

packing选项：pom--父工程，war--网页，jar--普通java工程

### 两种配置文件

```java
 //一般属性
spring.application.name=compute-service
server.port=80
server.tomcat.uri-encoding=GBK
//自定义属性
com.blog.name=博客
com.blog.uri=博客地址
```

### 文件占位符

### 实例化bean

加@Bean，id等于方法名

### 配置文件加载位置

按以下顺序先后加载，所有文件都会加载，形成互补

```
java -jar xxxx.jar --spring.config.location=D:/kawa/application.yml

项目根目录/config(不会打包)

项目根目录（不会打包）

resource/config

resource
```

### Profile多环境下的配置

三种环境下的配置文件优先级比默认的高

#### properties文件方式

首先创建三种环境下的配置文件

application-**dev/prod/test**.properties

指定配置文件方式

1. 在application.properties里指定配置文件

   spring.profiles.active=dev/prod/test

2. 虚拟机配置（优先级最高）

   -Dspring.profiles.active=dev/prod/test

![image-20200302100402488](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200302100402488.png)



3. 执行jar时指定配置文件
   -spring.profiles.active=dev/prod/test

#### yml文件方式

直接在一个yml文件里编辑

也同样适用虚拟机配置和jar配置

```
spring:
 profies:
  active:dev
---
server:
 port:8086
spring:
 profiles:test
---
server:
 port:8087
spring:
 profiles:dev
 ---
 server:
 port:8088
spring:
 profiles:prod
```

### 运行jar时指定参数

```
//不同参数用空格隔开
java -jar xxxx.jar --server.port=8080 --server.serverlet.context-path=/demo

java -jar xxxx.jar --spring.config.location=D:/kawa/application.yml
```

### springboot对日志的支持

 log4j-日志系统，出来较早

slf4j--日志框架

通过中间的适配，可以通过slf4j操作各种日志，我们只要学习slf4j的API即可



日志级别：

* Trace--最全
* Debug--开发
* Info--生产
* Warn
* Error



设置日志级别



输出日志到文件

```
logging:
 level:
  com.woniu: trace
 file: G/my/springboot.log  //输出到指定目录
 path: /spring/log   //输出到项目所在目录
 //两个目录都会输出
```



两种日志配置文件

log4j.properties--针对log4j的日志配置文件

logback.xml--针对logback的日志配置文件



springboot引入日志logback.xml，放在resource目录，可以设置文件输出目录，输出格式等，也可以用springboot的日志名logback-spring.xml

logback-spring.xml里的环境和运行环境必须相同，否则会报错。



logback-spring.xml里只需要改路径、级别，日志大小

### 对静态资源的访问

#### 通过webjars

webjars.com  --寻找静态资源

用依赖的形式导入jquery



两个路径的关系



![image-20200302142314323](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200302142314323.png)

![image-20200302142320384](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200302142320384.png)



![image-20200302142817111](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200302142817111.png)  --引用的地址



#### 通过resource下面的静态文件夹

springboot会自动扫描着四个目录，用于存放静态资源

resource

--META-INF/resources（不行）

--public   --存放html            可以                                

--static  可以

--resources 可以

 jva

--static 存放sj

![image-20200302144054366](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200302144054366.png)

访问时不需要添加这四个文件夹路径

classpath  --resouces目录


### 设置页面图标

把favicion.ico放在静态目录即可

https://tool.lu/favicon/   

### WebMvcConfigurer（重要）

必须要做，扩展MVC功能

如果在类上添加@EnableWebMvc，会使所有自动配置失效

设置controller和页面跳转

![image-20200303100848655](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200303100848655.png)

#### addResourceHandler



添加静态资源访问



### 设置热启动（没用）

![image-20200303101549847](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200303101549847.png)

![image-20200303101835686](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200303101835686.png)

修改文件后按ctrl F9 build

restful

### 拦截器

### 整合数据源

![image-20200305093113657](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200305093113657.png)

![image-20200305093304685](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200305093304685.png)

添加mysql驱动

![image-20200305093502920](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200305093502920.png)

DataSourceConfiguration配置了多个数据源，它们实现了DataSource接口，DataSourceConfiguration由spring提供

![image-20200305093832839](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200305093832839.png)

通过DataSource接口，可以获得连接对象，并且进行CRUD

![image-20200305093809799](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200305093809799.png)

DataSource配置文件

![image-20200305094226437](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200305094226437.png)



读取配置文件

![image-20200305163919301](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200305163919301.png)

#### 传统步骤

1. 配置application.yml
   1. 配置数据源
      加上数据源，建议加上时区
      ![image-20200305094620204](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200305094620204.png)

2. 创建dao层
   alt+enter 抛异常
   ![image-20200305095501290](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200305095501290.png)
3. 创建测试类
   ![image-20200305095546795](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200305095546795.png)

2.0开始默认数据源从tomcat换为![image-20200305095711160](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200305095711160.png)

关闭资源

![image-20200305100436457](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200305100436457.png)

#### 现在步骤，内置数据源

dao层![image-20200305101222118](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200305101222118.png)
![image-20200305101912777](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200305101912777.png)

#### 使用第三方数据源

导入

配置yml文件

![image-20200305110449390](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200305110449390.png)

### 整合JDBC

```properties
spring.datasource.username=root
spring.datasource.password=123456
spring.datasouce.url=jdbc:mysql:///0308?useUnicode=true&characterEncoding=utf8&serverTimezone=UTC
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```



### 整合mbatis(注解版)

优点：不需要配置

缺点：不能使用动态sql



### ![image-20200305113637225](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200305113637225.png)



@Mapper 创建接口的代理对象



get-查询

post-保存

delete-删除

put-更新





![image-20200305144129476](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200305144129476.png)

**新增数据后返回ID**

![image-20200305145335719](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200305145335719.png)

 为什么有红线，因为是接口，没有实现类。但是程序一旦运行就会个接口创建代理对象。解决方法是DeptMapper.java添加@Repository注解

![image-20200306022150545](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200306022150545.png)



### 整合mybatis（配置文件版）

1. yml添加

```
mybatis:
 config-location: classpath:mybatis/mybatis-config.xml
 mapper-location: classpath:mybatis/mapper/*.xml
```

2. 在resouces目录创建mybatis目录，存放mybatis的xml配置文件，添加分页插件的依赖



3. 创建mapper.xml文件，resouces/mybatis/mapper

@Mapper 通过接口生成代理对象

4. 可以不适用@Mapper接口，在启动类中添加@MapperScan(value="com.woniu.mapper")
5. 在mapper.xml里将emp和数据库表映射



insert返回ID，resultType必须加，否则会报错

![image-20200305162143770](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200305162143770.png)



### 整合c3p0

1. 配置c3p0Config.java，生成c3p0对象，并且将yml里的属性载入进去

### 代码生成器

* 配置依赖

  ```xml
  <dependency>
              <groupId>org.mybatis.generator</groupId>
              <artifactId>mybatis-generator-core</artifactId>
              <version>1.3.7</version>
          </dependency>
  ```

* 拷贝mbg.xml到根目录

  ```
  &要用&amp;代替
  ```

  

* 拷贝生成java

  

![image-20200306095002439](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200306095002439.png)

#### mbg.xml

* url不能使用&![image-20200306095211628](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200306095211628.png)

### 事务

```
//在service上添加注解
@Transactional
//在主配置类添加注解
@EnableTransactionManagement
```

### 设置web容器参数

实现ConfigurableServletWebServerFactory接口



![image-20200309095553837](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200309095553837.png)

### 注册自定义servlet

中央控制器通过路径来访问控制器

![image-20200309095546351](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200309095546351.png)

### 过滤器

实现Filter接口（注意包），然后在myConfigurableServletWebServerFactory中注册

contextInitialized方法常用语加载配置文件

destroy常用于清空缓存

### 监听器

实现特定的listener接口，然后

### 自定义web容器

web依赖里排除tomcat，引入其它web容器，比如jetty，undertow

|        | tomcat | jetty | undertow |
| ------ | ------ | ----- | -------- |
| jsp    | 支持   |       | 不支持   |
| 长连接 | 不好   | 较好  | 中等     |

### 配置多数据源



# thymeleaf

### 引入依赖

```xml
 <dependency>         
    <groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

### 添加提示

```html
<html lang="en" xmlns:th="http://www.thymeleaf.org">
```





![image-20200302161525737](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200302161525737.png)



![image-20200302162613975](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200302162613975.png)

Controller里，默认会在返回值里添加**/templates/**前缀和.html后缀

### Controller传递数据到thymeleaf的三种方式

* Map
* Model
* ModelAndView

### 常用指令

``` html  
<span th:text="${name}">admin</span>  //以文本显示，如果${name}获取不到则显示admin
th:text="张三"
th:text="111aaa"
th:utext="${}"   //以HTML效果显示

//格式化时间
${#dates.format(date, 'dd/MMM/yyyy HH:mm')}

th:object="${user}"  //定义子标签的对象
	th:text="*{uname}"   //父对象的属性
th:if=""   //优先级比较高

//直接修改标签属性
th:id=
th:class=

th:each="u:${lists}" th:if="$"


th:each="u,stat:${lists}"  //stat可以获取下标
th:text="${stat.index}"   //输出序号

${session.info}   session对象

${#strings.isEmpty()}  //判断字符串是否为空

//字符串拼接

th:include

@{/js/jquery3.1.js}  //使用绝对路径引用资源

th:href="@{http://www.baidu.com(name="peter",pass="1234")}"  //url传参，等于www.baidu.com?name=peter&pass=1234

//行内写法
[[#{login.remember}]]
```

### 编译加载静态资源

使用@{}引用图片会出现无法引用的情况，因为target不编译图片目录

放在<build>内

![image-20200303095701788](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200303095701788.png)

### 关闭缓存

```
spring:
 thymeleaf:
 	cache:false
```

![image-20200303105645180](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200303105645180.png)

### 国际化

在resouces目录创建i18n，里面创建properties文件

spring.messages.basename=i18n.login，注意messages在spring的下一级 

使用#{login.btn}  读取配置文件文字



yml配置

![image-20200306111144117](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200306111144117.png)



#### 语言超链接

```java
//设置超链接，login.html不是页面，而是路径
th:href="@{/login.html(lang=zh_CN)}"
//创建自定义解析器，实现LocalResolver接口
拦截请求，获取lang的值，通过字符串分隔得到国家和语言
```

实现LocaleResolver接口，根据获取的lang值设置Locale对象

```java
@Configuration
public class MyLocaleResolver implements LocaleResolver {


    @Override
    public Locale resolveLocale(HttpServletRequest request) {
        String language=request.getParameter("lang");
        System.out.println("language="+language);
        Locale locale=Locale.getDefault();
        if(!StringUtils.isEmpty(language)){
            String[] s=language.split("_");
            locale=new Locale(s[0], s[1]);
        }

        return locale;
    }

    @Override
    public void setLocale(HttpServletRequest request, HttpServletResponse response, Locale locale) {

    }
}
```

在配置类，添加自定义的解析器

```java
 @Bean
 public LocaleResolver localeResolver(){
     return new MyLocaleResolver();
}
```



### 抽取公共片段

![image-20200305001658455](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200305001658455.png)

![image-20200305001733193](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200305001733193.png)

前面是html页面名称，后面是fragment名称



### th:insert,th:replace,th:include区别

- **`th:insert`**  ：保留自己的主标签，保留th:fragment的主标签。
- `th:replace` ：不要自己的主标签，保留th:fragment的主标签。
- `th:include` ：保留自己的主标签，不要th:fragment的主标签。（官方3.0后不推荐）



# 问答系统项目

login.html  不是页面，只是路径

图片，css，js 加th:href  th:link  @{}

pom里添加打包静态资源

requestMapping两级路径一定要加/

@PostMapping  --只接收post请求

![image-20200303145357653](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200303145357653.png)

### 登录功能

![image-20200303152840686](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200303152840686.png)

### 转发

![image-20200303153030443](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200303153030443.png)

### 拦截

springboot 1.x  自动静态资源映射  但是2.X后拦截

经过拦截器需要添加static

### 引入顶部和左侧片段

![image-20200304094912844](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200304094912844.png)

![image-20200304095000940](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200304095000940.png)

修改左侧dashboard链接

![image-20200304095209952](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200304095209952.png)







### 左侧链接高亮

在链接里添加参数flag

![image-20200305005154103](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200305005154103.png)

在链接的class类判断flag字段

![image-20200304100038541](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200304100038541.png)

![image-20200304101559065](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200304101559065.png)



restuful风格传参

方法1

![image-20200305092449248](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200305092449248.png)

方法2

![image-20200305092456426](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200305092456426.png)



### 其它

![image-20200304095752568](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200304095752568.png)



Controller里要用集合来获得数据



搜索的反显

![image-20200304105810103](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200304105810103.png)

新增功能

![image-20200304144654043](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200304144654043.png)

不能使用转发

日期属性添加注解

![image-20200304145236801](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200304145236801.png)



**修改功能**

修改按钮向后传值

![image-20200304151845843](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200304151845843.png)

**删除功能**

![image-20200304153327498](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200304153327498.png)

![image-20200304153712108](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200304153712108.png)

del方法里通过ajax传送到后台

![image-20200304155207929](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200304155207929.png)



**restful风格传参**

![image-20200304185934492](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200304185934492.png)

### 重写（加入数据库）

* 配置c3p0
  注意url要加时区

  

* 逆向工程

* mybatis

* 

![image-20200306101613288](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200306101613288.png)



![image-20200306101728535](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200306101728535.png)

生日属性

![image-20200306114316895](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200306114316895.png)

### 思考题

### 首页制作

首页数据封装到一个类里

question里封装cate属性，封装用户（id，名称，头像）

answer里封装user，question



映射对象属性可以用automapping



启动类在其它位置

![image-20200310151301626](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200310151301626.png)



**vue**

![image-20200310153712863](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200310153712863.png)

beforeMount  渲染前查询

### 图片URL

![image-20200311093104777](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200311093104777.png)

### 上传图片

enctype

multipartfile

### 要求功能

1. 首页分类显示
   1. 首页链接能点进去
   2. 在问-显示所有问题
2. 个人中心
   1. 待回答
   2. 我的提问
   3. 我的回答
   4. 修改个人资料
3. 拦截器
4. 发问题
5. 回答问题
6. 评论

上午阶段考试，下午项目验收

周三多线程

周四--四阶段开始

# SpringData

用于处理数据，包括关系型数据库和非关系型数据库

无视持久层框架

我们不需要编写实现，只要操作接口和提供sql语句。

本质是操作JPA接口。

适用于中小型系统，大型系统不适用



![image-20200304162919067](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200304162919067.png)

JPA接口的作者是hibernate的作者

ORM映射：把数据和模型对应起来

O：Object

R：relation

M：model

mybatis没有实现JPA标准

![image-20200306144300291](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200306144300291.png)

### 操作步骤

JPA底层使用Hibernate

0. 配置pom
   jdbc jpa springdata

1. 配置yml
   
2. 创建entity

   1. 在实体类class上添加@Entity注解，代表这是和数据库映射的实体
   2. 在实体类class上添加@Table(name="表名")
   3.   在实体类class上添加@Proxy(lazy=true/false)延迟加载，使用对象的时候才查询
   4. 在主键上添加@Id
   5. 主键添加生成策略@GeneratedValue(AUTO/SEQUENSE/IDENTITY)  自增长   
   6. @Column(name="",length=,unique=,nullable)  指定列名，指定长度，是否为空等参数
   7. @Tempral  指定时间类型：Date，Time，TimeStamp

3. dao层继承
   ![image-20200306151632556](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200306151632556.png)
   findStudentBySnameAnd/OrAge

   findStudentBySnameLike()

4. 调用3中创建的类的方法

### 对象关系注解

创建外键的注解

save  保存

saveAndFlush  保存和修改

级联操作

![image-20200306160836882](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200306160836882.png)

* 一对一关系

自动先保存丈夫，然后保存妻子

所有的双向关系都只需要维护一方

husband.class 不需要配置注解

* 一对多关系

![image-20200306162407907](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200306162407907.png)

狗：

![image-20200306162340658](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200306162340658.png)

* 多对对关系

通过第三张表

比如学生和课程

![image-20200306165055924](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200306165055924.png)

![image-20200306164839649](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200306164839649.png)

### jpaRepository接口的方法



# 框架复习

### spring

<bean id class> --实际存放在map里

applicationContext  

生成bean工程的时候就已经生成了对象

![image-20200309142033648](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200309142033648.png)

注入：

set注入值，注入对象

@ComponentScan  

西安，高鹏老师  26中设计模式讲的很好

手工创建BeanFactory

IOC原理：

利用反射创建对象，利用工厂模式获取对象

### AOP

把通用功能封装起来（不是创建工具类），主动提供，甚至使用者都不知道AOP的存在，作用是解耦合

概念

通知：封装的通用功能

连接点：加通知的方法

切入点：连接点组合

切面：切入点+通知

织入：把通知加入到连接点的过程



通知类型

before：方法前执行

after：方法后执行，无论方法是否报错

returnAfter：方法正常执行后执行

错误通知：报错后执行

around：方法前后都会执行



如何选择切入点：

execution表达式

### 声明式事务（不是很懂）

#### 事务的特性

#### 事务并发带来的问题

事务的隔离级别

#### spring对事务的处理

#### 事务的传播行为

### mybatis

* ORM映射

  类id----主键

  类对象关系---外键关系

* 原理

  * sqlSessionFactory，一个数据供只有一个，线程安全
  * 通过sqlSessionFactory创建sqlSession对象，是mybatis中的一级缓存
  * sqlSession创建executor（statement）
  * 调用sqlSession中的方法，进行操作数据库的操作（selectOne，selectList，insert，update，delete）

* #和$的区别
  #是占位符方式，不能防止sql注入，字符串自动单引号

  

* 参数的传递

  封装成map集合，在接口的参数前添加@Param

  映射文件中通过list arrays param[0]  args[0]  

* xml中如何封装属性对象

  association--封装单个对象

  collection--集合

  resultMap--自定义对象

  resultType--

* 缓存

# 路径专题讲解

### 绝对路径

/：前面加上服务器地址http://localhost:8080，其实也是相对路径，相对于服务器根目录

webapp-root  ---web根路径

thymeleaf的@{/***}会自动添加webroot路径，也就是服务器路径+项目路径

### 相对路径

相对于URL地址映射地址，而不是服务器内的文件地址，也就是会在路径前添加当前文件的所在路径。

 ### 页面向后台传参

1. a链接

![image-20200316103531708](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200316103531708.png)

2. thymeleaf

![image-20200316103646729](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200316103646729.png)

3. ajax
   ![image-20200316104436972](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200316104436972.png)

   ![image-20200316104842821](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200316104842821.png)	

   以上两种方法后台可以通过对象来接收参数，需要添加@RequestBody，

   3.1 前台json对象传送到后台用@RequestBody接收，前台必须用JSON.stringfy() 格式化

# 线程

时间片--最小的时间单位

上下文切换--进程/线程之间切换的时间

线程--CPU调度的最小单位，真正干活的是线程



并行--同一时刻能处理的事情数量，取决于硬件性能

并发--单位时间内，可以处理事情的数量，在相同并行条件下，衡量处理能力。



JVM原生是多线程的

![image-20200318094530339](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200318094530339.png)





**配置文档注释**

![image-20200318094837435](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200318094837435.png)



### 线程创建方式

1. 继承Thread，适用于批量产生线程

   重写run方法，然后通过start方法调用run方法

   final  修饰不能重写

   线程名Thread-X  0开始

   缺点：单继承，扩展不方便

2. 实现Runnable接口
   Runnable的实现类不是接口，只是规范了run方法

启动线程方法：

![image-20200318103212936](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200318103212936.png)

run和start的区别：run在main线程，start新建了线程

方便后期扩展，可以再继承其它类

3. 使用匿名内部类
   ![image-20200318104256464](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200318104256464.png)

**JDK1.8之后接口由默认方法，可以有实现**

**函数式接口：只能有一个抽象方法，使用注解@FunctionalInterface标注**

lambda 表达式，替换匿名内部类写法

接口  变量名=(形参) -> {方法体}

![image-20200318105114873](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200318105114873.png)

![image-20200318105509514](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200318105509514.png)

4. 通过Callable接口（没听完整）
   重写call()

   两种方式创建:普通和匿名内部类

   **面试题:**runnable和callable的区别：前者的run()没有返回值,后者有返回值

   创建匿名内部类一定要指定泛型

   1. 创建Callable对象(可以用lambda简化,需指定返回值)
   2. 用FutureTask封装
   3. 创建Thread,futureTask作为形参

![image-20200318113854473](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200318113854473.png)

runnable解决了Thread的单继承问题

Callable解决了单继承和没有返回值的问题



### 线程的状态

sleep和wait的区别：sleep不释放锁，wait释放锁

优先级：1-10，默认5，优先级越高，获得CPU时间片的概率越大。线程优先级不可靠，不作为开发的手段。

### 守护线程

中断标志位：isInterrupted()

setDaemon(true/false)  设置是否为守护线程

守护线程和主线程同生共死，确保主线程的安全

用途：保证主线程的正常运行，资源释放，服务启动

注意：当调用setDaemon方法后，守护线程中的finally方法不会执行

### 中断线程

**过时的方式**

thread.stop()：已过时，暴力停止

thread.resume()：已过时，暴力暂停

thread.suspend():已过时，暴力停止

以上三个方法会导致线程不释放资源，资源一直被占用导致死锁，所以不适用



**推荐的方式**

interrupt()：将中断标志位设置为true，

如果出中断异常，会将中断标志位改为false，如果要再次中断，需要



**响应式中断（重要，没听懂）**



### 线程安全

#### synchronied锁

* 不推荐锁方法
* 如果锁普通方法，对象就是this
* 如果锁静态方法，对象就是当前类的字节码对象
* java中任意对象都可以作为锁
* 注意：使用this作为锁的时候只能有一个对象
* run方法里调用方法，可以实现方法的复用
* 可以使用private static final 对象当做锁，但是浪费空间（不推荐）
* 推荐使用类的字节码对象作为锁：xxx.class
* if(stock<=0)   临界值判断
* ++i,--i   不是单步原子性操作，它有三个步骤：取i，i+1，赋值给i
* 直接锁方法![image-20200318160703383](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200318160703383.png)

**内置原子类**

* 里面所有操作都具有单步原子性
* 原理：CAS（Compare and Swap，比较再交换）
* 使用了原子类就不用加锁了，仍然是线程安全的

#### ReentrantLock平锁（默认）

* 可以实现响应中断
* 需要手动获取锁和解锁

* isLock() ,tryLock()

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200318164749877.png" alt="image-20200318164749877"  />

native：代表C语言实现的方法

不能使用wait，notify

 

**步骤**

1. ![image-20200318174244028](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200318174244028.png)

2. try finally
3. try里lock.lcok()
4. finally里lock.unlock()
5. 创建信号量newCondition



**方法**

![image-20200318174926400](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200318174926400.png)

signal--唤醒



condition和方法是如何绑定的？



17：15之前的没听到



### 线程的顺序执行

**哪里等待就在哪里唤醒**

notify()不靠谱，notifyAll() 靠谱

### 非法监视异常

锁对象和wait,notify等调用对象不一致

### volatile 主存可见变量

# 单利模式

# 死锁

synchronied如何产生死锁？如何解决？

ReentrantLock如何产生死锁？如何解决？





