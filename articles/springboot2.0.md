# Spring Boot 2.0 升级指南

**前言**

Spring Boot已经发布2.0有5个月多，多了很多新特性，一些坑也慢慢被填上，最近有空，就把项目中Spring Boot 版本做了升级，顺便整理下升级的时候遇到的一些坑，做个记录，参考官方文档翻译。

**开始之前**

* 2.x 至少需要 JDK 8 的支持，2.x 里面的许多方法应用了 JDK 8 的许多高级新特性，所以你要升级到 2.0 版本，先确认你的应用必须兼容 JDK 8。
另外，2.x 开始了对 JDK 9 的支持。

* 2.x 对第三方类库升级了所有能升级的稳定版本，一些值得关注的类库升级我给列出来了。

    1.Spring Framework 5+
    2.Tomcat 8.5+
    3.Flyway 5+
    4.Hibernate 5.2+
    5.Thymeleaf 3+

在 Spring Boot 2.0 中，许多配置属性被重新命名/删除，开发人员需要更新application.properties/ application.yml相应的配置。为了帮助你解决这一问题，Spring Boot 发布了一个新spring-boot-properties-migrator模块。一旦作为该模块作为依赖被添加到你的项目中，它不仅会分析应用程序的环境，而且还会在启动时打印诊断信息，而且还会在运行时为您暂时迁移属性。在您的应用程序迁移期间，这个模块是必备的：

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-properties-migrator</artifactId>
</dependency>
```
> 注意：迁移完成之后，请确保从项目的依赖关系中移除该模块。

**构建您的 Spring Boot 应用程序**

Spring Boot Maven 插件
为了保持了一致性，并且避免与其他插件发生冲突，现在暴露的插件配置属性都以一个spring-boot前缀开始。

例如，以下命令prod使用命令行启用配置文件

```
mvn spring-boot:run -Dspring-boot.run.profiles=prod
```

**Spring Boot 新特性,默认动态代理策略**

Spring Boot现在默认使用CGLIB动态代理(基于类的动态代理), 包括AOP. 如果需要基于接口的动态代理(JDK基于接口的动态代理) , 需要设置spring.aop.proxy-target-class属性为false.


**开发 Web 应用程序嵌入式容器包装结构** 

为了支持响应式用例，嵌入式容器包结构已经被大幅度的重构。 EmbeddedServletContainer已被重新命名为，WebServer并且该`org.springframework.boot.context.embedded`包已被重新定位到`org.springframework.boot.web.embedded`。例如，如果您使用TomcatEmbeddedServletContainerFactory回调接口定制嵌入式 Tomcat 容器，则应该使用TomcatServletWebServerFactory。

特定于 Servlet 的服务器属性
许多server.* 属性 ( Servlet 特有的) 已经转移到server.servlet：

旧的属性|新的属性
-|-
server.context-parameters.* | server.servlet.context-parameters.*
server.context-path | server.servlet.context-path
server.jsp.class-name | server.servlet.jsp.class-name
server.jsp.init-parameters.* | server.servlet.jsp.init-parameters.*
server.jsp.registered | server.servlet.jsp.registered
server.servlet-path | server.servlet.path

**模板引擎**

Mustache模板默认的文件扩展名

Mustache模板的文件扩展名曾经是.html。现在的扩展名为.mustache,与官方规格和大多数的IDE插件保持一致。你可以通过更改`spring.mustache.suffix`配置文件的configuration key来重写这个新的默认值。

**Jackson/JSON支持**

在2.0版本，我们已经设置Jackson配置文件的默认值，将ISO-8601 strings写作了JSR-310。如果你希望返回之前的设置，可以添加`spring.jackson.serialization.write-dates-as-timestamps=true` 到你的配置文件中。

一个新的spring-boot-starter-json starter收集了必要的位去读写JSON。它不仅提供了jackson-databind，而且提供了和Java8一起运作的时候相当有用的组件：jackson-datatype-jdk8, jackson-datatype-jsr310 和jackson-module-parameter-names。如果你曾经手动地依赖这些组件，现在可以依赖这个新的starter取代。

**HTTP/2 支持**

为 Tomcat，Undertow 和 Jetty 提供 HTTP / 2 支持。支持取决于所选的 Web 服务器和应用程序环境（因为 JDK 8 不支持该协议）。

**配置属性的绑定**

在 Spring Boot 2.0 中，用于绑定Environment属性的机制`@ConfigurationProperties`已经完全彻底修改。我们借此机会收紧了松散绑定的规则，并修复了 Spring Boot 1.x 中的许多不一致之处。

新的BinderAPI 也可以`@ConfigurationProperties`直接在你自己的代码之外使用。例如，下面将结合到List的CustomerProperty对象：

```
List<CustomerProperty> people = Binder.get(applicationContext.getEnvironment())
    .bind("customer.property", Bindable.listOf(CustomerProperty.class))
    .orElseThrow(IllegalStateException::new);
