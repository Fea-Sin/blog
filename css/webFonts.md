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

`@font face`本质上就是一个定义字体或字体集的变量，这个变量不仅仅是简单的自定义字体，还包括字体重
命名，默认字体样式设置等等
