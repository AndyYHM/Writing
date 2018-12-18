# 20181112-PostgreSQL数据库dmp文件导入

_标注：dmp文件导入,场景：多个schema导入_

# 1. 环境准备：
postgres集群master节点上，postgres用户执行以下操作
```
cd ~
scp -rp  file1:实际路径    ./
chown  postgres:postgres  filename
```
# 2. 登录数据库
```
1.用 postgres 用户 登录postgres集群master数据库 ：
命令：psql
```

# 3. 新建数据库用户：
```
create user smlprft password 'smlprft';
create user countercust  password 'countercust';
create user cuanadb password 'cuanadb';
```

# 4.在pg master 数据库 依次导入数据：
```
# 向xpgdb导入
psql -f smlprft-20181110.dmp -d xpgdb

# 执行完成后，再执行下面的命令
psql -f countercust-20181110.dmp -d xpgdb

# 执行完成后，再执行下面的命令
psql -f cuanadb-20181110.dmp -d xpgdb
```
