## 颜色操作

**颜色处理函数**

颜色处理函数传入参数一般同所要改变的数据的单位相同，传入的百分比数值一般认为是绝对数值，
（而非与所改变值为单位的百分比），例如增加 10%，若原来的值是 10%，此时的结果是 20%。
如果将`method`参数设置为`relative`，此时的传入百分比值为相对于要改变的值，例如增加
10%，若原来值为 10%，此时结果为 11%。

数值会有严格的范围限制，返回的也会有统一显示格式

### 增加饱和度（saturate）

增加颜色饱和度

- color: 一个颜色对象

- amount: 0-100% 的百分比数值

- method: 可选参数，若为`relative`，传入百分比是相对值

返回`color`

`saturate(hsl(90, 80%, 50%), 20%)`
返回
`#80ff00 // hsl(90, 100%, 50%)`

`saturate(#80e619, 20%)`
返回
`#80ff00`

### 减小饱和度（desaturate）

`desaturate(hsl(90, 80%, 50%), 20%)`
返回
`#80cc33 // hsl(90, 60%, 50%)`

### 变亮（lighten）

`lighten(hsl(90, 80%, 50%), 20%)`
返回
`#b3f075 // hsl(90, 80%, 70%)`

### 变暗（darken）

`darken(hsl(90, 80%, 50%), 20%)`
返回
`#4d8a0f // hsl(90, 80%, 30%)`

### 增加不透明度（fadein）

增加不透明度，对不透明的颜色无效
`fadein(hsla(90, 90%, 50%, 0.5), 10%)`
返回
`rgba(128, 242, 13, 0.6) // hsla(90, 90%, 50%, 0.6)`

###  减少不透明度（fadeout）

减少不透明度，对不透明的颜色无效
`fadeout(hsla(90, 90%, 50%, 0.5), 10%)`
返回
`raba(128, 242, 13, 0.4) // hsla(90, 90%, 50%, 0.4)`

### fade

设置颜色的透明度为绝对值，不管原来颜色有没有透明度

- color: 一个颜色对象
- amount: 0-100% 百分比数值

`fade(hsl(90, 90%, 50%), 10%)`
返回
`raba(128, 242, 13, 0.1) // hsla(90, 90%, 50%, 0.1)`

### 旋转色调（spin）

角度的范围是 0-360，如果传入的值大于 360 则会做模处理，360 和 720 得到的结果相同。
请注意，颜色是通过 RGB 转换的，这不会为灰色保留色调值（因为色调在没有饱和度时没有意义），
请确保以保留色调的方式应用函数

`spin(saturate(#aaaaaa, 10%), 10)`

- color: 颜色对象
- angle: 一个需要旋转的角度数值（+ or -）
  `spin(hsl(10, 90%, 50%), 30)`
  返回 `#f2a60d // hsl(40, 90%, 50%)`
  `spin(hsl(10, 90%, 50%), -30)`
  返回`#f20d59 // hsl(340, 90%, 50%)`

### mix

按照一定比例融合两种颜色，不透明度也一并按照比例融合

- color1: 颜色对象
- color2; 颜色对象
- weight: 两种颜色的比例，默认是 50%

`mix(#ff0000, #0000ff, 50%)`
`mix(rgba(100, 0, 0, 1), rgba(0, 100, 0, 0.5), 50%)`

### tint

颜色与白色融合
`mix(#ffffff, @color, @weight)`

no-alpha
`tint(#007fff, 50%)`
with-alpha
`tint(rgba(0,0,255,0.5), 50%)`

### shade

颜色与黑色融合
`mix(#000000, @color, @weight)`

no-alpha
`shade(#007fff, 50%)`
with-alpha
`shade(rgba(0,0,255,0.5), 50%)`

### greyscale

去掉颜色的所有饱和度
`desaturate(@color, 100%)`

`greyscale(hsl(90, 90%, 50%))`
返回
`#808080 // hsl(90, 0%, 50%)`

greyscale 产生的灰色比较暗相比于原色

另一种方式是使用`luma`

```
@c: luma(hsl(90, 90%, 50%))
color: rgb(@c, @c, @c)
```

### luma

感知颜色的亮度

`luma(rgba(100, 200, 30))`
返回
`44%`