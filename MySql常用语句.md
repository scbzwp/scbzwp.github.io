MySql常用语句

1  进入MySql容器

```shell
docker exec -it mysql bash
```

2  连接MySql数据库

```shell
mysql -uroot -p123456
```

3  配置root用户允许远程访问

```mysql
use mysql;  //切换数据库
update user set host='%' where user='root'; //允许root用户远程访问
select user,host from user;   //查询
flush privileges;  //刷新权限立即生效
```

4  查看用户的授权

```mysql
select user,host,grant_priv from user;//无授权（grant_priv = 'N'）
```

5  创建用户,最后的参数是密码

```mysql
//只允许指定ip连接
create user 'photoview'@'localhost' identified by 'photoview';
//允许所有ip连接（用通配符%表示）
create user 'photoview'@'%' identified by 'photoview';
```

6  新用户授权

```mysql
//MySql 5.7版本 8.0以上版本去掉尾部的“identified by '新用户密码'”
//基本格式如下
grant all privileges on 数据库名.表名 to '新用户名'@'指定ip' identified by '新用户密码' ;
//示例
//允许访问所有数据库下的所有表
grant all privileges on *.* to '新用户名'@'指定ip' identified by '新用户密码' ;
//指定数据库下的指定表
grant all privileges on test.test to '新用户名'@'指定ip' identified by '新用户密码' ;
//这里授予的权限是只能操作一个数据库（数据库名为：photoview，把数据库名变成 *.* 就是给所有数据库的权限）（如果root用户只允许localhost登录，而这里授权的时候又给用户授予不管任何地址都可以登录的能力，则会报无权限的错。此时需要把root用户也改成‘%’的登录限制才行）（也就是说，授权者的可登录源ip范围应该 ≥ 被授权者的可登录源ip范围）
grant all privileges on photoview.* to 'photoview'@'localhost';
```

7  设置用户操作权限

```mysql
//设置用户拥有所有权限也就是管理员
grant all privileges on *.* to '新用户名'@'指定ip' identified by '新用户密码' WITH GRANT OPTION;
//拥有查询权限
grant select on *.* to '新用户名'@'指定ip' identified by '新用户密码' WITH GRANT OPTION;
//其它操作权限说明,select查询 insert插入 delete删除 update修改
//设置用户拥有查询插入的权限
grant select,insert on *.* to '新用户名'@'指定ip' identified by '新用户密码' WITH GRANT OPTION;
grant select,insert on photoview.* to 'photoview'@'localhost' WITH GRANT OPTION;
//取消用户查询的查询权限
revoke select on what from '新用户名';
```

8  删除用户

```mysql
drop user username@localhost;
```

9  修改后刷新权限

```mysql
flush privileges;
```

