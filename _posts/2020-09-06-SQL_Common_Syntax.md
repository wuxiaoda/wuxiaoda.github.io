---
layout:     post
title:      "SQL常用语法"
subtitle:   "SQL_Common_Syntax"
date:       2020-09-06
author:     "邬小达"
header-img: "img/post-programming-language.jpg"
tags:
    - 人工智能
---

## 查看行数

```mysql
## 返回数据表new_table 的所有记录
select * from new_table 

# 查看行名
show columns from train;

# 查看行数
select count(*) from train; 
select count(*) from (select distinct id from train) train;

# 查看前6行
select * from train limit 6

#查找数据表中 runoob_count 列是否为 NULL，必须使用IS NULL和IS NOT NULL
SELECT * FROM tcount_tbl 
    WHERE runoob_count IS NULL;
    
SELECT * from tcount_tbl 
   WHERE runoob_count IS NOT NULL;
   
# 索引
CREATE INDEX indexName ON tcount_tbl; 
```

## 0,1个数

```mysql
select sum(case when target =1 then 1 else 0 end) as 1_label_amount, sum(case when target = 0 then 1 else 0 end) as 0_label_amount from train
```

## 创建和删除表

```mysql
DROP TABLE IF EXISTS data
CREATE TABLE IF NOT EXISTS data
```

## 使用world数据库
```mysql
use world
```

## 创建表
```mysql
CREATE TABLE `world`.`new_table` (
  `idnew_table` int NOT NULL COMMENT '',
  `new_tablecol` char(45) NOT NULL COMMENT '',
  `new_tablecol1` char(45) NOT NULL COMMENT '',
  PRIMARY KEY (`idnew_table`, `new_tablecol`, `new_tablecol1`)  COMMENT '');
  
# 插入数据
INSERT INTO `world`.`new_table` 
     (`idnew_table`, `new_tablecol`, `new_tablecol1`) 
     VALUES ('12344', '123', '134566');
```

## where查询子句
```mysql
SELECT * from new_table WHERE idnew_table='123' or new_tablecol='45';

# 法一
select idnew_table,new_tablecol from new_table；
where idnew_table=34 or idnew_table=122

# 法二
select idnew_table,new_tablecol from new_table
where idnew_table in (34,122)

# not
select idnew_table,new_tablecol from new_table
where idnew_table not in (34,122)
```

## MySQL 排序(默认ASC升序)

```mysql
SELECT * from new_table
    ORDER BY idnew_table ASC

# 降序
SELECT * from new_table
    ORDER BY idnew_table DESC
```

## 生成相同的两张表，便于比较

```mysql
SELECT * FROM 
	Employee AS a,Employee AS b
```

## 运算

```mysql
# 1.将文本转为大写
select idnew_table, Upper(new_tablecol) as wiu
from new_table

# 2.时间和日期处理
select idnew_table from new_table
where Date(idnew_table) = '2016-2-7' 

select idnew_table from new_table
where Year(idnew_table) = 2016 and Month(idnew_table) = 2

# 3.计算平均值
select AVG(idnew_table) AS avg_idnew
from new_table
where new_tablecol= '%a%'

# 4.计数

# 统计行数
select count(*) as new_tablecol
from new_table  

# 统计new_tablecoll的行数
select count(new_tablecol) as count1
from new_table

# 5. 返回指定列的最大值
select max(idnew_table) as max_1 
from new_table

# 6.返回指定列的最小值
select min(idnew_table) as min_1 
from new_table

# 7.计算列值和
select sum(idnew_table) as avg_idnew
from new_table

# 9.只将不同值进行相加
select sum(distinct idnew_table) as avg_idnew
from new_table

# 10.组合使用
select count(new_tablecol) AS count1,
       max(idnew_table) AS max_1 ,
       min(idnew_table) AS min_1 ,
	   sum(idnew_table) AS avg_idnew
from new_table
```

## 分组数据
```mysql
# HAVING在group by时被使用，用于筛选数据，是where的功能。因为where不能用在group by

select new_tablecol1,count(new_tablecol) AS count2
from new_table
group by new_tablecol1

# 过滤分组
select new_tablecol1,count(new_tablecol) AS count2
from new_table
group by new_tablecol1
HAVING count(*)  >= 1

# 使用组合查询(UNION)(会删除重复的行)
select idnew_table,new_tablecol from new_table
where idnew_table=34 or idnew_table=122

# 或者
select idnew_table,new_tablecol from new_table
where idnew_table=34 
union
select idnew_table,new_tablecol from new_table
where idnew_table=122
```

# MySQL 合并表

