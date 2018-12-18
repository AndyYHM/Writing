# 20181218-PostgreSQL数据库Extension管理

_注意：在集群的一个数据库中安装扩展，在集群的另一个数据库要使用的话，仍需安装_

# 1. 查看当前已安装Extension
```
postgres=# \dx
				 List of installed extensions
  Name   | Version |   Schema   |         Description
---------+---------+------------+------------------------------
 plpgsql | 1.0     | pg_catalog | PL/pgSQL procedural language
(1 row)
```

# 2. 查看当前服务器可用的Extension扩展列表
```
postgres=# select name from pg_available_extensions;
		name
--------------------
 adminpack
 seg
 autoinc
 dict_xsyn
 pg_prewarm
 ltree
 ltree_plpython2u
 bloom
 pg_stat_statements
 ...
 ```

# 3. 安装可用的Extension扩展,查看验证
```
postgres=# create extension pg_stat_statements ;
CREATE EXTENSION
postgres=# \dx
									 List of installed extensions
		Name        | Version |   Schema   |                        Descr
iption
--------------------+---------+------------+-----------------------------
------------------------------
 pg_stat_statements | 1.5     | public     | track execution statistics o
f all SQL statements executed
 plpgsql            | 1.0     | pg_catalog | PL/pgSQL procedural language
(2 rows)
```
# 4. 删除Extension扩展，查看验证
```
postgres=# drop extension pg_stat_statements ;
DROP EXTENSION
postgres=# \x
Expanded display is on.
postgres=# \dx
List of installed extensions
-[ RECORD 1 ]-----------------------------
Name        | plpgsql
Version     | 1.0
Schema      | pg_catalog
Description | PL/pgSQL procedural language
```
