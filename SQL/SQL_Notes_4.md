# SQL学习笔记4——SQL中检索数据之排序查询
## 内容概要
* 语法
* `ORDER BY`子句
* 按多个列排序
* 按列位置排序
* 指定排序方向
## 语法
```mysql
select 查询列表
from 表
[where 筛选条件]
order by 排序列表 [asc|desc];
```
## `ORDER BY`子句
* 子句：SQL语句由子句构成，有些子句是必须的，而有的可选。我们前一节课学习过`SELECT`语句的`FROM`子句
* 按某列排序检索数据：`SELECT 展示列名 FROM 表名 ORDER BY 排序列名;`
## 按多个列排序
* `SELECT 展示列名 FROM 表名 ORDER BY 排序列名1,排序列名2;`
## 按列位置排序
* `SELECT 展示列名1,展示列名2 FROM 表名 ORDER BY 2;`
* 这里`ORDER BY`后面的数字就对应前面第几个展示列名
## 指定排序方向
* SQL排序默认升序
* 降序使用`DESC`关键字： `SELECT 展示列名 FROM 表名 ORDER BY 排序列名 DESC;`
* `DESC`关键字只应用到直接位于其前面的列名
* 如果想要在多个列上降序排列，需要对每个列指定`DESC`关键字
* `DESC`是`DESCENDING`的缩写，也可使用`DESCENDING`
## 特点
* 排序列表支持单个字段、多个字段、函数、表达式、别名
## 小结
本节课我们学习了`SELECT`语句的`ORDER BY`子句对检索出的数据进行排序，这个子句必须是`SELECT`语句的最后一个子句(除`limit`语句之外)。我们学习了如何按某列、多个列(升序、降序)排序





