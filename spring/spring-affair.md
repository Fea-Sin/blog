# Spring 的事物管理

Spring 框架所支持的事务管理，包括编程式事务管理和声明式事务管理

Spring 的事务管理简化了传统的数据库事务管理流程，提高了开发效率

数据库编程是互联网编程的基础，Spring 框架为开发者提供了 JDBC 模版模式，即 jdbc Template 它简化
了许多代码，但在实际应用中 jdbc Template 并不常用。在工作中更多的时候是使用 Hibernate 框架和 MyBaits
框架进行数据库编程

要了解 JdbcTemplate 类的常用方法，该类的常用方法是 update 和 query

```
public int update(String sql, Object args[])
```

返回的是更新的行数

实例

```
String insertSql = "insert into user values(null,?,?)";
Object param1[] = {"ceshi", "男"};
jdbcTemplate.update(sql, param1);
```

List 方法可以对数据进行查询操作，rowMapper 将结果集映射到用户自定义的类中

```
public List<T>query(String sql, RowMapper<T> rowMapper, Object args[])
```

示例

```
string selectSql = "select * from user";
RowMapper<MyUser> rowMapper = new BeanPropertyRowMapper<MyUser.class>;
List<MyUser> list = jdbcTemplate.query(sql, rowMapper, null)
```

实体类 MyUser

```java
package com.ch5;

public class MyUser {
  private Integer uid;
  private String uname;
  private String usex;

  public String toString() {
    return "myUser [uid=" + uid + ", uname=" + uname + ", usex=" + usex + "]";
  }
}
```

## 编程式事务管理

在代码中显示调用`beginTransation`、`commit`、`rollback`等与事务处理相关的方法，这就是编程式
事务管理

基于底层 API 的编程式事务管理是根据`PlatformTranscationManager`、`TransacationDefinition`、`TranscationStatus`
几个核心接口

事务处理的代码散落在业务逻辑代码中，破坏了原有代码的条理性，并且每一个业务方法都包含了类似的启动事务、提交以及回滚
事务的样板代码

TranscationTemplate 的 execute 方法有一个 TranscationCallback 接口类型的参数，该参数中定义了一个 doInTransaction
方法，通常以匿名内部类的方式实现 TranscationCallback 接口，并在其 doInTransaction 方法中书写业务逻辑代码。
在这里可以使用默认的事务提交和回滚规则

## 声明式事务管理

Spring 的声明式事务是通过 AOP 技术实现的事务管理，其本质是对方法前后进行拦截，然后在目标方法开始之前创建或者
加入一个事务，在执行完目标方法之后根据执行情况提交或者回滚事务

声明式事务管理最大的优点是不需要通过编程的方式管理事务，因而不需要在业务逻辑代码中掺杂事务处理代码，只需相关的事务
规则声明便可以将事务规则应用到业务逻辑中。通常情况下，在开发中使用声明式事务处理不仅因为其简单，更主要是因为这样
使得纯业务代码不被污染，极大地方便了后期的代码维护

与编程式事务管理相比，声明式事务管理唯一不足的地方是最细粒度只能作用到方法级别，无法做到像编程式事务管理那样可以
作用到代码块级别

Spring 的声明式事务管理可以通过两种方式来实现，一是基于 XML 的方式，二是基于`@Transactional`注解的方式

`@Transactional`注解可以作用于接口、接口方法、类以及类的方法上。当作用于类上时，该类的所有 public 方法都将
具有该类型的事务属性，同时也可以在方法级别使用该注解来覆盖类级别的定义

Spring 小组建议不要在接口或者接口方法上使用该注解，因为它只有在使用基于接口的代理时才会生效

如果不想对某个异常进行事务处理

```
不对RuntimeException回滚生效
@Transactional(rollbackFor=RuntimeException.class)
```