- INNER JOIN（内连接,或等值连接）：获取两个表中字段匹配的交集。
- LEFT JOIN（左连接）：获取左表所有记录，即使右表没有对应匹配的记录。
- RIGHT JOIN（右连接）： 与 LEFT JOIN 相反，用于获取右表所有记录，即使左表没有对应匹配的记录。

```mysql
SELECT a.runoob_id, a.runoob_author, b.runoob_count FROM runoob_tbl as a INNER JOIN tcount_tbl as b 
ON a.runoob_author = b.runoob_author

# 等价于
SELECT a.runoob_id, a.runoob_author, b.runoob_count FROM runoob_tbl as a, tcount_tbl as b 
WHERE a.runoob_author = b.runoob_author

# 商品活动流水表，表名为event，字段：goods_id， time；
# 求参加活动次数最多的商品的最近一次参加活动的时间

select a.goods_id,a.time
from event a
inner join
(
select goods_id,count(*)
from event
group by goods_id
order by count(*) desc
limit 1
) b
on a.goods_id = b.goods_id
order by a.goods_id,a.time desc
```

## 数据重塑

宽表

| Name | Company  | Sale2013 | Sale2014 | Sale2015 | Sale2016 |
| ---- | -------- | -------- | -------- | -------- | -------- |
| 苹果 | Apple    | 5000     | 5050     | 5050     | 5050     |
| 脸书 | Facebook | 2300     | 2900     | 2900     | 2900     |
| 腾讯 | Tencent  | 3100     | 3300     | 3300     | 3300     |

长表

| Name | Company  | YearN    | Sale |
| ---- | -------- | -------- | ---- |
| 苹果 | Apple    | Sale2013 | 5000 |
| 脸书 | Facebook | Sale2013 | 2300 |
| 腾讯 | Tencent  | Sale2013 | 3100 |
| 苹果 | Apple    | Sale2014 | 5050 |
| 脸书 | Facebook | Sale2014 | 2900 |
| 腾讯 | Tencent  | Sale2014 | 3300 |
| 苹果 | Apple    | Sale2015 | 5050 |
| 脸书 | Facebook | Sale2015 | 2900 |
| 腾讯 | Tencent  | Sale2015 | 3300 |
| 苹果 | Apple    | Sale2016 | 5050 |
| 脸书 | Facebook | Sale2016 | 2900 |
| 腾讯 | Tencent  | Sale2016 | 3300 |

* unpivot，宽表转长表

```mysql
select Name,Company,YearN,Sale from Table_A 
unpivot (Sale for YearN in (Sale2013,Sale2014,Sale2015,sale2016))
```

* 方法一：使用pivot，长表转宽表

```mysql
select * from Table_B 
pivot (max(Sale) for YearN in([Sale2013], [Sale2014], [Sale2015], [Sale2016]))
as Table_A
```

* 方法二：聚合函数[max或sum]配合case语句，长表转宽表

```mysql
select Name, Company,
sum (case YearN when 'Sale2013' then Sale else 0 end) as Sale2013, 
sum (case YearN when 'Sale2014' then Sale else 0 end) as Sale2014,
sum (case YearN when 'Sale2015' then Sale else 0 end) as Sale2015,
sum (case YearN when 'Sale2016' then Sale else 0 end) as Sale2016,
from Table_B group by Name
```

## 正则表达式

```mysql
# 检索new_tablecol中包含文本a或b的行
select new_tablecol from new_table
where new_tablecol regexp 'a|b'

# 匹配多个字符(匹配'1 ton','2 ton','3 ton')
select new_tablecol from new_table
where new_tablecol regexp '[123] ton'

# 匹配特殊字符（匹配.）
select new_tablecol from new_table
where new_tablecol regexp '\\.'
```

## 修改值

```mysql
# 把idnew_table列值为12344的改为666
UPDATE new_table 
     SET idnew_table=666
     WHERE idnew_table=12344;

# 7.删除idnew_table=122的行
DELETE FROM new_table WHERE idnew_table=122;
```

## like 子句

```mysql
# 表中获取runoob_author字段中以"wxd"结尾的所有记录
SELECT * from new_table 
    WHERE new_tablecol LIKE '%wxd';
    
# 表中获取runoob_author字段中任何位置出现"wxd"的所有记录
SELECT * from new_table 
    WHERE new_tablecol LIKE '%wxd%';
```

