# 第一周：
将postgresql、datagrip、mysql、mysql workbench下站安装到本地，熟悉软件界面的基本操作


# 第二周：关系模式
## 学习内容
### 1.关系数据库
掌握关系、元组、属性、域等基本概念
### 2.码
掌握超码、候选码、主码、外码之间的区别与联系
### 3.关系代数
掌握交、并、补、笛卡尔积、连接、选择、投影等基本运算


# 第三-六周：数据库中基本的增、删、改、查语句
## 学习内容
### 1.在数据库中创建关系
- create table 语句
  ```
  CREATE TABLE sheet_name(
   column1   type1,
   column2   type2,
   ...............
   PRIMARY KEY (column1,...),
   FOREIGN KEY (column1,...) REFERENCES sheet_name2(column1,...),
   UNIQUE (column1,...),
   CHECK (condition1,...));
  ```
  - 数据类型：数值型（int,small int,float(n),numeric(p,d),real）、字符串型（char(n),varchar(n),text）
  - 对于每列数据，可加PRIMARY KEY、FOREIGN KEY、UNIQUE、CHECK、not null、default等进行限制
  - 自增id：serial
### 2.查询语句
- select 语句
  ```
  SELECT column1,column2,.../*
  ```
  - 列之间可进行数值运算
  - 对列重新命名：as
  - 去重：distinct
- from 语句
  ```
  FROM table
  ```
- where 语句
  ```
  WHERE condition
  ```
  - 基本筛选符：>、>=、<、<=、=、！=
  - between...and...
  - in.../not in...
  - is (not) null  注意：不能用=
  - like 字符串匹配符：% 匹配任意多个字符，_ 匹配单个字符（转义字符：\）
- order by 语句
  ```
  ORDER BY column1 DESC, column2 ASC
  ```
  - 排除null：在where语句中加入is not null
  - null排最后：nulls last
- limit 语句
  ```
  LIMIT 取值数 OFFSET 跳过数
  ```
### 3.增加语句
```
INSERT INTO table (column1,column2,...)
VALUES (value1),(value2),...
```
### 4.修改语句
```
UPDATE table
SET column1 = ..., column2 = ...
```
### 5.删除语句
```
DELET FROM table
WHERE condition
```
DELET FROM table:清空表     TRUNCATE table:清空表     DROP table:删除表
### 6.集合操作
UNION：并   INTERSECT：交   EXCEPT：差
### 7.NULL部分
- NULL参与数值运算返回结果为空
- NULL与其他值进行比较得到的逻辑状态为unknown，unknown逻辑状态的数据在查询中不会显示
- NULL的填充：COALESCE(...,...) （返回第一个非空值）

## 总结与反思
- 这部分内容相对基础，重点掌握查询语句的应用


# 第七周：聚合函数
## 学习内容
### 1.聚合函数
MAX、MIN、SUM、AVG、COUNT
```
SELECT aggregate_fun(column)
```
- 聚合函数一般会忽略掉null值，但COUNT(*)统计所有的行数
### 2.group by 语句
```
GROUP BY column1,column2,...
```
- 当select语句中有聚合函数时，对筛选数据按某一标准分组
- 位于where语句之后，order by语句之前
- 包含所有的非聚合列
### 3.having 语句
```
HAVING condition
```
- 当select语句中有聚合函数时，对分组后的数据再次进行筛选
- 位于group by语句之后，order by语句之前
  
## 补充学习
### 1.窗口函数
```
函数名() OVER (
    PARTITION BY column1,colum2,...
    ORDER BY column1,colum2,...
)
```
- PARTITION BY：分组列
- ORDER BY：窗口内的排序规则
- 常见的窗口函数
  - 聚合函数（列名）
  - 排名函数：rank、dense_rank、row_number

## 总结与反思
- where 语句和having 语句：where和having可根据具体的业务逻辑与代码可读性来选择，一般来说where针对全局，而having针对局部
- group by 语句的使用：初次使用聚合函数时，常会忘记group by语句，对分组聚合的逻辑理解还不够深刻

# 第八周：子查询
## 学习内容
### 1.from 语句中的子查询
```
FROM (subquery)
```
### 2.select 语句中的子查询
```
SELECT (subquery)
```
- 标量子查询
- 相关子查询
### 3.where/having 语句中的子查询
```
WHERE column 筛选符 (subquery)   # 筛选符包括基本筛选符、IN等
WHERE column 筛选符 ANY/ALL(subquery)
WHERE EXISTS(subquery)
```
- 标量子查询
- 相关子查询
- EXISTS子查询

