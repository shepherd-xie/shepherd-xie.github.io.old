---
title: Spring
date: 2018-12-16 17:23:00
tags: 
categories: 
top:
---

![Spring Overview](./assets/spring-overview.png)

[TOC]



# [Spring Framework Documentation Version 5.1.0.RELEASE](https://docs.spring.io/spring/docs/current/spring-framework-reference/index.html)

# Core

## QuickStart

* applicationContext.xml
* BeanFactory/ApplicationContext
  * BeanFactory负责读取bean配置文档，管理bean的加载，实例化，维护bean之间的依赖关系，负责bean的声明周期。
  * ApplicationContext除了提供上述BeanFactory所能提供的功能之外，还提供了更完整的框架功能

## Bean

* `<bean id="..." class="..."/>`
* `scope` : singleton|prototype|request|session|application|websocket
* `lazy-init` : 默认支持singleton 不支持prototype
* 构造方法: 无参/有参
* 静态工厂: `<bean id="..." class="..." factory-method="..."/>`
* 实例工厂: `<bean id="..." factory-bean="..." factory-method="..."/>`

## Dependency injection - 依赖注入

* property: `<property name="..." value|ref="..."/>`
* constructor: `<constructor-arg index|name|type="..." value|ref="..."/>`
* p-namespace(property): `<bean id="..." class="..." p:driverClassName="..." p:url="..." p:username="..." p:password="..."/>`
* c-namespace(constructor): `<bean id="..." class="..." c:_0="..." c:_1-ref="..." c:name-ref="..." c:email="..."/>`
* EL(Expression Language): `<property name="..." value="#{<expression>}"/>`
  * `#` 是spring提供的EL表达式，用于获取对象的属性与方法
  * `$` 用于获取引入的properties文件中key的对应变量的值
* 复合类型: `<property name="dataSource.driverClassName" value|ref="..."/>`

## Annotation

* @Component|@Repository|@Service|@Controller
* @Autowired|@Inject|@Resource|@Value
* @Scope
* @Required|@Primary|@Qualifier
* @PostConstruct|@PreDestroy
* @ComponentScan
* JUnit4
  * @RunWith(SpringJunit4ClassRunner.class)
  * @ContextConfiguration

## `context:annotation-config`

在基于主机方式配置Spring时,Spring配置文件applicationContext.xml,你可能会见 `<context:annotation-config/>` 这样一条配置，它的作用是隐式的向Spring容器注册

* `AutowiredAnnotationBeanPostProcessor`
* `CommonAnnotationBeanPostProcessor`
* `PersistenceAnnotationBeanPostProcessor`
* `RequiredAnnotationBeanPostProcessor `

 这4个`BeanPostProcessor`。注册这4个bean处理器主要的作用是为了你的系统能够识别相应的注解。

1. 使用`@Autowired`注解，需声明`AutowiredAnnotationBeanPostProcessor`。
2. 使用`@PersistenceContext`注解，需声明`PersistenceAnnotationBeanPostProcessor`。
3. 使用`@Required`注解，需声明`RequiredAnnotationBeanPostProcessor`。
4. 使用`@Resource`、`@PostConstruct`、`@PreDestroy`等注解须声明`CommonAnnotationBeanPostProcessor`。

## `context:component-scan`

它的`base-package`属性指定了需要扫描的类包，类包及其递归子包中所有的类都会被处理。

> 使用 `<context：component-scan>` 隐式启用 `<context：annotation-config>` 的功能。使用 `<context：component-scan>` 时，通常不需要包含 `<context：annotation-config>` 元素。

# AOP

* Spring AOP代理机制
  * JDK动态代理: 针对实现了接口的类
  * CGLib动态代理: 针对没有实现接口的类，应用最底层的字节码增强技术生成当前类的子类对象
