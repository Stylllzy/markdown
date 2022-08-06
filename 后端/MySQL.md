其中，`information_schema`、`mysql`、`performance_schema`和`sys`是系统库



# 1. 关系模型

## 1.1 主键

关系表任意两条记录不能重复，不能重复不是指两条记录不完全相同，而是指能够通过某个字段唯一区分出不同的记录，这个字段被称为==主键==。

> 主键不要带有业务含义，应使用BIGINT自增或者GUID类型。主键也不应允许NULL

## 1.2 外键

把数据与另一张表关联起来，这种列称为外键。

用来实现一对多，多对多，一对一的关系。

## 1.3 索引

索引是关系数据库中对某一列或多个列的值进行预排序的数据结构。通过使用索引，可以让数据库系统不必扫描整个表，而是直接定位到符合条件的记录，这样就大大加快了查询速度。

`唯一索引`

身份证号，邮箱地址等，具有业务含义。



# 2. SQL的分类

## 2.1 DDL

数据定义语言

- CREATE
- ALTER
- DROP
- RENAME
- TRUNCATE

## 2.2 DML

数据操作语言

- INSERT
- DELETE
- UPDATE
- SELECT 

## 2.3 DCL

数据控制语言

- COMMIT
- ROLLBACK
- SAVEPOINT
- GRANT
- REVOKE

# 3. 查询数据

## 3.1 基本查询

```sql
-- 查询students表的所有数据
SELECT * FROM students;
-- 计算
SELECT 100+200;
```

不带 * 号的 select 可以用来判断当前数据库连接是否有效，SELECT 1;

## 3.2 条件查询

SELECT语句可以通过`WHERE`条件来设定查询条件，查询结果是满足查询条件的记录。例如，要指定条件“分数在80分或以上的学生”，写成`WHERE`条件就是`SELECT * FROM students WHERE score >= 80`。

因此，条件查询的语法就是：

```sql
SELECT * FROM <表名> WHERE <条件表达式>
- 可用 AND , OR 连接前后条件
```

如果不加括号，条件运算按照`NOT`、`AND`、`OR`的优先级进行，即`NOT`优先级最高，其次是`AND`，最后是`OR`。加上括号可以改变优先级。

## 3.3 投影查询

如果我们只希望返回某些列的数据，而不是所有列的数据，我们可以用`SELECT 列1, 列2, 列3 FROM ...`，让结果集仅包含指定列。这种操作称为投影查询。

```sql
SELECT id, score points, name FROM students WHERE gender = 'M';
```

列后接命名可对结果集的列进行重命名

## 3.4 排序

`升序`： ASC，从小到大；DESC，倒序

```mysql
-- 按成绩从低到高排
SELECT id, name, gender, score FROM students ORDER BY score;
-- 。。。从高到低
SELECT id, name, gender, score FROM students ORDER BY score DESC;
-- 如果score列有相同的数据，要进一步排序，可以继续添加列名。
-- 例如，使用ORDER BY score DESC, gender表示先按score列倒序，如果有相同分数的，再按gender列排序：
SELECT id, name, gender, score FROM students ORDER BY score DESC, gender;
```

## 3.5 分页查询

```sql
SELECT id, name, gender, score
FROM students
ORDER BY score DESC
-- 结果集从0号记录开始，最多取3条。注意SQL记录集的索引从0开始
LIMIT 3 OFFSET 0;
-- 第二页
LIMIT 3 OFFSET 3;
-- 第三页
LIMIT 3 OFFSET 6;
```

可见，分页查询的关键在于，首先要确定每页需要显示的结果数量`pageSize`（这里是3），然后根据当前页的索引`pageIndex`（从1开始），确定`LIMIT`和`OFFSET`应该设定的值：

- `LIMIT`总是设定为`pageSize`；
- `OFFSET`计算公式为`pageSize * (pageIndex - 1)`。

## 3.6 聚合查询

`COUNT(*)`

查询所有列的行数，要注意聚合的计算结果虽然是一个数字，但查询的结果仍然是一个二维表，只是这个二维表只有一行一列，并且列名是`COUNT(*)`。

```sql
-- 使用聚合查询:
-- num 为别名
SELECT COUNT(*) num FROM students;
```

