# 11、MySQL存储过程

------

# 1、存储过程

含义：一组预先编译好的SQL语句集合，理解成批处理语句，类似于java中的方法

## 1.1、语法

**创建**

```sql
create procedure 存储过程名(参数列表) begin 存储过程体  end
```

注意：

参数列表包含三部分：参数模式  参数名 参数类型

```sql
in stuname varchar(20);
```

参数模式：

```sql
in 该参数可以作为输入，即需要调用者传递
out 该参数可作为输出，即返回值
inout 该参数即可以作为输入也可以作为输出，既需要传入值，也可以返回值
```

如果存储过程只有一句话，begin end可省略

存储过程体中每条sql语句结尾必须加分号

存储过程的结尾可以使用delimiter重新设置

语法

```
delimiter $
```

**调用语法**

```
call 存储过程名(实参列表)
```

## 1.2、示范

1、空参列表

插入五条记录

存储过程如下

```sql
delimiter $
create procedure myp1()
begin
	insert into admin(username,password) values('join1','000'),('liliy','000'),('liliy','000'),('liliy','000'),('liliy','000');
end $
```

调用

```sql
call myp1()$
```

2、创建带in模式参数的存储过程

```sql
create procedure myp2(in beautyName varchar(20))
begin
	select bo.*
	from boys bo
	right join beauty b on bo.id = b.boyfriend_id
	where b.name=beautyName;
end $
```

调用

```sql
call myp2('名字')$
```

3、带out模式的存储过程

```
create procedure myp5(in beautyName varchar(20),out boyName varchar(20))
begin 
	select bo.boyName
	from boys bo
	inner join beauty b on bo.id = b.boyfried_id
	where b.name=beautyName
end $
```

4、带inout模式参数的存储过程

传入a和b，最终a和b都翻倍并返回

```
create procedure myp8(inout a int,inout b int)
begin
	set a=a*2;
	set b=b*2;
end $
```

# 2、删除

```sql
drop procedure 过程名;
```

# 3、查看

```sql
show create procedure 过程名;
```