* 术语
  * Joinpoint(连接点): 被拦截的点，在spring中，这些点指的是方法，因为spring只支持方法类型的连接点（可以切入的点）
  * Pointcut(切入点): 对哪些Joinpoint进行拦截的定义。（已经被切入的点）
  * Advice(通知/增强): 拦截到Joinpoint之后所要做的事情就是通知。通知分为前置通知、后置通知、异常通知、最终通知、环绕通知（切面要完成的功能）
  * Introduction(引介): 一种特殊的通知，在不修改类代码的前提下，Introduction可以在运行期为类动态地添加一些方法或Field。
  * Aspect(切面): Pointcut和Advice|Introduction的结合
  * Target(目标对象): 代理的目标对象
  * Proxy(代理): 一个类被AOP织入增强后，就产生了一个结果代理类。
  * Weaving(织入): 将增强应用到目标对象来创建新的代理对象的过程。spring采用动态代理织入，而AspectJ采用编译期织入和类装载期织入
* aop:config, aop:pointcut, aop:aspect, aop:before|after|after-returning|after-throwing|around
* `<aop:aspectj-autoproxy/>`, @Aspect, @Pointcut, @Before, @After, @AfterReturning, @AfterThrowing, @Around
* pointcut
  * `public * *(..)`: 任何public方法
  * `* set*(..)`: 任何set方法
  * `* com.xyz.service.UserService.(..)`: com.xyz.service.UserService类的任何方法
  * `* com.xyz.service.*.(..)`: com.xyz.service包下任何方法
  * `* com.xyz.service..*.(..)`: com.xyz.service包及其子包下任何方法

# Data Access/Integration

## JdbcTemplate

* `<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate" p:dataSource-ref="dataSource"/>`

## Transaction

