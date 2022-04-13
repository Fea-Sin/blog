# 文件的上传和下载

通过本章学习，可以掌握 Spring MVC 框架的单文件上传、多文件上传以及文件下载

文件上传是 Web 应用经常要面对的问题，对于 Java 应用而言上传文件有多种方式，可以使用文件流手工编程上传，
基于 commons-fileupload 组件上传等

Spring MVC 框架的文件上传是基于 commons-fileupload 组件的文件上传，只不过 Spring MVC 框架在原有文件上传
组件做了进一步封装

Commons 是 Apache 开放源代码组织中的一个 Java 子项目，该项目包括文件上传、命令行处理、数据库连接池、XML 配置文件
处理等模块。fileupload 就是其中用来处理基于表单的文件上传子项目，其性能优良，并支持任意大小的文件上传

## 基于表单的文件上传

标签`<input type='file'>` 会在浏览器中显示一个输入框和一个按钮，输入框可供用户填写本地文件的文件名和路径名，
按钮可以让浏览器打开一个文件选择框供用户选择文件

```html
<form action="upload" method="post" enctype="multipart/form-data">
  <input type="file" name="myfile" />
</form>
```

表单的 enctype 属性指定的是表单数据的编码方式

- `application/x-www-form-urlencode` 这是默认的编码方式，它只处理表单域里的 value 属性值

- `multipar/form-data` 该编码方式以二进制的方式来处理表单数据，并将文件域指定的内容封装到请求参数里

- `text/plain` 该编码方式只有当表单的 action 属性是 `mailto:URL`的形式时才使用，主要适用于直接通过
  表单发送邮件的方式

## MultipartFile 接口

Spring MVC 框架中上传文件时将文件相关信息及操作封装到`MultipartFile`对象中

该接口具有如下方法

- `byte[] getBytes()` 以字节数组的形式返回文件的内容

- `String getContentType()` 返回文件的内容类型

- `InputStream getInputStream()` 返回一个 InputStream，从中读取文件的内容

- `String getName()` 返回请求参数的名称

- `String getOriginalFilename()` 返回客户端的原始文件名称

- `long getSize()` 返回文件的大小，单位为字节

- `boolean isEmpty()` 判断被上传文件是否为空

- `void transferTo` 将上传文件保存到目标目录下

## 多文件上传

多文件上传

```java
package pojo;

import java.util.List;
import org.springframework.web.multipart.MultipartFile;

public class MultiFileDomain {
  private List<String> description;
  private List<MultipartFile> myfile;
}
```

## 文件下载

实现文件下载有两种方法，一种是通过超链接实现下载，另一种是利用编程编码实现下载。通过超链接实现下载
固然简单，但暴露了下载文件的真实位置，并且只能下载存放在 Web 应用程序所在目录下的文件

利用程序编码实现下载可以增加安全访问控制，还可以从任意位置提供下载数据，可以将文件存放到 web 应用程序
以外的目录中，也可以将文件保存到数据库中

利用程序实现下载需要设置两个报头

- Web 服务器需要告诉浏览器其所输出的类型不是普通文件或 HTML 文件，而是一个保存到本地的下载文件，这需要
  设置`Content-Type`的值为`application/x-msdownload`

- Web 服务器希望浏览器不直接处理相应的实体内容，而是由用户选择相应的实体内容保存到一个文件中，这需要
  设置`Content-Disposition`报头。该报头指定了接收程序处理数据内容的方式，在 HTTP 应用中只有 attachment
  是标准方式，attachment 表示要求用户干预。在 attachment 后面还可以指定 filename 参数，该参数是服务器
  建议浏览器将实体内容保存到文件中的文件名称

```
response.setHeader("Content-Type", "application/x-mddownload");
response.setHeader("Content-Disposition", "attachment; filename=" + filename);
```

**执行下载的 Controller**

```java
@RequestMapping("down")
public String down(@RequestParam String filename, HttpServletRequest request, HttpServletResponse reponse) {
  String aFilePath = null; // 要下载的文件路径
  FileInputStream in = null; // 输入流
  ServletOutputStream out = null; // 输出流

  try {
    aFilePath = request.getServletContext().getRealPath("uploadfiles")
    response.setHeader("Content-Type", "application/x-mddownload");
    response.setHeader("Content-Disposition", "attachment; filename=" + toUTF8String(filename));
    // 读入文件
    in = new FileInputStream(aFilePath + '\\' + filename);

    out = response.getOutputStream();
    out.flush();
    int aRead = 0;
    byte b[] = new byte[1024];
    while ((aRead = in.read(b)) != -1 & in != null) {
      out.write(b, 0, aRead);
    }
    out.flush();
    in.close();
    out.close();
  } catch (Throwable e) {
    e.printStackTrace();
  }
  logger.info("下载成功");
  return null;
}
```
