# Spring Boot 国际化

国际化是指软件开发时应该具备支持多种语言和地区的功能，开发的软件需要能同时应对不同国家和地区的用户访问，
并根据用户地区和语言习惯，提供相应的、符合阅读习惯的页面和数据。

在 Spring 项目中实现国际化，通常需要以下 3 步

- 编写国际化资源（配置）文件

- 使用 ResourceBundleMessageSource 管理国际化资源文件

- 在页面获取国际化内容

## 编写国际化资源文件

在 Spring Boot 的类路径下创建国际化资源文件，文件名格式为：`基本名_语言代码_国家或地区代码`
login_en_US.properties、login_zh_CN.properties

例如，在`src/main/resources`下创建一个 i18n 的目录，并在该目录中按照国际化资源文件命名格式分别创建
以下三个文件

- login.properties 无语言设置时生效

- login_en_US.properties 英语时生效

- login_zh_CN.properties 中文时生效

## 使用 ResourceBundleMessageSource 管理国际化资源文件

Spring Boot 已经对 ResourceBundleMessageSource 提供了默认的自动配置

Spring Boot 通过 MessageSourceAutoConfiguration 对 ResourceBundleMessageSource 提供了默认配置

MessageSourceProperties 为 basename、encoding 等属性提供了默认值

basename 表示国际化资源文件的基本名、其默认取值为 message，即 Spring Boot 默认会获取类路径下的`message.properties`
以及`message_xxx.properties`作为国际化资源文件

在 application.properties 等配置文件中修改

```
spring.messages.basename=i18n.login
```

## 获取国际化内容

```html
<form class="form-signin" th:aciton="@{/usr/login}" method="post">
  <h1 class="sign-title" th:text="#{login.btn}">Sign In</h1>

  <div class="registration">
    <a href="/registration.html" th:href="@{/registration.html}">
      [[#{login.signup}]]
    </a>
  </div>
</form>
```