> #### [事务隔离](https://zh.wikipedia.org/wiki/%E4%BA%8B%E5%8B%99%E9%9A%94%E9%9B%A2)
>
> * 一个数据库事务通常包含了一个序列的对数据库的读/写操作。它的存在包含有以下两个目的：
>
>   1.  为数据库操作序列提供了一个从失败中恢复到正常状态的方法，同时提供了数据库即使在异常状态下仍能保持一致性的方法。
>   2.  当多个应用程序在并发访问数据库时，可以在这些应用程序之间提供一个隔离方法，以防止彼此的操作互相干扰。
>
> * 并非任意的对数据库的操作序列都是数据库事务。数据库事务拥有以下四个特性，习惯上被称之为**ACID特性**。
>
>   * **原子性（Atomicity）**：事务作为一个整体被执行，包含在其中的对数据库的操作要么全部被执行，要么都不执行。
>   * **一致性（Consistency）**：事务应确保数据库的状态从一个一致状态转变为另一个一致状态。*一致状态*的含义是数据库中的数据应满足完整性约束。
>   * **隔离性（Isolation）**：多个事务并发执行时，一个事务的执行不应影响其他事务的执行。
>   * **持久性（Durability）**：已被提交的事务对数据库的修改应该永久保存在数据库中。
>
> * **事务隔离**（英语：Transaction Isolation）定义了数据库系统中一个操作的结果在何时以何种方式对其他并发操作可见。隔离是事务*ACID*四大属性之一。
>
> * ANSI/ISO SQL定义的标准隔离级别如下：
>
>   * **可串行化（SERIALIZABLE）**
>
>   最高的隔离级别。
>
>   在基于锁机制并发控制的DBMS实现可串行化，要求在选定对象上的读锁和写锁保持直到事务结束后才能释放。在SELECT的查询中使用一个“WHERE”子句来描述一个范围时应该获得一个“范围锁”（range-locks）。这种机制可以避免“幻影读”（phantom reads）现象（详见下文）。
>
>   当采用不基于锁的[并发控制]时不用获取锁。但当系统探测到几个并发事务有“写冲突”的时候，只有其中一个是允许提交的。这种机制的详细描述见“快照隔离”
>
>   * **可重复读（REPEATABLE READS）**
>
>   在可重复读隔离级别中，基于锁机制并发控制的DBMS需要对选定对象的读锁（read locks）和写锁（write locks）一直保持到事务结束，但不要求“范围锁”，因此可能会发生“幻影读”。
>
>   * **提交读（READ COMMITTED）**
>
>   在提交读级别中，基于锁机制并发控制的DBMS需要对选定对象的写锁一直保持到事务结束，但是读锁在SELECT操作完成后马上释放（因此“不可重复读”现象可能会发生，见下面描述）。和前一种隔离级别一样，也不要求“范围锁”。
>
>   * **未提交读（READ UNCOMMITTED）**
>
>   未提交读是最低的隔离级别。允许“脏读”（dirty reads），事务可以看到其他事务“尚未提交”的修改。
>
>   通过比低一级的隔离级别要求更多的限制，高一级的级别提供更强的隔离性。标准允许事务运行在更强的事务隔离级别上。(如在可重复读隔离级别上执行提交读的事务是没有问题的)
>
> * ***读现象***
>
>   * **脏读**
>
>     当一个事务允许读取另外一个事务修改但未提交的数据时，就可能发生脏读。
>
>     脏读和不可重复读（non-repeatable reads）类似。事务2没有提交造成事务1的语句1两次执行得到不同的结果集。在未提交读隔离级别唯一禁止的是更新混乱，即早期的更新可能出现在后来更新之前的结果集中。
>
>   * **不可重复读**
>     在一次事务中，当一行数据获取两遍得到不同的结果表示发生了“不可重复读”.
>
>     在基于锁的并发控制中“不可重复读”现象发生在当执行SELECT 操作时没有获得读锁或者SELECT操作执行完后马上释放了读锁； 多版本并发控制中当没有要求一个提交冲突的事务回滚也会发生“不可重复读”现象。
>
>   * **幻影读**
>     在事务执行过程中，当两个完全相同的查询语句执行得到不同的结果集。这种现象称为“幻影读（phantom read）”
>
>     当事务没有获取范围锁的情况下执行SELECT ... WHERE操作可能会发生“幻影读”。
>
>     “幻影读”是不可重复读的一种特殊场景：当事务1两次执行SELECT ... WHERE检索一定范围内数据的操作中间，事务2在这个表中创建了(如INSERT)了一行新数据，这条新数据正好满足事务1的“WHERE”子句。
>
> * ***隔离级别vs读现象***
>
>   | 隔离级别 |   脏读   | 不可重复读 |  幻影读  |
>   | :------: | :------: | :--------: | :------: |
>   | 未提交读 | 可能发生 |  可能发生  | 可能发生 |
>   |  提交读  |    -     |  可能发生  | 可能发生 |
>   | 可重复读 |    -     |     -      | 可能发生 |
>   | 可序列化 |    -     |     -      |    -     |
>
>   可序列化（Serializable）隔离级别不等同于可串行化（Serializable）。可串行化调度是避免以上三种现象的必要条件，但不是充分条件。
>   “可能发生”表示这个隔离级别会发生对应的现象，“-”表示不会发生。
>
> * ***隔离级别vs锁持续时间***
>
>   在基于锁的并发控制中，隔离级别决定了锁的持有时间。**"C"**-表示锁会持续到事务提交。 **"S"** –表示锁持续到当前语句执行完毕。如果锁在语句执行完毕就释放则另外一个事务就可以在这个事务提交前修改锁定的数据，从而造成混乱。
>
>   | 隔离级别 | 写操作 | 读操作 | 范围操作 (...where...) |
>   | :------: | :----: | :----: | :--------------------: |
>   | 未提交读 |   S    |   S    |           S            |
>   |  提交读  |   C    |   S    |           S            |
>   | 可重复读 |   C    |   C    |           S            |
>   | 可序列化 |   C    |   C    |           C            |
>

