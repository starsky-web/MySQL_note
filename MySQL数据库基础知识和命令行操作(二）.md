# 1、数据类型如何选择

日期按照格式，数值和字符串按照大小

# 2、增删改查

**增**：使用insert语句

```sql
INSERT INTO pet VALUES('Fluffy','Harold','cat','f','1993-02-04',null);
```

将括号的的值按顺序增加到名为pet的表的各个字段中

**删**：

```sql
delete from pet where name='Fluffy';
```

删除pet表中name='Fluffy'那一行的数据

**改**：

```sql
update pet set name='1' where owner='2';
```

将pet表中owner=‘2’那一行的name改为1

**查**：

前面提到的select语句

```sql
select * from 表名
```

## 小结：

增加：INSERT

删除：DELETE

修改：UPDATE

查询：SELECT

(不区分大小写)

# 3、MySQL建表约束

即对每个字段值的限制

## 3.1、主键约束、自增约束

查看表结构命令：desc + 表名

​	主键约束能够唯一确定一张表中的一条记录，也就是通过给某个字段添加约束，就可以使字段不重复且不为空

```sql
create table user(
	id int primary key,
	name varchar(20)
);
```

第二行primary key就是创建主键的关键字

**联合主键**：多个字段主键，加起来不重复就可以

```sql
create table user2(
	id int,
	name varchar(20),
	password varchar(20),
	primary key(id,name)
	);
```

同时给id和name添加主键约束，此时增加数据id和name至少有一个不同才能添加成功，但还是不可以为空

### 自增约束

可以和主键约束放在一个字段上

```sql
create table user3(
	id int primary key auto_increment,
	name varchar(20)
	);
```

第二行id字段使用了自增约束，输入数据时可以省略这一条

```sql
insert into user3 (name) values('zhangsan');
```

这里只给name字段赋了值，id可以自动生成

**如果创建表的时候忘记设置主键了**：

```sql
create table user4(
	id int,
	name varchar(20)
	);
```

创建后再执行以下命令来添加主键约束：

```sql
alter table user4 add primary key(id);
alter table user4 modify id int primary key;
```

两种方法都可以

**删除主键**：

```sql
alter table user4 drop primary key;
```

## 3.2、外键约束

涉及到两个表：父表，子表

假设有班级表和学生表两个表：

```sql
create table classes(
	id int primary key,
	name varchar(20)
	);
```

```sql
create table students(
	id int primary key,
	name varchar(20),
	class_id int,
	foreign key(class_id) references classes(id)
	);
```

学生表的class_id字段与班级表的id字段相关联，学生表中的class_id由班级表中的id决定，即主表中没有的值，副表就不可以使用

主表中的记录被副表引用，是不可以在副表中删除的



## 3.3、唯一约束

约束该字段值不能重复，创建：

```sql
create table user5(
	id int,
	name varvhar(20)
	);

alter table user5 add unique(name);
```

第六行添加唯一约束

或者创建时直接添加：

```sql
create table user6(
	id int,
	name varvhar(20),
	unique(name)
	);
```

直接写在第三行后面也可以，使用modify添加参考前面主键约束

**添加多个唯一约束时也是有一个不重复就可以**

### 删除唯一约束

方法和前面删除主键差不多

```sql
alter table user7 drop index name;
```



### 小结

1、建表的时候可以添加约束

2、可以使用alter....add....

3、alter.....modify

4、删除alter....drop....

## 3.4、非空约束

修饰的字段不能为空

```sql
create table user9(
	id int,
	name varchar(20) not null
	);
```



## 3.5、默认约束

当我们插入字段值的时候，如果没有传值，就会只用默认值

```sql
create table user10(
	id int,
	name varchar(20),
	age int default 10
	);
```

age字段默认为10