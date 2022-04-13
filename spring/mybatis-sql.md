# 动态 SQL

本章重点讲解如何拼接 MyBatis 的动态 SQL 语句。开发人员通常根据需求手动拼接 SQL 语句，这是一个极其麻烦的工作，
而 MyBatis 提供了对 SQL 语句动态组装的功能

MyBatis 的动态 SQL 元素与 JSTL 或 XML 文本处理器相似，常用`<if>`、`<choose>`、`<when>`、`<otherwise>`、
`<trim>`、`<where>`、`<set>`、`<foreach>`、`<bind>`等元素

`<if>` 元素是最常用的元素，它类似于 Java 中的 if 语句

`<choose>` 元素，有点像 Java 中的`switch`