| 函数 | 说明                                   |
| ---- | -------------------------------------- |
| SUM  | 计算某一列的合计值，该列必须为数值类型 |
| AVG  | 计算某一列的平均值，该列必须为数值类型 |
| MAX  | 计算某一列的最大值                     |
| MIN  | 计算某一列的最小值                     |

> 注意，`MAX()`和`MIN()`函数并不限于数值类型。如果是字符类型，`MAX()`和`MIN()`会返回排序最后和排序最前的字符。

`分组聚合`

```sql
-- 按class_id分组:
SELECT COUNT(*) num FROM students GROUP BY class_id;
```

## 3.7 多表查询

SELECT * FROM <表1> <表2>

这种一次查询两个表的数据，查询的结果也是一个二维表，它是`students`表和`classes`表的“乘积”，即`students`表的每一行与`classes`表的每一行都两两拼在一起返回。结果集的列数是`students`表和`classes`表的列数之和，行数是`students`表和`classes`表的行数之积。

## 3.8 连接查询

连接查询是另一种类型的多表查询。连接查询对多个表进行JOIN运算，简单地说，就是先确定一个主表作为结果集，然后，把其他表的行有选择性地“连接”在主表结果集上。

`INNER JOIN`

返回同时存在于两张表的行数据

1. 先确定主表，仍然使用`FROM <表1>`的语法；
2. 再确定需要连接的表，使用`INNER JOIN <表2>`的语法；
3. 然后确定连接条件，使用`ON <条件...>`，这里的条件是`s.class_id = c.id`，表示`students`表的`class_id`列与`classes`表的`id`列相同的行需要连接；
4. 可选：加上`WHERE`子句、`ORDER BY`等子句。

`RIGHT OUTER JOIN`

返回右表都存在的行。如果某一行仅在右表存在，那么结果集就会以`NULL`填充剩下的字段。

`LEFT OUTER JOIN`

返回左表都存在的行

`FULL OUTER JOIN`

把两张表的所有记录全部选择出来，并且，自动把对方不存在的列填充为NULL

<img src="C:\Users\11842\AppData\Roaming\Typora\typora-user-images\image-20220730165011162.png" alt="image-20220730165011162" style="zoom:67%;" />

JOIN查询需要先确定主表，然后把另一个表的数据“附加”到结果集上；

INNER JOIN是最常用的一种JOIN查询，它的语法是`SELECT ... FROM <表1> INNER JOIN <表2> ON <条件...>`；

JOIN查询仍然可以使用`WHERE`条件和`ORDER BY`排序。



# 4. 修改数据

## INSERT

`INSERT`语句的基本语法是：

```SQL
INSERT INTO <表名> (字段1, 字段2, ...) VALUES (值1, 值2, ...);
-- 添加一条记录
INSERT INTO students (class_id, name, gender, score) VALUES (2, '大牛', 'M', 80);
-- 添加多条记录
INSERT INTO students (class_id, name, gender, score) VALUES
  (1, '大宝', 'M', 87),
  (2, '二宝', 'M', 81);
```

对于自增主键 id ，在INSERT 语句中可以不出现

## UPDATE

`UPDATE`语句的基本语法是：

```SQL
UPDATE <表名> SET 字段1=值1, 字段2=值2, ... WHERE ...;
-- 更新id=1的记录
UPDATE students SET name='大牛', score=66 WHERE id=1;
-- 更新id=5,6,7的记录
UPDATE students SET name='小牛', score=77 WHERE id>=5 AND id<=7;
```

## DELETE

`DELETE`语句的基本语法是：

```SQL
DELETE FROM <表名> WHERE ...;
-- 删除id=1的记录
DELETE FROM students WHERE id=1;
-- 删除id=5,6,7的记录
DELETE FROM students WHERE id>=5 AND id<=7;
```

不带 WHERE 的 DELETE 会删除整个表的数据



# 5. 基本操作

```mysql
-- 列出所有数据库
SHOW DATABASE;
-- 创建数据库
CREATE DATABASE test;
-- 删除数据库
DROP DATABASE test;
-- 切换数据库
USE test;
-- 列出所有表
SHOW TABLES;
-- 查看表结构
DESC test;
-- 查看创建表的SQL语句
SHOW CREATE TABLE test;
-- 修改表，新增列birth
ALTER TABLE students ADD COLUMN birth VARCHAR(10) NOT NULL;
-- 修改birth列，例如把列名改为birthday，类型改为VARCHAR(20)
ALTER TABLE students CHANGE COLUMN birth birthday VARCHAR(20) NOT NULL;
-- 删除列
ALTER TABLE students DROP COLUMN birthday;
-- 推出MySQL
EXIT
```

