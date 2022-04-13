# MyBatis

MyBatis 是主流的 Java 持久层框架之一，它与 Hibernate 一样，也是一种 ORM(Object/Relational Mapping)对象关系映射
框架，因其性能优异，且具有高度的灵活性、可优化性、易于维护以及简单易学等特点，受到广大互联网企业和编程爱好者的青睐

## MyBatis 简介

MyBatis 是一个基于 Java 的持久层框架，MyBatis 提供的持久层框架包括`SQL Maps`和`Data Access Object(DAO)`，
它消除了几乎所有的 JDBC 代码和参数的手工设置以及结果集的检索

MyBatis 使用简单的 XML 或注解映射成数据库中的记录

MyBatis 是一个半自动映射的框架，因为 MyBatis 需要手动匹配 POJO、SQL 和映射关系。而 Hibernate 是一个全表映射的框架，
只需要提供 POJO 和映射关系即可

使用 MyBatis 框架时需要将它的核心包和依赖包引入到应用程序中。如果是 Web 应用，只需要将核心包和依赖包复制到`/WEB-INF/lib`
目录中

## MyBatis 与 Spring 的整合

实现 MyBatis 与 Spring 的整合需要导入相关 JAR 包，包括 MyBatis、Spring 以及其他 JAR 包

Spring 框架所需的 JAR 包包括它的核心模块 JAR、AOP 开发使用的 JAR、JDBC 和事务的 JAR 包

MyBatis 与 Spring 整合的中间 JAR 包

数据库驱动 JAR 包

数据源所需的 JAR 包

开发者只需要进行业务处理，不需要再写 SqlSession 对象的创建、数据库事务的处理等繁琐代码。因此 MyBatis
整合 Spring 后方便了数据库访问操作，提高了开发效率

## 映射器

MyBatis 框架的强大之处体现在 SQL 映射文件的编写上，本章重点讲解 SQL 映射文件的编写

映射器是 MyBatis 最复杂且最重要的组件，由一个接口加上 XML 文件(SQL 映射文件)组成，
MyBatis 的映射器也可以使用注解完成，但在实际应用中使用不广泛，原因如下

- 面对复杂的 SQL 会显得无力

- 注解的可读性较差

- 注解丢失了 XML 上下文相互引用的功能

### SQL 映射文件的常用配置元素

- `select` 查询语句，最常用，最复杂的元素之一，可以自定义参数，返回结果集

- `insert` 插入语句，执行后返回一个整数，代表插入的行数

- `update` 更新语句，执行后返回一个整数，代表更新的行数

- `delete` 删除语句，执行后返回一个整数，代表删除的行数

- `sql` 定义一部分 SQL，在多个位置被引用

- `resultMap` 从数据库结果集中来加载对象

**根据 uid 查询一个用户信息**

```xml
<select id="selectUserById" parameterType="Integer" resultType="com.po.MyUser">
  select * from user where uid = #{uid}
</select>
```

在实际开发中，查询 SQL 语句经常需要多个参数，例如多条件查询，当传递多个参数时，
`<select>`元素的`parameterType`属性值的类型，MyBatis 允许 Map 接口通过键值对传递多个参数

**查询陈姓男性用户信息**

```
public List<MyUser> selectAllUser(Map<String, Object> param);
```

```xml
<select id="selectAllUser" resultType="com.po.MyUser" parameterType="map">
  select * from user
  where uname like concat('%', #{u_name}, '%')
  and usex = #{u_sex}
</select>
```

参数名`u_name`和`u_sex`是 Map 的 Key

```java
public void test() {
  // 查询多个用户
  Map<String, Object> map = new HashMap<>();
  map.put("u_name", "陈");
  map.put("u_sex", "男");
  List<MyUser> list = userDao.selectAllUser(map);
  for (MyUser myUser : list) {
    System.out.println(myUser);
  }
}
```

如果 SQL 语句很复杂，参数很多，使用 Map 将很不方便，MyBatis 还提供了 Java Bean 传递多个参数的形式

### 使用 Java Bean 传递多个参数

创建一个名为`com.pojo`的包，包中创建一个 POJO 类`SelectUserParam`

```java
package com.pojo;

public class SelectUserParam {
  private String u_name;
  private String u_sex;
}
```

```java
public List<MyUser> selectAllUser(SelectUserParam param);
```

SQL 映射文件 UserMapper.xml 中`查询陈姓男性用户信息` 的代码修改为

```xml
<select id="selectAllUser" resultType="com.po.MyUser" parameterType="com.pojo.SelectUserParam">
  select * from user
  where uname like concat('%', #{u_name}, '%')
  and usex = #{u_sex}
</select>
```

最后`com.controller`包中`UserController`的查询多个用户的代码如下

```java
SelectUserParam su = new SelectUserParam();
su.setU_name("陈");
su.setU_sex("男");

List<MyUser> list = userDao.selectAllUser(su);
for (MyUser myUser : list) {
  System.out.println(myUser);
}
```

### <update>与<delete>元素

修改一个用户

```xml
<update id="updateUser" parameterType="com.po.MyUser">
  update user set uname = #{uname}, usex = #{usex} where uid = #{uid}
</update>
```

删除一个用户

```xml
<delete id="deleteUser" parameterType="Integer">
  delete from user where uid = #{uid}
</delete>
```

### <sql> 元素

<sql> 元素的作用在于可以定义 SQL 语句的一部分，以方便后面的 SQL 语句引用它，例如反复使用的列名
在 MyBatis 中只需要使用<sql>元素编写一次便能在其他元素中引用它

