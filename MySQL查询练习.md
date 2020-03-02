# 查询练习数据准备

1、学生表(student)

学号，姓名，性别，出生年月日，班级

```sql
mysql> create table student(
    -> sno varchar(20) primary key,
    -> sname varchar(20) not null,
    -> ssex varchar(20) not null,
    -> sbirthday datetime,
    -> class varchar(20)
    -> );
```



2、课程表(course)

课程号，课程名称，教师编号

```sql
mysql> create table course(
    -> cno varchar(20) primary key,
    -> cname varchar(20) not null,
    -> tno varchar(20) not null,
    -> foreign key(tno) references teacher(tno)
    -> );
```



3、成绩表(score)

学号，课程号，成绩

```sql
mysql> create table score(
    -> sno varchar(20) not null,
    -> cno varchar(20) not null,
    -> degree decimal,
    -> foreign key(sno) references student(sno),
    -> foreign key(cno) references course(cno),
    -> primary key(sno,cno)
    -> );
```



4、教师表(teacher)

教师编号，教师姓名。教师性别，出生年月日，职称，所在部门

```sql
mysql> create table teacher(
    -> tno varchar(20) primary key,
    -> tname varchar(20) not null,
    -> tsex varchar(10) not null,
    -> tbirthday datetime,
    -> prof varchar(20) not null,
    -> depart varchar(20) not null
    -> );
```



# 往数据表中添加数据

1、在学生表中添加以下数据

```sql
insert into student values('188','曾华','男','1997-09-01','95033');
insert into student values('105','匡明','男','1975-10-02','95031');
insert into student values('107','王丽','女','1976-01-23','95033');
insert into student values('101','李军','男','1976-02-20','95033');
insert into student values('109','王芳','女','1975-02-10','95031');
insert into student values('103','陆君','男','1974-06-03','95031');
```

2、教师表

```sql
insert into teacher values('804','李诚','男','1958-12-02','副教授','计算机系');
insert into teacher values('856','张旭','男','1969-03-12','讲师','电子工程系');
insert into teacher values('825','王萍','女','1972-05-05','助教','计算机系 ');
insert into teacher values('831','刘冰','女','1977-08-14','助教','电子工程系');
```

3、课程表

```sql
insert into course values('3-105','计算机导论','825');
insert into course values('3-245','操作系统','804');
insert into course values('6-166','数字电路','856');
insert into course values('9-888','高等数学','831');
```

4、成绩表

```sql
insert into score values('103','3-245','86');
insert into score values('105','3-245','75');
insert into score values('109','3-245','68');
insert into score values('103','3-105','92');
insert into score values('105','3-105','88');
insert into score values('109','3-105','76');
insert into score values('103','6-166','85');
insert into score values('105','6-166','79');
insert into score values('109','6-166','81');
```

# 查询练习

1、查询student表的所有信息

```sql
select * from student;
```



2、查询student表中的所有记录的sname，ssex和class列

```sql
select sname, ssex, class from student;
```



3、查询教师所有的单位，即不重复的depart列

```sql
select distinct depart from teacher; 
```



4、查询score表中成绩在60到80之间的所有记录

使用between...and...

```sql
select * from score where degree between 60 and 80;
```

或者使用运算符

```sql
select * from score where degree > 60 and degree < 80;
```



5、查询score表中成绩为85、86或88的记录

表示或者关系，in

```sql
select * from score where degree in(85, 86 ,88);
```



6、查询student表中“95031”班或性别为女的同学记录

使用or

```sql
select * from student where class='95031' or ssex='女';
```



7、以class降序查询student表的所有记录

升序(asc)，降序(desc)  **默认为升序**

```sqlite
select * from student order by class desc;
```



8、以con升序、degree降序查询score表的所有记录

即以cno升序排列，遇到相同的再以degree降序排列

```sql
select * from score order by cno asc,degree desc;
```



9、查询“95031”班的学生人数

统计count

```sql
select count(*) from student where class='95031';
```



10、查询score表中的最高分的学生学号和课程号。（子查询或排序）

```sql
select sno,cno from score where degree=(select max(degree) from score);
```