注意`EXIT`仅仅断开了客户端和服务器的连接，MySQL服务器仍然继续运行。

## 实用SQL语句

==插入或替换==

如果我们希望插入一条新记录（INSERT），但如果记录已经存在，就先删除原记录，再插入新记录。此时，可以使用`REPLACE`语句，这样就不必先查询，再决定是否先删除再插入：

```mysql
REPLACE INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99);
```

若`id=1`的记录不存在，`REPLACE`语句将插入新记录，否则，当前`id=1`的记录将被删除，然后再插入新记录。

==插入或更新==

如果我们希望插入一条新记录（INSERT），但如果记录已经存在，就更新该记录，此时，可以使用`INSERT INTO ... ON DUPLICATE KEY UPDATE ...`语句：

```mysql
INSERT INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99) ON DUPLICATE KEY UPDATE name='小明', gender='F', score=99;
```

若`id=1`的记录不存在，`INSERT`语句将插入新记录，否则，当前`id=1`的记录将被更新，更新的字段由`UPDATE`指定。

==插入或忽略==

如果我们希望插入一条新记录（INSERT），但如果记录已经存在，就啥事也不干直接忽略，此时，可以使用`INSERT IGNORE INTO ...`语句：

```
INSERT IGNORE INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99);
```

若`id=1`的记录不存在，`INSERT`语句将插入新记录，否则，不执行任何操作。





# 6. 事务

转账的例子

这种把多条语句作为一个整体进行操作的功能，被称为数据库*事务*。数据库事务可以确保该事务范围内的所有操作都可以全部成功或者全部失败。如果事务失败，那么效果就和没有执行这些SQL一样，不会对数据库数据有任何改动。

数据库事务具有ACID这4个特性：

- A：Atomic，原子性，将所有SQL作为原子工作单元执行，要么全部执行，要么全部不执行；
- C：Consistent，一致性，事务完成后，所有数据的状态都是一致的，即A账户只要减去了100，B账户则必定加上了100；
- I：Isolation，隔离性，如果有多个事务并发执行，每个事务作出的修改必须与其他事务隔离；
- D：Duration，持久性，即事务完成后，对数据库数据的修改被持久化存储。

要手动把多条SQL语句作为一个事务执行，使用`BEGIN`开启一个事务，使用`COMMIT`提交一个事务，这种事务被称为*显式事务*，例如，把上述的转账操作作为一个显式事务：

```mysql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
-- 提交事务
COMMIT;
-- 回滚事务
ROLLBACK;
```



## Read Uncommitted

Read Uncommitted是隔离级别最低的一种事务级别。在这种隔离级别下，一个事务会读到另一个事务更新后但未提交的数据，如果另一个事务回滚，那么当前事务读到的数据就是脏数据，这就是脏读（Dirty Read）。



## Read Committed

在Read Committed隔离级别下，一个事务可能会遇到不可重复读（Non Repeatable Read）的问题。

不可重复读是指，在一个事务内，多次读同一数据，在这个事务还没有结束时，如果另一个事务恰好修改了这个数据，那么，在第一个事务中，两次读取的数据就可能不一致。



## Repeatable Read

在Repeatable Read隔离级别下，一个事务可能会遇到幻读（Phantom Read）的问题。

幻读是指，在一个事务中，第一次查询某条记录，发现没有，但是，当试图更新这条不存在的记录时，竟然能成功，并且，再次读取同一条记录，它就神奇地出现了。



## Serializable

Serializable是最严格的隔离级别。在Serializable隔离级别下，所有事务按照次序依次执行，因此，脏读、不可重复读、幻读都不会出现。

虽然Serializable隔离级别下的事务具有最高的安全性，但是，由于事务是串行执行，所以效率会大大下降，应用程序的性能会急剧降低。如果没有特别重要的情景，一般都不会使用Serializable隔离级别。

> 默认隔离级别

如果没有指定隔离级别，数据库就会使用默认的隔离级别。在MySQL中，如果使用InnoDB，默认的隔离级别是Repeatable Read。