```
配置源可以像这样在 YAML 中表示：

```
customer:
  property:
  - first-name: wang
    last-name: lao
  - first-name: li
    last-name: sheng
```

**Actuator 改进**

在 Spring Boot 2.0 中 Actuator endpoints 有很大的改进。所有 HTTP Actuator endpoints 现在都在该/actuator路径下公开，并且生成的 JSON 有效负载得到了改进。

我们现在也不会在默认情况下暴露很多端点。如果您要升级现有的 Spring Boot 1.5 应用程序，请务必查看迁移指南并特别注意该`management.endpoints.web.exposure.include`属性。


**HikariCP**

Spring Boot 2.0 中的默认数据库池技术已从 Tomcat Pool 切换到 HikariCP。我们发现 Hakari 提供了卓越的性能，我们的许多用户更喜欢 Tomcat Pool

**初始化**

数据库初始化逻辑在 Spring Boot 2.0 中已经合理化。Spring Batch，Spring Integration，Spring Session 和 Quartz的初始化现在仅在使用嵌入式数据库时才会默认发生。该enabled属性已被替换为更具表现力枚举。例如，如果你想一直执行 Spring Batch 的初始化，您可以设置spring.batch.initialize-schema=always。

如果 Flyway 或 Liquibase 正在管理您的 DataSource 的模式，并且您正在使用嵌入式数据库，Spring Boot 现在会自动关闭 Hibernate 的自动 DDL 功能。

**配置文件绑定**

1.简单类型
在Spring Boot 2.0中对配置属性加载的时候会除了像1.x版本时候那样移除特殊字符外，还会将配置均以全小写的方式进行匹配和加载。所以，下面的4种配置方式都是等价的：

* properties格式：

```
spring.jpa.databaseplatform=mysql
spring.jpa.database-platform=mysql
spring.jpa.databasePlatform=mysql
spring.JPA.database_platform=mysql
```

* yaml格式：
```
spring:
  jpa:
    databaseplatform: mysql
    database-platform: mysql
    databasePlatform: mysql
    database_platform: mysql
```
Tips：推荐使用全小写配合-分隔符的方式来配置，比如：spring.jpa.database-platform=mysql

2.List类型
在properties文件中使用[]来定位列表类型，比如：

```
spring.my-example.url[0]=http://example.com
spring.my-example.url[1]=http://spring.io
```

也支持使用逗号分割的配置方式，上面与下面的配置是等价的：

```
spring.my-example.url=http://example.com,http://spring.io
```
而在yaml文件中使用可以使用如下配置：

```
spring:
  my-example:
    url:
      - http://example.com
      - http://spring.io
```

也支持逗号分割的方式：

```
spring:
  my-example:
    url: http://example.com, http://spring.io
```
注意：在Spring Boot 2.0中对于List类型的配置必须是连续的，不然会抛出UnboundConfigurationPropertiesException异常，所以如下配置是不允许的：
name[0]=aaa
name[2]=bbb
在Spring Boot 1.x中上述配置是可以的，name[1]由于没有配置，它的值会是null

3.Map类型
Map类型在properties和yaml中的标准配置方式如下：

properties格式：
```
spring.my-example.foo=bar
spring.my-example.hello=world
```
yaml格式：
```
spring:
  my-example:
    foo: bar
    hello: world
```
注意：如果Map类型的key包含非字母数字和-的字符，需要用[]括起来，比如：

```
spring:
  my-example:
    '[foo.baz]': bar
