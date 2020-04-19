# 2、MySQL中的约束‘

------



# 1、约束概念

对表中数据进行限定，保证数据正确性、有效性和完整性。

# 2、分类

主键约束 primary key

非空约束 not null 

唯一约束 unique

外键约束 foreign key

# 3、非空约束

即该列的值不能为空

创建表时添加

```sql
-- 在字段后加not null
create table demo(
	NAME varchar(20) not null
	);
```

删除上面的非空约束

```sql
alter table demo modify NAME varchar(20);
```

创建表后添加

```sql
alter table demo modify NAME varchar(20) not null;
```

# 4、唯一约束

即该列的值不可重复，使用unique关键字修饰

创建表时添加

```sql
create table demo(
	NAME varchar(20) unique -- 使用unique关键字即可
	);
```

删除唯一约束

```sql
alter table demo drop index NAME; -- 唯一约束其实是唯一索引
```

创建表后添加

```sql
alter table demo modify NAME varchar(20) unique
```

# 5、主键约束

主键不能为空，且唯一

一张表只能有一个字段为主键

在创建表时添加

```sql
create table demo(
	NAME varchar(20) primary key
	);
```

删除主键

```sql
alter table demo drop primary key;
```

创建表后添加主键

```sql
alter table demo modify NAME varchar(20) primary key;
```

## 自动增长

**概念**：如果某列是数字类型的，使用auto_increment 可以使其自动增长

创建表的时候添加

```sql
create table demo(
	id int primary key auto_increment -- 添加自增
	);
```

删除和增加

```sql
alter table demo modify id int; -- 删除
alter table demo modify id int auto_increment; -- 添加
```

# 6、外键约束

外键约束用于关联两个表的字段，添加后子表无权更改相关数据，父表再有引用的情况下也不能删除相关数据。

注意：子表中相关字段的值只能是父表中有的值

**在创建表时添加外键**

```sql
create table 表名(
	.....
	外键列
    constraint 外键名称 foreign key 外键列的名称 references 主表名称(主表列的名称)
);
```

**删除外键**

```sql
alter table 子表名 drop foreign key 外键名;
```



**创建表后添加外键**

```sql
alter table 子表名 add constraint 外键名称 foreign key 外键列的名称 references 主表名称(主表列的名称)
```

**级联操作**

修改外键的值正常情况要先取消子表的外键，再修改主表的值，再重新设置外键

但是可以使用级联操作来使父表和子表一起更改

需要在添加外键的时候设置级联更新

**设置级联更新**

```sql
alter table 子表名 drop foreign key 外键名; -- 先删除外键
alter table 子表名 add constraint 外键名称 foreign key 外键列的名称 references 主表名称(主表列的名称) on update cascade; -- 重新建外键并设置级联更新
```

设置后修改父表相关数据，子表的数据会一起被修改

**级联删除**

即主表删除相关数据，子表也会删除

```sql
alter table 子表名 drop foreign key 外键名; -- 先删除外键
alter table 子表名 add constraint 外键名称 foreign key 外键列的名称 references 主表名称(主表列的名称) on update cascade on delete cascade; -- 重新建外键并设置级联更新和级联删除
```

**注意**：级联更新和级联删除可以设置一个也可以一起设置