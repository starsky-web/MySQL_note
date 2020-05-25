# 9、DCL

------



# 1、管理用户

## **1、查询用户**

1、切换到mysql数据库

2、查询user表

## 2、创建用户

```sql
create user '用户名'@'主机名' identified by '密码';
-- %表示任意主机
```

## 3、删除用户

```sql
drop user '用户名'@'主机名';
```

## 4、修改密码

```sql
update user set password = password('新密码') where user = '用户名';

set password for '用户名'@'主机名' = password('新密码');
```

忘记root用户密码

```sql
cmd --> net stop mysql(停止mysql服务，管理员权限)
使用无验证方式启动mysql服务：mysqld --skip-grant-tables
启动新的cmd窗口，直接输入mysql命令登录，修改密码
关闭窗口，任务管理器关闭mysqld服务
启动mysql服务
```

# 2、管理权限

## 2.1、查询权限

```sql
show grants for '用户名'@'主机名';
```



## 2.2、授予权限

```sql
grant 权限列表 on 数据库名.表名 to '用户名'@'主机名';

grant all on *.* to '用户名'@'主机名';
-- 所有表的所有权限
```



## 2.3、撤销权限

```sql
revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名';
```