```java
package org.springframework.transaction;

public interface TransactionDefinition {、
    /**
     * 默认的spring事务传播级别
     * 如果上下文中已经存在事务，那么就加入到事务中执行，如果当前上下文中不存在事务，则新建事务执行。
     */
    int PROPAGATION_REQUIRED = 0;
    /**
     * 如果上下文存在事务，则支持事务加入事务，如果没有事务，则使用非事务的方式执行。
     */
    int PROPAGATION_SUPPORTS = 1;
    /**
     * 要求上下文中必须要存在事务，否则就会抛出异常。
     */
    int PROPAGATION_MANDATORY = 2;
    /**
     * 每次都会新建一个事务，并且同时将上下文中的事务挂起，执行当前新建事务完成以后，上下文事务恢复再执行。
     */
    int PROPAGATION_REQUIRES_NEW = 3;
    /**
     * 不支持事务。上下文中存在事务，则挂起事务，执行当前逻辑，结束后恢复上下文的事务。
     */
    int PROPAGATION_NOT_SUPPORTED = 4;
    /**
     * 要求上下文中不能存在事务，一旦有事务，就抛出异常。
     */
    int PROPAGATION_NEVER = 5;
    /**
     * 如果上下文中存在事务，则嵌套事务执行，如果不存在事务，则新建事务。
     */
    int PROPAGATION_NESTED = 6;

    /**
     * 默认的隔离级别
     * 使用数据库默认的事务隔离级别。
     */
    int ISOLATION_DEFAULT = -1;
    /**
     * 读未提交
     */
    int ISOLATION_READ_UNCOMMITTED = 1;
    /**
     * 读已提交
     */
    int ISOLATION_READ_COMMITTED = 2;
    /**
     * 可重复读
     */
    int ISOLATION_REPEATABLE_READ = 4;
    /**
     * 串行化
     */
    int ISOLATION_SERIALIZABLE = 8;

    /**
     * 默认-1，不超时
     */
    int TIMEOUT_DEFAULT = -1;

    /**
     * 获取事务传播行为
     * @return
     */
    int getPropagationBehavior();
    
    /**
     * 获取事务隔离级别
     * @return
     */
    int getIsolationLevel();

    /**
     * 获取事务超时时间
     * @return
     */
    int getTimeout();

    /**
     * 是否只读事务
     * @return
     */
    boolean isReadOnly();

    String getName();
}
```

* ```xml
  <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
      <property name="dataSource" ref="dataSource"/>
  </bean>
  ```

* ```xml
  <tx:advice id="transactionInterceptor" transaction-manager="transactionManager">
      <tx:attributes>
          <tx:method name="transfer" propagation="REQUIRED"/>
      </tx:attributes>
  </tx:advice>
  ```

* ```xml
  <aop:config>
      <aop:pointcut id="transactionPointcut" expression="execution(* com.shieh.service..*.*(..))"/>
      <aop:advisor advice-ref="transactionInterceptor" pointcut-ref="transactionPointcut"/>
  </aop:config>
  ```

* 使用 `<tx:annotation-driven/>` 开启事务注解,使用 `@Transactional` 在业务层声明事务

# Web

## QuickStart

* ```xml
  <!-- web.xml -->
  <servlet>
      <servlet-name>dispatcherServlet</servlet-name>
      <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
      <init-param>
          <param-name>contextConfigLocation</param-name>
          <param-value>classpath:dispatcherServlet.xml</param-value>
      </init-param>
      <load-on-startup>1</load-on-startup>
  </servlet>
  
  <servlet-mapping>
      <servlet-name>dispatcherServlet</servlet-name>
      <url-pattern>/</url-pattern>
  </servlet-mapping>
  ```

* ```xml
  <!-- dispatcherServlet.xml -->
  <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
      <property name="prefix" value="/WEB-INF/jsp/"/>
      <property name="suffix" value=".jsp"/>
  </bean>
  ```

