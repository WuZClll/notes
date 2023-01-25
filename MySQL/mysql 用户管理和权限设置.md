# mysql 用户管理和权限设置

![img](https://csdnimg.cn/release/blogv2/dist/pc/img/reprint.png)

[@进行中](https://blog.csdn.net/lu1171901273)![img](https://csdnimg.cn/release/blogv2/dist/pc/img/newCurrentTime2.png)于 2019-06-12 22:01:12 发布![img](https://csdnimg.cn/release/blogv2/dist/pc/img/articleReadEyes2.png)21368![img](https://csdnimg.cn/release/blogv2/dist/pc/img/tobarCollect2.png) 收藏 105

分类专栏： [mysql](https://blog.csdn.net/lu1171901273/category_7964735.html) 文章标签： [mysql](https://so.csdn.net/so/search/s.do?q=mysql&t=blog&o=vip&s=&l=&f=&viparticle=)

[![img](https://img-blog.csdnimg.cn/20201014180756780.png?x-oss-process=image/resize,m_fixed,h_64,w_64)mysql专栏收录该内容](https://blog.csdn.net/lu1171901273/category_7964735.html)

4 篇文章1 订阅

订阅专栏

# [MySQL创建用户与授权](https://www.cnblogs.com/sos-blue/p/6852945.html)

## 一. 创建用户

### 命令:

```sql
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
```

### 说明：

- username：你将创建的用户名
- host：指定该用户在哪个主机上可以登陆，如果是本地用户可用localhost，如果想让该用户可以**从任意远程主机登陆**，可以使用通配符`%`
- password：该用户的登陆密码，密码可以为空，如果为空则该用户可以不需要密码登陆服务器

### 例子：

```sql
CREATE USER 'dog'@'localhost' IDENTIFIED BY '123456';
CREATE USER 'pig'@'192.168.1.101_' IDENDIFIED BY '123456';
CREATE USER 'pig'@'%' IDENTIFIED BY '123456';
CREATE USER 'pig'@'%' IDENTIFIED BY '';
CREATE USER 'pig'@'%';
```

## 二. 授权:

### 命令:

```sql
GRANT privileges ON databasename.tablename TO 'username'@'host'
```

### 说明:

- privileges：用户的操作权限，如`SELECT`，`INSERT`，`UPDATE`等，如果要授予所的权限则使用`ALL`
- databasename：数据库名
- tablename：表名，如果要授予该用户对所有数据库和表的相应操作权限则可用`*`表示，如`*.*`

### 例子:

```sql
GRANT SELECT, INSERT ON test.user TO 'pig'@'%';
GRANT ALL ON *.* TO 'pig'@'%';
GRANT ALL ON maindataplus.* TO 'pig'@'%';
```

### 注意:

用以上命令授权的用户不能给其它用户授权，如果想让该用户可以授权，用以下命令:

```sql
GRANT privileges ON databasename.tablename TO 'username'@'host' WITH GRANT OPTION;
```

## 三.设置与更改用户密码

### 命令:

```sql
SET PASSWORD FOR 'username'@'host' = PASSWORD('newpassword');
```

如果是当前登陆用户用:

```sql
SET PASSWORD = PASSWORD("newpassword");
```

### 例子:

```sql
SET PASSWORD FOR 'pig'@'%' = PASSWORD("123456");
```

## 四. 撤销[用户权限](https://so.csdn.net/so/search?q=用户权限&spm=1001.2101.3001.7020)

### 命令:

```sql
REVOKE privilege ON databasename.tablename FROM 'username'@'host';
```

## 说明:

privilege, databasename, tablename：同授权部分

### 例子:

```html
REVOKE SELECT ON *.* FROM 'pig'@'%';
```

### 注意:

假如你在给用户`'pig'@'%'`授权的时候是这样的（或类似的）：`GRANT SELECT ON test.user TO 'pig'@'%'`，则在使用`REVOKE SELECT ON *.* FROM 'pig'@'%';`命令并不能撤销该用户对test数据库中user表的`SELECT` 操作。相反，如果授权使用的是`GRANT SELECT ON *.* TO 'pig'@'%';`则`REVOKE SELECT ON test.user FROM 'pig'@'%';`命令也不能撤销该用户对test数据库中user表的`Select`权限。

具体信息可以用命令`SHOW GRANTS FOR 'pig'@'%';` 查看。

## 五.删除用户

### 命令:

```html
DROP USER 'username'@'host';



 
```

权限简介

| 权限                    | 权限级别               | 权限说明                                                     |
| ----------------------- | ---------------------- | ------------------------------------------------------------ |
| CREATE                  | 数据库、表或索引       | 创建数据库、表或索引权限                                     |
| DROP                    | 数据库或表             | 删除数据库或表权限                                           |
| GRANT OPTION            | 数据库、表或保存的程序 | 赋予权限选项                                                 |
| REFERENCES              | 数据库或表             |                                                              |
| ALTER                   | 表                     | 更改表，比如添加字段、索引等                                 |
| DELETE                  | 表                     | 删除数据权限                                                 |
| INDEX                   | 表                     | 索引权限                                                     |
| INSERT                  | 表                     | 插入权限                                                     |
| SELECT                  | 表                     | 查询权限                                                     |
| UPDATE                  | 表                     | 更新权限                                                     |
| CREATE VIEW             | 视图                   | 创建视图权限                                                 |
| SHOW VIEW               | 视图                   | 查看视图权限                                                 |
| ALTER ROUTINE           | 存储过程               | 更改存储过程权限                                             |
| CREATE ROUTINE          | 存储过程               | 创建存储过程权限                                             |
| EXECUTE                 | 存储过程               | 执行存储过程权限                                             |
| FILE                    | 服务器主机上的文件访问 | 文件访问权限                                                 |
| CREATE TEMPORARY TABLES | 服务器管理             | 创建临时表权限                                               |
| LOCK TABLES             | 服务器管理             | 锁表权限                                                     |
| CREATE USER             | 服务器管理             | 创建用户权限                                                 |
| RELOAD                  | 服务器管理             | 执行flush-hosts, flush-logs, flush-privileges, flush-status, flush-tables, flush-threads, refresh, reload等命令的权限 |
| PROCESS                 | 服务器管理             | 查看进程权限                                                 |
| REPLICATION CLIENT      | 服务器管理             | 复制权限                                                     |
| REPLICATION SLAVE       | 服务器管理             | 复制权限                                                     |
| SHOW DATABASES          | 服务器管理             | 查看数据库权限                                               |
| SHUTDOWN                | 服务器管理             | 关闭数据库权限                                               |
| SUPER                   | 服务器管理             | 执行kill线程权限                                             |

 

| 权限分布 | 可能的设置的权限                                             |
| -------- | ------------------------------------------------------------ |
| 表权限   | 'Select', 'Insert', 'Update', 'Delete', 'Create', 'Drop', 'Grant', 'References', 'Index', 'Alter' |
| 列权限   | 'Select', 'Insert', 'Update', 'References'                   |
| 过程权限 | 'Execute', 'Alter Routine', 'Grant'                          |

1、GRANT命令使用说明：

  先来看一个例子，创建一个只允许从本地登录的超级用户jack，并允许将权限赋予别的用户，密码为：jack.

mysql> grant all privileges on *.* to jack@'localhost' identified by "jack" with grant option; Query OK, 0 rows affected (0.01 sec)

  GRANT命令说明：

  ALL PRIVILEGES 是表示所有权限，你也可以使用select、update等权限。

  ON 用来指定权限针对哪些库和表。

  *.* 中前面的*号用来指定数据库名，后面的*号用来指定表名。

  TO 表示将权限赋予某个用户。

  [jack@'localhost'](mailto:jack@' rel=) 表示jack用户，@后面接限制的主机，可以是IP、IP段、域名以及%，%表示任何地方。注意：这里%有的版本不包括本地，以前碰到过给某个用户设置了%允许任何地方登录，但是在本地登录不了，这个和版本有关系，遇到这个问题再加一个localhost的用户就可以了。

  IDENTIFIED BY 指定用户的登录密码。

  WITH GRANT OPTION 这个选项表示该用户可以将自己拥有的权限授权给别人。注意：经常有人在创建操作用户的时候不指定WITH GRANT OPTION选项导致后来该用户不能使用GRANT命令创建用户或者给其它用户授权。

备注：可以使用GRANT重复给用户添加权限，权限叠加，比如你先给用户添加一个select权限，然后又给用户添加一个insert权限，那么该用户就同时拥有了select和insert权限。

2、刷新权限

flush privileges;每次修改都需要重新刷新权限

3、查看权限

查看当前用户的权限：

mysql> show grants;

查看某个用户的权限：

mysql> show grants for 'jack'@'%';

4、回收权限

revoke delete on *.* from 'jack'@'localhost';

5、删除用户

首先查看

select host,user,password from user;

删除

drop user 'jack'@'localhost';

 6、对账户重命名

rename user 'jack'@'%' to 'jim'@'%';

示例

设置普通dba管理某个数据的权限

Grant ALL privileges on testdb to usernameDBA@’localhost’

Localhost是限制本地

Grant ALL on testdb to userNameDBA@’%’

ip没有限制

设置能查询mysql服务器上所有数据库的表

Grant select on *.* to userName@’%’

username用户不管在哪登陆都能操作mysql所有的数据库和表

Grant ALL on *.* to username@’%’

Username用户可以管理mysql服务器中所有的数据库

grant 作用在单个数据库上：

grant select on testdb.* to username@localhost;

grant 作用在单个数据表上：

Grant select on testdb.tablename to username@’%’

Grant作用在表的列上

Grant select(id,sex,rank) on testdb.tableName to username@’%’

 

查看用户权限

Show grants for username

赋予权限

Grant select on databaseName.* to username

Grant select, update, delete, insert on databaseName.* to username

回收权限

Revoke select, delete on databaseName.* from username

每次权限的修改都需要刷新权限

Flush privileges；

设置整个数据库的权限

Grant ALL on databaseName.* to username和 revoke ALL

Grant ALL on databaseName.table to username

授权给普通用户查询，插入，更新，删除对数据库所有表的权利

%标识这个用户ip没有限制

grant select, insert, update, delete on testdb.* to common_user@’%’

授权给开发者用户在testdb数据库创建表的权限，

限制ip只能是192.168.0开头的用户

grant create on testdb.* to [developer@’192.168.0.%’;](mailto:developer@’192.168.0.%’;)

设置索引的权限 设置用户在数据库上创建索引的权限

限制ip只能是192.168.0开头的用户

grant index on testdb.* to [userName@’192.168.0.%’;](mailto:userName@’192.168.0.%’;)

设置存储过程的权限

grant create routine on testdb.* to [username@’192.168.0.%’;](mailto:username@’192.168.0.%’;)

grant alter routine on testdb.* to [developer@’192.168.0.%’;](mailto:developer@’192.168.0.%’;)

```html
https://www.cnblogs.com/Richardzhu/p/3318595.html  这个链接是mysql权限管理详解
```