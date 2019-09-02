---
title: Spring Cloud Contract 实践
date: 2019-08-30 16:18:00
tags:
  - Spring Cloud
  - Test
categories:
  - Spring Cloud
---

# Spring Cloud Contract 实践

由于工作需要，第一次接触 Spring Cloud Contract 这个组件。中文翻译为 契约 ，它是一个用于微服务测试组件。在正常的微服务开发过程中，我们要进行测试时，通常情况下要启动全部的微服务项目，这个组件可以使我们在不启动其他微服务时完成测试。

<!-- more -->

## 什么是 Spring Cloud Contract

在正常的情况下，我们的微服务是由数量众多的调用方服务与被调用方服务组成：

![一般的微服务架构](https://cloud.spring.io/spring-cloud-contract/reference/html/images/Deps.png)

这种调用方式往往伴随着链式服务调用，如果想要测试左上角的服务，一般来说会有两个方法：

- 部署所有的微服务
- 在测试中模拟其他的微服务调用

这两种方式各有其优缺点。

**部署所有的微服务**

- 优点
  - 可以模拟生产环境
  - 测试服务之间的真实通信
- 缺点
  - 需要启动所有的配套服务、数据库和其他关联项目
  - 难以调试

**在测试中模拟其他的微服务调用**

- 优点
  - 无需依赖于其他的服务
  - 响应迅速
- 缺点
  - 模拟服务的可靠性不足

为了解决以上的问题，Spring 推出了 Spring Cloud Contract ，主要的思想是提供快速的测试响应而无需配置其他的微服务。在引入 Spring Cloud Contract 后，应用的依赖关系会变为如下图所示：

![使用了 Spring Cloud Contract](https://cloud.spring.io/spring-cloud-contract/reference/html/images/Stubs2.png)

Spring Cloud Contract 用来保证所使用的的 stubs 是由被调用方创建及维护的。

## 实践

Spring Cloud Contract 要求在被调用方声明并实现 契约 ，来由调用方进行使用。

### Provider

引入 Maven 依赖：

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-contract-verifier</artifactId>
    <scope>test</scope>
</dependency>
```

引入 Maven 插件：

```xml
<plugin>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-contract-maven-plugin</artifactId>
    <version>${spring-cloud-contract.version}</version>
    <extensions>true</extensions>
</plugin>
```

以 `REST` 形式编写 stubs 并实现，可以使用 `Grovvy` 或者 `Yaml` ，文件默认位于 `/src/test/resources/contracts` 目录下。

对于 HTTP stubs 需要指定路径、请求方法、状态码等信息，示例如下：

```groovy
package contracts

org.springframework.cloud.contract.spec.Contract.make {
    request {
        url "/api"
        method GET()
        headers {
            header(contentType(), applicationJsonUtf8())
        }
    }

    response {
        status OK()
        headers {
            header(contentType(), applicationJsonUtf8())
        }
        body(["apple", "banana"])
    }
}
```

```yaml
request:
  method: GET
  url: /api
  headers:
    Content-Type: application/json;charset=UTF-8
response:
  status: 200
  headers:
    Content-Type: application/json;charset=UTF-8
  body: ["apple", "banana"]
```

指定测试基类：

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-contract-maven-plugin</artifactId>
            <version>2.1.2.RELEASE</version>
            <extensions>true</extensions>
            <configuration>
              	<baseClassForTests>com.hello.spring.cloud.contract.provider.HelloSpringCloudContractProviderApplicationTests</baseClassForTests>
            </configuration>
        </plugin>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

指定测试接口实例：

```java
@Before
public void setUp() throws Exception {
  	RestAssuredMockMvc.standaloneSetup(new ApiController());
}
```

使用 `./mvnw clean install` 自动生成测试并进行检验，生成的测试包默认在 `target/generated-test-sources` 目录下。自动生成的测试类如下：

```java
@Test
public void validate_simpleProvider() throws Exception {
    // given:
    		MockMvcRequestSpecification request = given();

    // when:
        ResponseOptions response = given().spec(request)
          			.get("/api");

    // then:
        assertThat(response.statusCode()).isEqualTo(200);
        assertThat(response.header("Content-Type")).isEqualTo("application/json;charset=UTF-8");
    // and:
        DocumentContext parsedJson = JsonPath.parse(response.getBody().asString());
        assertThatJson(parsedJson).arrayField().contains("apple").value();
        assertThatJson(parsedJson).arrayField().contains("banana").value();
}
```

需要注意的是，必须要在测试基类中指定 `MockMvc` 实例，否则会报如下错误：

```
java.lang.IllegalStateException: 
You haven't configured a MockMVC instance. You can do this statically

RestAssuredMockMvc.mockMvc(..)
RestAssuredMockMvc.standaloneSetup(..);
RestAssuredMockMvc.webAppContextSetup(..);

or using the DSL:

given().
             mockMvc(..). ..
```

当服务中包含多个 `Controller` 实例时，也可以使用如下配置：

```java
@Autowired
private WebApplicationContext context;

@Before
public void setUp() throws Exception {
	RestAssuredMockMvc.webAppContextSetup(context);
}
```

### Consumer

添加 Maven 依赖：

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-contract-stub-runner</artifactId>
    <scope>test</scope>
</dependency>
```

编写测试用例，使用 `@AutoConfigureStubRunner` 注解：

```java
@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.NONE)
@AutoConfigureStubRunner(
        ids = {
                "com.hello:hello-spring-cloud-contract-provider-groovy:+:stubs:8081",
                "com.hello:hello-spring-cloud-contract-provider-yaml:+:stubs:8082"
        },
        stubsMode = StubRunnerProperties.StubsMode.LOCAL
)
public class HelloSpringCloudContractConsumerApplicationTests {
```

当 stubs 不在本地仓库时，使用如下方式从远程仓库获取 stubs ：

```java
@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.NONE)
@AutoConfigureStubRunner(
        ids = {
                "com.hello:hello-spring-cloud-contract-provider-groovy:+:stubs:8081",
                "com.hello:hello-spring-cloud-contract-provider-yaml:+:stubs:8082"
        },
        repositoryRoot = "https://repo.spring.io/libs-snapshot",
        stubsMode = StubRunnerProperties.StubsMode.REMOTE
)
public class HelloSpringCloudContractConsumerApplicationTests {
```

至此，Spring Cloud Contract 的基本功能已经全部实现。

## 总结

Spring Cloud Contract 为我们在微服务测试过程中提供了一个有效的解决方法，可以说是微服务部分 TDD 的最佳实践。

---

> 参考文献：
>
> - [Spring Cloud Contract Reference Documentation](https://cloud.spring.io/spring-cloud-contract/reference/html/index.html)
