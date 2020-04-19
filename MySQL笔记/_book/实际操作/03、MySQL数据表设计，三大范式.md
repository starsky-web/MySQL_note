# 3、数据库的三大范式

------



# 数据库的三大设计范式

三大范式即三个创建数据表的准则，根据自己的实际需求决定是否遵守即可

## 1、第一范式，（1NF）

数据表中的所有字段都是不可分割的原子值

例如一个地址信息，如果全部存在一个字段中就不符合第一范式，需要把国家，省份这些信息尽可能拆分到不同字段才满足。这样可以方便以后检索。

举例：

```sql
create table student1(
	id int primary key,
	name varchar(20)
	address varchar(20)
	);
```

如上创建的数据表地址中会含有很多可分割的信息，例如中国四川省成都市武侯区XXX大道这个地址，其中的国家，省份，城市，路名都可以拆分。所以应设计成如下形式

```sql
create table student2(
	id int primary key,
	name varchar(20),
	country varchar(20),
	province varchar(20),
	city varchar(20),
	details varchar(20)
	);
```

如上，地址被拆分成国家，省份，城市，详细地址四个部分，方便以后查找数据。（这里的详细地址其实还可以继续拆分）

**具体拆分到什么程度，根据实际情况决定，不一定非要满足范式**

## 2、第二范式（2NF）

必须是满足第一范式的前提下，第二范式要求，除主键外的每一列必须完全依赖于主键，如果出现不完全依赖，只可能发生在联合主键的情况下，此时就需要对表进行拆分。

例如，现在设计一个订单表

```sql
create table myorder(
	product_id int,
	customer_id int,
	product_name varchar(20)，
	customer_namr varchar(20),
    primary key(product_id,customer_id)
	);
```

这里存在一个问题，即产品id只与产品名称有关，而与用户id无关，同理，顾客id只与顾客名称有关，这种情况即是：除主键外的其他列，值依赖于主键的部分字段，而没有完全依赖主键。这样会在数据结构上出现问题，即一行中产品的数据和顾客的数据完全没有关系。为了应对这种情况，产品信息和顾客信息应该放在不同的表里，即这张表应该拆成3张表。

```sql
create table myorder(
	order_id int primary key,
	prodduct_id int,
	customer_id int
	);
```

```sql
create table product(
	id int primary key,
	name varchar(20)
	);
```

```sql
create table customer(
	id int primary key,
	name varchar(20)
	);
```

现在，每个表都满足第二范式，即**除主键外的其他字段都完全依赖于主键**。

第二范式即表中每一列的信息都必须与全部主键有联系，如果不满足，就要拆成多个表。

## 3、第三范式（3NF）

前提，必须满足第二范式，除开主键列的其他列之间**不能**有传递依赖关系

用前面的例子来说明，如果我在订单表中加一条customer_phone哟用来存储电话号码，那么在这张表中customer_phone可以与订单id有关系，也可以与顾客id有关系，即除开主键列的其他列之间有了依赖关系，所以不满足第三范式

```sql
create table myorder(
	order_id int primary key,
	prodduct_id int,
	customer_id int,
	customer_phone int  //这里与主键外的其他字段有联系，所以不应该放在这张表中，应该放在顾客表中
	);
```

