## Web Fonts

- font-family

可以设置一个或多个 family names，浏览器会从前往后选择相应字体，直到遇到系统可以
使用的字体，并设置为相应字体

```css
p {
  font-family: Helvetica, "Trebuchet MS", Verdana, sans-serif;
}
```

这里有少数字体可以保证在所有客户系统可用，被称为 `web safe fonts`(安全字体)
你可以把喜欢的字体放在前面，安全字体放在后面作为替代使用

`web fonts`是一个 CSS 特性可以指定特殊字体文件，从服务端获取并使用，IE6+ 以上浏览器
都能支持，这样所有浏览器都能使用所指定的特殊字体

语法 `@font-face`，指定所需的字体文件

```css
@font-face {
  font-family: "myFont";
  src: url("myFont.woff2");
}
```

现在就可以在 css 中使用在`@font-face`中指定的字体了

```css
html {
  font-family: "myFont", "Bitstream Vera Serif", serif;
}
```

如下是一些关于 fonts 更详细的介绍

- 字体并不是免费的，你必须购买，和遵守它们的许可协议。在未经允许情况下不能偷偷使用它们

- 所有主流浏览器都支持 WOFF/WOFF2(Web Open Font Format version 1/2)，即使是更老的浏览器
  如 IE9(2011 年发布)也支持 WOFF format

- WOFF2 支持整个 `TrueType` 和 `OpenType`规范，包括可变字体、彩色字体和字体集合

- 列出字体文件的顺序很重要，如果你为浏览器提供了多个字体文件，浏览器会选择第一个可用的字体文件，
  这就是你应该把最好的格式（也就是 WOFF2）放在首位的原因，老个字体格式依次放在后面

- 如果你需要支持老旧的浏览器，你应该提供 EOT(Embedded Open Type)，TTF(TrueType Font)和
  SVG web fonts

> 注意
>
> web fonts 从 IE4 开始已经被支持了

