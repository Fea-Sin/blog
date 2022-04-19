# Spring Boot 整合 MyBatis

MyBatis 是一个半自动化的 ORM 框架，所谓半自动化是指 MyBatis 只支持数据库查出的数据映射到 POJO 实体类上，
而实体到数据库的映射则需要自己编写 SQL 语句，相较于 Hibernate 这种完全自动化的框架，MyBatis 更加灵活，我们可以
根据自身的需求编写 sql 语句来实现复杂的数据库操作

MyBatis 也开发了一套基于 Spring Boot 模式的 starter: mybatis-spring-boot-starter

## 引入依赖

```xml
<dependency>
  <groupId>org.mybatis.spring.boot<groupId>
  <artifactId>mybatis-spring-boot-starter</artifactId>
  <version>2.2.0</version>
</dependency>
```

## 配置 MyBatis

在 application.properties 中对 MyBatis 进行配置

```yml
mybatis:
  mapper-locations: classpath:mybatis/mapper/*.xml
  # 扫描实体类的位置，此处指明扫描实体类的包
  type-aliases-package: net.biancheng.www.bean
  configuration:
    # 默认开启驼峰命名法
    map-underscore-to-camel-case: true
```

> 使用 MyBatis 时，必须配置数据源信息，例如数据库 URL、数据库用户型、数据库密码和数据库驱动等

## 创建实体类

| id  | user_id | user_name | password | email      |
| --- | ------- | --------- | -------- | ---------- |
| 1   | 001     | admin     | admin    | 124@qq.com |

根据数据库 user 表，创建相应的实体类

```java

public class User {
  private Integer id;
  private String userId;
  private String userName;
  private String password;
  private String email;

  public Integer getId() {
    return id;
  }
  public void setId(Integer id) {
    this.id = id;
  }

  public String getUserName() {
    return userName;
  }
  public void setUserName(String userName) {
    this.userName = userName  == null ? null : userName.trim();
  }
}
```

## 创建 Mapper 接口

在 net.biancheng.www.mapper 包中创建一个 UserMapper 接口，并在改类上使用`@Mapper`注解

```java
package net.biancheng.www.mapper;

import net.biancheng.www.bean.User;
import org.apache.ibatis.annotations.Mapper;

@Mapper
public interface UserMapper {
  // 通过用户名密码查询用户数据
  User getByUserNameAndPassword(User user);
}
```

## 创建 Mapper 映射文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="net.biancheng.www.mapper.UserMapper">
  <resultMap id="BaseResultMap" type="User">
    <id column="id" jdbcType="INTEGER" property="id" />
    <result column="user_id" jdbcType="VARCHAR" property="userId" />
    <result column="user_name" jdbcType="VARCHAR" property="userName" />
    <result column="password" jdbcType="VARCHAR" property="password" />
    <result column="email" jdbcType="VARCHAR" property="email" />
  </resultMap>
  <sql id="Base_Column_List">
    id, user_id, user_name, password, email
  </sql>
  <!-- 根据用户名密码查询用户 -->
  <select id="getByUserNameAndPassword" resultType="User">
    select *
    from user
    where user_name = #{userName, jdbcType=VARCHAR}
    and password = #{password, jdbcType=VARCHAR}
  </select>
</mapper>
```

使用 Mapper 进行开发时， 需要遵循以下规则

- mapper 映射文件中 namespace 必须与对应的 mapper 接口的完全限定名一致

- mapper 映射文件中 statement 的 id 必须与 mapper 接口中的方法的方法名一致

- mapper 映射文件中 statement 的 resultType 指定的类型必须与 mapper 接口中的方法的返回值类型一致

## 示例

在 net.biancheng.www.service 包中创建一个名为 UserService 的接口

```java
package net.biancheng.www.service;

import net.biancheng.www.bean.User;

public interface UserService {
  public User getByUserNameAndPassword(User user);
}
```

在 net.biancheng.www.service.impl 包中，创建 UserService 接口的实现类，并使用@Service 注解将其以
组件的形式添加到容器中

```java
package net.biancheng.www.service.impl;

import net.biancheng.www.bean.User;
import net.biancheng.www.mapper.UserMapper;
import net.biancheng.www.service.UserService;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;


@Service("userService")
public class UserServiceImpl implements UserService {
  @Autowired
  UserMapper userMapper;

  @Override
  public User getByUserNameAndPassword(User user) {
    user loginUser = userMapper.getByUserNameAndPassword(user);

    return loginUser;
  }
}
```

修改 LoginController 中的 doLogin()方法

```java
package net.biancheng.www.controller;

import lombok.extern.slf4j.Slf4j;

import net.biancheng.www.bean.User;
import net.biancheng.www.Service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;


import javax.servlet.http.HttpSession;
import java.util.Map;

@Slf4j
@Controller
public class LoginController {
  @Autowired
  UserService userService

  @RequestMapping("/user/login")
  public String doLogin(User user, Map<String, Object> map, HttpSession session) {
    // 从数据库中查询用户信息
    User loginUser = userService.getByUserNameAndPassword(user);
    if (loginUser != null) {
      session.setAttribute("loginUser", loginUser);
      log.info("登录成功，用户名" + loginUser.getUserName())
      // 防止重复提交使用重定向
      return "redirect:/main.html"
    } else {
      map.put("msg", "用户名或密码错误");
      log.error("登录失败");
      return "login";
    }
  }
}
```

通过上面学习，我们知道 mapper 映射文件其实是一个 XML 配置文件，它存在 XML 配置文件的通病，编写繁琐，容易出错

MyBatis 针对实际业务中使用最多的增删改查操作，分别提供了注解来替换 mapper 映射文件

@Select, @Insert, @Update, @Delete

```java
package net.biancheng.www.mapper;

import net.biancheng.www.bean.User;
import org.apache.ibatis.annotation.*;

import java.util.List;

@Mapper
public interface UserMapper {
  @Select("select * from user where user_name = #{userName, jdbcType=VARCHAR} and password = #{password,jdbc=VARCHAR}")
  List<User> getByUserNameAndPassword(User user);


  @Delete("delete from user where id = #{id,jdbc=INTEGER}")
  int deleteByPrimaryKey(Integer id);

      @Insert("insert into user ( user_id, user_name, password, email)" +
            "values ( #{userId,jdbcType=VARCHAR}, #{userName,jdbcType=VARCHAR}, #{password,jdbcType=VARCHAR}, #{email,jdbcType=VARCHAR})")
    int insert(User record);

        @Update(" update user" +
            "    set user_id = #{userId,jdbcType=VARCHAR},\n" +
            "      user_name = #{userName,jdbcType=VARCHAR},\n" +
            "      password = #{password,jdbcType=VARCHAR},\n" +
            "      email = #{email,jdbcType=VARCHAR}\n" +
            "    where id = #{id,jdbcType=INTEGER}")
    int updateByPrimaryKey(User record);

}
```

mapper 接口中的任何一个方法，都只能使用一种配置方式，注解和 mapper 映射文件二选一，不同的方法之间可以混合使用

我们可以根据 SQL 的复杂程度，选择不同的方式来提高效率

> 如果涉及的 sql 较为复杂时，则使用 XML 映射的方式更好一些
