# MySQL 函数

函数表示对输入参数值返回一个具有特定关系的值，MySQL 提供了大量丰富的函数，在进行数据库管理以及数据查询和操作
时将会经常用到各种函数。
通过对数据的处理，数据库功能可以变的更加强大，更加灵活得满足不同用户的需求。各类函数从功能方面主要分为以下几类，
数据函数、字符串函数、日期和时间函数、条件判断函数、系统信息函数和加密函数等其他函数

### 数学函数

数学函数主要用来处理数值数据，主要的数学函数有，绝对值函数、三角函数(正弦函数、余弦函数、正切函数、余切函数等)、
对数函数、随机函数等，在有错误产生时，数学函数将会返回空值 NULL

- 平方根函数`SQRT(9)`

- 求余函数`MOD(x,y)`

- 获取整数的函数`CELL(x)`、`CELLING(x)`和`FLOOR(x)`， CELL 和 CELLING 意义相同，返回不小于 x 的最小整数值，
  FLOOR 返回不大于 x 的最大整数值，返回值转化为一个`BIGINT`

- 获取随机数的函数`RAND()`和`RAND(x)`，RAND 返回一个随机浮点值范围在 0 到 1 之间。若指定一个整数参数 x，则它被用作
  种子值，用来产生重复序列

- 四舍五入函数`ROUND(x)`、`ROUND(x,y)`、`TRUNCATE(x,y)`
  `ROUND(x)` 返回最接近与参数 x 的整数，对 x 值进行四舍五入

`ROUND(x,y)`返回最接近于参数 x 的数，其值保留到小数点后面 y 位，若 y 为负值，则将保留 x 值到小数点左边 y 位

> y 值为负数时，保留的小数点左边的相应位数直接保存为 0，不进行四舍五入

`TRUNCATE(x,y)`返回被舍去至小数点后 y 位的数字 x。若 y 的值为`0`，则结果为不带小数部分。若 y 为负数，则截去 x 小数点
左起第 y 位开始后面所有低位的值

- 幂运算函数`POW(x,y)`、`POWER(x,y)`、`EXP(x)`

POW 或 POWER 函数返回 x 的 y 次乘方的结果值

EXP 函数返回 e 的 x 乘方的值

- 对数运算函数`LOG(x)`和`LOG10(x)`

LOG 函数计算自然对数，LOG10 函数返回以 10 为基数的对数

- 角度和弧度相互转换的函数`RADIANS(x)`和`DEGREES(x)`

RADIANS 将参数 x 由角度转化为弧度

DEGREES 将参数 x 由弧度转化为角度

- 正弦函数`SIN(x)`和反正弦函数`ASIN(x)`

- 余弦函数`COS(x)` 和反余弦函数`ACOS(x)`

- 正切函数`TAN(x)`、反正切函数`ATAN(x)`和余切函数`COT(x)`

COT 与 TAN 互为倒数函数

### 字符串函数

字符串函数主要用来处理数据库中的字符串数据，MySQL 中字符串函数有，计算字符串长度函数、字符串合并函数、字符串替换函数、
字符串比较函数、查找指定字符串位置函数等

#### 计算字符串字符数的函数和字符串长度的函数

`CHAR_LENGTH(str)` 返回值为字符串 str 所包含的字符个数，一个多字节字符算做一个单字符

`LENGTH(str)`返回值为字符串的字节长度，使用`utf8`编码字符集时，一个汉字是 3 个字节，一个数字或字母算一个字节

#### 合并字符串函数

`CONCAT(s1,s2,...)` 返回结果为连接参数产生的字符串，或许有一个或多个参数，如果有任何一个参数为 NULL，则返回
结果为 NULL，如果所有参数均为非二进制字符串，则结果为非二进制字符串。如果参数中含有一个二进制字符串，则结果
为一个二进制字符串

`CONCAT_WS(x,s1,s2,...)` 为 CONCAT With Separator，第一个参数 x 是其他参数的分隔符。分隔符可以是
一个字符串，也可以是其他参数，如果分隔符为 NULL，则结果为 NULL。函数会忽略任何分隔符参数后的 NULL 值