## 总结与反思
- ANY与ALL：ANY为任一匹配，ALL为全部匹配
- 相关子查询：可以把相关子查询看做一个循环语句（类比python中的for循环），即将外表（from语句中的表）中的数据逐条进行匹配
- EXISTS：本质上为判断语句，也可将之看为一个循环，对于外表（from语句中的表）中的数据逐条进行判断，符合判断条件则返回相应数据
- EXISTS与IN：当IN中子查询的返回数据过多时，建议用EXISTS代替，提高运行效率
- 子查询部分的逻辑较为复杂，在使用子查询时，可以先写内层再写外层，即先把所需要的子查询写出来，再将其嵌套在外层的查询中


# 第九周：关系模式的改变
## 学习内容
### 1.alter table语句的用法
- 增加列：add column
- 修改表名：rename to
- 修改列名：alter column ... to ...
- 修改数据类型：alter column ... type ...
- 修改默认值：alter column ... set default
- 删除默认值：alter column ... drop default
- 删除列：drop column ...

## 补充学习
### 1.alter table语句更多用法
- 增加主键：add constraint ... primary key (column1,..)
- 增加唯一约束：add constraint ... unique (column1,...)
- 增加约束：add constraint ... check (condition)


# 第十周：表连接
## 学习内容
### 1.自然连接
```
sheet1 NATURAL JOIN sheet2
```
自动匹配所有同名列
### 2.内连接
```
sheet1 INNER JOIN sheet2 ON sheet1.col1 = sheet2.col2
```
返回两表中匹配的行
### 3.外连接
```
sheet1 LEFT/RIGHT OUTER JOIN sheet2 ON sheet1.col1 = sheet2.col2
```
返回左（右）表所有行 + 右（左）表匹配行，右（左）表无匹配用 NULL填充
### 4.全连接
```
sheet1 FULL OUTER JOIN sheet2 ON sheet1.col1 = sheet2.col2
```
返回左表和右表的所有行，无匹配项用 NULL 填充

## 补充学习
### 1.自连接
在同一张表中可以根据不同的列对表进行自连接

## 总结与反思
- 自然连接是连接条件，不是连接类型
- 自然连接存在一定风险，尽量避免使用
- 当所连接的属性有相同的名称时，可用using代替on
- 内连接、外连接、全连接的选择，根据业务逻辑来判断
- 大部分表连接的内容均可通过子查询或where语句中的多条件来实现，何时选择用表连接，主要根据代码的可读性和逻辑上的简单性来考虑


# 第十一周：高级数据类型
## 学习内容
### 1.日期、时间型
掌握date、time、timestamp数据类型及date、time的相关运算（interval）
### 2.自定义类型
```
GREATE TYPE 类型名 AS ...
```
### 3.数据类型的转换
- ```
  CAST(数据 AS 新类型)
  ```
- 数据::新类型

## 补充学习
### 1.日期时间函数
- 获取当前日期时间：current_date、current_time、current_timestamp
- 提取日期时间：extract (... from ...)
- 格式化日期时间：to_char(...,'')


# 第十二周：E-R图
## 学习内容
### 1.ER模型
- 实体（集）、联系（集）、映射基数、基数约束
- 绘制E-R图
- 实体集、弱实体集、联系集的主码
### 2.将E-R图转化为关系模式
- 强实体集、弱实体集、联系集所包含的属性

## 总结与反思
- 当E-R图中有多个实体集时，映射基数可没有固定的写法
- E-R图章节的学习强化了我对表连接部分知识的理解。其中联系集相当于多表连接得到的结果，而映射基数、基数约束、各集合的主码也一定程度上反应了多表的连接方式


# 第十三周：关系数据库范式
## 学习内容
### 1.函数依赖
掌握函数依赖、平凡依赖、依赖闭包、无损分解等概念，学会了找出一个关系模式中的函数依赖
### 2.BCNF和3NF
- 学会了判断一个关系模式是否为BCNF范式、3NF范式
- 掌握BCNF分解算法

## 总结与反思
与之前的学习内容相比，这部分内容更偏理论。刚学习函数依赖部分的内容时觉得很抽象，但通过多次阅读、理解定义，逐渐明白了其所描述的内容；对于BCNF范式和3NF范式部分，结合具体的题目也很好地理解了其定义和BCNF分解算法。
- BCNF分解不一定依赖保持，但在用BCNF分解算法时，要尽量保证在分解的过程中用到原关系中所有的依赖（即在分解的过程中选择合适的函数依赖开始分解）


# 第十四-十五周：


