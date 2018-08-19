# Spring Boot 2.0 升级指南

**前言**
本文档主要通过开发指南的方式来帮助您将应用程序迁移到 Spring Boot 2.0，参考官方文档翻译

**在你开始之前**
首先，Spring Boot 2.0.0 要求 Java 8 或更高版本，不再支持 Java 6 和 7。

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

**Spring Boot 新特性**
* 默认动态代理策略

Spring Boot现在默认使用CGLIB动态代理(基于类的动态代理), 包括AOP. 如果需要基于接口的动态代理(JDK基于接口的动态代理) , 需要设置spring.aop.proxy-target-class属性为false.


**开发 Web 应用程序** 

* 嵌入式容器包装结构

为了支持响应式用例，嵌入式容器包结构已经被大幅度的重构。 EmbeddedServletContainer已被重新命名为，WebServer并且该org.springframework.boot.context.embedded包已被重新定位到org.springframework.boot.web.embedded。例如，如果您使用TomcatEmbeddedServletContainerFactory回调接口定制嵌入式 Tomcat 容器，则应该使用TomcatServletWebServerFactory。

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

Mustache模板的文件扩展名曾经是.html。现在的扩展名为.mustache,与官方规格和大多数的IDE插件保持一致。你可以通过更改spring.mustache.suffix配置文件的configuration key来重写这个新的默认值。

**Jackson/JSON支持**

在2.0版本，我们已经设置Jackson配置文件的默认值，将ISO-8601 strings写作了JSR-310。如果你希望返回之前的设置，可以添加spring.jackson.serialization.write-dates-as-timestamps=true 到你的配置文件中。

一个新的spring-boot-starter-json starter收集了必要的位去读写JSON。它不仅提供了jackson-databind，而且提供了和Java8一起运作的时候相当有用的组件：jackson-datatype-jdk8, jackson-datatype-jsr310 和jackson-module-parameter-names。如果你曾经手动地依赖这些组件，现在可以依赖这个新的starter取代。