可以从免费的字体网站下载所需字体，[dafont](https://www.dafont.com/)

在下载的字体包里，可能有多个文件，一些字体包含了不同的变种，例如 `thin`、`medium`、`bold`、`italic`、
`thin italic`等等

### 生成所需的字体格式

- 如果你是商业项目，首先确定获得许可

- 打开[Fontsquirrel](https://www.fontsquirrel.com/tools/webfont-generator)

- 上传你的字体文件

- 勾选`Yes, the fonts I'm uploading are legally eligible for web embedding`

- 点击下载

如果你需要兼容旧的浏览器，请选择`Expert`模式，选择`SVG`、`EOT`、`TTF`格式，然后下载

一些字体转换工具

- [sfnt2woff-zopfli](https://github.com/bramstein/sfnt2woff-zopfli) 将 ttf 转为 woff
- [fontforge](https://fontforge.org/en-US/) 将 ttf 转为 svg
- [batik ttf2svf](https://people.apache.org/~clay/batik/ttf2svg.html) 将 ttf 转为 svg
- [woff2](https://github.com/google/woff2)将 ttf 转为 woff2

### 使用生成的字体套件

```css
@font-face {
  font-family: "zantrokeregular";
  src: url("fonts/zantroke-webfont.woff2") format("woff2"), url("fonts/zantroke-webfont.woff")
      format("woff");
  font-weight: normal;
  font-style: normal;
}
```

## 真正了解@font face 规则

`@font-face`本质上就是一个定义字体或字体集的变量，这个变量不仅仅是简单的自定义字体，还包括字体重
命名，默认字体样式设置等等

`@font-face`主要规则

```css
@font-face {
  font-family: "example";
  src: url("example.ttf");
  font-style: normal;
  font-weight: normal;
  unicode-range: U+0025-00FF;
}
```

接下来的大量案例，是基于`local`本地字体做示意的，而 IE7，IE8 浏览器是不支持`local`本地字体的。
请用现代浏览器做测试。

**font-family**

`font-family`可以看成一个字体变量，名称可以很随意

```css
@font-face {
  font-family: "$";
  src: local("Microsoft Yahei");
}
```

这时，对普通 HTML 元素 css 设置其`font-family`属性值为`$`，则其字体就表现
成`微软雅黑`(如果本地有这个字体)，有一点需要注意，设置稀奇古怪的字符时，一定要加引号

**src**

src 表示调用字体文件，可以是本地字体文件（IE9+支持），也可以是线上地址（可能有跨域限制）

实用实例，字体文件名简写

很多网站使用`微软雅黑`字体，名称有点长，还要加引号，还有加空格，好麻烦
`font-family: 'Microsoft Yahei'`

可以利用`@font-face`简化

```css
@font-face {
  font-family: YH;
  src: local("Microsoft Yahei");
}
```

使用的时候直接

```css
.title {
  font-family: YH;
}
```

`local`本地作用不止这些

可以通过文字处理工具，提取页面中使用的特殊中文字体并生成新的体积更小的自定义
字体。

一些特殊的标题设计师喜欢用`方正粗雅宋`这个字体（已经购得版权），但是很显然，这么帅的字体，
几乎很少有用户安装，所以实际开发的时候，全都是通过工具而生成一个全新的精简版的`方正粗雅宋`字体文件，
通过`src`属性`url()`方法连接这个字体文件。

很棒的方案，但还不够完美，虽然安装`方正粗雅宋`这个字体的用户不多，但不代表没有用户使用。如果有用户
安装了这个字体，结果还要去下载额外文件，岂不是浪费。如果字体文件加载慢，还会看到文字`变形`的过程，
显然不够友好

我们可以借助`@font-face`规则中的`local`实现体验上的进一步提升

```css
@font-face {
  font-family: FZCYS;
  src: local("FZYaSongS-B-GB"), url("FZCYS.woff2"), url("FZCYS.woff"),
    url("FZCYS.ttf");
}
```

**font-style**

`@font-face`规则中`font-style`和`font-weight`类似，都是用来设置对应字体样式
或字重下该使用什么字体。这些字体是专门的斜体字，宾不是让文字的形状倾斜，而是专门设计的
倾斜的字体

css 规则值

```css
font-style: normal;
font-style: italic;
```

专门的斜体字不好找，我们用`方正姚体`和`方正舒体`做示意

```css
@font-face {
  font-family: "IFont";
  font-style: normal;
  src: local("FZYaoti");
}
@font-face {
  font-family: "IFont";
  font-style: italic;
  src: local("FZShuTi");
}
```

如何使用

```css
/* 此时使用 方正舒体 字体 */
p {
  font-family: "IFont";
  font-style: italic;
}
```

**font weight**

定义不同字重使用不同字体，对中文而言对不同粗细字体的使用很重要。
版权字体`汉仪旗黑`字重非常丰富，但是这字体不像`思源黑体`可以根据`font-weight`属性加载
对应字体文件，使用`@font-face`规则重新定义一下即可

css 规则

```css
font-weight: 100;
font-weight: 200;
font-weight: 300;
font-weight: 400;
font-weight: 500;
font-weight: 600;
font-weight: 700;
font-weight: 800;
font-weight: 900;
```

```css
@font-face {
  font-family: "QH";
  font-weight: 400;
  src: local("HYQihei 40S");
}
@font-face {
  font-family: "QH";
  font-weight: 500;
  src: local("HYQihei 50S");
}
@font-face {
  font-family: "QH";
  font-weight: 600;
  src: local("HYQihei 60S");
}
```

**unicode-range**

`unicode-range`的作用是可以让特定的字符或者字符片段使用特定的字体

微软雅黑的引号不好看，如果需要单独指定这两个引号使用其它字体，代码如下

```css
@font-face {
  font-family: BASE;
  src: local("Microsoft Yahei");
}
@font-face {
  font-family: quote;
  src: local("SimSun");
  unicode-range: U+201c, U+201d;
}
.title {
  font-family: quote, BASE;
}
```

## 使用在线字体服务

在线字体服务非常容易使用

[Google Fonts](https://www.google.com/fonts)

选择所需的字体，`<link>`或`@import`引入页面即可

```css
    <style>
      @import url('https://fonts.googleapis.com/css2?family=Ubuntu:ital,wght@0,300;1,700&display=swap');
      .title {
        font-family: 'Ubuntu', sans-serif;
        font-size: 40px;
        font-weight: 700;
        font-style: italic;
      }
    </style>
```
