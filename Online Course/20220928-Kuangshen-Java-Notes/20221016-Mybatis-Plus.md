# 阶段八：MyBatis-Plus

> MyBatisPlus 概述

需要提前掌握的基础知识：**MyBatis、Spring、SpringMVC**

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

```java
package com.loveling.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {

    private Long id;
    private String name;
    private Integer age;
    private String email;

}
```

- mapper 接口

```java
package com.loveling.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.loveling.pojo.User;
import org.springframework.stereotype.Repository;

// 在对应的 Mapper 上面继承基本的类 BaseMapper
@Repository // 代表持久层
public interface UserMapper extends BaseMapper<User> {

    // 所有的 CRUD 已经完成
    // 不需要像以前那样配置一大堆文件
}

```

- 注意点：需要在主启动类上去扫描我们的 mapper 包下的所有接口==@MapperScan("com.loveling.mapper")==
- 测试类中测试

```java
package com.loveling;

import com.loveling.mapper.UserMapper;
import com.loveling.pojo.User;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.List;

@SpringBootTest
class MybatisPlusApplicationTests {

	// 继承了 BaseMapper，所有的方法都来自所继承的父类
	// 当然，我们也可以编写自己的方法
	@Autowired
	private UserMapper userMapper;

	@Test
	void contextLoads() {
		// 参数是一个 Wrapper，条件构造器，这里我们先不用，置为 null
		// 查询全部用户
		List<User> users = userMapper.selectList(null);
		users.forEach(System.out::println);
	}

}
```

- 结果

![image-20221008230543582](C:\Users\timla\AppData\Roaming\Typora\typora-user-images\image-20221008230543582.png)

> 思考

1、SQL谁帮我们写的？MyBatis-Plus都写好了。

2、方法哪里来的？MyBatis-Plus都写好了。

# 配置日志

现在，所有的 SQL 都是不可见的，我们希望知道它是如何执行的，所以我们必须查看日志！

```properties
# 日志配置
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```

![image-20221016213038212](C:\Users\timla\AppData\Roaming\Typora\typora-user-images\image-20221016213038212.png)

配置完毕日志之后，以后在调试代码的时候就可以注意运行的 SQL 日志，慢慢地，就会发现 MyBatis-Plus 的好处！

# CRUD扩展

## 插入操作

> Insert 插入

```java
// 测试插入
@Test
public void testInsert() {
    User user = new User();
    user.setName("Molly");
    user.setAge(18);
    user.setEmail("MollyLoveTim@163.com");
    int result = userMapper.insert(user);

    System.out.println("result: "+ result); // 受影响的行数
    System.out.println(user); // 可以发现, id 会自动回填
}
```

![](C:\Users\timla\AppData\Roaming\Typora\typora-user-images\image-20221016215151113.png)

> 数据库插入 id 的默认值为：全局的唯一 id

## 主键生成策略

> 默认 ID_WORKER 全局唯一 id