#### 替换字符串的函数

`INSERT(s1,x,len,s2)` 返回字符串 s1 起始于 x 位置和被字符串 s2 取代的 len 个字符后的字符串。
如果 x 超过字符串长度，则返回原始字符串。如果 len 的长度超过字符串的长度，则从位置 x 开始替换。若任何
一个参数为 NULL，则返回值为 NULL

#### 获取指定长度的字符串的函数

`LEFT(s,n)` 返回字符串 s 开始的最左边 n 个字符

`RIGHT(s,n)` 返回字符串中右边的字符

#### 填充字符串的函数

`LPAD(s1,len,s2)` 左填充

`RPAD(s1,len,s2)` 右填充

#### 删除空格的函数

`LTRIM(s)`、`RTRIM(s)`、`TRIM(s)`

#### 删除指定字符串的函数

`TRIM(s1 FROM s)`函数删除字符串中两端指定的字符

#### 重复生成字符串的函数

`REPEAT(s,n)`

#### 替换函数

`REPLACE(s,s1,s2)` 使用字符串 s2 替代字符串 s 中所有的字符串 s1

比较字符串大小的函数`STRCMP(s1,s2)`

获取子串的函数`SUBSTRING(s,n,len)` 和`MID(s,n,len)`

匹配子串开始位置的函数`LOCATE(str1,str)`、`POSITION(str1 IN str)`、`INSTR(str,str1)`，返回子字符串
str1 在字符串 str 中的开始位置

字符串逆序的函数`REVERSE(s)`

### 日期和时间函数

日期和时间函数主要用来处理日期和时间值，一般的日期函数除了使用 DATE 类型的参数外，也可以使用 DATETIME
或者 TIMESTAMP 类型的参数，但会忽略这些值的时间部分。同样的以 TIME 类型值为参数的函数，可以接收 TIMESTAMP
类型的参数，但是忽略日期部分。许多日期函数可以同时接受数和字符串类型的两种参数

- 获取当前日期函数`CURDATE()`和`CURRENT_DATE()` 将当前日期按照`YYYY-MM-DD`或`YYYYMMDD`格式的值返回

- 获取当前时间函数`CURTIME()`和`CURRENT_TIME()` 将当前时间以`HH:MM:SS`或`HHMMSS`格式返回

- 获取当前日期和时间的函数`CURRENT_TIMESTAMP()`、`LOCALTIME()`、`NOW()`、`SYSDATE()`4 个函数的作用相同，
  均返回当前日期和时间`YYYY-MM-DD HH:MM:SS`或`YYYYMMDDHHMMSS`

#### UNIX 时间戳函数

UNIX_TIMESTAMP(date)若无参数调用，则返回一个 Unix 时间戳(1970-01-01 00:00:00 GMT 之后的秒数)，GMT 为格林尼治
标准时间

若用 date 来调用 UNIX_TIMESTAMP()，它会将参数值以`1970-01-01 00:00:00`后的秒数返回。date 可以是 DATE 字符串、
DATETIME 字符串、TIMESTAMP 或一个当地时间的 YYMMDD 或 YYYY-MM-DD 格式的数字

FROM_UNIXTIME(date)函数把 UNIX 时间戳转换为普通格式的时间，与 UNIX_TIMESTAMP()互为反函数

#### 返回 UTC 日期的函数和返回 UTC 时间的函数

UTC_DATE()函数返回当前 UTC(世界标准时间)日期值，其格式为`YYYY-MM-DD`或`YYYYMMDD`

UTC_TIME()返回当前 UTC 时间值，其格式为`HH:MM:SS`或`HHMMSS`

- 获取月份的函数`MONTH(date)`和`MONTHNAME(date)`
  MONTHNAME(date)函数返回日期 date 对应月份的英文全名

- 获取星期的函数`DAYNAME(d)`、`DAYOFWEEK(d)`、`WEEKDAY(d)`

