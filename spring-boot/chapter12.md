# Spring Boot 拦截器

我们对拦截器并不陌生，它可以根据 URL 对请求进行拦截，主要应用于登录校验、权限验证、乱码解决、性能监控和
异常处理等功能上

在 Spring Boot 项目中，使用拦截器功能通常需要以下 3 步

- 定义拦截器

- 注册拦截器

- 指定拦截规则(如果是拦截所有，静态资源也会被拦截)

## 定义拦截器

在 Spring Boot 中定义拦截器十分简单，只需创建一个拦截器类，并实现 HandlerInterceptor 接口即可

HandlerInterceptor 接口定义如下 3 个方法

- boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
  该方法在控制器处理请求方法前执行，返回值表示是否中断后续操作，返回 true 表示继续向下执行，返回 false 表示中断后续操作

- void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView)
  该方法在控制器处理请求方法调用之后、解析视图之前执行，可以通过此方法对请求域中的模型和视图做进一步修改

- void afterCompletion(HttpServletRequest request, HttpServletReponse response, Object handler, Exception ex)
  该方法在视图渲染结束后执行，可以通过此方法实现资源清理、记录日志信息等工作

创建一个 LoginInterceptor 的拦截器，对登录进行拦截

```java

package net.biancheng.www.component;

import lombok.extern.slf4j.Slf4j;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@Slf4j
public class LoginInterceptor implements HandlerInterceptor {
  /**
   * 目标方法执行前
   * @param request
   * @param response
   * @param handler
   * @return
   * @throws Exception
   */
   @Override
   public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
     Object loginUser = request.getSession().getAttribute('loginUser');
     if (loginUser == null) {
       // 未登录
       request.setAttribute('msg', '您没有权限进行此操作，请先登录');
       request.getRequestDispatcher("/index.html").forward(request, response);
       return false;
     } else {
       return true;
     }
   }

   /**
    * 目标方法执行后
    * @param request
    * @param response
    * @param handler
    * @return
    * @throws Exception
    */
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
      log.info('postHandle执行{}', modelAndView)
    }

    /**
     * @param request
     * @param response
     * @param handler
     * @param ex
     * @throws Exception
     */
     @Override
     public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
       log.info('afterCompletion执行异常{}', ex)
     }
}
```

## 注册拦截器

创建一个实现了 WebMvcConfigurer 接口的配置类，使用了@Configuration 注解的类，重写 addInterceptors()方法，并在
该方法中调用 registry.addInterceptor()方法将自定义的拦截器注册到容器中

```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {

  @Override
  public void addInterceptors(InterceptorRegistry registry) {
    registry.addInterceptor(new LoginInterceptor());
  }
}
```

## 指定拦截规则

修改 MyMvcConfig 配置类中 addInterceptors()方法的代码，指定拦截器规则

```java
@Slf4j
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
  @Override
  public void addInterceptors(InterceptorRegistry registry) {
    log.info("注册拦截器");
    // 拦截所有请求，包括静态资源
    // 放行路径
    registry.addInterceptor(new LoginInterceptor()).addPathPatters("/**").excludePathPatterns("/", "/login",
    "/index.html", "/user/login", "/css/**", "/images/**", "/js/**", "/fonts/**")
  }
}
```

创建一个 LoginController

```java
package net.biancheng.www.controller;

import lombok.extern.slf4j.Slf4j;
import net.biancheng.www.bean.User;
import org.springframework.stereotype.Controller;
import org.springframework.util.StringUtils;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;

import javax.servlet.http.HttpSession;
import java.util.Map;

@Slf4j
@Controller
public class LoginController {
  @RequestMapping("/user/login")
  public String doLogin(User user, Map<String, Object>, HttpSession session) {
    if (user != null && StringUtils.hasText(user.getUsername()) && "123456".equals(user.getPassword())) {
      session.setAttribute("loginUser", user);
      log.info("登录成功，用户名：" + user.getUsername());
      // 防止重复提交
      return "redirect:/main.html"
    } else {
      map.put("msg", "用户名密码错粗");
      log.error("登录失败")
      return "login";
    }
  }
}
```

在配置类 MyMvcConfig 中添加视图映射

```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
  @Override
  public void addViewControllers(ViewControllerRegistry registry) {
    // 当访问"/" 或 "/index.html" 时，直接跳转登录页面
    registry.addViewController("/").setViewName("login");
    registry.addViewController("/index.html").setViewName("login")

    registry.addViewController("/main.html").setViewName("main");
  }
}
```