* ```xml
  <!-- pom.xml -->
  <!-- 编译插件 -->
  <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>3.7.0</version>
      <configuration>
          <source>1.8</source>
          <target>1.8</target>
          <encoding>UTF-8</encoding>
      </configuration>
  </plugin>
  <!-- Tomcat插件 -->
  <plugin>
      <groupId>org.apache.tomcat.maven</groupId>
      <artifactId>tomcat7-maven-plugin</artifactId>
      <version>2.2</version>
      <configuration>
          <port>8080</port>
          <path>/spring-mvc</path>
          <uriEncoding>UTF-8</uriEncoding>
          <warSourceDirectory>${basedir}/target/${project.artifactId}</warSourceDirectory>
      </configuration>
  </plugin>
  ```

## Annotation

* `@RequestMapping`

  * `@GetMapping`,`@PostMapping`,`@PutMapping`,`@DeleteMapping`,`@PatchMapping`
  * glob 模式和通配符映射请求,使用`@PathVariable`获取
    * `?` 匹配一个字符
    * `*` 匹配路径段中的零个或多个字符
    * `**` 匹配零个或多个路径段
  * `consumes`: 根据请求缩小请求映射范围`Content-Type`
    * 该`consumes`属性还支持否定表达式 - 例如，`!text/plain`表示除了以外的任何内容类型`text/plain`。
    * `MediaType`提供常用媒体类型的常量，例如 `APPLICATION_JSON_VALUE`和`APPLICATION_XML_VALUE`。
  * `produces`: 根据`Accept`请求标头和控制器方法生成的内容类型列表来缩小请求映射
    * 媒体类型可以指定字符集。支持否定表达式
  * `params`: 根据请求参数条件缩小请求映射
  * `headers`: 根据请求头条件缩小请求映射

* `@PathVariable`

  * 使用`{varName:regex}`进行正则匹配

* `@RequestParam`，`@RequestHeader`，`@RequestAttribute`

  * `@RequestParam` 用于获取从表单传递的参数
  * `@RequestAttribute` 用于获取请求转发时设置的属性

* `@SessionAttributes`,`@SessionAttribute`,`@CookieValue`

* POST乱码

  ```xml
  <!-- web.xml -->
  <filter>
      <filter-name>characterEncodingFilter</filter-name>
      <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
      <init-param>
          <param-name>encoding</param-name>
          <param-value>UTF-8</param-value>
      </init-param>
      <init-param>
          <param-name>forceEncoding</param-name>
          <param-value>true</param-value>
      </init-param>
  </filter>
  <filter-mapping>
      <filter-name>characterEncodingFilter</filter-name>
      <url-pattern>/*</url-pattern>
  </filter-mapping>
  ```

* GET乱码`<uriEncoding>UTF-8</uriEncoding>`

## RESTful