```

**测试**
对 Spring Boot 2.0 中提供的测试支持进行了一些补充和调整：

@WebFluxTest已添加新注释以支持 WebFlux 应用程序的“切片”测试。
Converter和GenericConverter豆类现在自动扫描@WebMvcTest和@WebFluxTest。
@AutoConfigureWebTestClient已经添加了一个注释来提供一个WebTestClientbean 供测试使用。注释会自动应用于@WebFluxTest测试。
增加了一个新的ApplicationContextRunner测试实用程序，可以很容易地测试您的自动配置。我们已将大部分内部测试套件移至此新模型。详细信息请参阅更新的文档。

### 升级遇到的一些问题总结

1.Spring Data JPA 2.0.x findOne() 无效

在使用 Spring Data JPA 中，根据 主键获得对象，一般是使用`<S extends T> S findOne(Example<S> example);` 方法，或者 `T getOne(ID id);`，在 spring-data-jpa  2.0.x版本中，获取单个对象的方法改为 `Optional<T> findById(ID id);`返回的是一个`Optional<T>`（java8新特性）Optional 类是一个可以为null的容器对象。如果值存在则isPresent()方法会返回true，调用get()方法会返回该对象。delete()方法和findOne()类似也被去掉了，可以使用deleteById(Long id)来替换，还有一个不同点是deleteById(Long id)默认实现返回值为void。

spring-data-jpa 1.5.2 的CrudRepository 接口
```
@NoRepositoryBean
public interface CrudRepository<T, ID extends Serializable> extends Repository<T, ID> {

    <S extends T> S save(S entity);

    <S extends T> Iterable<S> save(Iterable<S> entities);

    T findOne(ID id);

    boolean exists(ID id);

    Iterable<T> findAll();

    Iterable<T> findAll(Iterable<ID> ids);

    long count();

    void delete(ID id);

    void delete(T entity);

    void delete(Iterable<? extends T> entities);

    void deleteAll();
}
```

spring-data-jpa 2.0.8 的CrudRepository 接口
```
@NoRepositoryBean
public interface CrudRepository<T, ID> extends Repository<T, ID> {

    <S extends T> S save(S entity);

    <S extends T> Iterable<S> saveAll(Iterable<S> entities);

    Optional<T> findById(ID id);

    boolean existsById(ID id);

    Iterable<T> findAll();

    Iterable<T> findAllById(Iterable<ID> ids);

    long count();

    void deleteById(ID id);

    void delete(T entity);

    void deleteAll(Iterable<? extends T> entities);

    void deleteAll();
}
```

2.`@GeneratedValue(strategy = GenerationType.AUTO)`Spring Boot 2.0 需要指定主键的自增策略，这个和 Spring Boot 1.0 有所区别，1.0 会使用默认的策略`@GeneratedValue(strategy = GenerationType.IDENTITY)`
```
@Id
@GeneratedValue(strategy= GenerationType.IDENTITY)
private long id;
```

3.jpa2.0的方言设置如果需要指定InnoDB存储引擎可以使用`database-platform: org.hibernate.dialect.MySQL57InnoDBDialect`默认会使用myisam（不支持事务）在jpa中无法体现换成mybatis可以很好的验证。

4.日志类报错：Spring Boot 2.0 默认不包含 log4j，建议使用 slf4j 。
```
import org.apache.log4j.Logger;
protected Logger logger = Logger.getLogger(this.getClass());
```
改为：
```
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
protected Logger logger =  LoggerFactory.getLogger(this.getClass());
```
5.Thymeleaf 3.0 默认不包含布局模块。

将 Pom 包升级到 2.0之后，访问首页的时候一片空白什么都没有，查看后台也没有任何的报错信息，首先尝试着跟踪了 http 请求，对比了一下也没有发现什么异常，在查询 Thymeleaf 3.0 变化时才发现：Spring Boot 2.0 中spring-boot-starter-thymeleaf 包默认并不包含布局模块，需要使用的时候单独添加，添加布局模块如下：

```
<dependency>
   <groupId>nz.net.ultraq.thymeleaf</groupId>
   <artifactId>thymeleaf-layout-dialect</artifactId>
</dependency>
```

6.分页组件PageRequest变化。

在 Spring Boot 2.0 中 ，方法new PageRequest(page, size, sort) 已经过期不再推荐使用，推荐使用以下方式来构建分页信息：
`Pageable pageable =PageRequest.of(page, size, Sort.by(Sort.Direction.ASC,"id"));`
跟踪了一下源码发现PageRequest.of()方法，内部还是使用的new PageRequest(page, size, sort)，只是最新的写法更简洁一些。

```
public static PageRequest of(int page, int size, Sort sort) {
    return new PageRequest(page, size, sort);
}
```

