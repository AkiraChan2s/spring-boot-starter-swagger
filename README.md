# 简介

该项目主要利用Spring Boot的自动化配置特性来实现快速的将swagger2引入spring boot应用来生成API文档，简化原生使用swagger2的整合代码。

- GitHub：https://github.com/dyc87112/spring-boot-starter-swagger
- 码云：http://git.oschina.net/didispace/spring-boot-starter-swagger
- 博客：http://blog.didispace.com

**小工具一枚，欢迎使用和Star支持，如使用过程中碰到问题，可以提出Issue，我会尽力完善该Starter**

# 版本基础

- Spring Boot：1.5.x
- Swagger：2.7.x

# 如何使用

在该项目的帮助下，我们的Spring Boot可以轻松的引入swagger2，主需要做下面两个步骤：

- 在`pom.xml`中引入依赖：

```xml
<dependency>
	<groupId>com.didispace</groupId>
	<artifactId>spring-boot-starter-swagger</artifactId>
	<version>1.2.0.RELEASE</version>
</dependency>
```

- 在应用主类中增加`@EnableSwagger2Doc`注解

```java
@EnableSwagger2Doc
@SpringBootApplication
public class Bootstrap {

    public static void main(String[] args) {
        SpringApplication.run(Bootstrap.class, args);
    }

}
```

默认情况下就能产生所有当前Spring MVC加载的请求映射文档。

# 参数配置

更细致的配置内容参考如下：

## 配置示例

```properties
swagger.title=spring-boot-starter-swagger
swagger.description=Starter for swagger 2.x
swagger.version=1.1.0.RELEASE
swagger.license=Apache License, Version 2.0
swagger.licenseUrl=https://www.apache.org/licenses/LICENSE-2.0.html
swagger.termsOfServiceUrl=https://github.com/dyc87112/spring-boot-starter-swagger
swagger.contact.name=didi
swagger.contact.url=http://blog.didispace.com
swagger.contact.email=dyc87112@qq.com
swagger.base-package=com.didispace
swagger.base-path=/**
swagger.exclude-path=/error, /ops/**
```

## 配置说明

### 默认配置

```
- swagger.title=标题
- swagger.description=描述
- swagger.version=版本
- swagger.license=许可证
- swagger.licenseUrl=许可证URL
- swagger.termsOfServiceUrl=服务条款URL
- swagger.contact.name=维护人
- swagger.contact.url=维护人URL
- swagger.contact.email=维护人email
- swagger.base-package=swagger扫描的基础包，默认：全扫描
- swagger.base-path=需要处理的基础URL规则，默认：/**
- swagger.exclude-path=需要排除的URL规则，默认：空
```

### Path规则说明

`swagger.base-path`和`swagger.exclude-path`使用ANT规则配置。

我们可以使用`swagger.base-path`来指定所有需要生成文档的请求路径基础规则，然后再利用`swagger.exclude-path`来剔除部分我们不需要的。

比如，通常我们可以这样设置：

```properties
management.context-path=/ops

swagger.base-path=/**
swagger.exclude-path=/ops/**, /error
```

上面的设置将解析所有除了`/ops/`开始以及spring boot自带`/error`请求路径。

其中，`exclude-path`可以配合`management.context-path=/ops`设置的spring boot actuator的context-path来排除所有监控端点。

### 分组配置

当我们一个项目的API非常多的时候，我们希望对API文档实现分组。从1.2.0.RELEASE开始，将支持分组配置功能。

![分组功能](https://github.com/dyc87112/spring-boot-starter-swagger/blob/master/images/swagger-group.png)

具体配置内容如下：

```
- swagger.docket.<name>.title=标题
- swagger.docket.<name>.description=描述
- swagger.docket.<name>.version=版本
- swagger.docket.<name>.license=许可证
- swagger.docket.<name>.licenseUrl=许可证URL
- swagger.docket.<name>.termsOfServiceUrl=服务条款URL
- swagger.docket.<name>.contact.name=维护人
- swagger.docket.<name>.contact.url=维护人URL
- swagger.docket.<name>.contact.email=维护人email
- swagger.docket.<name>.base-package=swagger扫描的基础包，默认：全扫描
- swagger.docket.<name>.base-path=需要处理的基础URL规则，默认：/**
- swagger.docket.<name>.exclude-path=需要排除的URL规则，默认：空
```

说明：`<name>`为swagger文档的分组名称，同一个项目中可以配置多个分组，用来划分不同的API文档。

**分组配置示例**

```properties
swagger.docket.aaa.title=group-a
swagger.docket.aaa.description=Starter for swagger 2.x
swagger.docket.aaa.version=1.2.0.RELEASE
swagger.docket.aaa.termsOfServiceUrl=https://gitee.com/didispace/spring-boot-starter-swagger
swagger.docket.aaa.contact.name=zhaiyongchao
swagger.docket.aaa.contact.url=http://spring4all.com/
swagger.docket.aaa.contact.email=didi@potatomato.club
swagger.docket.aaa.excludePath=/ops/**

swagger.docket.bbb.title=group-bbb
swagger.docket.bbb.basePackage=com.yonghui
```

说明：默认配置与分组配置可以一起使用。在分组配置中没有配置的内容将使用默认配置替代，所以默认配置可以作为分组配置公共部分属性的配置。
