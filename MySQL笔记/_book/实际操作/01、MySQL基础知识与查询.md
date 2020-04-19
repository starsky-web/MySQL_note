



# 1、MySQL基础知识与查询

------

[TOC]



# 1、基本概念

**特点**：

1、持久化存储数据的，其实数据库是一个文件系统

2、方便存储和管理数据

3、使用了统一的方法操作数据库——SQL

# 2、数据库软件

数据库概念的实现

# 3、MySQL的安装

疯狂下一步，加密记得选老版本的。默认安装会装到c盘，如果失败，需要卸载

# 4、登录和退出

登录:

```sql
mysql -u root -p
```

退出：

```sql
exit
```

通过IP链接别的电脑上的mysql服务器

```sql
mysql -hIP -u root -p
mysql --host=ip --user=root --password=root
```



# 5、MySQL的目录结构

## 5.1、安装目录



## 5.2、数据目录

# 6、SQL的通用语法

可以以单行或多行书写，以分号结尾

可用空格和制表符，不区分大小写

## 6.1、单行注释

```sql
-- 注释内容
#注释内容
/* 注释内容 */
```



# 7、SQL

结构化查询语言，可以应用于所有关系型数据库的规则

# 8、SQL分类

**DDL 数据定义语言**

​	用来定义数据库对象：数据库，表，列等。

​	关键字：create、drop、alter

**DML 数据操作语言**

​	用来对数据库中表的数据类型进行增删改。

​	关键字：insert、delete、update

**DQL 数据查询语言**

​	用来查询数据库中的记录

​	关键字：select、where

**DCL 数据控制语言**

​	用来定义数据库的访问权限和安全级别，及创建用户。

​	关键字：GRANT、REVOKE

# 9、DDL

## 1、操作数据库

查询

```sql
show databases; -- 查看数据库列表
show create database 数据库名; -- 查询指定数据库的创建语句
```

创建

```sql
create database 数据库名;
create database if not exists 数据库名; -- 在数据库不存在的时候创建
create database 数据库名 character set gbk; -- 设置字符集为gbk
```

修改

```sql
alter database 数据库名 character set utf8; -- 修改数据库编码
```

删除

```sql
drop database 数据库名; -- 删除数据库
drop database if exists 数据库名; -- 先判断存在
```

使用

```sql
select database(); -- 查询当前数据库
use 数据库名; -- 切换数据库
```





## 2、操作表

创建

```sql
create table 表名(
	列名 数据类型,
    列名2 数据类型
); -- 最后一列没有逗号
create table 表名 like 被复制的表名; -- 复制一张表
```



查询

```sql
show tables; -- 查询当前数据库表的名称
desc 表名; -- 查询表结构
```



删除

```sql
drop table 表名;
drop table if exists 表名;
```



修改

```sql
alter table 表名 rename to 新表名; -- 更改表名
alter table 表名 character set utf8; -- 改字符集
alter table 表名 add 列名 数据类型; -- 添加列
alter table 表名 change 列名 新列名 新数据类型; -- 修改列
alter table 表名 modify 列名 数据类型; -- 修改列
```

# 10、DML

增删改表中数据

## 1、添加

```sql
insert into 表名(列名1,2) values(值1,2); -- 添加值
-- 不定义列名则默认为所有列
-- 除了数字类型，其他类型需要引号
```



## 2、删除

```sql
delete from 表名 [where 条件]; -- 条件可不写,默认为删除整个表的记录
truncate table 表名; -- 删除表再创建一个一样的空表,效率更高
```



## 3、修改

```sql
update 表名 set 列名1 = 值1 ，列名2 = 值2 [where 条件]; -- 不加条件会影响整个表
```

# 11、DQL

查询表中记录

```sql
select = from 表名;
```

语法

```sql
select
	字段列表
from
	表名列表
where
	条件列表
group by
	分组字段
having
	分组之后的条件
order by
	排序
limit
	分页限定
```

## 1、基础查询

```sql
select name,age from student; -- 从student表中查询name和age
select address from student; -- 从student表中查address
select distinct address from student; -- distinct去除重复
select distinct name,address from student; -- 此时只有两个数据完全一样才会被去除

select name,math,English,math + English from student; -- 求math和English的和
/* 如果有一个为null，则结果为null */
select name,math,English,math + ifnull(English,0) from student;
/* 如果English为null则会被替换成0 */

select name,math,English,math + ifnull(English,0) AS 总分 from student; -- 相加结果显示是显示为总分，起别名,as可以不写，使用空字符
```

## 2、条件查询

where字句后跟条件

运算符

```sql
>  <  <=  >=  =  <>(不等号)
between...and...
in(集合)
like  模糊查询
	占位符：
		_ :单个任意字符
		% : 任意多个字符
is null
and 或 &&
or 或 ||
not 或 !
```

示例

```sql
select * from student where age > 20; -- 查询所有age>20的数据

select * from student age between 20 and 30; -- 查询所有age在20和30之间的数据

select * from where age = 22 or age = 18 or age = 25;
select * from where age in(22,18,15);
-- 以上两行都是查询age=22,18,15的数据

select * from student where English is null; -- 查询为null的数据只能用这个

select * from student where English is not null; -- 查询不为null的数据
```

## 3、模糊查询

```sql
select * from student where name like '马%'; -- 查询name中以马开头的数据
```

## 4、排序查询

语法

```sql
order by 排序字段1,排序字段2;
```

示例

```sql
select * from student order by math; -- 根据math列的数据来排序，默认升序

select * from student order by math desc; -- 降序排列
```

排序方式

ASC：升序

DESC：降序

```sql
select * from student order by math ASC, english ASC; -- 第一排序条件相同时按第二排序条件
```

## 5、聚合函数

将一列数据作为一个整体，进行纵向的计算

```sql
count：计算数量
max：计算最大值
min：最小值
sum：求和
avg：平均值
```

示例

```sql
select count(name) from student; -- 计算数量
```

注意：**聚合函数会排除null值**

```sql
select count(ifnull(english,0) from student; -- 把English中的null值换成0，方便记数
select count(*) from student; -- 一行中有一个数据即可记数
```

## 6、分组查询

语法

```sql
group by 分组字段
```

示例

```sql
select sex ,avg(math) from student gruop by sex; -- 性别相同的分为一组，按性别查询每组的数学平均数
select sex ,avg(math) from student where math > 70 gruop by sex; -- 只有数学大于70的才参加分组（分组前限定条件）
select sex ,avg(math), count(id) from student where math > 70 gruop by sex having count(id) > 2; -- 分组后人数要大于2，否则查询不出来
```

**注意：**

​		1、分组之后查询的字段为分组字段或聚合函数，不能使用其他字段。上面示例的结果是两个性别的数学平均数

​		2、where是在分组前限定，having是分组后限定，且where不能用聚合函数判断。

## 7、分页查询

百度即是分页查询

语法（limit只能在MySQL中用）

```sql
limit 开始的索引,每页查询的条数;
```

示例

```sql
select * from limit 0,3; -- 从0开始查，查三个记录
```

公式：开始的索引 = (当前页码 - 1)  * 每页显示的条数