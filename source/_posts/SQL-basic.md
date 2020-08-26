---
title: SQL应知应会
categories:
  - DAND-VIP
tags:
  - SQL
abbrlink: 15af4c28
date: 2018-10-06 21:30:09
---

>  If you want your life to be a magnificent story, then begin by realising that you are the author.

本文就Udacity数据分析入门课程中的SQL入门（P1阶段）和SQL进阶（P3阶段）的知识点进行总结。SQL的主要功能不外乎**增、删、改、查**四个，对于数据分析师来说，只需要掌握**查**就可以了。（因为增删改往往超出了数据分析师的职能范围）

> **注意：**本文是总结性质的，只能提供复习或者速查的功能，讲解得不会很详细，若想学习，还是要在教室内逐章学习。

# SQL简介

SQL是Structured Query Language的简写，也就是**结构化查询语言**。SQL 最受欢迎的功能是与数据库交互。

使用**传统关系数据库**与 SQL 交互有一些主要优点。最明显的 5 个优点是：

- SQL 很容易理解。
- 传统的数据库允许我们直接访问数据。
- 传统的数据库可使我们审核和复制数据。
- SQL 是一个可一次分析多个表的很好工具。
- 相对于 Google Analytics 等仪表板工具，SQL 可使我们分析更复杂的问题。

**为什么企业喜欢使用数据库**

1. 只有输入了需要输入的数据，以及只有某些用户能够将数据输入数据库，才能**保证数据的完整性**。
2. **可以快速访问数据** - SQL 可使我们从数据库中快速获取结果。  可以优化代码，快速获取结果。  
3. **可以很容易共享数据** - 多个人可以访问存储在数据库中的数据，所有访问数据库的用户获得的数据都是一样。

**SQL 与 NoSQL**

