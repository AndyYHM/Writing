# 20181218 - PostgreSQL Auto Commit Guide
>参考官网简介，https://www.postgresql.org/docs/10/ecpg-sql-set-autocommit.html

# 一、功能简介
Oracle中sqlplus里面执行DML语句；是需要提交commit；若错了；也可以回滚rollback； PostgreSQL psql里面默认是自动提交；执行完就马上提交，不能回滚，可以关闭自动提交。
Auto_Commit基于客户端（psql、pgadmin等等）SESSION连接参数AUTOCOMMIT，数据库SERVER端不进行控制；

# 二、操作验证
## 条件一、默认开启自动提交auto_commit功能
```
postgres=# \echo :AUTOCOMMIT
on
postgres=# create   table test_auto_commit(id int);
CREATE TABLE
postgres=# insert into test_auto_commit values (1111);
INSERT 0 1
postgres=# rollback ;
WARNING:  there is no transaction in progress
ROLLBACK
postgres=# select * from test_auto_commit ;
  id  
------
 1111
(1 row)

postgres=# update  test_auto_commit SET id = 1000 where id = 1111;
UPDATE 1
postgres=# rollback ;
WARNING:  there is no transaction in progress
ROLLBACK
postgres=# select * from test_auto_commit ;
  id  
------
 1000
(1 row)

postgres=# delete from test_auto_commit where  id = 1000;
DELETE 1
postgres=# rollback ;
WARNING:  there is no transaction in progress
ROLLBACK
postgres=# select * from test_auto_commit ;
 id
----
(0 rows)
```
结论：psql默认开启AUTOCOMMIT参数为on，INSERT、UPDATE、DELETE均无需显式输入commit命令，由客户端SESSION自动添加;

## 条件二、关闭默认开启自动提交auto\_commit功能
```
postgres=# \set AUTOCOMMIT off
postgres=# \echo :AUTOCOMMIT
off

postgres=# create   table test_auto_commit(id int);
CREATE TABLE

# INSERT 验证
postgres=# select * from test_auto_commit ;
 id
----
(0 rows)

postgres=# insert into test_auto_commit values (1111);
INSERT 0 1
postgres=# select * from test_auto_commit ;
  id  
------
 1111
(1 row)

postgres=# rollback ;
ROLLBACK
postgres=# select * from test_auto_commit ;
 id
----
(0 rows)

# UPDATE
postgres=# update  test_auto_commit SET id = 1000 where id = 1111;
UPDATE 1
postgres=# select * from test_auto_commit ;
  id  
------
 1000
(1 row)

postgres=# rollback;
ROLLBACK
postgres=# select * from test_auto_commit ;
  id  
------
 1111
(1 row)

#DELETE
postgres=# select * from test_auto_commit ;
  id  
------
 1111
(1 row)

postgres=# delete from test_auto_commit where  id = 1111;
DELETE 1
postgres=# select * from test_auto_commit ;
 id
----
(0 rows)

postgres=# rollback;
ROLLBACK
postgres=# select * from test_auto_commit ;
  id  
------
 1111
(1 row)

postgres=# delete from test_auto_commit where  id = 1111;
DELETE 1
postgres=# commit;
COMMIT
postgres=# select * from test_auto_commit ;
 id
----
(0 rows)
```
结论：修改psql默认开启AUTOCOMMIT参数为off，INSERT、UPDATE、DELETE均需显式输入commit命令，由客户端SESSION不会自动添加;
# 三、修改方法
## 1.检查当前AUTOCOMMIT参数值
```
postgres=# \echo :AUTOCOMMIT
on
```
## 2.修改当前AUTOCOMMIT参数值为OFF
```
postgres=# \set AUTOCOMMIT off
```
## 3.检查修改的AUTOCOMMIT参数值是否为OFF
```
postgres=# \echo :AUTOCOMMIT
off
```
## 4.仅限于psql工具
客户端所在系统用户执行以下语句，家目录下生成.psqlrc
```
$ echo "\set AUTOCOMMIT off" >>$HOME/.psqlrc
```
# 四、注意事项
设置后同时影响本 session，及以后的语句，客户端内临时修改AUTOCOMMIT参数，退出客户端失效
