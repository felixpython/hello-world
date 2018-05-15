### SQL notes

* sql中的时间和日期需要用单引号括起来，'2013-11-23'

* **Select**语句  
**Limit** *count* [**Offset** *skip*], count: 一页中显示多少条; skip: 从结果的多少条后开始显示。  
e.g: ```Limit 10 offset 150```, return 10 rows, starting with the 151st row

**Order by** *columns* [Desc], columns:按照一列或多列进行排序;desc:表示降序  
e.g: ```order by species, name```, sort result rows first by the species column, then with each species, sort by the name column.  

**Group by** *columns*, 它只和像count, sum这种聚合函数一起使用。  
e.g: ```select species, min(birthdate) from animals group by species```  
for each species of animal, find the smallest value of the birthdate column, that is ,the oldest animal's birthdate.  
```select name, count(*) as num from animals group by name```  
*count(*)* -> count all the rows; *as num* -> call the count column "num";

Chinest edition:  
Select 子句
这这些是我们到目前为止见到的所有 select 子句：
where
where 子句表示限制条件 — 从表格中过滤出符合特定规则的行。where 支持等于、不等于和布尔运算符等：
where species = 'gorilla' — 仅返回物种列的值为“gorilla”的行。
where name >= 'George' — 仅返回名称列在“George”之后（按字母顺序）的行。
where species != 'gorilla' and name != 'George' — 仅返回物种不是“gorilla”并且名称不是“George”的行。
limit / offset
limit 子句对结果表格可以返回的行数做出限制。可选 offset 子句表示要在结果中跳过多少行。所以 limit 10 offset 100 将返回 10 条结果，从第 101 行开始。
order by
order by 子句告诉数据库如何对结果排序 — 通常根据一个或多个列。所以 order by species, name 表示首先按照物种列排序，然后在每个物种里按照名称排序。
排序发生在 limit/offset 之前，所以你可以使用它们来提取出按字母顺序排列的页面结果（想想字典的页面）。

可选 desc 修饰符告诉数据库按照降序对结果排序，例如从大到小或从 Z 到 A。

group by
group by 子句只能用于汇总，例如 max 或 sum。没有 group by 子句的话，对集合执行选择语句将对整个选定表格进行汇总，只返回一行。对于 group by 子句，它将对 group by 子句中的列或表达式的每个唯一值返回一行。

columns -> tables -> aggregation -> sorting

**Insert**  
insert 语句的基本语法：

insert into table ( column1, column2, ... ) values ( val1, val2, ... );

如果值和表格的列顺序一样（从第一列开始），则不需要在 insert 语句中指定列：

insert into table values ( val1, val2, ... );

例如，如果表格有三列 (a, b, c)，你想要向 a 和 b 中插入值，你可以在 insert 语句中省略列名称。但是如果你想向 b 和 c 或 a 和 c 中插入值，则需要指定列。

单个 insert 语句只能插入一个表格中（而 select 语句可以使用 join 从多个表格中获取数据）。

having 子句和 where 子句工作原理差不多，但是它应用于 group by 汇总发生之后。语法如下所示：

select columns from tables group by column having condition ;

通常，至少有一列将是汇总函数，例如对表格的某列执行 count、max 或 sum 操作。要对汇总列应用 having，你需要使用 as 为其设定名称。例如，如果你有一个商店所售商品的表格，并且想要找出售出数量超过 5 件的所有商品，则可以使用：

select name, count(\) as num* from sales having num > 5;

你可以在 select 语句中仅使用 where、仅使用 group by 、或使用 group by 和 having 、或使用 where 和 group by 或三个都用到！

但是如果没有 group by 的情况下，使用 having 通常是不合理的。

如果你同时使用了 where 和 having， where 条件将过滤即将被汇总的行，having 条件将过滤汇总后的行。

关于 having 的更多详情请参阅以下网址：

[http://www.postgresql.org/docs/9.4/static/sql-select.html#SQL-HAVING](http://www.postgresql.org/docs/9.4/static/sql-select.html#SQL-HAVING)  

**Update**  
UPDATE *table* SET *column* = *value* WHERE *restriction* ;

**Delete**  
DELETE FROM *table* WHERE *restriction*;  

**DB-API**:  
import sqlite3

**Select** operation  
# Fetch some student records from the database.
db = sqlite3.connect("students")
c = db.cursor()
query = "select name, id from students order by name;"
c.execute(query)
rows = c.fetchall()

# First, what data structure did we get?
print "Row data:"
print rows

# And let's loop over it too:
print
print "Student names:"
for row in rows:
  print "  ", row[0]

db.close()

**杀掉linux进程**, ```kill -9 PID```, 要想查看进程ID号，使用```netstat -anp```,找到占用端口的进程。  
**关于规范化(Normal Form)**  
规范化表格的规则：
1. 每行都具有相同数量的列。
实际上，数据库系统不允许在不同的行里具有不同数量的列。但是如果某些列有时候为空，有时候不为空，或者如果我们将多个值放入一个字段里，我们可以调整下此规则。

拿动物园数据库中的 diet 表格为例。我们将某个物种会吃的多种食物放入多行，而不是放在一行里。这样的话，汇总和比较时就操作简单多了。

2. 表格存在唯一键，一行里的所有内容都围绕该键展开。
键可以是一列或多列。甚至可以是整行内容，就像 diet 表格一样。但是一个表格里没有重复的行。

更重要的是，如果我们要存储不是唯一的内容，例如人们的姓名，我们就使用唯一标识符（例如序列号）区分它们。这样可以确保我们不会将两个姓名相同的人的分数或违规停车罚单汇总到一起。

3. 与唯一键不相关的内容放在其他表格中。
例如 items 表格，里面有条目、它们的地点和地点的街道地址。地址不是关于条目的内容，它是关于地点的内容。将地址放到另一表格里可以节省空间，并避免造成混淆，我们始终可以使用 join 重新组成原始表格。

4. 表格不应该暗示不存在的关系。
例如 job_skills 表格，一行列出某人的技术技能（例如“Linux”）和某个语言技能（例如“法语”）。这样看起来就好像 Linux 技能是特定于法语，反之亦然，但现实中不是这种情况。规范化这一情况需要将技术技能和工作技能放在不同的表格里。  
