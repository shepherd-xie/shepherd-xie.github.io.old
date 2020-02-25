---
title: MyBatis
date: 2018-11-09 16:33:00
tags: 
categories: 
top:
---

## [参考](https://github.com/abel533/Mapper)[MyBatis-3-User-Guide-Simplified-Chinese.pdf]

<!-- more -->

## 1. 入门

* 什么是MyBatis
* MyBatis基本配置：config, mapper

## 2. 列名与参数不一致

* SQL方式：别名
* MyBatis方式：ResultMap
  * 强制标签顺序

## 3. #和$的区别

* `#` 预编译 自动转义 防止SQL注入 eg. name -> 'name' 用于赋值 使用参数名取值 eg. #{name}
* `$` 拼接字符串 用于传递表名，ORDER BY，GROUP BY 使用value或属性取值 eg. ${value}

## 4. 多参传递

* 索引 eg. #{0}
* 注解 eg. @Param(value = "fieldName")
* Map

## 5. 获取数据库生成ID

* Mapper配置 `<insert useGeneratedKeys="true" keyProperty="id"/>`
* Config配置 `<setting name="useGeneratedKeys" value="true"/>`
* 数据库不支持自动生成类型 `<selectKey keyProperty="id" resultType="int" order="AFTER"/>`

## 6. 动态SQL

* `<if test="value != null and value != ''"/>`
* choose when otherwise
* where set trim
* sql

## 7. [MyBatis Generator](http://www.mybatis.org/generator/)



```xml
<!-- generatorConfig.xml -->
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <context id="testTables" targetRuntime="MyBatis3">
        <!--序列化model-->
        <!--<plugin type="org.mybatis.generator.plugins.SerializablePlugin"/>-->

        <commentGenerator>
            <!--是否去除自动生成的注释 true：是 false：否-->
            <property name="suppressAllComments" value="true"/>
        </commentGenerator>

        <!--数据库连接信息-->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/world"
                        userId="root"
                        password="toor"/>
        <!--<jdbcConnection driverClass="oracle.jdbc.OracleDriver" -->
        <!--connectionURL="jdbc:oracle:thin:@localhost:1521:DB_NAME" -->
        <!--userId="scott" -->
        <!--password="tiger"/>-->

        <!-- 默认false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer，为 true时把JDBC DECIMAL 和 
			NUMERIC 类型解析为java.math.BigDecimal -->
        <javaTypeResolver>
            <property name="forceBigDecimals" value="false"/>
        </javaTypeResolver>

        <!-- targetProject:生成PO类的位置 -->
        <javaModelGenerator targetPackage="name.mybatis.pojo"
                            targetProject=".\src">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false"/>
            <!-- 从数据库返回的值被清理前后的空格 -->
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>
        <!-- targetProject:mapper映射文件生成的位置 -->
        <sqlMapGenerator targetPackage="name.mybatis.pojo"
                         targetProject=".\src">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false"/>
        </sqlMapGenerator>
        <!-- targetPackage：mapper接口生成的位置 -->
        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="name.mybatis.mapper"
                             targetProject=".\src">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false"/>
        </javaClientGenerator>
        <!-- 指定数据库表 -->
        <table tableName="city" domainObjectName="City"/>
        <table tableName="country" domainObjectName="Country"/>
        <table tableName="countrylanguage" domainObjectName="CountryLanguage"/>
        <!--<table schema="DB2ADMIN" tableName="ALLTYPES" domainObjectName="Customer">-->
        <!--<property name="useActualColumnNames" value="true"/>-->
        <!--<generatedKey column="ID" sqlStatement="DB2" identity="true"/>-->
        <!--<columnOverride column="DATE_FIELD" property="startDate"/>-->
        <!--<ignoreColumn column="FRED"/>-->
        <!--<columnOverride column="LONG_VARCHAR_FIELD" jdbcType="VARCHAR"/>-->
        <!--</table>-->

        <!-- 有些表的字段需要指定java类型
         <table schema="" tableName="">
            <columnOverride column="" javaType="" />
        </table> -->
    </context>
</generatorConfiguration>
```

```xml
<!-- pom.xml -->
<plugin>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator-maven-plugin</artifactId>
    <version>1.3.5</version>
    <configuration>
        <configurationFile>${basedir}/src/main/resources/generatorConfig.xml</configurationFile>
        <overwrite>false</overwrite>
        <verbose>true</verbose>
    </configuration>
    <dependencies>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>${mysql.version}</version>
        </dependency>
    </dependencies>
</plugin>
```

## 8. 缓存

