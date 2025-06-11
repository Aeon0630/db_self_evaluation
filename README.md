# 第一周：

# 第二周：关系模式
## 学习内容
### 1.关系数据库
- 关系：元组（无序性、不可重复性）、属性
- 关系模式
### 2.超码、候选码、主码、外码之间的区别与联系
### 3.关系代数


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
  - is null
  - like 字符串匹配符：% 匹配任意多个字符，_ 匹配单个字符（转义字符：\）
- order by 语句
  ```
  ORDER BY column1 DESC, column2 ASC
  ```
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
函数名() OVER (
    PARTITION BY column1,colum2,...
    ORDER BY column1,colum2,...
)
- PARTITION BY：分组列
- ORDER BY：窗口内的排序规则
- 常见的窗口函数
  - 聚合函数（列名）
  - 排名函数：rank、dense_rank、row_number


# 第八周：子查询



# 第九周：关系模式的改变



# 第十周：表连接



# 第十一周：高级数据类型



# 第十二周：E-R图



# 第十三周：关系数据库范式


