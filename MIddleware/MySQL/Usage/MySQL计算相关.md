### MySQL  函数计算相关

#### 字段的相关操作：

* 拼接字段：将值连结到一起构成单个值，在 `select` 语句中，使用 `cancat()` 函数来拼接两个列。`cancat()` 拼接串，即把多个串连接起来形成一个较长的串`select concat(vend_name, ' (', vend_country, ') ') form vendors`
* 删除数据右侧多余的空格：`RTrim(field)` 函数。
* 删除数据左侧多余的空格：`LTrim(field)`  函数。
* 删除数据左右两边多余的空格：`Trim()` 函数
* 使用别名，对于使用函数得到的值必须使用别名来引用
* 执行算术计算：mysql 支持基本的算术操作符，使用圆括号来区分优先顺序

#### 数据处理函数

* 常用文本处理函数

  |     函数      |        说明         |
  | :-----------: | :-----------------: |
  |   `Left()`    |  返回串左边的字符   |
  |  `Length()`   |    返回串的长度     |
  |  `Locate()`   |  找出串的一个子串   |
  |   `Lower()`   |   将串转换为小写    |
  |   `LTrim()`   |   去掉左边的空格    |
  |   `Rigth()`   |  返回串右边的字符   |
  |   `RTrim()`   |  去掉串右边的空格   |
  |  `Soundex()`  | 返回串的 soundex 值 |
  | `SubString()` |   返回子串的字符    |
  |   `upper()`   |   将串转换为大写    |

* 日期和时间处理函数

  |      函数       |            说明            |
  | :-------------: | :------------------------: |
  |   `AddDate()`   |  增加一个日期（天，周等）  |
  |   `AddTime()`   |  增加一个时间（时，分等）  |
  |   `CurDate()`   |        返回当前日期        |
  |   `CurTime()`   |        返回当前时间        |
  |    `Date()`     |   返回日期时间的日期部分   |
  |  `DateDiff()`   |      计算两个日期之差      |
  |  `Date_Add()`   |        日期运算函数        |
  | `Date_Format()` |     格式化日期或时间串     |
  |     `Day()`     |   返回一个日期的天数部分   |
  |  `DayOfWeek()`  |    返回日期对应的星期几    |
  |    `Hour()`     |   返回一个时间的小时部分   |
  |   `Minute()`    |   返回一个时间的分钟部分   |
  |    `Month()`    |     返回日期的月份部分     |
  |     `Now()`     |     返回当前日期和时间     |
  |   `Second()`    |    返回一个时间的秒部分    |
  |    `Time()`     | 返回一个日期时间的时间部分 |
  |    `Year()`     |   返回一个日期的年份部分   |
  |    `from_unixtime(int timestamp)`     |   返回对应时间戳的日期   |
  |    `unix_timestamp(string date)`     |   返回对应时间的时间戳   |

* 数值处理函数

  |   函数   |          说明          |
| :------: | :--------------------: |
  | `Abs()`  |   返回一个数的绝对值   |
  | `Cos()`  |   返回一个角度的余弦   |
  | `Exp()`  |   返回一个数的指数值   |
  | `Mod()`  |    返回除操作的余数    |
  |  `Pi()`  |       返回圆周率       |
  | `Rand()` | 返回一个随机数(浮点数) |
  | `Sin()`  |   返回一个角度的正弦   |
  | `Sqrt()` |   返回一个数的平方根   |
  | `Tan()`  |   返回一个角度的正切   |

#### 聚合函数

**运行在行组上，计算和返回单个值的函数**

* `AVG()`返回某列的平均值
* `COUNT()` 返回某列的行数
* `MAX()` 返回某列的最大值
* `MIN()` 返回某列的最小值
* `SUM()` 返回某列值的和