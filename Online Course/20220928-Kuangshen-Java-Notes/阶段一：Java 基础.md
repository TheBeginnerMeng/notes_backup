# 学习阶段

阶段一：Java 基础

- Java 基础
- GUI 编程
- 网络编程
- 多线程
- 注解和反射
- JavaSE
- JVM
- JUC 并发编程

阶段二：前端基础

- HTML
- CSS
- JS

阶段三：MySQL

阶段四：JavaWeb

- Maven、Tomcat、HTTP通信、Servelet、JSP

阶段五：SSM框架

- MyBatis
- Spring
- Spring MVC
- SSM整合

阶段六：微服务

- Vue
- SpringBoot
- Swagger
- SpringCloud
- 分析开源项目

阶段七：运维

- Linux
- Git

阶段八：进阶

- MyBatisPlus
- Redis
- ES
- 实例：仿京东搜索

# 阶段八：MyBatisPlus

> MyBatisPlus 概述

需要提前掌握的基础知识：MyBatis、Spring、SpringMVC

MyBatisPlus出现的原因：“为偷懒而生”，进一步简化了 CRUD 操作

官网地址：[MyBatis-Plus (baomidou.com)](https://baomidou.com/)

> 特性

- **无侵入**：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑
- **损耗小**：启动即会自动注入基本 CRUD，性能基本无损耗，直接面向对象操作
- **强大的 CRUD 操作**：内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求
- **支持 Lambda 形式调用**：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错
- **支持主键自动生成**：支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题
- **支持 ActiveRecord 模式**：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作
- **支持自定义全局通用操作**：支持全局通用方法注入（ Write once, use anywhere ）
- **内置代码生成器**：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用
- **内置分页插件**：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询
- **分页插件支持多种数据库**：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer 等多种数据库
- **内置性能分析插件**：可输出 SQL 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
- **内置全局拦截插件**：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作

> Java项目命名方式

命名包名目录的方式 ：com.公司名的简写.项目的名字.业务模块名

所有的学不会都是给懒找的借口！！！

# 快速入门

地址：[快速开始 | MyBatis-Plus (baomidou.com)](https://baomidou.com/pages/226c21/#初始化工程)

使用第三方组件的一般方式：

1、导入对应的依赖

2、研究依赖如何配置

3、代码如何编写

4、提高扩展技术能力

> 步骤

1、创建数据库`mybatis_plus`

2、创建 user 表

```mysql
DROP TABLE IF EXISTS user;

CREATE TABLE user
(
    id BIGINT(20) NOT NULL COMMENT '主键ID',
    name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
    age INT(11) NULL DEFAULT NULL COMMENT '年龄',
    email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
    PRIMARY KEY (id)
);
-- 真实开发中，还有一些必需字段，如version（乐观锁）、deleted（逻辑删除）、gmt_create、gmt_modified

DELETE FROM user;

INSERT INTO user (id, name, age, email) VALUES
(1, 'Jone', 18, 'test1@baomidou.com'),
(2, 'Jack', 20, 'test2@baomidou.com'),
(3, 'Tom', 28, 'test3@baomidou.com'),
(4, 'Sandy', 21, 'test4@baomidou.com'),
(5, 'Billie', 24, 'test5@baomidou.com');
```

3、编写项目，先初始化一个 SpringBoot 项目（可以使用 [Spring Initializer (opens new window)](https://start.spring.io/)快速初始化一个 Spring Boot 工程）

4、添加并导入依赖

```xml
<dependency>
      <!-- 数据库驱动 -->
         <groupId>mysql</groupId>
         <artifactId>mysql-connector-java</artifactId>
         <version>8.0.27</version>
      </dependency>
	 <!-- lombok 插件 -->
      <dependency>
         <groupId>org.projectlombok</groupId>
         <artifactId>lombok</artifactId>
      </dependency>
      <!-- mybatis-plus 驱动 -->
	  <!-- mybatis-plus 其实是自己开发的，并非官方的！ -->
      <dependency>
         <groupId>com.baomidou</groupId>
         <artifactId>mybatis-plus-boot-starter</artifactId>
         <version>3.0.5</version>
      </dependency>
```

说明：我们使用 mybatis-plus 可以节省大量的代码，尽量不要同时导入 mybatis 和 mybatis-plus ！版本的差异！

5、连接数据库，这一步和 mybatis 相同

```properties
# mysql 5 驱动不同 com.mysql.jdbc.Driver

# mysql 8 驱动不同 com.mysql.cj.jdbc.Driver、需要增加时区的配置
spring.datasource.username=root
spring.datasource.password=123mysql
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis_plus?useSSL=false&useUnicode=true&characterEncoding=utf-8&serverTimezone=GMT%2B8
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```

==6、传统方式 pojo-dao（连接 mybatis，配置 mapper.xml 文件）– service – controller==

6、使用了 mybatis-plus 之后

- pojo
- mapper 接口
- 使用