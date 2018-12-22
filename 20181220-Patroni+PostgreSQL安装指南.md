# 20181220-Patroni+PostgreSQL安装指南

> https://patroni.readthedocs.io/en/latest/README.html
>

_摘要：Patroni高可用方案部署说明_

_基于CentOS 7\Python3\Patroni1.5.3\PostgreSQL10.5\ETCD_

_离线安装Patroni_

_Author: andy_yhm@yeah.net_

_Date: 20181220_

_关键字：PostgreSQL,Python,Patroni_

# 一、基础环境说明


# 二、下载安装包
ETCD
```bash
wget https://github.com/etcd-io/etcd/releases/download/v3.3.10/etcd-v3.3.10-linux-amd64.tar.gz
```
Patroni
```bash
wget https://github.com/zalando/patroni/archive/v1.5.3.tar.gz
```
Python3
```Bash
wget https://www.python.org/ftp/python/3.6.6/Python-3.6.6.tgz
wget https://www.openssl.org/source/openssl-1.1.1-pre9.tar.gz
```
PostgreSQL10
```

```
# 三、ETCD
## 解压安装ETCD
```bash
cd {path_etc_dir}
tar xf etcd-v3.3.10-linux-amd64.tar.gz
cd etcd-v3.3.10-linux-amd64/
./etcd
```
Foreground执行etcd，使用ctrl+c退出，验证etcd可用

## 修改环境变量
```
PATRONI_ETCD_HOST: the host:port for the etcd endpoint.
PATRONI_ETCD_HOSTS: list of etcd endpoints in format host1:port1,host2:port2,etc...
PATRONI_ETCD_URL: url for the etcd, in format: http(s)://(username:password@)host:port
PATRONI_ETCD_PROXY: proxy url for the etcd. If you are connecting to the etcd using proxy, use this parameter instead of PATRONI_ETCD_URL
PATRONI_ETCD_SRV: Domain to search the SRV record(s) for cluster autodiscovery.
PATRONI_ETCD_CACERT: The ca certificate. If present it will enable validation.
PATRONI_ETCD_CERT: File with the client certificate
PATRONI_ETCD_KEY: File with the client key. Can be empty if the key is part of certificate.
```

## 配置ETCD文件

## 后台启动执行ETCD


# 四、Python3

## Python主程序安装验证
> 请参考我的文档
> https://github.com/AndyYHM/Writing/blob/Python/20180831-Linux%E7%8E%AF%E5%A2%83%E4%B8%8BPython%203.6.6%20%E7%9A%84%E5%AE%89%E8%A3%85%E8%AF%B4%E6%98%8E.md

## Patroni whl包下载（互联网环境）
编辑requirements.txt 或解压patroni安装包后主目录内获取
```
urllib3>=1.19.1,!=1.21
boto
psycopg2>=2.5.4
PyYAML
requests
six >= 1.7
kazoo>=1.3.1
python-etcd>=0.4.3,<0.5
python-consul>=0.7.0
click>=4.1
prettytable>=0.7
tzlocal
python-dateutil
psutil
cdiff
kubernetes>=2.0.0,<=7.0.0,!=4.0.*,!=5.0.*
```
联网场景下，下载python3环境的whl包,下载位置`/tmp/whl`下
```bash
pip download  -d /tmp/whl -r requirements.txt
```
## 离线测试主机上，部署

# 五、PostgreSQL10 安装


# 六、安装Patroni
## 离线安装
```bash
cd {path_Patroni_dir}
tar xf patroni-1.5.3.tar.gz
cd patroni-1.5.3/
./etcd
```
## 环境变量
### For Patroni
```Bash
PATRONI_CONFIGURATION: it is possible to set the entire configuration for the Patroni via PATRONI_CONFIGURATION environment variable. In this case any other environment variables will not be considered!
PATRONI_NAME: name of the node where the current instance of Patroni is running. Must be unique for the cluster.
PATRONI_NAMESPACE: path within the configuration store where Patroni will keep information about the cluster. Default value: "/service"
PATRONI_SCOPE: cluster name
PATRONI_LOGLEVEL: sets the general logging level (see the docs for Python logging)
PATRONI_REQUESTS_LOGLEVEL: sets the logging level for all HTTP requests e.g. Kubernetes API calls (see the docs for Python logging)
```
### For PostgreSQL
```bash
PATRONI_POSTGRESQL_LISTEN: IP address + port that Postgres listens to. Multiple comma-separated addresses are permitted, as long as the port component is appended after to the last one with a colon, i.e. listen: 127.0.0.1,127.0.0.2:5432. Patroni will use the first address from this list to establish local connections to the PostgreSQL node.
PATRONI_POSTGRESQL_CONNECT_ADDRESS: IP address + port through which Postgres is accessible from other nodes and applications.
PATRONI_POSTGRESQL_DATA_DIR: The location of the Postgres data directory, either existing or to be initialized by Patroni.
PATRONI_POSTGRESQL_CONFIG_DIR: The location of the Postgres configuration directory, defaults to the data directory. Must be writable by Patroni.
PATRONI_POSTGRESQL_BIN_DIR: Path to PostgreSQL binaries. (pg_ctl, pg_rewind, pg_basebackup, postgres) The default value is an empty string meaning that PATH environment variable will be used to find the executables.
PATRONI_POSTGRESQL_PGPASS: path to the .pgpass password file. Patroni creates this file before executing pg_basebackup and under some other circumstances. The location must be writable by Patroni.
PATRONI_REPLICATION_USERNAME: replication username; the user will be created during initialization. Replicas will use this user to access master via streaming replication
PATRONI_REPLICATION_PASSWORD: replication password; the user will be created during initialization.
PATRONI_SUPERUSER_USERNAME: name for the superuser, set during initialization (initdb) and later used by Patroni to connect to the postgres. Also this user is used by pg_rewind.
PATRONI_SUPERUSER_PASSWORD: password for the superuser, set during initialization (initdb).
```
## 修改Patroni配置文件

## 启动Patroni集群



# 七、PATRONICTL工具常用功能
## 状态查询

## 故障切换管理