你可能听说过 NoSQL，它表示 Not only SQL（不仅仅是 SQL）。使用 NoSQL  的数据库时，你编写的数据交互代码会与本节课所介绍的方式有所不同。**NoSQL  更适用于基于网络数据的环境**，而不太适用于我们现在要介绍的基于电子表格的数据分析。最常用的 NoSQL 语言之一是 [MongoDB](https://www.mongodb.com/)。

# SQL入门

## SQL书写规则

- SQL语句不区分大小写，因此SELECT与select甚至是SeLect的效果是相同的，但是要对命令和变量进行区分，所以默认**命令需要大写，其他内容如变量等则需要小写**；
- 表和变量名中不要出现空格，可使用下划线`_`替代。
- 查询语句中，使用单一空格隔开命令和变量
- 为提高代码的可移植性，请在查询语句结尾添加一个分号`；`

## SQL中的注释

- 行内注释

  使用两个连字符-，添加注释。

  ```sql
  SELECT col_name -- 这是一条注释
  FROM table_name;
  ```

- 多行注释

  多行注释以`/*`起始，以`*/`结尾。

  ```sql
  /*SELECT col_name 
  FROM table_name;*/
  SELECT col_2 
  FROM table_name;
  ```

## 检索数据(SELECT FROM LIMIT )

检索数据主要用的语句为：**SELECT**。

### 检索单列

```sql
SELECT col_name
FROM table_name;
```

从table_name表中检索col_name列。

### 检索多列

```sql
SELECT col_1,col_2,col_3
FROM table_name;
```

从table_name表中检索col_1,col_2和col_3列。

### 检索所有列

```sql
SELECT *
FROM table_name;	
```

使用通配符`*`，返回table_name表中的所有列；

### 检索某列中不同的值

```sql
SELECT DISTINCT col_1
FROM table_name;
```

检索col_1中具有唯一性的行，即唯一值。

### 限制检索的结果

使用LIMIT语句可以限制返回的行数。

```sql
SELECT col_1
FROM table_name
LIMIT 10;
```

返回前10行（即第0-第9行）。

也可以添加OFFSET语句，设置返回数据的起始行：

```sql
SELECT col_1
FROM table_name
LIMIT 10 OFFSET 5;
```

从第五行之后，返回十行数据（即第5-第14行）。

## 排序检索数据(ORDER BY)

- ORDER BY 语句用于根据指定的单列或多列对结果集进行排序。

- ORDER BY 语句默认按照升序对记录进行排序。（从小到大，从a到z）

- 如果您希望按照降序对记录进行排序，可以使用 DESC 关键字。
- 在指定一条ORDER BY子句时，应该保证它是SELECT语句中的最后一条子句。

### 按列排序

```sql
SELECT col_name
FROM table_name
ORDER BY col_name;
```

返回的数据会按照col_name列进行升序排序，这里col_name可以是单列也可以是多列，当然也可以使用非检索的列进行排序。

### 降序排序

```sql
SELECT col_1,col_2
FROM table_name
ORDER BY col_2 DESC,col_3;
```

返回的数据会按照col_2列降序，col_3列升序对col_1和col_2两列进行排序。

> 这里可以看出，DESC关键字的用法：只对跟在语句前面的变量有效。所以，想要对多列进行降序排序时，需要对每一列都指定DESC关键字。

## 过滤数据(WHERE)

- WHERE子句应该在表名（即FROM子句）之后给出。

- WHERE子句应在ORDER BY子句之前。
- 在过滤条件中的value是区分大小写的。

### 使用方法

```sql
SELECT col_1
FROM table_1
WHERE col_1 运算符 value;
```

### 运算符

| 运算符           | 描述                               |
| ---------------- | ---------------------------------- |
| =                | 等于                               |
| <>               | 不等于                             |
| >                | 大于                               |
| <                | 小于                               |
| >=               | 大于等于                           |
| <=               | 小于等于                           |
| BETWEEN...AND... | 在指定的两值之间                   |
| IS NULL          | 为NULL值                           |
| AND              | 逻辑运算符：与                     |
| OR               | 逻辑运算符：或                     |
| IN               | 制定条件范围筛选，可以简化OR的工作 |
| NOT              | 逻辑运算符：非                     |

> 注意：
>
> - SQL的版本不同，可能导致某些运算符不同（如不等于可以用！=表示），具体要查阅数据库文档。
> - 在同时输入AND和OR时，SQL会优先处理AND语句，你可以使用小括号来进行分组操作。

### 用通配符进行过滤（LIKE）

通配符是用来匹配值的一部分的特殊字符，跟在LIKE关键字后面进行数据过滤

| 通配符 | 描述                                       |
| ------ | ------------------------------------------ |
| %      | 表示任何字符出现任意次数                   |
| _      | 表示任何字符出现一次                       |
| []     | 指定一个字符集，它必须匹配该位置的一个字符 |
| ^      | 在[]中使用，表示否定                       |

示例：

```sql
SELECT col_1
FROM table_1
WHERE col_1 LIKE '_[^JM]%'
ORDER BY col_1;
```

如上筛选出的是，第二个字符为非J或M的数据。

## 创建计算字段

其实就是在检索数据的同时进行计算，并使用关键字AS将结果保存为某一列。

- 数值类型的计算

```sql
SELECT prod_id,quantity,item_price,quantity*item_price AS expanded_price
FROM orderitems
WHERE order_num = 200008;
```

输出：

```sql
prod_id         quantity       item_price      expanded_price
--------------------------------------------------------------
RGAN01			5				4.9900			24.9500
BR03			5				11.9900			59.9500
```

这里实现的就是使用quantity*item_price创建一个名为expanded_price的计算字段，也就是一个新列。

同样适用于计算的操作符有`+`（加），`-`（减）和`/`（除）。

- 字符类型的拼接

```sql
SELECT RTRIM(col_name) + '('+RTRIM(col_country)+')' AS col_title
FROM table_name
ORDER BY col_name;
```

输出：

```sql
col_title
------------------------
Bear Emporium(USA)
Bears R Us(USA)
Jouets et ours(France)
```

这里实现的就是将col_name列与col_country列进行了拼接，新列的名字叫做col_title。

> RTRIM()函数是去掉右边的所有空格，LTRIM()是去掉左边的所有空格，TRIM()是去掉两边的所有空格。

## 使用别名

在上一节中我们使用AS来为变量设置别名，你可能也见过如下所示的语句：

```sql
SELECT col1 + col2 AS total, col3
```

当然没有 **AS** 的语句也可以实现使用别名：

```sql
FROM tablename t1
```

以及

```sql
SELECT col1 + col2 total, col3
```

将col1+col2的结果设置名为total的列。

## 代码总结

| **语句** | **使用方法**                    | **其他详细信息**                             |
| -------- | ------------------------------- | -------------------------------------------- |
| SELECT   | SELECT **Col1**, **Col2**, …    | 提供你需要的列                               |
| FROM     | FROM **Table**                  | 提供列所在的表格                             |
| LIMIT    | LIMIT **10**                    | 限制返回的行数                               |
| ORDER BY | ORDER BY **Col**                | 根据列命令表格。与 **DESC** 一起使用。       |
| WHERE    | WHERE **Col > 5**               | 用于过滤结果的一个条件语句                   |
| LIKE     | WHERE **Col LIKE ‘%me%’**       | 仅提取出列文本中具有 ‘me’ 的行               |
| IN       | WHERE **Col IN (‘Y’, ‘N’)**     | 仅过滤行对应的列为 ‘Y’ 或 ‘N’                |
| NOT      | WHERE **Col NOT IN (‘Y’, “N’)** | **NOT** 经常与 **LIKE** 和 **IN** 一起使用。 |
| AND      | WHERE **Col1 > 5 AND Col2 < 3** | 过滤两个或多个条件必须为真的行               |
| OR       | WHERE **Col1 > 5 OR Col2 < 3**  | 过滤一个条件必须为真的行                     |
| BETWEEN  | WHERE **Col BETWEEN 3 AND 5**   | 一般情况下，语法比使用 **AND** 简单一些      |

# SQL进阶

## 链接表

### 基本链接（JOIN）

SQL最强大的功能之一就是能在数据查询的执行中进行表的链接（JOIN）。

在关系数据库中，将数据分解为多个表能更有效地存储，更方便地处理，但这些数据储存在多个表中，怎样用一条SELECT语句就检索出数据呢？那就要使用链接。

创建链接的方式很简单，如下便是使用WHERE创建链接：

```sql
SELECT col_1,col_2,col_3
FROM table_1,table_2
WHERE table_1.id = table2.id;
```

如上，col_1和col_2属于table_1表中，col_3属于table_2表中，而这两个表使用相同的id列进行匹配。这种方法被称为等值链接，也就是内链接，我们可以使用如下的语句，更直观地实现内连接：

```sql
SELECT col_1,col_2,col_3
FROM table_1 INNER JOIN table_2
ON table_1.id = table2.id;
```

当然你也可以使用别名，简化输入，并且标明各列与表的隶属关系：

```sql
SELECT t1.col_1,t1.col_2,t2.col_3
FROM table_1 t1 INNER JOIN table_2 t2
ON t1.id = t2.id;
```

如上代码同样适用于左链接、右链接和外链接：

- **LEFT JOIN** - 用于获取 **FROM** 中的表格中的所有行，即使它们不存在于 **JOIN** 语句中。

- **RIGHT JOIN** - 用于获取 **JOIN** 中的表格中的所有行，即使它们不存在于 **FROM** 语句中。
- **FULL JOIN**: 只要其中一个表中存在匹配，就返回行。

### 自链接

自链接经常用于对子查询的简化，如下示例：

假如要给Jim同一公司的所有顾客发送一封邮件，需要你先筛选出Jim的公司，然后再根据该公司筛选出所有的顾客。使用子查询的方式如下：

```sql
SELECT cust_id,cust_name,cust_contact
FROM customers
WHERE cust_name = (SELECT cust_name
                  FROM customers
                  WHERE cust_contact = 'Jim')
```

如果改为自链接的方式如下：

```sql
SELECT c1.cust_id,c1.cust_name,c1.cust_contact
FROM customers c1,customers c2
WHERE c1.cust_name = c2.cust_name
AND c2.cust_name = 'Jim';
```

结果是一样的，但是使用自链接的处理速度比子查询要快得多。

### 组合查询（UNION）

UNION 操作符用于合并两个或多个 SELECT 语句的结果集，使用方法也很简单，只要在多条SELECT语句中添加UNION关键字即可。

> 多数情况下，组合相同表的多个查询所完成的任务与具有多个WHERE子句的一个查询是一样的。

**注意**：UNION 内部的 SELECT 语句必须拥有相同数量的列，列也必须拥有相似的数据类型。而且UNION返回的结果只会选取不同的值（即唯一值）。

使用UNION的场合情况：

- 在一个查询中从不同的表返回结果；
- 对一个表执行多个查询返回结果。

示例：如下三个语句的结果是一致的。

- 原始语句

```sql
-- 查询一
SELECT cust_name,cust_email
FROM customers
WHERE cust_state IN ('str1','str2');

--查询二
SELECT cust_name,cust_email
FROM customers
WHERE cust_name = 'str3';
```

- 使用UNION链接

```sql
SELECT cust_name,cust_email
FROM customers
WHERE cust_state IN ('str1','str2')
UNION
SELECT cust_name,cust_email
FROM customers
WHERE cust_name = 'str3'
ORDER BY cust_name;
```

在最后添加了ORDER BY对所有SELECT语句进行排序，这里只是为了示例在使用UNION时如何进行排序。

- 使用WHERE

```sql
SELECT cust_name,cust_email
FROM customers
WHERE cust_state IN ('str1','str2')
OR cust_name = 'str3';
```

这里看起来使用UNION比WHERE更复杂，但对于较复杂的筛选条件，或者从多个表中检索数据时，使用UNION更简单一些。

- **UNION ALL** 命令和 UNION 命令几乎是等效的，不过 UNION ALL 命令会列出所有的值。

## SQL聚合

有时候我们只是需要获取数据的汇总信息，比如说行数啊、平均值啊这种，并不需要吧所有数据都检索出来，为此，SQL提供了专门的函数，这也是SQL最强大功能之一。

### 聚合函数

SQL的聚合函数如下所示：

| 函数    | 说明             |
| ------- | ---------------- |
| AVG()   | 返回某列的均值   |
| COUNT() | 返回某列的行数   |
| MAX()   | 返回某列的最大值 |
| MIN()   | 返回某列的最小值 |
| SUM()   | 返回某列的和     |

使用示例：

```sql
SELECT AVG(col_1) AS avg_col_1
FROM table_1;
```

> **注意：**聚合函数都会忽略列中的NULL值，但是COUNT(*)也就是统计全部数据的行数时，不会忽略NULL值。

### 聚合不同值

当添加DISTINCT参数时，就可以只对不同值（也就是某列中的唯一值）进行函数操作。

使用示例：

```sql
SELECT AVG(DISTINCT col_1) AS avg_col_1
FROM table_1;
```

## 数据分组

### 创建分组（GROUP BY）

前面的函数操作都是基于整个表去进行的，那如果想要依据某列中的不同类别（比如说不同品牌 不同性别等等）进行分类统计时，就要用到数据分组，在SQL中数据分组是使用GROUP BY子句建立的。

在使用GROUP BY时需要注意的几点：

- GROUP BY子句可以包含任意数量的列，因而可以对分组进行多重嵌套，类似于Pandas中的多重索引；
- GROUP BY子句必须出现在WHERE子句之后，ORDER BY之前。

使用示例：

```sql
SELECT col_1,COUNT(*) AS num_col
FROM table_1
GROUP BY col_1;
```

以上即可实现按col_1列中的不同类目进行行数统计。

### 过滤分组（HAVING）

在SQL入门中我们学过WHERE，它是对行数据进行筛选过滤的，那么，如果我想对创建的分组数据进行筛选过滤呢？这时候，你就要用到HAVING子句了，它与WHERE的操作符一致，只是换了关键字而已。

使用示例：

```sql
SELECT col_1,COUNT(*) AS num_col
FROM table_1
GROUP BY col_1
HAVING COUNT(*) >= 2;
```

这里我们就筛选出了具有两个以上类别的分组。

**注意：**使用HAVING时应该结合GROUP BY子句。

## 时间序列的处理（DATE）

在SQL中有一套专门的内置函数，用来处理时间序列，那就是DATE函数。

### SQL Date 数据类型

先了解一下在不同的数据库中的时间序列的表示。（了解即可）

**MySQL** 使用下列数据类型在数据库中存储日期或日期/时间值：

- DATE - 格式：YYYY-MM-DD
- DATETIME - 格式：YYYY-MM-DD HH:MM:SS
- TIMESTAMP - 格式：YYYY-MM-DD HH:MM:SS
- YEAR - 格式：YYYY 或 YY

**SQL Server** 使用下列数据类型在数据库中存储日期或日期/时间值：

- DATE - 格式：YYYY-MM-DD
- DATETIME - 格式：YYYY-MM-DD HH:MM:SS
- SMALLDATETIME - 格式：YYYY-MM-DD HH:MM:SS
- TIMESTAMP - 格式：唯一的数字

### DATE_TRUNC函数

**DATE_TRUNC** 使你能够将日期截取到日期时间列的特定部分。常见的截取依据包括`日期`、`月份` 和 `年份`。

语法：

```sql
DATE_TRUNC('datepart', timestamp)
```

其中`datepart`即为你的截取依据，后面的timestamp类型可以参考上面的Date数据类型。

>  我总结了一份SQL的`datepart`速查表放在了下面。

使用示例：

```sql
SELECT DATE_TRUNC('y',col_date) col_year
FROM table_1
GROUP BY 1
ORDER BY 1 DESC
LIMIT 10;
```

如上，我们将col_date列按照年（'y'）进行了分组，并按由大至小的顺序排序，取前10组数据。

### DATE_PART函数

**DATE_PART** 可以用来获取日期的特定部分，如获取日期2018-10-6的月份，只会获得一个结果6，这是它与DATE_TRUNC的最大区别。

语法：

```sql
DATE_PART ('datepart', date或timestamp)
```

其中`datepart`即为你的截取依据，后面的timestamp类型可以参考上面的Date数据类型。

使用示例：

```sql
SELECT DATE_PART('y',col_date) col_year
FROM table_1
GROUP BY 1;
```

如上，我们筛选了col_date列的年份，并依据它做了分组。

> 想了解更多DATE函数，可以戳[SQL日期和时间函数参考](https://docs.aws.amazon.com/zh_cn/redshift/latest/dg/Date_functions_header.html)

### datepart总结

如下给了很多的缩写，只记住最简单的即可。

| 日期部分或时间部分                | 缩写                                                         |
| --------------------------------- | :----------------------------------------------------------- |
| 世纪                              | c、cent、cents                                               |
| 十年                              | dec、decs                                                    |
| 年                                | y、yr、yrs                                                   |
| 季度                              | qtr、qtrs                                                    |
| 月                                | mon、mons                                                    |
| 周                                | w，与 **DATE_TRUNC**一起使用时将返回离时间戳最近的一个星期一的日期。 |
| 一周中的日 （ **DATE_PART**支持） | dayofweek、dow、dw、weekday                                                                                                              返回 0–6 的整数（星期日是0，星期六是6）。 |
| 一年中的日 （ **DATE_PART**支持） | dayofyear、doy、dy、yearday                                  |
| 日                                | d                                                            |
| 小时                              | h、hr、hrs                                                   |
| 分钟                              | m、min、mins                                                 |
| 秒                                | s、sec、secs                                                 |
| 毫秒                              | ms、msec、msecs、msecond、mseconds、millisec、millisecs、millisecon |

## CASE语句

CASE语句其实就相当于python中的if语句，是用来做条件的。

需要注意的几点：

- CASE 语句始终位于 SELECT 条件中。
- CASE 必须包含以下几个部分：WHEN、THEN 和 END。ELSE 是可选组成部分，用来包含不符合上述任一 CASE 条件的情况。
- 你可以在 WHEN 和 THEN 之间使用任何条件运算符编写任何条件语句（例如 WHERE），包括使用 AND 和 OR 连接多个条件语句。
- 你可以再次包含多个 WHEN 语句以及 ELSE 语句，以便处理任何未处理的条件。

使用示例：

```sql
SELECT account_id, CASE WHEN standard_qty = 0 OR standard_qty IS NULL THEN 0
                        ELSE standard_amt_usd/standard_qty END AS unit_price
FROM orders
LIMIT 10;
```

如上，我们使用CASE WHEN.(条件一).THEN.(条件一的结果).ELSE.(其他不符合条件一的结果).END语句，设立的两个条件，即当standard_qty为0或者不存在时我们返回0，当standard_qty不为0时进行计算，并储存为新列unit_price。

## 子查询与临时表格

我们之前所涉及到的都是从数据库表中检索数据的单条语句，但当我们想要检索的数据并不能直接从数据库表中获取，而是需要从筛选后的表格中再度去查询时，就要用到子查询和临时表格了。

子查询与临时表格所完成的任务是一致的，只不过子查询是通过嵌套查询完成，而另一种是通过WITH创建临时表格进行查询。

### 构建子查询

构建子查询十分简单，只需将被查询的语句放在小括号里，进行嵌套即可，但在使用时一定要注意格式要清晰。

使用示例：

```sql
SELECT *
FROM (SELECT DATE_TRUNC('day',occurred_at) AS day,channel, COUNT(*) AS events
      FROM web_events 
      GROUP BY 1,2
      ORDER BY 3 DESC) sub
GROUP BY channel
ORDER BY 2 DESC;
```

如上，我们创建了一个子查询，放在小括号里，并将其命名为sub。在子查询中也注意到了各个子句上下对齐，这样条例更清晰。

### 临时表格（WITH）

这种方法，就是使用WITH将子查询的部分创建为一个临时表格，然后再进行查询即可。

我们还是使用上面子查询的例子，这次用临时表格的形式实现：

```sql
WITH sub AS(
SELECT DATE_TRUNC('day',occurred_at) AS day,channel, COUNT(*) AS events
FROM web_events 
GROUP BY 1,2
ORDER BY 3 DESC)

SELECT *
FROM sub
GROUP BY channel
ORDER BY 2 DESC;
```

如上，我们将被嵌套的子查询单独拎出来，用WITH创建了一个临时表格，再之后又使用SELECT根据该表格进行查询。

## SQL数据清理

这一节主要针对数据清理讲解了几个SQL中的常用函数，一般来说，也都是用在筛选阶段，更详尽的数据清理还是要放在python中去进行。

### 字符串函数

- **LEFT、RIGHT、LENGTH**

LEFT和RIGHT相当于是字符串截取，**LEFT** 是从左侧起点开始，从特定列中的每行获取一定数量的字符，而**RIGHT**是从右侧。

**LENGTH**就是获取字符串的长度，相当于python中的len()。

语法：

```sql
LEFT(phone_number, 3) -- 返回从左侧数，前三个字符
RIGHT(phone_number, 8)
LENGTH(phone_number) 
```

- **POSITION**、**STRPOS**、**SUBSTR**

这三个函数都是与**位置**相关的函数。

**POSITION** 和**STRPOS** 可以获取某一字符在字符串中的位置，这个位置是从左开始计数，最左侧第一个字符位置为1，但他俩的语法稍有不同。

**SUBSTR**可以筛选出指定位置后指定数量的字符。

语法：

```sql
POSITION(',' IN city_state)
STRPOS(city_state, ‘,’) --跟上面的语句等价
SUBSTR(city_state,4,5) -- 返回city_state字符串中，以第4个字符开头的5个字符。
```

- **字符串拼接（CONCAT）**

顾名思义，就是将两个字符串进行拼接。

语法：

```sql
CONCAT(first_name, ' ', last_name) -- 结果为：first_name last_name
--或者你也可以使用双竖线来实现上述任务
first_name || ' ' || last_name
```

### 更改数据格式

- TO_DATE函数

TO_DATE函数可以将某列转为DATE格式，主要是将单独的月份或者年份等等转换为SQL可以读懂的DATE类型数据。

语法：

```sql
TO_DATE(col_name,'datepart') 
TO_DATE('02 Oct 2001', 'DD Mon YYYY');
```

这里是将col_name这列按照datepart转化为DATE类型的数据，datepart可以参考之前的总结。

- CAST函数

CAST函数是SQL中进行数据类型转换的函数，但经常用于将字符串类型转换为时间类型。

语法：

```sql
CAST(date_column AS DATE)
-- 你也可以写成这样
date_column::DATE
```

这里是将date_column转换为DATE格式的数据，其他时间相关的数据类型与样式对照可以参考上面写过的**SQL Date数据类型**，确保你想转换的数据样式与数据类型对应。

### 缺失值的处理

之前有提到过如何筛选出缺失值，即使用**WHERE**加上**IS NULL**或者**IS NOT NULL**。

那么如何对缺失值进行处理呢？（其实这里可以直接无视，筛选出来后在python中再进行处理）

SQL中提供了一个替换NULL值的函数**COALESCE**。

使用示例：

```sql
COALESCE(col_1,0) -- 将col_1中的NULL值替换为0
COALESCE(col_2,'no DATA') -- 将col_2中的NULL值替换为no DATA
```

# 总结

好啦，至此课程中的所有SQL知识点已经总结完了，并且也给大家做了适当的补充，希望大家能够用得上。未来的数据分析师之路，还要继续加油呀！

![](http://wx4.sinaimg.cn/bmiddle/006Cmetyly1fmtcyy5h20j30dw0dwaiw.jpg)

# 附：SELECT子句顺序

下表中列出了全文中涉及到的子句，在进行使用时，应严格遵循下表中从上至下的顺序。

| 子句         | 说明               | 是否必须使用             |
| ------------ | ------------------ | ------------------------ |
| SELECT       | 要返回的列或表达式 | 是                       |
| FROM         | 用于检索数据的表   | 仅在从表中选择数据时使用 |
| JOIN...ON... | 用于链接表         | 仅在需要链接表时使用     |
| WHERE        | 过滤行数据         | 否                       |
| GROUP BY     | 分组数据           | 仅在按组计算时使用       |
| HAVING       | 过滤分组           | 否                       |
| ORDER BY     | 对输出进行排序     | 否                       |
| LIMIT        | 限制输出的行数     | 否                       |