* 默认开启局部session缓存
* 映射语句文件中的所有insert，update和delete语句会刷新缓存
* 开启二级缓存（表级缓存）在mapper设置`<cache/>`实体必须实现Serializable

## 9. 嵌套查询

* 多级单表查询
  * `<association select=""/>`
  * `<collection select=""/>`
* 关联查询

## 10. 延迟加载

* 参考MyBatis配置`<settings/>`

## 11. [TK MyBatis插件](https://github.com/abel533/Mapper)

* ```xml
  <dependency>
      <groupId>tk.mybatis</groupId>
      <artifactId>mapper</artifactId>
      <version>4.0.4</version>
  </dependency>
  ```

* ```xml
  <plugin>
      <groupId>org.mybatis.generator</groupId>
      <artifactId>mybatis-generator-maven-plugin</artifactId>
      <version>${mybatis-generator-core.version}</version>
      <configuration>
          <configurationFile>${basedir}/src/main/resources/generator/generatorConfig.xml</configurationFile>
          <overwrite>false</overwrite>
          <verbose>true</verbose>
      </configuration>
      <dependencies>
          <dependency>
              <groupId>mysql</groupId>
              <artifactId>mysql-connector-java</artifactId>
              <version>${mysql-connector-java.version}</version>
          </dependency>
          <dependency>
              <groupId>tk.mybatis</groupId>
              <artifactId>mapper</artifactId>
              <version>${mapper.version}</version>
          </dependency>
      </dependencies>
  </plugin>
  ```

* ```xml
  <!--使用 Configuration 方式进行配置-->
  <bean id="mybatisConfig" class="tk.mybatis.mapper.session.Configuration">
      <!-- 配置通用 Mapper，有三种属性注入方式 -->
      <property name="mapperProperties">
          <value>
              notEmpty=true
          </value>
      </property>
  </bean>
  
  <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
      <property name="dataSource" ref="dataSource"/>
      <property name="configuration" ref="mybatisConfig"/>
  </bean>
  
  <!-- 不需要考虑下面这个，注意这里是 org 的 -->
  <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
      <property name="basePackage" value="tk.mybatis.mapper.configuration"/>
      <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
  </bean>
  ```

  最小侵入式配置

* ```java
  public interface MyMapper<T> extends Mapper<T>, MySqlMapper<T> {
      //TODO
      //FIXME 特别注意，该接口不能被扫描到，否则会出错
  }
  ```

* ```java
  package top.shieh.my.shop.commons.util;
  
  import org.mybatis.generator.api.IntrospectedColumn;
  import org.mybatis.generator.api.IntrospectedTable;
  import org.mybatis.generator.api.PluginAdapter;
  import org.mybatis.generator.api.dom.java.FullyQualifiedJavaType;
  import org.mybatis.generator.api.dom.java.Method;
  import org.mybatis.generator.api.dom.java.TopLevelClass;
  
  import java.util.List;
  
  public class LombokPlugin extends PluginAdapter {
      private FullyQualifiedJavaType dataAnnotation = new FullyQualifiedJavaType("lombok.Data");
  
      @Override
      public boolean validate(List<String> warnings) {
          return true;
      }
  
      @Override
      public boolean modelBaseRecordClassGenerated(TopLevelClass topLevelClass, IntrospectedTable introspectedTable) {
          this.addDataAnnotation(topLevelClass);
          return true;
      }
  
      @Override
      public boolean modelPrimaryKeyClassGenerated(TopLevelClass topLevelClass, IntrospectedTable introspectedTable) {
          this.addDataAnnotation(topLevelClass);
          return true;
      }
  
      @Override
      public boolean modelRecordWithBLOBsClassGenerated(TopLevelClass topLevelClass, IntrospectedTable introspectedTable) {
          this.addDataAnnotation(topLevelClass);
          return true;
      }
  
      @Override
      public boolean modelGetterMethodGenerated(Method method, TopLevelClass topLevelClass, IntrospectedColumn introspectedColumn, IntrospectedTable introspectedTable, ModelClassType modelClassType) {
          return false;
      }
  
      @Override
      public boolean modelSetterMethodGenerated(Method method, TopLevelClass topLevelClass, IntrospectedColumn introspectedColumn, IntrospectedTable introspectedTable, ModelClassType modelClassType) {
          return false;
      }
  
      private void addDataAnnotation(TopLevelClass topLevelClass) {
          topLevelClass.addImportedType(this.dataAnnotation);
          topLevelClass.addAnnotation("@Data");
      }
  }
  ```

* 将 `org.mybatis.spring.mapper.MapperScannerConfigurer` 改为 `tk.mybatis.spring.mapper.MapperScannerConfigurer`