```xml
<sql id="comColumns">id,uname,usex</sql>


<select id="selectUser" resultType="com.po.MyUser">
  select <include refid="comColumns"> from user
</select>
```

## <resultMap> 元素

<resultMap>元素表示结果映射集，是 MyBatis 中最重要也是最强大的元素，主要用来定义映射规则、级联的更新
以及定义型转化器等

一条查询 SQL 语句执行后将返回结果，而结果可以使用 Map 存储，也可以使用 POJO 存储

任何 select 语句都可以使用 Map 存储结果，示例

```xml
<!-- 查询所有用户信息存储到Map中 -->
<select id="selectAllUserMap" resultType="map">
  select * from user
</select>
```

测试上述 SQL 配置文件
首先在`com.dao.UserDao`接口中添加如下接口方法

```java
public List<Map<String, Object>> selectAllUserMap();
```

然后在`com.controller`包的`UserController`类中调用接口方法

```java
List<Map<String, Object>> lmp = userDao.selectAllUserMap();
for (Map<String, Object> map : lmp) {
  System.out.println(map)
}
```

上述 Map 的 key 是 select 语句查询的字段名(必须完全一样)，而 Map 的 value 是查询返回结果中
字段对应的值，一条记录映射到一个 Map 对象中。Map 用起来很方便，但可读性稍差，有的开发者喜欢用 POJO 的方式

### 使用 POJO 存储结果集

有的开发者喜欢使用 POJO 的方式存储结果集，一方面可以使用自动映射，例如使用 resultType 属性，但有时候是为了
更复杂的映射或级联

## 级联查询

级联关系是一个数据库实体的概念，有 3 种级联关系，分别是一对一级联、一对多级联以及多对多级联。

级联的优点是获取关联数据十分方便，但是级联过多会增加数据库系统的复杂度，同时降低系统的性能。
在实际开发中要根据实际情况判断是否需要使用级联

如果表 A 中有一个外键引用了表 B 的主键，A 表就是子表，B 表就是父表。当查询表 A 的数据时，通过表
A 的外键将表 B 的相关记录返回，这就是级联查询。

例如，当查询一个人的信息时，同时根据外键(身份号)将他的身份证信息返回

### 一对一级联查询

一对一级联关系在现实生活中是十分常见的，例如一个大学生只有一张一卡通，一张一卡通只属于一个学生。再如人与身份证的关系

MyBatis 如何处理一对一级联查询？MyBatis 通过`<resultMap>`元素的子元素`<association>`处理这种一对一级联关系

如下示例说明一对一级联查询，身份证表`idcard` 个人信息表 `person`

**创建持久化类**

持久化类 Idcard

```java
package com.po;

public class Idcard {
  private Integer id;
  private String code;

  @Override
  public String toString() {
    return "Idcard [id=" + id + ",code=" + code + "]";
  }
}
```

持久化类 Person

```java
package com.po;

public class Person {
  private Integer id;
  private String name;
  private Integer age;

  // 个人身份证关联
  private Idcard card;

  @override
  public String toString() {
    return "Person [id=" + id + ",name=" + name + ",age=" + age + ",card="
    + card + "]";
  }
}
```

**创建映射文件**

首先，在 MyBatis 的核心配置文件`mybatis-config.xml`中打开延迟加载开关

在使用 MyBatis 嵌套查询方式进行关联查询时，使用 MyBatis 的延迟加载可以在一定程度上提高查询
效率

```xml
<settings>
  <!-- 打开延迟加载开关 -->
  <settiing name="lazyLoadingEnabled" value="true"></setting>
  <setting name="aggressiveLazyLoading" value="false"></setting>
</settings>
```

创建映射文件`IdCardMapper.xml`

```xml
<mapper namespace="com.dao.IdCardDao">
  <select id="selectCodeById" parameterType="Integer" resultType="com.po.Idcard">
    select * from idcard where id = #{id}
  </select>
</mapper>
```

映射文件`PersonMapper.xml`

```xml
<mapper namespace="com.dao.PersonDao">
  <resultMap type="com.po.Person" id="cardAndPerson1">
    <id property="id" column="id" />
    <result property="name" column="name" />
    <result property="age" column="age" />
    <!-- 一对一查询 -->
    <association property="card" column="idcard_id" javaType="com.po.Idcard" select="com.dao.IdCardDao.selectCodeById">
  </resultMap>
  <select id="selectPersonById1" parameterType="Integer" resultMap="cardAndPerson1">
    select * from person where id = #{id}
  </select>
</mapper>
```

### 一对多级联查询

在实际生活中一对多级联关系有许多，例如一个用户可以有多个订单，而一个订单只属于一个用户

### 多对多级联查询

其实，MyBatis 没有实现多对多级联，这是因为多对多级联可以通过两个一对多级联进行替换
例如，一个订单可以有多种商品，一种商品可以对应多个订单，订单与商品就是多对多的级联关系

```xml
<resultMap type="com.po.Orders" id="allOrdersAndProducts">
  <id property="id" column="id" />
  <result property="ordersn" column="ordersn" />
  <!-- 多对多级联 -->
  <collection property="products" ofType="com.po.Product">
    <id property="id" column="pid" />
    <result property="name" column="name"/>
    <result property="price" column="price">
  </collection>
</resultMap>
<select id="selectallOrdersAndProducts" resultMap="allOrderAndProducts">
  select o.*,p.id as pid, p.name, p.price
  from orders o, orders_detail od, product  p
  where od.orders_id = o.id
  and od.product_id = p.id
</select>
```