分布式系统唯一 id 生成：[分布式系统唯一ID生成方案汇总 - nick hao - 博客园 (cnblogs.com)](https://www.cnblogs.com/haoxinyue/p/5208136.html)

雪花算法：snowflake 是 Twitter 开源的分布式 ID 生成算法，结果是一个 long 型的 ID。其核心思想是：使用41 bit 作为毫秒数，10 bit 作为机器的 ID（5个 bit 是数据中心，5个 bit 的机器 ID），12 bit 作为毫秒内的流水号（意味着每个节点在每毫秒可以产生 4096 个 ID），最后还有一个符号位，永远是0。具体实现的代码可以参看https://github.com/twitter/snowflake。雪花算法支持的 TPS 可以达到419万左右（2^22*1000）。

> 主键自增

我们需要配置主键自增：

1、实体类字段上`@TableId(type = IdType.AUTO)`
2、数据库的主键字段一定要是自增！

3、再次测试插入即可！

> 其余的源码解释

```java
public enum IdType {
    AUTO(0), // 数据库自增id
    NONE(1), // 未设置主键
    INPUT(2), // 手动输入 
    ID_WORKER(3), // 默认的全局唯一 id
    UUID(4), // 全局唯一id UUID
    ID_WORKER_STR(5); // ID_WORKER 字符串表示法
}
```

## 更新操作

```java
// 测试更新
@Test
public void testUpdate() {

    User user = new User();
    // 通过条件自动拼接动态 SQL
    user.setId(5L);
    user.setName("MollyIsSoPretty");
    //		注意, updateById, 但是参数是一个对象
    int i = userMapper.updateById(user);
    System.out.println(i);
}
```

![image-20221017150117870](https://raw.githubusercontent.com/TheBeginnerMeng/notes_backup/main/imgs/image-20221017150117870.png)

所有的 SQL，MyBatis-Plus 都可以帮你动态配置！

## 自动填充

创建时间、修改时间，一般是根据操作自动化生成的，不希望手动去更新这些字段！

阿里巴巴开发手册：所有的数据库表：gmt_create、gmt_modified 几乎所有的表都要配置上，而且需要自动化！

> 方式一：数据库级别

1、在表中新增字段 create_time、update_time

2、再次测试插入方法，需要先同步实体类！

```java
private Date createTime;
private Date updateTime;
```

> 方式二：代码级别

1、删除数据库的默认值，更新操作！

2、实体类字段属性上需要增加注解

```java
// 字段添加填充内容
@TableField(fill = FieldFill.INSERT)
private Date createTime;

@TableField(fill = FieldFill.INSERT_UPDATE)
private Date updateTime;
```

3、编写处理器来处理这个注解即可！

```java
package com.loveling.mybatis_plus.handler;

import com.baomidou.mybatisplus.core.handlers.MetaObjectHandler;
import lombok.extern.slf4j.Slf4j;
import org.apache.ibatis.reflection.MetaObject;
import org.springframework.stereotype.Component;

import java.util.Date;

@Component
@Slf4j // 一定不要忘记把处理器加入到 IOC 容器中！
public class MyMetaObjectHandler implements MetaObjectHandler {
    // 插入时的填充策略
    @Override
    public void insertFill(MetaObject metaObject) {
        log.info("start insert fill...");
//        setFieldValByName(String fieldName, Object fieldVal, MetaObject metaObject)
        this.setFieldValByName("createTime", new Date(), metaObject);
        this.setFieldValByName("updateTime", new Date(), metaObject);
    }

    // 更新时的填充策略
    @Override
    public void updateFill(MetaObject metaObject) {
        log.info("start update fill...");
        this.setFieldValByName("updateTime", new Date(), metaObject);
    }
}
```

4、测试插入

5、测试更新，观察时间字段是否按照策略变更！

## 乐观锁

在面试过程中，经常会被问到乐观锁，悲观锁！

> 乐观锁：顾名思义，十分乐观，认为不会出现问题，不论干什么都不去上锁！如果出现了问题，再次更新值测试
>
> 悲观锁：顾名思义，十分悲观，它总是认为会出现问题，不论干什么都去上锁！再去操作！

我们这里主要讲解乐观锁机制！

乐观锁实现方式：

- 取出记录时，获取当前 version
- 更新时，带上这个 version
- 执行更新时，set version = newVersion where version = oldVersion
- 如果 version 不对，就更新失败

```sql
乐观锁：先查询，获得版本号 version = 1
-- A 线程
update user set name = "Tim", version = version + 1 where id = 2 and version = 1;

-- B 线程抢先完成，这个时候 version 字段就变成了2，会导致 A 修改失败！
update user set name = "Tim", version = version + 1 where id = 2 and version = 1;
```

> 测试 MyBatis-Plus 乐观锁插件

1、给数据库表增加 version 字段

2、实体类增加对应的字段

```java
@Version // 乐观锁注解
private Integer version;
```

3、注册组件

```java
package com.loveling.mybatis_plus.config;

import com.baomidou.mybatisplus.extension.plugins.OptimisticLockerInterceptor;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.transaction.annotation.EnableTransactionManagement;

@MapperScan("com.loveling.mybatis_plus.mapper")
@EnableTransactionManagement
@Configuration
public class MyBatisPlusConfig {

    // 注册乐观锁插件
    @Bean
    public OptimisticLockerInterceptor optimisticLockerInterceptor() {
        return new OptimisticLockerInterceptor();
    }
}
```

4、测试

```java
// 测试乐观锁成功！
@Test
public void testOptimisticLocker() {
    // 1、查询用户信息
    User user = userMapper.selectById(1L);
    // 2、修改用户信息
    user.setName("timlau111");
    user.setAge(18);
    // 3、执行更新操作
    userMapper.updateById(user);
}

// 测试乐观锁失败，多线程下！
@Test
public void testOptimisticLocker2() {
    // 线程1
    // 1、查询用户信息
    User user = userMapper.selectById(1L);
    // 2、修改用户信息
    user.setName("timlau111");
    user.setAge(18);

    // 模拟另一个线程执行了插队操作
    User user2 = userMapper.selectById(1L);
    // 2、修改用户信息
    user2.setName("timlau222");
    user2.setAge(18);

    // 3、执行更新操作
    userMapper.updateById(user2);
    // 自旋锁进行操作
    userMapper.updateById(user); // 如果没有乐观锁，就会覆盖插队线程的值
}
```