[**表现层状态转换**](https://zh.wikipedia.org/wiki/%E8%A1%A8%E7%8E%B0%E5%B1%82%E7%8A%B6%E6%80%81%E8%BD%AC%E6%8D%A2)（REST，英文：Representational State Transfer）是Roy Thomas Fielding博士于2000年在他的博士论文中提出来的一种*万维网软件架构*风格，目的是便于不同软件/程序在网络（例如互联网）中互相传递信息。



需要注意的是，REST是设计风格而不是标准。REST通常基于使用HTTP，URI，和XML以及HTML这些现有的广泛流行的协议和标准。

* 资源是由URI来指定。
* 对资源的操作包括获取、创建、修改和删除资源，这些操作正好对应HTTP协议提供的GET、POST、PUT和DELETE方法。
* 通过操作资源的表现形式来操作资源。
* 资源的表现形式则是XML或者HTML，取决于读者是机器还是人，是消费web服务的客户软件还是web浏览器。当然也可以是任何其他的格式。

浏览器 form 表单只支持 GET 与 POST 请求，而 DELETE、PUT 等 method 并不支持，spring3.0 添加了一个过滤器，可以将这些请求转换为标准的 http 方法，使得支持 GET、POST、PUT 与 DELETE 请求，该过滤器为`HiddenHttpMethodFilter`。

`HiddenHttpMethodFilter` 只对 POST 请求的表单进行过滤，在表单内部添加 `name="_method"` 进行类型声明。

```xml
<form action="..." method="POST">
	<input type="hidden" name="_method" value="DELETE"/>
	......
</form>
```

## 排除静态资源

* 使用`<mvc:default-servlet-handler/>`指定默认处理器处理静态资源

  * 单独使用会让所有访问都进行默认处理，导致`@RequestMapping`注解失效，需要配合`<mvc:annotation-driven/>`使用

* 使用`<mvc:resources/>`指定静态资源匹配规则

  ```xml
  <mvc:resources mapping="/resources/**"
      location="/public, classpath:/static/"
      cache-period="31556926" />
  ```

* 在`web.xml`进行配置默认servlet

  ```xml
  <servlet-mapping>
      <servlet-name>default</servlet-name>
      <url-pattern>*.css</url-pattern>
  </servlet-mapping>
  <servlet-mapping>
      <servlet-name>default</servlet-name>
      <url-pattern>*.gif</url-pattern>
  </servlet-mapping>
  <servlet-mapping>
      <servlet-name>default</servlet-name>
      <url-pattern>*.mp4</url-pattern>
  </servlet-mapping>
  <servlet-mapping>
      <servlet-name>default</servlet-name>
      <url-pattern>*.png</url-pattern>
  </servlet-mapping>
  <servlet-mapping>
      <servlet-name>default</servlet-name>
      <url-pattern>*.jpg</url-pattern>
  </servlet-mapping>
  <servlet-mapping>
      <servlet-name>default</servlet-name>
      <url-pattern>*.js</url-pattern>
  </servlet-mapping>
  <servlet-mapping>
      <servlet-name>default</servlet-name>
      <url-pattern>*.html</url-pattern>
  </servlet-mapping>
  ```

## `url-pattern`

* `url-pattern` 中 `/` 与 `/*` 的区别，参考[Difference between / and /* in servlet mapping url pattern](https://stackoverflow.com/questions/4140448/difference-between-and-in-servlet-mapping-url-pattern)
  * `/` 表示匹配所有路径，例如 `/login` ，它覆盖容器的默认Servlet的映射
  * `/*` 表示匹配所有的路径及资源，例如 `/login` ， `*.jsp`

## `mvc:annotation-driven`

* `<mvc:annotation-driven/>` 是一种简写形式，其声明会自动注册 `DefaultAnnotationHandlerMapping`和`AnnotationMethodHandlerAdapter` 两个Bean，是Spring MVC为`@Controllers`分发请求所必需的
  * 同时它还提供了数据绑定支持，`@NumberFormatannotation`支持，`@DateTimeFormat`支持，`@Valid`支持，读写XML的支持（JAXB），读写JSON的支持（Jackson）
  * 使用其子标签`<mvc:path-matching>`为Spring MVC提供路径匹配模式，默认情况下`/index`和`/index.xxx`都可以到`@RequestMapping("/index")`，使用`<mvc:path-matching suffix-pattern="false"/>`强制进行路径匹配

> `DefaultAnnotationHandlerMapping`和`AnnotationMethodHandlerAdapter` 在Spring 3.2版本中已声明弃用，在Spring 3.2 及以上版本中建议使用 `RequestMappingHandlerMapping` 与 `RequestMappingHandlerAdapter`

## 类型转换

* 默认情况下Spring MVC会使用格式化程序将请求数据转换为对象，Spring MVC内部已经实现了 `Number` 与 `Date` 类型的转换规则，需要使用时可以在字段上添加 `@NumberFormat` 和 `@DateTimeFormat` 注解

  * 同时它也支持自定义格式化程序和转换器，实现 `Converter` 接口，并在 `SpringConfig.xml` 中进行配置

    ```xml
    <mvc:annotation-driven conversion-service="conversionService"/>
    
    <bean id="conversionService"
          class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
        <property name="converters">
            <set>
                <bean class="org.example.MyConverter"/>
            </set>
        </property>
        <property name="formatters">
            <set>
                <bean class="org.example.MyFormatter"/>
                <bean class="org.example.MyAnnotationFormatterFactory"/>
            </set>
        </property>
        <property name="formatterRegistrars">
            <set>
                <bean class="org.example.MyFormatterRegistrar"/>
            </set>
        </property>
    </bean>
    ```

## JSON请求

* 使用`@RequestBody`传递请求参数
* 使用`@ResponseBody`返回处理结果

## 异常处理

* `@ExceptionHandler`

  * 结合`@ControllerAdvice`使用

  > 参考[@ExceptionHandler方法](https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html#mvc-ann-exceptionhandler)

## 拦截器

所有`HandlerMapping`实现都支持处理程序拦截器，这些拦截器在您要将特定功能应用于某些请求时很有用 - 例如，检查主体。拦截器必须`HandlerInterceptor`从 `org.springframework.web.servlet`包中实现三种方法，这些方法应该提供足够的灵活性来进行各种预处理和后处理：

* `preHandle(..)`：在执行实际处理程序之前
* `postHandle(..)`：处理程序执行后
* `afterCompletion(..)`：完成请求完成后

该`preHandle(..)`方法返回一个布尔值。您可以使用此方法来中断或继续执行链的处理。当此方法返回时`true`，处理程序执行链继续。当它返回false时，`DispatcherServlet` 假定拦截器本身已处理请求（并且，例如，呈现适当的视图）并且不继续执行执行链中的其他拦截器和实际处理程序。

见[拦截](https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html#mvc-config-interceptors)在MVC配置一节的如何配置拦截器的实例。您也可以通过在各个`HandlerMapping`实现上使用setter直接注册它们 。

请注意，`postHandle`对于在之前和之前 编写和提交响应的方法`@ResponseBody`和`ResponseEntity`方法不太有用。这意味着对响应进行任何更改都为时已晚，例如添加额外的标头。对于此类方案，您可以实现并将其声明为[Controller Advice](https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html#mvc-ann-controller-advice) bean或直接对其进行配置 。`HandlerAdapter``postHandle``ResponseBodyAdvice``RequestMappingHandlerAdapter`

## 文件上传

* ```xml
  <!-- pom.xml -->
  <dependency>
      <groupId>commons-fileupload</groupId>
      <artifactId>commons-fileupload</artifactId>
      <version>1.3.2</version>
  </dependency>
  ```

* ```xml
  <!-- SpringConfig.xml -->
  <!-- 必须配置id -->
  <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
      <!--默认 ISO-8859-1-->
      <property name="defaultEncoding" value="UTF-8"/>
      <property name="maxInMemorySize" value="10240"/>
      <property name="uploadTempDir" value="/upload/"/>
      <property name="maxUploadSize" value="-1"/>
  </bean>
  ```

* ```jsp
  <form action="${pageContext.request.contextPath}/fileupload" method="post" enctype="multipart/form-data">
      <input type="file" name="file">
      ...
  </form>
  ```

* ```java
  @PostMapping("/form")
  public String handleFormUpload(@RequestParam("name") String name,
                                 @RequestParam("file") MultipartFile file) {
  
      if (!file.isEmpty()) {
          byte[] bytes = file.getBytes();
          // store the bytes somewhere
          return "redirect:uploadSuccess";
      }
  
      return "redirect:uploadFailure";
  }
  ```

# SpringBoot

## 排除自动装配

```java
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})
```