- `WEEK(d)` 函数查询指定日期时一年中第几周

- `DAYOFYEAR(d)`和`DAYOFMONTH(d)`返回 d 是一年中的第几天`1~366`和 d 是一个月中的第几天`1~31`

- 获取年份、季度、小时、分钟、秒钟的函数`YEAR(date)`、`QUARTER(date)`、`MINUTE(time)`、`SECOND(time)`

- 获取日期指定的函数`EXTRACT(type FROM date)`
  `EXTRACT(YEAR_MONTH FROM '2011-07-12 01:02:03')` 201107

- `TIME_TO_SEC(time)`返回已转化为秒的 time 参数

- `SEC_TO_TIME(seconds)` 返回被转化为小时、分钟、秒数的 seconds 参数值，格式为`HH:MM:SS`或`HHMMSS`

## 条件判断函数

条件判断函数也称为控制流程函数，根据满足的条件不同，执行相应的流程

- IF(expr,v1,v2) expr 是 TRUE(expr <> 0 and expr <> NULL)则返回 v1，否则返回 v2
  IF()的返回值为数字或字符串值

- IFNULL(v1,v2)假如 v1 不为 NULL，则返回 v1，否则返回值为 v2

#### CASE 函数

CASE expr WHEN v1 THEN r1 [WHEN v2 THEN r2] [ELSE rn] END

该函数表示，如果 expr 值等于某个 vn，则返回对应位置 HTEN 后面的结果。如果与所有值都不相等，则返回
ELSE 后面的 rn

## 系统信息函数

本节将介绍系统信息函数，MySQL 中的系统信息有，数据库的版本号、当前用户名和连接数、系统字符集、最后一个
自动生成的 ID 值等

- VERSION() 查看当前 MySQL 版本

- CONNECTION_ID()返回 MySQL 服务器当前连接的次数

- SHOW PROCESSLIST/SHOW FULL PROCESSLIST;

### 获取用户名的函数

USER()、CURRENT_USER、CURRENT_USER()、SYSTEM_USER()、SESSION_USER()这几个函数返回当前被 MySQL 服务器
验证的用户名和主机名组合

- 获取字符串的字符集函数，CHARSET('abc')，返回系统默认的字符集`utf8`

- 获取最后一个自动生成的 ID 值的函数，LAST_INSERT_ID()

## 加密函数

加密函数主要用来对数据进行加密和界面处理，以保证某些重要数据不被别人获取，这些函数在保证数据库安全时非常有用。

- 加密函数 PASSWORD(str)
  PASSWORD(str)从原明文密码 str 计算并返回加密后的密码字符串，当参数为 NULL 时，返回 NULL。PASSWORD 函数在
  MySQL 服务器的鉴定系统中使用，不应将它用在个人的应用程序中，PASSWORD 加密是单向的，与 Unix 中密码被加密的方式不同

- 加密函数 MD5(str)
  MD5(str)为字符串算出一个 MD5 128 比特校验和，该值以 32 位十六进制的二进制字符串形式返回

- 加密函数`ENCODE(str,pswd_str)`，使用`pswd_str`加密`str`

- 解密函数`DECODE(crypt_str, pswd_str)` 使用`pswd_str`解密加密后的字符串`crypt_str`

- 重复执行指定操作的函数，`BENCHMARK(count,expr)`，BENCHMARK 函数重复`count`次表达式`expr`，
  它可以用于计算 MySQL 处理表达式的速度，结果通常为 0

- 改变字符集的函数，`CONVERT(... USING ...)`
  示例，`CONVERT('string' USING latin1)`

- 改变数据类型的函数，CAST(x, AS type)和 CONVERT(x,type)，函数将一个类型的值转换为另一个类型的值，
  可转换的 type 有，`BINARY`、`CHAR`、`DATE`、`TIME`、`DATETIME`、`DECIMAL`、`SIGNED`、`UNSIGNED`
  示例，CAST(100 AS CHAR(2)) -> 10
  CONVERT('2010-10-10 12:12:12', TIME) -> 12:12:12