## ALTER命令(修改数据表名或字段)
```mysql
use world 
# 创建一个表
create table testalter_tbl
      (
      i INT,
      c CHAR(1)
      );
	  
# 删除，添加或修改表字段
ALTER TABLE testalter_tbl DROP i; #删除以上创建表的 i 字段

# 在表 testalter_tbl 中添加 i 字段，并定义数据类型
ALTER TABLE testalter_tbl ADD i INT;

# 指定新增字段的位置
ALTER TABLE testalter_tbl DROP i;
ALTER TABLE testalter_tbl ADD i INT FIRST;
ALTER TABLE testalter_tbl DROP i;
ALTER TABLE testalter_tbl ADD i INT AFTER c;

# 修改字段类型及名称,你可以在ALTER命令中使用 MODIFY 或 CHANGE 子句 。
# 例如，把字段 c 的类型从 CHAR(1) 改为 CHAR(10)
ALTER TABLE testalter_tbl MODIFY c CHAR(10);
ALTER TABLE testalter_tbl CHANGE i j BIGINT; # 在 CHANGE 关键字之后，紧跟着的是你要修改的字段名，然后指定新字段的类型及名称
ALTER TABLE testalter_tbl CHANGE j j INT;

# 指定字段 j 为 NOT NULL 且默认值为100 
ALTER TABLE testalter_tbl 
    MODIFY j BIGINT NOT NULL DEFAULT 100;
	
# 修改字段默认值
ALTER TABLE testalter_tbl ALTER j SET DEFAULT 1000

ALTER TABLE testalter_tbl ALTER j DROP DEFAULT

# 修改数据表类型，可以使用 ALTER 命令及 TYPE 子句来完成
# 将表 testalter_tbl 的类型修改为 MYISAM 
ALTER TABLE testalter_tbl TYPE = MYISAM;
SHOW TABLE STATUS LIKE 'testalter_tbl'\G

# 修改表名
ALTER TABLE testalter_tbl RENAME TO alter_tbl;
```

## MySQL 处理重复数据

```mysql
# 防止表中出现重复数据(设置指定的字段为 PRIMARY KEY（主键）  或者 UNIQUE（唯一） 索引来保证数据的唯一性)
CREATE TABLE person_tbl
(
   first_name CHAR(20) NOT NULL,
   last_name CHAR(20) NOT NULL,
   sex CHAR(10),
   PRIMARY KEY (last_name, first_name)
);
# 字段first_name，last_name数据不能重复

# INSERT IGNORE INTO与INSERT INTO的区别就是INSERT IGNORE会忽略数据库中已经存在的数据，
# 如果数据库没有数据，就插入新的数据，如果有数据的话就跳过这条数据
# INSERT IGNORE INTO当插入数据时，在设置了记录的唯一性后，如果插入重复数据，将不返回错误，只以警告形式返回。 
# REPLACE INTO 如果存在primary 或 unique相同的记录，则先删除掉。再插入新记录
INSERT IGNORE INTO person_tbl (last_name, first_name)
       VALUES( 'Jay', 'Thomas');
INSERT IGNORE INTO person_tbl (last_name, first_name)
       VALUES( 'Jay', 'Thomas');

# 统计last_name, first_name的重复数据
SELECT COUNT(*) as repetitions, last_name, first_name
       FROM person_tbl
       GROUP BY last_name, first_name
       HAVING repetitions > 1;
	   
# 读取不重复的数据

# 方式一
SELECT DISTINCT last_name, first_name
       FROM person_tbl
       ORDER BY last_name;

# 方式二
SELECT last_name, first_name
       FROM person_tbl
       GROUP BY (last_name, first_name);
	   
# 删除重复数据
ALTER IGNORE TABLE person_tbl
   ADD PRIMARY KEY (last_name, first_name);
```

## MySQL 复制表

```mysql
# 步骤一
SHOW CREATE TABLE runoob_tbl \G

# 步骤二：修改SQL语句的数据表名，并执行SQL语句。
 CREATE TABLE `world`.`clone_tbl` (
   `runoob_id` int(11) NOT NULL auto_increment,
   `runoob_title` varchar(100) NOT NULL default '',
   `runoob_author` varchar(40) NOT NULL default '',
   `submission_date` date default NULL,
    PRIMARY KEY  (`runoob_id`),
    UNIQUE KEY `AUTHOR_INDEX` (`runoob_author`)
    ) ENGINE=InnoDB;

# 步骤三：如果你想拷贝数据表的数据你可以使用 INSERT INTO... SELECT  语句来实现。
INSERT INTO clone_tbl (runoob_id,
                            runoob_title,
                            runoob_author,
                            submission_date)
     SELECT runoob_id,runoob_title,
            runoob_author,submission_date
	 FROM runoob_tbl;
desc clone_tbl
select * from clone_tbl
```

