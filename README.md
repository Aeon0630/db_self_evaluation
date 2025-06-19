# 数据库原理与应用
- 记录学习内容与收获
- [作业](https://github.com/Aeon0630/db_homework)
- [相关练习](https://www.nowcoder.com/exam/oj?page=1&tab=SQL%E7%AF%87&topicId=199)

# 第一周：软件下载安装
了解数据库基本知识，将postgresql、datagrip、mysql、mysql workbench下站安装到本地，熟悉软件界面的基本操作（如创建数据库、导入数据表、查看数据表基本信息等）。


# 第二周：关系模式
## 学习内容与收获
### 1.关系数据库
掌握关系、元组、属性、域等基本概念，将关系型数据库与excel中的表格进行类比。其中关系可以看为元组的集合，且元组具有无序性和不可重复性。
### 2.码
掌握超码、候选码、主码、外码之间的区别与联系。
### 3.关系代数
掌握交、并、差、笛卡尔积、连接、选择、投影等基本运算。其中交、并、差要求各个关系之间有相同的关系模式。


# 第三-六周：数据库中基本的增、删、改、查语句
## 学习内容与收获
### 1.在数据库中创建关系
- create table 语句
  ```ruby
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
  - 对于每列数据，可加PRIMARY KEY、REFERENCES sheet_name2(column1,...)、UNIQUE、CHECK(condition)、not null、default ... 等进行限制
  - 对于有外码的情况
    - 默认禁止删除被参照关系中所对应的值
    - 级联删除：ON DELETE CASCAVE
    - 设为NULL：ON DELETE NULL
    - 设为默认值：ON DELETE SET DEFAULT
  - 自增id：serial
### 2.查询语句
- select 语句
  ```ruby
  SELECT column1,column2,...
  SELECT *  # 筛选所有列
  ```
  - 列数据可进行运算
  - 对列重新命名：as
  - 去重：distinct
- from 语句
  ```ruby
  FROM table
  ```
- where 语句
  ```ruby
  WHERE condition（条件语句）
  ```
  - 比较运算符：>、>=、<、<=、=、！=
  - between...and...
  - in.../not in...
  - is (not) null  注意：不能用=
  - like 字符串匹配符：% 匹配任意多个字符，_ 匹配单个字符（转义字符：\）
  - 条件中可包含from表中的所有列
- order by 语句
  ```ruby
  ORDER BY column1 DESC, column2 ASC
  ```
  - 一般情况默认null最大
  - 排除null：在where语句中加入is not null条件
  - null排最后：在order by语句最后加入nulls last
  - 可包含from表中的所有列和select语句中的聚合列
- limit 语句
  ```ruby
  LIMIT 取值数 OFFSET 跳过数
  ```
### 3.增加语句
```ruby
INSERT INTO table (column1,column2,...)
VALUES (value1),(value2),...
```
### 4.修改语句
```ruby
UPDATE table
SET column1 = ..., column2 = ...
```
### 5.删除语句
```ruby
DELET FROM table
WHERE condition
```
DELET FROM table:清空表     TRUNCATE TABLE table:清空表     DROP TABLE table:删除表
### 6.集合操作
UNION：并   INTERSECT：交   EXCEPT：差
### 7.NULL部分
- NULL参与数值运算返回结果为空
- NULL与其他值进行比较得到的逻辑状态为unknown，unknown逻辑状态的数据在查询中不会显示
- NULL的填充：COALESCE(列名,...,...) （返回第一个非空值）


# 第七周：聚合函数
## 学习内容与收获
### 1.聚合函数
MAX、MIN、SUM、AVG、COUNT
```ruby
SELECT aggregate_fun(column)
```
- 聚合函数一般会忽略掉null值，但COUNT(*)统计所有的行数
### 2.group by 语句
```ruby
GROUP BY column1,column2,...
```
- 对筛选数据按某一标准分组
- 位于where语句之后，order by语句之前
- 当select语句中包含聚合列和非聚合列时，group by语句中需包含所有的非聚合列
- 当select语句中只包聚合列时，group by可不存在
- 当select语句中只包含非聚合列时，group by语句中需包含所有的非聚合列（除了只查询单列以外，其他时候无意义）
### 3.having 语句
```ruby
HAVING condition
```
- 对分组后的数据再次进行筛选
- 位于group by语句之后，order by语句之前
- having语句中的条件可包含select语句中的类名和聚合式，以及参与表中其他的聚合式
  
## 补充学习
### 1.窗口函数
```ruby
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
## 学习内容与收获
### 1.from 语句中的子查询
```ruby
FROM (subquery)
```
### 2.select 语句中的子查询
```ruby
SELECT (subquery)
```
- 标量子查询
- 相关子查询
### 3.where/having 语句中的子查询
```ruby
WHERE column 筛选符 (subquery)   # 筛选符包括基本的比较运算符、IN等
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
## 学习内容与收获
### 1.alter table语句的用法
```ruby
增加列：
ALTER TABLE sheet_name ADD COLUMN col1,...
修改表名：
ALTER TABLE sheet_name RENAME TO new_sheet_name
修改列名：
ALTER TABLE sheet_name RENAME COLUMN col TO new_col
修改数据类型：
ALTER TABLE sheet_name ALTER COLUMN col TYPE
修改默认值：
ALTER TABLE sheet_name ALTER COLUMN col SET DEFAULT ...
删除默认值：
ALTER TABLE sheet_name ALTER COLUMN col DROP DEFAULT
删除列：
ALTER TABLE sheet_name DROP COLUMN col1,...   # 被其他关系外码所引用时，不可随意删除，采用级联删除（在最后加上CASCADE）
```

## 补充学习
### 1.alter table语句更多用法
```ruby
增加主键：
ALTER TABLE sheet_name ADD CONSTRAINT pk_sheet_name PRIMARY KEY (col1,...)
增加唯一键约束：
ALTER TABLE sheet_name ADD CONSTRAINT unique_col UNIQUE(col)
增加约束：
ALTER TABLE sheet_name ADD CONSTRAINT check_col CHECK(condition)
```


# 第十周：表连接
## 学习内容与收获
### 1.自然连接
```ruby
sheet1 NATURAL JOIN sheet2
```
自动匹配所有同名列
### 2.内连接
```ruby
sheet1 INNER JOIN sheet2 ON sheet1.col1 = sheet2.col2
```
返回两表中匹配的行
### 3.外连接
```ruby
sheet1 LEFT/RIGHT OUTER JOIN sheet2 ON sheet1.col1 = sheet2.col2
```
返回左（右）表所有行 + 右（左）表匹配行，右（左）表无匹配用 NULL填充
### 4.全连接
```ruby
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
## 学习内容与收获
### 1.日期、时间型
掌握date、time、timestamp数据类型及date、time的相关运算（通过interval实现）
### 2.自定义类型
```ruby
CREATE TYPE 类型名 AS ...
```
### 3.数据类型的转换
- ```ruby
  CAST(数据 AS 新类型)
  ```
- 数据::新类型

## 补充学习
### 1.日期时间函数
- 获取当前日期时间：current_date、current_time、current_timestamp
- 提取日期时间：extract (... from ...)
- 格式化日期时间：to_char(...,'')


# 第十二周：E-R图
## 学习内容与收获
### 1.ER模型
- 掌握了实体（集）、联系（集）、属性及其分类等概念
- 学会了E-R图的绘制，其中实体集用方框表示、联系集用菱形表示，实体集之间的联系方式通过映射基数和基数约束表示
- 学会了判断实体集、弱实体集、联系集各自的主码
### 2.将E-R图转化为关系模式
- 学会了判断强实体集、弱实体集、联系集转化为对应关系模式后所包含的属性；需要注意的是若映射基数为one-to-one，则两个实体集对应的关系模式中均包含外码，若映射基数为one-to-many，则many实体集对应的关系模式中包含外码

## 总结与反思
- 映射基数的表示、联系集的主码、强实体集转化为关系模式均与映射基数相关；其中对于一对多和多对一这两种映射基数，“一”中的元组可与“多”中多个元组产生联系，而“多”中的元组只能与“一”中的一个元组产生联系
- 当E-R图中有多个实体集时，映射基数可没有固定的写法
- E-R图章节的学习强化了我对表连接部分知识的理解。其中联系集相当于多表连接得到的结果，而映射基数、基数约束、各集合的主码也一定程度上反应了多表的连接方式


# 第十三周：关系数据库范式
## 学习内容与收获
### 1.函数依赖
掌握了函数依赖、平凡依赖、依赖闭包、无损分解等概念，学会了找出一个关系模式中的函数依赖
### 2.1NF、2NF、3NF和BCNF
- 1NF范式：所有的属性均为简单属性（保证了原子性），不满足1NF的数据库不是关系型数据库
- 2NF范式：满足1NF范式的同时，非主键属性需完全依赖于主键属性，消除部分依赖、减少冗余
- 3NF范式：满足2NF范式的同时，非主键属性需直接依赖于主键属性，消除传递依赖、进一步减少冗余
  ```
  对于函数依赖α→β，下面至少一个成立
  - α→β为平凡依赖
  - α为超码
  - β-α的每个属性均属于某个候选码
  ```
- BCNF范式：满足3NF范式的同时，所有属性都不依赖于候选码
  ```
  对于函数依赖α→β，下面至少一个成立
  - α→β为平凡依赖
  - α为超码
  ```
- BCNF分解算法：从不满足BCNF范式的函数依赖开始分解

## 总结与反思
与之前的学习内容相比，这部分内容更偏理论。刚学习函数依赖部分的内容时觉得很抽象，但通过多次阅读、理解定义，逐渐明白了其所描述的内容；对于各种范式的判定和BCNF分解算法，结合具体的示例也对这部分有了较好的理解。
- 在函数依赖中判断是否为超码：若其闭包包含所有属性，则为超码
- BCNF分解不一定依赖保持，但在用BCNF分解算法时，要尽量保证在分解的过程中用到原关系中所有的依赖（即在分解的过程中选择合适的函数依赖开始分解）


# 第十四-十五周：存储、索引、查询优化及事物
## 学习内容与收获
### 1.存储
- DBMN存储逻辑：文件——页——数据
- 页：了解页的定义、组织方式、结构（header+data）和布局（空列表、变长数据的标记、分页的槽结构）
- 存储模型：行存储，适用于在线事物处理（OLTP）；列存储，适用于在线分析处理（OLAP）
- 数据字典：通过查询语句——数据字典——文件/页——读取数据的机制实现查询语句
- 大对象存储：要求单个记录（元组）的大小不能超过页的大小，一般可通过溢出页和外部存储引用的方法解决此问题
### 2.索引
- 有序索引：区分聚集索引和非聚集索引、稠密索引和稀疏索引；其中聚集与非聚集索引区分的是索引顺序与存储顺序之间的关系，稠密与稀疏索引区分的是搜索码对应的值与索引项的值之间的数据大小关系
- 哈希索引：掌握哈希函数（将任意长度的值转化为固定长度的值）、哈希表（存储结构）、哈希冲突、查询类型（仅支持等值查询）、查询效率（O(1)）
- B+树索引：掌握其组成（根节点、内部节点、叶子节点，节点间可为父子、兄弟关系）、平衡树（存储结构）、查询类型（支持等值、比较、排序等）、查询效率（O(logN)）
  - 根节点与内部节点：遵循[P1,K1,...Pn-1,Kn-1,Pn]的结构，其中P为指向子节点的指针，K为索引键值；根节点的孩子数可为2-n个，内部节点的孩子数可为n/2-n个
  - 叶子节点：存储实际数据
### 3.查询优化
同一sql语句可写为不同的关系代数表达式，在执行查询的真实过程中，优化器会选择查询效率更高的关系代数表达式来执行查询（一般先筛选再投影）
### 4.事物
是单个逻辑工作单元，满足原子性、隔离性、持久性、一致性4大特征（ACID）


# 拓展学习
由于在业界mysql的使用更为广泛，所以在课后也同步学习了B站上mosh老师讲[sql的课程](https://www.bilibili.com/video/BV1UE41147KC/?spm_id_from=333.337.search-card.all.click&vd_source=799e8e881eca4f6927ede1cbdec3673e)。mysql的语法与postgresql的语法极为相似，所以该线上课程更多作为对课堂学习的巩固和补充。下面将简单地对mysql和postgresql的不同部分做一个总结。
- 字符串：mysql中可用""或''来表示字符串，不区分字符串的大小写，一般用concat来连接字符串；postgresql中只能用''来表示字符串，区分字符串大小写，可用concat或||来连接字符串
- 表连接：mysql中没有全连接，但可以通过left join + right join + union来实现
- 自增id：mysql中通过auto_increment实现
- 空值处理：mysql中除了用coalesce函数，还可以用ifnull函数
- 条件判断：mysql中除了用case语句，还可以用if语句
- 日期时间函数
  - 获取当前日期时间： mysql——curdate、curtime、now   postgresql——current_date、current_time、current_timestamp
  - 日期时间格式化： mysql——date_format(...,'')、time_format(...,'')   postgresql——to_char(...,'')
  - 日期时间增加/减少： mysql——date_add(...,interval ...)、date_sub(...,interval ...)   postgresql——interval
  - 日期时间差值计算： mysql——datediff(结束日期,开始日期)   postgresql——直接加减

此外，虽然sql语言比较简单，但为熟悉sql的应用同时与业务相结合，在牛客网上进行了一些在线编程的练习（主要完成了快速入门、必知必会两部分的练习）。\
![Alt Text](niukewang_sql1.jpg)
![Alt Text](niukewang_sql2.jpg)


# 总体评价与感悟
- 基础知识掌握与理解（9.5分/10分）：对于知识的掌握还算不错，但课堂上与老师的互动较少，所以这部分扣掉0.5分。
- 拓展学习与探索（10分/10分）：除了课堂上的学习外，课后也花费了不少功夫。在此过程中，AI给了我极大的帮助，不仅加快了我进行信息检索的速度和能力，还让我更加会提问，对所学内容有了更充分、深入的探索和理解。
- 应用与实践（10分/10分）：虽然课堂以理论讲解为主，但对我来说这门课程更偏应用，所以在课后也做了更多相关的练习。在练习中不单单是对这门语言的熟悉，更多地是在业务场景的背景下去理解sql语言运行的逻辑。
- 作业（18分/20分）：作业部分率有疏忽，有时候做的比较草率，所以这部分扣掉2分。

总评：47.5分/50分
